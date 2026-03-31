# basedbuild.dev
basebuild.dev
import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Screen settings
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Temple Run - 2D Prototype")
clock = pygame.time.Clock()
FPS = 60

# Colors
SKY = (135, 206, 235)
GROUND_COLOR = (139, 69, 19)
PLAYER_COLOR = (50, 205, 50)
OBSTACLE_COLOR = (255, 0, 0)
COIN_COLOR = (255, 215, 0)
TEXT_COLOR = (255, 255, 255)

# Fonts
font = pygame.font.SysFont("Arial", 36)
small_font = pygame.font.SysFont("Arial", 24)

# Player settings
player_width = 50
player_height = 60
player_x = 100
player_y = HEIGHT - player_height - 50
player_vel_y = 0
gravity = 1.2
jump_strength = -22
is_jumping = False

# Game variables
score = 0
game_speed = 6
running = True
game_over = False

# Lists for objects
obstacles = []
coins = []

# Spawn timers
spawn_timer = 0

def draw_player():
    pygame.draw.rect(screen, PLAYER_COLOR, (player_x, player_y, player_width, player_height))
    # Simple "head"
    pygame.draw.circle(screen, (255, 220, 180), (player_x + 25, player_y + 15), 12)

def draw_ground():
    pygame.draw.rect(screen, GROUND_COLOR, (0, HEIGHT - 50, WIDTH, 50))

def spawn_objects():
    global spawn_timer
    spawn_timer += 1
    if spawn_timer > 40:  # Adjust for frequency
        if random.random() < 0.6:  # Spawn obstacle ~60% chance
            obstacles.append([WIDTH, HEIGHT - 60 - 50])  # x, y (on ground)
        if random.random() < 0.7:  # Spawn coin
            coins.append([WIDTH, random.randint(100, HEIGHT - 120)])
        spawn_timer = 0

def update_and_draw_obstacles():
    global game_over
    for obs in obstacles[:]:
        obs[0] -= game_speed
        pygame.draw.rect(screen, OBSTACLE_COLOR, (obs[0], obs[1], 50, 60))
        
        # Collision with player
        player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
        obs_rect = pygame.Rect(obs[0], obs[1], 50, 60)
        if player_rect.colliderect(obs_rect):
            game_over = True
        
       
