*DRAFT* TEI XPointer Requirements and Notes
===========================================
(Hugh Cayless – philomousos@gmail.com)

xpath
-----
    Sequence xpath(XPATH)
*Definitions*

XPATH: an XPath path expression, following the latest scheme adopted by the W3C, that returns an element or text node. The behavior of range schemes on XPaths returning multiple nodes is undefined. XPaths returning atomic values (e.g. substring()) are illegal because they represent extracted values rather than locations in the source document. XPath expressions that address attribute nodes are only legal in the xpath() scheme.

left
----
    Point left(IDREF | XPATH)
*Definitions*

IDREF: the value of an @xml:id attribute in the source document.

right
-----
    Point right(IDREF | XPATH)
    
string-index
------------
    Point string-index(IDREF | XPATH, OFFSET)
*Definitions*

OFFSET: a positive, negative, or zero integer. An offset of 0 represents the position immediately before the first character in either the first text node child of the node addressed in the XPATH|IDREF parameter or the first following-sibling text node, if the addressed element contains no text node descendants.

range
-----
    Sequence range(POINTER, POINTER[, POINTER, POINTER …])
*Definitions*

POINTER: IDREF | XPATH | left() | right() | string-index()

string-range
------------
    Sequence string-range(IDREF | XPATH, OFFSET, LENGTH[, OFFSET, LENGTH ...])
*Definitions*

LENGTH: a positive integer denoting the length of the string being addressed.

match
-----
    Sequence match(IDREF | XPATH, 'REGEX'[, INDEX]) 
*Definitions*

REGEX: a regular expression

INDEX: a positive integer, beginning at 1, denoting the index of the match in the set of matches which is addressed by the scheme
NOTE: Because the REGEX portion of the pointer is delimited with single quotes, those must be escaped if they occur inside the regular expression. The current escape syntax is \', so to indicate the string "don't" inside an element "foo", you could write #match(foo,'don\'t'). This is subject to change

Points and Sequences
--------------------
Points are adjacent to element nodes or characters in text nodes. A Point represents a point between element nodes or individual characters in a text node. Every point is adjacent to either characters or elements, and never to another point. In fact, in the character representation of an XML document, every position between data characters, start-tags or end-tags is a point, and there are no other points. If one treats all character content as if it were broken into single-character text-nodes, every point is definable as either

* the point preceding a node, and if that node has a predecessor in document order, then it is the same as the point following that predecessor; or
* the point following a node, and if that node has a successor in document order, then it is the same as the point preceding that successor.


A Sequence follows the definition in the [XPath 2.0 Language recommendation](http://www.w3.org/TR/xpath20/#id-basics), with one change:

\[Definition: A sequence is an ordered collection of zero or more items.\] 

\[Definition: An item is either a partial text node (was: "an atomic value") or a node.\] 

\[Definition: A node is an instance of one of the node kinds defined in [XQuery 1.0 and XPath 2.0 Data Model (Second Edition)](http://www.w3.org/TR/xpath20/#datamodel).\]


Further, an XPointer sequence must consist of nodes, as defined above, except for the first and last items in the sequence, which may be portions of text nodes. A sequence addressed by range() for example, might start in the middle of one text node and end inside another. In this case, the first and last items in the sequence would be partial text nodes. It must be understood that TEI pointers cannot address attribute nodes (except via the xpath() scheme) or partial element nodes (at all). 

TEI XPointers comprise a declarative mini-language. As such, they do not directly return values and cannot be executed. They address pieces of XML documents, but what is to be done with those pieces is wholly dependent on the context in which they are being interpreted. So when left() is defined as addressing a Point, this does not mean that it is a function returning a Point. Rather, it describes the position immediately before the node identified in its parameter. We can imagine it as being like an instruction to a word processor to put the cursor at a particular point in a document being edited.
