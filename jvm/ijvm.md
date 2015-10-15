# in the name of ALLAH


Note:
- 32bit JVM is faster than 64bit( about 20% ).
- By "Ordinary Object Pointers - OOPs" the 32bit JVM can address about 32GB of RAM.

# Basic Tools
- "jstat -gcutil <pid>" , "jstat -gccapacity <pid>" , "jstat -gc <pid>" , ...
- "jcmd <pid> VM.version" , "jcmd <pid> VM.command_line" , ...
- "jps"
- "java -XX:+PrintFlagsFinal ..."

# Code Cache Size
- has fixed maximum size.
- "-XX:ReservedCodeCacheSize=N" & "-XX:InitailCodeCacheSize" ( N => 1K, 2k, 3M, 4M, 5G, 6g )
- default value in Java8( C1 = 32m, C2 & d64 = 240m )
- default value in Java7( C1 = 32m, C2 = 32m, -d64 = 48m, -d64 & TieredCompilation = 96m )
- "Code Cache Size" matters for GC, JIT, 32bit JVM ( limited to < 4G memory )


# JIT-Compilre
- Just compiles Recent "Hot-Code"!
- "Hot-Code" depends on "-XX:CompilerThreshold=N" ( N => C1 = 1500, C2/d64 = 10000 )
- "Hot-Code"s can be "method"s or "loop-block"s
- The JIT for "loop-block" should do "on stack replacement - OSR"
- The OSR compilation is triggered by:
  trigger = ( CompilerThreshold * ((OnStackReplacementPercentage - InterpreterProfilePercentage)/100) )
- Periodically ( When JVM reaches a safepoint ) the value of counter is reduced.

- C1 or "client" by "-client"
- C2 or "server" by "-server"
- d64 by "-d64"
- Also "-XX:+TieredCompilation"
- In 64bit compiled JVM you actually use "server".
- The "java -client -XX:+TieredCompilation" will disable tiered-compilation silently.
- C2 is default compiler in Java8
- In the output of "java -version" you can check the jit-compiler.

- "-XX:+PrintCompilation"; default is false
- "timestamp compilation_id attributes (tiered_level) method_name size deopt"
- attributes: % = OSR , s = synchronized , ! = has exception handler , b = compilation in blocking mode , n = wrapper for native
- size is for java-byte-code, not for compiled code
- "jstat -compiler <pid>"
- "jstat -printcompilation <pid> <delay>"

- Compilation can failed because of: "Code cache filled" or "Concurrent class loading"
- Compilation Threads: "-XX:CICompilerCount=N" for total threads to use by jit-compiler.
- (Non)Blocking Compilation : "-XX:+BackgroundCompilation" ( default : true )

# Inlining
- "-XX:+Inline" ( default : true )
- Disabling this feature might cause 50% Performance decrement.
- Depends on method size ( bytes )
- "-XX:MaxFreqInlineSize=N" ( default : 325 bytes ) | For methods that frequently called.
- "-XX:MaxInlineSize=N" ( default : 35 bytes ) | For every method.
- "final" keyword don't affect peformance and inlining ( for recent java versions ).
- Don't be afraid of small methods

# Escape Analysis
- "-XX:+DoEscapeAnalysis" ( default : true ).
- Simpler Code == Better Optimization

# Deoptimization: Not Entrant Code or Zombie Codes.

# Tiered Compilation Levels :
- 0 : Interpreted code
- 1 : Simple C1 compiled code
- 2 : Limited C1 compiled code
- 3 : Full C1 compiled cpde
- 4 : C2 compiled code

