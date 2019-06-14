MicroPython.js
==============

MicroPython transmuted into Javascript by Emscripten.

Dependencies
------------

Building micropython.js bears the same requirements as the standard MicroPython
ports with the addition of Emscripten (and uglify-js for the minified file). 

Install the browsix port of emscripten by following instructions at this link: https://github.com/klanmiko/emscripten/blob/browsix-wasm/README.md

Ensure that the relevant environment variables have been set according to the instructions above.

If memory errors are encountered it may be worthwhile to modify the tool.
`fastcomp-1.37.22/lib/Target/JSBackend.cpp` may require the minor fix 
found in JSBackend.patch. This patch attempts to address situations where
C code running through Emscripten is denied access to Javascript variables 
leading to false-positives in the MicroPython garbage collector as variables 
with pointers exclusively in Javascript will be erased prematurely.
Refer to Emscripten documentation for instructions on building Emscripten 
from source.

Build instructions
------------------


In order to build micropython.js, run:

    $ make

Do not run `emmake make` as the Makefile in this repository already uses emcc for compilation

To generate the minified file micropython.min.js, run:

    $ make min


Testing
-------

Run the test suite using:

    $ make test

API
---
Previous C functions have no longer been exported to javascript. Instead, interaction with Micropython.js can be done using pipes, to read and write to the REPL from a javascript frontend.
