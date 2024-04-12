import pygame

# Define some colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Define the paddle class
class Paddle(pygame.sprite.Sprite):
    def __init__(self, color, width, height):
        super().__init__()

        # Pass in the color of the paddle, its width and height.
        # Set the background color and set it to be transparent
        self.image = pygame.Surface([width, height])
        self.image.fill(BLACK)
        self.image.set_colorkey(BLACK)

        # Draw the paddle (a rectangle)
        pygame.draw.rect(self.image, color, [0, 0, width, height])

        # Fetch the rectangle object that has the dimensions of the image.
        self.rect = self.image.get_rect()

# Define the ball class
class Ball(pygame.sprite.Sprite):
    def __init__(self, color, width, height):
        super().__init__()

        # Pass in the color of the ball, its width and height.
        # Set the background color and set it to be transparent
        self.image = pygame.Surface([width, height])
        self.image.fill(BLACK)
        self.image.set_colorkey(BLACK)

        # Draw the ball (a circle)
        pygame.draw.ellipse(self.image, color, [0, 0, width, height])

        # Fetch the rectangle object that has the dimensions of the image.
        self.rect = self.image.get_rect()

        # Reset the ball to the starting position
        self.reset()

    def update(self):
        # Update the position of the ball
        self.rect.x += self.speed_x
        self.rect.y += self.speed_y

        # Check if the ball has hit the top or bottom of the screen
        if self.rect.y < 0 or self.rect.y > SCREEN_HEIGHT - self.rect.height:
            self.speed_y *= -1

        # Check if the ball has hit the left or right paddle
        if self.rect.colliderect(player1.rect) or self.rect.colliderect(player2.rect):
            self.speed_x *= -1

    def reset(self):
        # Reset the ball to the starting position
        self.rect.x = SCREEN_WIDTH / 2
        self.rect.y = SCREEN_HEIGHT / 2
        self.speed_x = 5
        self.speed_y = 5

# Initialize Pygame
pygame.init()

# Set the height and width of the screen
SCREEN_WIDTH = 700
SCREEN_HEIGHT = 400

# Create the screen
screen = pygame.display.set_mode([SCREEN_WIDTH, SCREEN_HEIGHT])

# Create the clock
clock = pygame.time.Clock()

# Create the player paddles
player1 = Paddle(GREEN, 10, 100)
player2 = Paddle(RED, 10, 100)

# Create the ball
ball = Ball(WHITE, 10, 10)

# Game loop
while True:
    # Process events (e.g. key presses)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Clear the screen
    screen.fill(BLACK)

    # Update the player paddles
    player1.update()
    player2.update()

    # Update the ball
    ball.update()

    # Draw the player paddles
    screen.blit(player1.image, player1.rect)
    screen.blit(player2.image, player2.rect)

    # Draw the ball
    screen.blit(ball.image, ball.rect)

    # Update the display
    pygame.display.flip()

    # Limit the framerate
    clock.tick(60)