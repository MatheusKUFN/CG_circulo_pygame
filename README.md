import pygame  # Biblioteca para gerenciamento de janelas e eventos
from pygame.locals import *  # Importa constantes do Pygame
from OpenGL.GL import *  # Importa funções do OpenGL
from OpenGL.GLU import *  # Importa funções utilitárias do OpenGL
from OpenGL.GLUT import *  # Importa GLUT para utilitários gráficos do OpenGL
import math  # Importa a biblioteca matemática para cálculos trigonométricos

eiX = 0
eiY = 0
eiZ = -5
rotacao = 0

def init():
    """Configuração inicial do OpenGL."""
    glClearColor(0.0, 0.0, 0.0, 1.0)  # Define a cor de fundo como preta
    glClearDepth(1.0)  # Define a profundidade do buffer de profundidade
    glEnable(GL_DEPTH_TEST)  # Ativa o teste de profundidade para ocultação
    glDepthFunc(GL_LEQUAL)  # Define a função de profundidade como "menor ou igual"
    glMatrixMode(GL_PROJECTION)  # Define a matriz de projeção
    glLoadIdentity()  # Reseta a matriz de projeção
    gluPerspective(45.0, 640.0 / 480.0, 0.1, 100.0)  # Define a perspectiva 3D
    glMatrixMode(GL_MODELVIEW)  # Retorna à matriz de modelo/visualização

def draw_circle(radius, num_segments=100):
    """Desenha um círculo usando OpenGL."""
    glLoadIdentity()  # Reseta a matriz de modelagem
    glTranslatef(eiX, eiY, eiZ)  # Move o círculo para frente no eixo Z
    glRotatef(rotacao, 0, 1, 0)
    glColor3f(0.0, 1.0, 0.0)  # Define a cor do círculo como verde

    glBegin(GL_LINE_LOOP)  # Inicia o desenho de uma linha fechada (círculo)
    for i in range(num_segments):  # Loop para desenhar os pontos do círculo
        angle = 2.0 * math.pi * i / num_segments  # Calcula o ângulo do ponto atual
        x = radius * math.cos(angle)  # Calcula a coordenada X do ponto
        y = radius * math.sin(angle)  # Calcula a coordenada Y do ponto
        glVertex2f(x, y)  # Define o ponto no círculo
    glEnd()  # Finaliza o desenho do círculo

def main():
    global eiX, eiY, eiZ, rotacao
    """Função principal para inicializar o Pygame e renderizar o círculo."""
    pygame.init()  # Inicializa o Pygame
    pygame.display.set_mode((640, 480), DOUBLEBUF | OPENGL)  # Cria a janela com suporte a OpenGL
    init()  # Configurações iniciais do OpenGL

    running = True  # Variável de controle do loop principal
    while running:  # Loop principal do programa
        for event in pygame.event.get():  # Verifica eventos do Pygame
            if event.type == pygame.QUIT:  # Se o usuário fechar a janela
                running = False  # Sai do loop
            if event.type == KEYDOWN:  # Se uma tecla for pressionada

                if event.key == K_a:  # Se a tecla A for pressionada
                    eiX += -0.2  # Move o triângulo para a esquerda
                elif event.key == K_d:  # Se a tecla D for pressionada
                    eiX += 0.2  # Move o triângulo para a direita
                    
                elif event.key == K_w:  # Se a tecla W for pressionada
                    eiY += 0.2  # Move o triângulo para cima
                elif event.key == K_s:  # Se a tecla S for pressionada
                    eiY += -0.2  # Move o triângulo para baixo

                elif event.key == K_z:  # Se a tecla W for pressionada
                    rotacao += 5.2  # Move o triângulo para cima
                elif event.key == K_x:  # Se a tecla S for pressionada
                    rotacao += -5.2  # Move o triângulo para baixo

            if event.type == MOUSEBUTTONDOWN:  # Se o usuário rolar o mouse
                if event.button == 4:  # Scroll para cima
                    eiZ += 0.2  # Aumenta a escala do triângulo no eixo Z
                elif event.button == 5:  # Scroll para baix
                    eiZ -= 0.2  # Diminui a escala do triângulo no eixo Z

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)  # Limpa a tela e o buffer de profundidade
        draw_circle(2)  # Chama a função para desenhar o círculo com raio 2
        pygame.display.flip()  # Atualiza a tela
        pygame.time.wait(10)  # Pequeno atraso para controlar a taxa de atualização

    pygame.quit()  # Finaliza o Pygame quando o loop termina

if __name__ == "__main__":
    main()  # Executa a função principal se o script for rodado diretamente
