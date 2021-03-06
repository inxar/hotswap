# Hotswap

*Dynamic class reloading and object state migration for the JVM*

## About

**HotSwap** provides a robust
library for updating the implementation of an object at runtime,
otherwise known as *hotswapping*.  This is achieved through
recompilation, dynamic class reloading, and object state migration
throughout the life of an application. It is
fundamentally useful for runtime incremental development of an application.

## Example

```java
// Assume you have an interface 'Transformer' in your classpath
// in src/main/java.  It has a single method 'Object transform(Object)'.
// Further assume that 'MatrixTransformer' is an implementation of
// Transformer in src/test/java.

// Create a compiler instance which acts as a factory for
// ProxyClass objects.
ProxyCompiler compiler = new KJavacCompiler();
compiler.setClasspath("target/classes");
compiler.setSourcepath("src/test/java");
compiler.setDestinationpath("target/hotswap");

// Create a ProxyClass.  A monitor is setup that watches for
// source files changes in the file.
ProxyClass cls = compiler.load("org.example.MatrixTransformer");

// Create a Proxy object that implements Transformer
Transformer t = (Transformer)cls.newInstance();

// Before the transform method is actually invoked, a check is performed
// to see if the source file has changed.  If so, the class is
// recompiled and the object instance is swapped out.  The result
// object will have been generated by the updated code.  Cool!
Object input = ...
Object output = t.transform(input);
```

# LICENSE

MIT

# Requirements

WHen it was originally written in 2001, the reqs were JDK1.2 for basic
use, JDK1.3 for use of <a
href="http://java.sun.com/j2se/1.3/docs/guide/reflection/proxy.html">Dynamic
Proxy Classes</a>.  With the `ProxyClassWatcher` implementation, JDK
1.7 is required (`java.nio.file.WatchService`).  If necessary, a
custom alternative `ProxyClassMonitor` implementation can be provided,
removing the JDK1.7 requirement.

# Download

```
Maven central
```

# Installation

Include the <code>hotswap.jar</code> in your classpath.  You also need
to include <code>$JAVA_HOME/lib/tools.jar</code> in the classpath in
order to use <code>KJavacCompiler</code>.

<a name="documentation">
# Documentation

The manual is the <code>org.inxar.hotswap.*</code> package
description apidocs.
