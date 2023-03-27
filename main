import pygame
from pygame import mixer
from fighter import Fighter

mixer.init()
pygame.init()

#Creating window
WINDOW_WIDTH = 1000
WINDOW_HEIGHT = 600

display_surface = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
pygame.display.set_caption("Senshi")

#FPS
clock = pygame.time.Clock()
FPS = 60

#define color
RED = (255, 0, 0)
YELLOW = (255, 255, 0)
WHITE = (255, 255, 255)

#define game variables
intro_count = 3
last_count_update = pygame.time.get_ticks()
score = [0, 0]#player score. [p1, p2]
round_over = False
ROUND_OVER_COOLDOWN = 2000

#define fighter variables
WARRIOR_SIZE = 162
WARRIOR_SCALE = 4
WARRIOR_OFFSET = [72, 56] #placing images correctly on screen due to size of the 2 above variables
WARRIOR_DATA = [WARRIOR_SIZE, WARRIOR_SCALE, WARRIOR_OFFSET]
WIZARD_SIZE = 100
WIZARD_SCALE = 5
WIZARD_OFFSET = [41, 28]
WIZARD_DATA = [WIZARD_SIZE, WIZARD_SCALE,WIZARD_OFFSET]

#load music plus sounds
pygame.mixer.music.load("assets/audio/backgmusic.mp3")
pygame.mixer.music.set_volume(0.5)
pygame.mixer.music.play(-1, 0.0, 5000)
sword_fx = pygame.mixer.Sound("assets/audio/swordAttack.wav")
sword_fx.set_volume(0.20)
martial_fx = pygame.mixer.Sound("assets/audio/martialArt.wav")
martial_fx.set_volume(0.5)

#load background
bg_image = pygame.image.load("assets/images/background/peakpx.jpg").convert_alpha()

#load spritesheets
warrior_sheet = pygame.image.load("assets/images/player1/warrior1.png").convert_alpha()
wizard_sheet = pygame.image.load("assets/images/player2/Li1.png").convert_alpha()

#load victory image
victory_img = pygame.image.load("assets/images/icons/victory_img1.png").convert_alpha()
DEF_IMG = (200, 200)
victory_img = pygame.transform.scale(victory_img, DEF_IMG)


#define frames in animation
WARRIOR_FRAMES = [10,8,3,7,7,3,7]
WIZARD_FRAMES = [8,8,2,4,4,4,4]

#define font
count_font = pygame.font.Font("assets/fonts/ZhiMangXing-regular.ttf", 80)
score_font = pygame.font.Font("assets/fonts/ZhiMangXing-regular.ttf", 30)

#funtion for draw text
def draw_text(text, font, text_col, x, y):
    img = font.render(text, True, text_col)
    display_surface.blit(img, (x, y))

#drawing background
def draw_bg():
    scaled_bg = pygame.transform.scale(bg_image, (WINDOW_WIDTH, WINDOW_HEIGHT))
    display_surface.blit(scaled_bg, (0,0))

#health bar
def draw_health_bar(health, x, y):
    ratio = health / 100
    pygame.draw.rect(display_surface, WHITE, (x - 2, y - 2, 404, 34))
    pygame.draw.rect(display_surface, RED, (x, y, 400, 30))
    pygame.draw.rect(display_surface, YELLOW, (x, y, 400 * ratio, 30))

#create fighter
fighter_1 = Fighter(1, 200, 310, False, WARRIOR_DATA, warrior_sheet, WARRIOR_FRAMES, sword_fx)
fighter_2 = Fighter(2, 750, 310, True, WIZARD_DATA, wizard_sheet, WIZARD_FRAMES, martial_fx)

#run game
running = True
while running:

    #FPS
    clock.tick(FPS)

    #draw background
    draw_bg()

    #show player stats
    draw_health_bar(fighter_1.health, 20, 20)
    draw_health_bar(fighter_2.health, 580, 20)
    draw_text(f"P1: " + str(score[0]), score_font, RED , 20, 60)
    draw_text(f"P2: " + str(score[1]), score_font, RED , 580, 60)
    

    #update count down
    if intro_count <= 0:
        #player movement
        fighter_1.move(WINDOW_WIDTH, WINDOW_HEIGHT, display_surface, fighter_2, round_over)
        fighter_2.move(WINDOW_WIDTH, WINDOW_HEIGHT, display_surface, fighter_1, round_over)
    else:
        #display count timer
        draw_text(str(intro_count), count_font, RED, WINDOW_WIDTH / 2, WINDOW_HEIGHT / 3)
        #update count timer
        if (pygame.time.get_ticks() - last_count_update) >= 1000:
            intro_count -= 1
            last_count_update = pygame.time.get_ticks()
            

    #update fighters
    fighter_1.update()
    fighter_2.update()

    #draw fighter
    fighter_1.draw(display_surface)
    fighter_2.draw(display_surface)

    #check for player defeat
    if round_over == False:
        if fighter_1.alive == False:
            score[1] += 1
            round_over = True
            round_over_time = pygame.time.get_ticks()
        elif fighter_2.alive == False:
            score[0] += 1
            round_over = True
            round_over_time = pygame.time.get_ticks()
    else:
        #display victory
            display_surface.blit(victory_img, (410,150))
            if pygame.time.get_ticks() - round_over_time > ROUND_OVER_COOLDOWN:
                round_over = False
                intro_count = 3
                fighter_1 = Fighter(1, 200, 310, False, WARRIOR_DATA, warrior_sheet, WARRIOR_FRAMES, sword_fx)
                fighter_2 = Fighter(2, 750, 310, True, WIZARD_DATA, wizard_sheet, WIZARD_FRAMES, martial_fx)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    #update display
    pygame.display.update()

#quit game
pygame.quit()
