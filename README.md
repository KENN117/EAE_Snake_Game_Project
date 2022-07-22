# EAE_Snake_Game_Project
import pygame
import random
import time
import os

pygame.font.init()
pygame.mixer.init()

from pygame.mouse import get_rel

# Init pygame
pygame.init()

# Define colours
LIME = (0, 255, 0)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
YELLOW = (255, 255, 0)

WIDTH, HEIGHT = 600, 400
game_display = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('Snake Game by Kenneth')

clock = pygame.time.Clock()

SNAKE_SIZE = 10
SNAKE_SPEED = 15

MESSAGE_FONT = pygame.font.Font('resources/Pixel-Regular.ttf', 30)
score_font = pygame.font.Font('resources/Pixel-Regular.ttf', 25)




COLLECT_SOUND = pygame.mixer.Sound(os.path.join('resources/collect.mp3'))
GAME_OVER_SOUND = pygame.mixer.Sound(os.path.join('Game Over.mp3'))





def print_score(score):
    text = score_font.render("Score: " + str(score), True, YELLOW)
    game_display.blit(text, [0, 0])


def draw_snake(snake_size, snake_pixels):
    for pixel in snake_pixels:
        pygame.draw.rect(game_display, LIME, [pixel[0], pixel[1], snake_size, snake_size])


def run_game():
    def play_background_music(self):
        pygame.mixer.music.load("resources/background music.mp3")
        pygame.mixer.music.play()


    game_over = False
    game_close = False

    x = WIDTH / 2
    y = HEIGHT / 2

    x_speed = 0
    y_speed = 0

    snake_pixels = []
    snake_length = 1

    target_x = round(random.randrange(0, WIDTH - SNAKE_SIZE) / 10.0) * 10.0
    target_y = round(random.randrange(0, HEIGHT - SNAKE_SIZE) / 10.0) * 10.0

    while not game_over:


        while game_close:
            game_display.fill(BLACK)
            game_over_message = MESSAGE_FONT.render("Game Over!", True, RED)
            game_display.blit(game_over_message, [WIDTH / 3, HEIGHT / 3])
            print_score(snake_length - 1)
            GAME_OVER_SOUND.play()

            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_ESCAPE:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_RETURN:
                        run_game()
                if event.type == pygame.QUIT:
                    game_over = True
                    game_close = False

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_speed = -SNAKE_SIZE
                    y_speed = 0
                if event.key == pygame.K_RIGHT:
                    x_speed = SNAKE_SIZE
                    y_speed = 0
                if event.key == pygame.K_UP:
                    x_speed = 0
                    y_speed = -SNAKE_SIZE
                if event.key == pygame.K_DOWN:
                    x_speed = 0
                    y_speed = SNAKE_SIZE

        if x >= WIDTH or x < 0 or y >= HEIGHT or y < 0:
            game_close = True

        x += x_speed
        y += y_speed

        game_display.fill(BLACK)
        pygame.draw.rect(game_display, YELLOW, [target_x, target_y, SNAKE_SIZE, SNAKE_SIZE])

        snake_pixels.append([x, y])

        if len(snake_pixels) > snake_length:
            del snake_pixels[0]

        for pixel in snake_pixels[:-1]:
            if pixel == [x, y]:
                game_close = True

        draw_snake(SNAKE_SIZE, snake_pixels)
        print_score(snake_length - 1)

        pygame.display.update()

        if x == target_x and y == target_y:
            target_x = round(random.randrange(0, WIDTH - SNAKE_SIZE) / 10.0) * 10.0
            target_y = round(random.randrange(0, HEIGHT - SNAKE_SIZE) / 10.0) * 10.0
            snake_length += 1

        clock.tick(SNAKE_SPEED)

    pygame.quit()
    quit()


run_game()
