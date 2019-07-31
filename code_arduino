//Code ngay 26/07
    #include <ESP8266WiFi.h>
    #include <SocketIOClient.h>
    #include <ArduinoJson.h>
     
    SocketIOClient client;
    //StaticJsonBuffer<200> jsonBuffer;
    DynamicJsonBuffer jsonBuffer;
    const char* ssid = "thaihung";          //Tên mạng Wifi mà Socket server của bạn đang kết nối
    const char* password = "79n6dvn123";  //Pass mạng wifi ahihi, anh em rãnh thì share pass cho mình với.
     
    char host[] = "101.99.22.250";  //Địa chỉ IP dịch vụ, hãy thay đổi nó theo địa chỉ IP Socket server của bạn.
    int port = 3484;                  //Cổng dịch vụ socket server do chúng ta tạo!
     
    //từ khóa extern: dùng để #include các biến toàn cục ở một số thư viện khác. Trong thư viện SocketIOClient có hai biến toàn cục
    // mà chúng ta cần quan tâm đó là
    // RID: Tên hàm (tên sự kiện
    // Rfull: Danh sách biến (được đóng gói lại là chuối JSON)
    extern String RID;
    extern String Rfull;
    
    char* trangthai="TAT";
     
    //Một số biến dùng cho việc tạo một task
    unsigned long previousMillis = 0;
    long interval = 2000;
     
    void setup()
    {
        pinMode(D7, INPUT);
        //Khoi tao den va bat den len
        pinMode(LED_BUILTIN, OUTPUT);
        pinMode(D7, INPUT);
        digitalWrite(LED_BUILTIN, HIGH);
        //Bật baudrate ở mức 115200 để giao tiếp với máy tính qua Serial
        Serial.begin(115200);
        delay(10);
     
        //Việc đầu tiên cần làm là kết nối vào mạng Wifi
        Serial.print("Ket noi vao mang ");
        Serial.println(ssid);
     
        //Kết nối vào mạng Wifi
        WiFi.begin(ssid, password);
     
        //Chờ đến khi đã được kết nối
        while (WiFi.status() != WL_CONNECTED) { //Thoát ra khỏi vòng 
            delay(500);
            Serial.print('.');
        }
     
        Serial.println();
        Serial.println(F("Da ket noi WiFi"));
        Serial.println(F("Di chi IP cua ESP8266 (Socket Client ESP8266): "));
        Serial.println(WiFi.localIP());
     
        if (!client.connect(host, port)) {
            Serial.println(F("Ket noi den socket server that bai!"));
            return;
        }
     
        //Khi đã kết nối thành công
        if (client.connected()) {
            //Thì gửi sự kiện ("connection") đến Socket server ahihi.
            Serial.println(F("Gui 1 su kien connection len server"));
            client.send("connection", "message", "Connected !!!!");
        }
    }
     
    void loop()
    {
        //Serial.println(F("BAT DAU"));
        if (millis() - previousMillis > interval) {
            //lệnh:
            previousMillis = millis();
     
            //gửi sự kiện "atime" là một JSON chứa tham số message có nội dung là Time please?
            if(digitalRead(LED_BUILTIN))
            {
              trangthai="TAT";
            }
            else
            {
              trangthai="BAT";
            }
            if(trangthai=="BAT")
              Serial.println(F("Sau 2s gui trang thai cua den: BAT"));
            else
              Serial.println(F("Sau 2s gui trang thai cua den: TAT"));
            client.send("atime", "message", trangthai);
        }

        /*
        digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
        if(digitalRead(LED_BUILTIN))
        {
          Serial.println(F("Trang thai hien tai la BAT"));
        }
        else
        {
          Serial.println(F("Trang thai hien tai la TAT"));
        }
        delay(5000);                       // wait for a second
        digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
        if(digitalRead(LED_BUILTIN))
        {
          Serial.println(F("Trang thai hien tai la BAT"));
        }
        else
        {
          Serial.println(F("Trang thai hien tai la TAT"));
        }
        delay(10000); */

        
        //Khi bắt được bất kỳ sự kiện nào thì chúng ta có hai tham số:
        //  +RID: Tên sự kiện
        //  +RFull: Danh sách tham số được nén thành chuỗi JSON!
        if (client.monitor()) {
            Serial.println(RID);
            Serial.println(Rfull);

            JsonObject& root = jsonBuffer.parseObject(Rfull);
            if(!root.success()) {
              Serial.println("parseObject() failed");
            }
            else{
              Serial.println("parseObject() ok");
              if(root["led"][0])
                {
                  Serial.println("true");
                  Serial.println(F("Nhan duoc lenh la BAT"));
                  if(trangthai!="BAT")
                    {
                      digitalWrite(LED_BUILTIN, LOW);
                      pinMode(D7, OUTPUT);
                      Serial.println(F("Neu la TAT thi BAT len"));    
                      trangthai="BAT";
                    }
                }
              else 
                {
                  Serial.println("false");
                  Serial.println(F("Nhan duoc lenh la TAT"));
                  if(trangthai=="BAT")
                    {
                      digitalWrite(LED_BUILTIN, HIGH);
                      pinMode(D7, INPUT);
                      Serial.println(F("Neu dang la BAT thi TAT di"));    
                      trangthai="TAT";
                    }
                }
              if(root["led"][1])
                Serial.println("true");
              else 
                 Serial.println("false");
              }
        }
                    
        //Kết nối lại!
        if (!client.connected()) {
          Serial.println(F("Bi ngat ke noi thi ket noi lai"));
          client.reconnect(host, port);
        }
    }
