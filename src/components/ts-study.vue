<template>
    <div>
    </div>
</template>

<script setup lang="ts">

let a: boolean = true; 
let b: number = 10;
let c: string = "哈哈哈"
const sym = Symbol();
let obj = {
    [sym]: "semlinker",
}//symbol

let d: object[] = [{ 'sss': '2' }, { 'sss': '1' }];
let d1: string[] = ['1', '2', '3'];
let d2: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
let list2: Array<object> = [{ 'sss': '2' }, { 'sss': '1' }];
let list3: Array<string> = ['1', '2', '3'];

//元组 Tuple:元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
//比如，可以定义一对值分别为string和number类型的元组。
let x : [string, number] = ['aaa', 123];
// x[2] = '5555'
console.log(x, 'ddd');

//枚举 enum：枚举类型可以为一组数值赋予友好的名字。
enum Color {Red=2, Green, Blue=3}
let cc : Color = Color.Blue;
// let zz : Color = Color[2];

//任意值 any：还不清楚类型的变量指定一个类型
let e: any = 4;
e = "maybe a string instead";
e = false;
//只知道一部分数据的类型时，any类型也是有用的。
// 比如，有一个数组，它包含了不同的类型的数据
let an: any[] = [1, true, "free"];
an[1] = 100;


//空值 void：声明一个void类型的变量,只能为它赋予undefined和null
//或者就是函数返回为空（void）
let v: void = undefined;

//Null 和 Undefined
let u: undefined = undefined;
let n: null = null;

//never：永不存在的值的类型
//any也不可以赋值给never
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}
// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}
// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}

//接口
interface LabelledValue {
    label: string;
    width?: number; //带有可选属性的接口
    readonly x: number; //只读属性
}
//函数类型
interface SearchFunc {
    (source: string, subString: string): boolean;
}
//可索引类型
interface StringArray {
    [index: number]: string
}
//类接口
interface clockInterface{
    currentTime: Date;
    setTime(d: Date): any;
}
class Clock1 implements clockInterface {
    currentTime: Date | undefined;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number){};
}
//类静态部分与实力部分
interface ClockConstructor1 {
    new (hour: number, minute: number): any;
}
class Clock2 implements ClockConstructor1 {
    currentTime: Date | undefined;
    constructor(h: number, m: number) {}
}
//继承接口
interface Shape {
    color: string;
}
interface Square extends Shape {
    sideLength: number;
}
//混合类型
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
function getCounter(): Counter {
    let counter = <Counter>function (start: number){};
    counter.interval = 123;
    counter.reset = function(){};
    return counter;
}
//接口继承类
class Control {
    private state: any;
}
interface SelecttableControl extends Control{
    select(): void;
}
class Button extends Control implements SelecttableControl {
    select() {}
}
class TextBox extends Control {

}


//类
class Gereter{
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        console.log("Hello：" + this.greeting);
        return "Hello：" + this.greeting
    }
}
let greeter = new Gereter("world");
greeter.greet();
//继承
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}
class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!', 'dog');
    }
}
const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark()
class Animal1 {
    name: string;
    constructor(theName: string){this.name = theName}
    move(distanceInMeters: number = 0) {
        console.log(this.name + 'moved' + distanceInMeters);
    }
}
//继承eg2（稍复杂）
//typescript强制规定
//派生类在构造函数访问this之前一定要调用super()函数
//super函数会执行基类的构造函数
//默认public
class snake extends Animal1 {
    constructor(name: string){
        super(name)
    }
    move(distanceInMeters=5){
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}
class horse extends Animal1{
    constructor(name:string){
        super(name);
    }
    move(distanceInMeters=45){
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}
let sam = new snake('Sammy the Python');
let tom = new horse('Tommy the palomino');
sam.move()
tom.move(34)


//函数（function）
//也可以匿名函数
let myAdd = function(x: number, y: number): number{return x+y;}
//TypeScript里的每个函数参数都是必须的,不能传递null或undefined作为参数
//传递给一个函数的参数个数必须与函数期望的参数个数一致。
//在参数名旁使用?实现可选参数的功能
//可选参数必须跟在必须参数后面。
//如果我们想让first name是可选的，那么就必须调整它们的位置，把first name放在后面。
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
//带默认值的参数出现在必须参数前面，用户必须明确的传入undefined值来获得默认值
function buildName1(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}
let result3 = buildName("Bob", "Adams");
let result4 = buildName1(undefined, "Adams");
// console.log(result4, 'res4');


//泛型（generic）:使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据
function identity<T>(arg: T): T {
    return arg;
}
//使返回值的类型与传入参数的类型是相同的 类型变量<T>
//T帮助我们捕获用户传入的类型
//我们定义了泛型函数后，可以用两种方法使用。 第一种是，传入所有的参数，包含类型参数

//这里明确的指定了T是string类型，并做为一个参数传给函数，使用了<>括起来而不是()。
let output = identity<string>('myString');
//利用了类型推论 – 即编译器会根据传入的参数自动地帮助我们确定T的类型：
let output1 = identity('myString');
//泛型接口
interface GenericIdentityFn<T> {
    (arg: T): T;
}
function identity1<T>(arg: T): T {
    return arg;
}
let myIdentity: GenericIdentityFn<number> = identity1;
//泛型类
class GenericNumber<T> {
    zeroValue: T | undefined;
    add: ((x: T, y: T) => T) | undefined;
}
let  myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

//枚举（enum）定义有名字的数字常量
enum Direction {
    Up = 1,
    Down = 2*2,
    Left,
    Right
}
// 枚举成员被当作是常数:
//1、不具有初始化函数并且之前的枚举成员是常数
//2、枚举成员使用常数枚举表达式初始化
//注意：要么都赋值、要么都不赋值或者number
//使用
let derections = [Direction.Down, Direction.Left, Direction.Right, Direction.Up]
console.log(derections, 'derections')


//类型推论（type inference）：类型是在哪里如何被推断的
let x1 = 33;//变量x1的类型被推断为数字
let x2 = [0, 1, null];
//为了推断x2的类型，我们必须考虑所有元素的类型。 
//这里有两种选择：number和null。 
//计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型。

//symbols:原生类型
let symb1 = Symbol();
let symb2 = Symbol('key');
//Symbols是不可改变且唯一的
let symb3 = Symbol('key');
console.log(symb2 === symb3);//false
//像字符串一样，symbols也可以被用做对象属性的键。
let sym4 = Symbol();
let obj4 = {
    [sym4]: "value"
};
console.log(obj4[sym4]);//value
//Symbols也可以与计算出的属性名声明相结合来声明对象的属性和类成员
const getClassNameSymbol = Symbol();
class C4 {
    [getClassNameSymbol](){
        return "C4"
    }
}
let c4 = new C4();
let className = c4[getClassNameSymbol]();
console.log(className, 'calssNAme');//C4


//Iterators和Generators:可迭代性
//当一个对象实现了Symbol.iterator属性时，我们认为它是可迭代的
//例如：Array\Map\Set\String...
//for..of会遍历可迭代的对象，调用对象上的Symbol.iterator方法
let someArray = [1, 'string', false];
for(let entry of someArray) {
    console.log(entry, 'entry');// 1, "string", false
}
for (let i in list) {
    console.log(i); // "0", "1", "2",
}
//for..in可以操作任何对象；它提供了查看对象属性的一种方法。 
//但是for..of关注于迭代对象的值。
let maps = new Set(someArray);
console.log(maps);


//模块：从ECMAScript 2015开始，JavaScript引入了模块的概念。TypeScript也沿用这个概念。
//模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。
//模块使用模块加载器去导入其它的模块
//导出：export/导入：import
//命名空间（内部模块）：写法namespace X {}
//Base URL：baseUrl来告诉编译器到哪里去查找模块
//声明合并：编译器将针对同一个名字的两个独立声明合并为单一声明
//合并后的声明同时拥有原先两个声明的特性，任何数量的声明都可被合并；不局限于两个声明


// function printLabel(labelledObj: LabelledValue) {
//     console.log(labelledObj.width, 'lllll')
// }
// let myObj = {label: 'size 10 Object', width: 222, x: 10}
// printLabel(myObj);




</script>

<style scoped></style>
