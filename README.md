# arduino_python
참고 사이트 : https://coding-kindergarten.tistory.com/179

#문자열 읽는 코드 (파이썬)
import serial
import time

py_serial = serial.Serial(
    port = 'COM18',
    baudrate=115200,
)

while True:
    commend = input('아두이노에게 내릴 명령:')
    py_serial.write(commend.encode())
    time.sleep(0.1)

    if py_serial.readable():
        response = py_serial.readline()

        print(response[:len(response)-1].decode())


#문자열 읽는 코드 (아두이노)
char cmd[50];  // Create a character array to store the incoming data
#define LED 7

void setup() {
  // Start serial communication (baud rate: 9600)
  Serial.begin(115200);
  pinMode(LED,OUTPUT);
}

void loop() {
  // When serial communication is transmitted from the computer, read it line by line into the cmd array.
  if (Serial.available()) {
    Serial.readBytesUntil('\n', cmd, sizeof(cmd));

    // Check if the received data contains "person"
    if (strstr(cmd, "person") != NULL) {
      Serial.println("find person!");
      digitalWrite(LED, HIGH);
    } else {
      Serial.println("nothing find");
      digitalWrite(LED, LOW);
    }

    // Clear the cmd array to prepare for the next input
    memset(cmd, 0, sizeof(cmd));

    delay(500); // Optional delay
  }
}
