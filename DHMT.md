[TOC]



## **LAB 01** – *Sử dụng thuật toán Bressenham và Midpoint để cài đặt các thuật toán*

*vẽ các đường sau:*
1.1.Thuật toán vẽ đoạn thẳng (xét 4 trường hợp của hệ số góc)
1.2.Thuật toán vẽ đường tròn
1.3.Thuật toán vẽ ellipse
1.4.Vẽ các đường cong khác: parabol, hyperbol, sin(x), cos(x)
*Yêu cầu*: Sử dụng sự kiện click chuột để xác định đoạn thẳng, đường tròn,
ellipse,… cần vẽ

### **1.1 ve bang bresenham**

<img src="1.jpg" alt="img" style="zoom:120%;" />



*(y-y1)/( x-x1) = ( y2-y1)/( x2-x1) hay y = mx + b*
* m = (y2-y1)/( x2-x1)*
* b = y1- m.x1*
* Δy = m Δx*

y=2x+1:

![Anh ve thuat toan](2.PNG)



```c
#include <GL/glut.h>

#include <math.h>

#include <stdlib.h>

int ww = 600, wh = 400;

int first = 0;

int xi, yi, xf, yf;

void putPixel(int x, int y) {
  glColor3f(0.3, 0.2, 0.0); 
  glBegin(GL_POINTS);
  glVertex2i(x, y);    
  glEnd();
  glFlush(); 
}
void display() {
  glClearColor(0.4, 0.7, 0.5, 1.0);
  glColor3f(0.2, 0.3, 0.3);
  glClear(GL_COLOR_BUFFER_BIT);
  glFlush();
}

void bresenhamAlg(int x0, int y0, int x1, int y1) {
  int dx = abs(x1 - x0);
  int dy = abs(y1 - y0);
  
  int x, y;
  
  if (dx >= dy)
  {
    int d = 2 * dy - dx;
    int ds = 2 * dy;
    int dt = 2 * (dy - dx);

    if (x0 < x1)
    {
      x = x0;
      y = y0;
    } else
    {
      x = x1;
      y = y1;
      x1 = x0;
      y1 = y0;
    }
    putPixel(x, y);    
    while (x < x1)
    {
      if (d < 0)
        d += ds;
      else {
        if (y < y1) {
          y++;
          d += dt;
        } else {
          y--;
          d += dt;
        }
      }
      x++;
      putPixel(x, y);
    }
  }
  //truong hop con lai 
  else {
    int d = 2 * dx - dy;
    int ds = 2 * dx;
    int dt = 2 * (dx - dy);
    if (y0 < y1) {
      x = x0;
      y = y0;
    } else {
      x = x1;
      y = y1;
      y1 = y0;
      x1 = x0;
    }
    putPixel(x, y);
    while (y < y1)
    {
      if (d < 0)
        d += ds;
      //ve theo chieu xuong duoi
      //...  
      else {
        if (x > x1) {
          x--;
          d += dt;
        } else {
          x++;
          d += dt;
        }
      }
      y++;
      putPixel(x, y);
    }
  }

}

void mouse(int btn, int state, int x, int y) {

  if (btn == GLUT_LEFT_BUTTON && state == GLUT_DOWN)

  {
    switch (first)
    {
    case 0:
      xi = x;
      yi = (wh - y);
      first = 1;
      break;
    case 1:
      xf = x;
      yf = (wh - y);
      bresenhamAlg(xi, yi, xf, yf);
      first = 0;
      break;
    }
  }
}

void myinit() {
  glViewport(0, 0, ww, wh);
  glMatrixMode(GL_PROJECTION);
  glLoadIdentity();
  gluOrtho2D(0.0, (GLdouble) ww, 0.0, (GLdouble) wh);
  glMatrixMode(GL_MODELVIEW);
}
int main(int argc, char ** argv) {
  glutInit( & argc, argv);
  glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
  glutInitWindowSize(ww, wh);
  glutCreateWindow("Lab1: Bresenham Line Algorithm");
  glutDisplayFunc(display);
  myinit();
  glutMouseFunc(mouse);
  glutMainLoop();
  return 0;
}
```

### 1.2.Thuật toán vẽ đường tròn



![](3.PNG)

![](4.PNG)

![](5.PNG)

```c
#include <GL/glut.h>

#include <math.h>

#include <stdlib.h>

int ww = 800, wh = 800;

int first = 0;

int xi, yi, xf, yf;

void putpixel(int x, int y) {
  glColor3f(0.3, 0.2, 0.0); 
  glBegin(GL_POINTS);
  glVertex2i(x, y);    
  glEnd();
  glFlush(); 
}
void Bres_Circle(int r){
    int x,y,p;
    x=0;y=r;p=3-2*r;
    while(x<=y){
        putpixel(150+x,150+y);
        putpixel(150-x,150+y);
        putpixel(150-x,150-y);
        putpixel(150+x,150-y);
        putpixel(150+y,150+x);
        putpixel(150+y,150-x);
        putpixel(150-y,150+x);
        putpixel(150-y,150-x);
        if(p<0) p+=4*x+6;
        else{
            p+=4*(x-y)+10;
            y--;
        }
        x++;
    }
}
void MidPoint_Circle(int r){
  int x=0,y=r,p=3-2*r;
  while(x<=y){
    putpixel(150+x,150+y);
    putpixel(150-x,150+y);
    putpixel(150-x,150-y);
    putpixel(150+x,150-y);
    putpixel(150+y,150+x);
    putpixel(150+y,150-x);
    putpixel(150-y,150+x);
    putpixel(150-y,150-x);

    if(p<0)
      p=p+2*x+3;
    else{
      p=p+2*(x-y)+5;
      y--;
    }  
    x++;
  }
}
void display() {
  glClearColor(0.4, 0.7, 0.5, 1.0);
  glColor3f(0.2, 0.3, 0.3);
  glClear(GL_COLOR_BUFFER_BIT);
  glFlush();
}


void mouse(int btn, int state, int x, int y) {

  if (btn == GLUT_LEFT_BUTTON && state == GLUT_DOWN)

  {
    xi=0;
    yi=y;
    MidPoint_Circle(yi);
  }
  if(btn == GLUT_RIGHT_BUTTON && state == GLUT_DOWN){
  	xi=0;
    yi=y;
    Bres_Circle(yi);
  }
}

void myinit() {
  glViewport(0, 0, ww, wh);
  glMatrixMode(GL_PROJECTION);
  glLoadIdentity();
  gluOrtho2D(0.0, (GLdouble) ww, 0.0, (GLdouble) wh);
  glMatrixMode(GL_MODELVIEW);
}
int main(int argc, char ** argv) {
  glutInit( & argc, argv);
  glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
  glutInitWindowSize(ww, wh);
  glutCreateWindow("Lab2: Midpoinr Circle Algorithm");
  glutDisplayFunc(display);
  myinit();
  glutMouseFunc(mouse);
  glutMainLoop();
  return 0;
}
```

### 1.3.Thuật toán vẽ ellipse

![](7.PNG)

![](8.PNG)

![](12.PNG)

![](9.PNG)

![](10.PNG)

![](11.PNG)

```c
#include <GL/glut.h>
#include <math.h>  
#include <stdio.h>
#include <iostream>
using namespace std;
bool first = true;
int X1 = -1, X2 = -1 , Y1 = -1, Y2 =-1 ;
int my_a,my_b;
void Ve4diem(int xc,int yc,int x, int y)
{
	glColor3f(1.0, 0.0, 0.0);
	glBegin(GL_POINTS);
    glVertex3i(xc+x,yc+y,0);
    glVertex3i(xc-x,yc+y,0);
    glVertex3i(xc-x,yc-y,0);
    glVertex3i(xc+x,yc-y,0);
	glEnd();
	glFlush();
}
float tinhKhoangCach(int x1, int y1, int x2, int y2){
	return sqrt( (x2 - x1)* (x2 - x1) + (y2 - y1)* (y2 - y1) );
}

void veEllip(int xtam, int ytam, int a, int b){
	float p , a2 , b2;
	int x,y;
	a2 = a*a;
	b2 = b*b;
	x = 0 ;
	y = b;
	p=2*((float)b2/a2)-(2*b)+1;//d1-d2
	//ve nhanh tu tren xuong
	while(((float)b2/a2)*x <= y ){
		Ve4diem(xtam, ytam, x ,y);
		if (p < 0){
			p = p + 2* ((float)b2/a2) *(2*x + 3);
		}else {
			p = p - 4*y + 2*((float)b2/a2) * (2*x +3);
			y--;
		}
		x++;
	}
	//ve nhanh tu duoi len
	y = 0;
	x = a;	
	p=2*((float)a2/b2)-2*a+1;
	while(((float)a2/b2)*y<=x)
    {
        Ve4diem(xtam,ytam,x,y);
        if(p<0)
        {
            p=p +2*((float)a2/b2)*(2*y+3);
        }
        else
        {
            p=p- 4*x + 2*((float)a2/b2)*(2*y+3);
            x=x-1;
        }
        y=y+1;
    }
    
    
}
void mouse(int button, int state , int x , int y) {	
	int mousex = x ;
	int mousey = 600-y;	
		switch(button){
		case GLUT_LEFT_BUTTON:
			if (state == GLUT_DOWN) {
			if (first == true ){
			X1 = mousex ;
			Y1 = mousey;
			glBegin(GL_POINTS);
    		glVertex2f(X1,Y1);
			glEnd();
			veEllip(X1,Y1,my_a,my_b);	
			break;
			}
		}
		case GLUT_RIGHT_BUTTON:
			if (state == GLUT_DOWN) {
			glClear(GL_COLOR_BUFFER_BIT ); 
			glColor3f(1.0, 0.0, 0.0);				
		 	glFlush();
			break;
			}
		}
}


void display(){
	glClear(GL_COLOR_BUFFER_BIT);
	glClearColor(1.0 ,1.0,1.0,1.0);
	glColor3f(1.0, 0.0, 0.0);
	glOrtho(0 , 600, 0, 600 , -1.0 , 1.0);
	glFlush(); 
}


int main(){
	cout << "Nhap a (vd: 120): "; cin>>my_a;
    cout << "Nhap b (vd: 30):";cin>>my_b;
	glutInitDisplayMode (GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize (600, 600); 
	glutInitWindowPosition (0, 0);
	glutCreateWindow("VE ELLIP BRE");
	glColor3f(1.0, 1.0, 1.0); 
	glutDisplayFunc(display);			
	glutMouseFunc(mouse);	
	glutMainLoop();		
}



```

**Ve bang middpoint**

```c
#include <GL/glut.h>
#include <math.h>  
bool first = true;
int X1 = -1, X2 = -1 , Y1 = -1, Y2 =-1 ;
void Ve4diem(int xc,int yc,int x, int y)
{
	glColor3f(1.0, 0.0, 0.0);
	glBegin(GL_POINTS);
    glVertex3i(xc+x,yc+y,0);
    glVertex3i(xc-x,yc+y,0);
    glVertex3i(xc-x,yc-y,0);
    glVertex3i(xc+x,yc-y,0);
	glEnd();
	glFlush();
}
float tinhKhoangCach(int x1, int y1, int x2, int y2){
	return sqrt( (x2 - x1)* (x2 - x1) + (y2 - y1)* (y2 - y1) );
}

void veEllip(int xtam, int ytam, int a, int b){
 int x, y, fx, fy, a2, b2, p,x0,y0;
    x = 0;
    y = b;
    a2 = a*a;
    b2 = b*b;
    fx = 0;
    fy = 2 * a2 * y;   
    Ve4diem(xtam ,ytam, x, y);
    x0=(int)(a2/sqrt(a2+b2));
    y0=(int)(b2/sqrt(a2+b2));
    p=b2 - a2*b +a2/4 ; //p1 tai(o,b)
    while(x<=x0)
    {
        Ve4diem(xtam, ytam, x, y);
        if (p<0) p+=(2*x+3)*b2;
        else{ p+=(2*x+3)*b2-2*a2*(y-1);
			y--;
		}
        x++;
    }
    //p = b2*(x +0.5)*(x +0.5) + a2*(y-1)*(y-1) - a2*b2;
	x=a; y=0; p=a2-a*b2+(1/4)*b2;
    while(y<=y0)
    {
        Ve4diem(xtam, ytam, x, y);
        if (p<0) p+=a2*(2*y+3);
        else{ p+=(2*y+3)*a2-2*b2*(x-1);
			x--;
          }
        y++;
    }
    
    
}
void mouse(int button, int state , int x , int y) {	
	int mousex = x ;
	int mousey = 600-y;
		switch(button){
		case GLUT_LEFT_BUTTON:
			if (state == GLUT_DOWN) {
			if (first == true ){
			X1 = mousex ;
			Y1 = mousey;
			first = false;
			break;
			}
			else {
			X2 = mousex;
			Y2 = mousey;
			first = true ;
			glBegin(GL_POINTS);
    		glVertex2f(X1,Y1);
    		//glVertex2f(X2,Y2);
			glEnd();
			float s = tinhKhoangCach(X1, Y1, X2, Y2);
			veEllip(X1,Y1,s,30);	
			break;
			}
		}
		case GLUT_RIGHT_BUTTON:
			if (state == GLUT_DOWN) {
			glClear(GL_COLOR_BUFFER_BIT ); 
			glColor3f(1.0, 0.0, 0.0);				
		 	glFlush();
			break;
			}
		}
}





void display(){
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(1.0, 0.0, 0.0);
	glOrtho(0 , 600, 0, 600 , -1.0 , 1.0);
	glFlush(); 
}


int main(){
	glutInitDisplayMode (GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize (600, 600); 
	glutInitWindowPosition (0, 0);
	glutCreateWindow("VE ELLIP MID POINT");
	glColor3f(0.0, 0.0, 0.0); 
	glutDisplayFunc(display);			
	glutMouseFunc(mouse);
	glutMainLoop();
		
}


```



### 1.4.Vẽ các đường cong khác





![anh dong minh hoa duong cong](https://upload.wikimedia.org/wikipedia/commons/d/db/B%C3%A9zier_3_big.gif)

![anh](13.PNG)

![](14.PNG)

```c

#include <GL/gl.h>
#include <GL/glu.h>
#include <GL/glut.h>
#include <iostream>
#include <math.h>
#include <string.h>
#include <stdio.h>
#include <unistd.h>
#include <Windows.h>


int p0[2];
int p1[2];
int p2[2];
int p3[2];
int tar=4;


//ham hien thi text
// 2 thong so dau chi vi tri hien thi
// thong so cuoi la chuoi ky tu

void print(int x, int y, char *string)
{
    int len, i;
    
    //lap tao do cho text(x,y)
    glRasterPos2f(x, y);
    
    //lay chieu dai choui de hien thi
    len = (int) strlen(string);
    
    //vong lap de hien thi chuoi
    for (i = 0; i < len; i++)
    {
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24,string[i]);
    }
}

int* bezier(float t, int* p0,int* p1,int* p2,int* p3)
{
    int res[2];
    //x^y 
    //(1-t)^3*p0.x + 3*t*(1-t)^2*p1.x + 3*t^2
    res[0]=pow((1-t),3)*p0[0]+3*t*pow((1-t),2)*p1[0]+3*pow(t,2)*(1-t)*p2[0]+pow(t,3)*p3[0];
    res[1]=pow((1-t),3)*p0[1]+3*t*pow((1-t),2)*p1[1]+3*pow(t,2)*(1-t)*p2[1]+pow(t,3)*p3[1];
    return res;
}
/*
doc them ve cong thuc: https://www.gatevidyalay.com/bezier-curve-in-computer-graphics-examples/
pow((1-t),3)*p0[0] = (1-t)^3*p0.x
3*t*pow((1-t),2)*p1[0] = 3*t*(1-t)^2*p1.x
3*pow(t,2)*(1-t)*p2[0] = 3*t^2*(1-t)*p2.x
pow(t,3)*p3[0]; = t^3*p3.x
*/
void Display() {
    glClear(GL_COLOR_BUFFER_BIT);
    
    print(750,500,"Bezier Control Points");
    glColor3f(1,0,0);
    char* p0s [20];
    //bien doi thanh string vao p0s
    sprintf((char *)p0s,"P0={%d,%d}",p0[0],p0[1]);
    print(785,450,(char *)p0s);
    glColor3f(0,1,0);
    char* p1s [20];
    sprintf((char *)p1s,"P1={%d,%d}",p1[0],p1[1]);
    print(785,400,(char *)p1s);
    glColor3f(0,0,1);
    char* p2s [20];
    sprintf((char *)p2s,"P2={%d,%d}",p2[0],p2[1]);
    print(785,350,(char *)p2s);
    glColor3f(1,1,1);
    char* p3s [20];
    sprintf((char *)p3s,"P3={%d,%d}",p3[0],p3[1]);
    print(785,300,(char *)p3s);
    
    //thuc hien thuat toan benzier
    glPointSize(1);
    glColor3f(1,1,0);
    glBegin(GL_POINTS);
    for(float t=0;t<1;t+=0.001)
    {
        int* p =bezier(t,p0,p1,p2,p3);
        glVertex3f(p[0],p[1],0);
    }
    glEnd();
    glPointSize(9);
    glBegin(GL_POINTS);
    glColor3f(1,0,0);
    glVertex3f(p0[0],p0[1],0);
    glColor3f(0,1,0);
    glVertex3f(p1[0],p1[1],0);
    glColor3f(0,0,1);
    glVertex3f(p2[0],p2[1],0);
    glColor3f(1,1,1);
    glVertex3f(p3[0],p3[1],0);
    glEnd();
    
    glFlush();
}

void mo(int x, int y)
{
    y=600-y;
    if(x<0)
        x=0;
    if(x>700)
        x=700;
    if(y<0)
        y=0;
    if(y>600)
        y=600;
    if(tar==0)
    {
        p0[0]=x;
        p0[1]=y;
    }
    else if(tar==1)
    {
        p1[0]=x;
        p1[1]=y;
    }
    else if(tar==2)
    {
        p2[0]=x;
        p2[1]=y;
    }
    if(tar==3)
    {
        p3[0]=x;
        p3[1]=y;
    }
    glutPostRedisplay();
}

void mou(int b,int s,int x, int y)
{
    y=600-y;
    if(b==GLUT_LEFT_BUTTON && s==GLUT_DOWN)
    {
        if(p0[0]<x+9&&p0[0]>x-9&&p0[1]<y+9&&p0[1]>y-9)
        {
            tar=0;
        }
        else if(p1[0]<x+9&&p1[0]>x-9&&p1[1]<y+9&&p1[1]>y-9)
        {
            tar=1;
        }
        else if(p2[0]<x+9&&p2[0]>x-9&&p2[1]<y+9&&p2[1]>y-9)
        {
            tar=2;
        }
        else if(p3[0]<x+9&&p3[0]>x-9&&p3[1]<y+9&&p3[1]>y-9)
        {
            tar=3;
        }
    }
    if(b==GLUT_LEFT_BUTTON && s==GLUT_UP)
        tar=4;
}

int main(int argc, char** argr) {
    glutInit(&argc, argr);
    
    glutInitWindowSize(1000, 600);
    
    p0[0]=100;
    p0[1]=100;
    
    p1[0]=100;
    p1[1]=500;
    
    p2[0]=500;
    p2[1]=500;
    
    p3[0]=500;
    p3[1]=100;
    
    glutCreateWindow("OpenGL - Bezier Curve Stimulation");
    glutDisplayFunc(Display);
	/*
		glutMotionFunc and glutPassiveMotionFunc set the motion and passive motion callback respectively for the current window. The motion callback for a window is called when the mouse moves within the window while one or more mouse buttons are pressed. The passive motion callback for a window is called when the mouse moves within the window while no mouse buttons are pressed.
		Lenh goi lai cua so khi cocn chuot chuyen dong trong cua so voi 1 hay nhieu nut chuot dc nhan
	*/
    glutMotionFunc(mo);
    glutMouseFunc(mou);
    
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
    gluOrtho2D(0.0, 1000, 0.0, 600);
    
    glutMainLoop();
    return 0;
}
```



