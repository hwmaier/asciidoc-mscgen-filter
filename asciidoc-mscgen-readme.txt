mscgen filter for AsciiDoc
==========================
Author: Henrik Maier

Version: 1.1


Introduction
------------

Mscgen (link:http://www.mcternan.me.uk/mscgen/[]) is a small program that parses
Message Sequence Chart descriptions and produces PNG, SVG, EPS or server side
image maps (ismaps) as the output. The program and the language it parses are
similar to the Graphviz Dot tools.

Using the AsciiDoc mscgen filter, Message Sequence Chart descriptions can be
embedded into AsciiDoc documents and processed into either PNG bitmap or SVG
vector graphics.

Usage
-----

- The mscgen filter is invoked by setting the listing block or
  paragraph style (the first positional block attribute) to '"mscgen"'.
- The second positional attribute (named 'target') is optional, it sets
  the name of the generated image file. If this is not supplied a
  file name is automatically generated.


This AsciiDoc block:

.....................................................................
["mscgen"]
---------------------------------------------------------------------
msc {
 arcgradient = 8;

 a [label="Client"],b [label="Server"];

 a=>b [label="data1"];
 a-xb [label="data2"];
 a=>b [label="data3"];
 a<=b [label="ack1, nack2"];
 a=>b [label="data2", arcskip="1"];
 |||;
 a<=b [label="ack3"];
 |||;
}
---------------------------------------------------------------------
.....................................................................

renders:

["mscgen"]
---------------------------------------------------------------------
msc {
 arcgradient = 8;

 a [label="Client"],b [label="Server"];

 a=>b [label="data1"];
 a-xb [label="data2"];
 a=>b [label="data3"];
 a<=b [label="ack1, nack2"];
 a=>b [label="data2", arcskip="1"];
 |||;
 a<=b [label="ack3"];
 |||;
}
---------------------------------------------------------------------


Installation
------------

In addition to AsciiDoc you will need to have installed:

- The mscgen command line utility (link:http://www.mcternan.me.uk/mscgen[]).
  mscgen must be excutable from the command line.


The filter was developed and tested on Windows using mscgen 0.21.
and AsciiDoc 8.6.3.


Known Issues
------------

If a version of mscgen earlier than 0.21 is used, SVGs are not sized correctly
by FOP when producing PDFs as the width and height attribute are missing in
the SVG file. Use PNG format with those earlier versions.


Examples
--------

The following examples are taken from the mscgen distribution.


.Program flow example
["mscgen"]
---------------------------------------------------------------------
msc {
  a, "b", c ;

  a->b [ label = "ab()" ] ;
  b->c [ label = "bc(TRUE)"];
  c->c [ label = "process(1)" ];
  c->c [ label = "process(2)" ];
  ...;
  c->c [ label = "process(n)" ];
  c->c [ label = "process(END)" ];
  a<-c [ label = "callback()"];
  ---  [ label = "If more to run", ID="*" ];
  a->a [ label = "next()"];
  a->c [ label = "ac()"];
  b<-c [ label = "cb(TRUE)"];
  b->b [ label = "stalled(...)"];
  a<-b [ label = "ab() = FALSE"];
}
---------------------------------------------------------------------


.Colour sample
["mscgen"]
---------------------------------------------------------------------
msc {
  hscale="1.5";

  a [label="TASK_A\n(priority=45)", textcolour="red"],
  b [label = "TASK_B\n(priority=60)", URL="www.dilbert.com", linecolour="green"],
  c [label = "TASK_C\n(priority=61)", ID="5", IDURL="www.slashdot.org"] ;

  a->b [ label = "Message 1", linecolour="red"] ;
  b->c [ label = "Message 1 + a" ] ;
  a<=c [ label = "Response", textcolour="red"];
  a->c [ label = "Line 1\nLine 2", linecolour="green", textcolour="red" ];
  a<-c [ label = "Line One\n\"Line Two\"", ID="6", IDURL="www.slashdot.org"];
  a->b [ label = "Linky", URL="www.google.co.uk"];
  *<:b [ label = "Double Linky", URL="www.google.co.uk", textcolour="#ff7f7f"];
  c:>b [ label = "Double"];
  a:>a [ label = "" ];
  c<:c;

}
---------------------------------------------------------------------


.Example MSC using boxes
["mscgen"]
---------------------------------------------------------------------
msc {

   # The entities
   A, B, C;

   # The relations
   |||;

   // The following three boxes will be aligned due to the use
   // of "comma" to separate them (instead of semi-colon).

   A box A [label="box"],   // Box (with "square corners") placed upon line "A"
   B rbox B [label="rbox"], // Box (with "round corners") upon the line "B"
   C abox C [label="abox"], // Box (with "<>-sides") upon the line "C"
   |||; // This adds some space between the boxes here and the boxes below.
        //  - similar to an empty line in a text document.


   A abox B [label="abox"]; // Box spanning from A to B (covering both).
   |||;
   B rbox C [label="rbox"];
   |||;
   A box A [label="box"],
   B rbox B [label="rbox"],
   C abox C [label="abox"],
   ---;
   ---;

}
---------------------------------------------------------------------

