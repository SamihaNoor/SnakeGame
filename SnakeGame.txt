/*
Computer Graphics [B]

Project Title: Snake Game

Members:
1. Samiha Noor [18-36812-1]
2. Fariaz Haque [18-36934-1]

Instructions:
1. Use arrow keys to control the snake
2. Score points by eating the food

*/

#include<GL/gl.h>
#include<GL/glut.h>
#include<ctime>
#include<windows.h>
//#include<stdlib.h>

#define COLUMNS 40
#define ROWS 30             //grid columns and rows
#define FPS 10

#define UP 8
#define DOWN 2
#define RIGHT 6
#define LEFT 4

#define MAX 50              //max snake size

int snake_length=5;
int posX[60]={10,10,10,10,10},posY[60]={10,9,8,7,6};            //snake
short direction=RIGHT;
bool dead= false;
bool food=true;
int foodX,foodY;
int score;

void init()
{
    glClearColor(0.0,0.0,0.0,1.0);
}

void unit(int x,int y)
{
    if(x==0 || y==0 || x==COLUMNS-1 || y==ROWS-1)
    {
        glColor3f(1.0,0.0,0.0);
        glRectd(x,y,x+1,y+1);
    }
    else if(x==22)                                  //obstacle
    {
        glColor3f(1.0,0.0,0.0);
        glBegin(GL_QUADS);
        glVertex2f(x,10);
        glVertex2f(x+7,10);
        glVertex2f(x+7,11);
        glVertex2f(x,11);
        glEnd();
    }
    else if(x==5)                                   //obstacle
    {
        glColor3f(1.0,0.0,0.0);
        glBegin(GL_QUADS);
        glVertex2f(x,5);
        glVertex2f(x+1,5);
        glVertex2f(x+1,15);
        glVertex2f(x,15);
        glEnd();
    }
    else if(x==12)                                   //obstacle
    {
        glColor3f(1.0,0.0,0.0);
        glBegin(GL_QUADS);
        glVertex2f(x,21);
        glVertex2f(x+5,21);
        glVertex2f(x+5,22);
        glVertex2f(x,22);
        glEnd();
    }
    else if(x==35)                                   //obstacle
    {
        glColor3f(1.0,0.0,0.0);
        glBegin(GL_QUADS);
        glVertex2f(x,15);
        glVertex2f(x+1,15);
        glVertex2f(x+1,18);
        glVertex2f(x,18);
        glEnd();
    }
}
void random(int &x,int &y)
{
    int maxX = COLUMNS-2;
    int maxY = ROWS-2;
    int minXY = 1;
    srand(time(NULL));
    x= minXY + rand()% (maxX - minXY);
    y= minXY + rand()% (maxY - minXY);
}                                                               //generate food randomly
void drawFood()
{
    if(food)
        random(foodX,foodY);
    food=false;
    glColor3f(1.0,1.0,0.0);
    glBegin(GL_QUADS);
    glVertex2f(foodX+0.5,foodY);
    glVertex2f(foodX+1,foodY+0.5);
    glVertex2f(foodX+0.5,foodY+1);
    glVertex2f(foodX,foodY+0.5);
    glEnd();

}
void drawSnake()
{
    for(int i=snake_length-1;i>0;i--)
    {
        posX[i]=posX[i-1];
        posY[i]=posY[i-1];
    }
    if(direction==UP)
        posY[0]++;
    else if(direction==DOWN)
        posY[0]--;
    else if(direction==RIGHT)
        posX[0]++;
    else if(direction==LEFT)
        posX[0]--;

    for(int i=0;i<snake_length;i++)
    {
        if(i==0)
            glColor3f(0.1,0.4,0.0);
        else
            glColor3f(0.1,0.6,0.0);
        glRectd(posX[i],posY[i],posX[i]+1,posY[i]+1);
    }

    if(posX[0]==0 || posX[0]==COLUMNS-1 || posY[0]==0 || posY[0] == ROWS-1)
        dead=true;                                                              //dead
    else if(posX[0]==22 || posY[0]==10)
    {
        for(int i=22;i<29;i++)
        {
            for(int j=10;j<11;j++)
            {
                if(posX[0]==i && posY[0]==j)
                    dead=true;                          //dead
            }
        }
    }
    else if(posX[0]==5 || posY[0]==5)
    {
        for(int i=5;i<6;i++)
        {
            for(int j=5;j<15;j++)
            {
                if(posX[0]==i && posY[0]==j)
                    dead=true;                          //dead
            }
        }
    }
    else if(posX[0]==12 || posY[0]==21)
    {
        for(int i=12;i<17;i++)
        {
            for(int j=21;j<22;j++)
            {
                if(posX[0]==i && posY[0]==j)
                    dead=true;                          //dead
            }
        }
    }

    else if(posX[0]==35 || posY[0]==15)
    {
        for(int i=35;i<36;i++)
        {
            for(int j=15;j<18;j++)
            {
                if(posX[0]==i && posY[0]==j)
                    dead=true;                          //dead
            }
        }
    }

    if(posX[0]==foodX && posY[0]==foodY)
    {
        score++;                                        //score + new food
        snake_length++;
        if(snake_length>MAX)
            snake_length=MAX;
         food=true;
    }
}
void drawObstacle()
{
    for(int x=0;x<COLUMNS;x++){
        for(int y=0;y<ROWS;y++){
            unit(x,y);
        }
    }
}
void time(int)
{
    glutPostRedisplay();
    glutTimerFunc(1000/FPS,time,0);
}
void specialKeyInput(int key,int,int)
{
    switch(key)
    {
        case GLUT_KEY_UP:
            if(direction!=DOWN)
                direction=UP;
            break;
        case GLUT_KEY_DOWN:
        if(direction!=UP)
            direction=DOWN;
        break;
        case GLUT_KEY_RIGHT:
            if(direction!=LEFT)
                direction=RIGHT;
            break;
        case GLUT_KEY_LEFT:
            if(direction!=RIGHT)
                direction=LEFT;
            break;
    }
}
void display( void )
{
    glClear(GL_COLOR_BUFFER_BIT);
    drawObstacle();
    drawSnake();
    drawFood();
    glColor3f(1.0,0.0,0.0);
    if(dead)
    {
        char _score[10];
        itoa(score,_score,10);
        char text[10]="Score: ";
        strcat(text,_score);
        MessageBox(NULL,text,"GAME OVER",0);
        exit(0);
    }
    glutSwapBuffers();
}
void reshape(int w,int h)
{
    glViewport(0,0,(GLsizei)w,(GLsizei)h);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    glOrtho(0.0,COLUMNS,0.0,ROWS,-1.0,1.0);
    glMatrixMode(GL_MODELVIEW);
}
int main(int argc,char **argv)
{
    glutInit(&argc,argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE);
    glutInitWindowSize(700,500);
    glutInitWindowPosition(300,100);
    glutCreateWindow("Snake Game");
    glutDisplayFunc(display);
    glutReshapeFunc(reshape);
    glutTimerFunc(0,time,0);
    glutSpecialFunc(specialKeyInput);
    init();
    glutMainLoop();
    return 0;
}
