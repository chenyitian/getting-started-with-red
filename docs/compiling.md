# 2. Compiling Native Red Apps From Source

Red is being actively developed, and new features are available regularly. To use the most recent development version of Red on any supported platform, or to produce native binaries for your chosen platform, you can compile your Red scripts using the Red source code:

- Download the source repository at [https://github.com/red/red](https://github.com/red/red). Click the "Download Zip" link, then unzip the package to a folder on your hard drive. For the purpose of this tutorial, we'll assume that you've unzipped the package into C:\ on a Windows machine.
- Download the Rebol interpreter from [http://www.rebol.com/download-core.html](http://www.rebol.com/download-core.html), and copy it directly into the root directory of the unpacked source folder above (i.e., C:\red\)

That's it. To compile a script:

- Open the Rebol interpreter
- At the console, type "do/args %red.r {%yourscript.red}"

When compilation is complete, you'll see the file name of the created executable displayed (i.e., yourscript.exe).

That's all it takes to get Red programs running from source.

NOTE: the console will allow you to see results much more quickly and naturally. You'll notice that some interactive examples are wrapped in "do []". This allows the code to be pasted into the interpreter before functions such as "ask", which require user input, are evaluated.