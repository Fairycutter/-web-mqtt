/*!
 * MindPlus
 * mpython
 *
 */
#include <MPython.h>
#include <DFRobot_Iot.h>
// 函数声明
void obloqMqttEventT0(String& message);
void obloqMqttEventT1(String& message);
void obloqMqttEventT2(String& message);
void obloqMqttEventT3(String& message);
// 静态常量
const String topics[5] = {"2018324145/ledRed","2018324145/ledGreen","2018324145/ledBlue","2018324145/fan",""};
const MsgHandleCb msgHandles[5] = {obloqMqttEventT0,obloqMqttEventT1,obloqMqttEventT2,obloqMqttEventT3,NULL};
// 创建对象
DFRobot_Iot myIot;


// 主程序开始
void setup() {
	mPython.begin();
	myIot.setMqttCallback(msgHandles);
	myIot.wifiConnect("602iot", "18wulian");
	while (!myIot.wifiStatus()) {yield();}
	display.setCursorLine(1);
	display.printLine("WiFi连接成功");
	myIot.init("192.168.31.75","siot","","dfrobot", topics, 1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(2);
	display.printLine("MQTT连接成功");
}
void loop() {

}

// 事件回调函数
void obloqMqttEventT0(String& message) {
	if ((message==String("ledRedOn"))) {
		rgb.write(0, 0xFF0000);
	}
	else {
		if ((message==String("ledRedOff"))) {
			rgb.write(0, 0x000000);
		}
	}
}
void obloqMqttEventT1(String& message) {
	if ((message==String("ledGreenOn"))) {
		rgb.write(1, 0x00FF00);
	}
	else {
		if ((message==String("ledGreenOff"))) {
			rgb.write(1, 0x000000);
		}
	}
}
void obloqMqttEventT2(String& message) {
	if ((message==String("ledBlueOn"))) {
		rgb.write(2, 0x0000FF);
	}
	else {
		if ((message==String("ledBlueOff"))) {
			rgb.write(2, 0x000000);
		}
	}
}
void obloqMqttEventT3(String& message) {
	if ((message==String("fanOn"))) {
		rgb.write(0, 0xFF0000);
		rgb.write(1, 0x00FF00);
		rgb.write(2, 0x0000FF);
	}
	else {
		if ((message==String("fanOff"))) {
			rgb.write(-1, 0x000000);
		}
	}
}
