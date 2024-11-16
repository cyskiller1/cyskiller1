import pygame
import sys

# 초기화
pygame.init()

# 화면 크기 설정
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("JumpKing Style Game")

# 색상 정의
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# 게임 설정
GRAVITY = 0.5
JUMP_STRENGTH = -10
ground_height = HEIGHT - 50

# 플레이어 클래스
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 2, ground_height - 50)
        self.velocity_y = 0
        self.on_ground = False

    def update(self):
        # 중력 적용
        self.velocity_y += GRAVITY
        self.rect.y += self.velocity_y

        # 땅에 닿으면 중지
        if self.rect.bottom >= ground_height:
            self.rect.bottom = ground_height
            self.velocity_y = 0
            self.on_ground = True
        else:
            self.on_ground = False

    def jump(self):
        if self.on_ground:
            self.velocity_y = JUMP_STRENGTH

# 게임 루프 설정
def main():
    clock = pygame.time.Clock()
    player = Player()
    all_sprites = pygame.sprite.Group()
    all_sprites.add(player)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    player.jump()

        # 화면 업데이트
        all_sprites.update()

        # 배경 그리기
        screen.fill(WHITE)

        # 스프라이트 그리기
        all_sprites.draw(screen)

        # 화면 업데이트
        pygame.display.flip()

        # 프레임 속도 조절
        clock.tick(60)

# 게임 실행
if __name__ == "__main__":
    main()
