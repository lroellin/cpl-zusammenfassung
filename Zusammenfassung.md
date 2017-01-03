# C++ Spick
C++ Spick

TODO

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
* Operatoren Priorität (gleich wie C)
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

## Include Guard

Um die ODR (One Definition Rule) nicht zu verletzen, verwendet man Guard Statements

```C++
#ifndef SAYHELLO_H_
#define SAYHELLO_H_
#include <iosfwd>
void sayHello(std::ostream &out);
#endif /* SAYHELLO_H_ */
```

# Klassen

Es gibt zwei Keywords, ``struct``und ``class``. Die sind äquivalent, ausser
* struct ist standardmässig public
* class ist standardmässig private

Beispiel mit 3 Files

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
* Nach einem Fehler (.fail) muss man den Zustand mit .clear() wieder löschen, die ungültigen Eingaben rausholen und weiterfahren

istream Zustände:
| bit | query | entered |
| failbit | ``is.fail()`` | formatted input failed |
| eofbit | ``is.eof`` | end of input reached |
| badbit | ``is.bad`` | unrecoverable I/O error |



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

# Iterators

Include für alle Iterators: #include <iterator>

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

## std::vector

Entspricht ArrayList in java

**Include / Initialisieren**: ``#include <vector> / std::vector<int> v{};``

**Iterator**: random access

**Insert**:

``insert (const_iterator position, const value_type& val);``

``push_back (const value_type& val);``

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
