import pygame
import time
import random

pygame.init()

display_width = 450
display_height = 450

bg_game = pygame.image.load("/Users/aenegron/Downloads/background.gif")
i1 = pygame.image.load("/Users/aenegron/Downloads/start.png")
i11 = pygame.image.load("/Users/aenegron/Downloads/start_red.png")
bg_start = pygame.image.load('/Users/aenegron/Downloads/background.gif')

screen = pygame.display.set_mode((display_width, display_height))
surface1 = pygame.Surface(screen.get_size())
surface1 = surface1.convert()
surface1.blit(bg_start, (0, 0))

pygame.display.set_caption("AREA 51")

character = pygame.image.load("/Users/aenegron/Downloads/character.png")
alien_icon = pygame.image.load("/Users/aenegron/Downloads/alien.png")
bullet_icon = pygame.image.load("/Users/aenegron/Downloads/bullet.png")

clock = pygame.time.Clock()


def game_loop_pokemon():
x_change = 0
y_change = 0
x_icon = 175
y_icon = 175
die = False

while not die:
for event in pygame.event.get():
if event.type == pygame.QUIT:
exit()

if event.type == pygame.KEYDOWN:
if event.key == pygame.K_LEFT:
x_change = -5
if event.key == pygame.K_RIGHT:
x_change = 5
if event.key == pygame.K_UP:
y_change = -5
if event.key == pygame.K_DOWN:
y_change = 5
if event.type == pygame.KEYUP:
x_change = 0
y_change = 0

x_icon += x_change
y_icon += y_change
screen.fill((255, 255, 255))
image(x_icon, y_icon)
pygame.display.update()
clock.tick(60)


def start():
n1 = True
while n1:
clock.tick(30)
buttons = pygame.mouse.get_pressed()
x1, y1 = pygame.mouse.get_pos()

if x1 >= 75 and x1 <= 375 and y1 >= 75 and y1 <= 375:
surface1.blit(i11, (75, 75))
if buttons[0]:
n1 = False
else:
surface1.blit(i1, (75, 75))

screen.blit(surface1, (0, 0))
pygame.display.update()

for event in pygame.event.get():

if event.type == pygame.QUIT:
print("quit")

pygame.quit()

exit()

screen.blit(bg_game, (0, 0))
pygame.display.update()


def image(icon, x, y):
screen.blit(icon, (x, y))


def text_objects(text, font):
textSurface = font.render(text, True, (0, 0, 0))
return textSurface, textSurface.get_rect()


def message_display(text):
largeText = pygame.font.Font('freesansbold.ttf', 50)
TextSurf, TextRect = text_objects(text, largeText)
TextRect.center = ((display_width / 2), (display_height / 2))
screen.blit(TextSurf, TextRect)

pygame.display.update()

time.sleep(2)
game_loop_pacman()


def message_display_sub(text):
largeText = pygame.font.Font('freesansbold.ttf', 35)
TextSurf, TextRect = text_objects(text, largeText)
TextRect.center = ((display_width / 2), (display_height / 2 + 25))
surface2.blit(TextSurf, TextRect)
pygame.display.update()

time.sleep(2)
game_loop_pacman()


def fail():
message_display("You Failed")
message_display_sub("Press mouse to start again")


def player_score(count):
font = pygame.font.SysFont("freesansbold.ttf", 25)
text = font.render("Score: " + str(count), True, (0, 0, 0))
screen.blit(text, (0, 0))


def game_loop_pacman():
x_change = 0
x_icon = 175
y_icon = 350

alien_startx = random.randrange(0, display_width - 100)
alien_starty = -200
alien_speed = 7

bullet_x = x_icon
bullet_y = y_icon
bullet_change_x = 0
bullet_change_y = 0

die = False

scores = 0

while not die:
for event in pygame.event.get():
if event.type == pygame.QUIT:
pygame.quit()
quit()
if event.type == pygame.KEYDOWN:
if event.key == pygame.K_LEFT:
x_change = -5
bullet_change_x = -5
if event.key == pygame.K_RIGHT:
x_change = 5
bullet_change_x = 5
if event.key == pygame.K_SPACE:
bullet_change_y = 7
bullet_change_x = 0
if event.type == pygame.KEYUP:
if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
x_change = 0
bullet_change_x = 0

x_icon += x_change
bullet_y -= bullet_change_y
alien_starty += alien_speed

screen.blit(bg_game, (0, 0))
player_score(scores)

image(alien_icon, alien_startx, alien_starty)
image(bullet_icon, bullet_x, bullet_y)
image(character, x_icon, y_icon)

if scores > 15 and scores < 30:
alien_speed = 8
elif scores > 30:
alien_speed = 10
if scores > 40:
break

if x_icon == 0 or x_icon == display_width - 100:
fail()
die = True
for event in pygame.event.get():
if event.type == pygame.mouse.get_pressed():
die = False

if bullet_y == 0:
bullet_y = y_icon
bullet_x = x_icon

if alien_starty > display_height:
alien_starty = -200
alien_startx = random.randrange(0, display_width - 100)

if y_icon < alien_starty + 65:
print('y crossover')

if x_icon > alien_startx and x_icon < alien_startx + 65 or x_icon + 65 > alien_startx and x_icon + 65 < alien_startx + 65:
print('x crossover')
fail()
die = True
for event in pygame.event.get():
if event.type == pygame.mouse.get_pressed():
die = False

if bullet_y < alien_starty + 55:
print('bullet_y crossover')

if bullet_x >= alien_startx and bullet_x + 35 <= alien_startx + 65 or bullet_x + 35 >= alien_startx and bullet_x + 65 <= alien_startx + 65:
if alien_starty - bullet_y < 5:
print('bullet_x crossover')
# time.sleep(1)
scores += 1
alien_starty = -200
alien_startx = random.randrange(0, display_width - 100)
bullet_y = y_icon
bullet_x = x_icon

pygame.display.update()
clock.tick(60)


start()
game_loop_pacman()
pygame.quit()
quit()
