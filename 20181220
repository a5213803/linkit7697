#include <LWiFi.h>
#include "MCS.h"
#include "DHT.h"

int t;
int h;

char _lwifi_ssid[] = "Ws";
char _lwifi_pass[] = "0989787226";

MCSDevice mcs("DAYgMDXC", "1b5iElzuvnkc9eqC");
MCSDisplayInteger temp("temp");
MCSDisplayInteger humid("humid");
MCSControllerOnOff MySwitch("myswitch");

DHT __dht2(4, DHT11);

void setup()
{
  Serial.begin(9600);

  mcs.addChannel(temp);
  mcs.addChannel(humid);
  mcs.addChannel(MySwitch);
  Serial.println("Wi-Fi 開始連線");
  while (WiFi.begin(_lwifi_ssid, _lwifi_pass) != WL_CONNECTED) { delay(1000); }
  Serial.println("Wi-Fi 連線成功");
  while(!mcs.connected()) { mcs.connect(); }
  Serial.println("MCS 連線成功");
  Serial.begin(9600);
  pinMode(LED_BUILTIN, OUTPUT);
  __dht2.begin();
}

void loop()
{
  while (!mcs.connected()) {
    mcs.connect();
    if (mcs.connected()) { Serial.println("MCS 已重新連線"); }
  }
  mcs.process(100);

  t = __dht2.readTemperature();
  h = __dht2.readHumidity();
  temp.set(t);
  humid.set(h);

  Serial.print("攝氏溫度 : ");
  Serial.println(t);
  Serial.print("相對溼度 :");
  Serial.println(h);
  
  if (MySwitch.updated() || t > 27){
    Serial.print("控制通道更新 :");
    Serial.println(MySwitch.value());

    if (MySwitch.value() || t> 27) {
      digitalWrite(LED_BUILTIN, HIGH);

    } 
    else {
      digitalWrite(LED_BUILTIN, LOW);
    }
     }
  delay(1000);
}
