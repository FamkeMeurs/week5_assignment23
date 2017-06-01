# week5_assignment23
Creative Technology - Bachelor year 1
Module 4 - Algorithms in Create 
Made by: Maartje Aalders, Marloes ten Hage, Famke van Meurs and Mirel Nijhuis


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
