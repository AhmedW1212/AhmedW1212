import pygame
import random

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
CAT_SIZE = 50
TREAT_SIZE = 30
OBSTACLE_SIZE = 40
CAT_SPEED = 5
OBSTACLE_SPEED = 3
WHITE = (255, 255, 255)
CAT_COLOR = (255, 0, 0)
TREAT_COLOR = (0, 255, 0)
OBSTACLE_COLOR = (0, 0, 255)

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Cat Game")

# Initialize cat's position
cat_x = (SCREEN_WIDTH - CAT_SIZE) // 2
cat_y = SCREEN_HEIGHT - CAT_SIZE - 10

# Initialize treat and obstacle positions
treat_x = random.randint(0, SCREEN_WIDTH - TREAT_SIZE)
treat_y = random.randint(0, SCREEN_HEIGHT - TREAT_SIZE)
obstacle_x = random.randint(0, SCREEN_WIDTH - OBSTACLE_SIZE)
obstacle_y = random.randint(0, SCREEN_HEIGHT - OBSTACLE_SIZE)

# Game loop
running = True
score = 0

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()

    if keys[pygame.K_LEFT] and cat_x > 0:
        cat_x -= CAT_SPEED
    if keys[pygame.K_RIGHT] and cat_x < SCREEN_WIDTH - CAT_SIZE:
        cat_x += CAT_SPEED
    if keys[pygame.K_UP] and cat_y > 0:
        cat_y -= CAT_SPEED
    if keys[pygame.K_DOWN] and cat_y < SCREEN_HEIGHT - CAT_SIZE:
        cat_y += CAT_SPEED

    # Check for collisions
    if (
        cat_x < treat_x + TREAT_SIZE
        and cat_x + CAT_SIZE > treat_x
        and cat_y < treat_y + TREAT_SIZE
        and cat_y + CAT_SIZE > treat_y
    ):
        score += 1
        treat_x = random.randint(0, SCREEN_WIDTH - TREAT_SIZE)
        treat_y = random.randint(0, SCREEN_HEIGHT - TREAT_SIZE)

    if (
        cat_x < obstacle_x + OBSTACLE_SIZE
        and cat_x + CAT_SIZE > obstacle_x
        and cat_y < obstacle_y + OBSTACLE_SIZE
        and cat_y + CAT_SIZE > obstacle_y
    ):
        running = False

    # Move the obstacle
    obstacle_x -= OBSTACLE_SPEED
    if obstacle_x < 0:
        obstacle_x = SCREEN_WIDTH
        obstacle_y = random.randint(0, SCREEN_HEIGHT - OBSTACLE_SIZE)

    # Clear the screen
    screen.fill(WHITE)

    # Draw the cat, treat, and obstacle
    pygame.draw.rect(screen, CAT_COLOR, (cat_x, cat_y, CAT_SIZE, CAT_SIZE))
    pygame.draw.rect(screen, TREAT_COLOR, (treat_x, treat_y, TREAT_SIZE, TREAT_SIZE))
    pygame.draw.rect(screen, OBSTACLE_COLOR, (obstacle_x, obstacle_y, OBSTACLE_SIZE, OBSTACLE_SIZE))

    # Display the score
    font = pygame.font.Font(None, 36)
    score_text = font.render(f"Score: {score}", True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    # Update the screen
    pygame.display.update()

# Game over screen
font = pygame.font.Font(None, 72)
game_over_text = font.render("Game Over", True, (255, 0, 0))
screen.blit(game_over_text, ((SCREEN_WIDTH - 250) // 2, (SCREEN_HEIGHT - 100) // 2))
pygame.display.update()

# Wait for a moment before closing the game
pygame.time.delay(2000)

# Quit Pygame
pygame.quit()
