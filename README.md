g++ filename.c -lGL -lGLU -lglut
./a.out

1) DDA Line::::::::::::::::::::::::

#include<GL/glut.h>
#include<stdlib.h>
#include<stdio.h>
float x1,x2,s,y2;
void display()
{
float dy,dx,step,x,y,k,Xin,Yin;
dx=x2-x1;
dy=y2-s;
if(abs(dx)>abs(dy))
{
step=abs(dx);}
else
step=abs(dy);
Xin=dx/step;
Yin=dy/step;
x=x1;
y=s;
glBegin(GL_POINTS);
glVertex2i(x,y);
glEnd();
for (k=1;k<=step;k++)
{
   x=x+Xin;
   y=y+Yin;
glBegin(GL_POINTS);
glVertex2i(x,y);
glEnd();
}
  glFlush();
}
void init(void)
{
glClearColor(1.0,1.0,1.0,1.0);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(-100,100,-100,100);
}
 
int main(int argc, char**argv){
printf("enter the value of x1:");
scanf("%f",&x1);
printf("enter the value of Y1:");
scanf("%f",&s);
printf("enter the value of x2:");
scanf("%f",&x2);
printf("enter the value of y2:");
scanf("%f",&y2);
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(500,500);
glutInitWindowPosition(100,100);
glutCreateWindow("DDA Line Algo");
init();
glutDisplayFunc(display);
glutMainLoop();
return 0;
}


2) Bresenham's Line::::::::::::::::::::::::::

#include<GL/glut.h>
#include<math.h>
#include<stdlib.h>
#include<iostream>
#include<stdio.h>
using namespace std;
int sign(float arg)
{
if(arg<0)
return -1;
else if(arg==0)
return 0;
else
return 1;
}
void Bre(int X1,int Y1,int X2,int Y2)
{
float dx=abs(X2-X1);
float dy=abs(Y2-Y1);
int s1,s2,exc,y,x,i;
float g,temp;
x=X1;
y=Y1;
s1=sign(X2-X1);
s2=sign(Y2-Y1);
glBegin(GL_POINTS);
if(dy>dx)
{
temp=dx;
dx=dy;
dy=temp;
exc=1;
}
else
{
exc=0;
}
g=2*dy-dx;
i=1;
while(i<=dx)
{
glVertex2d(x,y);
while(g>=0)
{
if(exc==1)
x=x+s1;
else
y=y+s2;
g=g-2*dx;
}
if(exc==1)
y=y+s2;
else
x=x+s1;
g=g+2*dy;
i++;
}
glEnd();
glFlush();
}
void display()
{Bre(40,40,200,200);
Bre(250,4000,100,100);
}
void myInit(void)
{
glClearColor(1.0,1.0,1.0,0.0);
glColor3f(1,1,1);
glPointSize(1.0);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0.0,640.0,0.0,480.0);
}
int main(int argc,char **argv)
{
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(640,480);
glutInitWindowPosition(100,150);
glutCreateWindow("Bresenham Line ");
glutDisplayFunc(display);
myInit();
glutMainLoop();
return 0;
}
      

3) Bresenham's Circle:::::::::::::::::::::::::::::::::

#include<GL/glut.h>
#include<stdlib.h>
#include<stdio.h>
void symmetry(double xc,double yc,double x,double y)
{
glBegin(GL_POINTS);
glVertex2i(xc+x,yc+y);
glVertex2i(xc+x,yc-y);
glVertex2i(xc+y,yc+x);
glVertex2i(xc+y,yc-x);
glVertex2i(xc-x, yc-y);
glVertex2i(xc-x, yc+y);
glVertex2i(xc-y, yc-x);
glVertex2i(xc-y, yc+x);
glEnd();
}
void circle(double x1,double y1,double r)
{
int x=0,y=r;
float p=(5.0/4.0)-r;
symmetry(x1,y1,x,y);
while(x<y)
{
x=x+1;
if(p<0)
{
p=p+2*x+1;
}
else
{
y=y-1;
p=p+2*(x-y)+1;
}
symmetry(x1,y1,x,y);
}
glFlush();
}
void display()
{
circle(200,200,120);
}
void init(void)
{
glClearColor(1.0,1.0,1.0,1.0);
glColor3f(1,1,1);
glPointSize(2);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0.0,640.0,0.0,480.0);
}
int main(int argc,char**argv){
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(500,500);
glutInitWindowPosition(100,100);
glutCreateWindow("circle");
init();
glutDisplayFunc(display);
glutMainLoop();
return 0;
}


4) Polygon Filling::::::::::::::::::::::::::::::

#include<iostream>
#include<math.h>
#include<GL/glut.h>
using namespace std;
int a, b, Dx, Dy, temp, interchange, e;
int s1, s2;
void myInit() {
glClearColor(1.0, 1.0, 1.0, 0.0);
glMatrixMode(GL_PROJECTION);
gluOrtho2D(0.0, 640.0, 0.0, 480.0);
}
int sign(int a) {
if (a > 0) {
return 1;
}
else {
return -1;
}
}
void plot(int a, int b) {
glBegin(GL_POINTS);
glVertex2i(a, b);
glEnd();
glFlush();
}
void DrawPolygon(int x, int y, int m, int n) {
a = x; b = y;
Dx = abs(m - x);
Dy = abs(n - y);
s1 = sign(m - x);
s2 = sign(n - y);
if (Dy > Dx) {
temp = Dx;
Dx = Dy;
Dy = temp;
interchange = 1;
}
else {
interchange = 0;
}
e = 2 * Dy - Dx;
plot(a, b);
for (int i = 1; i <= Dx; i++) {
plot(a, b);
while (e >= 0) {
if (interchange == 1)
a = a + s1;
else
b = b + s2;
e = e - 2 * Dx;
}
if (interchange == 1) {
b = b + s2;}
else {
a = a + s1;
}
e = e + 2 * Dy;
}
}
void putpixel(int c, int d, float* fillColor) {
glColor3f(fillColor[0], fillColor[1], fillColor[2]);
glBegin(GL_POINTS);
glVertex2i(c, d);
glEnd();
glFlush();
}
void boundaryFill4(int x, int y, float* fillColor, float* boundarycolor) { //Boundary fill algorithm
float color[3];
glReadPixels(x, y, 1.0, 1.0, GL_RGB, GL_FLOAT, color);
if ((color[0] != boundarycolor[0] || color[1] != boundarycolor[1] || color[2] !=
boundarycolor[2]) && (
color[0] != fillColor[0] || color[1] != fillColor[1] || color[2] != fillColor[2])) {
putpixel(x, y, fillColor);
boundaryFill4(x + 1, y, fillColor, boundarycolor);
boundaryFill4(x - 2, y, fillColor, boundarycolor);
boundaryFill4(x, y + 2, fillColor, boundarycolor);
boundaryFill4(x, y - 2, fillColor, boundarycolor);
}
}void Floodfill4(int x, int y, float* fillColor, float* interiorColor) {
float color[3];
glReadPixels(x, y, 1.0, 1.0, GL_RGB, GL_FLOAT, color);
if (color[0] == interiorColor[0] && color[1] == interiorColor[1] && color[2] == interiorColor[2]) {
putpixel(x, y, fillColor);
Floodfill4(x + 1, y, fillColor, interiorColor);
Floodfill4(x - 1, y, fillColor, interiorColor);
Floodfill4(x, y + 1, fillColor, interiorColor);
Floodfill4(x, y - 1, fillColor, interiorColor);
}
}
void myMouse(int button, int state, int x, int y) {
y = 480 - y;
if (button == GLUT_LEFT_BUTTON)
{
if (state == GLUT_DOWN)
{
float boundaryCol[] = { 0,1,0 };
float fillcolor[] = { 1,0,1 };
boundaryFill4(x, y, fillcolor, boundaryCol);
}
}
}
void myMouse1(int button, int state, int x, int y) {
y = 480 - y;
if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) {
float interiorcolor[] = { 1,1,1 };float fillcolor[] = { 1,1,0 };
Floodfill4(x, y, fillcolor, interiorcolor);
}
}
void myDisplay() {
glLineWidth(3.0); glPointSize(2.0);
glClear(GL_COLOR_BUFFER_BIT); glColor3f(0, 1, 0);
DrawPolygon(100, 100, 200, 200);
DrawPolygon(200, 200, 400, 200);
DrawPolygon(400, 200, 400, 100);
DrawPolygon(400, 100, 100, 100); glEnd();
glFlush();
}
void myMenu(int item) {
if (item == 1) {
glutMouseFunc(myMouse);
}
else if (item == 2) {
glutMouseFunc(myMouse1);
}
}
int main(int argc, char** argv) {
glutInit(&argc, argv);
glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
glutInitWindowSize(640, 480);
glutInitWindowPosition(200, 200);
glutCreateWindow("Polygon Filling");glutDisplayFunc(myDisplay);
glutCreateMenu(myMenu);
glutAttachMenu(GLUT_RIGHT_BUTTON);
glutAddMenuEntry("Boundary fill",1);
glutAddMenuEntry("Flood fill",2);
myInit();
glutMainLoop();
return 0;
}
