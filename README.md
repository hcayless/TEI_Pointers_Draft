TEI_Pointers_Draft
==================

Stable home of the TEI Pointers Draft specification, for which see 
[TEIXPointers](https://github.com/hcayless/TEI_Pointers_Draft/blob/master/TEIXPointers.md), the unstable version of 
which is on [Google Docs](https://docs.google.com/document/d/1JsMA-gOGrevyY-crzHGiC7eZ8XdV5H_wFTlUGzrf20w/edit#). Both 
versions are active, and you may comment on the Google Docs version, but this one has a commit history, and can support 
releases, etc.

See <http://www.tei-c.org/release/doc/tei-p5-doc/en/html/SA.html#SATS> for the original specification. Draft revsions are available at <http://hcayless.github.io/TEI-Guidelines/Guidelines-web/en/html/SA.html#SATS>. There is a demo 
implementation available at <https://github.com/hcayless/tei-xpointer.js> and a working demo at 
<http://tei.philomousos.com/>.

Notes on Requirements
---------------------

* The original XPointer specification envisioned the pointers as extensions of XPath, and as such, they were meant to be actual functions. The TEI Pointers described here are a notation describing parts of XML documents. They are meant to be resolved rather than executed.
* Each pointer specification must have a full signature in the specification, that is, a listing of allowed parameters and a “return” type.
* The precise meaning and function is dependent on the context within which they are used.
* The issue of namespaces needs to be addressed, either via the xmlns() scheme or by declaring the TEI namespace as the default. I would prefer the latter. The xmlns() scheme would then only need to be employed for elements outside the TEI namespace.
* The object types for addressed parts of a document need to be clarified. I will argue for only two types: Sequences (as defined in XPath 2.0, see <http://www.w3.org/TR/xpath20/#dt-sequence>) and Points. 
* A Range, as specified in the original pointer specification is just a Sequence, and as a result, a Range can not contain a not-well-formed document fragment. Or at least, not one that wouldn't be well-formed if it were wrapped by an element. 
* Rules for how elements partially contained by a Range are to be resolved must be established. My current thinking is that partially contained elements (i.e. where only the start or end tag is inside the range) are not to be considered part of the returned sequence. Since a child node of the partially-contained parent will be a member of the addressed Sequence, it will always be possible to grab its parent, or a copy of it. An exception should perhaps be made where addressed text nodes are entirely contained by a parent element.
* We need either xpath2() in addition to xpath1() or just xpath(), which would be understood to cover both versions.
* These schemes have been most commonly thought of in the context of XInclude, i.e. they are meant to be used to import a portion of an external document during processing. But other uses are equally possible: they could be used during XSLT processing to convert standoff markup to inline markup, for example, or in a web browser to scroll the page to the part of the document referred to by the pointer (like fragment identifiers currently work).
* As designed, the current pointer schemes (or some of them at least) are composable, i.e. you can nest them. I will argue against such nesting except in the case of range functions, which may take functions returning points as parameters.
* The first parameter of any function must be either an IDREF or an XPath (obviously only an XPath in the case of the xpath*() function(s). 
* The type of the first parameter can be distinguished in the following way: an IDREF must be an NCName as defined by <http://www.w3.org/TR/1999/REC-xml-names-19990114/#NT-NCName> whereas an XPath expression must either begin with a “/” or be wrapped in a function, such as id(). Put more simply, XPaths will contain characters illegal in IDREFs: either “(“and “)”, or “/” and so the two can always be distinguished trivially.
* left() and right() can only work on single elements or text nodes, not attributes or sequences.
* A new scheme is needed to address points inside a text node. I suggest string-index().
* If an XPath returning an element or text node is specified as the beginning or end of a range, it will be returned as part of the resulting sequence.
* range() and string-range() Pointers will need to be able to address non-contiguous sections of text because of the way TEI structures like choice and app operate on the text stream—marking places where it forks and then rejoins. A name might begin outside an app, for example, and complete in one reading, but not the lemma.


