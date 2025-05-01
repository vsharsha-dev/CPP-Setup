# CPP-Setup
This repository is purely about setting up your PC for C++ projects

# Terminal-Only C++ Project Setup on macOS

This guide helps you set up and run C++ projects **entirely via terminal** on macOS — no IDE, no VS Code tools.

---

## Prerequisites

```bash
xcode-select --install       # installs clang++
brew install cmake ninja
```

clang++ — macOS default C++ compiler.

cmake — to generate builds.

ninja — as the fast build system.

## Example Project Setup

### 1. Folder Structure

```bash
mkdir -p ~/order-book-engine/{src,include,build}
cd ~/order-book-engine
```

Now create these files:

include/OrderBook.h

```bash
#pragma once

class OrderBook {
public:
    OrderBook();
    void addOrder(int id, double price, int qty);
    void cancelOrder(int id);
};
```

src/OrderBook.cpp

```bash
#include "OrderBook.h"
#include <iostream>

OrderBook::OrderBook() {}

void OrderBook::addOrder(int id, double price, int qty) {
    std::cout << "Adding order " << id << " with price " << price << "\n";
}

void OrderBook::cancelOrder(int id) {
    std::cout << "Cancelling order " << id << "\n";
}
```

src/main.cpp

```bash
#include "OrderBook.h"

int main() {
    OrderBook ob;
    ob.addOrder(1, 100.5, 10);
    ob.cancelOrder(1);
    return 0;
}
```

CMakeLists.txt

```bash
cmake_minimum_required(VERSION 3.20)

project(OrderBookEngine LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wall -Werror")

include_directories(include)

add_executable(order_book
    src/main.cpp
    src/OrderBook.cpp
)
```

### 2. Build the Project Using Terminal

From the project root:

```bash
cd build
cmake -G Ninja ..
ninja
```

If successful, it creates:

```bash
./order_book
```

Run it:

```bash
./order_book
```

You’ll see:

```bash
Adding order 1 with price 100.5
Cancelling order 1
```

### Manual Compilation Without CMake
(Just to understand what CMake is doing)

```bash
clang++ -std=c++20 -O3 -Wall -Werror -Iinclude src/*.cpp -o order_book
./order_book
```

This is what CMake automates for you behind the scenes.
