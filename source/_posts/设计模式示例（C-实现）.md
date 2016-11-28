---
title: 设计模式示例（C++实现）
date: 2016-11-13 22:22:43
tags:
- 设计模式
- C++
categories:
- Software Engineer
- 设计模式
---

最近参加了软考，在准备考试过程中发现其中给出设计类图让通过指定的设计模式进行设计的题目对于练习C++和软件开发是一种不错的的方式，因为学校中的C++课程只讲C++语言，虽然也讲了封装、继承、多态等特性，但是缺少相应的练习，使得没有深入的认识和理解。同时类似于程序设计、软件工程的课程设计，其中概要设计、详细设计也都是文档写的很好看，最后编码从没按照设计中的内容来写。最终导致了学完C++，用的时候却不知从何下手。所以这里总结了[全国计算机技术与软件专业技术资格（水平）考试参考用书](http://www.ruankao.org.cn/)系列中的部分题目，希望可以达到以下两个目的：

<!--more-->

* 练习C++，掌握理解更多的特性，而不是只停留在掌握语法的阶段

* 提高软件开发方面的能力，主要是熟悉、学习基本的设计模式，学会如何设计并按照设计实现相应的编码

但需要特别指出的是设计模式还是推荐阅读这方面的经典书籍：[设计模式 可复用面向对象软件的基础](https://book.douban.com/subject/1052241/)。我只是总结软考中的题目是因为题目可以让我们快速的对问题有所了解，并且我暂时也只是想练习C++，但如果想要对问题有系统、详细的了解还是需要读书。

### Command（命令）模式

例：某灯具厂商欲生产一个灯具遥控器，该遥控器具有7个可编程的插槽，每个插槽都有开关按钮，对应着一个不同的灯。利用该遥控器能够统一控制房间内该厂商所有品牌灯具的开关，现采用Command（命令）模式实现该遥控器的软件部分。Command模式的类图如图所示。

![commandpattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/commandpattern.jpg)

```cpp
/*************************************************
* Command Pattern
* Author : robocoder
* Date : 2016 - 11 - 13
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/

#include <iostream>
#include <string>

using namespace std;

class Light{
private:
	string name;
public:
	Light(string name){ this->name = name; }
	void on(){ cout << name + " on" << endl; }
	void off(){ cout << name + " off" << endl; }
};

class Command{
public:
	virtual void execute(){}
};

class LightOnCommand :public Command{
private:
	Light* light;
public:
	LightOnCommand(Light* light){ this->light = light; }
	void execute(){ light->on(); }
};

class LightOffCommand :public Command{
private:
	Light* light;
public:
	LightOffCommand(Light* light){ this->light = light; }
	void execute(){ light->off(); }
};

class RemoteControl{
private:
	Command* onCommands[7];
	Command* offCommands[7];
public:
	RemoteControl(){}
	void setCommand(int slot, Command* onCommand, Command* offCommand){
		onCommands[slot] = onCommand;
		offCommands[slot] = offCommand;
	}
	void onButtonWasPushed(int slot){ onCommands[slot]->execute(); }
	void offButtonWasPushed(int slot){ offCommands[slot]->execute(); }
};

int main() {
	RemoteControl* remoteControl = new RemoteControl();
	Light* livingRoomLight = new Light("Living Room");
	Light* kitchenLight = new Light("Kitchen");
	LightOnCommand* livingRoomLightOn = new LightOnCommand(livingRoomLight);
	LightOffCommand* livingRoomLightOff = new LightOffCommand(livingRoomLight);
	LightOnCommand* kitchenLightOn = new LightOnCommand(kitchenLight);
	LightOffCommand* kitchenLightOff = new LightOffCommand(kitchenLight);
	remoteControl->setCommand(0, livingRoomLightOn, livingRoomLightOff);
	remoteControl->setCommand(1, kitchenLightOn, kitchenLightOff);
	remoteControl->onButtonWasPushed(0);
	remoteControl->offButtonWasPushed(0);
	remoteControl->onButtonWasPushed(1);
	remoteControl->offButtonWasPushed(1);
	return 0;
}
```

### Observer（观察者）模式

例：某实验室欲建立一个实验室环境监测系统，以显示实验室的温度、湿度以及洁净度等环境数据。当获取到最新的环境测量数据时，显示的环境数据能够更新。现在采用观察者（Observer）模式来开发该系统，观察者模式的类图如图所示。

![observerpattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/observerpattern.jpg)

```cpp
/*************************************************
* Observer Pattern
* Author : robocoder
* Date : 2016 - 11 - 13
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/

#include <iostream>
#include <vector>

using namespace std;

class Observer{
public:
	virtual void update(float temp, float humidity, float cleanness) = 0;
};

class Subject{
public:
	virtual void registerObserver(Observer* o) = 0;
	virtual void removeObserver(Observer* o) = 0;
	virtual void notifyObserver() = 0;
};

class EnvironmentData : public Subject{
private:
	vector<Observer*> observers;
	float temperature, humidity, cleanness;
public:
	void registerObserver(Observer* o){ observers.push_back(o); }
	void removeObserver(Observer* o){}
	void notifyObserver(){
		for (vector<Observer*>::const_iterator it = observers.begin(); it != observers.end(); it++)
		{
			(*it)->update(temperature, humidity, cleanness);
		}
	}
	void measurementsChanged(){ notifyObserver(); }
	void setMeasurements(float temperature, float humidity, float cleanness){
		this->temperature = temperature;
		this->humidity = humidity;
		this->cleanness = cleanness;
		measurementsChanged();
	}
};

class CurrentConditionsDisplay : public Observer{
private:
	float temperature, humidity, cleanness;
	Subject* envData;
public:
	CurrentConditionsDisplay(Subject* envData){
		this->envData = envData;
		this->envData->registerObserver(this);
	}
	void update(float temperature, float humidity, float cleanness){
		this->temperature = temperature;
		this->humidity = humidity;
		this->cleanness = cleanness;
		display();
	}
	void display(){
		cout << temperature << " " << humidity << " " << cleanness << endl;
	}
};

int main(){
	EnvironmentData* envData = new EnvironmentData();
	CurrentConditionsDisplay* currentDisplay = new CurrentConditionsDisplay(envData);
	envData->setMeasurements(80, 65, 30.4f);
	return 0;
}
```

### Bridge（桥接）模式

例：欲开发一个绘图软件，要求使用不同的绘图程序绘制不同的图形。以绘制直线和圆形为例，对应的绘图程序如表所示。

|  ^-^ |DP1|DP2|
| :--: | :--: | :--: |
|绘制直线|draw_a_line(x1,y1,x2,y2)|drawline(x1,y1,x2,y2)|
|绘制圆|draw_a_circle(x,y,r)|drawcircle(x,y,r)|

该绘制软件的扩展性要求，将不断扩充新的图形和新的绘图程序。为了避免出现类爆炸的情况，现采用桥接（Bridge）模式来实现上述要求，得到如图所示的类图。

![bridgepattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/bridge.jpg)

```cpp
/*************************************************
* Bridge Pattern
* Author : robocoder
* Date : 2016 - 11 - 15
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/

#include <iostream>

using namespace std;

class DP1{
public:
	static void draw_a_line(double x1, double y1, double x2, double y2){ cout << "DP1 draw_a_line" << endl; }
	static void draw_a_circle(double x, double y, double r){ cout << "DP1 draw_a_circle" << endl; }
};

class DP2{
public:
	static void drawline(double x1, double y1, double x2, double y2){ cout << "DP2 drawline" << endl; }
	static void drawcircle(double x, double y, double r){ cout << "Dp2 drawcircle" << endl; }
};

class Drawing{
public:
	virtual void drawLine(double x1, double y1, double x2, double y2) = 0;
	virtual void drawCircle(double x, double y, double r) = 0;
};

class V1Drawing : public Drawing{
public:
	void drawLine(double x1, double y1, double x2, double y2){ DP1::draw_a_line(x1, y1, x2, y2); cout << x1 << " " << y1 << " " << x2 << " " << y2 << endl; }
	void drawCircle(double x, double y, double r){ DP1::draw_a_circle(x, y, r); cout << x << " " << y << " " << r << endl; }
};

class V2Drawing : public Drawing{
public:
	void drawLine(double x1, double y1, double x2, double y2){ DP2::drawline(x1, y1, x2, y2); cout << x1 << " " << y1 << " " << x2 << " " << y2 << endl; }
	void drawCircle(double x, double y, double r){ DP2::drawcircle(x, y, r); cout << x << " " << y << " " << r << endl; }
};

class Shape{
public:
	Shape(Drawing* dp){ _dp = dp; }
	void drawLine(double x1, double y1, double x2, double y2){ _dp->drawLine(x1, y1, x2, y2); }
	void drawCircle(double x, double y, double r){ _dp->drawCircle(x, y, r); }
private:
	Drawing* _dp;
};

class Rectangle : public Shape{
private:
	double _x1, _y1, _x2, _y2;
public:
	Rectangle(Drawing* dp, double x1, double y1, double x2, double y2) : Shape(dp){ _x1 = x1; _y1 = y1; _x2 = x2; _y2 = y2; }
	void draw(){ drawLine(_x1, _y1, _x2, _y2); }
};

class Circle : public Shape{
private:
	double _x, _y, _r;
public:
	Circle(Drawing* dp, double x, double y, double r) : Shape(dp){ _x = x; _y = y; _r = r; }
	void draw(){ drawCircle(_x, _y, _r); }
};

int main(){
	V1Drawing* dp1 = new V1Drawing();
	V2Drawing* dp2 = new V2Drawing();
	Rectangle* rectangle1 = new Rectangle(dp1, 0, 1, 1, 1);
	rectangle1->draw();
	Rectangle* rectangle2 = new Rectangle(dp2, 0, 1, 1, 1);
	rectangle2->draw();
	Circle* circle1 = new Circle(dp1, 0, 1, 1);
	circle1->draw();
	Circle* circle2 = new Circle(dp2, 0, 1, 1);
	circle2->draw();
	return 0;
}
```

### Prototype（原型）模式

例：现要求实现一个能够自动生成求职简历的程序，简历的基本内容包括求职者的姓名、性别、年龄及工作经历。希望每份简历中的工作经历有所不同，并尽量减少程序中的重复代码。
现采用原型模式（Prototype）来实现上述要求，得到如图所示的类图。

![prototypepattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/prototype.jpg)

```cpp
/*************************************************
* Prototype Pattern
* Author : robocoder
* Date : 2016 - 11 - 15
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>
#include <string>

using namespace std;

class Cloneable{
public:
	virtual Cloneable* clone() = 0;
};

class WorkExperience : public Cloneable{
private:
	string workDate;
	string company;
public:
	Cloneable* clone(){
		WorkExperience* obj = new WorkExperience();
		obj->workDate = this->workDate;
		obj->company = this->company;
		return obj;
	}
	void setWorkDate(string wd){ 
		workDate = wd; 
	}
	void setCompany(string comp){ 
		company = comp; 
	}
	string toString(){
		return workDate + company;
	}
};

class Resume : public Cloneable{
private:
	string name, sex, age;
	WorkExperience* work;
	Resume(WorkExperience* work){
		this->work = (WorkExperience*)work->clone();
	}
public:
	Resume(string name){ 
		work = new WorkExperience(); 
		this->name = name; 
	}
	void setPersonalInfo(string sex, string age){ 
		this->sex = sex; 
		this->age = age; 
	}
	void setWorkExperience(string workDate, string company){ 
		work->setWorkDate(workDate); 
		work->setCompany(company); 
	}
	Cloneable* clone(){
		Resume* obj = new Resume(this->work);
		obj->name = this->name;
		obj->sex = this->sex;
		obj->age = this->age;
		return obj;
	}
	string toString(){
		return name + sex + age + work->toString();
	}
};

int main(){
	Resume* a = new Resume("张三");
	a->setPersonalInfo("男", "29");
	a->setWorkExperience("1998-2000", "A company");
	cout << a->toString() << endl;
	Resume* b = (Resume*)a->clone();
	b->setWorkExperience("2001-2006", "B company");
	cout << b->toString() << endl;
	cout << a->toString() << endl;
	return 0;
}
```

### Abstract Factory（抽象工厂）模式

例：现欲开发一个软件系统，要求能够同时支持多种不同的数据库，为此采用抽象工厂模式设计该系统。以SQL Server和Access两种数据库以及系统中的数据库表Department为例，其类图如图所示。

![abstractfactorypattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/abstractfactory.jpg)

```cpp
/*************************************************
* Abstract Factory Pattern
* Author : robocoder
* Date : 2016 - 11 - 16
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>

using namespace std;

//数据库表
class Department{};

class IDepartment{
public:
	virtual void insert(Department* department) = 0;
	virtual Department* getDepartment(int id) = 0;
};

class SqlserverDepartment :public IDepartment{
public:
	void insert(Department* department){
		cout << "Insert a record into Department in SQL Server!\n";
	}
	Department* getDepartment(int id){ return new Department(); }
};

class AccessDepartment :public IDepartment{
public:
	void insert(Department* department){
		cout << "Insert a record into Department in ACCESS!\n";
	}
	Department* getDepartment(int id){ return new Department(); }
};

class IFactory{
public:
	virtual IDepartment* creatDepartment() = 0;
};

class SqlserverFactory :public IFactory{
public:
	IDepartment* creatDepartment(){
		return new SqlserverDepartment();
	}
};

class AccessFactory :public IFactory{
public:
	IDepartment* creatDepartment(){
		return new AccessDepartment();
	}
};

int main(){
	SqlserverFactory* sqlFactory = new SqlserverFactory();
	SqlserverDepartment* sqlDepartment = (SqlserverDepartment*)sqlFactory->creatDepartment();
	Department* a = new Department();
	sqlDepartment->insert(a);

	AccessFactory* accessFactory = new AccessFactory();
	AccessDepartment* accessDepartment = (AccessDepartment*)accessFactory->creatDepartment();
	accessDepartment->insert(a);
	return 0;
}
```

### Decorator（装饰）模式

例：某咖啡店在卖咖啡时，可以根据顾客的要求在其中加入各种配料，咖啡店会根据所加入的配料来计算费用。咖啡店所供应的咖啡及配料的种类和价格如表所示。

|咖啡|价格/杯（￥）|配料|价格/杯（￥）|
|:--:|:--:|:--:|:--:|
|蒸馏咖啡（Espresso）|25|摩卡（Mocha）|10|
|深度烘焙咖啡（DarkRoast）|20|奶泡（Whip）|8|

现采用装饰器（Decorator）模式来实现计算费用的功能，得到如图所示的类图。

![decoratorpattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/decorator.jpg)

```cpp
/*************************************************
* Decorator Pattern
* Author : robocoder
* Date : 2016 - 11 - 16
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>
#include <string>

using namespace std;

const int ESPRESSO_PRICE = 25;
const int DARKROAST_PRICE = 20;
const int MOCHA_PRICE = 10;
const int WHIP_PRICE = 8;

//饮料
class Beverage{
protected:
	string description;
public:
	virtual string getDescription(){
		return description;
	}
	virtual int cost() = 0;
};

//配料
class CondimentDecorator :public Beverage{
protected:
	Beverage* beverage;
};

//蒸馏咖啡
class Espresso :public Beverage{
public:
	Espresso()
	{
		description = "Espresso";
	}
	int cost(){
		return ESPRESSO_PRICE;
	}
};

//深度烘焙咖啡
class DarkRoast :public Beverage{
public:
	DarkRoast()
	{
		description = "DarkRoast";
	}
	int cost(){
		return DARKROAST_PRICE;
	}
};

//摩卡
class Mocha :public CondimentDecorator{
public:
	Mocha(Beverage* beverage){
		this->beverage = beverage;
	}
	string getDescription(){
		return beverage->getDescription() + ",Mocha";
	}
	int cost(){
		return MOCHA_PRICE + beverage->cost();
	}
};

//奶泡
class Whip :public CondimentDecorator{
public:
	Whip(Beverage* beverage){
		this->beverage = beverage;
	}
	string getDescription(){
		return beverage->getDescription() + ",Whip";
	}
	int cost(){
		return WHIP_PRICE + beverage->cost();
	}
};

int main(){
	Beverage* beverage = new DarkRoast();
	beverage = new Mocha(beverage);
	beverage = new Whip(beverage);
	cout << beverage->getDescription() << "￥" << beverage->cost() << endl;
	return 0;
}
```

### Composition（组合）模式

例：某饭店在不同的时段提供多种不同的餐饮，其菜单的结构图如图所示。

![composition](http://ofzyomgms.bkt.clouddn.com/designpatterns/compositionstructure.jpg)

现在采用组合（Composition）模式来构造该饭店的菜单，使得饭店可以方便地在其中增加新的餐饮形式，得到如图所示的类图。其中MenuComposition为抽象类，定义了添加（add）新菜单和打印饭店所有菜单信息（print）的方法接口。类Menu表示饭店提供的每种餐饮形式的菜单，如煎饼屋菜单、咖啡屋菜单。每种菜单中都可以添加子菜单，例如图中的甜点菜单。类MenuItem表示菜单中的菜式。

![compositionpattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/composition.jpg)

```cpp
/*************************************************
* Composition Pattern
* Author : robocoder
* Date : 2016 - 11 - 16
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>
#include <list>
#include <string>

using namespace std;

class MenuComponent{
protected:
	string name;
public:
	MenuComponent(string name){
		this->name = name;
	}
	string getName(){
		return name;
	}
	virtual void add(MenuComponent* menuComponent) = 0; //添加新菜单
	virtual void print() = 0; //打印菜单信息
};

class MenuItem :public MenuComponent{
private:
	double price;
public:
	MenuItem(string name, double price) :MenuComponent(name){ this->price = price; }
	double getPrice(){ return price; }
	void add(MenuComponent* menuComponent){ return ; }
	void print(){
		cout << " " << getName() << "," << getPrice() << endl;
	}
};

class Menu :public MenuComponent{
private:
	list<MenuComponent*> menuComponents;
public:
	Menu(string name) :MenuComponent(name){}
	void add(MenuComponent* menuComponent){
		menuComponents.push_back(menuComponent); 
	}
	void print(){
		cout << "\n" << getName() << "\n-----------------------------" << endl;
		std::list<MenuComponent*>::iterator iter;
		for (iter = menuComponents.begin(); iter != menuComponents.end();iter++)
		{
			(*iter)->print();
		}
	}
};

int main(){
	MenuComponent* allMenus = new Menu("ALL MENUS");
	MenuComponent* dinerMenu = new Menu("DINER　MENU");
	MenuComponent* coffee = new MenuItem("Espresso", 20);
	allMenus->add(coffee);
	MenuComponent* dumpling = new MenuItem("Dumpling", 30);
	dinerMenu->add(dumpling);
	allMenus->add(dinerMenu);
	allMenus->print();
	return 0;
}
```

### State（状态）模式

例：某大型商场内安装了多个简易的纸巾售卖机，自动出售2元钱一包的纸巾，且每次仅售出一包纸巾。纸巾售卖机的状态图如图所示。

![state](http://ofzyomgms.bkt.clouddn.com/designpatterns/state1.jpg)

采用状态（State）模式来实现该纸巾售卖机，得到如图所示的类图。其中类State为抽象类，定义了投币、退币、出纸巾等方法接口。类SoldState、SoldOutState、NoQuarterState和HasQuarterState分别对应图中纸巾售卖机的4种状态：售出纸巾、纸巾售完、没有投币、有2元钱。

![statepattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/state.jpg)

```cpp
/*************************************************
* State Pattern
* Author : robocoder
* Date : 2016 - 11 - 17
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>

using namespace std;

class TissueMachine;

class State{
public:
	virtual void insertQuarter() = 0;//投币
	virtual void ejectQuarter() = 0;//退币
	virtual void turnCrank() = 0;//按下“出纸巾”按钮
	virtual void dispense() = 0;//出纸巾
};

//没有投币
class NoQuarterState :public State{
private:
	TissueMachine* tissueMachine;
public:
	NoQuarterState(TissueMachine* tissueMachine){ this->tissueMachine = tissueMachine; }
	void insertQuarter();
	void ejectQuarter();
	void turnCrank();
	void dispense();
};

//有2元钱（已投币）
class HasQuarterState :public State{
private:
	TissueMachine* tissueMachine;
public:
	HasQuarterState(TissueMachine* tissueMachine){ this->tissueMachine = tissueMachine; }
	void insertQuarter();
	void ejectQuarter();
	void turnCrank();
	void dispense();
};

//售出纸巾
class SoldState :public State{
private:
	TissueMachine* tissueMachine;
public:
	SoldState(TissueMachine* tissueMachine){ this->tissueMachine = tissueMachine; }
	void insertQuarter();
	void ejectQuarter();
	void turnCrank();
	void dispense();
};

//纸巾售完
class SoldOutState :public State{
private:
	TissueMachine* tissueMachine;
public:
	SoldOutState(TissueMachine* tissueMachine){ this->tissueMachine = tissueMachine; }
	void insertQuarter();
	void ejectQuarter();
	void turnCrank();
	void dispense();
};

class TissueMachine{
private:
	State *soldOutState, *noQuarterState, *hasQuarterState, *soldState, *state;
	int count;
public:
	TissueMachine(int numbers){
		soldOutState = new SoldOutState(this);
		noQuarterState = new NoQuarterState(this);
		hasQuarterState = new HasQuarterState(this);
		soldState = new SoldState(this);
		this->count = numbers;
		if (count > 0)
		{
			this->state = noQuarterState;
		}
	}

	void insertQuarter(){ state->insertQuarter(); }
	void ejectQuarter(){ state->ejectQuarter(); }
	void turnCrank(){
		state->turnCrank();
		state->dispense();
	}
	void setState(State* state){ this->state = state; }
	State* getHasQuarterState(){ return hasQuarterState; }
	State* getNoQuarterState(){ return noQuarterState; }
	State* getSoldState(){ return soldState; }
	State* getSoldOutState(){ return soldOutState; }
	void setCount(int numbers){ this->count = numbers; }
	int getCount(){ return count; }
};

void SoldOutState::insertQuarter(){ cout << "机器无纸巾，已退回硬币！" << endl; }
void SoldOutState::ejectQuarter(){ cout << "自动售货机根本没有硬币！" << endl; }
void SoldOutState::turnCrank(){ cout << "机器无纸巾，请不要操作机器" << endl; }
void SoldOutState::dispense(){}

void NoQuarterState::insertQuarter(){
	tissueMachine->setState(tissueMachine->getHasQuarterState());
	cout << "已投币！" << endl;
}
void NoQuarterState::ejectQuarter(){ cout << "自动售货机根本没有硬币！" << endl; }
void NoQuarterState::turnCrank(){ cout << "请投币" << endl; }
void NoQuarterState::dispense(){}

void HasQuarterState::insertQuarter(){ cout << "已投币！请不要重复投币！已退回重复投币！" << endl; }
void HasQuarterState::ejectQuarter(){
	tissueMachine->setState(tissueMachine->getNoQuarterState());
	cout << "已取币！" << endl;
}
void HasQuarterState::turnCrank(){
	tissueMachine->setState(tissueMachine->getSoldState());
	cout << "请等待自动售货机出纸巾！" << endl;
}
void HasQuarterState::dispense(){}

void SoldState::insertQuarter(){ cout << "请等待自动售货机出纸巾！请不要投币！已退回投币！" << endl; }
void SoldState::ejectQuarter(){
	tissueMachine->setState(tissueMachine->getNoQuarterState());
	cout << "请等待自动售货机出纸巾！无法取回已消费的硬币！" << endl;
}
void SoldState::turnCrank(){ cout << "请等待自动售货机出纸巾！已响应你的操作！" << endl; }
void SoldState::dispense(){
	if (tissueMachine->getCount() > 0){
		tissueMachine->setState(tissueMachine->getNoQuarterState());
		tissueMachine->setCount(tissueMachine->getCount() - 1);
		cout << "你的纸巾，请拿好！" << endl;
	}
	else{
		tissueMachine->setState(tissueMachine->getSoldOutState());
		cout << "已退回你的硬币！纸巾已卖光，等待进货！" << endl;
	}
}

int main(){
	TissueMachine *tissueMachine = new TissueMachine(1);
	cout << "纸巾数：" << tissueMachine->getCount() << endl;
	tissueMachine->insertQuarter();
	tissueMachine->turnCrank();
	cout << "纸巾数：" << tissueMachine->getCount() << endl;
	tissueMachine->turnCrank();
	cout << "纸巾数：" << tissueMachine->getCount() << endl;
	tissueMachine->insertQuarter();
	tissueMachine->turnCrank();
	return 0;
}
```

### Chain of Responsibility（责任链）模式

例：已知某企业的采购审批是分级进行的，即根据采购金额的不同由不同层次的主管人员来审批，主任可以审批5万元以下（不包括5万元）的采购单，副董事长可以审批5万元至10万元（不包括10万元）的采购单，董事长可以审批10万元至50万元（不包括50万元）的采购单，50万元及以上的采购单就需要开会讨论决定。

采用责任链设计模式（Chain of Responsibility）对上述过程进行设计后得到的类图如图所示。

![chainofresponsibilitypattern](http://ofzyomgms.bkt.clouddn.com/designpatterns/chainofresponsibility.jpg)

```cpp
/*************************************************
* Chain of Responsibility Pattern
* Author : robocoder
* Date : 2016 - 11 - 17
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>
#include <string>

using namespace std;

//采购单
class PurchaseRequest{
public:
	double amount;//采购金额
	int number;//采购单编号
	string purpose;//采购目的
};

//审批者
class Approver{
private:
	Approver* successor;
public:
	Approver(){
		successor = NULL;
	}
	virtual void processRequest(PurchaseRequest aRequest){
		if (successor != NULL)
		{
			successor->processRequest(aRequest);
		}
	}
	void setSuccessor(Approver* aSuccessor){
		successor = aSuccessor;
	}
};

//例会
class Congress :public Approver{
public:
	void processRequest(PurchaseRequest aRequest){
		if (aRequest.amount >= 500000){
			cout << "例会正在审批" << endl;
		}
		else
		{
			Approver::processRequest(aRequest);
		}
	}
};

//董事长
class President :public Approver{
public:
	void processRequest(PurchaseRequest aRequest){
		if (aRequest.amount >= 100000 && aRequest.amount < 500000)
		{
			cout << "董事长正在审批" << endl;
		}
		else
		{
			Approver::processRequest(aRequest);
		}
	}
};

//副董事长
class VicePresident :public Approver{
public:
	void processRequest(PurchaseRequest aRequest){
		if (aRequest.amount >= 50000 && aRequest.amount < 100000)
		{
			cout << "副董事长正在审批" << endl;
		}
		else
		{
			Approver::processRequest(aRequest);
		}
	}
};

//主任
class Director :public Approver{
public:
	void processRequest(PurchaseRequest aRequest){
		if (aRequest.amount < 50000)
		{
			cout << "主任正在审批" << endl;
		} 
		else
		{
			Approver::processRequest(aRequest);
		}
	}
};

int main(){
	Congress meeting;
	President Tammy;
	VicePresident Sam;
	Director Larry;
	meeting.setSuccessor(NULL);
	Tammy.setSuccessor(&meeting);
	Sam.setSuccessor(&Tammy);
	Larry.setSuccessor(&Sam);

	PurchaseRequest aRequest;
	cin >> aRequest.amount;
	Larry.processRequest(aRequest);
	return 0;
}
```

### Strategy（策略）模式

例：某游戏公司现欲开发一款面向儿童的模拟游戏，该游戏主要模拟现实世界中各种鸭子的发声特征、飞行特征和外观特征。游戏需要模拟的鸭子种类及其特征如表所示。

|鸭子种类|发声特征|飞行特征|外观特征|
|:--:|:--:|:--:|:--:|
|灰鸭(MallardDuck)|发出“嘎嘎”声(Quack)|用翅膀飞行(FlyWithWings)|灰色羽毛|
|红头鸭(RedHeadDuck)|发出“嘎嘎”声(Quack)|用翅膀飞行(FlyWithWings)|灰色羽毛、头部红色|
|棉花鸭(CottonDuck)|不发声(QuackNoWay)|不能飞行(FlyNoWay)|白色|
|橡皮鸭(RubberDuck)|发出橡皮与空气摩擦的声音(Squeak)|不能飞行(FlyNoWay)|黑白橡皮色|

为支持将来能够模拟更多种类鸭子的特性，采用策略（Strategy）模式设计的类图如图所示。

![strategy](http://ofzyomgms.bkt.clouddn.com/designpatterns/strategy.jpg)

其中，Duck为抽象类，描述了抽象的鸭子，而类RubberDuck、MallardDuck、CottonDuck和RedHeadDuck分别描述具体的鸭子种类，方法fly()、quack()和display()分别表示不同种类的鸭子都具有飞行特征、发声特征和外观特征；类FlyBehavior与QuackBehavior为抽象类，分别用于表示抽象的飞行行为与发声行为；类FlyNoWay与FlyWithWings分别描述不能飞行的行为和用翅膀飞行的行为；类Quack、Squeak与QuackNoWay分别描述发出“嘎嘎”声的行为、发出橡皮与空气摩擦声的行为与不发声的行为。

```cpp
/*************************************************
* Strategy Pattern
* Author : robocoder
* Date : 2016 - 11 - 17
* HomePage : https://robocoderhan.github.io/
* Email : robocoder@foxmail.com
*************************************************/
#include <iostream>

using namespace std;

class FlyBehavior{
public:
	virtual void fly() = 0;
};

class QuackBehavior{
public:
	virtual void quack() = 0;
};

class FlyWithWings :public FlyBehavior{
public:
	void fly(){ cout << "使用翅膀飞行" << endl; }
};

class FlyNoWay :public FlyBehavior{
public:
	void fly(){ cout << "不能飞行" << endl; }
};

class Quack :public QuackBehavior{
public:
	void quack(){ cout << "发出\'嘎嘎\'声！" << endl; }
};

class Squeak :public QuackBehavior{
public:
	void quack(){ cout << "发出空气与橡皮摩擦声！" << endl; }
};

class QuackNoWay :public QuackBehavior{
public:
	void quack(){ cout << "不能发声！" << endl; }
};

class Duck{
protected:
	FlyBehavior* flyBehavior;
	QuackBehavior* quackBehavior;
public:
	void fly(){ flyBehavior->fly(); }
	void quack(){ quackBehavior->quack(); }
	virtual void display() = 0;
};

class RubberDuck :public Duck{
public:
	RubberDuck(){
		flyBehavior = new FlyNoWay();
		quackBehavior = new Squeak();
	}
	~RubberDuck(){
		if (!flyBehavior) delete flyBehavior;
		if (!quackBehavior) delete quackBehavior;
	}
	void display(){}
};

int main(){
	RubberDuck rubberDuck;
	rubberDuck.fly();
	rubberDuck.quack();
	return 0;
}
```