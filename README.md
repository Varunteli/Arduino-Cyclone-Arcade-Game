# Arduino-Cyclone-Arcade-Game
# Arduino Cyclone Arcade Game

An interactive Arduino-based Cyclone Arcade Game project inspired by the classic arcade game. Players try to stop a moving light on an LED strip at the correct target position by pressing a button. This project provides a hands-on approach to embedded systems, Arduino programming, and basic game mechanics.

## Project Overview
This game is built with an Arduino Uno, WS2812B LED strip, and a push button. When the game starts, an LED moves along the strip, and the player tries to stop it on a target by pressing the button.

### Components
- Arduino Uno
- WS2812B LED Strip (30 LEDs)
- Push Button
- 10kΩ Resistor (for pull-up)
- Power Supply (5V or 12V for LEDs)

### How It Works
1. Game Start: The player presses a button to start the game.
2. Moving Light: The light on the LED strip begins to move.
3. Stop the Light: The player presses the button again, aiming to stop the light at the target position.
4. Win Condition: The player's timing is evaluated to see if they hit the target position.

## Advantages
- Engaging Hands-On Learning: This project provides an interactive way to learn embedded systems, combining hardware and programming in a fun, game-like setting.
- Enhances Timing and Precision Skills: The Cyclone game challenges players’ reflexes, making it a great tool for testing and improving timing and precision.
- Customizable Features: The game can easily be modified, with room for additional features like scoring, speed adjustments, sound, and more.
- Cost-Effective and Portable: The project requires only a few components, making it budget-friendly and easy to set up in different locations.
- Foundational Skills for Arduino: This project covers core Arduino skills, including LED control, input handling, and basic game logic, making it ideal for beginners.

## Circuit Diagram
![image](https://github.com/user-attachments/assets/1db7a11f-8912-47a1-ac37-cc41b232c865)


## Code
The Arduino code handles LED animations and button input to simulate the game mechanics.

### Sample Code
#include <Adafruit_NeoPixel.h>

#define LED_PIN 6       // LED strip pin
#define LED_COUNT 30    // Number of LEDs in the strip
#define BUTTON_PIN 2    // Button pin for starting and stopping

Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

int currentLED = 0;
bool gameActive = false;

void setup() {
  strip.begin();
  strip.setBrightness(50); // Adjust brightness
  strip.show(); // Initialize all LEDs to 'off'
  
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {
  if (digitalRead(BUTTON_PIN) == LOW && !gameActive) {
    startGame();
  }
  
  if (gameActive) {
    playGame();
  }
}

void startGame() {
  gameActive = true;
  currentLED = 0;
  strip.clear();
  strip.setPixelColor(currentLED, strip.Color(255, 0, 0)); // Initial LED color
  strip.show();
  delay(1000); // Wait before starting
}

void playGame() {
  static unsigned long lastUpdate = 0;
  unsigned long now = millis();
  
  if (now - lastUpdate > 100) { // LED moves every 100ms
    strip.setPixelColor(currentLED, strip.Color(0, 0, 0)); // Turn off current LED
    currentLED = (currentLED + 1) % LED_COUNT; // Move to the next LED
    strip.setPixelColor(currentLED, strip.Color(255, 0, 0)); // Turn on next LED
    strip.show();
    lastUpdate = now;
  }

  if (digitalRead(BUTTON_PIN) == LOW) { // Stop when button pressed
    stopGame();
  }
}

void stopGame() {
  gameActive = false;
  strip.clear();
  strip.show();
}
## Instructions
1. Hardware Setup: Connect the LED strip, button, and resistor as per the circuit diagram.
2. Upload Code: Load the code onto the Arduino using the Arduino IDE.
3. Play: Press the button to start and stop the game!

## Future Improvements
- Add sound effects using a piezo buzzer.
- Implement a scoring system to track accuracy.
- Add difficulty levels with increased LED speed.

## License
This project is open-source and available under the MIT License.

