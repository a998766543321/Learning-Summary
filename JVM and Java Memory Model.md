## Background

Java has **2** such environments and everyone working with Java has to start their work after setting up one of these environments on their local development or production environment platforms

* JRE (Java Runtime Environment): the minimum environment needed for running a Java application (no support for developing). It includes **JVM (Java Virtual Machine)** and deployment tools. >> for everyone 
* JDK (Java Development Kit): the complete development environment used for developing and executing Java applications. It includes both JRE and development tools. >> for developers


Java source codes are compiled into an intermediate state called **bytecode** (i.e. a **.class** file) using the **Java Compiler (javac)** which comes inbuilt with **JDK**.

This bytecode is in hexadecimal format with **opcode-operand** lines and **JVM** can interpret these instructions (without further recompilations) into native machine language

Therefore, bytecode acts as a **platform-independent intermediary state** which is portable among any JVM regardless of underlying OS and hardware architecture

However, since JVMs are developed to run and communicate with the underlying hardware & OS structure, we need to select the appropriate JVM version for our OS version (Windows, Linux, Mac) and processor architecture (x86, x64).

## JVM Architecture

* JVM is only a specification, and its implementation is different from vendor to vendor
* JVM resides on the RAM
* JVM is multi-threaded

## JVM components
1. Class Loader Subsystem
    * It loads, links, and initializes the class file (.class) when it refers to a class for the first time at runtime (not compile time).
