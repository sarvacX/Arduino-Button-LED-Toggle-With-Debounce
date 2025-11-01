# Arduino-Button-LED-Toggle-With-Debounce
Learn how to use a pushbutton to toggle an LED on and off with simple software debouncing in Arduino.

## 🎯 Introduction / Objective

This project demonstrates how to control an LED using a **pushbutton** on an **Arduino** board.  
Unlike a basic circuit where the LED stays ON only while the button is pressed, this version introduces a **toggle feature** —  
pressing the button once turns the LED **ON**, and pressing it again turns it **OFF**.  

To make the input more reliable, the project also includes **simple debouncing logic**, which prevents false triggers caused by the mechanical “bounce” of the button.  

🔹 **Goal:** Learn how to use digital inputs and outputs together, implement state change detection, and apply software debouncing in Arduino.

---

## ⚙️ Components Used
| Component | Quantity | Description |
|------------|-----------|-------------|
| Arduino UNO | 1 | Microcontroller board used to run the code |
| Breadboard | 1 | For connecting the circuit without soldering |
| LED (any color) | 1 | Visual output indicator |
| Pushbutton | 1 | Input device to control the LED |
| Resistor 220Ω| 1 | Limits current to the LED |
| Resistor 10kΩ | 1 | Used as a pull-down resistor for the button |
| Jumper Wires | As needed | For making connections |

> 💡 **Note:** In my prototype, I used a 10 kΩ resistor for the LED because I didn’t have a 220 Ω one available.  
> The LED still worked but was very dim. Recommended value: **220 Ω**.

## 🔌 Circuit Diagram / Wiring

**Connections:**
- **Digital Pin 13** → **Resistor (220Ω)** → **LED Anode (+)**  
- **LED Cathode (–)** → **GND**  
- **Pushbutton one side** → **+5V**  
- **Pushbutton other side** → **Digital Pin 2**  
- **10kΩ resistor** between **Pin 2** and **GND** (pull-down resistor)

## 🔌 Circuit Diagram / Wiring

### Breadboard View
![Breadboard View](images/breadbordview.png)

### Schematic View
![Schematic Diagram](images/schematic.png)

## ⚙️ How It Works

This project demonstrates how a **pushbutton switch** can be used to **toggle an LED ON and OFF** using an Arduino, even after the button is released.

Unlike a simple circuit where the LED only lights while the button is held down, this version uses **state change detection** and **debouncing logic** so each press changes the LED’s state cleanly — once ON, once OFF.

---

### 🔹 Step-by-Step Explanation

1. **LED Connection:**  
   The LED is connected to **digital pin 13** through a **220 Ω resistor** to limit current and protect the LED.  

2. **Pushbutton Connection:**  
   The pushbutton is connected to **digital pin 2**.  

3. **Pull-down Resistor (10 kΩ):**  
   Keeps the input pin **LOW (0)** when the button isn’t pressed, preventing random signals.  

4. **Button Press:**  
   When the button is pressed, the pin reads **HIGH (1)** because it connects to 5V.  

5. **Toggle Logic:**  
   The Arduino detects a change from LOW → HIGH.  
   - If the LED was OFF, it turns **ON**.  
   - If it was ON, it turns **OFF**.  

6. **Debouncing:**  
   A short delay (500 ms) filters out the quick “noise” signals caused by the button’s mechanical bounce, ensuring the LED toggles only once per press.

---

### 🧠 In Short:
🟢 Press button → LED turns ON (and stays ON)  
⚫ Press button again → LED turns OFF  

---

### 💡 Additional Note:
In this version also, a **10 kΩ resistor** has been temporarily used in place of the LED’s **220 Ω resistor** due to component availability.  
The LED still worked but appeared dim. For proper brightness and safety, use a **220 Ω resistor** for the LED.

## 🔄 Simple Logic Flow

Start
│
│
Button Pressed?
├── Yes → Change LED State
│ ├── If LED was OFF → Turn ON
│ └── If LED was ON → Turn OFF
│
│
(Wait briefly to debounce)
│
Return to checking button
│
End

## 💻 Code Explanation

```cpp
// Turn on LED when the button is pressed
// and keep it on after it is released
// including simple de-bouncing

const int LED = 13;      // Pin connected to the LED
const int BUTTON = 2;    // Pin connected to the pushbutton

int val = 0;             // Current reading from the button
int old_val = 0;         // Previous button reading
int state = 0;           // LED state: 0 = OFF, 1 = ON

void setup() {
  pinMode(LED, OUTPUT);     // Set LED pin as OUTPUT
  pinMode(BUTTON, INPUT);   // Set button pin as INPUT
}

void loop() {
  val = digitalRead(BUTTON);      // Read the current button state

  // Check for a state change (LOW → HIGH)
  if ((val == HIGH) && (old_val == LOW)) {
    state = 1 - state;            // Toggle LED state (0→1 or 1→0)
    delay(500);                   // Simple debounce delay
  }

  old_val = val;                  // Update old value for next loop

  // Control LED based on state
  if (state == 1) {
    digitalWrite(LED, HIGH);      // Turn LED ON
  } else {
    digitalWrite(LED, LOW);       // Turn LED OFF
  }
}
```

### 🎬 Demo / Result  
[▶️ Watch Demo Video](videos/pushbuttonadvance.mp4)

## 🧩 Challenges & Learning

### 🔹 Challenges Faced:
- **Multiple LED toggles on a single press:**  
  At first, the LED turned ON and OFF rapidly due to the button’s “bouncing” effect — tiny quick ON/OFF signals caused by the physical button.
- **Remembering LED state:**  
  Initially, the LED would turn OFF as soon as the button was released. Implementing a **state variable** helped the LED stay ON until the next press.
- **Timing and Debouncing:**  
  Finding the right delay value (like `delay(500)`) was tricky — too short caused double toggles, too long made the button feel slow.

---

### 🔹 Key Learnings:
- Learned how to **detect state changes** (`LOW → HIGH`) instead of constantly reading button status.  
- Understood how to **toggle outputs** using a simple logic formula: `state = 1 - state`.  
- Gained practical knowledge of **button debouncing** and how to make input signals more stable.  
- Strengthened understanding of how **Arduino loops continuously** to check inputs and update outputs.  

---

💡 *This project was a great step forward from the basic button-LED control — introducing state management, reliable input handling, and smarter logic.*

## 🧩 Challenges Faced & Learnings

### 🔹 Challenges Faced:
- **Understanding the button toggle logic:**  
  It was tricky to make the LED stay ON even after releasing the button.  
  Learning how to store and update the LED’s state (`state = 1 - state`) helped solve this.  

- **Dealing with button bounce:**  
  The LED sometimes toggled multiple times with a single press.  
  Adding a **simple debounce delay** (`delay(500)`) fixed this by ignoring fast unwanted signals.  

---

### 🔹 What I Learned:
- How to make an LED **toggle ON/OFF** using **state change detection** in Arduino.  
- How **debouncing logic** prevents false readings from mechanical button noise.   
Clear understanding of **digital input (button)** and **digital output (LED)** operations.  
- Improved skills in **debugging breadboard connections** and reading Arduino circuit layouts.  

---

💡 *This upgraded version deepened my understanding of digital I/O handling, button logic, and clean signal reading — a key foundation for more advanced Arduino projects.*

**Add a Buzzer or Multiple LEDs:**
   - Make the circuit more interactive — for example, light up multiple LEDs in sequence or play a sound when the button is pressed.

**Use Internal Pull-up Resistor:**
   - Modify the code to use Arduino’s built-in `INPUT_PULLUP` mode instead of an external resistor.
   - Learn how logic levels invert (pressed = LOW, released = HIGH).

**Experiment with Different Components:**
   - Replace the LED with a relay module or motor driver to control higher-power devices.
   
**Create a Mini Project:**
   - Build a “Night Lamp” or “Touch Switch” system using the same principles.