# Remote-control-with-a-sensor
    #include "Adafruit_APDS9960.h"
    Adafruit_APDS9960 apds;
    #include <IRremote.h>

    IRsend irsend;
    // the setup function runs once when you press reset or power the board
    void setup() {
    Serial.begin(115200);

    if (!apds.begin()) {
    Serial.println("failed to initialize device! Please check your wiring.");
    }
    else Serial.println("Device initialized!");

    //gesture mode will be entered once proximity mode senses something close
    apds.enableProximity(true);
    apds.enableGesture(true);
    }

    // the loop function runs over and over again forever
    void loop() {

    //read a gesture from the device
    uint8_t gesture = apds.readGesture(); //read the movement the apds 9960 senses 
    if (gesture == APDS9960_DOWN){        // if th movement of the gesture is down 
    Serial.println("ZOOM -");             // Show on the screen 
    irsend.sendNEC(0xC1AA916E, 32);       // Send the HEX to the projector to put the volume down 
    }
    if (gesture == APDS9960_UP){ 
    Serial.println("ZOOM +");
    irsend.sendNEC(0xC1AA11EE, 32);
    }
    if (gesture == APDS9960_LEFT){
    Serial.println("VOLUME -");
    irsend.sendNEC(0xC1AA9966, 32);
    }
    if (gesture == APDS9960_RIGHT){
    Serial.println("VOLUME +");
    irsend.sendNEC(0xC1AA19E6, 32);
    }

    }
