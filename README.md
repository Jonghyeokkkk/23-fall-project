int count=0;
int score=0;

float [] xx1, yy1, xx2, yy2, xx3, yy3;    //xx1,yy1 ->Ryan 좌표, xx2,yy2 -> face 좌표, xx3,yy3 -> Robot 좌표

int x,y;
void setup(){
   size(1000, 1000);
   xx1= new float[10];
   xx2= new float[10];
   xx3= new float[10];
   yy1= new float[10];
   yy2= new float[10];
   yy3= new float[10];
   for (int i=0; i<10; i++) {
     xx1[i]=random(50, 1000);
     yy1[i]=random(50, 1000);
     xx2[i]=random(50, 1000);
     yy2[i]=random(50, 1000);
     xx3[i]=random(50, 1000);
     yy3[i]=random(50, 1000);
   }

}
void draw(){
  background(0,255,255);
 
  if(count<90){      //마우스로 커서 조작 설정
    y++;
  }
  else if(count<150){
    x++;
  }
  else count=0;
  fill(255,255,0);
  circle(x,y,30);
  if(y>500)y=0;
  if(x>500)x=0;
  x=mouseX;
  y=mouseY;
  count++;
  for (int i=0; i<10; i++) {    //떨어지는 캐릭터들 설정
    yy1[i]+=10;
    yy2[i]+=5;
    yy3[i]+=7;
    Ryan(xx1[i],yy1[i],1);
    face(xx2[i],yy2[i],1);
    robot(xx3[i],yy2[i],1);
    if (yy1[i]<0||yy1[i]>1000){      //바닥에 닿을 경우 다시 위에서부터 시작
      yy1[i]=0;
    }
    if (yy2[i]<0||yy2[i]>1000){
      yy2[i]=0;
    }
    if (yy3[i]<0||yy3[i]>1000){
      yy3[i]=0;
    }
  }
    
  collide();
}
void collide() {    //충돌 설정. 부딪힐 경우에 부딪힌 캐릭터들은 없어짐. face 캐릭터를 먹을 경우에 +2점,
                    //라이언을 먹을 경우 +1점, 로봇을 먹을 경우 -1점

  for (int i=0; i<10; i++) {

    float dx1=x-xx1[i];
    float dx2=x-xx2[i];
    float dx3=x-xx3[i];
    float dy1=y-yy1[i];
    float dy2=y-yy2[i];
    float dy3=y-yy3[i];
    if (5<=abs(dx1)&abs(dx1)<=15&5<=abs(dy1)&abs(dy1)<=15) {
      score+=1;
      xx1[i]=-50;
    } else if (0<=abs(dx2)&abs(dx2)<=10&0<=abs(dy2)&abs(dy2)<=10) {
      score+=2;
      xx2[i]=-50;
    } else if (0<=abs(dx3)&abs(dx3)<=10&0<=abs(dy3)&abs(dy3)<=10) {
      score--;
      xx3[i]=-50;
    }

    textSize(90);
    text("score:"+score, 150, 150);
    
  }
  //score>=25일 경우에 success
  //score<25일 경우에
  //ryan, face가 아예 없는 경우에 fail
  int temp=1;
  if(score>=25){
    textSize(100);
    text("SUCCESS!!",300,500);
  }
  else if(score<25){
    for(int i=0;i<10;i++){
      if(xx1[i]==-50){
        if(xx2[i]!=-50){
          temp=0;
        }
      }
      if(xx2[i]!=-50){
        temp=0;
      }
    }
    if(temp==1){
    textSize(100);
    text("fail",300,500);
  }
  }
}

void Ryan(float x,float y,float d){
  fill(250,180,0);
  strokeWeight(0.8*d);
  circle(x-18*d,y-25*d,15*d);    //라이언 왼쪽귀
  circle(x+18*d,y-25*d,15*d);    //라이언 오른쪽귀
  circle(x,y,60*d);    //라이언 얼굴
  strokeWeight(1*d);
  line(x-17*d,y-16*d,x-9*d,y-16*d);    //라이언 왼쪽눈썹
  line(x+9*d,y-16*d,x+17*d,y-16*d);    //라이언 오른쪽눈썹
  fill(0,0,0);    //라이언 왼쪽 눈
  circle(x-14*d,y-12*d,2*d);
  fill(0,0,0);    //라이언 오른쪽 눈
  circle(x+14*d,y-12*d,2*d);
  circle(x,y+3*d,4*d);    //라이언 코
  strokeWeight(0.8*d);    //라이언 입
  fill(255);
  circle(x-2.5*d,y+7.5*d,7*d);
  circle(x+2.5*d,y+7.5*d,7*d);
  noStroke();
  circle(x,y+7.5*d,5*d);    //가운데 선 없애기
  stroke(0);
  strokeWeight(1);
}
int yellow1 = 255;
int yellow2 = 200;

void face(float a, float b, float d) {
  fill(yellow1, yellow2, 0);  // 얼굴 색상을 노란색으로 설정
  circle(a, b, d*50);  // 얼굴을 그립니다.

  // 눈 그리기
  fill(0);  // 눈 색상을 검은색으로 설정
  ellipse(a-d*10, b-d*10, d*5, d*5);  // 왼쪽 눈
  ellipse(a+d*10, b-d*10, d*5, d*5);  // 오른쪽 눈

  // 웃는 입 그리기
  noFill();
  stroke(0);
}

void robot(float x, float y, float d){
  strokeWeight(5);
  rect(x,y,d*30,d*30);
  rect(x+d*5, y+d*5, d*7, d*9);
  rect(x+d*18, y+d*5, d*7,d*9);
  line(x+d*30/2,y-100,x+d*30/2,y);
  circle(x+d*30/2,y-100,d*4);
  rect(x+d*5,y+d*20,d*20,d*5);
}
