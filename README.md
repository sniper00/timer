# Usage

```
#include <ostream>
#include <chrono>
#include "timer.hpp"

static std::time_t milliseconds()
{
    return std::chrono::duration_cast<std::chrono::milliseconds>(std::chrono::steady_clock::now().time_since_epoch()).count();
}

int main()
{
    moon::timer timer;

    int count = 0;
    timer.repeat(1000, 10, [&count](moon::timer_t id) {
        std::cout << "repeat 10 times timer expired:" << ++count<< std::endl;
     });

    auto timerid = timer.repeat(1000, 10, [](moon::timer_t id) {
        std::cout << "Should not output this line " << std::endl;
        });
    timer.remove(timerid);

    timer.repeat(1000,-1, [](moon::timer_t id) {
        std::cout << "Infinite timer output." << std::endl;
        });

    //update timer in your loop
    while (true)
    {
        timer.update(milliseconds());
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }

    return 0;
}

```