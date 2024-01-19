# RST
import sys
import pygame
from pygame.locals import QUIT
import random

# The pygame.init() function initializes all the modules required for Pygame to work. This function must be called before using any other Pygame functions.
# The pygame.display.set_mode((640, 480)) function creates a window with a size of 640 pixels wide and 480 pixels tall. This is the display surface where you can draw your game.
# The pygame.display.set_caption('Pong') function sets the title of the display window to "Pong". This is the text that appears in the title bar of the window.

pygame.init()
disp = pygame.display.set_mode((640,480))
pygame.display.set_caption('Pong')

# This section of the code retrieves the size of the display window using the get_size() method and assigns the width to the variable dispX and the height to the variable dispY.
# After that, the variable border_height is set to 10, likely to represent the height of a border at the bottom of the display window.

dispX, dispY = disp.get_size()
border_height = 10  

# Defines the Paddles Width
# Defines the Paddle Height

paddleWidth = 10
paddleHeight = 60

paddleWidth = 10
paddleHeight = 60

# paddleWidth = 10 and paddleHeight = 60 are setting the width and height of the paddles in the game.
# player1X = 20 and player2X = dispX - paddleWidth - 20 are setting the initial x-coordinates for the left and right paddles, respectively.
# player1Y = 20 and player2Y = 20 are setting the initial y-coordinates for the left and right paddles, respectively.
# speed = 5 sets the speed at which the paddles will move.
# ballX = round(dispX / 2) and ballY = round(dispY / 2) set the initial x and y coordinates of the ball to the middle of the display.
# ballRadius = 10 sets the radius of the ball.
# ballXDirection = random.choice([-1, 1]) and ballYDirection = random.choice([-1, 1]) select a random initial direction for the ball to move in the x and y axes.

player1X = 20
player2X = dispX - paddleWidth - 20
player1Y = 20
player2Y = 20
speed = 5  
ballX = round(dispX / 2)
ballY = round(dispY / 2)
ballRadius = 10
ballXDirection = random.choice([-1, 1])
ballYDirection = random.choice([-1, 1])

# Sets the balls speed on the X axle
# Sets the balls speed on the Y axle

ballXSpeed = 4
ballYSpeed = 3

# While runGame is set to True, the game will continue running

runGame = True  

# This code is used to control how fast the game runs by regulating the frames per second (FPS).

clock = pygame.time.Clock()  

# while the game is running the game will run at 60 frames per second

while runGame:
      clock.tick(60)  

# For each event, it checks if the type of event is "QUIT". This event type signifies that the user has attempted to close the game window or quit the game. If the event type is "QUIT", the program calls pygame.quit() to uninitialize all pygame modules, and then calls sys.exit() to exit the program.

      for event in pygame.event.get():
          if event.type == QUIT:
              pygame.quit()
              sys.exit()

# the variables ballX and ballY are being updated to simulate the movement of a ball

      ballX += ballXDirection * ballXSpeed
      ballY += ballYDirection * ballYSpeed

#  this code is used to get the state of all keys on the keyboard

      keys = pygame.key.get_pressed()

# In this code it controls the movement of the paddles using Up arrow down arrow W and S

       if keys[pygame.K_UP]:
          player2Y = max(player2Y - speed, 0)
      if keys[pygame.K_DOWN]:
          player2Y = min(player2Y + speed, dispY - paddleHeight)
      if keys[pygame.K_w]:
          player1Y = max(player1Y - speed, 0)
      if keys[pygame.K_s]:
          player1Y = min(player1Y + speed, dispY - paddleHeight)

# this code fills in the area of the game and the code under that sets the border on the bottom that the ball will bounce off of
          
           disp.fill((0, 0, 0, 0))

      pygame.draw.rect(
        disp, [ 255, 255, 255], (0, dispY - border_height, dispX, border_height))

# this code creates the players paddles and the ball

        player1 = pygame.draw.rect(
        disp, [200, 200, 200], (player1X, player1Y, paddleWidth, paddleHeight))
      player2 = pygame.draw.rect(
        disp, [200, 200, 200], (player2X, player2Y, paddleWidth, paddleHeight))
      ball = pygame.draw.circle(
        disp, (200, 200, 200), (int(ballX), int(ballY)), ballRadius)

# If the ball is colliding with either paddle, it will change the direction of the ball's X movement by flipping it in the opposite direction

        if player1.colliderect(ball) or player2.colliderect(ball):
          ballXDirection = -ballXDirection
          
# This code checks if the ball is close to the top or bottom edge of the screen. If the ball is too close to the top or bottom, it changes the direction of the ball's vertical movement

        if ballY < ballRadius or ballY > dispY - ballRadius - border_height:
          ballYDirection *= -1

# if the ball went past either paddles the game would end

        if ballX < 0 or ballX > dispX:
          print("Game over")
          runGame = False
          
# the function pygame.display.update() is used to update the display surface with any changes that have been made

        pygame.display.update()
