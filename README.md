# room
thanks
import pygame
import random

# 게임 초기화
pygame.init()

# 게임 창 설정
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("파이게임 슈팅 게임")

# 게임 색상 정의
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# 플레이어 설정
player_size = 50
player_x = width // 2 - player_size // 2
player_y = height - player_size
player_speed = 8

# 총알 설정
bullet_size = 10
bullet_speed = 10
bullet_list = []

# 적 설정
enemy_size = 50
enemy_x = random.randint(0, width - enemy_size)
enemy_y = 0
enemy_speed = 5

# 게임 루프
running = True
while running:
    # 이벤트 처리
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bullet_x = player_x + player_size // 2 - bullet_size // 2
                bullet_y = player_y
                bullet_list.append([bullet_x, bullet_y])

    # 플레이어 이동
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < width - player_size:
        player_x += player_speed

    # 총알 이동
    for bullet in bullet_list:
        bullet[1] -= bullet_speed
        if bullet[1] < 0:
            bullet_list.remove(bullet)

    # 적 이동
    enemy_y += enemy_speed
    if enemy_y > height:
        enemy_x = random.randint(0, width - enemy_size)
        enemy_y = 0

    # 충돌 검사
    for bullet in bullet_list:
        if bullet[0] < enemy_x + enemy_size and bullet[0] + bullet_size > enemy_x and \
                bullet[1] < enemy_y + enemy_size and bullet[1] + bullet_size > enemy_y:
            enemy_x = random.randint(0, width - enemy_size)
            enemy_y = 0
            bullet_list.remove(bullet)

    if player_x < enemy_x + enemy_size and player_x + player_size > enemy_x and \
            player_y < enemy_y + enemy_size and player_y + player_size > enemy_y:
        running = False

    # 화면 그리기
    screen.fill(WHITE)
    pygame.draw.rect(screen, BLUE, (player_x, player_y, player_size, player_size))
    pygame.draw.rect(screen, RED, (enemy_x, enemy_y, enemy_size, enemy_size))
    for bullet in bullet_list:
        pygame.draw.rect(screen, RED, (bullet[0], bullet[1], bullet_size, bullet_size))

    pygame.display.update()

# 게임 종료
pygame.quit()
