# week5_assignment23
Creative Technology - Bachelor year 1
Module 4 - Algorithms in Create 
Made by: Maartje Aalders, Marloes ten Hage, Famke van Meurs and Mirel Nijhuis


float angle = 0 ;
float aVelocity = 1;
float demper;

Flower flower;

void setup() {
  size(600, 600);
  flower = new Flower(5, 20);
}

void draw() {
  background(255);
  stroke(0);
  translate(width/2, height/2);
  flower.move();
  flower.display();
}


class MSDS {
  float angle = 0 ;
  float aVelocity = 50;
  float demper = 0.9;
  float springConstant = 0.01;
  float mass = 50;
  float fd;              // force of damper
  float fs;              // force of spring
  float fm;              // force of mass
  float momentum;
  float lengte; 

  MSDS(float segmentsLength) {
    lengte = segmentsLength;
  }

  void display() {
    rotate(radians(angle));      // rotates the screen 
    translate(sin(radians(angle)) * lengte, cos(radians(angle)) * lengte); 
    angle += aVelocity;          // add velocity to the angle
    stroke(0, 230, 0);
    line(0, 0, 0, lengte);       // draws the line
  }

  void move() {
    // fd = d * v
    // fs = integraal van v*k
    // v = integraal van force * 1/m
    fd = demper * aVelocity;
    angle += aVelocity;
    fs = angle * springConstant;
    fm = -fd - fs;
    momentum += fm;
    aVelocity = momentum * (1/mass);
  }
}



class Flower {
  float [] force;
  float [] velocity;
  MSDS [] segments;

  Flower(int numOfSegments, float segmentsLength) {
    force = new float [numOfSegments];
    velocity = new float [numOfSegments];
    segments = new MSDS [numOfSegments];
    for (int i=0; i < segments.length; i++) {
      segments[i] = new MSDS(segmentsLength);
    }
  }

  void Calculate() {
    for (int i = 0; i < segments.length; i++) {
      force[i]=segments[i].fm;
      velocity[i]=segments[i].aVelocity;
    }

    for (int j = 1; j < force.length; j++) {
      segments[j-1].fm = force[j-1] -force[j];
    }

    for (int k = 1; k < velocity.length; k++) {
      segments[k].aVelocity = velocity[k] - velocity[k-1];
    }
  }

  void move() {
    Calculate();
    for (int i = 0; i < segments.length; i++) {
      segments[i].move();
    }
  }

  void display() {
    rotate(PI);
    for (int i = 0; i < segments.length; i++) {
      segments[i].display();
    }
  }
}

