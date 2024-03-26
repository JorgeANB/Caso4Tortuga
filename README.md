#include <GL/glut.h>
#include <iostream>

int screenWidth, screenHight;

void getScreenResolution() {
	// Obtiene la resolución de la pantalla:
	screenWidth = glutGet(GLUT_SCREEN_WIDTH);
	screenHight = glutGet(GLUT_SCREEN_HEIGHT);
	std::cout << "Resolución de pantalla: " << screenWidth << "x" << screenHight << std::endl;
}
struct Turtle
{
	float x, y; // Posición de la tortuga
	float angle; // Ángulo de orientación de la tortuga
};
Turtle turtle = { 0.0, 0.0, 0.0 }; // Inicialización de la tortuga

void moveTurtle(float dx, float dy) {
	turtle.x += dx;
	turtle.y += dy;
}

void rotateTurtle(float angle) {
	turtle.angle += angle;
}

void drawTurtle() {
	glClear(GL_COLOR_BUFFER_BIT);
	glLoadIdentity();

	// Aplica las transformaciones
	glTranslatef(turtle.x, turtle.y, 0.0);
	glRotatef(turtle.angle, 0.0, 0.0, 1.0);

	// Dibujar caparazón de la tortuga
	float colors[13][3] = {
		{1.0, 0.0, 0.0}, // Rojo
		{1.0, 0.5, 0.0}, // Naranja
		{1.0, 1.0, 0.0}, // Amarillo
		{0.0, 1.0, 0.0}, // Verde
		{0.0, 0.8, 0.8}, // Cyan
		{0.0, 0.0, 1.0}, // Azul
		{0.5, 0.0, 1.0}, // Morado
		{1.0, 0.0, 1.0}, // Magenta
		{0.8, 0.0, 0.0}, // Marrón
		{0.8, 0.8, 0.8}, // Gris
		{0.0, 1.0, 1.0}, // Aqua
		{0.6, 0.3, 0.1}, // Café
		{0.3, 0.3, 0.3}, // Gris oscuro
	};

	// Utilizamos GL_TRIANGLES_fan para dibujar cada sección del caparazón
	int numSections = 13;
	float sectionWidth = 0.1;
	for (int i = 0; i < numSections; i++)
	{
		glColor3fv(colors[i]);
		glBegin(GL_TRIANGLE_FAN);
		glVertex2f(-0.5 + i * sectionWidth, 0.3); // Vértice superior
		for (int j = 0; j < numSections; j++)
		{
			float angle = j * (3.14159265359 / 10);
			glVertex2f(-0.5 + i * sectionWidth + 0.05 * cos(angle), 0.3 + 0.05 * sin(angle));
		}
		glEnd();
	}

	// Dibujar cabeza de la tortuga
	glColor3f(0.0, 1.0, 0.0); // Color verde
	glBegin(GL_TRIANGLES);
	glVertex2f(0.0, 0.2); // Vértice superior
	glVertex2f(-0.1, -0.1); // Vértice inferior izquierdo
	glVertex2f(0.1, -0.1); // Vértice inferior derecho
	glEnd();

	// Dibujar cuerpo de la tortuga
	glColor3f(0.0, 0.0, 1.0); //Color azul
	glBegin(GL_QUAD_STRIP);
	glVertex2f(-0.05, 0.1); // Extremo izquierdo superior
	glVertex2f(0.05, 0.1); // Extremo derecho superior
	glVertex2f(-0.05, -0.2); // Extremo izquierdo inferior
	glVertex2f(0.05, -0.2); // Extremo derecho inferior
	glEnd();

	// Dibujar extremidades de la toruga
	glColor3f(1.0, 0.0, 0.0); // Color rojo
	glBegin(GL_TRIANGLE_STRIP);

	// Extremidades izquierdas
	glVertex2f(-0.05, 0.1); // Frente izquierdo
	glVertex2f(-0.08, 0.0); // Trasero izquierdo
	glVertex2f(-0.02, 0.0); //Inferior izquierdo

	// Extremidades derechas
	glVertex2f(0.05, 0.1); // Frente derecho
	glVertex2f(0.08, 0.0); // Trasero derecho
	glVertex2f(0.02, 0.0); //Inferior derecho
	glEnd();

	// Dibujar ojos de la tortuga
	glColor3f(0.3, 0.3, 0.3); // Color gris obscuro
	glBegin(GL_POINTS);
	glVertex2f(-0.03, 0.25); // Ojo izquierdo
	glVertex2f(0.03, 0.25); // Ojo derecho
	glEnd();

	glFlush();
}

void keyboard(unsigned char key, int x, int y) {
	switch (key)
	{
	case 'w': // Tecla 'w' para mover hacia arriba
		moveTurtle(0.0, 0.1);
		break;
	case 's': // Tecla 's' para mover hacia abajo
		moveTurtle(0.0, -0.1);
		break;
	case 'a': // Tecla 'a' para rotar hacia la izquierda
		rotateTurtle(10.0);
		break;
	case 'd':
		rotateTurtle(-10.0);
		break;
	case 'q': // Tecla 'q' para salir
		exit(0);
		break;
	}
	glutPostRedisplay(); //Vuelve a dibujar la pantalla
}

void specialKeys(int key, int x, int y) {
	// Define la distancia de movimiento
	float moveDistance = 0.1;

	// mueve la tortuga según la tecla presionada
	switch (key)
	{
	case GLUT_KEY_UP: // Tecla de flecha arriba
		moveTurtle(0.0, moveDistance);
		break;
	case GLUT_KEY_DOWN: // Tecla de flecha abajo
		moveTurtle(0.0, -moveDistance);
		break;
	case GLUT_KEY_LEFT: // Tecla de flecha izquierda
		moveTurtle(-moveDistance, 0.0);
		break;
	case GLUT_KEY_RIGHT: // Tecla de flecha derecha
		moveTurtle(moveDistance, 0.0);
		break;
	}
	glutPostRedisplay(); // Vuelve a dibujar la pantalla
}

int main(int argc, char** argv) {
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(screenWidth, screenHight);
	glutCreateWindow("Tortuga con transformaciones");

	getScreenResolution();

	glutDisplayFunc(drawTurtle);
	glutKeyboardFunc(keyboard);
	glutSpecialFunc(specialKeys);
	glutMainLoop();

	return 0;
}
