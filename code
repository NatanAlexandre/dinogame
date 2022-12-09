import pygame
from pygame.locals import *
from sys import *
import os
from random import randrange, choice

diretorio_principal = os.path.dirname(__file__)
diretorio_imagens = os.path.join(diretorio_principal, 'imagens')
diretorio_sons = os.path.join(diretorio_principal, 'imagens/sons')

pygame.init()
pygame.mixer.init()

largura = 640
altura = 480

branco = (255, 255, 255)

screen = pygame.display.set_mode((largura, altura))

pygame.display.set_caption('Dino Game')

sprite_sheet = pygame.image.load(os.path.join(diretorio_imagens, 'dinoSpritesheet.png')).convert_alpha()

som_colisao = pygame.mixer.Sound(os.path.join(diretorio_sons, 'sons_death_sound.wav'))
som_colisao.set_volume(1)

som_pontuacao = pygame.mixer.Sound(os.path.join(diretorio_sons, 'sons_score_sound.wav'))
som_pontuacao.set_volume(1)

colidiu = False
escolha_obstaculo = choice([0, 1])

pontos = 0

velocidade_jogo = 10


def exibe_mensagem(msg, tam, cor):
    fonte = pygame.font.SysFont('comicsansms', tam, True, False)
    mensagem = f'{msg}'
    texto_formatado = fonte.render(mensagem, True, cor)
    return  texto_formatado


def reiniciar_jogo():
    global pontos, velocidade_jogo, colidiu, escolha_obstaculo
    pontos = 0
    velocidade_jogo = 10
    colidiu = False
    dino.rect.y = altura - 64 - 96//2
    dino.pulo = False
    dino_voador.rect.x = largura
    cacto. rect.x = largura
    chao.rect.x = largura
    nuvem.rect.x = largura
    escolha_obstaculo = choice([0, 1])


class Dino(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.som_pulo = pygame.mixer.Sound(os.path.join(diretorio_sons, 'sons_jump_sound.wav'))
        self.som_pulo.set_volume(1)
        self.imagens_dinossauro = []
        for i in range(3):
            img = sprite_sheet.subsurface((i * 32, 0), (32, 32))
            img = pygame.transform.scale(img, (32 * 3, 32 * 3))
            self.imagens_dinossauro.append(img)
        self.index_lista = 0
        self.image = self.imagens_dinossauro[self.index_lista]
        self.rect = self.image.get_rect()
        self.mask = pygame.mask.from_surface(self.image)
        self.pos_y = altura - 64 - 96//2
        self.rect.center = (100, altura - 64)
        self.pulo = False

    def pular(self):
        self.pulo = True
        self.som_pulo.play()


    def update(self):
        if self.pulo == True:
            if self.rect.y <= 200:
                self.pulo = False
            self.rect.y -= 20
        else:
            if self.rect.y < self.pos_y:
                self.rect.y += 20
            else:
                self.rect.y = self.pos_y
        if self.index_lista > 2:
            self.index_lista = 0
        self.index_lista += 0.25
        self.image = self.imagens_dinossauro[int(self.index_lista)]


class Nuvens(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = sprite_sheet.subsurface((7 * 32, 0), (32, 32))
        self.image = pygame.transform.scale(self.image, (3 * 32, 3 * 32))
        self.rect = self.image.get_rect()
        self.rect.y = randrange(50, 200, 50)
        self.rect.x = largura - randrange(30, 300, 90)

    def update(self):
        if self.rect.topright[0] < 0:
            self.rect.y = randrange(50, 200, 50)
            self.rect.x = largura
        self.rect.x -= velocidade_jogo - 3


class Chao(pygame.sprite.Sprite):
    def __init__(self, pos_x):
        pygame.sprite.Sprite.__init__(self)
        self.image = sprite_sheet.subsurface((6 * 32, 0), (32, 32))
        self.image = pygame.transform.scale(self.image, (2 * 32, 2 * 32))
        self.rect = self.image.get_rect()
        self.mak = pygame.mask.from_surface(self.image)
        self.rect.y = altura - 64
        self.rect.x = pos_x * 64

    def update(self):
        if self.rect.topright[0] < 0:
            self.rect.x = largura
        self.rect.x -= 10


class Cacto(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = sprite_sheet.subsurface((5 * 32, 0), (32, 32))
        self.image = pygame.transform.scale(self.image, (2 * 32, 2 * 32))
        self.rect = self.image.get_rect()
        self.rect.center = (largura, altura - 64)
        self.mask = pygame.mask.from_surface(self.image)
        self.ecolha = escolha_obstaculo
        self.rect.x = largura

    def update(self):
        if self.ecolha == 0:
            if self.rect.topright[0] < 0:
                self.rect.x = largura
            self.rect.x -= velocidade_jogo


class DinoVoador(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.imagens_dinossauro = []
        for i in range(3, 5):
            img = sprite_sheet.subsurface((i * 32, 0), (32, 32))
            img = pygame.transform.scale(img, (32 * 3, 32 * 3))
            self.imagens_dinossauro.append(img)
        self.index_lista = 0
        self.image = self.imagens_dinossauro[self.index_lista]
        self.mask = pygame.mask.from_surface(self.image)
        self.escolha = escolha_obstaculo
        self.rect = self.image.get_rect()
        self.rect.y = randrange(250, 350, 50)
        self.rect.x = largura

    def update(self):
        if self.escolha == 1:
            if self.rect.topright[0] < 0:
                self.rect.y = randrange(200, 350, 50)
                self.rect.x = largura
            self.rect.x -= velocidade_jogo
            if self.index_lista > 1:
                self.index_lista = 0
            self.index_lista += 0.25
            self.image = self.imagens_dinossauro[int(self.index_lista)]


todas_sprites = pygame.sprite.Group()
dino = Dino()
todas_sprites.add(dino)

grupo_obstaculos = pygame.sprite.Group()

for i in range(4):
    nuvem = Nuvens()
    todas_sprites.add(nuvem)
todas_sprites.add(nuvem)

for i in range(largura*2//64):
    chao = Chao(i)
    todas_sprites.add(chao)

dino_voador = DinoVoador()
todas_sprites.add(dino_voador)


cacto = Cacto()
todas_sprites.add(cacto)

grupo_obstaculos = pygame.sprite.Group()
grupo_obstaculos.add(cacto)
grupo_obstaculos.add(dino_voador)

relogio = pygame.time.Clock()

while True:
    relogio.tick(30)
    screen.fill(branco)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()
        if event.type == KEYDOWN:
            if event.key == K_SPACE and colidiu == False:
                if dino.rect.y != dino.pos_y:
                    pass
                else:
                    dino.pular()
            if event.key == K_SPACE and colidiu == True:
                reiniciar_jogo()
    colisoes = pygame.sprite.spritecollide(dino, grupo_obstaculos, False, pygame.sprite.collide_mask)

    if cacto.rect.topright[0] <= 0 or dino_voador.rect.topright[0] <= 0:
        escolha_obstaculo = choice([0, 1])
        cacto.rect.x = largura
        dino_voador.rect.x = largura
        cacto.ecolha = escolha_obstaculo
        dino_voador.escolha = escolha_obstaculo

    todas_sprites.draw(screen)

    if colisoes and colidiu == False:
        som_colisao.play()
        colidiu = True
    if colidiu == True:
        if pontos % 100 == 0:
            pontos += 1
        gameover = exibe_mensagem('GAME OVER', 40, (0, 0, 0))
        screen.blit(gameover, (210, 200))
        restart = exibe_mensagem('Pressione SPACE para reiniciar', 20, (0, 0, 0))
        screen.blit(restart, (180, 260))
    else:
        pontos += 1
        todas_sprites.update()
        texto_pontos = exibe_mensagem(pontos, 40, (0, 0, 0))
    if pontos % 100 == 0:
        som_pontuacao.play()
        if velocidade_jogo >= 23:
            velocidade_jogo += 0
        else:
            velocidade_jogo += 1

    screen.blit(texto_pontos, (520, 30))

    pygame.display.flip()
