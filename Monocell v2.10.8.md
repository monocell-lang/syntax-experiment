# Monocell progarming language  
(Version 2.10)  
By ConFusion  
  
  
  
  
**CONTENTS:**  
**# Program structure + toolchains**  
**# Variables**  
**# Primitive types**  
**# Comments**  
**# Arrays**  
**# Composites**  
**# Conditional**  
**# Loops**  
**# Special controls**  
**# Optionals**  
**# Console IO**  
**# Functions**  
**# Function types**  
**# Structs**  
**# Enums**  
**# Modules**  
**# Static variables**  
**# Constants**  
  
  
  
  
  
  
## # Program structure + toolchains  
### Toolchains  
```
Language name:        Monocell
Source extension:     .mcs
Module extension:     .mcm
Developed since:      May 2025
Compiler:             cell
Project manager:      Cauldron (cdrr)
Mascot:               slime

```
  
```
Compile          cell main.mcs -dbg app.exe
                 cell main.mcs -rls app.exe

Run              ./app.exe

```
  
```
Create           cdrr new ProjectName
                 cdrr summon ProjectName

Build            cdrr build
                 cdrr cook

Debug without    cdrr debug
compilation      cdrr seethru

Run              cdrr run
                 cdrr serve

Help             cdrr help
                 cdrr helpmepls

Add module       cdrr addmod ModuleName
                 cdrr minion ModuleName

Module tree      cdrr viewproj
visual           cdrr hierachy

```
  
### Program structure  
```
void main() {
   print("Hello world");
}

/// run:   ./helloworld.exe

```
  
```
void main(str[2] args)
{
   str& name = args[0];
   int age = ((<int?>(age) == true{val})? val, 0);
   print("Hi, I'm {name}, {age} years old");
}

/// run:   ./greet.exe ConFusion 21

```
  
  
  
  
  
## # Variables  
### Declaration  
* Monocell is a statically typed language, which means the type of every variable must be known as soon as it's declared  
  
Example 1: type and value  
```
int x = 9; 
// type: int
// value: 9

str s = "hello";
// type: str
// value: "hello"

```
  
Example 2: naming  
```
wchar FrogEmoji = '🐸';     //✅
float _range = 1.2;         //✅
ulong house_number = 20;    //✅

wstr _ = "hello";     //❌ _ is a keyword

```
  
Example 3: immutable vs mutable  
```
str myname = "Kelly";
//👉 myname is immutable

color mut mycolor = sky_blue;
//👉 mycolor is mutable

myname = "Charlie";  //❌ cant change value
mycolor = banana_yellow;  //✅

```
  
Example 4: array elements  
```
bool[4] isfull = { false, false, false, false };
//👉 isfull elements are immutable

uint[2] mut fib = { 0, 1 };
//👉 fib elements are mutable

isfull[2] = true;  //❌ cant change value
fib[0] = 3;  //✅
fib[1] = 5;  //✅

```
  
### Scope and shadowing  
* The scope of a variable tells us where its declared and destroyed  
* When it goes outa scope, its ded😵. Using a destroyed obj = UB city👻  
Example 1: variable scope  
```
void main()
{
   int SquareSum = 0;
   int CubeSum = 0;
   for (k: 1..100)
   {
      SquareSum += k * k;
      CubeSum += k * k * k;
   }
   // k is destroyed
   println("sum of square: {SquareSum}");
   println("sum of cubes: {CubeSum}");
   return;
}
// SquareSum and CubeSum are destroyed

```
  
Example 2: shadowing  
```
int score = 97;
println("score in number: {score}");

str score = into_words(score);
println("score in text: {score}");

```
  
Example 3: more shadowing  
```
double dx = pow(2, -25); //too long for test run!
if (final_run == true)
{
   speed_test(my::asin, dx);
   precision_test(my::asin, std::asin, dx);
}
else
{
   double dx = pow(2, -15);
   speed_test(my::asin, dx);
   precision_test(my::asin, std::asin, dx);
}

```
  
**Late init**  
* Every local variable must be initialized before using  
* You can still delay its initialization and prepare the logic to get the right value for it  
  
Example 1: get code  
```
int code;  // immutable but not value😱?
           // dont worry, will be there🫣👇

if (sys.byte_order == Byte_Order::LittleEndian)
{
   code = 4321;
}
elif (sys.byte_order == Byte_Order::BigEndian)
{
   code = 1234;
}
elif (sys.byte_order == Byte_Order::PDPEndian)
{
   code = 3412;
}
else
{
   code = <int>("🫠");
}

```
  
Example 2: inline finding  
```
str s = "Arrr, let's burry our treasure at X";

bool found;

bool mut result = false;
for (char ch: s)
{
   if (ch == 'X')
   {
      result = true;
      break;
   }
}
found = result;

```
  
### Noinit keyword  
* Use when you dont want to initialize a variable  
* Only mutable variables can be marked noinit  
  
Example:  
```
static bool[10] mut working = noinit;
byte[256] mut buffer = noinit;

```
  
### The auto keyword  
* The auto keyword lets the compiler decide the variable type  
Example 1:  
```
auto tittle = "how to cosplay as zombie🧟‍♂️";
// type: wstr

auto pi = 3.141592f;
// type: float

auto dothings() { donothing(); }
// type: void

auto getId = int(SalesData col) { return col.id; };
// type: int(SalesData)

```
  
Example 2: 💣💣🔫  
```
auto var; //❌ God knows what it is😑. Ask him!

double num = <auto>(var); //🤡 bro, what are u casting 'var' into?

```
  
  
  
  
  
## # Primitive types  
* Variable types are devided into: primitives, modified types (using type modifiers), classes, and composites  
* Primitive types are the most fundamental building blocks of all other types (tho, they didnt exist in dino times🦕)  
⚠️ warning: your possible reaction: "??🤨"  
```
Name         Name        Literal      Size
                         Suffix       in Bytes
------------------------------------------------
#0 Void
void         -           -            -
------------------------------------------------
#1 Boolean
bool         -           -            1 byte
------------------------------------------------
#2 Integer
-            i8          -            1 byte
short        i16         s            2 bytes
int          i32         -            4 bytes
long         i64         l            8 bytes
-            i128        _            16 bytes
------------------------------------------------
byte         u8          b            1 byte
ushort       u16         us           2 bytes
uint         u32         u            4 bytes
ulong        u64         ul           8 bytes
-            u128        _            16 bytes
------------------------------------------------
#3 Real
half         f16         h            2 bytes
float        f32         f            4 bytes
double       f64         -            8 bytes
quad         f128        q            16 bytes
------------------------------------------------
#4 Character
char         _           -            1 byte
wchar        _           w            4 bytes
------------------------------------------------
#5 String
str          _           _            24 bytes
wstr         _           w            24 bytes
------------------------------------------------
#6 File
file         -           -            24 bytes
------------------------------------------------

```
  
  
  
  
  
  
## # Comments  
### Single line: //  
Example:  
```
// Helper functions
string toStr(double);
int find(T[] arr, T elem);

int x = 1; //correct
//let x: i32 = 1; wrong language🦀!

```
  
### Mid section: /**/  
Example:  
```
int susfunc(int val, int& ref /* this variable is kinda sus! */, char ch) {...}

```
  
**Warning: #[...]# (glows orange)**  
* Cursed entities ahead ⚠️! Approach with caution in mind and salt in hands 🫴🧂  
Example:  
```
#[ DONT'T TOUCH THIS CODE, NOBODY UNDERSTANDS IT BUT IT RUNS!😡 ]#

#[ Here lies the abyss, don't stare, it snares ]#

#[ Experimental code, only half-tested, DON'T USE! ]#

```
  
  
  
  
  
## # Type: Arrays  
### Static arrays  
* Arrays are composites but all elements are the same type  
* Remember 3 things  
    1. Copying an array = copying all of its elements  
    2. Comparing an array = comparing all of its elements  
    3. Size mismatch = compile time error  
* Use x[::] to get array size  
* Type notation:   
*                           Type[SIZE]*  
Example 1:  
```
str[3] names = { "Kendy", "Fin", "Laura" };

double[5] arr = { 3.14159; 5 }; //repeated 5 times

```
  
Example 2:  
```
int ArrSum(int[10] arr)
{
   int mut sum = 0;
   for (i: arr)
      sum += i;
   return sum;
}

```
  
Example 3:  
```
auto incr = { 1.234f, 2.345f, 3.456f, 4.567f }; 
// Type: float[4]

auto packet = { { '\n', 0.5 }; 32 };
// Type: [char, double][32]

auto rubic = { { { 555u; 3 }; 3 }; 3 };
// Type: uint[3][3][3]

auto mat = { 
   { 1, 2, 3 },
   { 4, 5, 6 },
   { 7, 8, 9 },
   { 10, 11, 12 }
};
// Type: int[3][4]

```
  
Example 4: wrong type  
```
auto dinos = { "Tyrano🦖", "Branchio🦕", "Stego" };
// Type: [wstr, wstr, str]

auto dinos = { "Tyrano🦖", "Branchio🦕", "Stego"w }; //👈 added sufix
// Type: wstr[3]

```
  
Example 5: wrong size  
```
int[3] nums1 = { 77, 88, 99 };  //✅
int[3] nums2 = { 11, 22 };      //❌
int[3] nums3 = { 0; 3 }         //✅

```
  
Example 6: printing  
```
char[6] teacup = { 'T', 'E', 'A', 'C', 'U', 'P' };
println("{teacup}");

//⚠️ Type of elements must be castible into str

```
output:  
```
{ T, E, A, C, U, P }

```
  
Example 7:   
```
int[5] arr = { 21, 22, 23, 24, 25 };
int[5] accum = @init {

   int[5] mut result = { 1; 5 };
   int mut accumulator = 1;

   for (i: 0..4)
   {
      accumulator *= arr[i];
      result[i] = accumulator;
   }

   break @init result;
};

```
  
### Array arithmetics  
* Array supports most arithmentic operators (+ - * / %) logical operators (== != && || !), and bitwise operators (& | ^ ~ >> <<) and their assignment version  
* There is 1 more operation between array and single value called **fused multiply add** (FMA)  
* Array arithmetic operations are optimized with SIMD  
  
Example 1: floating point multiplication  
```
double[8] arr = { 1.5, 2.5, 3.5, 4.5, 5.5, 6.5, 7.5, 8.5 };
double scale = ...;
double[8] res = arr * { scale; 8 };

```
  
Example 2: div assign  
```
int[4] mut a = { 1, 9, 25, 49 }
a /= { 1, 3, 5, 7 };

```
  
Example 3: value - scale combo  
```
half[16] value = @value {
   auto mut res = { 0.0h, 16 };
   for (i: 0..15)
      res[i] = value_func(i);
   break @value res;
};

half[16] propability = @probability {
   auto mut res = { 0.0h, 16 };
   for (i: 0..15)
      res[i] = probability_func(i);
   break @probability res;
};

half mean = @mean {
   half mut res = 0.0h;
   for (val: value * probability)
      res += val;
   break @mean val;
};

```
  
Example 4: fused multipled add  
```
double morm_calc = a * scale + shift;
double fma_calc = a *+ { scale, shift };

```
  
Example 5: horner's method  
```
// expr: x = (((a0 + b0) * a1 + b1) * a2 + b2) ...

double[13] a = {...};
double[13] b = {...};

double mut x = 1;
for (i: 0..12)
   x *+= { a[i], b[i] };

```
  
Example 6: fma-ing arrays  
```
double[8] arr = { 1.5, 2.5, ... };
double[2][8] args = { { 1.8, 2.8 }, { 4.5, 5.5 }, ... };
double[8] res = arr *+ args;

```
  
  
  
  
  
  
## # Types: Composites  
### Composites  
* A composite is a group of several values of different types packed together as one  
* composite fields are numbered quite similarly to arrays  
  
```
Example 1: using a composite
// scattered
str title = "Moby Dick";
str author = "Herman Melville";
int year = 1851;
int pages = 822;

// grouped
[str, str, int, int] mybook = { "Moby Dick", "Herman Melville", 1851, 822 };

```
  
Example 2: field access  
```
[str, str, int, int] mybook = { "Moby Dick", "Herman Melville", 1851, 822 };

println("title: {mybook.(0)}");
println("author: {mybook.(1)}");
println("published year: {mybook.(2)}");
println("number of pages: {mybook.(3)}");

```
  
Example 2: composites  
```
auto person = { "Alice", 17, 166, 45.9f };
// Type: [str, int, int, float]

auto goods = { "Pika T-shirt", 0.03, 0.4 };
// Type: [str, double, double ]

auto user = { "AB8501156421", "Asia", true };
// Type: [str, str, bool]

auto settings = { true, 37, "Hardcore" };
// Type: [bool, int, str]

```
  
Example 3: nested composites  
```
[str, int, [int, str, int]] employee =
{
   "Josh Edop",
   10000,
   { 110812, "Accounting & Finance Department", 2 }
};


```
```
println("name: {employee.(0)}");
println("salary: {employee.(1)}");

```
```
println("department id: {employee.(2).(0)}");
println("department name: {employee.(2).(1)}");
println("working on floor: {employee.(2).(2)}");

```
  
Example 4: single element composite (weird🤨 but allowed)  
```
wstr s = "Im outside the braces";
[wstr] tup = { "Im inside the braces" };

```
  
Example 5:  
```
[int, wstr] beer_cheer = { 123, "Dô!🍻" };

[int, wstr] mut tup = beer_cheer;  //✅
tup = { 321, "Go!🐎" };            //✅
tup = { 12.34, 7749, 22.0f };      //❌Wrong type!

```
  
Example 6: comparison  
```
[int, char, float] tup = { 1, 'A', 1.0 };

println(tup == { 1, 'A', 1.0 });  //✅ (true)
println(tup == { 2, 'B', 2.0 });  //✅ (false)
println(tup == { "ABC", 69 });    //❌Wrong type!

//👆 one-to-one comparison (just like arrays)

```
  
### Named fields  
* Raw composites sometime make u go brainrot:  
"bro whats even the 4th element of the 2nd element of this composite?😵‍💫"  
* Using field name = instant clarity☀️  
  
Example 1: named fields  
```
[str title, str author, int year, int pages] mybook = { "Moby Dick", "Herman Melville", 1851, 822 };

println("title: {mybook.title}");
println("author: {mybook.author}");
println("published year: {mybook.year}");
println("number of pages: {mybook.pages}");

[str name, int salary, [int id, str name, int floor] department] employee =
{
   "Josh Edop",
   10000,
   { 110812, "Accounting & Finance Department", 2 }
};


```
```
println("name: {employee.name}");
println("salary: {employee.salary}");

```
```
println("department id: {employee.department.id}");
println("department name: {employee.department.name}");
println("working on floor: {employee.department.floor}");

```
  
Example 2:  
```
void main()
{
   auto book = { 
      .title = "Uncle Tom's Cabin", 
      .author = "Harriet Beecher Stowe", 
      .year = 1852, 
      .pages = 636 
   };
   // Type: [str title, str author, int year, int pages]

   println("title: {book.title}");
   println("author: {book.author}");
   println("published year: {book.year}");
   println("number of pages: {book.pages}");
}

```
output:  
```
title: Uncle Tom's Cabin
author: Harriet Beecher Stowe
published year: 1852
number of pages: 636

```
  
Example 3:  
```
auto aboutArthur = {
   .personal = { 
      .name = "Arthur", 
      .family_name = "Pendragon", 
      .year = 472
   },
   .parents = { 
      .mother = "Igraine",
      .father = "Uther" 
   },
   .mariage /*🇫🇷*/ = { 
      .maried = true, 
      .spouses = { "Guinevere", "Lionor" } 
      .children = { "Amir", "Lohot", "Gawain", "Mordred" }
   }
};
// Type: 
[
   [str name, str family_name, int year] personal, 
   [str mother, str father] parents, 
   [
      bool maried, 
      [str, str] spouses
      [str, str, str, str] children
   ] mariage, 
]

```
  
### Composites and arrays  
* Static arrays and composites are actually the same animals🦁 👉😱👈  
* Static arays are just composites but elements are all the same type  
* Arrays are just composites but all fields have the same type 👉 runtime index unlocked!  
Example 1:  
```
[wchar, wchar, wchar] emo2 = { '😄', '😙', '🤣' };
wchar[3] emo1 = { '😄', '😙', '🤣' };
//Same✅

```
  
Example 2:  
```
[int, char, char] notArr = { 123, 'A', 'B' };
int elem = notArr[2];  //❌ nah, different size

wstr[3] 👋greet = { "Yo", "Hi", "🦶💪" };
wstr elem = 👋greet[2];  //✅ array

[wstr, wstr, wstr] monk🐵 = { "🙉", "🙈", "🐒" };
wstr elem = monk🐵[2];  //✅ array

auto myEnglish = { 
   .agree = "OK", 
   .disagree = "Nah bro",
   .surprised = "Wtf?"
};
str elem = myEnglish.agree;
str elem = myEnglish.2;
str elem = myEnglish[2];  //✅ array, i guess🙄

```
  
Example 3:  
```
str[3] places = { "Big Ben", "Stonehenge", "Windsor Castle" };

[str, str, str] mut where_to_go = places; //✅assign

where_to_go = { "Waikiki Beach", "Perl Harbor", "Hawaii Vocanoes National Park" }; //✅reassign

```
  
Example 4:  
```
[wchar, wchar] tup = { '🎲', '💣' };

wchar[2] array1 = { '🎲', '💣' };
wchar[2] array2 = { '💣', '🎲' };

println(tup == array1); //✅ (true)
println(tup == array2); //✅ (false)

```
  
Example 5:  
```
double[3] nums = { 3.14159, 2.71828, 3e8 };

println("pi = {nums.0}, e = {nums.1}, c = {nums.2}");

```
output:  
```
pi = 3.14159, e = 2.71828, c = 3.0e8

```
  
### Group assignmemt  
* Gaslight the compiler into thinking that those animals 🐒🦁🐿️ came from the same zoo  
* Works for arrays too!  
Example 1:  
```
[wchar, wchar, wchar] zoo = { '🐒', '🦁', '🐿️' };

wchar mut monkey;
wchar mut lion;
wchar mut squirel;
{ monkey, lion, squirel } = zoo;

```
  
Example 2:  
```
{ str plant, uint days } = { "white lotus", 54 };

println("I've been planting {plant} for {days} days");

```
output:  
```
I've been planting white lotus for 54 days

```
  
Example 3: group assignment for array  
```
str[] books = { "Moby Dick", "Uncle Tom's Cabin", "Pride and Prejudice" };

{ str book1, str book2, str book3 } = books;

print("book 1 is {book1}");
print("book 2 is {book2}");
print("book 3 is {book3}");

```
output:  
```
book 1 is Moby Dick
book 2 is Uncle Tom's Cabin
book 3 is Pride and Prejudice

```
  
Example 4: nested group asignment  
```
[str, [int, str]] student = { "Abby", { 78, "B+" } };

{ str name, { int grade_num, str grade_word } } = student;

```
  
Example 5: factoring out the type (⚠️ always declare, beware shadowing🌚)  
```
wchar { ghost, chic, cac } = { '👻', '🐣', '🌵' };

str { MD, UC, PnP } = books;

[str, double] { _1, _2, _3 } = race_rank(...);

```
  
Example 6:  
```
double[2] Fourier_transform(int freq) {...}

void main()
{
   for (int mut freq; 1 <= freq < pow(10, 6)) {
		   double { cos, sin } = Fourier(freq);
		   //do something...
   }
}

```
  
Example 7:  
```
auto { name, age, height } = { "John Doe", 38, 195 };  
//Me looking at my creation that makes sense but somehow also kinda dont 👀

```
  
Example 8: doge the bombs💣  
```
wchar[] DodgeMe = { '💣', '💣', '🌻', '💣', '🌻' };

//only take the flowers
{ _, _, wchar flw1, _, wchar flw2 } = DodgeMe;

```
  
Example 9:  
```
[int, double, double] bululu = { 1, 1.5, 2.5 };

//factor the type out (only care the used ones)
double { _, x, y } = bululu;

```
  
  
  
  
  
## # Conditional  
### If-else  
* You already knew what it means😐  
* elif == else if  
Example:  a (not so) fun ATM box  
```
print("how much do you wanna withdraw?");

auto amount = read<double>();
if (amount <= user.balance && amount > 0)
{
   user.balance -= amount;
   recordHistory(WITHDRAW, time());
   outputCash();
   println("withdraw successfully!");
}
elif (amount > user.balance)
{
   println("insufficient balance");
   println("wanna take a loan? Dont worry, only 0.01% interest😘 (per day🙄)");
   user.takeloan(1000); //not gonna let u say no🤡
}
else
{
   println("please enter a positive amount(🤨)");  
}

```
  
### Switch-case  
* Compares the test value with every case, from top to bottom (you need == for comparison)  
* You can have overlapping cases and all of them will run  
* Secretly OP in some cases 🫥💪  
Example 1: name of 12 months  
```
int month = read<int>();
switch (month)
{
   case 1, 2, 3:
      println("Spring 🌸");
   case 4:
      println("your lie🤥");
   case 4, 5, 6:
      println("Summer ☀️🌻🍉🍋🍍");
   case 7, 8, 9:
      println("Fall 🍂");
   case 10, 11, 12:
      println("Winter ☃️");
   else:
      println("Nah fam, not 12 month = straight to the void🌀");
}

```
output:  
```
12
Winter ☃️

```
  
Example 2:   
```
println("i see...");
auto dude = read();
switch (dude)
{
   case "Tom", "Harry", "Ben":
      println("Certified dude👊!");
   case "Lauriet":
      println("Not a dude but thats OK 🤙!");
   case "Tom", "Ben":
      println("Team red🍎");
   case "Harry", "Lauriet":
      println("Team blue🐬");
   else:
      println("Who's that🤔? Wanna come?");
}

```
output:  
```
i see...
Lauriet
Not a dude but thats OK 🤙!
Team blue🐬

```
###   
### Conditional expressions  
* Still the same animals but where hats 🧢 and drink🧋  
* Lets you choose values without declaring mut variables  
* Brings up a few traumas for older devs  
###   
### Example 1: price tag  
```
float price = (age < 5? 0.0, age < 15? 2.5, age < 60? 5.0, 0.0);

```
  
Example 2: hat color  
```
int[3] color = if (me == "boy") { 244, 191, 199 } 
               elif (me == "girl") { 152, 203, 0 } 
               else { 193, 95, 60 };

```
  
**Switch expression**  
* Same energy but with switch  
  
Example 3: emoji-izer 👉👉👉  
```
// version 1
wchar emoji = (name; "cool"? '😎', "freaked"? '😱', "fire"? '🔥', "cactus"? '🌵', "skull"? '💀', _? '🫥');

// version 2
wchar emoji = switch (name)
{
   case "cool":    '😎'
   case "freaked": '😱'
   case "fire":    '🔥'
   case "cactus":  '🌵'
   case "skull":   '💀'
   else: '🫥'
};

```
  
  
  
**# Loops**  
### While loop  
* Keep looping while the condition holds  
Example 1: computing e^{x^2}  
```
double useless_exp(int x)
{
   double EULER = 2.718281828459;
   double mut result = 1;
   uint mut i = 0;
   while (i < (x * x))
   {
      result *= EULER;
      i += 1;
   }
   return result;
}

```
  
Example 2: printing a BST 🎄  
```
type BST = [
   [str data, bool available][]+ node,
   uint size
];

void BST_Preorder(BST& tree)
{
   stack<uint> mut st;
   st.push(0); //0 is root

   while (!st.empty())
   {
      //grab a node
      uint curr = st.top();
      st.pop();

      //print it
      print(tree.node[curr]);

      //look at next nodes
      uint left = 2 * curr + 1;
      uint right = 2 * curr + 2;

      if (tree.node[right].available)
         st.push(right);
      if (tree.node[left].available)
         st.push(left);
   }
}

```
  
### For loop  
* Loop for a number of times  
Example 1: looping by range  
```
//printing an array
for (i: 0..(SIZEOF(arr) - 1))
{
   print("{arr[i]}, ");
}

//casting matrix to str
str <str>(matrix& m)
{
   str mut result = "";
   for (i: 0..<m.ROWS) 
   {
      for (j: 0..<m.COLS) 
      {
         result += "{m.data[i][j]} ";
      }
      result += "\n";
   }
   return result;
}

```
  
Example 2: looping over set  
```
//updating info
for (mut guy: UserList) 
{
   if (guy.isAdmin()) 
   {
      guy.updateInfo();
   }
}

//printing an array
for (x: { 10, 20, 30, 40, 50, 60 })
{
   print("x = {x}");
}

```
  
Example 3: ignoring iterator  
```
double mut t = 0.5 * (x + 1);
for (0..<6)
{
   t = 0.5 * (t + x / t);
}

```
  
Example 4: increment but not 1  
```
uint double_fact(uint n)  // double factorial n!!
{
   uint mut result = 1;
   if (n % 2 == 1)
   {
      for (i: 3..n: 2)
         result *= i;
   }
   else
   {
      for (i: 2..n : 2)
         result *= i;
   }
   return result;
}

```
  
Example 5: moveing a string  
```
char[100] mut s = <char[100]>("birds of a feather"c);
int size = 18;

for (i: (size - 1)..5: -1)
{
   s[i] = s[i - 5];
}

```
  
### Do-while loop  
* Loop until the stop condition is met  
* Starts the first loop before checking the condition  
Example: bubble sort  
```
int[] BubbleSort(int mut[] arr)
{
   if (arr[::] < 2)
      return arr;

   bool mut changes_made = false;
   do {
      changes_made = false;
      for (i: 0..(arr[::] - 2))
      {
         if (arr[i + 1] < arr[i])
         {
            { arr[i], arr[i + 1] } = { arr[i + 1], arr[i] };
            changes_made = true;
         }
      }
   } while (changes_made);

   return arr;
}

```
  
**Forever loops and break block**  
* The **loop { ... }** block loops forever  
* The **{ ... }** does nothing but can still return value  
* both can use **break** to return value  
  
Example 1: keep saying  
```
loop {
   if (read() == "I love Monocell")
      break;
}

```
  
Example 2: persistent input  
```
int val;

loop 
{
   str input = trim(read());
   int? input = <int?>(input);

   if (input == true { int got })
   {
      val = got;
      break;
   }

   // continue looping until we got an int
}

```
  
Example 3: still persistent input but  
```
int val = int loop 
{
   str input = trim(read());
   int? input = <int?>(input);

   if (input == true { int got })
   {
      break got;
   }
}

```
  
Example 4: (dont read TOO carefully🤨)  
```
int? opt1 = true{3};
int? opt2 = true{4};

int value = int {
   if (opt1 == false{} || opt2 == false{})
      break 0;
   elif ({ opt1, opt2 } == { true{ int a }, true { int b } })
   {
      if (b == 0)
         break a;
      break a / b;
   }
   else
      break 1;
}

```
  
**Loop labling**  
* Use loop lable to break or continue the exact loop  
  
Example 1: ATM console  
```
void main()
{
   @ATM loop
   {
      println("Wellcome!");
      println("1. Log in");
      println("2. Sign up");
      println("3. exit");

      @MENU while (true)
      {
         str input = read();
         int? input = <int?>(input);

         if (input == false{})
         {
            println("please check your input");
            continue @MENU;
         }
         else unwrap input;

         switch (input)
         {
            case 1: 
               //...

            case 2: 
               //...

            case 3: 
               break @ATM;

            else:
               println("please check your input");
      }
   }
}

```
  
Example 2:  
```
@iter_posts 
for (mut post: user.waiting_posts())
{
   @iter_words 
   for (badword: badword_List)
   {
      if (post.contain(badword))
      {
         post.set(flag::BadWord);
         continue @iter_posts;
      }
   }
   post.approve();
}

```
  
Example 3: not loop but still usable  
```
str bookName = "red rain";
bool foundBook = @Outer bool
{
   for (person: authors)
   {
      for (name: person.publishes())
      {
         if (name == bookName)
         {
            break @Outer true;
         }
      }
   }
   break false;
}

```
  
  
  
  
## # Special controls  
### exit  
* Means "If this situation occur, there's no way to handle it" 👉 Yeet the program entirely  
* Mark the logical branch of a function as unreachable. Compiler trusts you and optimizes as if this branch doesn't exist at all. If you lie to it, 💥💥☠️  
  
Example 1: exit  
```
int dont_know_what_to_return()
{ 
   //❌ compile error
}

int dont_know_what_to_return()
{
   exit;  //✅ explicitly kill the process
}

```
  
Example 2: rule out meaningless inputs  
```
enum Distance_Unit { Mile, Kilometer, Nautical_Mile }

Distance_Unit <Distance_Unit>(int u)
{
   switch (u)
   {
      case 0: 
         return Mile;

      case 1:
         return Kilometer;

      case 2: 
         return Nautical_Mile;

      else: 
         exit;  // You swear to the compiler u <= 2!
    }
}

void main()
{
   auto unit = <Distance_Unit>(7); // 💥💥☠️
}

```
  
Example 3: limmit input range  
```
int fib(int n)
{
   if (n < 0)
      exit;  // "I'll never call fib(-1), i swear!"
   
   if (n == 0)
      return 0;
   if (n == 1)
      return 1;
   return fib(n - 1) + fib(n - 2);
}

void main()
{
   int val = fib(9);   // ✅ Safe, 🏍️💨
   int val = fib(-1);  // YOU LIED! program.🪦()
}

```
  
Example 4: ensure program's critical component  
```
import <file>;

impl std::File.std::Open_Error?
{
   std::File trust(this mut& self)
   {
      if (self == true{ File f })
         return mov f;
      exit;
   }
}

void main()
{
   std::File f = open("home/nothing.txt").trust();
}

```
  
### Pause  
*  Pause the current process. There are two ways to invoke this statement:  
    1. ** ⁠pause <ulong>;**  
Put the current thread to sleep for a specified duration in milliseconds. The OS scheduler will wake it up when time is due.  
    2. **pause;⁠   (TODO)**  
Suspend the current thread indefinitely. Execution resumes only when the thread is explicitly woken by an external signal or event.  
  
Example 1: sleep for a cetain duration  
```
import <io>;

void main()
{
   println("Engine ignition in...");
   
   for (i: 3..0: i -= 1)
   {
      println("{i}...");
      pause 1000;  // ⏳ Pause process for 1000ms
   }
   
   println("🚀 LIFT OFF!");
}

```
  
  
  
  
  
  
## # Type: Optionals  
* An optional of a type can be in value state or empty state  
* 2 values of optionals are true{...} and false{}  
  
Example 1: optionals  
```
int? opt = true{1}; //holds 1 as value
int? opt = false{}; //doesnt hold any value

```
  
Example 2: element search  
```
int? findfirst(str value, str const[]& arr)
{
   for (0 <= i < arr[::])
       if (arr[i] == value)
           return true{i};

   return false{};
}

```
  
* Reverse initialization lets u "catch" the wraped value inside optionals  
* Instead of initializing then assigning, it initializes to extract the existing value  
Example 1: geting the value  
```
int? result = findfirst(3, arr);
if (result == true{ int index })
    println("found the first 3 at {index}");
else
    println("cant find any 3");

=> checks if true/false and extracts value if present

```
  
Example 2: system checking  
```
if (record[1][0] == true{ bool active } && active) 
	  print("system is active");
else
	  print("system is inactive or unknown");

//same as:
if (record[1][0] == true{ true }) 
	  print("system is active");
else
	  print("system is inactive or unknown");

```
  
Example 3: using group assignment  
```
[str, int]? user = true{ { "Sir Davis", 70 } };

if (user == true{ { str name, int age } }) {
	  print("hi {name}, you're {age}!");
}

```
  
Example 4: optional + conditional expression  
```
int? opt = true{32};
int val = if (opt == true{ int x }) x else 999;

```
  
Example 5: changing the true{ value }  
```
wstr mut? maybeName = true{"Tom😾"};
if (opt == true{ wstr mut name }) {
    name = "Jerry🐭";
}
// => maybeName == true{ "Jerry🐭" };

```
  
* An optional may also carry failure details  
Type notation:  
*                 Type.FailType?*  
*    => Type? == Type.void?*  
Example 1:   
```
int.str? opt = true{81};
int.str? opt = false{"some reasons..."};

```
  
Example 3:  
```
float.str? divide(float a, float b)
{
   if (b == 0)
      return false{"dividing by zero"};
   else
      return true{a / b};
}

```
  
Example 4:  
```
void.[int, str]? fetchProfile(uint uid)
{
   if (!isConnected())
      return false{ { 110, "no internet" } };

   if (timeout())
      return false{ { 302, "server timeout" } };

   if (!UserList.has(id))
      return false{ { 500, "can't find user with username: {UserList[id].name}" } };

   makeRequest(UserList[id]);
   return true{};
}

if (fetchProfile(uid = 01125) == false{ { int code, str description } })
{
    print("error {code}: {description}{endl}");
}

```
  
* Optional is nest-able inside any composites  
Example 1: using optional in composites  
```
[int?, int] var = { true{1}, 1 };

[bool, bool?] foo = { false, false{} };

[int, char, float]? whatIsThis = true{ { 1, 'A', 2.25 } };

```
  
Example 2: nested  
```
[str, [int?, int]] info = { "amy", { false{}, 9981 } };

float?[3] = { true{1.25}, false{}, true{0.0} };

int?? optopt = true{ false{} };

```
  
Example 3: 🤯  
```
[int?, [char, str].char?][3] bigData = 
{
	{ true{1}, true{ { 'A', "hi" } } },
	{ false{}, false{ 'K' } },
	{ true{2}, true{ { 'B', "yo" } } }
};

[str.str&[]+?, wstr(int?)??].[uint, ulong.str??]? whatisthis = ...; //😵😵😵

```
  
Example 4: nesting optionals  
```
int.char?.float?.str? BigOpt;

if (BigOpt == true{ int.char?.float? BigOpt1 })
{
    if (BigOpt1 == true{ int.char? BigOpt2 })
    {
        if (BigOpt2 == true{ int BigOpt3 })
        {
           //do things
        }
        elif (BigOpt2 == false { c }) {...}
    }
    elif (BigOpt1 == false{ f }) {...}
}
elif (BigOpt == false{ s }) {...}

```
  
Example 5: decision tree               
```
                int.char?.float?.str?
                        /       \
    true: int.char?.float?   false: str
               /      \
  true: int.char?   false: float
          /    \
  true: int   false: char

```
  
* The unwrap statement creates a reference to the inner value of the optional  
* To use it, first use if or switch-case to handle the error  
Example 1: opening a file  
```
file? status = piff();
if (status == false{})
{
   println("error: file not found");
   exit;
}
else unwrap status;

str character_name = readln(status);
str play_time = readln(status);
str been_offline_for = readln(status);

```
  
Example 2: finding 👻  
```
str[5] strings = ...;
for (str s in strings)
{
   int[]+? pos = s.find("found me!👻");
   if (pos == false{})
   {
      println("no 👻 today☹️");
      continue;
   }
   else unwrap pos;

   for (int i in pos)
   {
      println("found 👻 at {i}!");
   }
}

```
  
Example 3: using unwrap to de-nest nested if-else  
```
int.char?.float?.str? BigOpt;

if (BigOpt == false{ str s }) {...}
else unwrap BigOpt;
// => typeof(BigOpt) == int.char?.float?


if (BigOpt == false{ float f })
else unwrap BigOpt;
// => typeof(BigOpt) == int.char?


if (BigOpt == false{ char ch })
else unwrap BigOpt;
// => typeof(BigOpt) == int

```
  
* Raw IO input always returns a str  
* For data to be used as another type, you have to parse it (somehow IO output and input sections are light years apart from each other🫤)  
* Parsing is actually just casting str into an optional 🤯  
  
Example 1: integer parsing  
```
long? <long?>(str& s)
{
   if (s[::] == 0)
      return false{};

   uint mut i = if (s[0] == '-' || s[0] == '+') 1 else 0;
   long result = 0;
   while (i < s[::])
   {
      if (s[i] < '0' || s[i] > '9')
         return false{};

      result = 10 * result + <long>(s[i] - '0');
      i++;
   }
   result = if (s[0] == '-') -result else result;
   return true{ result };
}

```
  
Example 2: testing out whether an input is a number  
```
void main()
{
   while (true)
   {
      println("-------------------------------");
      long? input = <long?>(read<str>());
      if (input == true{ long value })
      {
         println(color("YES", Color::green) + "(value: {value})");
      }
      else
      {
         println(color("NO", Color::red));
      }
   }
}

```
output:  
```
-----------------------------
abc
NO
-----------------------------
123
YES (value: 123)
-----------------------------
-6sq081
NO
-----------------------------
-86633
YES (value: -86633)

```
  
Example 3: the logic of read<long>()  
```
long read<long>()
{
   while (true)
   {
      long? input = <long?>(read<str>());
      if (input == true{ long value })
         return value;
   }
}

```
  
Example 4: parse result with fail type  
```
enum ParseFail { weirdchar, toobig }
long.ParseFail? <long.ParseFail?>(str& s) {...}

void main()
{
   long.FailType? value =
      <long.ParseFail?>("999999999999999999999");

   switch (value)
   {
      case false{ ParseFail::weirdchar }:
         println("bro, that aint a number😬");
      case false{ ParseFail::toobig }:
         println("this aint god's machine🥱");
      case true{ value }: //extracts and shadows
         println("yeah thats it👈! you got: {value}");
   }
   return;
}

```
  
  
  
  
  
  
## # Console IO  
* Classic **print** and **println** functions for output  
* Classic **read** function for input  
* Use {expr} to insert something into the string  
Example 1: basic output  
```
print("Hello world!");

```
output:  
```
Hello world!

```
  
Example 2: input with read(), insert with {expr}  
```
print("Teacher: What's your name? {endl}");
print("Student: My name is ");
str name = read<str>();
print("Teacher: Hi, little {name}! Welcome to the class! {endl}");

```
output:  
```
Teacher: What's your name?
Student: My name is John
Teacher: Hi little John! Welcome to the class!

```
  
Example 3: {x + y}  
```
print("gimme 2 integers. I'll add them up for u!");
int x = read<int>();
int y = read<int>();
print("Brrrr⚙️... {x} plus {y} equals {x + y}{endl}");

```
output:  
```
gimme 2 integers. I'll add them up for u!
31 22
Brrrr⚙️... 31 plus 22 equals 53

```
  
Example 4: the persistent read() function  
```
print("Grany: how old are you child?");
print("Kid: I'm ");
uint kid_age = read<uint>();
...

```
output:  
```
Grany: how old are you child?
Kid: I'm older than u
Wait what?
Hello
Ohh nooo😰
im sorry, pls stopp😭
7
Grany: you are 7? When i was the same age as u, i was sooo energetic🥰. Let grany give u a candy🍭

```
  
* {expr} is native to string  
* Every expression is evaluated normally and the result will be cast to string using <str>(expr)  
Example:  
```
str message = "this month's interest is {user.interest * 100}%.{endl} Your current ballace is {user.ballance}.{endl} Have a good day!{endl}";

```
  
🌵Tiny note:  
Every function has a type, including print(), read(), and <str>()  
```
Function               Type
-----------------------------------
print                  void(wstr)
read<str>              str(void)
read<int>              int(void)
read<float>            float(void)
<str>(int x)           str(int)
<str>(float x)         str(float)
<str>(complex x)       str(complex)

```
  
* The <str>() function is a kinda special function (u can easily see just from the look of it, cant u?🫤)  
* To be able to print or use {expr} you must define one for the type  
Example: string cast for complex  
```
//cast function
<str>(complex z) {
		return "{z.real} + {z.imag}i";
}

void main()
{
   //printing
   complex z(-1,3);
   println("z = {z}");

   //casting
   str s = <str>(z);
   print(s);

   //inserting to string
   str msg = "conjugate of z: {z.conj()}";
   println(msg);
}

```
output:  
```
z = -1 + 3i
-1 + 3i
conjugate of z: -1 + -3i

```
  
  
  
  
  
## # Functions  
### Function definition  
* 😐🫱🫱 You can define functions anywhere you like, even inside another function   
* No function overloading! (except for operators 🫲🙂‍↔️🫱)  
Example:  
```
//Define here👇
void sayhi(wstr who) 
{
   println("hi, {who}!");
}

void main() 
{
   //Or define here👇
   void sayhello(wstr who) 
   {
      println("hello, {who}!");
   }

   sayhi("Sir Meme Loỏd🤡!");
   sayhello("Sir Gay lörd✊!");
   saywha();
   return;
   
   //Or define even here👇
   void saywha() 
   {
      println("wha👻!");
   }
}

```
output:  
```
hi, Sir Meme Loỏd🤡!
hello, Sir Gay lörd✊!
wha👻!

```
  
### Function scopes  
* Global functions and local functions must be defined in order  
* Functions in the same class or module can be defined freely, no order needed  
Example 1: global  
✅ normal order  
```
double pi()
{
   return 3.14159;
}

double pi_square()
{
   return pi() * pi();
}

main() {...}

```
  
❌ upsidedown 🐧  
```
double pi_square()
{
   return pi() * pi();  //whats even "pi()" 🤨?
}

double pi()
{
   return 3.14159;
}

main() {...}

```
  
Example 2: local   
```
void main()
{
   void helper1() {...}

   helper1();  //✅
   helper2();  //❌ helper2 is helpless right'ere🐒

   void helper2() {...}
}

```
  
Example 3: shadowing  
```
void who_are_u()
{
   println("Im Yu!👊👊");
}

void main()
{
   who_are_u();

   for (1 <= i <= 2)
   {
      void who_are_u()
      {
         println("Im Mi!👋");
      }

      who_are_u();
   }
}

```
output:  
```
Im Yu!👊👊
Im Mi!👋
Im Mi!👋

```
  
Example 4: module scope  
```
module jump
{
   pub void printA(uint n) 
   {
      println("its A!");
      if (n == 0) {
         return;
      }
      printB(n - 1);
   }

   pub void printB(uint n) 
   {
      println("its B!");
      if (n == 0) {
         return;
      }
      printB(n - 1);
   }
}

void main() 
{
   jump::printA(5);
}

```
output:  
```
its A!
its B!
its A!
its B!
its A!
its B!

```
  
Example 5: no shadowing = defined once  
```
class ship
{
   void honk() { println("brrroooo!💨"); }
   void honk() { println("beeepp!"); }  //❌🤨
}

```
  
### Labeling arguments  
* Labeling arguments with the parameter name makes your code more readable (cuz function calls sometimes looks like secreet codes😵‍💫)  
* Arguments still in the same order (dont shuffle, it aint Poker🤡)   
* That gonna save me time writing this damn compiler☠️☠️  
Example 1: ⚠️pure chaos ☢️ for u and ur teamates  
```
void main() {
   double LandingPos = shoot(13, 20, -0.832, true); // It compiles but 🤮🤮🤮🤮

   generate_map(889041, true, true, false, true); // Bro, what is bool in the fourth place was for again🥴?
   return;
}

```
  
Example 2:  
```
void main()
{
   double landingPos = distance(v = 13, h = 20, angle = -0.832, air = true);

   generate_map(seed = 889041, 
                Island = true, 
                TerrainsOverlap = true, 
                InfiniteCaves = false, 
                ArgressiveMonster = true);
   return;
}

```
  
Example 3: 🧨🧨  
```
generate_map(889041, true, true, 
             InfiniteCaves = false, 
             ArgressiveMonster = true);

// bro, whatya doin? Make it readable, not cursed💀
// 👉 named or unamed, choose ONE!

```
  
**Default arguments**  
* Give a parameter a default argument  
* Use _ in place of default args to skip it  
* This exists for some reason🤔  
Example 1:   
```
void makeLayer(color background = transparent) {...}

void main()
{
   makeLayer(white);
   makeLayer(_); //transparent
}

```
** **  
Example 2:  
```
double log(double base = 2.718, double x) {...}
double Harmonic(float x1 = 0, float x2 = 0, float x3 = 0) {...}

void main()
{
   log(_, 5);       
   // log(2.718, 5)

   Harmonic(x1 = _, x2 = _, x3 = 7); 
   // GeomMean(0, 0, 7)
}

```
  
Example 3:  
```
void three_numbers(int a = 77, int b = 88, int c = 99) {...}

void main()
{
   three_numbers(111, 111, _); //✅
   three_numbers(111, 111);    //✅

   three_numbers(222, _, _);   //✅
   three_numbers(222);         //✅

   three_numbers(_, _, _);     //✅
   three_numbers();            //✅
}

```
  
Example 4:  
```
void dumb(bool NoDefault) {...}

void main()
{
   dumb(true);  //✅
   dumb(false); //✅

   dumb(_); //💥
   dumb();  //💥💥🌋
}

```
  
**Lambdas (namesless functions)**  
* Yes, functions without a name, literally  
* Use **thisfn** to refer to the current function (any function can use it, i just didnt tell u🙄, my bad😇)  
Example 1: whuts lambda🤔?  
```
void main()
{
   int(int) f = int(int x) {
      if (x > 0)
         return 2 * x;
      else
         return x * x;
   };
   
   for (int mut i; -5 <= i <= 5)
   {
      println("i = {i}, f(i) = {f(i)}");
   }
   return;
}

```
output:  
```
i = -5, f(i) = 25
i = -4, f(i) = 16
i = -3, f(i) = 9
i = -2, f(i) = 4
i = -1, f(i) = 1
i = 0, f(i) = 0
i = 1, f(i) = 2
i = 2, f(i) = 4
i = 3, f(i) = 6
i = 4, f(i) = 8
i = 5, f(i) = 10

```
  
Example 2: auto for return type  
```
double result = integral(
   auto(double x) {
      return asin(cos(x) / 2.0) * cos(11.0 * x);
   }, 0, PI
);

```
  
Example 3: more realistic use case  
```
while (true)
{
   double[1000] batch = sample_data(
   source = double?()
   {
      str path = "~/myProg/.sensor";
      return parse<double>(read<str>(pikk(path)); 
   },
   filter = double(double x)
   {
      if (x > 20e3)
         return 20e3;
      else if (x < 3.18e-7)
         return 0;
      else
         return x;
   });

   //process data...
}

```
  
### Example 4: lambdas can be called immediately  
```
void main()
{
   int sum = int(int a, int b) { return a + b; }(1, 2); 
   //✅

   int dub = int(int a) { return a + a; }(3);
   //✅

   int pow3 = int namedfunc(int a) { return a * a * a; }(5);
   //✅ named, but also OK👌

   println("1 + 2 = {sum}");
   println("3 + 3 = {dub}");
   println("5^3 = {pow3}");
   return;
}

```
output:  
```
1 + 2 = 3
3 + 3 = 6
5^3 = 125

```
  
Example 5: inline recusion logic  
```
void main()
{
   println("gimme an unsigned integer n");

   uint n = read<uint>();
   uint fibn = uint(uint n) {
      if (n <= 1) return 1
      return thisfn(n - 1) + thisfn(n - 2);
   }(n);
   
   println("the nth fibonacci number is {fibn}");
   return;
}

```
output:  
```
gimme an unsigned integer n
8
the nth fibonacci number is 21

```
  
  
  
  
  
## # Types: Function type  
### Basic function type  
* Every function is a value of its type  
* The function type encodes the types of the parameters and the return type  
Type notation:  
*           ret(param1, param2, ...)*  
  
Example 1: function types  
```
int findToken(str s, str& token);
=> int(str, str&)

Vec<str> parse(str s);
=> Vec<str>(str)

void print_statistics(Character&);
=> void(Character&)

Complex secretNum();
=> Complex(void)

str ANSIcolor(str text, Color c);
=> str(str, Color)

void sayHello();
=> void(void)

```
  
Note:  
```
int foo() {...}
int foo(void) {...}
// 👆 These are the same

```
  
Example 2: function variables  
```
double sqrt(double x) {...}
double exp(double x) {...}
double pow(double x, double p) {...}

double(double) f1 = sqrt;
double(double) mut f2 = exp;
double(double, double) mut f3 = pow;

f1 = sqrt; //❌ f1 is immutable
f2 = sqrt; //✅ OK!
f3 = sqrt; //❌ wrong type!

```
  
Example 3: printing something  
```
void main()
{
   println("whats inside sqrt?🤔");
   println("sqrt = {sqrt}"); //❌ Nah bro, nothing to print here👻
   return;
}

```
  
So... can we print anything 🤔?  
```
void main()
{
   void DrawCircle() {...}
   void(void) funcvar = DrawCircle;

   //✅ call address:
   ulong addr1 = calladdr(DrawCircle);
   ulong addr2 = calladdr(func);
   void(void)* addr3 = &funcvar;

   println("calladdr DrawCircle: {/h; addr1}");
   println("calladdr funcvar:    {/h; addr2}");
   println("address funcvar:     {/h; &funcvar}");
   return;
}

```
output:  
```
calladdr DrawCircle: 0000055dcb664515
calladdr funcvar:    0000055dcb664515
address funcvar:     000007ffec9b693b

```
  
🪴 Note:  
- Call addresses and pointers are both addresses  
- Only pointers are addresses of typed data  
- Raw addresses are represented by a 64-bit unsigned integer (on 64-bit systems)  
  
Example 5: pointers  
```
int x = 5;
str s = "ascii is art";
void(void) f = FigletTree;

str* ptr2 = &"Hello"; 
//❌ "hello" is not a variable

void(void)* ptr3 = &DrawCircle; 
//❌ same, DrawCircle is a literal

int* ptr1 = &1; 
//❌ whatya doin? Thats illegal in 47 counties💀

int* ptr1 = &x;        //✅
str* ptr2 = &s;        //✅
void(void)* ptr3 = &f; //✅

```
  
* A function type can be used as return type and parameter  
* Function type parameters can be named (just like named composite fields  
Example 1: function as parameter  
```
double[10] map(double[10] arr, double(double) f)
{
   double mut[10] out;
   for (0 <= i < 10)
      out[i] = f(arr[i]);
   return out;
}
// Type: double[10](double[10], double(double))

void main()
{
   double[10] arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

   double[10] squared_elem = map(arr, double(double x) { return x * x; });

   println("squared elems: {squared_elem}");
   return;
}

```
output:  
```
squared elems: { 1, 4, 9, 16, 25, 36, 49, 64, 81, 100 }

```
  
Example 2: function as return type  
```
double(double) myfunc(str name)
{
   switch (name)
   {
      case "exp":
         return exp;
      case "log":
         return log;
      case "sqrt":
         return sqrt;
      else:
         return double (double) { return 7749; };
   }
}
// type: double(double)(str)

println("exp(10) = {myfunc("exp")(10)}");

```
output:  
```
exp(10) = 22026.5

```
  
Example 3: function returning function returning function returing...  
```
[int, char](str) func {...}
=> func(hello) -> [int, char]

[int, char](str)(long) func {...}
=> func(-84e20) -> [int, char](str)
=> func(-84e20)("hello") -> [int, char]

[int, char](str)(long)(wchar) func {...}
=> func('🍄') -> [int, char](str)(long)
=> func('🍄')(-84e20) -> [int, char](str)
=> func('🍄')(-84e20)("hello") -> [int, char]

[int, char](str)(long)(wchar)(void) func {...}
=> func() -> [int, char](str)(long)(wchar)
=> func()('🍄') -> [int, char](str)(long)
=> func()('🍄')(-84e20) -> [int, char](str)
=> func()('🍄')(-84e20)("hello") -> [int, char]

```
  
Example 4: auto return type = easy💥  
```
auto wtf_is_this()
{
   return thisfn;  //and thinks✨ its genius🧠
}

```
  
🎃 Note: whats happening 🤨?  
```
function takes no param
=> Type = return_type(void)

function return itself
=> return_type = return_type(void)

expanding return_type...
=> return_type = (return_type(void))(void)
               = return_type(void)(void)

expanding return type...
=> return_type = (return_type(void))(void)(void)
               = return_type(void)(void)(void)

...

STILL exapanding return type... (💀)
=> return_type = return_type(void)...(void)(void)

👉 Infinite recursion. Compiler eats its own tail and becomes Ouroboros🐍 😬😬

```
  
Example 5: spoting 🐍  
```
auto snake()
{
   return thisfn;
   //❌🐍
}

auto eatme()
{
   return { 123, "hello", thisfn };
   //❌still 🐍, just in a box
}

auto nope()
{
   return func(thisfn);
   //❌😑 nah bro, nice try🐍
}

```
  
* Functions can be casted (to void(...) only!) 🤯  
* Do this if you only need the function's side effect while ignoring its return value  
Example:  
```
void f1(int x)
{...}

str[]+ f2(int x)
{...}

[int, char] f3(int x)
{...}

[int, char, quad&] f4(int x, int y)
{...}

<void(int)>(f1)(6636);
<void(int)>(f2)(7749);
<void(int)>(f3)(8864);
<void(int)>(f4)(9981); //💥💀 arity mismatch

```
  
### Function type with capture  
* A function definition can use local **runtime** data. Depening on that data, the function behavior may be change over time or between runs  
* The version of that local object inside the function is called the "capture" of that object  
Type notation:  
*            ret(param1, param2, ... | capture1, capture2, ...)*  
*=> ret(param1, param2, ...) == ret(param1, pram2, ... | void)*  
  
🍄‍🟫 Note:   
The order of the capture list doesnt matter  
Example 1: capture by value  
```
for (0 <= n <= 100)
{
   bool nIsPrime(void | n)
   {
      for (2 <= j <= <long>(sqrt(n)))
      {
         if (n % j == 0)
            return false;
      }
      return true;
   }

   if (nIsPrime())
   {
      println("{n}");
   }
}

```
  
Example 2: capture by reference  
```
int mut count = 0;
void countup(void | mut count) { count += 1; }
// Type: void(void | int mut&)

void countdown(void | mut count) { count -= 1; }
// Type: void(void | int mut&)

countup();    //count = 1
countup();    //count = 2
countup();    //count = 3
countdown();  //count = 2
countup();    //count = 3
countdown();  //count = 2
countdown();  //count = 1

```
  
Example 3: capture list shuffle  
```
double(double | double(double), int) Chebychev_integrand(double(double f, int k)
{
   return double(double x | f, k)
   {
      if (k == 0
         return (1.0/PI) * f(cos(x));
      else
         return (2.0/PI) * f(cos(x)) * cos(k * x);
   };
}
// Type: double(double | double(double), int)(double(double), int)


double(double | int, double(double)) f = Chebychev_integrand(asin, 11); 
//✅ shuffling doesnt change meaning

```
  
Example 4: auto for captured types  
```
double integral(double(double | auto) f, double lower, double upper) {...}

//printing a table of chebychev coef for asin
for (1 <= k <= 31; 2)
{
   double res = integral(Chebychev_integrand(asin, k), 0, PI);
   println("{res},");
}

```
  
Example 5: making a lil sine wave  
```
double(double | auto) sinewave(double amp, double freq)
{
   return double(double x | amp, freq)
   {
      return amp * sin(_2PI * freq * x);
   };
}

void draw_graph(double(double | auto) wavefunc) {...}

draw_grap(wavefunc = sinewave(2.5, sqrt(2)));

```
  
### Default capture  
* The _ means capture variables   
* Mutable variables are captures by mutable reference  
* Imutable variables are captured by immutable reference  
Example 1: using _ for capture list  
```
double mut x ...;
int n ...;

double sqrt_x = auto(void | _)
// Type: double(void | double mut&, int&)
{
   double mut sum = 0;
   for (1 <= i <= n)
      sum += sin(i * x)
   return sum; //trust me, this approximates sqrt🙄
}();

```
  
Example 2: a simple way to use inline recursion  
```
int CHOICES = 9;

int choice = auto(void | _)
// Type: int(void | int&)
{
   println("MENU:");
   println("1. Log in");
   println("2. Sign up");
   //...
   println("8. Change language");
   println("9. Quit");

   str input = read<str>();
   int? input = <int?>(input);

   if (input == false{})
   {
      println("error: please enter number 1-{CHOICES}");
      return thisfn();
   }
   unwrap input;

   if (input < 1 || input > CHOICES)
   {
      println("error: please enter number 1-{CHOICES}");
      return thisfn();
   }
   else
      return input;
}();

// Handle choices...

```
  
🪷 4 types of capture:  
```
Type             Example
-------------------------------------------
by copy          int(void | var)

by immutable     int(void | immut var)
reference

by mutable       int(void | mut var)
reference

by move          int(void | mov var)

```
  
  
  
  
## # Type: Struct  
### Data fields  
* Structs let you group related data together like composites  
* Structs are exclusive. Different structs = Different types (even if fields are the same)  
Example 1: class data  
```
struct User {
   str username,      // 👈 data fields
   str email,
   int days_active,
   bool online,
}

void main()
{
   User user1 = User { 
      "Samurai", 
      "Samuel@Walex.com", 
      28, 
      false 
   };

   User user2 = User {
      username = "MikoIsCute",
      email = "Miko.luvluv@greenmail.com",
      days_active = 0,
      online = true
   };
}

```
  
Example 2: each class is a different types  
```
struct A { int x, str s }
struct B { int x, str s }

void main()
{
   A var0 = A { 88, "hi" }; //✅
   A var1 = B { 88, "hi" }; //❌ B
   A var2 = { 88, "hi" };   //❌ [int, str]
}

```
  
Example 3: field default value  
```
class Button {
   byte[3] color = Color::red,  // red button
   float radius = 50,           // 50 pixels
   str label = "",              // no label
}

void main()
{
   auto b = Button { _, 27, "Nuke" };       //✅
   auto b = _;
}

```
  
Example 4:  
```
class First {
   str YesDefault = "hello";
   int NoDefault;
}

void main()
{
   auto var = First { _, 7749 };  //✅
   auto var = _;                  //❌ 💣😶

   //👉 need default value for all fields
}

```
  
### Member functions  
* Functions defined within the class belong to the class's scope (you can also define other things😉)  
* If the first parameter is both the same type as the class and named "self", magic✨ happens  
  
  
Example 1: complex numbers (no magic here yet ☹️,  just math 🧠🧮)  
```
struct Complex {
   double re,
   double im
}

/////////////////////////////////
/////// Individual declare //////
/////////////////////////////////

Complex Complex::def()
{
   return Complex { 0.0, 0.0 };
}

Complex Complex::cord(double re, double im)
{
   return Complex { re, im };
}

/////////////////////////////////
/////// Grouped declare /////////
/////////////////////////////////

impl Complex
{
   Complex pol(double r, double th)
   {
      return Complex {
         re = r * cos(th),
         im = r * sin(th)
      };
   }

   double mod(Complex& self)  👈 for magic✨
   {
      return sqrt(self.re * self.re + self.im * self.im);
   }

   void rot(Complex mut& self, double th)
   {
      self.re = self.re * cos(th) - self.im * sin(th);
      self.im = self.re * sin(th) + self.im * cos(th);
   }
}

///////////////////////////
///////////////////////////
///////////////////////////

str <str>(Complex& z)
{
   return "{z.re} + {z.im}i";
}

void main()
{
   Complex z0 = Complex {};
   Complex z1 = Complex::cord(-1, 2);
   Complex z2 = Complex::pol(3, -PI / 4);

   println("z0 = {z0}");
   println("z1 = {z1}");
   println("z2 = {z2}\n");
}

```
output:  
```
z0 = 0 + 0i
z1 = -1 + 2i
z2 = 2.12132 + -2.12132i

```
  
Example 2: magic happens here ✨🖐️  
```
void main()
{
   ...
 
   //Normie func calls😶
   println("|z0| = { Complex::mod(z0) }");
   println("|z1| = { Complex::mod(z1) }");
   println("|z2| = { Complex::mod(z2) }\n");

   //Magic func calls🪄✨
   println("|z0| = { z0.mod() }");
   println("|z1| = { z1.mod() }");
   println("|z2| = { z2.mod() }\n");
}

```
  
Example 4: the paramater self  
```
struct Lala {}

void Lala::tryme(Lala& self);   //✅
void Lala::trymenow(int self);  //❌ self cant have type int
}

void main()
{
   auto lala = Lala {};
   lala.tryme();     //✅
   123.trymenow();   //❌ compiler.😬💥();
}

```
  
Example 5: Alias "this" (only in scope)  
```
enum Weapon
{
   wooden_stick,
   red_brick,
   two_fingers
}

struct Character
{
   uint HP,
   Weapon weapon,
   Shield? shield 
}

impl Character
{
   // type this = Character; 👈 FREE type alias🤑

   this spawn()
   {
      return this { 
         HP = 800,
         weapon = wooden_stick,
         shield = false{}
      };
   }

   void normal_atk(this& self, Goblin mut& enemy)
   {
      println("hero: bonk!");
      enemy.takeDMG(weapon.DMG);
   }

   ...
}

```
  
Example 7: automatic scope resolution  
```
struct Character {...}
struct Goblin {...}

void main()
{
   Goblin mut horde = spawn_horde();  
   // 👉 Goblin::spawn_horde()

   Goblin mut wizard = spawn_wizard();  
   // 👉 Goblin::spawn_wizard()

   Character mut hero = spawn();
   // 👉 Character::spawn()

   Goblin mut warior = wizard.summon();

   hero.normal_atk(horde);
   hero.normal_atk(horde);
   hero.normal_atk(horde);
   hero.ultimate_IAmAtomic({ horde, wizard, warior });
}

```
output:  
```
🧌🧙‍♂️!!
Ya Ya! Aka 🗡️💥😈!

hero: bonk!
hero: bonk!
hero: bonk!
hero: 👆👆👆 💥💥🔥☢️🔥☢️🔥☢️

😵😵😵

```
  
Example 8: deduction failed successfully  
```
struct Stick {}
...

void main()
{
   Stick t = Stick::make();  //✅
   Stick t = make();         //✅ Stick::make()

   Stick r = auto::make();   //❌ 🐒?
   auto r = auto::make();    //❌ ??🙉
}

```
  
Example 5: under the hôd🤫  
```
struct A {
   double a,
   short b,
   int c,
   bool d,
   char* e
}

void main()
{
   println("
   size: { sizeof<A> }
   offsets:
      a: { <ulong>(A::$a) }
      b: { <ulong>(A::$b) }
      c: { <ulong>(A::$c) }
      d: { <ulong>(A::$d) }
      e: { <ulong>(A::$e) }
   "R);
}

```
output:  
```
size: 24
offsets:
   a: 0
   b: 20
   c: 16
   d: 22
   e: 8

```
  
🌵 What's happening:  
```
# Originally
fields: double, short, int c, bool d, char*
sizes: 8 bytes, 2 bytes, 4 bytes, 1 byte, 8 bytes

# After sort
fields: double, char*, int, short, bool
offset: [8 bytes] | [8 bytes] | [4 bytes][2 bytes][1 byte][padding (1 byte)]
align: 8 bytes

```
  
  
  
  
  
## # Type: Enums  
### Enum values  
* Enums are user defined types specialized for finite and case-wise data  
* The number and meaning the enum values depends on how you define it  
* Automatic scope resolution enabled  
Example 1: enum values  
```
enum connection { 
   wired, 
   wifi, 
   disconnected, 
   error
}

void main()
{
   connection my_pc = wired;
   connection my_laptop = wifi;
   connection potato🥔_pc = disconnected;
}

```
  
Example 2: printing enum values  
```
void main()
{
   //... (example 1)

   println("status:");
   println("my pc:     {my_pc}");
   println("my laptop: {my_laptop}");
   println("potato pc: {potato🥔_pc}");
}

```
output:  
```
status:
my pc:     wired
my laptop: wifi
potato pc: disconnected

```
  
Example 3: auto  
```
enum connection { 
   wired, 
   wifi, 
   disconnected, 
   error
}

enum print_error {
   no_ink,
   no_paper,
   disconnected,
}

auto var = wifi;         //❌ ambiguous!😶‍🌫️
auto var = no_paper;     //❌
auto var = disconnected; //❌

auto var = connection::disconnected;  //✅
auto var = connection::disconnected;  //✅
auto var = print_error::no_paper;     //✅
auto var = print_error::disconnected; //✅

```
  
Example 4: enum + switch = 💪  
```
enum Direction { 
   North, 
   East, 
   South, 
   West 
}

struct Walker {
   int x,
   int y
}

void Walker::walk(this mut& self, Direction dir, int steps)
{
   switch (dir)
   {
      case North:
         self.y += steps;

      case East:
         self.x += steps;

      case South:
         self.y -= steps;

      case West:
         self.x -= steps;
   }
}

```
###   
### Enum inner data  
* Enum values can have inner data  
* different variants carry different data only related to itself  
* size of type == size of biggest value => can fit all values  
Example 1: inner data  
```
enum Shape
{
   dot,
   circle(float),
   triangle(float, float),
   rectangle(float, float)
}

Shape x = dot;
Shape tr = triangle(2.0, 3.6);
Shape re = rectangle(1.2, 4.2);

```
  
Example 2: area for each shape  
```
enum shape {
   //... (example 1)
}

float area(shape shp)
{
   if (shp == dot)
   {
      return 0;
   }
   elif (shp == circle(float radius))
   {
      return PI * radius * radius;
   }
   elif (shp == triangle(float height, float base))
   {
      return 1.0 / 2.0 * height * base;
   }
   elif (shp == rectangle(float height, float width))
   {
      return height * width;
   }
   //else 👈 already covered all variants
}

```
  
Example 3: switch = less boilerplate  
```
enum shape {
   //... (example 1)
}

float area(shape shp)
{
   return switch(shp)
   {
      case dot:
         0
      case circle(float radius):
         PI * radius * radius
      case triangle(float height, float base):
         0.5 * height * base
      case rectangle(float height, float width):
         height * width
   };
}

```
  
Example 4: named inner fields  
```
enum Action 
{
   Quit,
   Move(uint line),
   Write(str text),
   WriteColor(str text, byte[3] color)
}

struct TextEditor {
   file editor,
   uint line
}

void TextEditor::act(TextEditor mut& self, Action act)
{
   switch (act)
   {
      case Quit:
         println("Bye");

      case Move(uint line):
         self.line = line;

      // 👇 ignore naming
      case Write(_):
         print(act.text, editor);

      // 👇 ignore only second field
      case WriteColor(str text, _):
         file_print(color(text, cl), editor);
   }
}

```
  
Example 5: the right fields in the right branch  
```
enum Action
{
   Quit,
   Move(uint line),
   Write(str text),
   WriteColor(str text, byte[3] color)
}

void execute(Action a)
{
   if (a == Move(_))
   {
      move_to(a.line);
   }
   elif (a == Write(_))
   {
      uint ln = a.line;    // ❌ No a.line
      write_text(a.text);  // ✅
   }
   elif (a == WriteColor(str txt, byte[3] cl))
   {
      write_text(a.text);          // ❌ No a.text
      color(txt, a.color));        // ❌ No a.color
      write_text(color(txt, cl));  // ✅
   }
}

```
  
Example 6:  rule: name all or none  
```
enum Output
{
   compile(Format),             //🥶  no name
   transpile(Lang),             //🥶  no name
   help(int id, bool verbose)   //🥵  named
}

// 👉 No, it ugly, me no like it👹

```
  
Example 7: multi-level enum  
```
enum Weapon
{
   Sword,
   Spear(float length),
   Throw {
      Bomb(float radius),
      Shiiii(bool stink)
   },
   Gun {
      common: int mag
      Glock(bool dual),
      AK47(_)
   }
}

Weapon left_hand = Spear(1.95);
Weapon right_hand = Gun(31)::AK47;
Weapon middle_hand = Gun(17)::Glock(true); //if there is a middle hand🖐️😐

```
  
Example 8:  
```
enum Weapon {
   //... (example 4)
}

[wstr, float] calc_dmg(float base_dmg, Weapon weapon)
{
   return switch (weapon)
   {
      case Sword:
         { "🪓", base_dmg * 2 }

      case Spear(float length):
         { "Poke 🪛", base_dmg * length * 1.5 }

      case Throw::Bomb(float radius):
         { "BOOOOOOOM! 💥🔥🔥", base_dmg * radius * radius * PI }

      case Throw::Shiiii(bool stink):
         { "💩 🤮🤮🤮💀", if (stink) 999999 else 1 }

   //for both glock and AK47
      case Gun(int mag):
         { "Pew! Pew! 🔫", base_dmg * mag }
   };
}

```
  
Example 9: ignore inner data  
```
enum Token
{
   word(str)
   number(float),
   bracket
}

bool count_non_bracket(Token[]+ arr)
{
   int mut count = 0;
   for (Token tok in arr)
   {
      if (tok == word || tok == number)
         count++;
   }
   return count;
}

```
  
**Enum encodes**  
* Enum can encode data of the same types. Encoded data is stored in a hidden immutable static array  
* The values must be compile time constants  
* You can only choose 1 out of inner data and value encoding  
  
Example 1:  
```
enum Color: byte[3]
{
   Red:   { 255, 0, 0 },
   Green: { 0, 255, 0 },
   Blue:  { 0, 0, 255 }
   ...
}

void main()
{
   renderText(
      text = "hello world!",
      font = Times_New_Roman,
      color = [Color::Blue]  // gets { 0, 0, 255 }
   );
}

```
  
Example 2:  
```
enum Ops: int(int, int)
{
   Add: int(int a, int b) { return a + b; },
   Sub: int(int a, int b) { return a - b; },
   Mul: int(int a, int b) { return a * b; },
   Div: int(int a, int b) { return a / b; }
}

int rand_op(int a, int b)
{
   return [<!!Ops>((a + b) % 4)](a, b);
}

```
  
### Enum members  
* Classes but with enumerated values  
* Define values, not fields  
Example 1:  
```
enum class UIComp
{
   Blank(byte[3] color),
   Text(byte[3] color, str text),
   Image(file link),
   Button(byte[3] color, void(Action) onclick),
   Slider(byte[3] color, bool real, float[2] range),
   AutoRun(void(void) autorun)
}

void UIComp::change_color(this mut& self, byte[3] color)
{
   switch (self)
   {
      case Blank(byte[3] mut c):
         c = color;

      case Text(byte[3] mut c, _):
         c = color;

      case Button(byte[3] mut c, _):
         c = color;

      case Slider(byte[3] mut c, _, _):
         c = color;
   }
}

```
  
Example 2: 🌻 vs 🧟  
```
enum class PlantProj
{
   pea(PeaType),
   melon(MelonType),
   coconut(CoconutType),
   spike,
}

impl PlantProj
{
   enum PeaType { normal, snow, fire, plasma }
   enum MelonType { normal, winter }
   enum CoconutType { normal, giant }
   enum Area { single, lane, _3x3 }

   Area area(this& self)
   {
      switch (self)
      {
         case pea: 
            return single;

         case melon, coconut:
            return _3x3;

         case spike: 
            return lane;
      }
   }
}

```
  
  
  
  
## # Modules  
* Modules scope up symbols including function, struct, enum, and static variabls (Compiler screams if u scope up main()!😖)  
* There are 3 types of visibility policies in a module:  
    - **pub** (public) means the symbol can be used outside the module but still considered inside the module's scope  
    - **priv** (private) means the symbol can not be used outside the module  
    - **export** means the symbol is pushed to the outside scope  
* By default, everything inside a module is **priv**. The outside cant see whats inside of it  
  
### Public and private  
* Can be used as field or mark on each symbol  
  
Example 1: modules  
```
module A
{
   int helper() {...}
   pub int func() {...}
}

void main()
{
   println("func() = { A::func() }");
   //✅ A::func is visible

   println("helper() = { A::helper() }");
   //❌ A::helper is hidden
}

```
  
Example 2: policy fields  
```
module A {
priv:

   int helper() {...}  // priv

pub:

   int func() {...}    // pub
}

```
  
Example 3: policy override inside field  
```
module A {
priv:

   int helper() {...}      // priv
   pub void ovrA() {...}   // pub

pub:

   int func() {...}        // pub
   priv void ovrB() {...}  // priv
}

```
  
Example 4: policy consistency  
```
module stat
{
   // declaration
   priv double mean(double[] arr);
   pub double variance(double[] arr);
 
   // definition
   pub double mean(double[] arr) {...}      //❌
   pub double variance(double[] arr) {...}  //✅
}

```
  
### User-defined types  
* Consists of policy for initializer and policy for field access  
* By default, struct field and enum variants follow its type's access policy  
  
Example 1: public struct  
```
module contact
{
   pub struct Account {
      int id,
      str name,
      str nickname
   }
   // => id, name, nickname are public
}

void main()
{
   contact::Account {...};   //✅

   person.nickname;   //✅
   person.name;       //✅
}

```
  
Example 2: one priv field 👉 no initializer for u  
```
module contact
{
   pub struct Account {
      pub int id,
      pub str name,
      priv str nickname
   }

   void inside() 
   {
      Account {...};         //✅ ok
   }
}

void main()
{
   contact::Account {...};   //❌ priv nickname🛡️

   person.nickname;   //❌
   person.name;       //✅
}

```
  
Example 3: public enum  
```
module sci
{
   pub enum Material { 
      Iron, 
      Paper, 
      Banana_Leaf 
   }
   // => Iron, Paper, Banana_Leaf are public
}

void main()
{
   sci::Material var = sci::Material::Banana_Leaf;
}

```
  
Example 4: private enum variants  
```
module mod
{
   pub struct S {
      priv int field
   }

   pub enum E { 
      priv Value
   }
}

void main()
{
   auto x = mod::S { 123 };  //❌ blocked initializer 🫷🥬
   auto y = mod::E::value;   //❌ blocked initializer 🫷🍖

   // NOTE: enum variants are initializers
}

```
  
Example 5: private enums and structs  
```
module sci
{
   priv enum Material { 
      Iron, 
      Paper, 
      Banana_Leaf 
   }
   // => Iron, Paper, Banana_Leaf are private

   priv struct Rocket {
      int color,
      Material material
   }
   // => color, material are private
}

type MyRocket = sci::Rocket;     //❌
int hardness(sci::Material m);   //❌
sci::Rocket make_rocket();       //❌

void main()
{
   sci::Rocket R = ...;  //❌ priv sci::Rocket
}

```
  
Example 6: factory functions  
```
module sci
{
   priv enum Material { Iron, Paper, Banana_Leaf }

   priv struct Rocket {
      pub int color,
      pub Material material
   }

   pub Rocket craft_Rocket() {...}
   pub void Rocket::launch(Rocket& self) {...}
}

void experiment()
{
   auto r = sci::craft_rocket();   //✅
   r.color;      //✅
   r.launch();   //✅
}

```
  
Example 7: empty structs  
```
module Aphabet
{
   pub struct Latin {}         // Latin {}: YES
   priv struct Greek {}        // Greek {}: NO
   pub struct Slavic { priv }  // Slavic {}: NO
}

```
  
### Export  
* Rules for export is pretty much the same as public  
* The only thing new: A struct or enum can't have both export fields/variants/members and public fields/variants/members at the same time ⚔️🪃💥  
  
Example 1: pub, priv, export  
```
module MY
{
   pub    void visible() {...}
   export void exported() {...}
   priv   void hidden🫥() {...}
}

void main()
{
   MY::scoped();   //✅
   exported();     //✅
   hidden🫥();     //🫷❌🙅‍♂️
}

```
  
Example 2: export colision  
```
module A
{
   export void ImGroot() {...}
}

module B
{
   export void ImGroot() {...}  
   //💥 already defined in A
}

void ImGroot() {...}  
//💥 already defined in A

```
  
Example 3: export struct  
```
module health
{
   export struct Vital
   {
      export float pulse;
      priv float temp;
      priv float oxygen;
   }

   export Vital Vital::get_analysis();        //✅

   export float Vital::get_temp(this& self);  //✅

   pub float Vital::get_oxygen(this& self);   //❌
}

void the_dilemma()
{
   Vital v = get_analysis();

   /////////////////////////////////
   //////// imaginary code /////////
   /////////////////////////////////

   health::Vital::get_oxygen(v);  
   //❌ no health::Vital exist!

   Vital::get_oxygen(v);  
   //❌ no global::Vital::get_oxygen exist!

   Vital::(health::get_oxygen)(v);  
   // error: 0 sanity found😵‍💫😵. banning this syntax

   /////////////////////////////////
   /////////////////////////////////
   /////////////////////////////////
}

```
  
Example 4: likewise but for enums  
```
module lighting
{
   priv enum Light {
      priv Neon,
      priv LED,
      export Latern👻
   }

   pub void install(Light l) {...}

   export void Light::shine(Light& self);  //✅
   pub void refuel(Light& self);           //❌
}

void main()
{
   lighting::install(Neon);              //❌ priv Light::Neon
   lighting::install(Light::Lantern👻);  //❌ priv Light
   lighting::install(Lantern👻);         //✅
}

```
  
### Homonymous modules  
* Different modules can have the same name  
    - Public symbols can be used without scope resolution   
    - Private symbols can have the same name but still only accessible to its local module. So no collision happens  
(compiler magic✨ handshake🤝)  
=> Illusion of a single module  
  
Example 1: modules of the same name  
```
module calculus
{
   pub double arith_mean(double(double) f, double lower, double upper) {...}
}

module calculus
{
   pub double geom_mean(double(double) f, double lower, double upper) {...}
}

```
  
Example 2: uniqueness of private symbols  
```
module typedefs {
pub:

   type length_t = double;

priv:

   type speed_t = int;
}

module typedefs {
pub:

   type sqlenght_t = length_t[2];  //✅ pub
   type vspeed_t = speed_t[2];     //❌ priv
}

```
  
Example 3: hidden = no colision  
```
module art {
priv:

   void helper1() {...}
   void helper2() {...}
   pub void render_donut() {...}
}

module art {
priv:

   void helper1() {...}  //✅ a different helper1
   void helper2() {...}  //✅ a different helper2
   pub void render_donut() {...}  //💥 collision city😵😵

   pub void render_figlet(str text) {...}
}

```
  
### Anonymous modules  
* Modules can be anonymous  
* Functions inside can be exported but not publicized  
* Types inside can be publicized but can not be used directly  
  
Example 1: anonymous modules  
```
module
{
   pub double sinh(double x);     //❌ ???::sinh😬
   export double sinh(double x);  //✅
}

```
  
Example 2: versioning, hiding implementation details  
```
module
{
   //helpers
   double asin_helper1() {...}
   double asin_helper2() {...}
   double asin_coef_table = {...}

   //old versions
   double asin_old_maclurin(double x) {...}
   double asin_old_chebyshev_slow(double x) {...}

   //current versions
   export double asin(double x) {...} //✅
}

void main()
{
   double(double) f = asin;
}

```
  
Example 3: restrict type usage (pt.1)  
```
module
{
   pub enum Direction
   {
      Nort,
      East,
      South,
      West
   }

   export void sail(float spd, Direction di) {...}
}

void main()
{
   (🫲🫲🤨?)::Direction dir1 = West;
   // ❌ what scope?

   auto dir2 = (🫲🫲🤨?)::Direction::West;
   // ❌ what scope?

   auto dir3 = West;
   // ❌ Automatic Scope Resolution FAILED!

   sail(spd = 12.8, di = West);
   // ✅
}

```
  
Example 4: restrict type usage (pt.2)  
```
module
{
   pub struct Box {
      int* items,
      int count
   }

   export Box createBox() {...}
   export void recieveBox(Box) {...}
}

void main()
{
   // use Box directly
   ???::Box mybox = createBox();  //❌ whuts "???"

   // use Box indirectly
   auto mybox = createBox()  //✅
   createBox().items;        //✅
   recieveBox(createBox());  //✅
}

```
  
Example 5: Cursed😱, dont look🫣  
```
module
{
   pub enum Crate::Type { Carton, Wood, Leaf };

   pub struct Crate {
      Type crate_type,
      int space
   }

   export Crate build_crate(Crate::Type type, int space) { 
      return Crate { type, space }; 
   }
}

void main()
{
   // decltype + Automatic Scope Resolution
   type A = decltype(build_crate(Carton, 0));
   type B = decltype(build_crate(Carton, 0).crate_type);
   
   A my_crate = Crate { B::Carton, 12 };
   B my_crate_type = B::Leaf;
}

```
  
### Nested modules  
* Nested modules are modules living inside another module  
* Modules can be pub or export, whole module or part of it  
pub = visible to 1-level-lower scope  
export = -1 scope  
  
Example 1: nested modules  
```
module math
{
   pub double exp(double x) {...}

   module cordic
   {
      pub double sin(double x) {...}
      // 👉 pub to math

      pub(global) double cos(double x) {...}
      // 👉 pub to global

      pub(hyp) double asin(double x) {...}
      // 👉 pub to hyp
   }

   pub module hyp
   {
       double sinh(double x) {...}
       double cosh(doubel x) {...}
   }
}

void main()
{
   auto f = math::exp;           //✅
   auto g = math::cordic::sin;   //❌ pub to math
   auto h = math::cordic::cos;   //✅ pub to global
   auto i = math::cordic::asin;  //❌ pub to hyp
}

```
  
Example 2: export  
```
module math
{
   export double exp(double x) {...}

   module cordic
   {
      export double sin(double x) {...}
      // 👉 export to math

      export(global) double cos(double x) {...}
      // 👉 pub to global

      export(hyp) double asin(double x) {...}
      // 👉 pub to hyp
   }

   module hyp
   {
       double sinh(double x) {...}
       double cosh(doubel x) {...}
   }
}

void main()
{
   auto f = exp;   //✅
   auto g = sin;   //❌ pub to math, priv to global
   auto h = cos;   //✅ pub to global
   auto h = asin;  //❌ pub to hyp, priv to global
}

```
  
Example 3: remote definition  
```
module math
{
   // declare as pub
   pub double sin(double x);

   module cordic
   {
      export double sin(double x) {...}
      // 👉 export to math
      // 👉 now math::sin has definition!

      export double cos(double x) {...}
      // 👉 export to math as priv
   }
}

void main()
{
   auto f = math::sin;  //✅
   auto g = math::cos;  //❌ priv
}

```
  
Example 4: access policy fields  
```
module browser
{
   module element {
   pub(global):

      struct Page {...}
      struct Link {...}

   pub:

      struct File {...}
      struct Ads {...}
   }

   module mini_apps {
   priv:

      struct Mail {...}
      struct Maps {...}

   export(global):

      struct Assistance {...}
   }
}

```
  
Example 5: yes, it exists😬  
```
module A
{
   module A
   {
      pub void func() {}
   }

   void func()
   {
      func();    // global::A::func(), Aka itself
      A::func(); // global::A::A::func()
      global::A::func();  // also itself
   }
}

```
  
Example 6: nested module merging  
```
module physics
{
   module formula
   {
      pub double gravity(double m1, double m2, double r) {...}
   }
}

module physics
{
   module formula
   {
      type Pstat = [double m, double r];
      pub double launch_energy(double obj_m, Pstat planet) {...}
   }
}

```
  
Example 7: absolute scope  
```
module math
{
   pub struct Poly {
      priv Vec<double> coef
   }

   priv helper() { println("Helping..."); }

   module global::std
   {
      pub str fmt_element(Poly& P) {
         helper();  //✅ math::helper
         ...
      }
      // 👉 pub to math
   }

   void print_Poly(Poly& P) {
      println(format("P = {}", P));
   }
}

```
  
### Module files  
* A module can be made portable by puting into a .mcm file  
* Each .mcm file holds a module. Its name is declared using:  
```
@module <name>  // set module name
@module _       // use file name

```
* To use those portable modules:  
```
import "lib.mcm";  (relative/absolute path)
import <lib.mcm>;  (system path)

```
  
Example 1: module file  
# File: Poly.mcm  
```
@module math
pub struct Poly {
   Vec<double> coef
}

impl Poly
{
   pub Poly from(double[] coef)
   {
      return Poly { from(coef) };
   }

   pub uint deg(Poly& self)
   {
      return self.coef.size() - 1;
   }

   pub double op[[]](Poly& self, uint i)
   {
      if (0 <= i && i < coef[::])
         return self.coef[i]
      else
         return 0;
   }

   pub double op[()](Poly& self, double x)
   {
      double mut { sum, a } = { 0, 1 };
      for (i: 0..self.deg())
      {
         sum += self[i] * a;
         a *= x;
      }
      return sum;
   }
}

str monomial(Poly& P, uint i)
{
   bool first = (i == P[P.deg()]);
   double val = P[i];
      
   if (i == 0)
   {
      if (first)
         return "{val}";
      elif (val > 0)
         return " + {val}";
      else if (val < 0)
         return " - {-val}";
   }
   elif (i == 1)
   {
      if (first)
         return "{val}x";
      elif (val == 1)
         return " + x";
      elif (val == -1)
         return " - x";
      elif (val > 0)
         return " + {val}x";
      elif (val < 0)
         return " - {-val}x";
   }
   else
   {
      if (first)
         return "{val}x^{i}";
      elif (val == 1)
         return " + x^{i}";
      elif (val == -1)
         return " - x^{i}";
      elif (val > 0)
         return " + {val}x^{i}";
      elif (val < 0)
         return " - {-val}x^{i}";
   }
   return "";
}
         
export str <str>(Poly& P)
{
   str result = "";
   for (P.deg() >= i >= 0)
   {
      result += monomial(P, i);
   }
   return result;
}

```
  
Example 2: importing module files  
# File: main.mcs  
```
import <io>;         // for std::println
import "Poly.mcm";
import "math.mcm";   // for math::sum

void main()
{
   math::Poly P = from({ -1, 0, 3, -2 });
   std::println("P(x) = {P}");

   double sum = math::sum(P, 0, 10);
   std::println("sum k: 0 -> 10 \{ P(k) } = { sum }");
}

```
output:  
```
P(x) = -2x^3 + 3x^2 - 1
sum k: 0 -> 10 { P(k) } = -4906

```
  
Example 3: trying to import a .mcs file (spoiler: 💥)  
# File: source.mcs  
```
void be_helpful()
{
   std::println("😇");
}

```
  
# File: main.mcs  
```
import "source.mcs";  
// error: @module not found

```
  
Example 4: module imports is private  
```
module 🦆
{
   import <io>;     //✅
   pub import<io>;  //❌

   void who()
   {
      std::println("🦆🖐️💦");
   }
}

module 🐥
{
   import <io>;
   void who()
   {
      std::println("🐣🖐️");
   }
}

void main()
{
   std::println("what");  //❌ std::println? who😑?
}

```
  
Example 5: importing each other  
# File: matrix.mcm  
```
@module math
import "vector.mcm";

pub struct Matrix_3x1 {
   priv double[1][3] elem
}

impl Matrix_3x1
{
   pub this from(double[1][3] elem)
   {
      return this { elem };
   }

   pub vector_3d toVec(this& self)
   {
      return from(self.elem[0][0], self.elem[1][0], self.elem[2][0]);
   }
}

```
  
# File: vector.mcm  
```
@module math
import "matrix.mcm";

pub struct Vector_3d {
priv:

   double x,
   double y,
   double z
}

impl Vector_3d
{
   pub this from(double x, double y, double z)
   {
      return this { x, y, z };
   }

   pub Matrix_3x1 toMat(this& self)
   {
      return from({ 
         { x }, 
         { y }, 
         { z } 
      });
   }
}

```
  
# File: main.mcs  
```
import "vector.mcm"

void main()
{
   Vector_3d v = from(3, -1, 2);
   // ✅

   Matrix_3x1 m = from({ { 3 }, { -1 }, { 2 } });
   // ❌
}

```
  
Example 6: anonymous module file  
# File: benchmak.mcm  
```
@module
import <clock>;


static std::Static_Clock clock = init();
static double mut time = 0;

export void benchmark_start()
{
   time = clock.now().by_ms();
}

export double benchmark_stop()
{
   double temp = time;
   time = clock.now().by_ms();
   return time - temp;
}

```
  
# File: main.mcs  
```
import <math>;
import "benchmak.mcm";

void main()
{
   start();
   for (i: 0..1e5)
   {
      [[noerase]]
      double dump = std::asin(<double>(i) / 1e5);
   }
   println("Elapse: {benchmark::stop()}");
}

```
  
### Embeded binary  
* Monocell module files store both text and binary  
* Compiling a .mcm file adds the newest binary to it  
* You can also clear the binay part using the compiler  
Example 1:  
```
compile:   cell Poly.mcm -dbg
           cell Poly.mcm -rls

clear:     cell Poly.mcm -clear
           cell A.mcm B.mcm C.mcm -clear

```
  
Example 2:  
```
cell main.mcs -dbg app.exe
👉 Also compiles uncompiled imported .mcm

cell main.mcs -dbg -call app.exe
👉 Compiles everything (call = compile all)

cell main.mcs A.mcm -dbg app.exe
👉 Guarantees recompiling A.mcm

```
  
  
  
  
  
## # Static variables  
* Variables that can live across all scopes and only die when the program ends  
* They live in the data segment of the binary. They don't need a function to exist (the address of a static variable is actually a compile-time constant!🤯)  
* Can only be initialized by a compile time constant  
* Can not be shadowed be another static variablemfrom the same scope  
  
Example 1: system-wide constants  
```
static int MAJOR = 1;
static int MINOR = 28;
int REGULAR = 97;  //❌ local var💣

void main()
{
   str s = read();
   if (s == "--version")
      println("cell version {MAJOR}.{MINOR}");
}

```
  
Example 2: hidden state  
```
module precision
{
   static uint mut value = 6;

   pub void set(uint new_value)
   {
      value = new_value;
   }

   export str fmt(f64 value) {...}
}

void main()
{
   println("1/3 = { fmt(1.0 / 3.0) }");
   precision::set(10);
   println("1/3 = { fmt(1.0 / 3.0) }");
}

```
output:  
```
0.333333
0.3333333333

```
  
Example 3: unique Boss monster  
```
struct Boss {
   int HP,
   int ATK,
   int DEF
}

impl Boss
{
   Boss mut& The_King_of_Corpes()
   {
      static Boss mut him = Boss { 2700, 180, 500 };
      return him;
   }

   Boss mut& The_Forsaken_Chamber_Keeeper()
   {
      static Boss mut him = Boss { 5000, 200, 880 };
      return him;
   }

   void talk(Boss& self) {...}
   void attack(Boss& self) {...}
}

void main()
{
   Boss::The_King_of_Corpes().talk();
}

```
  
Example 4: monster population  
```
module Moth_Monster
{
   static uint pop_limit = 30_000_000_000;
   static uint mut current_pop = 5;

   pub void breed()
   {
      if (current_pop * 2 <= pop_limit)
         current_pop *= 2;
      else
         current_pop = pop_limit;
   }

   pub void disease()
   {
      current_pop /= 2;
   }
}

```
  
Example 5: tables  
```
module
{
   static double[] xi =
   {
      ...
   };

   static double[] ci =
   {
      ...
   };

   export double Gauss(double(double) f, double lower, double upper)
   {
      double scale = (upper - lower) / 2;
      double mid = (upper + lower) / 2;

      double mut sum = 0;
      for (i: 0..32)
      {
         sum += ci[i] * f(scale * xi[i] + mid);
      }
      return scale * sum;
   }
}

```
  
Example 6: class special value  
```
struct Complex {
   double re,
   double im,
}

static Complex Complex::i = Complex { 0, 1 };

void main()
{
   auto z = Complex::i;
}

```
  
Example 7: pub/export static variables  
```
struct A {...}

module helper
{
   static int variant_count = 10;
   pub static void(A mut&)[num] destructor = {...};
   export static bool(A&, A&)[num] comparator = {...}
}

```
  
Example 8: shadowing  
```
static int x = 1;

module M
{
   static int x = 2;  //✅ shadowing accepted
}

void func()
{
   static int x = 3;  //✅ shadowing accepted
}

static int x = 4;  //❌ nah bro, two global::x can't coexist!

```
  
  
  
  
  
## # Constants  
### Constant expressions  
* Const expressions are expression evaluated at compile time  
* Constant variable  
    1. Immutable  
    2. = (some constant expression)  
* Static variables can only be initialized using compile-time constants  
=> All immutable static variables are compile time constants  
  
Example 1: compile-time constants  
```
double PI = 3.141592;
int ELEVEN = 11:

```
  
Example 2: compile-time functions  
```
import <math>;
double SQURT_PI = std::sqrt(3.141592);

```
  
Example 3: array size  
```
static int N = 10;

void main()
{
   float[N] arr = { 5.66; N };

   int size = N * 3 / 2;   // equals 15
   float[size] arr = { 7.62; size };
}

```
  
### Constant functions  
* Some functions are qualified to be called at compile time. These are called const-qualified functions  
* When these functions are invoked, they become constant expressions (even if the function returns nothing)  
* Only contant variables can be used as const-qualified functions  
* Const qualification is automatic but you can use [const] to let the compiler check whether it is or nah  
  
🪴 To achieve const qualification, the function:  
    1. Must not IO  
        * Why? Accessing file is not within CVM (Compiler Virtual Machine) space  
    2. Must not return a value containing pointer to heap  
        * Why? CVM itself is a program. It has it own heap, which is not the compiled program's heap. Storing a pointer to CVM's heap is UB  
    3. Must not use value of mutable static variables  
        * Why? It creates some surprises in your code  
    4. Must not use noinit variables  
        * Why? Const functions are expected to be pure. Noinit variables may cause undefined behavior (luck dependent output)  
    5. Must not use memory transmutation related to pointers  
        * Why? Not gonna let you hide a pointer in a ulong. Ble ble ble😛  
    6. Must not access any out-of-bound addresses  
        * Why? Again, these memory change over CVM runs so the dereferenced value is luck dependent  
  
Example 1: const-qualified sqrt implementation  
```
[const] double sqrt(double mut x)
{
   if (x < 0)  return NaN;
   if (x == 0) return 0
   if (x == 1) return 1;

   double mut scale = 1;
   while (x < 0.4)
   {
      x *= 4;
      scale *= 0.5;
   }

   while (x > 1.6)
   {
      x *= 0.25;
      scale *= 2;
   }

   double mut y = 0.5 * (x + 1);
   for (0..4)
   {
      y = 0.5 * (y + x / y);
   }

   return y * scale;
}

```
  
Example 2: IO is not allowed  
```
[const] void lemme_print()
{
   println("yolo");  // ❌ STOP RIGHT THERE!
}

```
  
Example 3: surprize 💣🎁  
```
int mut val = 1;

int getval()
{
   return val;  // looks OK, no mutations🤡
}

void main()
{
   val = 2;

   // if getval was const-qualified
   int x = getval();  // 1 💣🎁

   // reality
   int x = getval();  // 2
}

NOTE: This is why we have rule 3

```
  
Example 4: reference  
```
[const] int[10] pow_transform(int[10]& arr, int n)
{
   int[10] mut res = { 0; 10 };
   for (i: 0..9)
   {
      res[i] = pow(arr[i], n);
   }
   return res;
}

static int[10] arr = { 2; 10 };
static int[10] pw = pow_transform(arr, 3);

```
  
Example 5: pointer dance  
```
[const] int mut& ref(int mut& p1, int mut& p2)
{
   return (&p1 > &p2? p1, p2);
}

static int[2] mut arr = { 5, 4 };
static int& later_var = ref(arr[0], arr[1]);

```
  
Example 6: compare 2 strings  
```
[const] [char*, ulong] strcmp(str s1, str s2)
{
   if (s1[::] > s2[::])
      return "longer"c;
   elif (s1[::] == s2[::])
      return "equal"c;
   else
      return "shorter"c;
}

void main()
{
   [char*, ulong] cmp_s = strcmp("hello", "hi");
}

```
  
Example 7: const lambda  
```
void main()
{
   int(int) f = [const] int(int x) { return x * x; };  //✅ f is const, compiler can see the function

   int(int) mut g = [const] int(int x) { return x * x; };  //❌ g is mutable

   int local_var = 7749;
   auto h = [const] int(int x | local_var) { return x * x; };
   //❌ This is beyond redemption🪦👻
}

```
  
### The const operator  
* Forces check compile time on any expression  
  
Example 1: forced const checking  
```
void main()
{
   double SQRT_PI_OVER_TWO = const my::sqrt(PI) / 2.0;
}

```
  
Example 2: const block  
```
void main()
{
   // const variable
   int[8] arr = { 0, 1, 2, 3, 4, 5, 6, 7 };

   int[8] rev = const @init_arr
   {
      int[8] mut res = { 0; 8 };
      for (i: 0..7)
         res[i] = arr[7 - i];
      break @init_arr res;
   }

   // array size
   int[rev[2]] arr = { 7749; rev[2] };
}

```
  
Example 3: calculate table at compile time  
```
module
{
   [const] double[100] calc_xi(int n) {...}
   [const] double[100] calc_ci(int n) {...}

   static double[100] xi = const calc_xi(32);
   static double[100] ci = const calc_ci(32);

   export double Gauss(double(double) f, double lower, double upper) {...}
}

```
  
Example 6: const immediate lambda  
```
void main()
{
   int x = const int(int x) { return x + 5; }(100);
   // 👆No need for [const]
}

```
  
Example 7: const vs [const]  
```
void func()
{
   int(int) f = const int(int x) { return x + 5; };
   // the value of f is const
}

static int(int) g = [const] int(int x) { return x + 5; };
// g is a const-qualified function

```
  
  
  
  
