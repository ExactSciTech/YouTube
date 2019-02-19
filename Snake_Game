import pygame as pg
import random

TITLE = "Snake"
WIDTH = 640
HEIGHT = 480
FPS = 15

# define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)
YELLOW = (255, 255, 0)

class Player(pg.sprite.Sprite):
    def __init__(self, game):
        pg.sprite.Sprite.__init__(self)
        self.game = game
        self.image = pg.Surface((20,20)) #32 x 24 plane for the snake
        self.image.fill(YELLOW)
        self.rect = self.image.get_rect()
        self.rect.topleft = (WIDTH / 2, HEIGHT / 2)
        self.vel = [0,0]
        self.length = 0
        self.tail = self.game.tail.sprites
        self.last_pos = self.rect.center

    def update(self):
        self.last_pos = self.rect.center
        self.rect.centerx += self.vel[0]
        self.rect.centery += self.vel[1]

        if self.rect.left < 0 or self.rect.right > WIDTH or self.rect.top < 0 or self.rect.bottom > HEIGHT:
            self.game.playing = False

class Tail(pg.sprite.Sprite):
    def __init__(self, game, index):
        pg.sprite.Sprite.__init__(self)
        self.game = game
        self.image = pg.Surface((20,20)) #32 x 24 plane for the snake
        self.image.fill(BLUE)
        self.rect = self.image.get_rect()
        self.index = index
        self.last_pos = self.rect.center

    def update(self):
        self.last_pos = self.rect.center
        if self.index == 0:
            self.rect.center = self.game.player.last_pos
        else:
            self.rect.center = self.game.tail.sprites()[self.index-1].last_pos

class Fruit(pg.sprite.Sprite):
    def __init__(self):
        pg.sprite.Sprite.__init__(self)
        self.image = pg.Surface((20,20)) #32 x 24 plane for the snake
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.new_pos()

    def new_pos(self):
        self.rect.left = 20*(random.randrange(10, WIDTH-10)//20)
        self.rect.top = 20*(random.randrange(10, HEIGHT-10)//20)

class Game:
    def __init__(self):
        # initialize game window, etc
        pg.init()
        self.screen = pg.display.set_mode((WIDTH, HEIGHT))
        pg.display.set_caption(TITLE)
        self.clock = pg.time.Clock()
        self.running = True

    def new(self):
        # start a new game
        self.tail = pg.sprite.Group()
        self.all_sprites = pg.sprite.Group()
        self.player = Player(self)
        self.all_sprites.add(self.player)
        self.fruit = Fruit()
        self.all_sprites.add(self.fruit)
        self.run()

    def run(self):
        # Game Loop
        self.playing = True
        self.frame = 0
        while self.playing:
            self.clock.tick(FPS)
            if self.player.vel: self.frame += 1
            self.events()
            self.update()
            self.draw()

    def update(self):
        # Game Loop - Update
        self.all_sprites.update()
        self.tail.update()

        if pg.sprite.spritecollide(self.player, self.tail, True):
            self.playing = False

        if pg.sprite.collide_rect(self.player, self.fruit):
            self.player.length += 1
            self.fruit.new_pos()
            if self.tail.sprites():
                self.tail.add(Tail(self, self.tail.sprites()[-1].index+1))
            else: self.tail.add(Tail(self, 0))

    def events(self):
        # Game Loop - events
        for event in pg.event.get():
            # check for closing window
            if event.type == pg.QUIT:
                if self.playing:
                    self.playing = False
                self.running = False
            if event.type == pg.KEYDOWN:
                keys = pg.key.get_pressed()
                if not self.player.vel[1]:
                    if keys[pg.K_UP]: self.player.vel = [0,-20]
                    if keys[pg.K_DOWN]: self.player.vel = [0,20]
                if not self.player.vel[0]:
                    if keys[pg.K_RIGHT]: self.player.vel = [20,0]
                    if keys[pg.K_LEFT]: self.player.vel = [-20,0]

    def score(self):
        return int(self.frame**0.1 * self.player.length)

    def draw(self):
        # Game Loop - draw
        self.screen.fill(BLACK)
        self.all_sprites.draw(self.screen)
        self.tail.draw(self.screen)
        self.draw_text(f"Score: {self.score()}", 40, 10,10)
        # *after* drawing everything, flip the display
        pg.display.flip()

    def draw_text(self, text, size, x, y, color = WHITE):
        self.screen.blit(pg.font.SysFont(pg.font.match_font('arial', False, False), size).render(text, True, color),(x, y))

g = Game()
while g.running:
    g.new()

pg.quit()
