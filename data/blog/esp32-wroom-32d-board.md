# Working with the ESP32 Wroom 32D

So you recently purchased an ESP32 board of make: Wroom 32D. Or maybe your lab has it lying around and you would like to play with it a little. Thankfully, you've come to the right place. In this tutorial, you will learn the basics of working with Wifi and HTTP on the ESP32 board.

![The ESP32 Board of make Wroom 32D](https://github.com/vicradon/engineering-blog/assets/40396070/582aed97-5612-446a-bcdd-1dd302a9f4f9)

You can find the docs here: https://docs.espressif.com/projects/esp-idf/en/stable/esp32/hw-reference/esp32/get-started-devkitc.html

<!-- The board is basically an ESP32 board with a something something chip. -->

## Making HTTP Requests

The board comes with a built-in HTTP Client library which in my opinion, is complementary to the built-in Wifi library. The basic idea is making HTTP requests when connected to a Wifi network. Let's see the code for making an HTTP GET request.

### Making an HTTP GET Request

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
  
const char* ssid = "WifiSSID";
const char* password =  "wifipassword";

void setup() {
  Serial.begin(115200);
  delay(4000);   // Delay needed before calling the WiFi.begin
  
  WiFi.begin(ssid, password); 
  
  while (WiFi.status() != WL_CONNECTED) { // Check for the connection
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  
  Serial.println("Connected to the WiFi network");
}

void loop() {
  if(WiFi.status() == WL_CONNECTED){
    HTTPClient http;

    http.begin("https://about.com");
    int httpResponseCode = http.GET();

    if(httpResponseCode>0) {
      String response = http.getString();
      Serial.println(httpResponseCode);
      Serial.println(response);
    } else {
      Serial.println("Error in WiFi connection");
    }
  }
}
```

The idea is to connect to the Wifi network in the setup function then make the HTTP request in the loop function.

### Making an HTTP Post Request

Making an HTTP POST request is a bit different because you need to add the HTTP headers after the `http.begin(URL)` declaration. If you add it before, it won't work.

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
  
const char* ssid = "WifiSSID";
const char* password =  "WifiPassword";
  
void setup() {
  
  Serial.begin(115200);
  delay(4000);   // Delay needed before calling the WiFi.begin
  
  WiFi.begin(ssid, password); 
  
  while (WiFi.status() != WL_CONNECTED) { //Check for the connection
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  
  Serial.println("Connected to the WiFi network");
  
}
  
void loop() {
  
 if(WiFi.status()== WL_CONNECTED){   //Check WiFi connection status
  
   HTTPClient http;   
  
   http.begin("http://jsonplaceholder.typicode.com/posts");  //Specify destination for HTTP request
   http.addHeader("Content-Type", "text/json");             //Specify content-type header
  
   int httpResponseCode = http.POST("{\"data\": \"posting from ESP32\"}");   //Send the actual POST request
  
   if(httpResponseCode>0){
  
    String response = http.getString();                       //Get the response to the request
  
    Serial.println(httpResponseCode);   //Print return code
    Serial.println(response);           //Print request answer
  
   }else{
  
    Serial.print("Error on sending POST: ");
    Serial.println(httpResponseCode);
  
   }
  
   http.end();  //Free resources
  
 }else{
  
    Serial.println("Error in WiFi connection");   
  
 }
  delay(10000);  //Send a request every 10 seconds
}
```

That's it basically.
