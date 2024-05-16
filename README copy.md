# dsp_filters_ros2

ROS 2 vendor package for the [DspFilters](https://github.com/vinniefalco/DSPFilters) library by Vinnie Falco.

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

