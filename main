import pygame
import sys
import time
import random

WIDTH = 500
HEIGHT = 500
TITLE = "Cubes!" 
FPS = 60 
fpsCouter = 60

BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

running = True

mouseX = 0
mouseY = 0
pmouseX = 0
pmouseY = 0
movedX = 0
movedY = 0

score = 0

# Inicializar Pygame
pygame.init()

# Crear la pantalla
screen = pygame.display.set_mode([WIDTH, HEIGHT], pygame.RESIZABLE)
pygame.display.set_caption(TITLE)



# Clase Jugador
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([50, 50])
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = WIDTH // 2
        self.rect.y = HEIGHT - 70
        self.speedX = 0

    def update(self):
        self.rect.x += self.speedX

        if self.rect.x < 0:
            self.rect.x = 0
        elif self.rect.x > WIDTH - 50:
            self.rect.x = HEIGHT - 50

# Clase Enemigo
class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([30, 30])
        self.image.fill(BLACK)
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(WIDTH - 30)
        self.rect.y = random.randrange(-100, -40)
        self.speedY = random.randrange(1, 3)

    def update(self):
        self.rect.y += self.speedY
        if self.rect.y > HEIGHT + 10:
            self.rect.x = random.randrange(WIDTH - 30)
            self.rect.y = random.randrange(-100, -40)
            self.speedY = random.randrange(1, 3)

# Crear grupo de sprites
sprites_group = pygame.sprite.Group()

# Crear jugador
player = Player()
sprites_group.add(player)

# Crear enemigos
enemies = pygame.sprite.Group()
for i in range(5):
    enemy = Enemy()
    enemies.add(enemy)
    sprites_group.add(enemy)

# Reloj para controlar la velocidad de actualización de la pantalla
timer = pygame.time.Clock()



def events():
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            global running
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                player.speedX = -5
            elif event.key == pygame.K_RIGHT:
                player.speedX = 5
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                player.speedX = 0

def updateVariables():
    global WIDTH
    global HEIGHT
    global mouseX, mouseY, pmouseX, pmouseY, movedX, movedY

    sz = pygame.display.get_window_size()
    WIDTH = sz[0]
    HEIGHT = sz[1]

    mouse = pygame.mouse.get_pos()
    pmouseX = movedX
    pmouseY = movedY

    mouseX = mouse[0]
    mouseY = mouse[1]

    movedX = mouseX - pmouseX
    movedY = mouseY - pmouseY

def updateFrame():
    global fpsCouter
    pygame.display.flip()
    time.sleep(1/FPS)
    fpsCouter += 1

def update():
    updateVariables()
    events()
    updateFrame()

def background(color):
    pygame.draw.rect(screen,color, [0,0,WIDTH,HEIGHT])

def rect(x,y,w,h,color = "white"):
    pygame.draw.rect(screen,color,(x,y,w,h))

def draw():
    consolas = pygame.font.match_font('consolas')
    #background('black')
    rect(mouseX,mouseY,10,10)
    show_text(screen,consolas,str(score), 'red', 40, 10, 20)

def show_text(screen,font,text,color, size, x, y):
	type_letter = pygame.font.Font(font,size)
	surface = type_letter.render(text,True, color)
	rectangle = surface.get_rect()
	rectangle.center = (x, y)
	screen.blit(surface,rectangle)
        
while running:
    
    draw()
    update()
    mouse = pygame.mouse.get_pos()
    
    # Actualizar sprites
    sprites_group.update()

    # Comprobar colisiones
    colisiones = pygame.sprite.spritecollide(player, enemies, True)

    if colisiones:
        score += 1

    for eachenemy in enemies:
        if eachenemy.rect.y == HEIGHT:
            score -= 1
            if score == -1:
                sys.exit()
        
    if not enemies:
        for i in range(5):
            enemy = Enemy()
            enemies.add(enemy)
            sprites_group.add(enemy)
        
    # Limpiar la pantalla
    screen.fill('green')

    # Dibujar sprites en la pantalla
    sprites_group.draw(screen)

    # Actualizar pantalla
    pygame.display.flip()

    # Limitar la velocidad de actualización de la pantalla
    timer.tick(60)
