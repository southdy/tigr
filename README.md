# README

TIGR is a tiny graphics library, for when you just need to draw something in a window without any fuss. TIGR doesn't want to do everything. We don't want to bloat your program with hundreds of extra DLLs and megabytes of frameworks.

TIGR is free to copy with no restrictions; see tigr.h

We don't want to supply every possible function you might ever need. There are already plenty of add-on libraries
for doing sound, XML, 3D, whatever. Our goal is simply to allow you to easily throw together small 2D programs when you need them.

TIGR's core is a simple framebuffer library. On top of that, we provide a few helpers for the common tasks that 2D programs generally need:

 - Create bitmap windows.
 - Direct access to bitmaps, no locking.
 - Basic drawing helpers (plot, line, blitter).
 - Text output.
 - PNG loading and saving code.

TIGR is designed to be small and independent. A typical 'hello world' is less than 40KB. We don't require you to distribute any additional DLLs; everything is baked right into your program.

### How to set up TIGR ###

TIGR is supplied as a single .h file.
To use it, you just drop them right into your project. No fancy build systems, no DLL hell, no package managers.

1. Grab  ** tigr.c ** and ** tigr.h **
2. Throw them into your project.
3. Link with D3D9.LIB (or -ld3d9) on Windows, or -framework OpenGL and -framework Cocoa on OSX
4. You're done!

### How do I program with TIGR? ###

Here's an example Hello World program. For more information, just read ** tigr.h ** to see the APIs available.

```
#include "tigr.h"

int main(int argc, char *argv[])
{
    Tigr *screen = tigrWindow(320, 240, "Hello", 0);
    while (!tigrClosed(screen))
    {
        tigrClear(screen, tigrRGB(0x80, 0x90, 0xa0));
        tigrPrint(screen, tfont, 120, 110, tigrRGB(0xff, 0xff, 0xff), "Hello, world.");
        tigrUpdate(screen);
    }
    tigrFree(screen);
    return 0;
}
```

Or using c++ wrapper

```
#include "tigr-cpp.h"

int main(int argc, char* argv[]) {
    auto t = std::make_shared<tigr::Window>(200, 200, "Example", (int)tigr::WindowFlag::AUTO);
    auto image = std::make_shared<tigr::Bitmap>(200, 200);

    while (!t->isClosed()) {
        t->clear(0, 0, 0);

        for (int y = 0; y < 200; ++y) {
            for (int x = 0; x < 200; ++x) {
                if ((x % 2 == 0 && y % 2 != 0) || (x % 2 != 0 && y % 2 == 0))
                    image->setPixel(x, y, tigr::RGBA{ 255, 255, 255 });
                else
                    image->setPixel(x, y, tigr::RGBA{ 0, 0, 0 });
            }
        }

        t->blitTint(image, tigr::RGBA{ 255, 0, 0 });
        t->blitTint(image, 10, 10, 0, 0, 180, 180, tigr::RGBA{ 0, 0, 255 });
        t->blitTint(image, 20, 20, 0, 0, 160, 160, tigr::RGBA{ 0, 255, 0 });
        t->print(0, 0, tigr::RGBA{ 255, 255, 255 }, "Hello world");
        t->update();
    }
    return 0;
}
```
