# Shape Rendering Library (C++)

A small object-oriented C++ library for drawing geometric shapes in the console.
It supports circles, triangles, parallelograms, and composite shapes made of up to five figures.

The key idea is **separating what a shape is from how it is displayed**. Shape classes store
geometry only, while separate display classes decide how to render them. This is an
application of the **Bridge design pattern**: a new display mode can be added without
changing any shape class, and the display of an existing shape can be switched at runtime.

## Example output

```
   p
  ppp
pppppp
 ppp
  p
```
```
Drawing a parallelogram from vectors (3, 2), (2, -2).
```
The same `Parallelogram` object drawn with `GraphicalDisplay`, then with `TextDisplay`.

## Architecture

```
Shape (abstract)  ──has a──▶  Display (interface)
 ├── Circle                    ├── GraphicalDisplay   draws with characters in the console
 ├── Triangle                  └── TextDisplay        prints a text description
 ├── Parallelogram
 └── ComplexShape  (holds up to 5 shapes)
```

| Class | Responsibility |
|-------|----------------|
| `Display` | Abstract interface with `drawCircle()`, `drawTriangle()`, `drawParallelogram()` |
| `GraphicalDisplay` | Renders shapes in the console using a scanline algorithm with linear interpolation |
| `TextDisplay` | Prints a text description of each shape |
| `Shape` | Abstract base class; holds a pointer to a `Display` and provides `changeDisplay()` |
| `Circle`, `Triangle`, `Parallelogram` | Store geometry and delegate drawing to the current `Display` |
| `ComplexShape` | Composite shape that draws up to five shapes in the order they were added |
| `MyExceptions`, `Validation` | Custom exception hierarchy and input validation |

## C++ concepts used

- Inheritance, abstract classes and polymorphism (pure virtual functions)
- Bridge design pattern, runtime switching of behavior
- Custom exception hierarchy derived from `std::runtime_error`
- Input validation (radius, vector values, null pointers)
- Separation into header and source files

## Usage

```cpp
Display* graphical = new GraphicalDisplay();
Display* text = new TextDisplay();

Shape* circle = new Circle(graphical, 5);
circle->draw();                 // drawn with 'c' characters

circle->changeDisplay(text);
circle->draw();                 // "Drawing a circle with radius 5."
```

## Build and run

With CMake:

```bash
cmake -S . -B build
cmake --build build
./build/shapes
```

Or directly with g++:

```bash
g++ -std=c++17 -IHeaders SourceFiles/*.cpp -o shapes
./shapes
```