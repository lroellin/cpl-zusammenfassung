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
    * Unterschied class, struct
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
    * Guard statements
* Constructor
    * Konstruktoren mit direkter Zuweisung, Reihenfolge?
    * Copy
    * Move
    * Destructor
* Files lesen / schreiben

[TOC]

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

# Include Files

Eigene Includes immer zuoberst!

## Include Guard

Um die ODR (One Definition Rule) nicht zu verletzen, verwendet man Guard Statements

```C++
#ifndef SAYHELLO_H_
#define SAYHELLO_H_
#include <iosfwd>
void sayHello(std::ostream &out);
#endif /* SAYHELLO_H_
```
