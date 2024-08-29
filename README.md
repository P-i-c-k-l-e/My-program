'''
This quiz is about the show, The Wotwots
By Bailey M
22/07/24
Version: 1.3 

'''
import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants    
WIDTH, HEIGHT = 800, 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
FONT_SIZE = 36

# Setup display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('The Wotwots Quiz')
font = pygame.font.SysFont(None, FONT_SIZE)

def draw_text(text, font, color, surface, x, y):
    """Helper function to draw text on the screen."""
    textobj = font.render(text, True, color)
    textrect = textobj.get_rect()
    textrect.center = (x, y)
    surface.blit(textobj, textrect)

def ask_question(question, correct_answers):
    """Displays a question and checks if the answer is correct."""
    input_box = pygame.Rect(WIDTH // 4, HEIGHT // 2 + 100, WIDTH // 2, FONT_SIZE + 10)
    color_inactive = pygame.Color('lightskyblue3')
    color_active = pygame.Color('dodgerblue2')
    color = color_inactive
    active = True  # Start with input active
    text = ''
    txt_surface = font.render(text, True, color)
    width = max(200, txt_surface.get_width()+10)
    input_box.w = width

    while True:
        screen.fill(WHITE)
        draw_text(question, font, BLACK, screen, WIDTH // 2, HEIGHT // 4)
        screen.blit(txt_surface, (input_box.x+5, input_box.y+5))
        pygame.draw.rect(screen, color, input_box, 2)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if active:
                    if event.key == pygame.K_RETURN:
                        if text.strip().lower() in [answer.lower() for answer in correct_answers]:
                            return True
                        else:
                            return False
                    elif event.key == pygame.K_BACKSPACE:
                        text = text[:-1]
                    else:
                        text += event.unicode
                    txt_surface = font.render(text, True, color)
                if event.key == pygame.K_ESCAPE:
                    pygame.quit()
                    sys.exit()

        pygame.display.flip()

def start_quiz():
    """Starts the quiz and evaluates the user's performance."""
    questions = [
        ("What is the name of the blue character in The Wotwots?", ["spottywot"]),
        ("Now, what is the name of the pink one?", ["dottywot"]),
        ("What colour are the Wotwots?", ["blue and pink", "pink and blue", "blue", "pink"]),
        ("What is the name of the drinks the Wotwots make?", ["fruity loopies"]),
        ("Final question, what is the name of the helicopter called in the show?", ["heli"]),
        # Add more questions here
    ]

    score = 0

    for question, correct_answers in questions:
        if ask_question(question, correct_answers):
            screen.fill(WHITE)
            draw_text("Correct!", font, BLACK, screen, WIDTH // 2, HEIGHT // 2)
            pygame.display.flip()
            pygame.time.wait(1000)
            score += 1
        else:
            screen.fill(WHITE)
            draw_text("Wrong!", font, BLACK, screen, WIDTH // 2, HEIGHT // 2)
            pygame.display.flip()
            pygame.time.wait(1000)

    screen.fill(WHITE)
    draw_text(f"Quiz completed. Your score is {score} out of {len(questions)}.", font, BLACK, screen, WIDTH // 2, HEIGHT // 2)
    pygame.display.flip()
    pygame.time.wait(3000)

def main():
    """Main function to handle user input and start the quiz."""
    running = True
    while running:
        screen.fill(WHITE)
        draw_text("Welcome to the quiz about the show, The Wotwots", font, BLACK, screen, WIDTH // 2, HEIGHT // 3)
        draw_text("Please answer 'yes' if you would like to start the quiz. If not, answer 'no'", font, BLACK, screen, WIDTH // 2, HEIGHT // 2)
        pygame.display.flip()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_y:
                    start_quiz()
                    running = False
                elif event.key == pygame.K_n:
                    screen.fill(WHITE)
                    draw_text("You have ended the quiz. Thank you for checking it out.", font, BLACK, screen, WIDTH // 2, HEIGHT // 2)
                    pygame.display.flip()
                    pygame.time.wait(2000)
                    pygame.quit()
                    sys.exit()

if __name__ == "__main__":
    main()
