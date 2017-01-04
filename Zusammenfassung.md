# C++ Spick
C++ Spick

TODO
Lroellin: 

* Ausgaben-Formatierung
* ADL


* Const
* Streams
    * String stream
    * Buffered
* Algorithms
* Initialisierung mit () vs {}
* Lambdas
    * Functor
    * Function Interface
* Cin / cout
    * Oct, dec usw.
* Cute
* Klassen
    * Vererbung
    * Virtual
    * Interface
    * Abstract
    * Enum
        * Scoped
        * unscoped
* Operator overloading
    * In Klasse und extern
* inline
* Using
* Namespace
    * Anonymous
* Variable Scopes
* Call by value / call by reference
* Exceptions
* Header files
    * Inline
* Constructor
    * Konstruktoren mit direkter Zuweisung, Reihenfolge?
    * Copy
    * Move
    * Destructor
* Files lesen / schreiben

[TOC]


# Include Files

Eigene Includes immer zuoberst!

Funktionen in Header-Files typischerweise nur als Deklaration. Wenn kurze Funktion (oder Template): ``inline``

* Klassen-Member-Funktionen sowie Templates sind implizit inline.

## Include Guard

Um die ODR (One Definition Rule) nicht zu verletzen, verwendet man Guard Statements

```C++
#ifndef SAYHELLO_H_
#define SAYHELLO_H_
#include <iosfwd>
void sayHello(std::ostream &out);
#endif /* SAYHELLO_H_ */
```

### Beispiel mit 3 Files

Hello.h

```C++
#ifndef HELLO_H_
#define HELLO_H_
#include <iosfwd>

struct Hello {
  void sayHello(std::ostream &out) const;
};
#endif /* HELLO_H_ */
```
Semikolon nicht vergessen!

Hello.cpp

```C++
#include "Hello.h"
#include <ostream>

void Hello::sayHello(std::ostream &out) const {
  out << "Hello world!\n";
}
```

main.cpp

```C++
#include "Hello.h"
#include <iostream>

int main() {
  Hello hello{};
  hello.sayHello(std::cout);
}
```

# Klassen

Es gibt zwei Keywords, ``struct``und ``class``. Die sind äquivalent, ausser

* struct ist standardmässig public
* class ist standardmässig private

Eine gute Klasse kennt eine Klasseninvariante, d.h. dass eine Instanz sich immer in einem guten Zustand befindet. Falls eine Änderung diese Invarianz verletzt, wird sie entweder zurückgerollt oder zerstört. Aber nicht im FUBAR-Zustand belassen.

## Beispielklasse Date

```C++
#ifndef DATE_H_
#define DATE_H_
class Date {
	int year, month, day;
public:
	Date(int year, int month, int day)
	: year{year}, month{month}, day{day}
	{
		...
	}
	static bool isLeapYear(int year) {
		...
	}
private:
	bool isValidDate() const {
		...
	}
};

#endif /* DATE_H_ */
```

## Access Specifier
* private
* protected (auch Subklassen)
* public

Visibilities können auch mehrmals verwendet werden

## Member Variables
Haben einen Typ und einen Namen. So const wie möglich. 

``<type> <name>``

## Static Member-Variablen
**Im Header**: als ``static`` oder als ``static const`` deklarieren. ``static const`` dürfen auch gleich initialisiert werden: 

```C++
Class Date {
	static const Date myBirthday;
	staic const Date favoriteStudentsBirthday;
	static const int zero{0};
}
```

**Im Implementationsfile**: kein ``static``. Es dürfen auch const-Variablen hier initialisiert werden (aber auch nicht const): 

```C++
#include "Date.h"
Date const Date::myBirthday{1964, 12, 24};
Date Date::favoriteStudentsBirthday{1995, 5, 10};
```

**Ausserhalb der Klasse**: mit ``<Classname>::<member>

```C++
#include "Date.h"
Date::favoriteStudentsBirthday = ...;
```


## Konstruktor
Der Konstruktor ist eine spezielle Member Funktion. Er hat **keinen** Rückgabewert. Es gibt eine Initializer-List für Member-Initialisierung

```C++
<class name>(<parameters>)
	: <initializer-list>
{}
```

### Spezielle Konstruktoren

**Default Constructor**
``Date(); / Date d{};``

Keine Parameter, implizit verfügbar wenn es keine anderen Konstruktoren gibt. Initialisiert die Member-Variablen mit Default-Werten

**Copy Constructor**
``Date(Date const &); / Date d2{d};``

Hat einen ``<own type> const &`` Parameter. Implizit verfügbar (ausser es gibt einen expliziten Move-Konstruktor oder Assignment-Operator). Kopiert alle Member-Variablen. Implementiert man normalerweise nicht selber. 

**Move Constructor**
``Date(date &&); / date d2{std::move(d)}``

Hat einen ``<own type> &&`` Parameter. Implizit verfügbar (ausser es gibt einen expliziten Copy-Konstruktor oder Assignment-Operator). Verschiebt alle Member (d ist dann tot). Impemenetiert man normalerweise nicht selber.

**Typeconversion Constructor**
``explicit Date(std::string const &); / Date d{"19/10/2016"s}``

Hat einen ``<other-type> const &`` Parameter. Konvertiert den Input-Typ wenn möglich. ``explicit`` deklarieren, damit nicht versucht wird ein anderer Typ in diesen String (im Beispiel) hineinzupressen.


### Implementation
Der Konstruktor soll die Invariante etablieren und die Member initialisieren. Konstruktoren bauen nur valide Instanzen und werfen ansonsten Exceptions. Beim Default-Konstruktor ohne Parameter sollten sinnvolle Defaultwerte gesetzt werden

Date.cpp

```C++ 
Date::Date(int year, int month, int day)
	: year{year}, month{month}, day{day}
{
	if(!isValidDate()) {
		throw std::out_of_range("invalid date");
	}
}

Date::Date() : Date{1980, 1, 1} {}

Date(Date const & other) : Date(other.year, other.month, other.day) {}
```

### Konstruktor mit std::istream &
Man kann einen Konstruktor definieren, der explizit nur istreams entgegennimmt:
``explicit Date(std::istream & in)``

Wenn das erstellen fehlschlägt, wird eine Exception geworfen.

```C++
Date::Date(std::istream & in) 
	: year{}, month{}, day{}
{
	read(in)
	if(in.fail)) {
		throw std::out_of_range("invalid date");
	}
}
```

Man könnte natürlich auch eine Factory-Funktion machen. 

### Konstruktoren wieder default machen/löschen
Wenn man einen eigenen Konstruktur gemacht hat, ist der Default-Konstruktor weg. Nun kann man einen der Konstruktoren wieder default machen mit

``<constructor-name>() = default``

Ebenso kann man Konstruktoren löschen:

``<constructor-name>() = delete


## Destruktoren
Genannt wie der Default-Konstruktor mit einem ~ zu Beginn: 
``~Date();``

Muss alle Ressourcen freigeben. Implizit verfügbar. Darf keine Exception werfen! Wird automatisch am Ende des Blocks für alle lokalen Instanzen aufgerufen.

## Vererbung
Base-Klassen werden nach dem Klassennamen spezifiziert

``class <name> : <base1>, ..., <baseN>``

Die Vererbung kann sogar eine Visibility haben. Dies beschränkt die **maximale** Visibility der geerbten Member.

## Implementation
Die eigentliche Implementierung sollte die Klasse im Header-File inkludieren und dann die Methoden implementieren. Wichtig: die Scope Specifier beachten

```C++
#include "Date.h"

Date::Date(int year, int month, int day)
	: year{year}, month{month}, day{day}
{
		...
}
bool Date::isLeapYear(int year) {
	...
}
bool Date::isValidDate() const {
	...
}
```

## Benutzung

```C++
#include "Date.h"

void foo() {
	Date today{2016, 10, 19};
	
	Date::isLeapYear(2016)
}
```

## Member-Funktionen
Dürfen die Invariante nicht verletzen. 

Es gibt das implizite ``this``-Objekt. Zugriff mit dem Pfeil ``->``: ``this->day``. Aber auch einfach ``day``.

In einer const-Memberfunktion dürfen die Member nicht verändert werden. Es können nur const-Member aufgerufen werden. 

### Static Member-Funktionen
Es gibt kein this-Objekt, können **nicht** const sein. Kein static Keyword. 

Aufruf: ``<classname>::<member>(): Date::isLeapYear(2016);``

## Operator-Overloading
> When in doubt, do as the ints do

Wie eine Funktion deklariert, allerdings als mit speziellem Namen: ``<returntype> operator<op>(<parameters>);

Unäre/binäre Parameter haben einen bzw. zwei Parameter. 

**Überladbare Operatoren**:

```
+----+-----+-----+-------+--------+----------+
| +  | -   | *   | /     | %      | ^        |
+----+-----+-----+-------+--------+----------+
| &  | |   | ~   | !     | ,      | =        |
+----+-----+-----+-------+--------+----------+
| <  | >   | <=  | >=    | ++     | --       |
+----+-----+-----+-------+--------+----------+
| << | >>  | ==  | !=    | &&     | ||       |
+----+-----+-----+-------+--------+----------+
| += | -=  | /=  | %=    | ^=     | &=       |
+----+-----+-----+-------+--------+----------+
| |= | *=  | <<= | >>=   | []     | ()       |
+----+-----+-----+-------+--------+----------+
| -> | ->* | new | new[] | delete | delete[] |
+----+-----+-----+-------+--------+----------+
```


**Nicht überladbare Operatoren**

* ::
* .*
* .
* ?:

### Beispiel: Date vergleichbar machen
year, month und day vergleichen. Wir nutzen den ``operator<``. Immer const &! Rückgabewert ``bool``. Achtung, hier wird eine Entwicklung gezeigt, damit man den Unterschied sieht.

Erste Variante: als freier Operator (in Klassenfile, aber unterhalb Klassendefinition). Hier zwei Parameter, Date lhs und Date rhs. ``inline`` verwenden, da im Header definiert.

```C++
class Date {
	int year, month, day; // private
};

ACHTUNG DAS GEHT NICHT!!!
inline bool operator<(Date const & lhs, Date const & rhs) {
	lhs.year? Geht nicht, kein Access auf private Member
}
```

Zweite Variante: als Member Operator (innerhalb Klassendefinition). Nur noch ein Parameter, Date rhs (implizites lhs/this). Ebenso implizites ``inline`` als Member. Da die Methode ``const`` ist, ist auch das ``this`` const.

```C++
class Date {
	int year, month, day; // private
	
	bool operator<(Date const & rhs) const {
		return year < rhs.year || 
			(year == rhs.year && (month < rhs.month ||
				(month == rhs.month &&  day == rhs.day )))
};
```

**In Verwendung**: einfach den ``<`` Operator verwenden:

```C++
std::cout << "is d older? " << (d < Date::myBirthday);
```

**Syntactic Sugar für Vergleiche**
``std::tie``kreiert ein Tupel. (``<tuple>``)

```C++
	return std::tie(year, month,day) < std::tie(rhs.year, rhs.month, rhs.day);
```

``std::tuple`` (Header) bietet die folgenden Operatoren

* operator==
* operator!=
* operator<
* operator<=
* operator>
* operator>=

Der Vergleich ist immer komponentenweise von links nach rechts. 

**Andere Vergleiche implementieren**

```C++
class Date {	int year, month, day; //privatepublic:	bool operator<(Date const & rhs) const;};
inline bool operator>(Date const & lhs, Date const & rhs) {  return rhs < lhs;}inline bool operator>=(Date const & lhs, Date const & rhs) {  return !(lhs < rhs);}inline bool operator<=(Date const & lhs, Date const & rhs) { 
	return !(rhs < lhs);}inline bool operator==(Date const & lhs, Date const & rhs) {	return !(lhs < rhs) && !(rhs < lhs); 
}inline bool operator!=(Date const & lhs, Date const & rhs) { 
	return !(lhs == rhs);}
```

Die ganzen Operatoren ausserhalb der Klasse sehen wie Boilerplate-Code aus. Deshalb gibts von Boost eine Klasse von der man erben kann, die genau diese zusätzlichen Operatoren bietet. ``private`` erben reicht aus.

Benötigt ``<``, bietet

* ``>``
* ``<=``
* ``>=``

```C++
#include "boost/operators.hpp"
#include <tuple>

class Date : private boost::less_than_comparable<Date> {	int year, month, day; //privatepublic:	bool operator<(Date const & rhs) const {
		return std::tie(year, month,day) < std::tie(rhs.year, rhs.month, rhs.day);
	}};
```

### Beispiel: Date an std::cout senden
Was wir wollen:
```C++
std::cout << Date::myBirthday;
```

Erste Variante: als freier Operator. Parameter: ``std::ostream &`` und ``Date const &``. Rückgabewert: ``std::ostream &`` für Output Chaining

Problem: kann private Member nicht ansprechen.

Zweite Variante: als Memberfunktion. Problem: std::ostream darf nicht links sein in Calls. Andersherum würds zwar vom Compiler her gehen (``Date::myBirthday << std::cout``) aber das ist per Konvention falsch.

Dritte Variante: public ``print(std::ostream & os) const``-Memberfunktion, die von operator<< aufgerufen wird. Jetzt funktioniert alles!

```C++
#include <ostream>

class Date {
	int year, month, day;
public:
	std::ostream & print(std::ostream & os) const {
		os << year << "/" << month << "/" 

};

inline std::ostream & operator<<(std::ostream & os, Date const & date) {
	return date.print(os);
}
```

Dieses "Pattern" braucht man immer wieder, auch z.B. fürs Einlesen. In den Streams gibt es auch ein Beispiel fürs Einlesen von Dates.


# Argument Dependent Lookup (ADL)
Wenn man Funktionen ausserhalb der Klasse, aber im selben Headerfile nutzt, sollte man die Klasse und die Funktion mit demselben Namespace versehen. Wenn der Compiler eine nicht-qualified Funktion oder einen nicht-qualified Operator auffindet, schaut er sich den Namespace der Typen an, die involviert sind. 


# Variablen

``<type> <name> {<value>};``

```C++
int anAnswer{42};
int const zero{};
```

Initialisierung kann weggelassen werden, wird aber nicht empfohlen. Leere Klammern bedeuten Default-Initialization
Mit ``=``können wir den Compiler den Typ entscheiden lassen (nicht mit geschw. Klammern kombinieren)

```C++
auto const i = 5
```

Mit ``const`` **muss** initialisiert werden (mit geschweiften Klammern). Mit ``constexpr``wird der Wert zur Compile-Zeit festgelegt.
Nicht vergessen: **As const as possible**

```C++
int const theAnswer{6*7}
double constexpr pi{3.14}
```

C++ definiert den Begriff des lvalue und rvalue. Man darf beispielsweise nur lvalues inkrementieren

```C++
x = 6 * 7
x // lvalue
6 * 7 // rvalue
x++ // ok
5++ // nicht ok
```

Liste des Bösen:
* eine Variable *darf* innerhalb eines Blocks neu verwendet werden, dies ist kein Fehler
* globale Variablen

# Typen

Eingebaute Typen (ohne include)

* bool
* char, unsigned char, *wchar_t*, *char16_t*, *char32_t*
* short, int, long, long long
* unsigned short, unsigned, unsigned long, unsigned long long
* float, double long double
* weitere

## Literale
* U/L für Integer (unsigned/long), Gross-/Kleinschreibung egal
* Exponenten mit E für float/double
* "ab"s macht einen String aus "ab", benötigt ``using namespace std::literals`` <== TODO prüfen, V2 S16


# Streams
Streams haben einen Status, der anzeigt ob I/O erfolgreich war oder nicht

* Nur .good() Streams können noch I/O
* Nach einem Fehler (.fail()) muss man den Zustand mit .clear() wieder löschen, die ungültigen Eingaben rausholen (.ignore())und weiterfahren

istream Zustände:

bit | query | entered
----|-------|---------
failbit | ``is.fail()`` | formatted input failed
eofbit | ``is.eof`` | end of input reached
badbit | ``is.bad`` | unrecoverable I/O error



Beispiel: robustes Einlesen eines int, mit istringstream als Zwischenstream

```C++
int inputAge(std::istream& in) {
  while(in) {
    std::string line{};
    getline(in, line);
    std::istringstream is{line};
    int age{-1};
    if(is >> age) {
      return age;
    }
  }
  return -1;
}
```

## Beispiel: Date read() implementieren
**Header:**

```C++
In der Klasse:
std::istream & read(std::istream & is);

Unterhalb der Klasse:
inline std::istream & operator>> std::istream & is, Date & date) {
	return date.read(is);
}
```

**``.read()`` implementieren**
Precondition: std::istream ist im .good()-State. Wenn wir kein Datum extrahieren können, setzen wir std::istream in den fail-State.

Wenn der Input nicht verwendet werden kann, wird das Objekt nicht überschrieben. 

```C++
class Date {  int year, month, day;public:	std::istream & read(std::istream & is) {		int year{-1}, month{-1}, day{-1};		char sep1, sep2;		//read values		is >> year >> sep1 >> month >> sep2 >> day; 
		try {			Date input{year, month, day};			//overwrite content of this object (copy-ctor)
			(*this) = input;			//clear stream if read was ok			is.clear();		} catch (std::out_of_range & e) {			//set failbit			is.setstate(std::ios::failbit | is.rdstate());		}		return is; }
	}
};
```

 


# Operatoren

* Wie aus Java bekannt
* ``and, or, not`` sind alternative Schreibweisen für ``&&, ||, !``
* ``bitand, bitor, xor`` sind ``&, |, ^``




## Reihenfolge

Achtung: in einer einzelnen Expression, wenn die Funktionsaufrufe nur durch Komma getrennt sind, ist die Reihenfolge undefiniert.

<table class="wikitable">

<tbody><tr>
<th style="text-align: left"> Precedence
</th>
<th style="text-align: left"> Operator
</th>
<th style="text-align: left"> Description
</th>
<th style="text-align: left"> Associativity
</th></tr>
<tr>
<th> 1
</th>
<td> <code>::</code>
</td>
<td> <a href="/w/cpp/language/identifiers#Qualified_identifiers" title="cpp/language/identifiers">Scope resolution</a>
</td>
<td style="vertical-align: top" rowspan="6"> Left-to-right
</td></tr>
<tr>
<th rowspan="5"> 2
</th>
<td style="border-bottom-style: none"> <code>a++</code>&nbsp;&nbsp; <code>a--</code>
</td>
<td style="border-bottom-style: none"> Suffix/postfix <a href="/w/cpp/language/operator_incdec" title="cpp/language/operator incdec">increment and decrement</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code><i>type</i>()</code>&nbsp;&nbsp; <code><i>type</i>{}</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/explicit_cast" title="cpp/language/explicit cast">Functional cast</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>a()</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_other#Built-in_function_call_operator" title="cpp/language/operator other">Function call</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>a[]</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_member_access#Built-in_subscript_operator" title="cpp/language/operator member access">Subscript</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>.</code>&nbsp;&nbsp; <code>-&gt;</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_member_access#Built-in_member_access_operators" title="cpp/language/operator member access">Member access</a>
</td></tr>
<tr>
<th rowspan="9"> 3
</th>
<td style="border-bottom-style: none"> <code>++a</code>&nbsp;&nbsp; <code>--a</code>
</td>
<td style="border-bottom-style: none"> Prefix <a href="/w/cpp/language/operator_incdec" title="cpp/language/operator incdec">increment and decrement</a>
</td>
<td style="vertical-align: top" rowspan="9"> Right-to-left
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>+a</code>&nbsp;&nbsp; <code>-a</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> Unary <a href="/w/cpp/language/operator_arithmetic#Unary_arithmetic_operators" title="cpp/language/operator arithmetic">plus and minus</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>!</code>&nbsp;&nbsp; <code>~</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_logical" title="cpp/language/operator logical">Logical NOT</a> and <a href="/w/cpp/language/operator_arithmetic#Bitwise_logic_operators" title="cpp/language/operator arithmetic">bitwise NOT</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>(<i>type</i>)</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/explicit_cast" title="cpp/language/explicit cast">C-style cast</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>*a</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_member_access#Built-in_indirection_operator" title="cpp/language/operator member access">Indirection</a> (dereference)
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>&amp;a</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_member_access#Built-in_address-of_operator" title="cpp/language/operator member access">Address-of</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>sizeof</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/sizeof" title="cpp/language/sizeof">Size-of</a><sup id="cite_ref-1" class="reference"><a href="#cite_note-1">[note 1]</a></sup>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>new</code>&nbsp;&nbsp; <code>new[]</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/new" title="cpp/language/new">Dynamic memory allocation</a>
</td></tr>
<tr>
<td style="border-top-style: none"> <code>delete</code>&nbsp;&nbsp; <code>delete[]</code>
</td>
<td style="border-top-style: none"> <a href="/w/cpp/language/delete" title="cpp/language/delete">Dynamic memory deallocation</a>
</td></tr>
<tr>
<th> 4
</th>
<td> <code>.*</code>&nbsp;&nbsp; <code>-&gt;*</code>
</td>
<td> <a href="/w/cpp/language/operator_member_access#Built-in_pointer-to-member_access_operators" title="cpp/language/operator member access">Pointer-to-member</a>
</td>
<td style="vertical-align: top" rowspan="12"> Left-to-right
</td></tr>
<tr>
<th> 5
</th>
<td> <code>a*b</code>&nbsp;&nbsp; <code>a/b</code>&nbsp;&nbsp; <code>a%b</code>
</td>
<td> <a href="/w/cpp/language/operator_arithmetic#Multiplicative_operators" title="cpp/language/operator arithmetic">Multiplication, division, and remainder</a>
</td></tr>
<tr>
<th> 6
</th>
<td> <code>a+b</code>&nbsp;&nbsp; <code>a-b</code>
</td>
<td> <a href="/w/cpp/language/operator_arithmetic#Additive_operators" title="cpp/language/operator arithmetic">Addition and subtraction</a>
</td></tr>
<tr>
<th> 7
</th>
<td> <code>&lt;&lt;</code>&nbsp;&nbsp; <code>&gt;&gt;</code>
</td>
<td> Bitwise <a href="/w/cpp/language/operator_arithmetic#Bitwise_shift_operators" title="cpp/language/operator arithmetic">left shift and right shift</a>
</td></tr>
<tr>
<th rowspan="2"> 8
</th>
<td style="border-bottom-style: none"> <code>&lt;</code>&nbsp;&nbsp; <code>&lt;=</code>
</td>
<td style="border-bottom-style: none"> For <a href="/w/cpp/language/operator_comparison" title="cpp/language/operator comparison">relational operators</a> &lt; and ≤ respectively
</td></tr>
<tr>
<td style="border-top-style: none"> <code>&gt;</code>&nbsp;&nbsp; <code>&gt;=</code>
</td>
<td style="border-top-style: none"> For <a href="/w/cpp/language/operator_comparison" title="cpp/language/operator comparison">relational operators</a> &gt; and ≥ respectively
</td></tr>
<tr>
<th> 9
</th>
<td> <code>==</code>&nbsp;&nbsp; <code>!=</code>
</td>
<td> For <a href="/w/cpp/language/operator_comparison" title="cpp/language/operator comparison">relational operators</a> = and ≠ respectively
</td></tr>
<tr>
<th> 10
</th>
<td> <code>a&amp;b</code>
</td>
<td> <a href="/w/cpp/language/operator_arithmetic#Bitwise_logic_operators" title="cpp/language/operator arithmetic">Bitwise AND</a>
</td></tr>
<tr>
<th> 11
</th>
<td> <code>^</code>
</td>
<td> <a href="/w/cpp/language/operator_arithmetic#Bitwise_logic_operators" title="cpp/language/operator arithmetic">Bitwise XOR</a> (exclusive or)
</td></tr>
<tr>
<th> 12
</th>
<td> <code>|</code>
</td>
<td> <a href="/w/cpp/language/operator_arithmetic#Bitwise_logic_operators" title="cpp/language/operator arithmetic">Bitwise OR</a> (inclusive or)
</td></tr>
<tr>
<th> 13
</th>
<td> <code>&amp;&amp;</code>
</td>
<td> <a href="/w/cpp/language/operator_logical" title="cpp/language/operator logical">Logical AND</a>
</td></tr>
<tr>
<th> 14
</th>
<td> <code>||</code>
</td>
<td> <a href="/w/cpp/language/operator_logical" title="cpp/language/operator logical">Logical OR</a>
</td></tr>
<tr>
<th rowspan="7"> 15
</th>
<td style="border-bottom-style: none"> <code>a?b:c</code>
</td>
<td style="border-bottom-style: none"> <a href="/w/cpp/language/operator_other#Conditional_operator" title="cpp/language/operator other">Ternary conditional</a><sup id="cite_ref-2" class="reference"><a href="#cite_note-2">[note 2]</a></sup>
</td>
<td style="vertical-align: top" rowspan="7"> Right-to-left
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>throw</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/throw" title="cpp/language/throw">throw operator</a>
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>=</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_assignment#Builtin_direct_assignment" title="cpp/language/operator assignment">Direct assignment</a> (provided by default for C++ classes)
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>+=</code>&nbsp;&nbsp; <code>-=</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_assignment#Builtin_compound_assignment" title="cpp/language/operator assignment">Compound assignment</a> by sum and difference
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>*=</code>&nbsp;&nbsp; <code>/=</code>&nbsp;&nbsp; <code>%=</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_assignment#Builtin_compound_assignment" title="cpp/language/operator assignment">Compound assignment</a> by product, quotient, and remainder
</td></tr>
<tr>
<td style="border-bottom-style: none; border-top-style: none"> <code>&lt;&lt;=</code>&nbsp;&nbsp; <code>&gt;&gt;=</code>
</td>
<td style="border-bottom-style: none; border-top-style: none"> <a href="/w/cpp/language/operator_assignment#Builtin_compound_assignment" title="cpp/language/operator assignment">Compound assignment</a> by bitwise left shift and right shift
</td></tr>
<tr>
<td style="border-top-style: none"> <code>&amp;=</code>&nbsp;&nbsp; <code>^=</code>&nbsp;&nbsp; <code>|=</code>
</td>
<td style="border-top-style: none"> <a href="/w/cpp/language/operator_assignment#Builtin_compound_assignment" title="cpp/language/operator assignment">Compound assignment</a> by bitwise AND, XOR, and OR
</td></tr>
<tr>
<th> 16
</th>
<td> <code>,</code>
</td>
<td> <a href="/w/cpp/language/operator_other#Built-in_comma_operator" title="cpp/language/operator other">Comma</a>
</td>
<td> Left-to-right
</td></tr></tbody></table>

# Funktionen

Wenn der Block Scope endet ``}`` ist die Lebenszeit vorbei
> } collects garbage

Achtung: in Funktionen können Variablen Shadowing machen, dies ist nicht verboten (wie in Java)

## Scopes Summary

* Global Scope ::
	* named Namespaces ::name:: (may be nested)
		* anonymous namespace (hides name from linker)
			* class scope (members)
				* function scope (parameters)
					* block scope (local variables)
						* temporaries (subexpression results) 

Achtung mit Referenzen. Wenn Parameter als Referenzen reinkommen, haben Änderungen darauf natürlich auch Einfluss auf die Originalvariable. Ebenso **NIE** eine lokale Variable als Referenz zurückgeben. Beim Stack abräumen geht diese flöten und die HSR brennt ab. Es sind **einzig** die eigenen Parameter wieder als Referenz zurückzugeben.

## Parameter Passing - Return Values

* Pass by value: ``f(type par)`` (bevorzugt)
* Pass by const ref: ``f(type const & par)``
* Pass by reference: ``f(type & par)``

* Return by value: ``type f()``
* return by reference: ``type & f(); type const &g``

## Function Overloading

```C++
void incr(int & var);
void incr(int & var, unsigned delta);
```

Nur wenn die Parameter-Typen unterschiedlich sind oder eine andere Anzahl haben, nicht der Rückgabewert. Overload wird zur Compile-Zeit entschieden. 

##  Default Arguments
``void incr(int & var, unsigned delta=1);``

Implizites Overloading. Wenn es n default Argumente gibt, gibt es n+1 Versionen der Funktion. 

## Funktionen als Parameter
Funktionen sind First-Class-Parameter in C++

```C++
void printfunc(double x, double f(double)) {
	std::cout << f(x);
}
```

In C nur als Function Pointer möglich

## Contract/Exceptions

Wenn eine Funktion ihren Contract nicht erfüllen kann, kann man ein paar Sachen tun. Hier aufgeführt sind nur diese, welche mit C++ besonders Sinn machen oder deren Syntax speziell ist

* Ignorieren
* Standard-Resultat (z.B. anonymous)
	* Macht mehr Sinn, wenn der Aufrufer das Standardresultat im Fehlerfall bestimmen kann
* Error-Wert: Sentinel
* Error Status Side Effect: Parameter oder globale Variable verändern
* Exceptions

### Exceptions

Benötigen kein throw, es kann alles geworfen werden was kopierbar ist ``throw 15;``. 

* Es gibt keine Möglichkeit zu spezifizieren was geworfen werden kann. 
* Es gibt keine Checks, ob man eine Exception nicht auffängt. 
* Es gibt keine Meta-Informationne
	* kein Stacktrace, keine Quellcode-Position
* Wenn eine Exception geworfen wird während eine Exception nach oben propagiert wird, bricht das Programm ab

Exceptions können natürlich gefangen werden

```C++
try {
	...
} catch ( type const &e) { // als Referenz
	...
}
``` 

> throw by value, catch by const reference

Reihenfolge ist wichtig, der erste gewinnt. Ein Catch All ``catch(...) {}`` (das ist tatsächlich die Syntax) muss zuletzt sein.

Es gibt vordefinierte Exception Types in ``<stdexcept>>``

* std::logic_error 
	* std::domain_error
	* std::invalid_argument
	* std::length_error
	* std::out_of_range
* std::runtime_error
	* std::range_error
	* std::overflow_error
	* std::underflow_error

Man kann als Konstruktorargument immer einen String als Grund angeben. Ebenso gibt es die ``.what()`` Member Funktion um den Grund zu erfragen

### CUTE Exceptions
Mit CUTE kann man die Exception mit ``ASSERT_THROWS(square_root(-1.0), std::invalid_argument);`` erfragen

# Move
Streams können nicht kopiert werden, aber "gemovet". Dabei werden sie wie kopiert, aber die Innereien werden rausgerissen". Die alte Variable ist dann unbrauchbar.

Move-Constructor: ``std::ofstream(std::ofstream &&)`` 

# Lambdas

Wenn es alle möglichen Klammern hat, ist es wahrscheinlich ein Lambda.

```C++
[lambda_capture]
(parameters)->return_type {
	statements
}
```

* Capture benennt Variablen die vom umgebenden Scope genommen werden oder erstellt sogar neue
	* ``=`` copy
	* ``&`` reference
	* Typ wird abgeleitet
* Parameter sind wie Funktionsparameter, auto möglich
* return_type kann weggelassen werden wenn void oder die return-Statements im Typ konsistent sind (Compiler erkennts) 

# Namespaces

Es gibt den globalen Namespaces, ``::``, zum Beispiel ``::read``. Sub-Namespaces sind erlaubt.

Ein Namespace kann mehrere Male geöffnet und geschlossen werden. Mit ``using`` kann man Namen von anderen Namespaces in den eigenen Scope importieren

Beispiel

```C++
namespace demo {
	void foo(); // 1
	namespace subdemo {
		void foo() {//2}
	}
namespace demo {
	void bar() {
		foo(); // 1
		subdemo::foo(); // 2
	}
}
void demo::foo() {//1} // definition
int main() {
	using demo::subdemo::foo;
	foo() // 2
	demo::foo() // 1
	demo::bar()
}
```

Dazu gibt es noch den anonymen Namespace, wenn man den Namen weglässt. Damit kann man Sachen ausserhalb des Files verstecken. Ist aber pöse. 




# Using
Von ``using`` gibt es zwei Varianten

* "Alias" mit ``using input=std::istream_iterator<std::string>``
* Member in Namespace übernehmen, z.B. Konstruktoren mit ``using std::set<T, COMPARE>::set;``

# Iterators

Include für alle Iterators: ``#include <iterator>``

* Jeder Container bietet Iteratoren
* Es gibt immer ein Paar von Iteratoren, ``begin(v)`` und ``end(v)``
* Es gibt die "allgemeine" Version wie oben, oder die spezialisierte Version ``v.begin()/v.end()``. Im Zweifelsfall die spezialisierte Version verwenden.
* C++-Iteratoren kennen das Ende nicht. Man kann aber gegen das Ende vergleichen ``iterator != v.end()``
* Auf Elemente mittels ``*`` zugreifen ``*iterator``
* Nächster Schritt des Iterators: ``++iterator`` 

Achtung, das Ende ist **vor** ``end``. 

Um read-only zu garantieren sollte ``cbegin()/cend()`` verwendet werden.

Liste des Bösen

* Eigene Loops

## Spezielle Iteratoren für I/O
**``std::ostream_iterator<T>`` gibt Werte vom Typ ``T`` an den gegebenen ``std::ostream`` aus**
Endet wenn Input-Range fertig ist
``copy(begin(v), end(v), std::ostream_iterator<int>{std::cout, ", "});``

**``std::istream_iterator<T>`` liest Werte vom Typ ``T``vom gegebenen ``std::istream``**
Endet wenn der istream nicht länger ``good`` ist

Diese nutzen aber beide intern den ``operator>>`` für Input. Der überspringt Leerzeichen und White Space
Für eine perfekte Kopie brauchtn wir auch den Rest. Dies geht mit ``istreambuf_iterator<char>``.

**Beispiele**
Kopieren mit istream_iterator

```C++
#include <iterator>
#include <iostream>
#include <algorithm>
#include <string>
int main() {
	using input=std::istream_iterator<std::string>;
	input eof{};
	input in{std::cin};
	std::ostream_iterator<std::string> out{std::cout, " "};
	copy(in, eof, out)
}
```

Kopieren mit istreambuf_iterator

```C++
#include <iterator>
#include <iostream>
#include <algorithm>
int main() {
	using input=std::istreambuf_iterator<char>;
	input eof{};
	input in{std::cin};
	std::ostream_iterator<char> out{std::cout};
	copy(in, eof, out)
}
```

## Kategorien

<table class="boxed">
<tbody><tr><th colspan="4">category</th><th>properties</th><th>valid expressions</th></tr>
<tr><td colspan="4" rowspan="2">all categories</td><td><i><a href="/CopyConstructible">copy-constructible</a></i>, <i><a href="/CopyAssignable">copy-assignable</a> and <i><a href="/Destructible">destructible</a></i></i></td><td><code>X b(a);<br>
b = a;</code></td></tr>
<tr><td>Can be incremented</td><td><code>++a<br>
a++</code></td></tr>
<tr><td rowspan="10"><a href="/RandomAccessIterator">Random Access</a></td><td rowspan="6"><a href="/BidirectionalIterator">Bidirectional</a></td><td rowspan="5"><a href="/ForwardIterator">Forward</a></td><td rowspan="2"><a href="/InputIterator">Input</a></td><td>Supports equality/inequality comparisons</td><td><code>a == b<br>
a != b</code></td></tr>
<tr><td>Can be dereferenced as an <i>rvalue</i></td><td><tt>*a<br>
a-&gt;m</tt></td></tr>
<tr><td><a href="/OutputIterator">Output</a></td><td>Can be dereferenced as an <i>lvalue</i> <br>
(only for <i>mutable iterator types</i>)</td><td><tt>*a = t<br>
*a++ = t</tt></td></tr>
<tr><td rowspan="2"></td><td><i><a href="/DefaultConstructible">default-constructible</a></i></td><td><tt>X a;<br>
X()</tt></td></tr>
<tr><td>Multi-pass: neither dereferencing nor incrementing affects dereferenceability</td><td><code>{ b=a; *a++; *b; }</code> </td></tr>
<tr><td colspan="2"></td><td>Can be decremented</td><td><tt>--a<br>
a--<br>
*a--</tt></td></tr>
<tr><td colspan="3" rowspan="4"></td><td>Supports arithmetic operators <tt>+</tt> and <tt>-</tt></td><td><tt>a + n<br>
n + a<br>
a - n<br>
a - b</tt></td></tr>
<tr><td>Supports inequality comparisons (<tt>&lt;</tt>, <tt>&gt;</tt>, <tt>&lt;=</tt> and <tt>&gt;=</tt>) between iterators</td><td><tt>a &lt; b<br>
a &gt; b<br>
a &lt;= b<br>
a &gt;= b</tt></td></tr>
<tr><td>Supports compound assignment operations <tt>+=</tt> and <tt>-=</tt></td><td><tt>a += n<br>
a -= n</tt></td></tr>
<tr><td>Supports offset dereference operator (<tt>[]</tt>)</td><td><tt>a[n]</tt></td></tr>
</tbody></table>


**Nicht möglich mit const Iterator**

## std::istream_iterator

Iterator für istream. Benutzen für formatierte Eingaben, z.B. doubles, ints. Beispiel:


```C++
int main () {
  double value1, value2;
  std::cout << "Please, insert two values: ";
  std::istream_iterator<double> eos;
  std::istream_iterator<double> iit (std::cin);
  if (iit != eos) value1=*iit;
  ++iit;
  if (iit != eos) value2=*iit;
  std::cout << value1 << "*" << value2 << "=" << (value1*value2);
}
```

## std::istreambuf_iterator

Iterator für istream. Benutzen für unformatierte Eingaben, z.B. char für char. Beispiel:

```C++
int main () {
  std::istreambuf_iterator<char> eos{};                    
  std::istreambuf_iterator<char> iit (std::cin.rdbuf());
  std::string mystring{};
  std::cout << "Please, enter your name: ";
  while (iit!=eos && *iit!='\n') mystring+=*iit++;
  std::cout << "Your name is " << mystring << ".\n";
}
```
TODO: Syntax-Highlighting stolpert über zweitletzte Zeile. Fix mit **

## std::ostream_iterator

Iterator für ostream. Formatierte Ausgabe. Beispiel:

```C++
int main () {
  std::vector<int> myvector{0,1,2,3,4,5,6,7,8,9};
  std::ostream_iterator<int> out_it (std::cout,", ");
  std::copy ( myvector.begin(), myvector.end(), out_it );
}
```

Output: "0, 1, 2, 3, 4, 5, 6, 7, 8, 9, “

## std::ostreambuf_iterator

Iterator für ostream. Unformatierte Ausgabe, char für char. Beispiel:

```C++
int main () {
  std::string mystring ("Some text here...\n");
  std::ostreambuf_iterator<char> out_it (std::cout);
  std::copy ( mystring.begin(), mystring.end(), out_it);
}
```

## std::reverse_iterator

Iterator in umgekehrter Reihenfolge. Funktioniert nur bei Birirectional oder Random Access.  Benutzung entweder mit rbegin(), rend() auf einem Container oder wie folgt:

```C++
int main () {
  std::vector<int> myvector{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
  using iter_type = std::vector<int>::iterator;
  iter_type from = myvector.begin();
  iter_type until = myvector.end();                 
  std::reverse_iterator<iter_type> rev_until (from);
  std::reverse_iterator<iter_type> rev_from (until);     
  while (rev_from != rev_until) std::cout << ' ' << *rev_from++;
}
```

Output: " 9 8 7 6 5 4 3 2 1 0"

# Containers

**Funktionen aller Container (ausser wenn anders vermerkt)**:

begin(), end(), cbegin(), cend(), rbegin(), rend(), crbegin(), crend(), size(), empty()

Statt ``operator[]`` wird ``.at`` empfohlen wenn verfügbar, da es dort Bound Checks gibt (HSR brennt nicht ab)

**Iterieren "foreach"**

```C++
for(auto const i:v) {
	std::cout << "element: " << i << "\n";
}
```
Um auch ändern zu können, braucht man eine Referenz als Loop-Variable

```C++
for(auto const &j:v) {
	j *= 2;
}
```

## std::vector

Entspricht ArrayList in java

**Include / Initialisieren**: ``#include <vector> / std::vector<int> v{};``, alternativ mit Angabe der Grösse ``std::vector<int> v(6)`` oder gleich mit 2 füllen ``(6,2)``

Ebenso kann er aus zwei Iteratoren konstruiert werden.

**Iterator**: random access

**Insert**:

``insert (const_iterator position, const value_type& val);``

``push_back (const value_type& val);``

``back_inserter(v) // Ziel für Copy``

Mit unterschiedlichen Werten füllen:

```C++
std::vector<double> w{};
generate_n(std::back_inserter(w),5,
	[x=2.0]() mutable {return x *= 2.0;}
);
```

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

pop_back();

**Get/Change**: ``operator[]``

## std::array

Wrapper über Klassischen C array mit length feld

**Include/Initialisieren**: ``#include <array>/std::array<int, 6> a{}; //6=size``

**Iterator**: random access

**Insert**: -

**Delete**: -

**Get/Change**: ``operator[]``

## std::deque

Ähnlich wie vector aber zusätzlich noch ``push_front(), pop_front()`` Methoden.

**Include / Initialisieren**: ``#include <dequeue> / std::deque<int> d{};``

**Iterator**: random access

**Insert**:

``insert(const_iterator position, const value_type& val);``

``push_back (const value_type& val);``

``push_front(const value_type& val);``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

``pop_back(); pop_front();``

**Get/Change**: ``operator[]``

## std::list

Double Linked List

**Include / Initialisieren**: ``#include <list> / std::list<int> l{};``

**Iterator**: bidirectional

**Insert**:

``insert(const_iterator position, const value_type& val);``

``push_back (const value_type& val);``

``push_front(const value_type& val);``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

``pop_back(); pop_front();``

**Get**: ``front(); back; // für mittlere Elemente Iterators benutzen``

## std::forward_list

Singly Linked List

**Include/Initialisieren**:``#include<forward_list>/std::forward_list<int> l{};``

**Iterator**: forward

**Insert**:

``insert_after(const_iterator position, value_type& val);``

``push_front(const value_type& val);``

**Delete**:

``erase_after (const_iterator position);``

``pop_front();``

**Get**: ``front(); //für andere Elemente Iterators benutzen``

## std::stack

LIFO (last in first out), **keine Iteratoren!**

**Include / Initialisieren**: ``#include <stack> / std::stack<int> s{};``

**Iterator**: -

**Insert**: ``push(const value_type& val);``

**Delete**: ``pop();``

**Get**: ``top(); // peek() in java``

## std::queue

FIFO (first in first out), **keine Iteratoren!**

**Include / Initialisieren**: ``#include <queue> / std::queue<int> {};``

**Iterator**: -

**Insert**: ``push(const value_type& val);``

**Delete**: ``pop();``

**Get**: ``front();``

## std::priority_queue

Pop nimmt grösstes Element aus der Queue, **keine Iteratoren!**

**Include / Initialisieren**: ``#include <queue> / std::priority_queue<int> {};``

**Iterator**: -

**Insert**: ``push(const value_type& val);``

**Delete**: ``pop();``

**Get**: ``top();``

## std::set

Entspricht TreeSet in Java, aufsteigend sortiert ohne Duplikate

**Include / Initialisieren**: ``#include <set> / std::set<int> s{};``

**Iterator**: bidirectional (immer const)

**Insert**: ``insert (const value_type& val);``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

**Get**: iterators

## std::multiset

Aufsteigend sortiert mit Duplikaten

**Include / Initialisieren**: ``#include <set> / std::multiset<int> s{};``

**Iterator**: bidirectional (immer const)

**Insert**: ``insert (const value_type& val);``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

**Get**: iterators

## std::map

Entspricht TreeMap in Java, aufsteigend sortiert mit Key und Value

**Include / Initialisieren**: ``#include <map> / std::map<Key,Value> s{};``

**Iterator**: bidirectional über std::pair<Key, Value>

**Insert**: ``insert (std::pair<Key, Value>{"key", “value”});``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

**Get**: ``map[key]``

## std:multimap

Map mit mehreren Elementen mit gleichen Keys

**Include / Initialisieren**: ``#include <map> / std::multimap<Key,Value> s{};``

**Iterator**: bidirectional über std::pair<Key, Value>

**Insert**: ``insert (std::pair<Key, Value>{"key", “value”});``

**Delete**:

``erase (const_iterator position);``

``erase (const_iterator first, const_iterator last);``

**Get**: ``multimap.find(Key)`` -> iterator über alle values

## std::unordered_set

HashSet in Java, nicht benutzen → C++ advanced

## std::unordered_map

HashMap in Java, nicht benutzen → C++ advanced

# Algorithms
Ein paar Beispiele:
Jeder Container hat ``size()``. Was wenn man nur zwei Iteratoren hat? ``std::distance(begin, end)``

for_each (halbböse): 

```C++
for_each(begin(v), end(v),[](auto x) {
	std::cout << x++ << "\n";
});
```

Wenn man eine Funktion ``print(int x)`` hat, geht auch
``for_each(begin(v), end(v), print);``

<table class="t-dsc-begin">

<tbody><tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Non-modifying_sequence_operations">  Non-modifying sequence operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/all_any_none_of" title="cpp/algorithm/all any none of"> <span class="t-lines"><span>all_of</span><span>any_of</span><span>none_of</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   checks if a predicate is <span class="t-c"><span class="mw-geshi cpp source-cpp"><span class="kw2">true</span></span></span> for all, any or none of the elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_all_any_none_of&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/for_each" title="cpp/algorithm/for each"> <span class="t-lines"><span>for_each</span></span></a></div></div>
</td>
<td>   applies a function to a range of elements <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_for_each&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/for_each_n" title="cpp/algorithm/for each n"> <span class="t-lines"><span>for_each_n</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   applies a function object to the first n elements of a sequence  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_for_each_n&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/count" title="cpp/algorithm/count"> <span class="t-lines"><span>count</span><span>count_if</span></span></a></div></div>
</td>
<td>   returns the number of elements satisfying specific criteria  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_count&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/mismatch" title="cpp/algorithm/mismatch"> <span class="t-lines"><span>mismatch</span></span></a></div></div>
</td>
<td>   finds the first position where two ranges differ  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_mismatch&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/equal" title="cpp/algorithm/equal"> <span class="t-lines"><span>equal</span></span></a></div></div>
</td>
<td>   determines if two sets of elements are the same  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_equal&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/find" title="cpp/algorithm/find"> <span class="t-lines"><span>find</span><span>find_if</span><span>find_if_not</span></span></a></div><div><span class="t-lines"><span></span><span></span><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   finds the first element satisfying specific criteria  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_find&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/find_end" title="cpp/algorithm/find end"> <span class="t-lines"><span>find_end</span></span></a></div></div>
</td>
<td>   finds the last sequence of elements in a certain range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_find_end&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/find_first_of" title="cpp/algorithm/find first of"> <span class="t-lines"><span>find_first_of</span></span></a></div></div>
</td>
<td>   searches for any one of a set of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_find_first_of&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/adjacent_find" title="cpp/algorithm/adjacent find"> <span class="t-lines"><span>adjacent_find</span></span></a></div></div>
</td>
<td>   finds the first two adjacent items that are equal (or satisfy a given predicate)  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_adjacent_find&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/search" title="cpp/algorithm/search"> <span class="t-lines"><span>search</span></span></a></div></div>
</td>
<td>   searches for a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_search&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/search_n" title="cpp/algorithm/search n"> <span class="t-lines"><span>search_n</span></span></a></div></div>
</td>
<td>   searches for a number consecutive copies of an element in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_search_n&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Modifying_sequence_operations">  Modifying sequence operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/copy" title="cpp/algorithm/copy"> <span class="t-lines"><span>copy</span><span>copy_if</span></span></a></div><div><span class="t-lines"><span></span><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   copies a range of elements to a new location  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/copy_n" title="cpp/algorithm/copy n"> <span class="t-lines"><span>copy_n</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   copies a number of elements to a new location  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_copy_n&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/copy_backward" title="cpp/algorithm/copy backward"> <span class="t-lines"><span>copy_backward</span></span></a></div></div>
</td>
<td>   copies a range of elements in backwards order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_copy_backward&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/move" title="cpp/algorithm/move"> <span class="t-lines"><span>move</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   moves a range of elements to a new location  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_move&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/move_backward" title="cpp/algorithm/move backward"> <span class="t-lines"><span>move_backward</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   moves a range of elements to a new location in backwards order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_move_backward&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/fill" title="cpp/algorithm/fill"> <span class="t-lines"><span>fill</span></span></a></div></div>
</td>
<td>   copy-assigns the given value to every element in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_fill&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/fill_n" title="cpp/algorithm/fill n"> <span class="t-lines"><span>fill_n</span></span></a></div></div>
</td>
<td>   copy-assigns the given value to N elements in a range <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_fill_n&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/transform" title="cpp/algorithm/transform"> <span class="t-lines"><span>transform</span></span></a></div></div>
</td>
<td>   applies a function to a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_transform&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/generate" title="cpp/algorithm/generate"> <span class="t-lines"><span>generate</span></span></a></div></div>
</td>
<td>   assigns the results of successive function calls to every element in a range <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_generate&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/generate_n" title="cpp/algorithm/generate n"> <span class="t-lines"><span>generate_n</span></span></a></div></div>
</td>
<td>   assigns the results of successive function calls to N elements in a range <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_generate_n&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/remove" title="cpp/algorithm/remove"> <span class="t-lines"><span>remove</span><span>remove_if</span></span></a></div></div>
</td>
<td>   removes elements satisfying specific criteria  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_remove&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/remove_copy" title="cpp/algorithm/remove copy"> <span class="t-lines"><span>remove_copy</span><span>remove_copy_if</span></span></a></div></div>
</td>
<td>   copies a range of elements omitting those that satisfy specific criteria  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_remove_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/replace" title="cpp/algorithm/replace"> <span class="t-lines"><span>replace</span><span>replace_if</span></span></a></div></div>
</td>
<td>   replaces all values satisfying specific criteria with another value  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_replace&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/replace_copy" title="cpp/algorithm/replace copy"> <span class="t-lines"><span>replace_copy</span><span>replace_copy_if</span></span></a></div></div>
</td>
<td>   copies a range, replacing elements satisfying specific criteria with another value  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_replace_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/swap" title="cpp/algorithm/swap"> <span class="t-lines"><span>swap</span></span></a></div></div>
</td>
<td>   swaps the values of two objects  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_swap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/swap_ranges" title="cpp/algorithm/swap ranges"> <span class="t-lines"><span>swap_ranges</span></span></a></div></div>
</td>
<td>   swaps two ranges of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_swap_ranges&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/iter_swap" title="cpp/algorithm/iter swap"> <span class="t-lines"><span>iter_swap</span></span></a></div></div>
</td>
<td>   swaps the elements pointed to by two iterators  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_iter_swap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/reverse" title="cpp/algorithm/reverse"> <span class="t-lines"><span>reverse</span></span></a></div></div>
</td>
<td>   reverses the order of elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_reverse&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/reverse_copy" title="cpp/algorithm/reverse copy"> <span class="t-lines"><span>reverse_copy</span></span></a></div></div>
</td>
<td>   creates a copy of a range that is reversed  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_reverse_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/rotate" title="cpp/algorithm/rotate"> <span class="t-lines"><span>rotate</span></span></a></div></div>
</td>
<td>   rotates the order of elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_rotate&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/rotate_copy" title="cpp/algorithm/rotate copy"> <span class="t-lines"><span>rotate_copy</span></span></a></div></div>
</td>
<td>   copies and rotate a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_rotate_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/random_shuffle" title="cpp/algorithm/random shuffle"> <span class="t-lines"><span>random_shuffle</span><span>shuffle</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-until-cxx17">(until C++17)</span></span><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   randomly re-orders elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_random_shuffle&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/sample" title="cpp/algorithm/sample"> <span class="t-lines"><span>sample</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   selects n random elements from a sequence <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_sample&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/unique" title="cpp/algorithm/unique"> <span class="t-lines"><span>unique</span></span></a></div></div>
</td>
<td>   removes consecutive duplicate elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_unique&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/unique_copy" title="cpp/algorithm/unique copy"> <span class="t-lines"><span>unique_copy</span></span></a></div></div>
</td>
<td>   creates a copy of some range of elements that contains no consecutive duplicates  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_unique_copy&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Partitioning_operations">  Partitioning operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_partitioned" title="cpp/algorithm/is partitioned"> <span class="t-lines"><span>is_partitioned</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   determines if the range is partitioned by the given predicate  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_partitioned&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partition" title="cpp/algorithm/partition"> <span class="t-lines"><span>partition</span></span></a></div></div>
</td>
<td>   divides a range of elements into two groups  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partition&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partition_copy" title="cpp/algorithm/partition copy"> <span class="t-lines"><span>partition_copy</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   copies a range dividing the elements into two groups  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partition_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/stable_partition" title="cpp/algorithm/stable partition"> <span class="t-lines"><span>stable_partition</span></span></a></div></div>
</td>
<td>   divides elements into two groups while preserving their relative order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_stable_partition&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partition_point" title="cpp/algorithm/partition point"> <span class="t-lines"><span>partition_point</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   locates the partition point of a partitioned range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partition_point&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Sorting_operations">  Sorting operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_sorted" title="cpp/algorithm/is sorted"> <span class="t-lines"><span>is_sorted</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   checks whether a range is sorted into ascending order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_sorted&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_sorted_until" title="cpp/algorithm/is sorted until"> <span class="t-lines"><span>is_sorted_until</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   finds the largest sorted subrange  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_sorted_until&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/sort" title="cpp/algorithm/sort"> <span class="t-lines"><span>sort</span></span></a></div></div>
</td>
<td>   sorts a range into ascending order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_sort&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partial_sort" title="cpp/algorithm/partial sort"> <span class="t-lines"><span>partial_sort</span></span></a></div></div>
</td>
<td>   sorts the first N elements of a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partial_sort&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partial_sort_copy" title="cpp/algorithm/partial sort copy"> <span class="t-lines"><span>partial_sort_copy</span></span></a></div></div>
</td>
<td>   copies and partially sorts a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partial_sort_copy&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/stable_sort" title="cpp/algorithm/stable sort"> <span class="t-lines"><span>stable_sort</span></span></a></div></div>
</td>
<td>   sorts a range of elements while preserving order between equal elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_stable_sort&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/nth_element" title="cpp/algorithm/nth element"> <span class="t-lines"><span>nth_element</span></span></a></div></div>
</td>
<td>   partially sorts the given range making sure that it is partitioned by the given element  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_nth_element&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Binary_search_operations_.28on_sorted_ranges.29">  Binary search operations (on sorted ranges) </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/lower_bound" title="cpp/algorithm/lower bound"> <span class="t-lines"><span>lower_bound</span></span></a></div></div>
</td>
<td>   returns an iterator to the first element <i>not less</i> than the given value <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_lower_bound&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/upper_bound" title="cpp/algorithm/upper bound"> <span class="t-lines"><span>upper_bound</span></span></a></div></div>
</td>
<td>   returns an iterator to the first element <i>greater</i> than a certain value <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_upper_bound&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/binary_search" title="cpp/algorithm/binary search"> <span class="t-lines"><span>binary_search</span></span></a></div></div>
</td>
<td>   determines if an element exists in a certain range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_binary_search&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/equal_range" title="cpp/algorithm/equal range"> <span class="t-lines"><span>equal_range</span></span></a></div></div>
</td>
<td>   returns range of elements matching a specific key <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_equal_range&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Set_operations_.28on_sorted_ranges.29">  Set operations (on sorted ranges) </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/merge" title="cpp/algorithm/merge"> <span class="t-lines"><span>merge</span></span></a></div></div>
</td>
<td>   merges two sorted ranges  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_merge&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/inplace_merge" title="cpp/algorithm/inplace merge"> <span class="t-lines"><span>inplace_merge</span></span></a></div></div>
</td>
<td>   merges two ordered ranges in-place  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_inplace_merge&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/includes" title="cpp/algorithm/includes"> <span class="t-lines"><span>includes</span></span></a></div></div>
</td>
<td>   returns true if one set is a subset of another  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_includes&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/set_difference" title="cpp/algorithm/set difference"> <span class="t-lines"><span>set_difference</span></span></a></div></div>
</td>
<td>   computes the difference between two sets  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_set_difference&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/set_intersection" title="cpp/algorithm/set intersection"> <span class="t-lines"><span>set_intersection</span></span></a></div></div>
</td>
<td>   computes the intersection of two sets  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_set_intersection&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/set_symmetric_difference" title="cpp/algorithm/set symmetric difference"> <span class="t-lines"><span>set_symmetric_difference</span></span></a></div></div>
</td>
<td>   computes the symmetric difference between two sets  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_set_symmetric_difference&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/set_union" title="cpp/algorithm/set union"> <span class="t-lines"><span>set_union</span></span></a></div></div>
</td>
<td>   computes the union of two sets  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_set_union&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Heap_operations">  Heap operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_heap" title="cpp/algorithm/is heap"> <span class="t-lines"><span>is_heap</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   checks if the given range is a max heap <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_heap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_heap_until" title="cpp/algorithm/is heap until"> <span class="t-lines"><span>is_heap_until</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   finds the largest subrange that is a max heap  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_heap_until&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/make_heap" title="cpp/algorithm/make heap"> <span class="t-lines"><span>make_heap</span></span></a></div></div>
</td>
<td>   creates a max heap out of a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_make_heap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/push_heap" title="cpp/algorithm/push heap"> <span class="t-lines"><span>push_heap</span></span></a></div></div>
</td>
<td>   adds an element to a max heap  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_push_heap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/pop_heap" title="cpp/algorithm/pop heap"> <span class="t-lines"><span>pop_heap</span></span></a></div></div>
</td>
<td>   removes the largest element from a max heap  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_pop_heap&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/sort_heap" title="cpp/algorithm/sort heap"> <span class="t-lines"><span>sort_heap</span></span></a></div></div>
</td>
<td>   turns a max heap into a range of elements sorted in ascending order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_sort_heap&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Minimum.2Fmaximum_operations">  Minimum/maximum operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/max" title="cpp/algorithm/max"> <span class="t-lines"><span>max</span></span></a></div></div>
</td>
<td>   returns the greater of the given values  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_max&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/max_element" title="cpp/algorithm/max element"> <span class="t-lines"><span>max_element</span></span></a></div></div>
</td>
<td>   returns the largest element in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_max_element&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/min" title="cpp/algorithm/min"> <span class="t-lines"><span>min</span></span></a></div></div>
</td>
<td>   returns the smaller of the given values  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_min&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/min_element" title="cpp/algorithm/min element"> <span class="t-lines"><span>min_element</span></span></a></div></div>
</td>
<td>   returns the smallest element in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_min_element&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/minmax" title="cpp/algorithm/minmax"> <span class="t-lines"><span>minmax</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   returns the smaller and larger of two elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_minmax&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/minmax_element" title="cpp/algorithm/minmax element"> <span class="t-lines"><span>minmax_element</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   returns the smallest and the largest elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_minmax_element&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/clamp" title="cpp/algorithm/clamp"> <span class="t-lines"><span>clamp</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   clamps a value between a pair of boundary values  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_clamp&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/lexicographical_compare" title="cpp/algorithm/lexicographical compare"> <span class="t-lines"><span>lexicographical_compare</span></span></a></div></div>
</td>
<td>   returns true if one range is lexicographically less than another  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_lexicographical_compare&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/is_permutation" title="cpp/algorithm/is permutation"> <span class="t-lines"><span>is_permutation</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   determines if a sequence is a permutation of another sequence  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_is_permutation&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/next_permutation" title="cpp/algorithm/next permutation"> <span class="t-lines"><span>next_permutation</span></span></a></div></div>
</td>
<td>   generates the next greater lexicographic permutation of a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_next_permutation&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/prev_permutation" title="cpp/algorithm/prev permutation"> <span class="t-lines"><span>prev_permutation</span></span></a></div></div>
</td>
<td>   generates the next smaller lexicographic permutation of a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_prev_permutation&amp;action=edit">[edit]</a></span>
</td></tr>


<tr>
<td colspan="2"> <h5> <span class="mw-headline" id="Numeric_operations">  Numeric operations </span></h5>
</td></tr>

<tr class="t-dsc-header">
<td colspan="2"> <div>Defined in header <code>&lt;numeric&gt;</code> </div>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/iota" title="cpp/algorithm/iota"> <span class="t-lines"><span>iota</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx11">(C++11)</span></span></span></div></div>
</td>
<td>   fills a range with successive increments of the starting value  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_iota&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/accumulate" title="cpp/algorithm/accumulate"> <span class="t-lines"><span>accumulate</span></span></a></div></div>
</td>
<td>   sums up a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_accumulate&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/inner_product" title="cpp/algorithm/inner product"> <span class="t-lines"><span>inner_product</span></span></a></div></div>
</td>
<td>   computes the inner product of two ranges of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_inner_product&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/adjacent_difference" title="cpp/algorithm/adjacent difference"> <span class="t-lines"><span>adjacent_difference</span></span></a></div></div>
</td>
<td>   computes the differences between adjacent elements in a range  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_adjacent_difference&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/partial_sum" title="cpp/algorithm/partial sum"> <span class="t-lines"><span>partial_sum</span></span></a></div></div>
</td>
<td>   computes the partial sum of a range of elements  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_partial_sum&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/reduce" title="cpp/algorithm/reduce"> <span class="t-lines"><span>reduce</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   similar to <span class="t-lc"><a href="/w/cpp/algorithm/accumulate" title="cpp/algorithm/accumulate">std::accumulate</a></span>, except out of order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_reduce&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/exclusive_scan" title="cpp/algorithm/exclusive scan"> <span class="t-lines"><span>exclusive_scan</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   similar to <span class="t-lc"><a href="/w/cpp/algorithm/partial_sum" title="cpp/algorithm/partial sum">std::partial_sum</a></span>, excludes the ith input element from the ith sum <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_exclusive_scan&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/inclusive_scan" title="cpp/algorithm/inclusive scan"> <span class="t-lines"><span>inclusive_scan</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   similar to <span class="t-lc"><a href="/w/cpp/algorithm/partial_sum" title="cpp/algorithm/partial sum">std::partial_sum</a></span>, includes the ith input element in the ith sum <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_inclusive_scan&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/transform_reduce" title="cpp/algorithm/transform reduce"> <span class="t-lines"><span>transform_reduce</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   applies a functor, then reduces out of order  <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_transform_reduce&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/transform_exclusive_scan" title="cpp/algorithm/transform exclusive scan"> <span class="t-lines"><span>transform_exclusive_scan</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   applies a functor, then calculates exclusive scan <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_transform_exclusive_scan&amp;action=edit">[edit]</a></span>
</td></tr>

<tr class="t-dsc">
<td>  <div class="t-dsc-member-div"><div><a href="/w/cpp/algorithm/transform_inclusive_scan" title="cpp/algorithm/transform inclusive scan"> <span class="t-lines"><span>transform_inclusive_scan</span></span></a></div><div><span class="t-lines"><span><span class="t-mark-rev t-since-cxx17">(C++17)</span></span></span></div></div>
</td>
<td>   applies a functor, then calculates inclusive scan <br> <span class="t-mark">(function template)</span> <span class="editsection noprint plainlinks" title="Edit this template"><a rel="nofollow" class="external text" href="http://en.cppreference.com/mwiki/index.php?title=Template:cpp/algorithm/dsc_transform_inclusive_scan&amp;action=edit">[edit]</a></span>
</td></tr>



</tbody></table>
