<img width="1440" height="1920" alt="image" src="https://github.com/user-attachments/assets/f0b777e2-263c-4f03-ba58-022bfded7e58" />

My project was built off the potentiometer knob. The main tutorial I used was the "Basics of Potentiometers" tutorial on the Arduino Docs. This got me the wiring for 3 leds and the knob. I was able to add another led by just copy and pasting the code for one and then changing the bulb name and digital pin value. And then I used the "Blink" tutorial on the Arduino Docs to get all of my leds blinking, and then I lowered the value of the lights to make them dimmer overall. Then made it so the led the potentiometer is stays on. I was originally going to code the song Candy Store from Heathers to play on a little speaker because of the bulbs being red, green, yellow, and blue. I follow the "Arduino Speaker Tutorial" from Buildelectroniccircuts and I was able to make the speaker make sounds but I couldn't manage to put down all the notes for Candy Store to actually sound like the song so I scrapped it.

Code:

Clay Shirky <clay.shirky@nyu.edu> 
*/

int potPin = A3;
int potVal = 0;

int yelPin = 3;
int redPin = 9;
int grnPin = 10;
int bluPin = 11;

// Brightness levels
int dimVal = 20;
int brightVal = 255;

// Blink timing
unsigned long lastBlinkTime = 0;
bool blinkOn = true;
int blinkInterval = 500; // ms between blink toggles

void setup()
{
  pinMode(yelPin, OUTPUT);
  pinMode(redPin, OUTPUT);   
  pinMode(grnPin, OUTPUT);   
  pinMode(bluPin, OUTPUT); 
}

// Main
void loop()
{
  potVal = analogRead(potPin);   // 0-1023


  int selected; // 0 = yellow, 1 = red, 2 = green, 3 = blue

  if (potVal < 256)
  {
    selected = 0; // Yellow
  }
  else if (potVal < 512)
  {
    selected = 1; // Red
  }
  else if (potVal < 768)
  {
    selected = 2; // Green
  }
  else
  {
    selected = 3; // Blue
  }

  // Handle blink timing (non-blocking)
  unsigned long now = millis();
  if (now - lastBlinkTime >= blinkInterval)
  {
    lastBlinkTime = now;
    blinkOn = !blinkOn;
  }

  // Work out each LED's brightness this frame
  int yelOut = (selected == 0) ? brightVal : (blinkOn ? dimVal : 0);
  int redOut = (selected == 1) ? brightVal : (blinkOn ? dimVal : 0);
  int grnOut = (selected == 2) ? brightVal : (blinkOn ? dimVal : 0);
  int bluOut = (selected == 3) ? brightVal : (blinkOn ? dimVal : 0);

  analogWrite(yelPin, yelOut);
  analogWrite(redPin, redOut);
  analogWrite(grnPin, grnOut);
  analogWrite(bluPin, bluOut);
}

Technical: Basically the potentiometer is an adjustable resistor which can control the flow of electricity to different leds, making them different. I used a rotary potentiometer since it's the most basic one and the only one I knew how to connect to the breadboard upright. Since the potentiometer only controls the flow of electricity, it could be used for audio controls, leds, and as a basic joystick. How it works with code is that you can program based on the reading you get on the potentiometer.

Peer Support: Originally I was going to try to use a joystick, basically three resistors, and obviously it's not a really great idea to start with three when you don't know one. SO, I was told by a classmate to switch to an easier one when I asked how to even connect the joystick to the breadboard. I think it helped me understand it more since it was the very simplest version.

Use-Case: The potentiometer could probably be used as an audio volume control or a light selector or just anything that requires highering or lowering. For light selecting only, I would just need to get rid of the blinking and then make it so the led the potentiometer is selecting is the only one on. The skill I'd have to rely on most if I kept developing my skills on the potentiometer would have to be debouncing and probably also wire management.
