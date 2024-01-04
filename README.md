Welcome to StarQuant
==================

<p align="left">
   <img src ="https://img.shields.io/badge/language-c%2B%2B%7Cpython-orange.svg"/>
   <img src ="https://img.shields.io/badge/c%2B%2B-%3E11-blue.svg"/>
    <img src ="https://img.shields.io/badge/python-3.7-blue.svg" />
    <img src ="https://img.shields.io/badge/platform-linux%7Cwindows-brightgreen.svg"/>
    <img src ="https://img.shields.io/badge/build-passing-green.svg" />
    <img src ="https://img.shields.io/badge/license-MIT-blue.svg"/>
</p>

[English](README_eng.md) 



**StarQuant** is a lightweight, comprehensive quantitative trading backtesting system for individual (ordinary) users. It is currently mainly used for futures and options programmed trading (CTP interface, in real market testing), and is also Supports stock trading functions (Zhongtai xtp, Kuanrui oes interface, to be tested and improved).

Community-verified icon




Current Progress
2022.10 The backtest version is updated, see the backtest branch, and supports common backtest functions in the Pro version.

2020.12: Completed the beta version of version 1.0, in the ctp real disk test:

1) For varieties with good liquidity and large market openings, the transaction time and price of tick-level backtesting and actual trading under non-large transactions are consistent;

2) Alibaba Cloud/Tencent Cloud works continuously 7*24 hours without manual intervention, the API counter automatically reconnects and disconnects, the market price is automatically recorded, and the strategy is automatically initialized, started and stopped;



## Features

* Using multi-process and multi-thread mode, the market trading interface, strategy execution, and GUI interface are all independent processes, supporting multiple process communication modes. For ordinary applications, message queues can be used, with delays in the order of hundreds of microseconds (actually measured at 30 -100 microseconds, the throughput is around 10k/s, which is enough for futures), and the delay is low enough for ordinary users. For high throughput and low latency of stock L2 tick-by-tick data, the shared memory communication mode can be used to achieve 10 microseconds. Class-low latency, all transaction data is logged at the same time;
* Supports real offer programmed trading, supports multiple API interfaces (such as CTP penetration for futures, Yishengtap, Zhongtai XTP for stocks, Kuanrui OES), supports simultaneous login of multiple futures and securities company accounts, multiple The policy group operates independently, and can set trigger conditions to realize automatic login, logout, reset, etc., and can achieve 7*24 hours of work; it has a risk control module, which can set parameters such as flow control and total volume control; supports local condition orders (stop loss), one-click total withdrawal, position closing and other custom instruction types;
* Supports real-time dynamic management of strategies (add, delete, edit parameters, start, stop, reload, etc., similar to vnpy's ctastrategy module), and can manage multiple strategy processes;
* Supports real-time dynamic management of strategies (add, delete, edit parameters, start, stop, reload, etc., similar to vnpy's ctastrategy module), and can manage multiple strategy processes;
* Supports bar and tick backtesting at different time scales; has lite, pro and batch backtesting modes. The lite mode is a single-bid mode. In batch mode, multiple processes can be used to backtest different strategies and contracts in batches at the same time, making it easy to combine different strategies. Check the comprehensive income. The pro mode supports simultaneous backtesting of any number of targets. Regular expressions determine the name range of the target. The batch mode supports simultaneous backtesting of multiple processes, making it easy to separate the targets to achieve parallel and fast backtesting;
* The backtest results can view the overall indicators (yield, Sharpe rate, maximum retracement, etc.), yield curve, all transaction details, daily details, display corresponding buying and selling point marks on the K line, and display tick data for easy analysis; you can Filter transactions under specified conditions, and backtest results can be exported as csv and corresponding pictures; support custom indicator display and analysis;
* Supports backtesting parameter optimization and screening functions, allowing multi-process parameter optimization and genetic algorithm optimization;
* Supports real market quotation subscription and data storage (tick, bar), message queue communication mode is similar to vnpy's data recorder, separate process mode, and also has the function of vnpy's csv loader, supports bar and tick data import; shared memory communication In mode, the memory data is directly saved asynchronously on the hard disk, and there is a special export viewing script for data processing and analysis; it supports downloading data from multiple data sources (RQData, Tushare, JoinQuant);
* Support simulated trading (Paper brokerage, simple matching) based on real offer market data to facilitate testing before real offer;
* Use Qt visual interface as the front end to facilitate management, monitoring and operation. Relevant monitoring information is recorded and can be exported to csv files; real-time K-line data can be viewed; basic contract information can be viewed; specified view controls can be selectively displayed/hidden Unit, adjust view layout; also supports command line (cli, tui) form monitoring;
* Support WeChat real-time push and receive information (itchat or Server sauce, etc.)
*Linux, windows cross-platform support;



## PRO version features
* Backtesting supports any number of targets; supports multi-process parallel batch backtesting with different strategy combinations; backtesting results can be filtered according to specified conditions;
*Mongodb data reading and writing is based on the batch mode of pymongo, which is 2-10 times faster than the normal version;
* Backtesting supports data reading cursor mode to save memory consumption and supports batch combination of different backtesting results;
* Backtesting supports setting saving, backtesting result reading, and script mode;
* Backtesting supports distributed computing and optimization;
* Supports factor statistical analysis, correlation analysis, batch factor generation and regression analysis to facilitate rapid factor screening;
* Supports vectorized fast backtesting (intraday and overnight) based on the prediction model, and the test set and training set are evaluated separately;
* Supports customized display of various indicators, including order flow;
* Support tick-level target combination price (spread) analysis and indicator analysis, support tick data analysis and indicator display
* The real disk supports shared memory communication, with a delay of 10 microseconds and a throughput of 100,000/second;

Note: The current open source code has all basic functions from backtesting to real trading, and also shows the prototype and framework of the system to facilitate secondary development and customization on this basis. The customized code (pro version) is currently open source Some functions have been added, and some functions have not yet been open sourced. In the future, if the number of stars is large (more than 1k), open source can be considered.

## system structure

The main framework of the system is implemented based on C++ and adopts c-s architecture. Backtesting has two modes: event-driven mode and vectorization. It adopts modular loose coupling design. The market, transaction and data records on the server side are separate threads. The server side and GUI interface and strategy The process communication between them adopts message queue mode (nanomsg) or shared memory (based on Kung Fu Yi Jin Jing). Market data can be forwarded to the strategy process in the form of messages through relevant ports. The strategy order operation also forwards instructions to the service through relevant ports. terminal, and then call the relevant counter api. The market api supports CTP, TAP, etc. The data can be recorded locally (csv file or Mongodb database), and the strategy can be implemented in python or c++. The GUI is based on PyQt5 and supports manual trading, strategic trading, entrusted position account and other information viewing.


## Development environment
During the development process of this system, we referred to the existing open source software vnpy, kungfu, etc.
Main development environment:

(1) Manjaro（arch，Linux内核4.14)，python 3.8，gcc 9

(2) Ubuntu20.04, 

(3) Windows 10 ,vs2019

Third-party libraries:
(1) System installation required: boost, libmongoc-1.0

(2) You need to install the system or compile nanomsg 1.0, log4cplus 2.04, yaml-cpp, fmt5.3, ta-lib under cppsrc/ by yourself.

(3) python depends on psutil, pyyaml, pyqt, qdarkstyle and other packages, see requirement.txt under the backtest branch

## Run

First, you need to compile the library under cppsrc, and install the dynamic link libraries of boost, nanomsg, CTP, TAP and other counter APIs in advance.
The compilation process is similar to the original project. Use [CMake](https://cmake.org) to compile:

```
$ cd cppsrc
$ mkdir build
$ cd build
$ cmake ..
$ make
```
After compilation is completed, copy the executable file apiserver.exe under cppsrc/build/StarQuant to the home directory and run it to start the server.
The gui interface can be started by executing gui.py, running strategy.py can start a separate strategy process, and running recorder.py can start the market recording process.

## Writing convention
-------------------
For c++, refer to Google’s c++ guide, cpplint check;

python uses flake8 checking and autopep8 formatting;

Variable naming: Class names use camel case, with the first letter of the word capitalized and not containing the _ character; class member variable letters are generally lowercase, followed by _, such as data_, global variables are modified with g_, and functions are generally in mixed case.



## Instructions for use
-------
Variety symbol convention:
  Take the form of full name: exchange type commodity name contract, such as SHFE F RB 1905
  For ctp, the program's internal API will be converted into the corresponding abbreviation form, rb1905,
The format of message transmission between market, transaction and strategy: message header | message content
 Message header: destination|source address|type, the types are:

 Message content: data of corresponding type

## Demo
----------
![ ](demos/demopro.gif "Backtest Demo 1")
![ ](demos/live3.png "Regular version of real trading mode display")
![ ](demos/livepro.png "Customized version of real trading display")
![ ](demos/btpro4.png "Customized version of backtest order flow analysis display")
![ ](demos/btpro1.png "Batch backtest")
![ ](demos/btpro2.png "Backtest result screening, K-line, indicator analysis")

## TODO

This system has completed the beta version of version 1.0 and is currently undergoing real-time testing. At the same time, the C++ backtest, vectorized backtest, and cli interface are being improved.




