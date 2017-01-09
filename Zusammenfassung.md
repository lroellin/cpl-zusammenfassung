

# C++ Spick
##TODO
* Demo verstehen (V12, S. 11)
* V12, S.21
* Templates-Erweiterungen (V12)
* Ausgaben-Formatierung (Oct, dec usw.)
* Beispiele
	* Lexicographic Compare mit Algorithms
	* Ring5 (V6, S31, auch Übung)
* const Beschreiben, unterschied const Methoden und const Variabeln
* Function Interface (C++11)
	* V9 S.19-28 anschauen (übersprungen)
* Initialisierung mit () vs {}
* Cute
* Klassen
    * Interface
    * Abstract
    * Enum
        * Scoped
        * unscoped
* Beispiel für Anonymous Namespace?
* Call by value / call by reference
* Files lesen / schreiben
* Dekonstruktor ist nicht virtual

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Variablen](#variablen)
- [Typen](#typen)
  - [Literale](#literale)
- [Operatoren](#operatoren)
  - [Reihenfolge](#reihenfolge)
- [Funktionen](#funktionen)
  - [Scopes Summary](#scopes-summary)
  - [Parameter Passing - Return Values](#parameter-passing---return-values)
  - [Function Overloading](#function-overloading)
  - [Default Arguments](#default-arguments)
  - [Funktionen als Parameter](#funktionen-als-parameter)
- [Include Files](#include-files)
  - [Include Guard](#include-guard)
    - [Beispiel mit 3 Files](#beispiel-mit-3-files)
  - [Wichtige includes](#wichtige-includes)
- [Const/non-const und Value/Reference](#constnon-const-und-valuereference)
- [Kommandozeilenargumente übergeben](#kommandozeilenargumente-%C3%BCbergeben)
- [Memory (Heap)](#memory-heap)
  - [Beispiel: Eltern und Kinder](#beispiel-eltern-und-kinder)
- [Move](#move)
- [Namespaces](#namespaces)
- [CUTE TODO ist das nötig?](#cute-todo-ist-das-n%C3%B6tig)
- [Using](#using)
- [Streams](#streams)
  - [Beispiel: Date read() implementieren](#beispiel-date-read-implementieren)
- [Iterators](#iterators)
  - [Spezielle Iteratoren für I/O](#spezielle-iteratoren-f%C3%BCr-io)
  - [Kategorien](#kategorien)
  - [Spezialfunktionen](#spezialfunktionen)
  - [std::istream_iterator](#stdistream_iterator)
  - [std::istreambuf_iterator](#stdistreambuf_iterator)
  - [std::ostream_iterator](#stdostream_iterator)
  - [std::ostreambuf_iterator](#stdostreambuf_iterator)
  - [std::reverse_iterator](#stdreverse_iterator)
- [Containers](#containers)
  - [std::vector](#stdvector)
  - [std::array](#stdarray)
  - [std::deque](#stddeque)
  - [std::list](#stdlist)
  - [std::forward_list](#stdforward_list)
  - [std::stack](#stdstack)
  - [std::queue](#stdqueue)
  - [std::priority_queue](#stdpriority_queue)
  - [std::set](#stdset)
  - [std::multiset](#stdmultiset)
  - [std::map](#stdmap)
  - [std:multimap](#stdmultimap)
  - [std::unordered_set](#stdunordered_set)
  - [std::unordered_map](#stdunordered_map)
- [Nackte Arrays](#nackte-arrays)
  - [Initialisieren](#initialisieren)
  - [Länge erkennen mit Type Deduction](#l%C3%A4nge-erkennen-mit-type-deduction)
- [Lambdas](#lambdas)
  - [Captures und Parameter](#captures-und-parameter)
    - [Spezialfall mutable](#spezialfall-mutable)
- [Functor](#functor)
    - [Als Parameter](#als-parameter)
- [Algorithms](#algorithms)
  - [Ranges](#ranges)
  - [Beispiele aus Vorlesung](#beispiele-aus-vorlesung)
  - [Suffix-Versionen](#suffix-versionen)
  - [Heap-Algorithmen](#heap-algorithmen)
  - [Fallen](#fallen)
  - [Tabelle](#tabelle)
- [Klassen](#klassen)
  - [Beispielklasse Date](#beispielklasse-date)
  - [Access Specifier](#access-specifier)
  - [Member Variables](#member-variables)
  - [Static Member-Variablen](#static-member-variablen)
  - [Konstruktor](#konstruktor)
    - [Spezielle Konstruktoren](#spezielle-konstruktoren)
    - [Implementation](#implementation)
    - [Konstruktor mit std::istream &](#konstruktor-mit-stdistream-)
    - [Konstruktoren wieder default machen/löschen](#konstruktoren-wieder-default-machenl%C3%B6schen)
  - [Destruktoren](#destruktoren)
  - [Vererbung](#vererbung)
  - [Implementation](#implementation-1)
  - [Benutzung](#benutzung)
  - [Member-Funktionen](#member-funktionen)
    - [Static Member-Funktionen](#static-member-funktionen)
  - [Operator-Overloading](#operator-overloading)
    - [Beispiel: Date vergleichbar machen](#beispiel-date-vergleichbar-machen)
    - [Beispiel: Date an std::cout senden](#beispiel-date-an-stdcout-senden)
- [Vererbung](#vererbung-1)
  - [Mehrfachvererbung](#mehrfachvererbung)
  - [Initialisierung](#initialisierung)
  - [Sichtbarkeit](#sichtbarkeit)
  - [Object Slicing](#object-slicing)
  - [Probleme mit Vererbung und pass-by-value](#probleme-mit-vererbung-und-pass-by-value)
  - [Virtual](#virtual)
  - [Abstrakte Klassen](#abstrakte-klassen)
- [Argument Dependent Lookup (ADL)](#argument-dependent-lookup-adl)
- [Enums](#enums)
  - [Wert festlegen](#wert-festlegen)
  - [Typ festlegen](#typ-festlegen)
- [Contract/Exceptions](#contractexceptions)
  - [Exceptions](#exceptions)
  - [Exceptions abfragen mit CUTE](#exceptions-abfragen-mit-cute)
- [Templates](#templates)
  - [Function Templates](#function-templates)
  - [Concepts](#concepts)
  - [Overloads](#overloads)
  - [Variadic Template Function](#variadic-template-function)
  - [Class Templates](#class-templates)
    - [Erben in Template Classes](#erben-in-template-classes)
    - [Spezialisierung](#spezialisierung)
    - [Template Terminologie](#template-terminologie)
    - [Sack mit Initializer List füllen](#sack-mit-initializer-list-f%C3%BCllen)
    - [Container variieren](#container-variieren)
    - [Templates als Adapter](#templates-als-adapter)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Variablen

``<type> <name> {<value>};``

```C++
int anAnswer{42};
int const zero{};
```

Initialisierung kann weggelassen werden, wird aber nicht empfohlen. Leere Klammern bedeuten Default-Initialisierung
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

## bool
In Bedingungen können alle Werte stehen, die zu bool oder Zahlen konvertiert werden können. Dabei gilt der Wert 0 als false.

## Literale
* U/L für Integer (unsigned/long), Gross-/Kleinschreibung egal
* Exponenten mit E für float/double
* "ab"s macht einen String aus "ab", benötigt ``using namespace std::literals``

```C++
void sayHello(){
	using namespace std::literals;
	out << "hello"s;
}
```


# Operatoren

* Wie aus Java bekannt
* ``and, or, not`` sind alternative Schreibweisen für ``&&, ||, !``
* ``bitand, bitor, xor`` sind ``&, |, ^``

## Reihenfolge

Achtung: in einer einzelnen Expression, wenn die Funktionsaufrufe nur durch Komma getrennt sind, ist die Reihenfolge undefiniert.
<table>
  <tr>
     <th style="text-align: left"> Precedence</th>
     <th style="text-align: left"> Operator</th>
     <th style="text-align: left"> Description</th>
     <th style="text-align: left"> Associativity</th>
  </tr>
  <tr>
     <th>1</th>
     <td><code>::</code></td>
     <td>Scope resolution</td>
     <td style="vertical-align: top" rowspan="6"> Left-to-right</td>
  </tr>
  <tr>
     <th rowspan="5"> 2</th>
     <td style="border-bottom-style: none"> <code>a++</code>&nbsp;&nbsp; <code>a--</code></td>
     <td style="border-bottom-style: none"> Suffix/postfix increment and decrement</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code><i>type</i>()</code>&nbsp;&nbsp; <code><i>type</i>{}</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Functional cast</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>a()</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Function call</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>a[]</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Subscript</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>.</code>&nbsp;&nbsp; <code>-&gt;</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Member access</td>
  </tr>
  <tr>
     <th rowspan="9"> 3</th>
     <td style="border-bottom-style: none"> <code>++a</code>&nbsp;&nbsp; <code>--a</code></td>
     <td style="border-bottom-style: none"> Prefix increment and decrement</td>
     <td style="vertical-align: top" rowspan="9"> Right-to-left</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>+a</code>&nbsp;&nbsp; <code>-a</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Unary plus and minus</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>!</code>&nbsp;&nbsp; <code>~</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Logical NOT and bitwise NOT</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>(<i>type</i>)</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> C-style cast</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>*a</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Indirection (dereference)</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>&amp;a</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Address-of</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>sizeof</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Size-of<sup id="cite_ref-1">[note 1]</sup></td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>new</code>&nbsp;&nbsp; <code>new[]</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Dynamic memory allocation</td>
  </tr>
  <tr>
     <td style="border-top-style: none"> <code>delete</code>&nbsp;&nbsp; <code>delete[]</code></td>
     <td style="border-top-style: none"> Dynamic memory deallocation</td>
  </tr>
  <tr>
     <th>4</th>
     <td><code>.*</code>&nbsp;&nbsp; <code>-&gt;*</code></td>
     <td>Pointer-to-member</td>
     <td style="vertical-align: top" rowspan="12"> Left-to-right</td>
  </tr>
  <tr>
     <th>5</th>
     <td><code>a*b</code>&nbsp;&nbsp; <code>a/b</code>&nbsp;&nbsp; <code>a%b</code></td>
     <td>Multiplication, division, and remainder</td>
  </tr>
  <tr>
     <th>6</th>
     <td><code>a+b</code>&nbsp;&nbsp; <code>a-b</code></td>
     <td>Addition and subtraction</td>
  </tr>
  <tr>
     <th>7</th>
     <td><code>&lt;&lt;</code>&nbsp;&nbsp; <code>&gt;&gt;</code></td>
     <td>Bitwise left shift and right shift</td>
  </tr>
  <tr>
     <th rowspan="2"> 8</th>
     <td style="border-bottom-style: none"> <code>&lt;</code>&nbsp;&nbsp; <code>&lt;=</code></td>
     <td style="border-bottom-style: none"> For relational operators &lt; and ≤ respectively</td>
  </tr>
  <tr>
     <td style="border-top-style: none"> <code>&gt;</code>&nbsp;&nbsp; <code>&gt;=</code></td>
     <td style="border-top-style: none"> For relational operators &gt; and ≥ respectively</td>
  </tr>
  <tr>
     <th>9</th>
     <td><code>==</code>&nbsp;&nbsp; <code>!=</code></td>
     <td>For relational operators = and ≠ respectively</td>
  </tr>
  <tr>
     <th>10</th>
     <td><code>a&amp;b</code></td>
     <td>Bitwise AND</td>
  </tr>
  <tr>
     <th>11</th>
     <td><code>^</code></td>
     <td>Bitwise XOR (exclusive or)</td>
  </tr>
  <tr>
     <th>12</th>
     <td><code>|</code></td>
     <td>Bitwise OR (inclusive or)</td>
  </tr>
  <tr>
     <th>13</th>
     <td><code>&amp;&amp;</code></td>
     <td>Logical AND</td>
  </tr>
  <tr>
     <th>14</th>
     <td><code>||</code></td>
     <td>Logical OR</td>
  </tr>
  <tr>
     <th rowspan="7"> 15</th>
     <td style="border-bottom-style: none"> <code>a?b:c</code></td>
     <td style="border-bottom-style: none"> Ternary conditional<sup id="cite_ref-2">[note 2]</sup></td>
     <td style="vertical-align: top" rowspan="7"> Right-to-left</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>throw</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> throw operator</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>=</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Direct assignment (provided by default for C++ classes)</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>+=</code>&nbsp;&nbsp; <code>-=</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Compound assignment by sum and difference</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>*=</code>&nbsp;&nbsp; <code>/=</code>&nbsp;&nbsp; <code>%=</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Compound assignment by product, quotient, and remainder</td>
  </tr>
  <tr>
     <td style="border-bottom-style: none; border-top-style: none"> <code>&lt;&lt;=</code>&nbsp;&nbsp; <code>&gt;&gt;=</code></td>
     <td style="border-bottom-style: none; border-top-style: none"> Compound assignment by bitwise left shift and right shift</td>
  </tr>
  <tr>
     <td style="border-top-style: none"> <code>&amp;=</code>&nbsp;&nbsp; <code>^=</code>&nbsp;&nbsp; <code>|=</code></td>
     <td style="border-top-style: none"> Compound assignment by bitwise AND, XOR, and OR</td>
  </tr>
  <tr>
     <th>16</th>
     <td><code>,</code></td>
     <td>Comma</td>
     <td>Left-to-right</td>
  </tr>
</table>

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

## Parameter Passing - Return Values

* Pass by value: ``f(type par)`` (bevorzugt)
* Pass by const ref: ``f(type const & par)`` (bevorzugt wenn nichts verändert wird)
* Pass by reference: ``f(type & par)``

* Return by value: ``type f()``
* return by reference: ``type & f(); type const &g``

Achtung mit Referenzen. Wenn Parameter als Referenzen reinkommen, haben Änderungen darauf natürlich auch Einfluss auf die Originalvariable. Ebenso **NIE** eine lokale Variable als Referenz zurückgeben. Beim Stack abräumen geht diese flöten und die HSR brennt ab. Es sind **einzig** die eigenen Parameter wieder als Referenz zurückzugeben.

<table>
	<tr>
		<th></th> <th>value</th> <th>reference</th>
	</tr>
	<tr>
		<td>non-const</td>
		<td>
			Word(std::string value)<br><br>
			- Argument wird kopiert<br>
			- Änderungen im Wert haben keine Auswirkungen auf Aufrufer-Seite<br>
			- Für primitive und kleine Typen
		</td>
		<td>
			Word(std::string & value)<br>
			<br>
			- Argument wird aus dem Speicher as-is benutzt<br>
			- Änderungen im Wert haben Auswirkungen auf Aufrufer-Seite<br>
			- Benutzt wenn die Seiteneffekte gewünscht sind
		</td>
	</tr>
	<tr>
		<td>const</td>
		<td>
			Word(std::string const value)<br><br>
			- Argument wird kopiert<br>
			- Wert kann nicht verändert werden<br>
			- Für primitive und kleine Typen
		</td>
		<td>
			Word(std::string const & value)<br><br>
			- Argument wird aus dem Speicher as-is benutzt<br>
			- Wert kann nicht verändert werden<br>
			- Kann für grössere Objekte verändert werden
		</td>
	</tr>
</table>

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

## Aufruf Sequenz von Funktionen

```C++
void main() {
	askForName(std::cout);
	sayGreeting(std::cout, inputName(std::cin), inputName(std::cin));
}
```
Es ist nicht klar welche Funktion inputName zuerst aufgerufen wird.

# Include Files

Zu Beachten:

* Eigene Includes immer zuoberst!
* Funktionen in Header-Files typischerweise nur als Deklaration.
* Wenn kurze Funktion (oder Template): ``inline``
* Klassen-Member-Funktionen sowie Templates sind implizit inline.
* includes mit Anführungszeichen für includes aus dem gleichen Verzeichnis
* includes mit spitzen Klammer für includes aus Libraries und anderen Verzeichnissen

## Include Guard

Um die ODR (One Definition Rule) nicht zu verletzen, verwendet man Guard Statements
Include Guards werden vom Prepocesser bearbeitet. Dies bedeutet, dass man für Klassen/Methoden in verschiedenen Namespaces und Files, auch das Include Guard verschieden sein.

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

## Wichtige includes
```C++
#include <algorithm> // für alle Algorithms ausser die in <numeric>
#include <functional>
#include <iterator>
#include <numeric>
#include <exception> // für Standard Exceptions TODO Beispiel
#include <stdexcept> // TODO was ist der Unterschied zu exception?
#include <string> // std::string
// IO
#include <iosfwd> // Vorwärtsdeklaration von istream und ostream (nur typedefs ohne Methoden usw.), nur in Headers verwenden
#include <istream> // istream definition und implementation
#include <ostream> // ostream definition und implementation
#include <iostream> // istream, ostream und cin, cout
#include <sstream>> // string stream
```

# Kommandozeilenargumente übergeben
Main ist folgendermassen definiert:
``int main(int argc, char * argv[])``
``int main()``

``argc`` hat die Anzahl Argumente drin. ``argv[]`` ist ein Array von Char-Pointern (ein Array von "Strings"), also die einzelnen Argumente.

Die eigentlichen Argumente beginnen erst bei ``argv+1``, im ersten Element steht der Programmname. Es endet bei ``argv+argv``.

# Memory (Heap)
Man könnte das Memory selber mit ``new`` allozieren. Das ist aber pöse.

Ebenso sollte man nie plain-Pointers verwenden.

**``std::unique_ptr<T>`` und ``std::make_unique``**

Für unshared Heap-Memory, oder für lokales was auf den Heap muss (z.B. Stack zu klein).

```C++
std::unique_ptr<int> aFactory(int i) {
	return std::make_unique<int>(i);
}

auto answer = aFactory(42);
// Transfer of Ownership
auto answer2=std::move(answer)
```

Die Pointer haben immer nur einen Owner und können nicht kopiert werden (nur by value zurückgegeben werden)


Wenn man C-Pointer (z.B. von gewissen Funktionen) als unique_ptr verpackt, werden sie beim ``}`` automatisch ge-free-d.

Weil immer nur einem der Pointer gehört, wird irgendwann garantiert das Memory aufgeräumt.

**``std::shared_ptr<T>`` und ``std::make_shared<T>``**

Funktionieren mehr wie die Java-Referenzen. Können kopiert und umhergereicht werden, der letzte löscht das Licht aus und löscht das allozierte Objekt. ``make_shared<T>`` sorgt dafür, dass die public Konstruktorparameter von T benutzt werden können

Wenn etwas wirklich auf den Heap muss, Factory verwenden

```C++
#include <memory> // oder <boost/shared_ptr.hpp>
#include <string>
struct A {
	A(int a, std::string b, char c){}
};

std::shared_ptr<A> A_factory() {
	return std::make_shared<A>(5, "hi", 'a');
}

// Usage
int main() {
	auto an_a=A_factory();
	auto b = an_a; // second pointer to same object
	A c{*b} // copy constructor
	auto another = std::make(shared)<A>(c); // copy-constructor on heap
}
```

Wenn Instanzen einer Klassenhierarchie durch ``shared_ptr<base>`` repräsentiert werden, aber durch ``make_shared<concrete>()`` erstellt werden, muss der Destruktor nicht mehr virtual sein. shared_ptr mekrt sich den konkreten Destruktor.

Man kann in ein zirkuläres Dependency-Problem rennen. Um das zu umgehen, braucht man ``weak_ptr`` um diese zu brechen.

## Beispiel: Eltern und Kinder
``shared_ptr`` Zyklen: ``weak_ptr/shared_from_this``

Eine Personenklasse soll erstellt werden:

* jede Person kennt ihre Eltern (Vater, Mutter) wenn noch am Leben
* Jede Person kann verheiratet werden
* Jede Person kennt ihre Kinder
	* Mutter und Vater kennen beide ihre Kinder

Damit das sauber funktioniert:

* Alle lebenden Objekte in einer separaten Datenstruktur als ``shared_ptr``
* Abhängigkeiten durch ``weak_ptr``

Wenn man sie nun aus der Datenstruktur der lebenden Objekte entfernt, werden die Objekte zerstört. Der ``weak_ptr`` hat zwar eine Referenz, muss aber konkretisiert werden (zu einem ``shared_ptr``) und bei diesem Schritt kann erkannt werden dass das eigentliche Objekt weg ist.

==> [Biblische Familie](BiblischeFamilie.pdf)


# Move
Streams können nicht kopiert werden, aber "gemovet". Dabei werden sie wie kopiert, aber die Innereien werden rausgerissen". Die alte Variable ist dann unbrauchbar.

Move-Constructor: ``std::ofstream(std::ofstream &&)``


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

# CUTE TODO ist das nötig?
CUTE kennt viele Makros und man sollte sie auch nutzen. Will man zum Beispiel die relationalen Operatoren prüfen, dann nicht einfach ``ASSERT_EQUAL(true, a < b)`` sondern ``ASSERT_LESS(a, b)``.


# Using

* "Alias" mit ``using input=std::istream_iterator<std::string>``
* Parent-Members in Namespace übernehmen, z.B. Konstruktoren mit ``using std::set<T, COMPARE>::set;``
* Funktionen eines Namespaces in den scope übernehmen z.B. ``using namespace std``

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

<style>
	.strong-border-left {
		border-left: #202020 3px solid;
	}
</style>
<table  style="font-size:85%; text-align:center;">
  <tr>
    <td colspan="3"> <tt>ios_base::iostate</tt> flags</td>
    <td colspan="6" class="strong-border-left"> <code>basic_ios</code> accessors</td></tr>
  <tr>
    <td>eofbit</td>
    <td>failbit</td>
    <td>badbit</td>
    <td class="strong-border-left"><strong><tt>good()</tt></strong></td>
    <td><tt>fail()</tt></td>
    <td><tt>bad()</tt></td>
    <td><tt>eof()</tt></td>
    <td><tt>operator bool</tt></td>
    <td><tt>operator!</tt></td></tr>
  <tr>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td class="strong-border-left" style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
  </tr>
  <tr>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td></tr>
  <tr>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td></tr>
  <tr>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td></tr>
  <tr>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td></tr>
  <tr>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td></tr>
  <tr>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td></tr>
  <tr>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td class="strong-border-left" style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
    <td style="background:#ff9090; color:black; vertical-align: middle; text-align: center;"> false</td>
    <td style="background: #90ff90; color: black; vertical-align: middle; text-align: center;">true</td>
  </tr>
</table>



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

**``.read()`` implementieren**
Precondition: std::istream ist im .good()-State. Wenn wir kein Datum extrahieren können, setzen wir std::istream in den fail-State.

Wenn der Input nicht verwendet werden kann, wird das Objekt nicht überschrieben.

**Header:**
```C++
class Date {
  int year, month, day;
public:
	std::istream & read(std::istream & is) {
		int year{-1}, month{-1}, day{-1};
		char sep1, sep2;
		//read values
		is >> year >> sep1 >> month >> sep2 >> day;
		try {
			Date input{year, month, day};
			//overwrite content of this object (copy-ctor)
			(*this) = input;
			//clear stream if read was ok
			is.clear();
		} catch (std::out_of_range & e) {
			//set failbit
			is.setstate(std::ios::failbit | is.rdstate());
		}
		return is; }
	}
};

inline std::istream & operator>> std::istream & is, Date & date) {
	return date.read(is);
}
```

# Iterators

Include für alle Iterators: ``#include <iterator>``

* Jeder Container bietet Iteratoren
* Es gibt immer ein Paar von Iteratoren, ``begin(v)`` und ``end(v)``
* Es gibt die "allgemeine" Version wie oben, oder die spezialisierte Version ``v.begin()/v.end()``. Im Zweifelsfall die spezialisierte Version verwenden.
* C++-Iteratoren kennen das Ende nicht. Man kann aber gegen das Ende vergleichen ``iterator != v.end()``
* Auf Elemente mittels ``*`` zugreifen ``*iterator``
* Nächster Schritt des Iterators: ``++iterator``

Achtung, das Ende ist **vor** ``end``.

Um read-only zu garantieren sollte ``cbegin()/cend()`` verwendet werden. Wenn man von "hinten" beginnen möchte gibts ``rbegin()/rend()``.

Wenn man Iteratoren speichern will, am besten Typ ``auto``.

Beispiele:

```C++
void testRIterators() {
	std::vector<int> v {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	auto it = v.rbegin();
	it++;
	ASSERT_EQUAL(*it, 9);
}

int main() {
 std::vector<int> v {1,2,3,4,5,6,7,8,9};
 std::cout << *(--v.end()) << ", " << *(--v.rend());
 // Output 9, 1
}

int main() {
 std::vector<int> v {1,2,3,4,5,6,7,8,9};
 std::cout << *v.begin() << ", "<< *(--v.end())
   << ", " << *v.rbegin() << ", " << *(--v.rend());
 // Output 1, 9, 9, 1
}
```


Liste des Bösen

* Eigene Loops

## Spezielle Iteratoren für I/O
**``std::ostream_iterator<T>`` gibt Werte vom Typ ``T`` an den gegebenen ``std::ostream`` aus**
Endet wenn Input-Range fertig ist
``copy(begin(v), end(v), std::ostream_iterator<int>{std::cout, ", "});``

**``std::istream_iterator<T>`` liest Werte vom Typ ``T``vom gegebenen ``std::istream``**
Endet wenn der istream nicht länger ``good`` ist

Diese nutzen aber beide intern den ``operator>>`` für Input. Der überspringt Leerzeichen und White Spaces.
Für eine perfekte Kopie bräuchten wir auch den Rest. Dies geht mit ``istreambuf_iterator<char>``.

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

**Input Iteratoren**

* Das aktuelle Element kann nur einmal ausgelesen werden, der Iterator muss danach inkrementiert werden -- TODO strikethrough, das war anscheinend falsch (Errata in Vorlesung 11)
* Kann Iteratoren vergleichen

**Forward Iterator**

* Das Element kann gelesen und verändert werden (ausser der Container oder die Elemente sind const)
* Kann nicht rückwärts lesen
* Der Iterator kann aber kopiert werden für spätere Referenz

**Bidirectional Iterator**
* Das Element kann gelesen und verändert werden (ausser...)
* Kann vorwärts und rückwärts gehen
* Random Access sind bidirectional, aber können auch indexen

**Output Iterator**

* Kann einen Wert schreiben, aber nur einmal und muss danach inkrementiert werden

## Spezialfunktionen
Mit ``distance`` kann man die Anzahl Hops zählen, bis man zum anderen Iterator kommt. Mit ``advance`` kann man n Male hüpfen.


<table>
    <tr>
       <th colspan="4">category</th>
       <th>properties</th>
       <th>valid expressions</th>
    </tr>
    <tr>
       <td colspan="4" rowspan="2">all categories</td>
       <td><i> copy-constructible </i>, <i> copy-assignable  and <i> destructible </i></i></td>
       <td><code>X b(a);<br>
          b = a;</code>
       </td>
    </tr>
    <tr>
       <td>Can be incremented</td>
       <td><code>++a<br>
          a++</code>
       </td>
    </tr>
    <tr>
       <td rowspan="10"> Random Access </td>
       <td rowspan="6"> Bidirectional </td>
       <td rowspan="5"> Forward </td>
       <td rowspan="2"> Input </td>
       <td>Supports equality/inequality comparisons</td>
       <td><code>a == b<br>
          a != b</code>
       </td>
    </tr>
    <tr>
       <td>Can be dereferenced as an <i>rvalue</i></td>
       <td><tt>*a<br>
          a-&gt;m</tt>
       </td>
    </tr>
    <tr>
       <td> Output </td>
       <td>Can be dereferenced as an <i>lvalue</i> <br>
          (only for <i>mutable iterator types</i>)
       </td>
       <td><tt>*a = t<br>
          *a++ = t</tt>
       </td>
    </tr>
    <tr>
       <td rowspan="2"></td>
       <td><i> default-constructible </i></td>
       <td><tt>X a;<br>
          X()</tt>
       </td>
    </tr>
    <tr>
       <td>Multi-pass: neither dereferencing nor incrementing affects dereferenceability</td>
       <td><code>{ b=a; *a++; *b; }</code> </td>
    </tr>
    <tr>
       <td colspan="2"></td>
       <td>Can be decremented</td>
       <td><tt>--a<br>
          a--<br>
          *a--</tt>
       </td>
    </tr>
    <tr>
       <td colspan="3" rowspan="4"></td>
       <td>Supports arithmetic operators <tt>+</tt> and <tt>-</tt></td>
       <td><tt>a + n<br>
          n + a<br>
          a - n<br>
          a - b</tt>
       </td>
    </tr>
    <tr>
       <td>Supports inequality comparisons (<tt>&lt;</tt>, <tt>&gt;</tt>, <tt>&lt;=</tt> and <tt>&gt;=</tt>) between iterators</td>
       <td><tt>a &lt; b<br>
          a &gt; b<br>
          a &lt;= b<br>
          a &gt;= b</tt>
       </td>
    </tr>
    <tr>
       <td>Supports compound assignment operations <tt>+=</tt> and <tt>-=</tt></td>
       <td><tt>a += n<br>
          a -= n</tt>
       </td>
    </tr>
    <tr>
       <td>Supports offset dereference operator (<tt>[]</tt>)</td>
       <td><tt>a[n]</tt></td>
    </tr>
</table>

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

Wrapper über Klassischen C array mit length Feld

**Include/Initialisieren**: ``#include <array>/std::array<int, 6> a{{1, 2, 3, 4, 5}}; // 6=size, Achtung doppelte geschw. Klammern!``

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

Entspricht TreeSet in Java, aufsteigend sortiert ohne Duplikate (Sortierung als zweiter Template-Parameter möglich)

**Include / Initialisieren**: ``#include <set> / std::set<int> s{}; ODER AUCH std::set<int, std::less> s{}``

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


# Nackte Arrays
Wenn man mit nackten Arrays arbeitet, kann man diese trotzdem mit Iteratoren verwenden:

* Anfang: ``array``
* Ende: ``array+size``.

## Initialisieren
```C++
int five[5]{}; // 5 zeros
int four[4] = {1, 2, 3, 5};
doule d[4]{1.0, 2.0}; // d[2] = d[3] = 0.0
double m[2][3]{ {1, 2, 3} , {4, 5, 6} };
char s[6]{"hello"}; // 5 chars + '\0'
```
Achtung: mehrdimensionale Arrays nicht mit Komma schreiben. Der ``operator,`` evaluiert alle Subexpressions sequentiell und ist nur nützlich, wenn der erste Teil einen Seiteneffekt hat.

## Länge erkennen mit Type Deduction

```C++
template <typename T, unsigned N>
// () benötigt
void printArray(std::ostream & out, T const (&x)[N]) {
	copy(x, x+N, std::ostream_iterator<T>{out, ", "});
}
```

Die Type Deduction presst das dann so rein dass in N die Grösse steht. Wenn man ein Array mittels Liste erstellt, wird die Grösse automatisch festgelegt.

**Nackte Arrays sollten nicht verwendet werden. Ersetzen mit ``std::array``**


# Lambdas

Wenn es alle möglichen Klammern hat, ist es wahrscheinlich ein Lambda.

```C++
[lambda_capture]
(parameters)->return_type {
	statements
}
```

Beispiel:
```C++
auto const g=[](char c)->{return std::toupper(c);}; // beide Semikolon nicht vergessen!``
g('a');
```

* Parameter sind wie Funktionsparameter, auto möglich. Klammer kann auch weggelassen werden
* return_type kann weggelassen werden wenn void oder die return-Statements im Typ konsistent sind (Compiler erkennts). **Dann aber auch den Pfeil weglassen**.


## Captures und Parameter

* Capture benennt Variablen die vom umgebenden Scope genommen werden oder erstellt sogar neue
	* ``=`` copy
	* ``&`` reference
	* ``var=value`` erzeugt neue Capture-Variable mit Wert
	* Können auch kombiniert werden
		* ``=, &out`` capturet alle by copy, aber ``out`` by reference
		* ``&,=x`` capturet alle by reference, aber x by copy/value
		* Guideline: alle Captures explizit machen
	* Typ wird abgeleitet
	* Wenn man Member-Variablen captured, sind diese automatisch by reference, auch wenn man ``[=]`` automatisch by copy macht.
		* this ist schon eine Reference
* Parameter sind wie Funktionsparameter, auto möglich
* **Captures werden zur Definitionszeit festgelegt, Parameter zur Aufrufzeit**

```C++
void testLambdaCapture() {
	int n{5};
	auto lambdaCapture=[n](){return n;};
	n++;
	ASSERT_EQUAL(lambdaCapture(), 5);
	ASSERT_EQUAL(n, 6);
}

void testLambdaCaptureReference() {
	int n{5};
	auto lambdaCapture=[&n](){return n;};
	n++;
	ASSERT_EQUAL(lambdaCapture(), 6);
	ASSERT_EQUAL(n, 6);
}

void testLambdaParameter() {
	int n{5};
	auto lambdaCapture=[](int m){return m;};
	n++;
	ASSERT_EQUAL(lambdaCapture(n), 6);
	ASSERT_EQUAL(n, 6);
}
```
### Spezialfall mutable
Variables die by copy gecaptured werden sind im Lambda const, ausser wenn das Lambda als ``mutable`` gekennzeichnet wird. Das Lambda bekommt seine eigene Kopie der Variable oder das Lambda definiert die Variable gleich neu.

# Functor
Ein Functor ist eine Klasse, die einen Call-Operator hat: ``operator()``. Gegenüber einer normalen Funktion hat sie Speicher zur Verfügung, der auch bleibt.

Der Functor kann auch gleich mit ``donothingfunctor{}()`` konstruiert und gleich aufgerufen werden.

```C++
struct Accumulator {
	int count{0};
	int accumulated_value{0};
	void operator()(int value) {
		count++;
		accumulated_value += value;
	}
	int average() const;
	int sum() const;
}

// Achtung nicht Implementation von oberem
int average(std::vector<int> values) {
	Accumulator acc{};
	for(auto v: values) { acc(v); }
	return acc.average();
}
```

Lambdas werden intern mit Functors realisiert.

Es gibt ein paar Standard Functors, wie z.B. ``std::greater<>{}`` in ``<functional>``.

Binary Arithmetic and logical

* ``plus<>`` (+)
* ``minus<>`` (-)
* ``divides<>`` (/)
* ``multiplies<>`` (``*``)
* ``modulus<>`` (%)
* ``logical_and<>`` (&&)
* ``logical_or<>`` (||)

Unary

* ``negate<>`` (-)
* ``logcal_not<>`` (!)

Binary Comparison

* ``less<>`` (<)
* ``less_equal<>`` (<=)
* ``equal_to<>`` (==)
* ``greater_equal<>`` (>=)
* ``greater<>`` (>)
* ``not_equal_to<>`` (!=)

Diese lassen sich dann in manchen Containern als Vergleichsoperator mitgeben, allerdings nur wenn wie irreflexiv und transitiv sind (ah ja.)

### Als Parameter
``std::function<SIGNATURE>;``, also zum Beispiel ``std::function<bool(int)> apredicate{};``

Die Signatur wird geprüft.

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

## Ranges
Algorithmen brauchen oft Ranges, die man mit Iteratoren angibt.

## Beispiele aus Vorlesung
**transform**
Eine Range zu neuen Values umwandeln, oder zwei Ranges derselben Grösse.
Input: ``counts``: 3, 0, 1, 4, 0, 2
Input: ``letters``: g, a, u, y, f, o
Output: ggg, , u, yyyy, , oo

```C++
std::vector<std::string> combined{};
auto times = [](int i, char c) {return std::string(i, c);};
std::transform(begin(counts), end(counts), begin(letters), std::back_inserter(combined), times)
```

**merge**
Zwei Ranges sortiert mergen

**accumulate**
Kann auch so gewürgt werden dass es nicht-numerisches unterstützt, z.B. Listen.

**Erase/Remove**
Wenn man Elemente löschen will, so macht man das immer in zwei Teilen

1. Finden, was zu löschen ist. Technisch: alles zu Löschende ans Ende verscbieben und Iterator auf erstes zu löschendes Element zurückgeben: ``remove``
2. Danach löscht man von diesem gegebenen Iterator bis zum ``end``: ``erase`

```C++
// removes all elements with the value 5
v.erase( std::remove( v.begin(), v.end(), 5 ), v.end() );
```

## Suffix-Versionen
**``_if``**
Von manchen gibts noch eine ``_if``-Version, die noch ein Predicate nimmt

**``_n``**
Vielfach statt dem ``last``-Iterator einfach eine Angabe wie oft.

## Heap-Algorithmen
Bauen die Container so um, dass sie einem Heap entsprechen

**Operationen**

* ``make_heap``: 3*N Vergleiche
* ``pop_heap``: 2*log(N) Vergleiche
* ``push_heap``: log(N) Vergleiche
* ``sort_heap``: N*log(N) Vergleiche

## Fallen

* Die Iteratoren müssen natürlich zum selben Range gehören, sonst brennt die HSR ab.
* Bei den copy-Algorithmen muss genügend Platz im Ziel sein, sonst brennt die HSR ab. Wenn man sich nicht darum kümmern will: ``back_inserter``, ``front_inserter`` oder ``inserter``.
* Manche Operationen machen die Iteratoren ungültig, zum Beispiel ein Push-Back auf einem Vector. Der end-Iterator zeigt dann nicht mehr auf den richtigen Ort.

## Tabelle
<table>
<tbody><tr>
<td colspan="2"> <h5> Non-modifying sequence operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> all_of any_of none_of </div><div> (C++11) (C++11) (C++11) </div></div>
</td>
<td>checks if a predicate is true for all, any or none of the elements in a range
</td></tr>

<tr>
<td><div><div> for_each </div></div>
</td>
<td>applies a function to a range of elements
</td></tr>

<tr>
<td><div><div> for_each_n </div><div> (C++17) </div></div>
</td>
<td>applies a function object to the first n elements of a sequence
</td></tr>

<tr>
<td><div><div> count count_if </div></div>
</td>
<td>returns the number of elements satisfying specific criteria
</td></tr>

<tr>
<td><div><div> mismatch </div></div>
</td>
<td>finds the first position where two ranges differ
</td></tr>

<tr>
<td><div><div> equal </div></div>
</td>
<td>determines if two sets of elements are the same
</td></tr>

<tr>
<td><div><div> find find_if find_if_not </div><div> (C++11) </div></div>
</td>
<td>finds the first element satisfying specific criteria
</td></tr>

<tr>
<td><div><div> find_end </div></div>
</td>
<td>finds the last sequence of elements in a certain range
</td></tr>

<tr>
<td><div><div> find_first_of </div></div>
</td>
<td>searches for any one of a set of elements
</td></tr>

<tr>
<td><div><div> adjacent_find </div></div>
</td>
<td>finds the first two adjacent items that are equal (or satisfy a given predicate)
</td></tr>

<tr>
<td><div><div> search </div></div>
</td>
<td>searches for a range of elements
</td></tr>

<tr>
<td><div><div> search_n </div></div>
</td>
<td>searches for a number consecutive copies of an element in a range
</td></tr>


<tr>
<td colspan="2"> <h5> Modifying sequence operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> copy copy_if </div><div> (C++11) </div></div>
</td>
<td>copies a range of elements to a new location
</td></tr>

<tr>
<td><div><div> copy_n </div><div> (C++11) </div></div>
</td>
<td>copies a number of elements to a new location
</td></tr>

<tr>
<td><div><div> copy_backward </div></div>
</td>
<td>copies a range of elements in backwards order
</td></tr>

<tr>
<td><div><div> move </div><div> (C++11) </div></div>
</td>
<td>moves a range of elements to a new location
</td></tr>

<tr>
<td><div><div> move_backward </div><div> (C++11) </div></div>
</td>
<td>moves a range of elements to a new location in backwards order
</td></tr>

<tr>
<td><div><div> fill </div></div>
</td>
<td>copy-assigns the given value to every element in a range
</td></tr>

<tr>
<td><div><div> fill_n </div></div>
</td>
<td>copy-assigns the given value to N elements in a range
</td></tr>

<tr>
<td><div><div> transform </div></div>
</td>
<td>applies a function to a range of elements
</td></tr>

<tr>
<td><div><div> generate </div></div>
</td>
<td>assigns the results of successive function calls to every element in a range
</td></tr>

<tr>
<td><div><div> generate_n </div></div>
</td>
<td>assigns the results of successive function calls to N elements in a range
</td></tr>

<tr>
<td><div><div> remove remove_if </div></div>
</td>
<td>removes elements satisfying specific criteria
</td></tr>

<tr>
<td><div><div> remove_copy remove_copy_if </div></div>
</td>
<td>copies a range of elements omitting those that satisfy specific criteria
</td></tr>

<tr>
<td><div><div> replace replace_if </div></div>
</td>
<td>replaces all values satisfying specific criteria with another value
</td></tr>

<tr>
<td><div><div> replace_copy replace_copy_if </div></div>
</td>
<td>copies a range, replacing elements satisfying specific criteria with another value
</td></tr>

<tr>
<td><div><div> swap </div></div>
</td>
<td>swaps the values of two objects
</td></tr>

<tr>
<td><div><div> swap_ranges </div></div>
</td>
<td>swaps two ranges of elements
</td></tr>

<tr>
<td><div><div> iter_swap </div></div>
</td>
<td>swaps the elements pointed to by two iterators
</td></tr>

<tr>
<td><div><div> reverse </div></div>
</td>
<td>reverses the order of elements in a range
</td></tr>

<tr>
<td><div><div> reverse_copy </div></div>
</td>
<td>creates a copy of a range that is reversed
</td></tr>

<tr>
<td><div><div> rotate </div></div>
</td>
<td>rotates the order of elements in a range
</td></tr>

<tr>
<td><div><div> rotate_copy </div></div>
</td>
<td>copies and rotate a range of elements
</td></tr>

<tr>
<td><div><div> random_shuffle shuffle </div><div> (until C++17) (C++11) </div></div>
</td>
<td>randomly re-orders elements in a range
</td></tr>

<tr>
<td><div><div> sample </div><div> (C++17) </div></div>
</td>
<td>selects n random elements from a sequence
</td></tr>

<tr>
<td><div><div> unique </div></div>
</td>
<td>removes consecutive duplicate elements in a range
</td></tr>

<tr>
<td><div><div> unique_copy </div></div>
</td>
<td>creates a copy of some range of elements that contains no consecutive duplicates
</td></tr>


<tr>
<td colspan="2"> <h5> Partitioning operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> is_partitioned </div><div> (C++11) </div></div>
</td>
<td>determines if the range is partitioned by the given predicate
</td></tr>

<tr>
<td><div><div> partition </div></div>
</td>
<td>divides a range of elements into two groups
</td></tr>

<tr>
<td><div><div> partition_copy </div><div> (C++11) </div></div>
</td>
<td>copies a range dividing the elements into two groups
</td></tr>

<tr>
<td><div><div> stable_partition </div></div>
</td>
<td>divides elements into two groups while preserving their relative order
</td></tr>

<tr>
<td><div><div> partition_point </div><div> (C++11) </div></div>
</td>
<td>locates the partition point of a partitioned range
</td></tr>


<tr>
<td colspan="2"> <h5> Sorting operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> is_sorted </div><div> (C++11) </div></div>
</td>
<td>checks whether a range is sorted into ascending order
</td></tr>

<tr>
<td><div><div> is_sorted_until </div><div> (C++11) </div></div>
</td>
<td>finds the largest sorted subrange
</td></tr>

<tr>
<td><div><div> sort </div></div>
</td>
<td>sorts a range into ascending order
</td></tr>

<tr>
<td><div><div> partial_sort </div></div>
</td>
<td>sorts the first N elements of a range
</td></tr>

<tr>
<td><div><div> partial_sort_copy </div></div>
</td>
<td>copies and partially sorts a range of elements
</td></tr>

<tr>
<td><div><div> stable_sort </div></div>
</td>
<td>sorts a range of elements while preserving order between equal elements
</td></tr>

<tr>
<td><div><div> nth_element </div></div>
</td>
<td>partially sorts the given range making sure that it is partitioned by the given element
</td></tr>


<tr>
<td colspan="2"> <h5> Binary search operations (on sorted ranges) </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> lower_bound </div></div>
</td>
<td>returns an iterator to the first element <i>not less</i> than the given value
</td></tr>

<tr>
<td><div><div> upper_bound </div></div>
</td>
<td>returns an iterator to the first element <i>greater</i> than a certain value
</td></tr>

<tr>
<td><div><div> binary_search </div></div>
</td>
<td>determines if an element exists in a certain range
</td></tr>

<tr>
<td><div><div> equal_range </div></div>
</td>
<td>returns range of elements matching a specific key
</td></tr>


<tr>
<td colspan="2"> <h5> Set operations (on sorted ranges) </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> merge </div></div>
</td>
<td>merges two sorted ranges
</td></tr>

<tr>
<td><div><div> inplace_merge </div></div>
</td>
<td>merges two ordered ranges in-place
</td></tr>

<tr>
<td><div><div> includes </div></div>
</td>
<td>returns true if one set is a subset of another
</td></tr>

<tr>
<td><div><div> set_difference </div></div>
</td>
<td>computes the difference between two sets
</td></tr>

<tr>
<td><div><div> set_intersection </div></div>
</td>
<td>computes the intersection of two sets
</td></tr>

<tr>
<td><div><div> set_symmetric_difference </div></div>
</td>
<td>computes the symmetric difference between two sets
</td></tr>

<tr>
<td><div><div> set_union </div></div>
</td>
<td>computes the union of two sets
</td></tr>


<tr>
<td colspan="2"> <h5> Heap operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> is_heap </div><div> (C++11) </div></div>
</td>
<td>checks if the given range is a max heap
</td></tr>

<tr>
<td><div><div> is_heap_until </div><div> (C++11) </div></div>
</td>
<td>finds the largest subrange that is a max heap
</td></tr>

<tr>
<td><div><div> make_heap </div></div>
</td>
<td>creates a max heap out of a range of elements
</td></tr>

<tr>
<td><div><div> push_heap </div></div>
</td>
<td>adds an element to a max heap
</td></tr>

<tr>
<td><div><div> pop_heap </div></div>
</td>
<td>removes the largest element from a max heap
</td></tr>

<tr>
<td><div><div> sort_heap </div></div>
</td>
<td>turns a max heap into a range of elements sorted in ascending order
</td></tr>


<tr>
<td colspan="2"> <h5> Minimum/maximum operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;algorithm&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> max </div></div>
</td>
<td>returns the greater of the given values
</td></tr>

<tr>
<td><div><div> max_element </div></div>
</td>
<td>returns the largest element in a range
</td></tr>

<tr>
<td><div><div> min </div></div>
</td>
<td>returns the smaller of the given values
</td></tr>

<tr>
<td><div><div> min_element </div></div>
</td>
<td>returns the smallest element in a range
</td></tr>

<tr>
<td><div><div> minmax </div><div> (C++11) </div></div>
</td>
<td>returns the smaller and larger of two elements
</td></tr>

<tr>
<td><div><div> minmax_element </div><div> (C++11) </div></div>
</td>
<td>returns the smallest and the largest elements in a range
</td></tr>

<tr>
<td><div><div> clamp </div><div> (C++17) </div></div>
</td>
<td>clamps a value between a pair of boundary values
</td></tr>

<tr>
<td><div><div> lexicographical_compare </div></div>
</td>
<td>returns true if one range is lexicographically less than another
</td></tr>

<tr>
<td><div><div> is_permutation </div><div> (C++11) </div></div>
</td>
<td>determines if a sequence is a permutation of another sequence
</td></tr>

<tr>
<td><div><div> next_permutation </div></div>
</td>
<td>generates the next greater lexicographic permutation of a range of elements
</td></tr>

<tr>
<td><div><div> prev_permutation </div></div>
</td>
<td>generates the next smaller lexicographic permutation of a range of elements
</td></tr>


<tr>
<td colspan="2"> <h5> Numeric operations </h5>
</td></tr>

<tr>
<td colspan="2"> <div>Defined in header <code>&lt;numeric&gt;</code> </div>
</td></tr>

<tr>
<td><div><div> iota </div><div> (C++11) </div></div>
</td>
<td>fills a range with successive increments of the starting value
</td></tr>

<tr>
<td><div><div> accumulate </div></div>
</td>
<td>sums up a range of elements
</td></tr>

<tr>
<td><div><div> inner_product </div></div>
</td>
<td>computes the inner product of two ranges of elements
</td></tr>

<tr>
<td><div><div> adjacent_difference </div></div>
</td>
<td>computes the differences between adjacent elements in a range
</td></tr>

<tr>
<td><div><div> partial_sum </div></div>
</td>
<td>computes the partial sum of a range of elements
</td></tr>

<tr>
<td><div><div> reduce </div><div> (C++17) </div></div>
</td>
<td>similar to std::accumulate , except out of order
</td></tr>

<tr>
<td><div><div> exclusive_scan </div><div> (C++17) </div></div>
</td>
<td>similar to std::partial_sum , excludes the ith input element from the ith sum
</td></tr>

<tr>
<td><div><div> inclusive_scan </div><div> (C++17) </div></div>
</td>
<td>similar to std::partial_sum , includes the ith input element in the ith sum
</td></tr>

<tr>
<td><div><div> transform_reduce </div><div> (C++17) </div></div>
</td>
<td>applies a functor, then reduces out of order
</td></tr>

<tr>
<td><div><div> transform_exclusive_scan </div><div> (C++17) </div></div>
</td>
<td>applies a functor, then calculates exclusive scan
</td></tr>

<tr>
<td><div><div> transform_inclusive_scan </div><div> (C++17) </div></div>
</td>
<td>applies a functor, then calculates inclusive scan
</td></tr>
</tbody></table>


# Klassen

Es gibt zwei Keywords, ``struct``und ``class``. Die sind äquivalent, ausser

* struct ist standardmässig public
* class ist standardmässig private

Eine gute Klasse kennt eine Klasseninvariante, d.h. dass eine Instanz sich immer in einem guten Zustand befindet. Falls eine Änderung diese Invarianz verletzt, wird sie^ entweder zurückgerollt oder zerstört. Aber nicht im FUBAR-Zustand belassen.

## Beispielklasse Date

```C++
#ifndef DATE_H_
#define DATE_H_
class Date {
	int year, month, day;
public:
	Date(int year, int month, int day)
	: year{year}, month{month}, day{day} // initialzer list
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
}; //Semikolon am Ende der Klassendefinition nicht vergessen!

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
	read(in); // read muss selber implementiert werden
	if(in.fail)) {
		throw std::out_of_range("invalid date");
	}
}
```

Man könnte natürlich auch eine Factory-Funktion machen.

### Konstruktoren wieder default machen/löschen
Wenn man einen eigenen Konstruktur gemacht hat, ist der Default-Konstruktor weg. Mit folgendem Befehl kann man ihn einfach wieder definieren:

``<constructor-name>() = default;``

Ebenso kann man Konstruktoren löschen:

``<constructor-name>() = delete;``


## Destruktoren
Genannt wie der Default-Konstruktor mit einem ~ zu Beginn:
``~Date();``

Muss alle Ressourcen freigeben. Implizit verfügbar. Darf keine Exception werfen! Wird automatisch am Ende des Blocks für alle lokalen Instanzen aufgerufen.

TODO muss bei Klassen mit virtual Methoden immer virtual deklariert werden.

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
Dürfen die Invariance nicht verletzen.

Es gibt das implizite ``this``-Objekt. ``this`` ist ein Pointer und muss darum vor dem Zugriff dereferenziert werden. Entweder mit ``(*this).day`` oder mit ``this->day``. Wenn es im Scope der Methode nicht noch eine andere Variable "day" gibt kann auch einfach mit ``day`` darauf zugegriffen werden..

In einer const-Memberfunktion dürfen die Member nicht verändert werden. Es können nur const-Member aufgerufen werden.

### Static Member-Funktionen
Es gibt kein this-Objekt, können **nicht** const sein. Kein static Keyword.

Aufruf: ``<classname>::<member>(): Date::isLeapYear(2016);``

## Operator-Overloading
> When in doubt, do as the ints do

Wie eine Funktion deklariert, allerdings als mit speziellem Namen: ``<returntype> operator<op>(<parameters>);

Unäre/binäre Parameter haben einen bzw. zwei Parameter.

**Überladbare Operatoren**:

<table>
	<tr>
		<td>+</td> <td>-</td> <td>*</td> <td>/</td> <td>%</td> <td>^</td>
	</tr>
	<tr>
		<td>&</td> <td>|</td> <td>~</td> <td>!</td> <td>,</td> <td>=</td>
	</tr>
	<tr>
		<td><</td> <td>></td> <td><=</td> <td>>=</td> <td>++</td> <td>--</td>
	</tr>
	<tr>
		<td><<</td> <td>>></td> <td>==</td> <td>!=</td> <td>&&</td> <td>||</td>
	</tr>
	<tr>
		<td>+=</td> <td>-=</td> <td>/=</td> <td>%=</td> <td>^=</td> <td>&=</td>
	</tr>
	<tr>
		<td>|=</td> <td>*=</td> <td><<=</td> <td>>>=</td> <td>[]</td> <td>()</td>
	</tr>
	<tr>
		<td>-></td> <td>->*</td> <td>new</td> <td>new[]</td> <td>delete</td> <td>delete[]</td>
	</tr>
</table>

**Nicht überladbare Operatoren**

<table>
	<tr>
		<td>::</td> <td>.*</td><td>.</td><td>: ?  (ternär)</td>
	</tr>
</table>

### Beispiel: Date vergleichbar machen
year, month und day vergleichen. Wir nutzen den ``operator<``. Immer const &! Rückgabewert ``bool``. Achtung, hier wird eine Entwicklung gezeigt, damit man den Unterschied sieht.

Erste Variante: als freier Operator (in Klassenfile, aber unterhalb Klassendefinition). Hier zwei Parameter, Date lhs und Date rhs. ``inline`` verwenden, da im Header definiert.

```C++
class Date {
	int year, month, day; // private
};

// ACHTUNG DAS GEHT NICHT!!!
inline bool operator<(Date const & lhs, Date const & rhs) {
	lhs.year? // Geht nicht, kein Access auf private Member
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
class Date {
	int year, month, day; //private
public:
	bool operator<(Date const & rhs) const;
};

inline bool operator>(Date const & lhs, Date const & rhs) {
  return rhs < lhs;
}
inline bool operator>=(Date const & lhs, Date const & rhs) {
  return !(lhs < rhs);
}
inline bool operator<=(Date const & lhs, Date const & rhs) {
	return !(rhs < lhs);
}
inline bool operator==(Date const & lhs, Date const & rhs) {
	return !(lhs < rhs) && !(rhs < lhs);
}
inline bool operator!=(Date const & lhs, Date const & rhs) {
	return !(lhs == rhs);
}
```

Die ganzen Operatoren ausserhalb der Klasse sehen wie Boilerplate-Code aus. Deshalb gibts von Boost eine Klasse von der man erben kann, die genau diese zusätzlichen Operatoren bietet. ``private`` erben reicht aus.

Benötigt ``<``, bietet

* ``>``
* ``<=``
* ``>=``

```C++
#include "boost/operators.hpp"
#include <tuple>

class Date : private boost::less_than_comparable<Date> {
	int year, month, day; //private
public:
	bool operator<(Date const & rhs) const {
		return std::tie(year, month,day) < std::tie(rhs.year, rhs.month, rhs.day);
	}
};
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


# Vererbung

Syntax

```C++
class Base {};
class Derived : public Base {};
```

Die Reihenfolge ist wichtig! Wenn man ein Interface erbt, muss die Base public geerbt werden. Private ist möglich, aber für Mix-Ins gedacht (Mix-Ins sind Klassen die nur Funktionen hinzufügen, z.B. boost/operators.hpp)

## Mehrfachvererbung

```C++
class Base {};
struct MixIn{};
class MultipleBases : public Base, private MixIn {};
```

Die Basisklassen werden in Reihenfolge ihrer Angaben initialisiert. Normalerweise sind private Basisklassen falsches Design.

## Initialisierung
Die Base-Class-Konstruktoren-Aufrufe kommen vor die Member-Initializers in der Konstruktor-Initializer-List (vor Body).

Es gibt kein ``super()``. Die Klasse muss selber konstruiert werden, bevor der Body anfängt zu laufen.

```C++
class DerivedWithCtor : public Base {
	DerivedWithCtor(int i, int j):Base{i}, mvar{j} {}
};
```

TODO was ist mvar?! V14 S8

## Sichtbarkeit

Siehe Beispiel für Erklärung:

```C++
class Base {
protected:
	int a{0};
public:
	int b{1};
private:
	int c{2}; // auf private members kann nie aus Subklassen Zugegriffen werden
}

class A: private Base {
	// A kann auf a und b zugreifen
	// Subklassen von A können a und b nicht verwenden
	// Ausserhalb der Klassenhyrarchie kann a und b nicht verwendet werden
}

class B: protected Base {
	// B kann auf a und b zugreifen
	// Subklassen von B können a und b verwenden
	// Ausserhalb der Klassenhyrarchie kann a und b nicht verwendet werden
}

class C: public Base { // public ist default
	// C kann auf a und b zugreifen
	// Subklassen von C können a und b verwenden
	// Ausserhalb der Klassenhyrarchie kann a aber nicht b verwendet werden
}

```


## Object Slicing
```C++
struct Base {
	int a;
	Base(int a): a{a} {}
}

struct Sub: Base {
	int b;
	Sub(int a, int b): Base{a}, b{b} {}
}

Sub sub{1,2};
Base base{sub}; // ACHTUNG, Nur der Base Teil wird kopiert!
```


## Probleme mit Vererbung und pass-by-value

Wenn eine abgeleitete Klasse Funktionen definiert, werden die Base-Funktionen versteckt. Kann problematisch sein, vorallem mit const/non-const. **Achtung: dies gilt nicht wenn die Funktion einmal const (z.B. in Base) und einmal non-const (z.B. in Derived) gemacht wird.**

Um das zu umgehen macht man ein ``using`` auf die Base Class Funktionen, dann wird die Base-Funktion so behandelt als wär sie in der abgeleiteten Klasse und nimmt am Overloading teil.

TODO Beispiel

## Virtual
Wenn eine Funktion mit dynamischem Polymorphismus aufgerufen werden soll, muss sie in der Base-Klasse als ``virtual``deklariert werden. Sie bleibt dann virtual in allen abgeleiteten Klassen, bis eine sie als ``final`` deklariert.

```C++
class PolymorphicBase {
public:
	virtual void doit() { /* etwas */ }
};

class Implementor : public PolymorphicBase {
public:
	void doit() { /* etwas anderes */ }
};
```

Welche Funktion tatsächlich aufgerufen wird kommt auch drauf an, ob man einen Wert oder eine Referenz hat:

* Value-Objekt: Klassentyp definiert aufgerufene Funktion, egal ob virtual
* Reference oder Pointer: Virtual Member der abgeleiteten Klasse wird aufgerufen (durch Referenzen auf Basisklasse). Aber nur für Funktionen die ``virtual`` sind.

Wieso braucht man virtual: C++ baut Sachen nur auf, wenn man sie tatsächlich braucht. Wenn man eine Funktion ``virtual`` deklariert, wird eine ``vtable`` für diese Funktion aufgebaut, die immer trackt welche Funktion denn jetzt tatsächlich aufgerufen werden soll.

**Beispiel: Bird** TODO Beispiel verkürzen

```C++
#include <iostream>
using std::cout;

struct Animal {
	void makeSound() {
		cout << "---\n";
	}
	virtual void move() {
		cout << "---\n";
	}
	Animal() {
		cout << "animal born\n";
	}
	~Animal() {
		cout << "animal died\n";
	}
};
struct Bird: Animal {
	virtual void makeSound() {
		cout << "chirp\n";
	}
	void move() {
		cout << "fly\n";
	}
	Bird() {
		cout << "bird hatched\n";
	}
	~Bird() {
		cout << "bird crashed\n";
	}
};
struct Hummingbird: Bird {
	void makeSound() {
		cout << "peep\n";
	}
	virtual void move() {
		cout << "hum\n";
	}
	Hummingbird() {
		cout << "hummingbird hatched\n";
	}
	~Hummingbird() {
		cout << "hummingbird died\n";
	}
};

int main() {
	cout << "(a)----------------------------\n";
	Hummingbird hummingbird;
	cout << "=\n";
	Bird bird = hummingbird;
	cout << "&, =\n";
	Animal & animal = hummingbird;
	cout << "(b)-----------------------------\n";
	hummingbird.makeSound();
	bird.makeSound();
	animal.makeSound();
	cout << "(c)-----------------------------\n";
	hummingbird.move();
	bird.move();
	animal.move();
	cout << "(d)-----------------------------\n";
}
```

Output:

```
(a)----------------------------
animal born
bird hatched
hummingbird hatched
=
&, =
(b)-----------------------------
peep
chirp
---
(c)-----------------------------
hum
fly
hum
(d)-----------------------------
bird crashed
animal died
hummingbird died
bird crashed
animal died
```

## Abstrakte Klassen
```C++
struct AbstractBase {
	virtual ~AbstractBase(){}
	virtual void doitnow()=0;
};
```

Abstrakte Funktionen nennt man auch "pure virtual". Wenn man keine Implementation anbietet und dies explizit sagen will: ``= 0;``

Wenn (!) man Baissklassen mit virtuellen Member auf dem Heap alloziert **ohne ``shared_ptr``** muss der Destruktor auch ``virtual`` sein. Aber das ist sowieso böse.


# Argument Dependent Lookup (ADL)
Wenn man Funktionen ausserhalb der Klasse, aber im selben Headerfile nutzt, sollte man die Klasse und die Funktion mit demselben Namespace versehen. Wenn der Compiler eine nicht-qualified Funktion oder einen nicht-qualified Operator auffindet, schaut er sich den Namespace der Typen an, die involviert sind.


# Enums

Syntax

```C++
enum day_of_week {
	Mon, Tue, Wed, Thu, Fri, Sat, Sun
}
```
Alternativ noch mit class

```C++
enum class day of week (usw.)
```

Unterschied: ohne ``class`` leaken sie in den umgebenden Scope (``day = date::Sat``), am besten als Member einer Klasse genutzt. Mit ``class`` leaken sie nicht (``day == date::day_of_week::Sat``), und der darunterliegende Typ kann spezifiziert werden.

Die Enums starten normalerweise bei 0 und erhöhen sich um 1. Es ist möglich, die Operatoren zu überschreiben.

## Wert festlegen

Mit ``=`` kann man den Wert festlegen. Folgende erhalten dann einfach den Wert + 1.

**Beispiel Monate**
Bei den Monaten will man mit 1 beginnen, und man will auch Kurznamen. Dabei gibt es für Mai (May) aber keinen Kurznamen.

```C++
enum momth {
	jan = 1, feb, mar, apr, may, jun, jul, aug, sep, oct, nov, dec,
	january=jan, february, march, april, june=jun, july, august, september, october, november, december
};
```

## Typ festlegen
Der darunterliegende Typ kann jeder Ganzzahl-Typ sein (auch bool und char).

```C++
enum class launch_policy : unsigned_char
```

Wenn man den "Namen" eines Enums wieder will, muss man selber eine Lookup-Table implementieren.

```C++
enum class launch_policy : unsigned_char {
	sync = 1,
	async = 2,
	gpu = 4,
	process = 8,
	none = 0
}
```

# Contract/Exceptions

Wenn eine Funktion ihren Contract nicht erfüllen kann, kann man ein paar Sachen tun. Hier aufgeführt sind nur diese, welche mit C++ besonders Sinn machen oder deren Syntax speziell ist

* Ignorieren
* Standard-Resultat (z.B. anonymous)
	* Macht mehr Sinn, wenn der Aufrufer das Standardresultat im Fehlerfall bestimmen kann
* Error-Wert: Sentinel
* Error Status Side Effect: Parameter oder globale Variable verändern
* Exceptions

## Exceptions

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

## Exceptions abfragen mit CUTE
Mit CUTE kann man die Exception mit ``ASSERT_THROWS(square_root(-1.0), std::invalid_argument);`` erfragen

# Templates

Templates erlauben es, den eigenen Code an Code zu adaptieren, den es beim Erstellen des eigenen Code noch garnicht gibt.


## Function Templates
Die Typen werden bei Function Templates automatisch erkannt! Es ist aber möglich, sie in den spitzen Klammern zu deklarieren um Unklarheiten zu beseitigen.

Header-File:

```C++
namespace MyMin  { // namespace nicht zwingend
template <typename T>
T const & min(T const & a, T const & b) {
	return (a < b)? a : b;
}
}
```

Benutzung:

```C++
using MyMin::min;
using std::cout;
int i = 88;
cout << "min(i,42) = " << min(i,42) << '\n';
double pi = 3.1415;
double e = 2.7182;
cout << "min(e,pi) = " << min(e,pi) << '\n';
std::string s1 = "Hallo";
std::string s2 = "Hallihallo";

/* Es gibt auch std::min und wegen ADL wird ansonsten dieses genommen (s1 und s2 sind std::string) */
cout << "min(Hallo,Hallihallo) = " << MyMin::min(s1,s2)<<'\n';

/* Unterschiedliche Typen. Also festlegen!
Entweder im Template Argument (Template-Ebene) oder beim Aufruf mit Cast */
//min(2,pi); // compile error
min(static_cast<double>(2),pi);
min<double>(2,pi);

/* geht nicht wegen unterschiedlichen Längen
cout << "min(Peter,Toni) = " << min("Peter","Toni") << '\n'; */

/* ergibt Toni, weil hier Pointerwerte (Adressen) miteinander verglichen werden
Toni kommt später auf den Stack und hat damit auf x86-PCs eine tiefere Adresse */
cout << "min(Pete,Toni) = " << min("Pete","Toni") << '\n';

/* Lösung, damit tatsächlich Pete herauskommt und wir auch andere Längen testen können
Ansonsten gingen auch String Literals, "Toni"s */
cout << "min<std::string>(Pete,Toni) = " << min<std::string>("Pete","Toni") << '\n';
```

Random Fact: C-Style-Strings degenerieren zu einem Pointer, (es sind ``char``-Arrays), wenn sie einer Funktion übergeben werden.

## Concepts
Das Ausrechnen der Bedingungen an den Typ nennt sich **Concepts**. Anders als in Java kann man den Typ von T nicht genauer festlegen (z.B. ``implements Comparable<T>``). Wenn man sich unseren ``min``-Code anschaut erkennt man folgendes

* ``operator<(T const &, T const &)`` muss definiert sein
* Oder ``operator<(T,T)``
* Es muss einen Operator bool zurückgeben (wegen ``?:``)

## Overloads
Wir mussten beim bisherigen ``min`` einen Workaround einbauen, wenn wir mit C-Style-Strings hereinkommen. Das bedingt aber das Wissen durch den Aufrufer. Um das zu umgehen, bauen wir einen Overload ein.

Jetziges Header-File:

```C++
namespace MyMin  { // namespace nicht zwingend
template <typename T>
T const & min(T const & a, T const & b) {
	return (a < b)? a : b;
}
char const* min(char const* a, char const* b);
}
```

Und dazu noch ein Implementations-File:

```C++
#include "MyMin.h"
#include <string>
namespace MyMin {
char const* min(char const* a, char const* b) {
	return std::string(a) < std::string(b) ? a : b;
}
}
```

Sollte nicht zu oft verwendet werden. **Es gewinnt der spezifischste**.

## Variadic Template Function
Wenn man z.B. Funktionen wie printf implementieren will, kennt man die Anzahl Parameter zu Beginn nicht. Der ``...``-Operator gehört zur Syntax:

* vor einem Namen: definiert den Namen als Platzhalter für variable Anzahl Elemente
* nach einem Namen: expandiert den Namen zu allen Elementen
* zwischen zwei Namen: definiert den hinteren Namen als Liste von Parametern, vom Typ des vorderen Namen

Bei der Implementierung verwendet man die Rekursion

* Base Case ist 0 Argumente
* Rekursiver Fall ist 1 explizites Argument mit einem Schwanz als variadische Liste von Argumenten

```C++
template<typename...ARGS>
void variadic(ARGS...args) {
	println(std::cout, args...);
}

template<typename Head, typename... Tail>
void println(std::ostream & out, Head const & head, Tail const & ...tail) {
	out << head;
	if(sizeof...(tail)) { // expandiert, wenn sizeof grösser 0 ist
		out << ", ";
	}
	println(out, tail...); // recurse on tail
}
```

## Class Templates
Bei Klassen-Templates muss das Template-Argument immer spezifiziert werden. Es gibt keine Auto Deduction. **Die Begriffe Template Class und Class Template bedeuten dasselbe**.

Funktionen haben die Class Template-Parameter, können aber auch weitere Parameter haben (also auch Function Templates)

Beispiel Sack:

```C++
template <typename T>
class Sack
{
	using SackType=std::vector<T>
	// dependent name: wir verwenden einen Typ gegen aussen, der vom Template Parameter abhängt
	using size_type=typename SackType:size_type;
	SackType theSack{};
public:
	bool empty() const { return theSack.empty(); }
	// Weil wir den size_type ausgeben wollen, brauchen wir den dependent name oben

	size_type size() const { return theSack.size(); }
	void putInto(T const & item) { theSak.push_back(item); }
};

// Member-Funktion ausserhalb der Template-Klasse
template <typename T>
inline T Sack<T>::getOut() {
	if(!size()) { throw std::logic_error{"empty Sack"}; }
	// Pick random element
	auto index = static_cast<size_type>(rand() % size())
	T retval{theSack.at(index)};
	theSack.erase(theSack.begin()+index);
	return retval;
}
```

Es ist normal, dass Template Definitionen Typ-Aliase verwenden. Neuerdings mit using, früher mit typedef:

* ``using newname=existingtype;``
* ``typedef existingtype newname;``

Bei Membern ausserhalb des Class Templates (wie oben ``getOut()``):

* Template Intro wiederholen
* Inline um ODR zu gewährleisten

Templates gehören immer ins Header-File! Grund: das .cpp-File kann man auch kompiliert als Objektcode abgeben. Den Header gibt man aber im Source-Code ab. Da der Compiler bei Templates mehr oder minder Textersetzung macht (...), muss er das Template selber kompilieren können.

Funktionen immer direkt innerhalb der Klasse oder als inline-Funktion im Headerfile.

### Erben in Template Classes
Das Name-Lookup kann verwirrend sein. Immer ``this->`` oder alternativ ``classname::`` verwenden, damit man mit der richtigen Klasse spricht. TODO: evtl. noch anderes von V12 S. 12?

### Spezialisierung
Man kann Class Templates auch spezialisieren (wie Function Templates), zum Beispiel zum Umgang mit Pointern. Es wird auch hier wieder am meisten spezialisierte genommen. Achtung, die Templates müssen nicht miteinander verwandt sein, das ist eigentlich eher Konvention...

```C++
// forward declaration
template <typename T> class Sack;

// brauchts
template <>
class Sack<char const *> { // Spezialisierung
	// zum Beispiel andere Implementierung
};
```

### Template Terminologie

**Template Definition**
```C++
template <typename T>
class Sack{...}
```

**Template Declaration**
```C++
template <typename T>
class Sack;
```

**Class Template Explicit Specialization**
```C++
template<>
class Sack<char const *> {...}
```

**Template Partial Specialization**
```C++
template <typename T>
class Sack<T *> {...}
```

**Class Template Member Specialization**
```C++
template<>
void Sack<char const *>::putInto(char const *p) {...}
```

### Sack mit Initializer List füllen
Wenn man den Sack mit einer Initializer List füllen will (z.B. ``Sack<int> sack{1, 2, 3};``), kann man die bereitgestellte ``std::initializer_list<T>`` nutzen. Dazu definieren wir einen speziellen Konstruktur

```C++
Sack(std::initializer_list<T> il):theSack(il){}
```

### Container variieren
Man könnte jetzt auch noch den Container variieren. Achtung: hier müssen dann evtl. die Iteratoren angepasst/verallgemeinert werden. Da Container oft mehr als ein Argument haben, machen wir es gleich variadisch. Und damit man nicht immer ``std::vector`` angeben muss, bauen wir einen Default ein.

Unsere Template-Definition schaut jetzt wie folgt aus (Klammerung beachten!, Keyword ``class`` ebenso)
```C++
template <typename T, template<typename...> class container=std::vector>
class Sack {
(usw.)
```

Und dann z.B.: `Sack<int, std::list> listSack{1,2,3,4,5};

### Templates als Adapter
Wenn man zum Beispiel einen SafeVector bauen will, der den Vektor implementiert, aber ``operator[]`` so implementiert dass ein Index Bounday Check stattfindet.


```C++
template <typename T>
class safeVector : public std::vector<T> {
	using Base=std::vector<T>;
public:
	using size_type=typename Base::size_type;
	using std::vector<T>::vector; // ctors übernehmen, using ohne =

	T const & operator[](size_type index) const {
		return this->at(index);
	}

	T & operator[](size_type index) {
		return Base::at(index); // ginge auch this->
	}

	// dann noch front/base
};
```
