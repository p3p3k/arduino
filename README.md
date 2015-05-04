
String voice;
int forward = 12; 	// Pin 12 - dopredu
int reverse = 11;	// Pin 11 - vzad
int left = 10;		// Pin 10 - vľavo
int right = 9;		// Pin 9  - vpravo
int short_lights = 7;	// Pin 7  - obrysové svetlá
int long_lights = 6;	// Pin 6  - diaľkové svetlá
int reverse_lights = 4;	// Pin 4  - spiatočka
int echo = 3; // echo pin na senzore je pripojený na pin 3
int vzdialenost = 25; // vzdialenosť od predmetu 
long duration, distance; // dĺžka trvania vzdialenosti
char val;
int i;// Variable to receive data from the serial port
void setup()
{
  
    // inicializácia 
  pinMode(forward, OUTPUT);
  pinMode(reverse, OUTPUT);
  pinMode(left, OUTPUT);
  pinMode(right, OUTPUT);
  pinMode(short_lights, OUTPUT);
  pinMode(long_lights, OUTPUT);
  pinMode(reverse_lights, OUTPUT);

  Serial.begin(9600); 	// zapne seriový kanál
  
}
void allon(){      //zapne všetky svetlá
     digitalWrite(short_lights, HIGH); 
     
     }
void alloff(){        //vypne všetky svetlá
     digitalWrite(short_lights, LOW); 
     digitalWrite(long_lights, LOW); 
}
void start(){
  for(i=0;i<5;i++){  //rychle blikanie
 digitalWrite(short_lights, HIGH);
 delay(500);
 digitalWrite(short_lights, LOW);
 delay(500);
}
}


void off(){    //vypne len obrysové svetlá
    digitalWrite(short_lights, LOW); 
     
}
// pohyb dopredu
void go_forward() {
  digitalWrite(forward, HIGH);
  digitalWrite(reverse, LOW);
}

// zastavenie pohybu dopredu
void stop_go_forward() {
  digitalWrite(forward, LOW);
}

// cúvanie
void go_reverse() {
  digitalWrite(reverse, HIGH);
  digitalWrite(forward, LOW);
  digitalWrite(reverse_lights, HIGH);
}

// zastavenie cúvania
void stop_go_reverse() {
  digitalWrite(reverse, LOW);
  digitalWrite(reverse_lights, LOW);
}


// zabočí doľava
void go_left() {
  digitalWrite(left, HIGH);
  digitalWrite(right, LOW);
}

// zabočí do prava
void go_right() {
  digitalWrite(right, HIGH);
  digitalWrite(left, LOW);
}

// zastaví otáčanie a vráti sa do pôvodnej polohy
void stop_turn() {
  digitalWrite(right, LOW);
  digitalWrite(left, LOW);
}

// zastaví auto
void stop_car() {
  digitalWrite(forward, LOW);
  digitalWrite(reverse, HIGH);
  delay(1000);
  digitalWrite(reverse, LOW);
  digitalWrite(right, LOW);
  digitalWrite(left, LOW);
  digitalWrite(reverse_lights, LOW);
}

// zapne obrysové svetlá
void lights_on() {
  digitalWrite(short_lights, HIGH);
  }

// vypne obrysové svetlá
void lights_off() {
  digitalWrite(short_lights, LOW);
 
}

// zapne diaľkové svetlá
void long_lights_on() {
  digitalWrite(long_lights, HIGH);
}

// vypne diaľkové svetlá
void long_lights_off() {
  digitalWrite(long_lights, LOW);
}

// zapne cúvacie svetla
void back_lights_on() {
  digitalWrite(reverse_lights, HIGH);
}

// vypne cúvacie svetla
void back_lights_off() {
  digitalWrite(reverse_lights, LOW);
}
  
// načíta hodnotu zo serial portu a vykoná príkaz
void performCommand() {
  if (Serial.available()) {
    val = Serial.read();
  }
    if (val == 'f') { // vpred
      go_forward();
    } else if (val == 'z') { // zastaví akciu vpred 
      stop_go_forward();
    } else if (val == 'b') { // vzad 
      go_reverse();
    } else if (val == 'y') { // zastaví akciu vzad
      stop_go_reverse();
    }
     else if (val == 'l') { // vpravo
      go_right();
      
    } else if (val == 'r') { // vľavo
      go_left();
    } else if (val == 'v') { // zastaví otáčanie 
      stop_turn();
    } else if (val == 's') { //zastaví auto 
      stop_car();
    } else if (val == 'a') { // obrysové svetlá
      lights_on();
    } else if (val == 'c') { // vypne obrysové svetlá
      lights_off();
    } else if (val == 'd') { // zapne diaľkové svetlá
     long_lights_on();
    } else if (val == 'e') { // vypne diaľkové svetlá
    long_lights_off();
          }
    
  
}


void loop(){
  performCommand();
  
/*  
while (Serial.available()){  //Skontroluje či sú nejaé hodnoty k dispozícii na načítanie
  delay(10); //Toto oneskorenie stabilizuje vykonávanie programu
  char c = Serial.read(); //prečíta hodnoty zo sériového kanála
  if (c == '#') {break;} //zastaví loop ak zistí stlačenie znaku
  voice += c; //skratka pre hlas = voice + c
  }  
  if (voice.length() > 0) {
    Serial.println(voice); 
       if(voice == "*zapnúť") {allon();}  //Zapne svetlá
  else if(voice == "*vypnúť"){alloff();} //Vypne svetlá
  else if(voice == "*nebezpečenstvo") {start();}  //Začnú blikať obrysové svetlá
  else if(voice == "*off"){off();} //Vypne len obrysové svetlá
  voice="";}
 */
  pinMode(echo, OUTPUT); // nastavenie výstupu na echo pin
digitalWrite(echo, LOW); // echo pin je vypnutý
 delayMicroseconds(2); // počkáme 2us

 digitalWrite(echo, HIGH); //echo pin je zapnutý
 delayMicroseconds(5); // počkáme 5us
 
 digitalWrite(echo, LOW); // vypneme echo pin
 pinMode(echo, INPUT); //zmeníme echo z výstupu na vstup
 duration = pulseIn(echo, HIGH); //vypočíta vzdialenosť na záklde trvania 
 distance = duration/58.2; //vypočíta vzdialenosť na záklde trvania 
 Serial.println(distance); //vypíše na serial monitore vzdialenosť 
  if(distance==vzdialenost){ // ak bude distance < ako vzdialenosť 
      stop_car();
      start();
      
    }
    
 
  }


