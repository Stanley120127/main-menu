# main-menu

import pygame, sys

WIDTH = 800
WIN = pygame.display.set_mode((WIDTH, WIDTH))

class Button():
	def __init__(self, image, pos, text_input, font, base_color, hovering_color):
		self.image = image
		self.x_pos = pos[0]
		self.y_pos = pos[1]
		self.font = font
		self.base_color, self.hovering_color = base_color, hovering_color
		self.text_input = text_input
		self.text = self.font.render(self.text_input, True, self.base_color)
		if self.image is None:
			self.image = self.text
		self.rect = self.image.get_rect(center=(self.x_pos, self.y_pos))
		self.text_rect = self.text.get_rect(center=(self.x_pos, self.y_pos))

	def update(self, screen):
		if self.image is not None:
			screen.blit(self.image, self.rect)
		screen.blit(self.text, self.text_rect)

	def checkForInput(self, position):
		if position[0] in range(self.rect.left, self.rect.right) and position[1] in range(self.rect.top, self.rect.bottom):
			return True
		return False

	def changeColor(self, position):
		if position[0] in range(self.rect.left, self.rect.right) and position[1] in range(self.rect.top, self.rect.bottom):
			self.text = self.font.render(self.text_input, True, self.hovering_color)
		else:
			self.text = self.font.render(self.text_input, True, self.base_color)


pygame.init()

screen = pygame.display.set_mode((1280, 720))
pygame.display.set_caption("Shortest Path Algorithm")

background = pygame.image.load("Background.png")


def get_font(size):  # Returns Press-Start-2P in the desired size
    return pygame.font.Font("font.ttf", size)


def dijkstras():
    while True:
        # dijkstras_mouse_pos = pygame.mouse.get_pos()

        import Dijkstra
        Dijkstra.main()




        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    main_menu()


        pygame.display.update()


def a_star():
    while True:
        # astar_mouse_pos = pygame.mouse.get_pos()

        import astar
        astar.main(WIN,WIDTH)



        # option_back = Button(image=None, pos=(640, 460),
                       #       text_input="BACK", font=get_font(75), base_color="Black", hovering_color="Green")

        # option_back.changeColor(OPTIONS_MOUSE_POS)
        # option_back.update(screen)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            # if event.type == pygame.MOUSEBUTTONDOWN:
                # if OPTIONS_BACK.checkForInput(OPTIONS_MOUSE_POS):
                   # main_menu()

        pygame.display.update()


def main_menu():
    while True:
        screen.blit(background, (0, 0))

        menu_mouse_pos = pygame.mouse.get_pos()

        menu_text = get_font(100).render("Menu", True, "#b68f40")
        menu_rect = menu_text.get_rect(center=(640, 100))

        dijkstras_button = Button(image=pygame.image.load("Dijkstras Rect.png"), pos=(640, 250),
                                  text_input="Dijkstra", font=get_font(75), base_color="#d7fcd4", hovering_color="White")
        astar_button = Button(image=pygame.image.load("Astar Rect.png"), pos=(640, 400),
                              text_input="Astar", font=get_font(75), base_color="#d7fcd4", hovering_color="White")
        quit_button = Button(image=pygame.image.load("Quit Rect.png"), pos=(640, 550),
                             text_input="Quit", font=get_font(75), base_color="#d7fcd4", hovering_color="White")

        screen.blit(menu_text, menu_rect)

        for button in [dijkstras_button, astar_button, quit_button]:
            button.changeColor(menu_mouse_pos)
            button.update(screen)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.MOUSEBUTTONDOWN:
                if dijkstras_button.checkForInput(menu_mouse_pos):
                    dijkstras()
                if astar_button.checkForInput(menu_mouse_pos):
                    a_star()
                if quit_button.checkForInput(menu_mouse_pos):
                    pygame.quit()
                    sys.exit()

        pygame.display.update()


main_menu()
