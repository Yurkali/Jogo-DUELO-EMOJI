# Jogo-DUELO-EMOJI
# Importando a biblioteca random para rolagem de dados
import random

# Dado
def rolar_dado():
    return random.randint(1, 6)

# Inicializar variáveis j1 e j2
j1 = ""
j2 = ""
j1hp = 5
j1atk = 1
j2hp = 5
j2atk = 1


# Movimento no tabuleiro
def mover_jogador(jogador, tabuleiro):
    global j1hp
    global j2hp
    global j1atk
    global j2atk
    linha_atual, coluna_atual = encontrar_jogador(jogador, tabuleiro)
    linha_destino = linha_atual
    coluna_destino = coluna_atual

    while True:
        print("Escolha a direção para mover:")
        print("1 - Esquerda")
        print("2 - Direita")
        print("3 - Cima")
        print("4 - Baixo")
        direcao = int(input())

        if direcao == 1:
            coluna_destino -= 1
        elif direcao == 2:
            coluna_destino += 1
        elif direcao == 3:
            linha_destino -= 1
        elif direcao == 4:
            linha_destino += 1
        else:
            print("Direção inválida!")
            continue

        if linha_destino < 0 or linha_destino >= len(tabuleiro) or coluna_destino < 0 or coluna_destino >= len(tabuleiro[0]):
            print("Movimento inválido! Tente novamente.")
            linha_destino = linha_atual
            coluna_destino = coluna_atual
        else:
            break

    if tabuleiro[linha_destino][coluna_destino] == 0:
        tabuleiro[linha_atual][coluna_atual] = 0
        tabuleiro[linha_destino][coluna_destino] = jogador
    elif tabuleiro[linha_destino][coluna_destino] == "🆚":
        print("Emoji 🆚 ativado! Combate iniciado!")
        vencedor_combate = iniciar_combate(jogador, j1hp, j2hp)
        tabuleiro[linha_atual][coluna_atual] = 0
        tabuleiro[linha_destino][coluna_destino] = jogador
        if vencedor_combate:
            return True
    elif tabuleiro[linha_destino][coluna_destino] == "❤️":
        print("Emoji ❤️ ativado! Função coração!")
        j1hp = coracao(jogador, j1hp)
        tabuleiro[linha_atual][coluna_atual] = 0
        tabuleiro[linha_destino][coluna_destino] = jogador
    elif tabuleiro[linha_destino][coluna_destino] == "🍝":
        print("Emoji 🍝 ativado! Função macarrão!")
        j1hp, j1atk = macarrao(jogador, j1hp, j1atk)
        tabuleiro[linha_atual][coluna_atual] = 0
        tabuleiro[linha_destino][coluna_destino] = jogador
    elif tabuleiro[linha_destino][coluna_destino] == "⚔️":
        print("Emoji ⚔️ ativado! Função espada!")
        j1atk = espada(jogador, j1atk)
        tabuleiro[linha_atual][coluna_atual] = 0
        tabuleiro[linha_destino][coluna_destino] = jogador
    if linha_destino == len(tabuleiro) - 1 and coluna_destino == len(tabuleiro[0]) - 1:
        return True
    else:
        return False

# Função para encontrar a posição do jogador no tabuleiro
def encontrar_jogador(jogador, tabuleiro):
    for i in range(len(tabuleiro)):
        for j in range(len(tabuleiro[i])):
            if tabuleiro[i][j] == jogador:
                return i, j
    return -1, -1

# Combate
def iniciar_combate(jogador, j1hp, j2hp):
    print("Iniciando combate!")
    while j1hp > 0 and j2hp > 0:
        if jogador == j1:
            print("Vez do jogador 1 -", j1)
            j2hp = ataque_jogador(j1atk, j2hp)
            print("Vida do jogador 2:", j2hp)
            jogador = j2
        elif jogador == j2:
            print("Vez do jogador 2 -", j2)
            j1hp = ataque_jogador(j2atk, j1hp)
            print("Vida do jogador 1:", j1hp)
            jogador = j1
    if j1hp <= 0:
        print("Jogador 2 -", j2, "venceu o combate!")
        return j2
    else:
        print("Jogador 1 -", j1, "venceu o combate!")
        return j1

# Função de ataque
def ataque_jogador(atk, hp):
    dado = rolar_dado()
    print("Rolou o dado e obteve:", dado)
    if dado > 3:
        hp -= atk
        print("Acertou o ataque e causou", atk, "de dano!")
    else:
        print("Errou o ataque!")
    return hp

#Itens especiais
# Definindo os emojis
emoji_coracao = "❤️"
emoji_macarrao = "🍝"
emoji_espada = "⚔️"
emoji_vs = "🆚"
# Coração
def coracao(jogador, hp):
    print(jogador, "encontrou um coração!")
    print("➕1 de HP")
    hp += 1
    return hp

def macarrao(jogador, hp, atk):
    print(jogador, "encontrou um prato de macarrão!")
    print("➕1 de HP ➕1 de ATK")
    hp += 1
    atk += 1
    return hp, atk

def espada(jogador, atk):
    print(jogador, "encontrou uma espada!")
    print("➕1 ATK")
    atk += 1
    return atk


# Menu principal e seleção de personagem
print("██████╗ ██╗   ██╗███████╗██╗      ██████╗     ███████╗███╗   ███╗ ██████╗      ██╗██╗")
print("██╔══██╗██║   ██║██╔════╝██║     ██╔═══██╗    ██╔════╝████╗ ████║██╔═══██╗     ██║██║")
print("██║  ██║██║   ██║█████╗  ██║     ██║   ██║    █████╗  ██╔████╔██║██║   ██║     ██║██║")
print("██║  ██║██║   ██║██╔══╝  ██║     ██║   ██║    ██╔══╝  ██║╚██╔╝██║██║   ██║██   ██║██║")
print("██████╔╝╚██████╔╝███████╗███████╗╚██████╔╝    ███████╗██║ ╚═╝ ██║╚██████╔╝╚█████╔╝██║")
print("╚═════╝  ╚═════╝ ╚══════╝╚══════╝ ╚═════╝     ╚══════╝╚═╝     ╚═╝ ╚═════╝  ╚════╝ ╚═╝")
print()
print()
print("Escolha seu emoji e vá para o combate!")
print()
print()
print("Os jogadores começam com 5 de hp e 1 de atk, e se movem pelo tabuleiro a cada turno. Em cada casa, podem encontrar um dos seguintes itens:")
print("❤️ concede 1 de hp ao jogador.")
print("⚔️ concede 1 de atk ao jogador.")
print("🍝 concede 1 de hp e 1 de atk ao jogador.")
print("Escolha bem os itens que irá coletar para superar seu oponente!")
print()
print()
print("Assim que o emoji 🆚 localizado no centro do tabuleiro for tocado por algum jogador, o combate se inicia!")
print("O jogo irá rolar o dado para decidir se você acertou ou errou seu ataque! Valores maiores que 3 são acertos!")
print("Boa sorte!")
print()
print()
print(" ██████ ██   ██  ██████   ██████  ███████ ███████     ██    ██  ██████  ██    ██ ██████       ██████ ██   ██  █████  ██████   █████   ██████ ████████ ███████ ██████  ██ ")
print("██      ██   ██ ██    ██ ██    ██ ██      ██           ██  ██  ██    ██ ██    ██ ██   ██     ██      ██   ██ ██   ██ ██   ██ ██   ██ ██         ██    ██      ██   ██ ██ ")
print("██      ███████ ██    ██ ██    ██ ███████ █████         ████   ██    ██ ██    ██ ██████      ██      ███████ ███████ ██████  ███████ ██         ██    █████   ██████  ██ ")
print("██      ██   ██ ██    ██ ██    ██      ██ ██             ██    ██    ██ ██    ██ ██   ██     ██      ██   ██ ██   ██ ██   ██ ██   ██ ██         ██    ██      ██   ██    ")
print(" ██████ ██   ██  ██████   ██████  ███████ ███████        ██     ██████   ██████  ██   ██      ██████ ██   ██ ██   ██ ██   ██ ██   ██  ██████    ██    ███████ ██   ██ ██ ")
print()
print()
print("🅹🅾🅶🅰🅳🅾🆁 1")
print()
print("1 - 😎")
print("2 - 🤠")
print("3 - 🤡")

j1escolha = int(input(" "))
if j1escolha == 1:
    j1 = "😎"
elif j1escolha == 2:
    j1 = "🤠"
elif j1escolha == 3:
    j1 = "🤡"
else:
    print("Inválido!")

print("🅹🅾🅶🅰🅳🅾🆁 2")
print()
print("4 - 🤖")
print("5 - 🤓")
print("6 - 🥶")

j2escolha = int(input(" "))
if j2escolha == 4:
    j2 = "🤖"
elif j2escolha == 5:
    j2 = "🤓"
elif j2escolha == 6:
    j2 = "🥶"
else:
    print("Inválido!")

# Definindo o tabuleiro
tabuleiro = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, "🆚", 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0]
]

# Posicionando os jogadores no tabuleiro
tabuleiro[0][0] = j1
tabuleiro[8][8] = j2


# Preenchendo o tabuleiro com emojis aleatórios
for i in range(len(tabuleiro)):
    for j in range(len(tabuleiro[i])):
        if (i, j) != (0, 0) and (i, j) != (8, 8) and tabuleiro[i][j] != emoji_vs:
            random_emoji = random.choice([emoji_coracao, emoji_macarrao, emoji_espada])
            tabuleiro[i][j] = random_emoji

# Exibir o tabuleiro inicial
print()
for row in tabuleiro:
    print(" ".join(str(cell) for cell in row))
print()




# Loop principal do jogo
jogador_atual = 1
jogo_acabou = False

while not jogo_acabou:
    if jogador_atual == 1:
        print("Vez do jogador 1 -", j1)
        vencedor_j1 = mover_jogador(j1, tabuleiro)
        print()
        for row in tabuleiro:
            print(" ".join(str(cell) for cell in row))
        print()
        if vencedor_j1:
            print("Jogador 1 -", j1, "venceu o jogo!")
            jogo_acabou = True
        jogador_atual = 2
    elif jogador_atual == 2:
        print("Vez do jogador 2 -", j2)
        vencedor_j2 = mover_jogador(j2, tabuleiro)
        print()
        for row in tabuleiro:
            print(" ".join(str(cell) for cell in row))
        print()
        if vencedor_j2:
            print("Jogador 2 -", j2, "venceu o jogo!")
            jogo_acabou = True
        jogador_atual = 1
