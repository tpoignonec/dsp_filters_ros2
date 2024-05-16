# dsp_filters_ros2

Vendored version of the [DspFilters](https://github.com/vinniefalco/DSPFilters) library by Vinnie Falco to streamline its use in ros2 applications.

## Getting started

> `package.xml` :
```xml
...
<depend>dsp_filters_ros2</depend>
...
```

> `CMakeLists.txt` :
```cmake
...
find_package(dsp_filters_ros2)
...
ament_target_dependencies(<the_target> <type> dsp_filters_ros2)
...
```
> `xxx.cpp` :
```cpp
#include <dsp_filters_ros2/Dsp.hpp>

// Or using legacy path :
#include <DspFilters/Dsp.hpp>
```


## Usage examples

```cpp
// Create a Chebyshev type I Band Stop filter of order 3
// with state for processing 2 channels of audio.
Dsp::SimpleFilter <Dsp::ChebyshevI::BandStop <3>, 2> f;
f.setup (3,    // order
         44100,// sample rate
         4000, // center frequency
         880,  // band width
         1);   // ripple dB
f.process (numSamples, arrayOfChannels);
```
