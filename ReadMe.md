# 7 Segment Output von Potentiometer

### SYT EK

Verfasser: **Moritz Mairinger**

Datum: **30.09.2022**

## 1.  Einführung 

Für Ein und Ausgeben kann man bei Microcontrollern verschiedene Bauteile verwenden. Es gibt verschiedene arten von Displays, Leuchten oder anzeigen. Als eingab kann man rotierende, linieare oder ander Eingaben verwenden.

## 2. Projektbeschreibung

Bei diesem Projekt handelt es um eine Eingabe und zwei Ausgabemöglichkeiten die verbunden sind. Die Eingabe wird mit einem Potentiometer realisiert und die ausgabe ist eine LED und ein 7 Segment Display. Wenn der Potentiometer gedreht wird kann auf dem Display eine Zahl von 0 bis 9 ausgegeben werden. Die LED leuchtet immer dann auf wenn sich eine Zahl ändert. 

## 3. Arbeitsschritt

```c++
/*
  Showing numbers, chars and phrases
                             A (seg[0] in this project)
                            ---
F (seg[5] in this project) |   | B (seg[1] in this project)
                           |   | 
                            --- G (seg[6] in this project)
E (seg[4] in this project) |   | 
                           |   | C (seg[2] in this project)
                            ---  . dot or dicimal (seg[7] in this project)
                             D (seg[3] in this project)
 */
const int led = 10;
int lastWert = 0;
int wert = 0;
int ledState = HIGH;  
unsigned long previousMillis = 0;
const long interval = 10;
int time = 0;
#define A 8
#define B 7
#define C 6
#define D 5
#define E 4
#define F 3
#define G 2
#define DP 9 // decimal
#define common_cathode 0
#define common_anode 1
bool segMode = common_cathode; // set this to your segment type, my segment is common_cathode
int seg[] {A,B,C,D,E,F,G,DP}; // segment pins
byte chars = 35; // max value in the array "Chars"

byte Chars[35][9] { 
            {'0',1,1,1,1,1,1,0,0},//0
            {'1',0,1,1,0,0,0,0,0},//1
            .........................
            };
void setup() {
  // set segment pins as OUTPUT
  pinMode(seg[0],OUTPUT);
  pinMode(seg[1],OUTPUT);
  pinMode(seg[2],OUTPUT);
  pinMode(seg[3],OUTPUT);
  pinMode(seg[4],OUTPUT);
  pinMode(seg[5],OUTPUT);
  pinMode(seg[6],OUTPUT);
  pinMode(seg[7],OUTPUT);
  //setting led
  pinMode(led, OUTPUT);
}

void setState(bool mode){ //sets the hole segment state to "mode"
  for(int i = 0;i<=6;i++){
    digitalWrite(seg[i],mode);
  }
}

void Print(char Char){ // print any character on the segment ( Note : you can't use capital characters ) 
  int charNum = -1;// set search resault to -1
  setState(segMode);//turn off the segment
  
  for(int i = 0; i < chars ;i++){//search for the enterd character
    if(Char == Chars[i][0]){//if the character found
      charNum = i;//set the resault number into charNum ( because this function prints the character using it's number in the array )
    }
  }
 
  if(charNum == -1 ){// if the character not found
    for(int i = 0;i <= 6;i++){
      digitalWrite(seg[i],HIGH);
      delay(100);
      digitalWrite(seg[i],LOW);
    }
    for(int i = 0;i <= 2;i++){
      delay(100);
      setState(HIGH);
      delay(100);
      setState(LOW); 
    }
  }
  else{ // else if the character found print it
  for(int i = 0;i<8;i++){
    digitalWrite(seg[i],Chars[charNum][i+1]);
    }
  }
}

void Print(int num){ // print any number on the segment
  setState(segMode);//turn off the segment
  
  if(num > chars || num < 0 ){// if the number is not declared
    for(int i = 0;i <= 6;i++){
      digitalWrite(seg[i],HIGH);
      delay(100);
      digitalWrite(seg[i],LOW);
    }
    for(int i = 0;i <= 2;i++){
      delay(100);
      setState(HIGH);
      delay(100);
      setState(LOW); 
    }
  }
  else{ // else if the number declared, print it
    if(segMode == 0){ //for segment mode
      for(int i = 0;i<8;i++){
        digitalWrite(seg[i],Chars[num][i+1]);
      }
    }
    else{
      for(int i = 0;i<8;i++){
        digitalWrite(seg[i],!Chars[num][i+1]);
      }
    }
  }
}

void loop() {
  wert = analogRead(A5) / 110;		//the analog read signal is converted int a number between 0-9

  if(lastWert != wert){					//checks if the number has changed 	
    lastWert = wert;						//updates the last number
    digitalWrite(led, HIGH);		//sets LED HIGH
    delay(5);										//waits 5ms
    digitalWrite(led, LOW);			//sets LED LOW
  }
  Print(wert);
}
```

## Bilder
<img src="https://i.postimg.cc/J4Hcx0MT/Tremendous-Bigery.png" alt="Tremendous-Bigery.png"  />


__Darstellung der Schaltung in Tinkercad__

*Bilder der echten Schaltung*: 

## 1.

<img src="https://i.postimg.cc/Qdk6bwXP/IMG-0930.jpg" alt="IMG-0930.jpg" style="zoom:50%;" />

## 2. 

[<img src="https://i.postimg.cc/mrWK87fX/IMG-0933-2.jpg" alt="IMG-0933-2.jpg" style="zoom:67%;" />](https://postimg.cc/rDghp0px) 

​											

# 5. Zusammenfassung

In diesem Projekt hab ich eine simple Anzeige mithilfe von deinem Potentiometer angesteuert. Den Wechsel der Zahl habe ich mit einer LED visualisiert die für 5ms leuchtet.

Aktuell fünktioniert das Blinken der LED mit denem delay(). Das könnte zu Problemen führen wenn es nicht als seperates Program ausgeführt wird. Ich habe bemerkt, dass es in diesem Anwendungsfall keinen Unterschied macht. 

Großteil des codes ist von [CREATE.ARDUINO.CC](ttps://create.arduino.cc/projecthub/aboda243/get-started-with-seven-segment-c73200). Am Aanfang des codes werden chars und andere variablen deklariert in der whiler schleife wird das Display geupdated. 


# 6. Quellen 

[1]Analog Read Serial | Arduino Documentation | Arduino Documentation [online] Available at: <https://docs.arduino.cc/built-in-examples/basics/AnalogReadSerial> [Accessed 30 September 2022].

[2] Get started with seven segment - Arduino Project Hub [online] Available at: <https://create.arduino.cc/projecthub/aboda243/get-started-with-seven-segment-c73200> [Accessed 30 September 2022]

[3] Adam-P, “Markdown cheatsheet · Adam-P/markdown-here wiki,” *GitHub*. [online]. Available at: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet. [Accessed 8 October 2022].

[4] Postimages — free image hosting / image upload [online]. Available at: https://postimages.org/. [Accessed 10 October 2022].
