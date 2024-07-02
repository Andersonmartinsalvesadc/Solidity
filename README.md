# Solidity
import pygame
import time
import random

pygame.init()

# Definir cores
branco = (255, 255, 255)
amarelo = (255, 255, 102)
preto = (0, 0, 0)
vermelho = (213, 50, 80)
verde = (0, 255, 0)
azul = (50, 153, 213)

# Definir tamanho da tela
largura_display = 800
altura_display = 600

# Definir a tela
display = pygame.display.set_mode((largura_display, altura_display))
pygame.display.set_caption('Jogo da Cobrinha')

# Definir clock
clock = pygame.time.Clock()
snake_block = 10
snake_speed = 15

# Definir fontes
fonte_estilo = pygame.font.SysFont("bahnschrift", 25)
fonte_pontuacao = pygame.font.SysFont("comicsansms", 35)

def sua_pontuacao(pontuacao):
    valor = fonte_pontuacao.render("Pontuação: " + str(pontuacao), True, preto)
    display.blit(valor, [0, 0])

def nossa_cobrinha(snake_block, lista_cobra):
    for x in lista_cobra:
        pygame.draw.rect(display, preto, [x[0], x[1], snake_block, snake_block])

def mensagem(msg, cor):
    mesg = fonte_estilo.render(msg, True, cor)
    display.blit(mesg, [largura_display / 6, altura_display / 3])

def jogo():
    game_over = False
    game_close = False

    x1 = largura_display / 2
    y1 = altura_display / 2

    x1_mudanca = 0
    y1_mudanca = 0

    lista_cobra = []
    comprimento_cobra = 1

    comida_x = round(random.randrange(0, largura_display - snake_block) / 10.0) * 10.0
    comida_y = round(random.randrange(0, altura_display - snake_block) / 10.0) * 10.0

    while not game_over:

        while game_close == True:
            display.fill(azul)
            mensagem("Você perdeu! Pressione C para jogar novamente ou Q para sair", vermelho)
            sua_pontuacao(comprimento_cobra - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        jogo()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_mudanca = -snake_block
                    y1_mudanca = 0
                elif event.key == pygame.K_RIGHT:
                    x1_mudanca = snake_block
                    y1_mudanca = 0
                elif event.key == pygame.K_UP:
                    y1_mudanca = -snake_block
                    x1_mudanca = 0
                elif event.key == pygame.K_DOWN:
                    y1_mudanca = snake_block
                    x1_mudanca = 0

        if x1 >= largura_display or x1 < 0 or y1 >= altura_display or y1 < 0:
            game_close = True
        x1 += x1_mudanca
        y1 += y1_mudanca
        display.fill(azul)
        pygame.draw.rect(display, verde, [comida_x, comida_y, snake_block, snake_block])
        cabeça_cobra = []
        cabeça_cobra.append(x1)
        cabeça_cobra.append(y1)
        lista_cobra.append(cabeça_cobra)
        if len(lista_cobra) > comprimento_cobra:
            del lista_cobra[0]

        for x in lista_cobra[:-1]:
            if x == cabeça_cobra:
                game_close = True

        nossa_cobrinha(snake_block, lista_cobra)
        sua_pontuacao(comprimento_cobra - 1)

        pygame.display.update()

        if x1 == comida_x and y1 == comida_y:
            comida_x = round(random.randrange(0, largura_display - snake_block) / 10.0) * 10.0
            comida_y = round(random.randrange(0, altura_display - snake_block) / 10.0) * 10.0
            comprimento_cobra += 1

        clock.tick(snake_speed)

    pygame.quit()
    quit()

jogo()
Blockchain 
