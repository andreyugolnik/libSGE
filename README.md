This is a maintenance and code cleanup.
- Abs of unsigned values bug fixed.
- Include SDL path fixed.
- using namespace std removed.

***

SDL Graphics Extension (SGE)
====================================================================================
http://freshmeat.net/projects/sge/
http://www.etek.chalmers.se/~e8cal1/sge/index.html
http://www.digitalfanatics.org/cal/sge/index.html

Author: Anders Lindström - email: cal@swipnet.se


1. Intro
2. Requirements
3. Compiling
4. Library usage
5. Makefile options
   5.1 Using pure C with SGE (C_COMP)
   5.2 FreeType (USE_FT)
   5.3 The SDL_Image library (USE_IMG)
   5.4 The QUIET option (QUIET)
6. Cross compiling SGE to W32
7. Compiling SGE under W32 with MS VisC/C++
8. Thanks
9. Misc.


====================================================================================
1. Intro

SGE is an add-on graphics library for the Simple Direct Media Layer. SGE provides 
pixel operations, graphics primitives, FreeType rendering, rotation/scaling and much 
more.

This is free software (LGPL), read LICENSE for details.

SGE has the following parts:
[sge_surface]    Pixel operations, blitting and some pallete stuff.
[sge_primitives] Drawing primitives. 
[sge_tt_text]    FreeType font support.
[sge_bm_text]    Bitmapfont and SFont support.
[sge_textpp]     Classes for handling and rendering text.
[sge_shape]      Classes for blitting and sprites.
[sge_collision]  Basic collision detection.
[sge_rotation]   Rotation and scaling of surfaces.
[sge_blib]       Normal, filled, gourand shaded and texture mapped polygons.
[sge_misc]       Random number and delay functions.


Read docs/index.html for API documentation.

Always check WhatsNew for important (API) changes!

There is a "Beginners guide to SGE" in the html documentation about how to compile and 
use SGE in your own project, please read it if you're new to Unix/Linux development.

Read INSTALL for quick compile and install instructions.


====================================================================================
2. Requirements

-GNU Make.
-SDL 1.2+.
-An ANSI/ISO C++ compiler. SGE should conform to ANSI/ISO C++.
-Optional:
 -FreeType 2+
 -SDL_Image (see section 5.3)
-Some SDL knowledge.

First you need SDL (http://www.libsdl.org/) and the FreeType (2.x) library 
at http://www.freetype.org/ (you only need the FreeType library if you want 
to use SGE's truetype font routines, see section 5.2). The FreeType library is 
included in most Linux distributions (RPM users: install the freetype dev rpm
package from the install cd). 

After installing SDL and FreeType, don't forget to check that the dynamic
linker can find them (check /etc/ld.so.conf and run ldconfig).

You must also have a good C++ compiler (should be able to handle templates). Recent 
versions of GNU c++ works fine. SGE will use gcc/g++ as default, but this can be 
changed in the file "Makefile.conf".


====================================================================================
3. Compiling

Before compiling you might want to change some Makefile.conf options, see section 5.

Just do 'make install' to compile and install SGE. This will install SGE to the same 
place as SDL. You can change the install location by editing the PREFIX and PREFIX_H 
lines in the file "Makefile.conf".

If you just want to test the examples (and not install anything) you can just do
'make'. This will build a static version of SGE (libSGE.a). 

If you want a dynamic version of SGE (libSGE.so) but don't want the makefile to
install SGE, do 'make shared'.

To build the examples, goto the directory examples/ and do 'make'.

See the file INSTALL for more information. You can also read the file 
"docs/guide.html"


====================================================================================
4. Library usage

Do "#include "sge.h"" in your code after the normal "#include "SDL.h"" line.

The normal way to compile code that uses SGE is with the flag 
"`sdl-config --cflags`", but also add the flag "-I/path/to/SGE/headers" if the SGE 
headers are installed at any other place than the SDL headers.

The normal way to link code that uses SGE is with the flags 
"-lSGE `sdl-config --libs` -lstdc++", but also add the flag "-L/path/to/SGE/library" 
if SGE is installed at any other place than SDL. If you're using static linking then 
the flags "`freetype-config --libs`" and/or "-lSDL_image" also needs to be added if
FreeType and/or SDL_Image support is enabled.

Example:
g++ -Wall -O3 `sdl-config --cflags` -c my_sge_app.cxx
g++ -o my_sge_app my_sge_app.o -lSGE `sdl-config --libs` -lstdc++

See "docs/guide.html" and the code in examples/ for more information.


====================================================================================
5. Makefile options

Edit Makefile.conf to turn on/off (y/n) these options.


5.1 Using pure C with SGE (C_COMP)
 
Setting 'C_COMP = y' should allow both C and C++ projects to use SGE. However, you 
will not be able to use any of the overloaded functions (only the Uint32 color 
version of the overloaded functions will be available) or C++ classes from a C 
project. C++ projects should not be affected. Default is 'y'.


5.2 FreeType (USE_FT)

If you don't need the TT font routines or just don't want do depend on FreeType,
uncomment 'USE_FT = n' in Makefile.conf. Default behavior is to autodetect
FreeType.


5.3 The SDL_Image library (USE_IMG)

The SDL_Image library (http://www.libsdl.org/projects/SDL_image/index.html) will be
autodetected with the default setting. SDL_Image support enables SGE to load png 
images and thus use Karl Bartel's very nice SFont bitmapfonts 
(http://www.linux-games.com/sfont/). The use of SDL_Image can also be controlled by
setting 'USE_IMG = y/n' manually.


5.4 The QUIET option (QUIT)
Set 'QUIET = y' if you don't want the makefile to output any SGE specific messages.


====================================================================================
6. Cross compiling SGE to W32

SGE can be compiled by a win32 crosscompiler. You need a crosscompiled version of 
SDL (and FreeType or SDL_Image if used). Check SDL's documentation (README.Win32) on how to get and setup 
a cross-compiler.

A crosscompiler can be found at http://www.libsdl.org/Xmingw32/index.html. This 
crosscompiler uses the new MS C-Run-time library "msvcrt.dll", you can get it from 
www.microsoft.com/downloads (do a keyword search for "libraries update") if you 
don't already have it.

If you want to build a dll ('cross-make dll' or 'dll-strip') then you might want to
do 'ln -s ../../bin/i386-mingw32msvc-dllwrap dllwrap' in 
/usr/local/cross-tools/i386-mingw32msvc/bin.


====================================================================================
7. Compiling SGE under W32 with MS VisC/C++

Should work. Check the download page on SGEs homepage for project files (these are 
untested by me and are often outdated but may be of some help).

Note that if you don't use the makefile system to build SGE (as with VisC/C++) then
you must edit sge_config.h manually to change build options. The options in section
5 corresponds to defining the following symbols in sge_config.h:
_SGE_C_AND_CPP  - C_COMP=y
_SGE_NOTTF      - USE_FT=n
_SGE_HAVE_IMG   - USE_IMG=y

For example:
/* SGE Config header */
#define SGE_VER 030809
#define _SGE_C_AND_CPP
#define _SGE_HAVE_IMG


====================================================================================
8. Thanks.

Thanks goes to...
-Sam Lantinga for the excellent SDL library.
-The FreeType library developers.
-John Garrison for the PowerPak library. 
-Karl Bartel for the SFont library (http://www.linux-games.com/).
-Garrett Banuk for his bitmap font routines (http://www.mongeese.org/SDL_Console/).
-Seung Chan Lim (limsc@maya.com or slim@djslim.com).
-Idigoras Inaki (IIdigoras@ikerlan.es).
-Andreas Schiffler (http://www.ferzkopp.net/Software/SDL_gfx-2.0/index.html).
-Nicolas Roard (http://www.twinlib.org).

and many more...

====================================================================================
9. Misc.

Read the html documentation and study the examples. 



/Anders Lindström - email: cal@swipnet.se
