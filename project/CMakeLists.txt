cmake_minimum_required(VERSION 3.14)
project(hft_stock_tracker LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

# Low-latency build flags
add_compile_options(-O3 -march=native)

# Subdirectories for C++ components
add_subdirectory(src/cpp/common)
add_subdirectory(src/cpp/network)
add_subdirectory(src/cpp/market_data)
add_subdirectory(src/cpp/order_book)
add_subdirectory(src/cpp/event_pipeline)
add_subdirectory(src/cpp/execution)
