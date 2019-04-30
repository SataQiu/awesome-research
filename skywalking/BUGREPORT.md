# 问题记录

### 1. 系统兼容性

Ubuntu 18.04 环境下，
6.0.0-GA 版本 SkyWalking agent 会导致 SpringCloud GateWay 启动失败。

失败日志：

```sh
#
# A fatal error has been detected by the Java Runtime Environment:
#
#  SIGSEGV (0xb) at pc=0x00007f0213b83ab0, pid=16982, tid=139647358682880
#
# JRE version: Java(TM) SE Runtime Environment (8.0_11-b12) (build 1.8.0_11-b12)
# Java VM: Java HotSpot(TM) 64-Bit Server VM (25.11-b03 mixed mode linux-amd64 compressed oops)
# Problematic frame:
# C  0x00007f0213b83ab0
#
# Failed to write core dump. Core dumps have been disabled. To enable core dumping, try "ulimit -c unlimited" before starting Java again
#
# If you would like to submit a bug report, please visit:
#   http://bugreport.sun.com/bugreport/crash.jsp
#

---------------  T H R E A D  ---------------

Current thread (0x00007f022800e800):  JavaThread "main" [_thread_in_Java, id=16987, stack(0x00007f022f255000,0x00007f022f355000)]

siginfo:si_signo=SIGSEGV: si_errno=0, si_code=2 (SEGV_ACCERR), si_addr=0x00007f0213b83ab0

Registers:
RAX=0x0000000083a8b7e0, RBX=0x00007f0213e33b48, RCX=0x00007f02190743c0, RDX=0x000000008371b1e0
RSP=0x00007f022f352740, RBP=0x0000000000000000, RSI=0x0000000083a8b7e0, RDI=0x00007f0213e060a0
R8 =0x000000008371b1e0, R9 =0x0000000100472c28, R10=0x0000000083a8b7e0, R11=0x00007f0219941020
R12=0x0000000000000000, R13=0x00007f022f352730, R14=0x00000000d84292a8, R15=0x00007f022800e800
RIP=0x00007f0213b83ab0, EFLAGS=0x0000000000010246, CSGSFS=0x002b000000000033, ERR=0x0000000000000015
  TRAPNO=0x000000000000000e

Top of Stack: (sp=0x00007f022f352740)
0x00007f022f352740:   0000000000000000 00007f022f352790
0x00007f022f352750:   00007f022f352760 00007f021959a3d4
0x00007f022f352760:   00007f0213b53460 00007f022800e800
0x00007f022f352770:   0000000000000061 000000b900000000
0x00007f022f352780:   0000000000000080 00000000006ffcd4
0x00007f022f352790:   00007f022f3527c0 00007f021954c3bc
0x00007f022f3527a0:   00007f021954c3bc 00007f021954c3bc
0x00007f022f3527b0:   00000000d8429298 000000008371b1c0
0x00007f022f3527c0:   00007f0213de1b88 00007f022800e800
0x00007f022f3527d0:   0000000000000000 00007f022800e800
0x00007f022f3527e0:   00007f022f352860 00007f0213de1b88
0x00007f022f3527f0:   00007f022800e800 0000000000000060
0x00007f022f352800:   00007f022f352810 00007f0219565214
0x00007f022f352810:   00007f022f352b18 00007f022f352c28
0x00007f022f352820:   00007f022f352ac0 00007f0219045bda
0x00007f022f352830:   000000000000037f 0000000000000000
0x00007f022f352840:   0000000000000000 0000ffff00001fa0
0x00007f022f352850:   0000000000000000 0000000000000000
0x00007f022f352860:   0000000000000000 0000000000000000
0x00007f022f352870:   0000000000000000 0000000000000000
0x00007f022f352880:   0000000000000000 0000000000000000
0x00007f022f352890:   0000000000000000 0000000000000000
0x00007f022f3528a0:   0000000000000000 0000000000000000
0x00007f022f3528b0:   00000000d8429298 00007f021932cc9d
0x00007f022f3528c0:   0000000083a8b768 00000000d8429298
0x00007f022f3528d0:   0000000000000002 0000000000000000
0x00007f022f3528e0:   00007f022f352a20 00007f0219564a4c
0x00007f022f3528f0:   ffffffffffffffff ffffffffffffffff
0x00007f022f352900:   ffffffffffffffff ffffffffffffffff
0x00007f022f352910:   00000000d8429260 00007f021932cc9d
0x00007f022f352920:   00000000d8429260 0000000083a8b750
0x00007f022f352930:   0000000083a8b700 00007f022874cbc0 

Instructions: (pc=0x00007f0213b83ab0)
0x00007f0213b83a90:   05 ac ff 00 dc 0f 29 1b 39 21 55 39 52 51 29 31
0x00007f0213b83aa0:   29 6a 59 39 2c 39 59 ff 0c 1d ff 16 26 00 00 00
0x00007f0213b83ab0:   50 cc 69 2e 02 7f 00 00 d0 39 b8 13 02 7f 00 00
0x00007f0213b83ac0:   00 00 00 00 00 00 00 00 20 de c4 d3 01 7f 00 00 

Register to memory mapping:

RAX=0x0000000083a8b7e0 is an oop
org.springframework.beans.factory.support.DefaultSingletonBeanRegistry$$Lambda$100/973174587 
 - klass: 'org/springframework/beans/factory/support/DefaultSingletonBeanRegistry$$Lambda$100'
RBX=0x00007f0213e33b48 is an unknown value
RCX=0x00007f02190743c0 is at begin+12 in a stub
MethodHandle::interpreter_entry::_linkToStatic [0x00007f02190743b4, 0x00007f02190743d8[ (36 bytes)
RDX=0x000000008371b1e0 is an oop
java.lang.invoke.MemberName 
 - klass: 'java/lang/invoke/MemberName'
RSP=0x00007f022f352740 is pointing into the stack for thread: 0x00007f022800e800
RBP=0x0000000000000000 is an unknown value
RSI=0x0000000083a8b7e0 is an oop
org.springframework.beans.factory.support.DefaultSingletonBeanRegistry$$Lambda$100/973174587 
 - klass: 'org/springframework/beans/factory/support/DefaultSingletonBeanRegistry$$Lambda$100'
RDI=0x00007f0213e060a0 is an unknown value
R8 =0x000000008371b1e0 is an oop
java.lang.invoke.MemberName 
 - klass: 'java/lang/invoke/MemberName'
R9 =0x0000000100472c28 is an unknown value
R10=0x0000000083a8b7e0 is an oop
org.springframework.beans.factory.support.DefaultSingletonBeanRegistry$$Lambda$100/973174587 
 - klass: 'org/springframework/beans/factory/support/DefaultSingletonBeanRegistry$$Lambda$100'
R11=0x00007f0219941020 is at entry_point+0 in (nmethod*)0x00007f0219940ed0
R12=0x0000000000000000 is an unknown value
R13=0x00007f022f352730 is pointing into the stack for thread: 0x00007f022800e800
R14=0x00000000d84292a8 is an oop

[error occurred during error reporting (printing register info), id 0xb]

Stack: [0x00007f022f255000,0x00007f022f355000],  sp=0x00007f022f352740,  free space=1013k
Native frames: (J=compiled Java code, j=interpreted, Vv=VM code, C=native code)
C  0x00007f0213b83ab0

[error occurred during error reporting (printing native stack), id 0xb]


---------------  P R O C E S S  ---------------

Java Threads: ( => current thread )
  0x00007f0228dea000 JavaThread "spring.cloud.inetutils" daemon [_thread_in_native, id=17131, stack(0x00007f0208027000,0x00007f0208128000)]
  0x00007f0228b3b000 JavaThread "spring.cloud.inetutils" daemon [_thread_in_native, id=17072, stack(0x00007f02106c8000,0x00007f02107c9000)]
  0x00007f01ec002800 JavaThread "grpc-default-worker-ELG-1-4" daemon [_thread_in_native, id=17071, stack(0x00007f0208328000,0x00007f0208429000)]
  0x00007f01f0015800 JavaThread "grpc-default-worker-ELG-1-3" daemon [_thread_in_native, id=17070, stack(0x00007f0208429000,0x00007f020852a000)]
  0x00007f01e80c8000 JavaThread "grpc-default-worker-ELG-1-2" daemon [_thread_in_native, id=17069, stack(0x00007f02087d8000,0x00007f02088d9000)]
  0x00007f01e006a800 JavaThread "grpc-default-executor-0" daemon [_thread_blocked, id=17067, stack(0x00007f02104c6000,0x00007f02105c7000)]
  0x00007f01e0063800 JavaThread "grpc-default-worker-ELG-1-1" daemon [_thread_in_native, id=17066, stack(0x00007f02105c7000,0x00007f02106c8000)]
  0x00007f0228646000 JavaThread "Service Thread" daemon [_thread_blocked, id=17003, stack(0x00007f0211519000,0x00007f021161a000)]
  0x00007f0228643800 JavaThread "C1 CompilerThread1" daemon [_thread_blocked, id=17002, stack(0x00007f021161b000,0x00007f021171b000)]
  0x00007f0228641800 JavaThread "C2 CompilerThread0" daemon [_thread_blocked, id=17001, stack(0x00007f021171c000,0x00007f021181c000)]
  0x00007f022863f000 JavaThread "SkywalkingAgent-4-ServiceAndEndpointRegisterClient-0" daemon [_thread_blocked, id=17000, stack(0x00007f021181c000,0x00007f021191d000)]
  0x00007f022863d000 JavaThread "SkywalkingAgent-3-JVMService-consume-0" daemon [_thread_blocked, id=16999, stack(0x00007f021191d000,0x00007f0211a1e000)]
  0x00007f022863a800 JavaThread "SkywalkingAgent-2-JVMService-produce-0" daemon [_thread_blocked, id=16998, stack(0x00007f0211a1e000,0x00007f0211b1f000)]
  0x00007f0228638800 JavaThread "SkywalkingAgent-1-GRPCChannelManager-0" daemon [_thread_blocked, id=16997, stack(0x00007f0211b1f000,0x00007f0211c20000)]
  0x00007f0228633800 JavaThread "DataCarrier.default.Consumser.0.Thread" daemon [_thread_blocked, id=16996, stack(0x00007f0211c20000,0x00007f0211d21000)]
  0x00007f0228376800 JavaThread "Thread-1" daemon [_thread_blocked, id=16995, stack(0x00007f0211d88000,0x00007f0211e89000)]
  0x00007f0228133000 JavaThread "Signal Dispatcher" daemon [_thread_blocked, id=16994, stack(0x00007f02121c8000,0x00007f02122c9000)]
  0x00007f0228104000 JavaThread "Finalizer" daemon [_thread_blocked, id=16993, stack(0x00007f0212c98000,0x00007f0212d99000)]
  0x00007f0228100000 JavaThread "Reference Handler" daemon [_thread_blocked, id=16992, stack(0x00007f0212d99000,0x00007f0212e9a000)]
=>0x00007f022800e800 JavaThread "main" [_thread_in_Java, id=16987, stack(0x00007f022f255000,0x00007f022f355000)]

Other Threads:
  0x00007f02280fb000 VMThread [stack: 0x00007f0212e9b000,0x00007f0212f9b000] [id=16991]
  0x00007f022864b000 WatcherThread [stack: 0x00007f0211419000,0x00007f0211519000] [id=17004]

VM state:not at safepoint (normal execution)

VM Mutex/Monitor currently owned by a thread: None

Heap:
 PSYoungGen      total 154112K, used 38773K [0x00000000d6700000, 0x00000000e0980000, 0x0000000100000000)
  eden space 142848K, 21% used [0x00000000d6700000,0x00000000d854eb58,0x00000000df280000)
  from space 11264K, 68% used [0x00000000dfe00000,0x00000000e058e8f0,0x00000000e0900000)
  to   space 11776K, 0% used [0x00000000df280000,0x00000000df280000,0x00000000dfe00000)
 ParOldGen       total 24064K, used 17464K [0x0000000083400000, 0x0000000084b80000, 0x00000000d6700000)
  object space 24064K, 72% used [0x0000000083400000,0x000000008450e160,0x0000000084b80000)
 Metaspace       used 41358K, capacity 42740K, committed 42880K, reserved 1085440K
  class space    used 5898K, capacity 6200K, committed 6272K, reserved 1048576K

Card table byte_map: [0x00007f0218859000,0x00007f0218c40000] byte_map_base: 0x00007f021843f000

Marking Bits: (ParMarkBitMap*) 0x00007f022e6f0c20
 Begin Bits: [0x00007f021465e000, 0x00007f021658e000)
 End Bits:   [0x00007f021658e000, 0x00007f02184be000)

Polling page: 0x00007f022f36c000

CodeCache: size=245760Kb used=12854Kb max_used=12866Kb free=232905Kb
 bounds [0x00007f0219000000, 0x00007f0219ca0000, 0x00007f0228000000]
 total_blobs=5137 nmethods=4676 adapters=374
 compilation: enabled

Compilation events (10 events):
Event: 17.705 Thread 0x00007f0228643800 4788       3       org.springframework.asm.SymbolTable::hash (7 bytes)
Event: 17.705 Thread 0x00007f0228643800 nmethod 4788 0x00007f0219c8d350 code [0x00007f0219c8d4a0, 0x00007f0219c8d5d0]
Event: 17.706 Thread 0x00007f0228643800 4789       3       org.springframework.asm.SymbolTable::addMergedType (148 bytes)
Event: 17.707 Thread 0x00007f0228643800 nmethod 4789 0x00007f0219c8d650 code [0x00007f0219c8d820, 0x00007f0219c8def8]
Event: 17.708 Thread 0x00007f0228643800 4790       3       org.springframework.asm.Frame::merge (621 bytes)
Event: 17.709 Thread 0x00007f0228643800 nmethod 4790 0x00007f0219c8e450 code [0x00007f0219c8e6e0, 0x00007f0219c8f918]
Event: 17.709 Thread 0x00007f0228641800 4791       4       java.util.HashSet::add (20 bytes)
Event: 17.718 Thread 0x00007f0228641800 nmethod 4791 0x00007f0219c93790 code [0x00007f0219c93940, 0x00007f0219c93f28]
Event: 17.720 Thread 0x00007f0228641800 4792       4       sun.reflect.ByteVectorImpl::add (38 bytes)
Event: 17.722 Thread 0x00007f0228641800 nmethod 4792 0x00007f0219c90710 code [0x00007f0219c90860, 0x00007f0219c90968]

GC Heap History (10 events):
Event: 10.452 GC heap before
{Heap before GC invocations=30 (full 3):
 PSYoungGen      total 114176K, used 7243K [0x00000000d6700000, 0x00000000df480000, 0x0000000100000000)
  eden space 105984K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000dce80000)
  from space 8192K, 88% used [0x00000000dce80000,0x00000000dd592f68,0x00000000dd680000)
  to   space 9216K, 0% used [0x00000000deb80000,0x00000000deb80000,0x00000000df480000)
 ParOldGen       total 12800K, used 11314K [0x0000000083400000, 0x0000000084080000, 0x00000000d6700000)
  object space 12800K, 88% used [0x0000000083400000,0x0000000083f0cad0,0x0000000084080000)
 Metaspace       used 31802K, capacity 32470K, committed 32768K, reserved 1077248K
  class space    used 4618K, capacity 4779K, committed 4864K, reserved 1048576K
Event: 10.540 GC heap after
Heap after GC invocations=30 (full 3):
 PSYoungGen      total 114176K, used 1032K [0x00000000d6700000, 0x00000000df480000, 0x0000000100000000)
  eden space 105984K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000dce80000)
  from space 8192K, 12% used [0x00000000dce80000,0x00000000dcf82000,0x00000000dd680000)
  to   space 9216K, 0% used [0x00000000deb80000,0x00000000deb80000,0x00000000df480000)
 ParOldGen       total 17920K, used 12786K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 71% used [0x0000000083400000,0x000000008407c878,0x0000000084580000)
 Metaspace       used 31802K, capacity 32470K, committed 32768K, reserved 1077248K
  class space    used 4618K, capacity 4779K, committed 4864K, reserved 1048576K
}
Event: 13.020 GC heap before
{Heap before GC invocations=31 (full 3):
 PSYoungGen      total 114176K, used 107016K [0x00000000d6700000, 0x00000000df480000, 0x0000000100000000)
  eden space 105984K, 100% used [0x00000000d6700000,0x00000000dce80000,0x00000000dce80000)
  from space 8192K, 12% used [0x00000000dce80000,0x00000000dcf82000,0x00000000dd680000)
  to   space 9216K, 0% used [0x00000000deb80000,0x00000000deb80000,0x00000000df480000)
 ParOldGen       total 17920K, used 12786K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 71% used [0x0000000083400000,0x000000008407c878,0x0000000084580000)
 Metaspace       used 34954K, capacity 35820K, committed 35968K, reserved 1079296K
  class space    used 5031K, capacity 5197K, committed 5248K, reserved 1048576K
Event: 13.074 GC heap after
Heap after GC invocations=31 (full 3):
 PSYoungGen      total 130560K, used 6934K [0x00000000d6700000, 0x00000000df300000, 0x0000000100000000)
  eden space 122880K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000ddf00000)
  from space 7680K, 90% used [0x00000000deb80000,0x00000000df245b38,0x00000000df300000)
  to   space 10240K, 0% used [0x00000000ddf00000,0x00000000ddf00000,0x00000000de900000)
 ParOldGen       total 17920K, used 13869K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 77% used [0x0000000083400000,0x000000008418b7d0,0x0000000084580000)
 Metaspace       used 34954K, capacity 35820K, committed 35968K, reserved 1079296K
  class space    used 5031K, capacity 5197K, committed 5248K, reserved 1048576K
}
Event: 14.799 GC heap before
{Heap before GC invocations=32 (full 3):
 PSYoungGen      total 130560K, used 129814K [0x00000000d6700000, 0x00000000df300000, 0x0000000100000000)
  eden space 122880K, 100% used [0x00000000d6700000,0x00000000ddf00000,0x00000000ddf00000)
  from space 7680K, 90% used [0x00000000deb80000,0x00000000df245b38,0x00000000df300000)
  to   space 10240K, 0% used [0x00000000ddf00000,0x00000000ddf00000,0x00000000de900000)
 ParOldGen       total 17920K, used 13869K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 77% used [0x0000000083400000,0x000000008418b7d0,0x0000000084580000)
 Metaspace       used 37250K, capacity 38178K, committed 38400K, reserved 1081344K
  class space    used 5324K, capacity 5519K, committed 5632K, reserved 1048576K
Event: 14.906 GC heap after
Heap after GC invocations=32 (full 3):
 PSYoungGen      total 133120K, used 8815K [0x00000000d6700000, 0x00000000e0900000, 0x0000000100000000)
  eden space 122880K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000ddf00000)
  from space 10240K, 86% used [0x00000000ddf00000,0x00000000de79bed0,0x00000000de900000)
  to   space 11264K, 0% used [0x00000000dfe00000,0x00000000dfe00000,0x00000000e0900000)
 ParOldGen       total 17920K, used 16274K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 90% used [0x0000000083400000,0x00000000843e4878,0x0000000084580000)
 Metaspace       used 37250K, capacity 38178K, committed 38400K, reserved 1081344K
  class space    used 5324K, capacity 5519K, committed 5632K, reserved 1048576K
}
Event: 14.906 GC heap before
{Heap before GC invocations=33 (full 4):
 PSYoungGen      total 133120K, used 8815K [0x00000000d6700000, 0x00000000e0900000, 0x0000000100000000)
  eden space 122880K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000ddf00000)
  from space 10240K, 86% used [0x00000000ddf00000,0x00000000de79bed0,0x00000000de900000)
  to   space 11264K, 0% used [0x00000000dfe00000,0x00000000dfe00000,0x00000000e0900000)
 ParOldGen       total 17920K, used 16274K [0x0000000083400000, 0x0000000084580000, 0x00000000d6700000)
  object space 17920K, 90% used [0x0000000083400000,0x00000000843e4878,0x0000000084580000)
 Metaspace       used 37250K, capacity 38178K, committed 38400K, reserved 1081344K
  class space    used 5324K, capacity 5519K, committed 5632K, reserved 1048576K
Event: 14.994 GC heap after
Heap after GC invocations=33 (full 4):
 PSYoungGen      total 133120K, used 0K [0x00000000d6700000, 0x00000000e0900000, 0x0000000100000000)
  eden space 122880K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000ddf00000)
  from space 10240K, 0% used [0x00000000ddf00000,0x00000000ddf00000,0x00000000de900000)
  to   space 11264K, 0% used [0x00000000dfe00000,0x00000000dfe00000,0x00000000e0900000)
 ParOldGen       total 24064K, used 17464K [0x0000000083400000, 0x0000000084b80000, 0x00000000d6700000)
  object space 24064K, 72% used [0x0000000083400000,0x000000008450e160,0x0000000084b80000)
 Metaspace       used 37250K, capacity 38178K, committed 38400K, reserved 1081344K
  class space    used 5324K, capacity 5519K, committed 5632K, reserved 1048576K
}
Event: 17.229 GC heap before
{Heap before GC invocations=34 (full 4):
 PSYoungGen      total 133120K, used 122880K [0x00000000d6700000, 0x00000000e0900000, 0x0000000100000000)
  eden space 122880K, 100% used [0x00000000d6700000,0x00000000ddf00000,0x00000000ddf00000)
  from space 10240K, 0% used [0x00000000ddf00000,0x00000000ddf00000,0x00000000de900000)
  to   space 11264K, 0% used [0x00000000dfe00000,0x00000000dfe00000,0x00000000e0900000)
 ParOldGen       total 24064K, used 17464K [0x0000000083400000, 0x0000000084b80000, 0x00000000d6700000)
  object space 24064K, 72% used [0x0000000083400000,0x000000008450e160,0x0000000084b80000)
 Metaspace       used 40394K, capacity 41626K, committed 41856K, reserved 1085440K
  class space    used 5738K, capacity 6011K, committed 6016K, reserved 1048576K
Event: 17.281 GC heap after
Heap after GC invocations=34 (full 4):
 PSYoungGen      total 154112K, used 7738K [0x00000000d6700000, 0x00000000e0980000, 0x0000000100000000)
  eden space 142848K, 0% used [0x00000000d6700000,0x00000000d6700000,0x00000000df280000)
  from space 11264K, 68% used [0x00000000dfe00000,0x00000000e058e8f0,0x00000000e0900000)
  to   space 11776K, 0% used [0x00000000df280000,0x00000000df280000,0x00000000dfe00000)
 ParOldGen       total 24064K, used 17464K [0x0000000083400000, 0x0000000084b80000, 0x00000000d6700000)
  object space 24064K, 72% used [0x0000000083400000,0x000000008450e160,0x0000000084b80000)
 Metaspace       used 40394K, capacity 41626K, committed 41856K, reserved 1085440K
  class space    used 5738K, capacity 6011K, committed 6016K, reserved 1048576K
}

Deoptimization events (10 events):
Event: 13.687 Thread 0x00007f022800e800 Uncommon trap: reason=bimorphic action=maybe_recompile pc=0x00007f021971be84 method=org.apache.skywalking.apm.dependencies.net.bytebuddy.description.type.TypeDescription$Generic$OfNonGenericType.getSuperClass()Lorg/apache/skywalking/apm/dependencies/net/b
Event: 13.687 Thread 0x00007f022800e800 Uncommon trap: reason=bimorphic action=maybe_recompile pc=0x00007f021971be84 method=org.apache.skywalking.apm.dependencies.net.bytebuddy.description.type.TypeDescription$Generic$OfNonGenericType.getSuperClass()Lorg/apache/skywalking/apm/dependencies/net/b
Event: 13.687 Thread 0x00007f022800e800 Uncommon trap: reason=bimorphic action=maybe_recompile pc=0x00007f021971be84 method=org.apache.skywalking.apm.dependencies.net.bytebuddy.description.type.TypeDescription$Generic$OfNonGenericType.getSuperClass()Lorg/apache/skywalking/apm/dependencies/net/b
Event: 15.057 Thread 0x00007f022800e800 Uncommon trap: reason=null_check action=make_not_entrant pc=0x00007f0219a6064c method=java.net.URLClassLoader.findResource(Ljava/lang/String;)Ljava/net/URL; @ 16
Event: 15.480 Thread 0x00007f022800e800 Uncommon trap: reason=class_check action=maybe_recompile pc=0x00007f0219ae4514 method=java.io.DataOutputStream.write([BII)V @ 7
Event: 15.480 Thread 0x00007f022800e800 Uncommon trap: reason=class_check action=maybe_recompile pc=0x00007f0219ae4514 method=java.io.DataOutputStream.write([BII)V @ 7
Event: 15.480 Thread 0x00007f022800e800 Uncommon trap: reason=class_check action=maybe_recompile pc=0x00007f0219ae4514 method=java.io.DataOutputStream.write([BII)V @ 7
Event: 15.480 Thread 0x00007f022800e800 Uncommon trap: reason=class_check action=maybe_recompile pc=0x00007f0219ae4514 method=java.io.DataOutputStream.write([BII)V @ 7
Event: 17.705 Thread 0x00007f022800e800 Uncommon trap: reason=unreached action=reinterpret pc=0x00007f02191e90e8 method=org.springframework.asm.Frame.merge(Lorg/springframework/asm/SymbolTable;Lorg/springframework/asm/Frame;I)Z @ 397
Event: 17.715 Thread 0x00007f022800e800 Uncommon trap: reason=unreached action=reinterpret pc=0x00007f0219b1d990 method=org.springframework.asm.Type.appendDescriptor(Ljava/lang/Class;Ljava/lang/StringBuilder;)V @ 61

Internal exceptions (10 events):
Event: 17.687 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d82bd670) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.687 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d82c6ad0) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.689 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d82d65f8) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.690 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d82df9f0) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.704 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d834faa8) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.705 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d8364aa0) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.707 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d83780d0) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.716 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d83e8b90) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.718 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d83f97b8) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]
Event: 17.719 Thread 0x00007f022800e800 Exception <a 'java/security/PrivilegedActionException'> (0x00000000d84160e0) thrown at [/HUDSON/workspace/8-2-build-linux-amd64/jdk8u11/648/hotspot/src/share/vm/prims/jvm.cpp, line 1248]

Events (10 events):
Event: 17.715 Thread 0x00007f022800e800 DEOPT PACKING pc=0x00007f0219b1d990 sp=0x00007f022f351080
Event: 17.715 Thread 0x00007f022800e800 DEOPT UNPACKING pc=0x00007f0219047650 sp=0x00007f022f351048 mode 2
Event: 17.716 loading class org/springframework/objenesis/strategy/PlatformDescription
Event: 17.716 loading class org/springframework/objenesis/strategy/PlatformDescription done
Event: 17.718 loading class org/springframework/objenesis/instantiator/sun/SunReflectionFactoryInstantiator
Event: 17.718 loading class org/springframework/objenesis/instantiator/sun/SunReflectionFactoryInstantiator done
Event: 17.719 loading class org/springframework/objenesis/instantiator/sun/SunReflectionFactoryHelper
Event: 17.719 loading class org/springframework/objenesis/instantiator/sun/SunReflectionFactoryHelper done
Event: 17.721 loading class sun/reflect/SerializationConstructorAccessorImpl
Event: 17.721 loading class sun/reflect/SerializationConstructorAccessorImpl done


Dynamic libraries:
00400000-00401000 r-xp 00000000 08:01 7602635                            /opt/jdk1.8.0_11/bin/java
00600000-00601000 rw-p 00000000 08:01 7602635                            /opt/jdk1.8.0_11/bin/java
01e97000-01eb8000 rw-p 00000000 00:00 0                                  [heap]
83400000-84b80000 rw-p 00000000 00:00 0 
84b80000-d6700000 ---p 00000000 00:00 0 
d6700000-e0980000 rw-p 00000000 00:00 0 
e0980000-100000000 ---p 00000000 00:00 0 
100000000-100620000 rw-p 00000000 00:00 0 
100620000-140000000 ---p 00000000 00:00 0 
7f01d2fed000-7f01d2ffc000 r-xp 00000000 08:01 17564556                   /lib/x86_64-linux-gnu/libnss_myhostname.so.2
7f01d2ffc000-7f01d31fc000 ---p 0000f000 08:01 17564556                   /lib/x86_64-linux-gnu/libnss_myhostname.so.2
7f01d31fc000-7f01d31ff000 r--p 0000f000 08:01 17564556                   /lib/x86_64-linux-gnu/libnss_myhostname.so.2
7f01d31ff000-7f01d3200000 rw-p 00012000 08:01 17564556                   /lib/x86_64-linux-gnu/libnss_myhostname.so.2
7f01d3200000-7f01d33c0000 rw-p 00000000 00:00 0 
7f01d33c0000-7f01d3400000 ---p 00000000 00:00 0 
7f01d3400000-7f01d3600000 rw-p 00000000 00:00 0 
7f01d3600000-7f01d3800000 rw-p 00000000 00:00 0 
7f01d3800000-7f01d3a00000 rw-p 00000000 00:00 0 
7f01d3a00000-7f01d3c00000 rw-p 00000000 00:00 0 
7f01d3c00000-7f01d3e00000 rw-p 00000000 00:00 0 
7f01d3e00000-7f01d4000000 rw-p 00000000 00:00 0 
7f01d4000000-7f01d5bd7000 rw-p 00000000 00:00 0 
7f01d5bd7000-7f01d8000000 ---p 00000000 00:00 0 
7f01d8000000-7f01d8bed000 rw-p 00000000 00:00 0 
7f01d8bed000-7f01dc000000 ---p 00000000 00:00 0 
7f01dc000000-7f01dc021000 rw-p 00000000 00:00 0 
7f01dc021000-7f01e0000000 ---p 00000000 00:00 0 
7f01e0000000-7f01e0419000 rw-p 00000000 00:00 0 
7f01e0419000-7f01e4000000 ---p 00000000 00:00 0 
7f01e4000000-7f01e42c1000 rw-p 00000000 00:00 0 
7f01e42c1000-7f01e8000000 ---p 00000000 00:00 0 
7f01e8000000-7f01e80f6000 rw-p 00000000 00:00 0 
7f01e80f6000-7f01ec000000 ---p 00000000 00:00 0 
7f01ec000000-7f01ec021000 rw-p 00000000 00:00 0 
7f01ec021000-7f01f0000000 ---p 00000000 00:00 0 
7f01f0000000-7f01f027e000 rw-p 00000000 00:00 0 
7f01f027e000-7f01f4000000 ---p 00000000 00:00 0 
7f01f4000000-7f01f4021000 rw-p 00000000 00:00 0 
7f01f4021000-7f01f8000000 ---p 00000000 00:00 0 
7f01f8000000-7f01f8021000 rw-p 00000000 00:00 0 
7f01f8021000-7f01fc000000 ---p 00000000 00:00 0 
7f01fc000000-7f01fc102000 rw-p 00000000 00:00 0 
7f01fc102000-7f0200000000 ---p 00000000 00:00 0 
7f0200000000-7f0200021000 rw-p 00000000 00:00 0 
7f0200021000-7f0204000000 ---p 00000000 00:00 0 
7f0204000000-7f0204042000 rw-p 00000000 00:00 0 
7f0204042000-7f0208000000 ---p 00000000 00:00 0 
7f0208027000-7f020802a000 ---p 00000000 00:00 0 
7f020802a000-7f0208328000 rw-p 00000000 00:00 0 
7f0208328000-7f020832b000 ---p 00000000 00:00 0 
7f020832b000-7f0208429000 rw-p 00000000 00:00 0 
7f0208429000-7f020842c000 ---p 00000000 00:00 0 
7f020842c000-7f020852a000 rw-p 00000000 00:00 0 
7f020852a000-7f0208532000 r-xp 00000000 08:01 7604244                    /opt/jdk1.8.0_11/jre/lib/amd64/libmanagement.so
7f0208532000-7f0208732000 ---p 00008000 08:01 7604244                    /opt/jdk1.8.0_11/jre/lib/amd64/libmanagement.so
7f0208732000-7f0208733000 rw-p 00008000 08:01 7604244                    /opt/jdk1.8.0_11/jre/lib/amd64/libmanagement.so
7f02087d8000-7f02087db000 ---p 00000000 00:00 0 
7f02087db000-7f02088d9000 rw-p 00000000 00:00 0 
7f02088d9000-7f02088f0000 r-xp 00000000 08:01 17568525                   /lib/x86_64-linux-gnu/libresolv-2.27.so
7f02088f0000-7f0208af0000 ---p 00017000 08:01 17568525                   /lib/x86_64-linux-gnu/libresolv-2.27.so
7f0208af0000-7f0208af1000 r--p 00017000 08:01 17568525                   /lib/x86_64-linux-gnu/libresolv-2.27.so
7f0208af1000-7f0208af2000 rw-p 00018000 08:01 17568525                   /lib/x86_64-linux-gnu/libresolv-2.27.so
7f0208af2000-7f0208af4000 rw-p 00000000 00:00 0 
7f0208af4000-7f0208af9000 r-xp 00000000 08:01 17568474                   /lib/x86_64-linux-gnu/libnss_dns-2.27.so
7f0208af9000-7f0208cf9000 ---p 00005000 08:01 17568474                   /lib/x86_64-linux-gnu/libnss_dns-2.27.so
7f0208cf9000-7f0208cfa000 r--p 00005000 08:01 17568474                   /lib/x86_64-linux-gnu/libnss_dns-2.27.so
7f0208cfa000-7f0208cfb000 rw-p 00006000 08:01 17568474                   /lib/x86_64-linux-gnu/libnss_dns-2.27.so
7f0208cfb000-7f0208cfd000 r-xp 00000000 08:01 17568482                   /lib/x86_64-linux-gnu/libnss_mdns4_minimal.so.2
7f0208cfd000-7f0208efc000 ---p 00002000 08:01 17568482                   /lib/x86_64-linux-gnu/libnss_mdns4_minimal.so.2
7f0208efc000-7f0208efd000 r--p 00001000 08:01 17568482                   /lib/x86_64-linux-gnu/libnss_mdns4_minimal.so.2
7f0208efd000-7f0208efe000 rw-p 00002000 08:01 17568482                   /lib/x86_64-linux-gnu/libnss_mdns4_minimal.so.2
7f0208efe000-7f0208f01000 ---p 00000000 00:00 0 
7f0208f01000-7f020c000000 rw-p 00000000 00:00 0 
7f020c000000-7f020c027000 rw-p 00000000 00:00 0 
7f020c027000-7f0210000000 ---p 00000000 00:00 0 
7f0210020000-7f02100c6000 r--s 010bc000 08:01 7604092                    /opt/jdk1.8.0_11/jre/lib/ext/jfxrt.jar
7f02100c6000-7f02102c6000 rw-p 00000000 00:00 0 
7f02102c6000-7f02104c6000 rw-p 00000000 00:00 0 
7f02104c6000-7f02104c9000 ---p 00000000 00:00 0 
7f02104c9000-7f02105c7000 rw-p 00000000 00:00 0 
7f02105c7000-7f02105ca000 ---p 00000000 00:00 0 
7f02105ca000-7f02106c8000 rw-p 00000000 00:00 0 
7f02106c8000-7f02106cb000 ---p 00000000 00:00 0 
7f02106cb000-7f02109c9000 rw-p 00000000 00:00 0 
7f02109c9000-7f0210bc9000 rw-p 00000000 00:00 0 
7f0210bc9000-7f0210bd9000 r-xp 00000000 08:01 7604239                    /opt/jdk1.8.0_11/jre/lib/amd64/libnio.so
7f0210bd9000-7f0210dd9000 ---p 00010000 08:01 7604239                    /opt/jdk1.8.0_11/jre/lib/amd64/libnio.so
7f0210dd9000-7f0210dda000 rw-p 00010000 08:01 7604239                    /opt/jdk1.8.0_11/jre/lib/amd64/libnio.so
7f0210dda000-7f0210df0000 r-xp 00000000 08:01 7604236                    /opt/jdk1.8.0_11/jre/lib/amd64/libnet.so
7f0210df0000-7f0210fef000 ---p 00016000 08:01 7604236                    /opt/jdk1.8.0_11/jre/lib/amd64/libnet.so
7f0210fef000-7f0210ff0000 rw-p 00015000 08:01 7604236                    /opt/jdk1.8.0_11/jre/lib/amd64/libnet.so
7f0210ff0000-7f02111f0000 rw-p 00000000 00:00 0 
7f02111f0000-7f02111f7000 r--s 00000000 08:01 2100416                    /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
7f02111f7000-7f0211218000 r--p 00000000 08:01 3673271                    /usr/share/locale-langpack/zh_CN/LC_MESSAGES/libc.mo
7f0211218000-7f0211418000 rw-p 00000000 00:00 0 
7f0211418000-7f0211419000 ---p 00000000 00:00 0 
7f0211419000-7f0211519000 rw-p 00000000 00:00 0 
7f0211519000-7f021151c000 ---p 00000000 00:00 0 
7f021151c000-7f021161a000 rw-p 00000000 00:00 0 
7f021161a000-7f021161b000 ---p 00000000 00:00 0 
7f021161b000-7f021161e000 ---p 00000000 00:00 0 
7f021161e000-7f021171b000 rw-p 00000000 00:00 0 
7f021171b000-7f021171c000 ---p 00000000 00:00 0 
7f021171c000-7f021171f000 ---p 00000000 00:00 0 
7f021171f000-7f021181c000 rw-p 00000000 00:00 0 
7f021181c000-7f021181f000 ---p 00000000 00:00 0 
7f021181f000-7f021191d000 rw-p 00000000 00:00 0 
7f021191d000-7f0211920000 ---p 00000000 00:00 0 
7f0211920000-7f0211a1e000 rw-p 00000000 00:00 0 
7f0211a1e000-7f0211a21000 ---p 00000000 00:00 0 
7f0211a21000-7f0211b1f000 rw-p 00000000 00:00 0 
7f0211b1f000-7f0211b22000 ---p 00000000 00:00 0 
7f0211b22000-7f0211c20000 rw-p 00000000 00:00 0 
7f0211c20000-7f0211c23000 ---p 00000000 00:00 0 
7f0211c23000-7f0211d21000 rw-p 00000000 00:00 0 
7f0211d21000-7f0211d26000 r--s 00092000 08:01 7604087                    /opt/jdk1.8.0_11/jre/lib/jsse.jar
7f0211d26000-7f0211d28000 r--s 00007000 08:01 26482332                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/activations/apm-toolkit-opentracing-activation-6.0.0-GA.jar
7f0211d28000-7f0211d2a000 r--s 00002000 08:01 26482330                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/activations/apm-toolkit-log4j-2.x-activation-6.0.0-GA.jar
7f0211d2a000-7f0211d2c000 r--s 00003000 08:01 26482329                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/activations/apm-toolkit-log4j-1.x-activation-6.0.0-GA.jar
7f0211d2c000-7f0211d2d000 r--s 00003000 08:01 26482331                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/activations/apm-toolkit-logback-1.x-activation-6.0.0-GA.jar
7f0211d2d000-7f0211d2f000 r--s 00004000 08:01 26482333                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/activations/apm-toolkit-trace-activation-6.0.0-GA.jar
7f0211d2f000-7f0211d31000 r--s 00007000 08:01 26482324                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-elasticsearch-5.x-plugin-6.0.0-GA.jar
7f0211d31000-7f0211d33000 r--s 00002000 08:01 26482315                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-elastic-job-2.x-plugin-6.0.0-GA.jar
7f0211d33000-7f0211d35000 r--s 00003000 08:01 26482286                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/motan-plugin-6.0.0-GA.jar
7f0211d35000-7f0211d37000 r--s 00004000 08:01 26482296                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-springmvc-annotation-3.x-plugin-6.0.0-GA.jar
7f0211d37000-7f0211d39000 r--s 00006000 08:01 26482321                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-hystrix-1.x-plugin-6.0.0-GA.jar
7f0211d39000-7f0211d3b000 r--s 00008000 08:01 26482299                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-webflux-5.x-plugin-6.0.0-GA.jar
7f0211d3b000-7f0211d3c000 r--s 00003000 08:01 26482306                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-spymemcached-2.x-plugin-6.0.0-GA.jar
7f0211d3c000-7f0211d3e000 r--s 00006000 08:01 26482314                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-rocketmq-4.x-plugin-6.0.0-GA.jar
7f0211d3e000-7f0211d40000 r--s 00005000 08:01 26482317                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-httpasyncclient-4.x-plugin-6.0.0-GA.jar
7f0211d40000-7f0211d41000 r--s 00003000 08:01 26482287                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-mongodb-3.x-plugin-6.0.0-GA.jar
7f0211d41000-7f0211d43000 r--s 00006000 08:01 26482313                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-rocketmq-3.x-plugin-6.0.0-GA.jar
7f0211d43000-7f0211d44000 r--s 00004000 08:01 26482298                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-springmvc-annotation-5.x-plugin-6.0.0-GA.jar
7f0211d44000-7f0211d46000 r--s 00003000 08:01 26482319                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-servicecomb-java-chassis-0.x-plugin-6.0.0-GA.jar
7f0211d46000-7f0211d48000 r--s 00004000 08:01 26482293                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-springmvc-annotation-commons-6.0.0-GA.jar
7f0211d48000-7f0211d4a000 r--s 00007000 08:01 26482292                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-resttemplate-4.3.x-plugin-6.0.0-GA.jar
7f0211d4a000-7f0211d4d000 r--s 00011000 08:01 26482310                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-mysql-5.x-plugin-6.0.0-GA.jar
7f0211d4d000-7f0211d50000 r--s 00011000 08:01 26482281                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-jdbc-commons-6.0.0-GA.jar
7f0211d50000-7f0211d52000 r--s 00003000 08:01 26482320                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-servicecomb-java-chassis-1.x-plugin-6.0.0-GA.jar
7f0211d52000-7f0211d54000 r--s 00003000 08:01 26482307                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-sharding-jdbc-1.5.x-plugin-6.0.0-GA.jar
7f0211d54000-7f0211d56000 r--s 00002000 08:01 26482305                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-jetty-client-9.0-plugin-6.0.0-GA.jar
7f0211d56000-7f0211d58000 r--s 00003000 08:01 26482304                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-jetty-server-9.x-plugin-6.0.0-GA.jar
7f0211d58000-7f0211d5a000 r--s 00002000 08:01 26482303                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-jetty-client-9.x-plugin-6.0.0-GA.jar
7f0211d5a000-7f0211d5c000 r--s 00004000 08:01 26482311                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-h2-1.x-plugin-6.0.0-GA.jar
7f0211d5c000-7f0211d5e000 r--s 00004000 08:01 26482323                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-activemq-5.x-plugin-6.0.0-GA.jar
7f0211d5e000-7f0211d60000 r--s 00002000 08:01 26482300                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-struts2-2.x-plugin-6.0.0-GA.jar
7f0211d60000-7f0211d61000 r--s 00004000 08:01 26482328                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-canal-1.x-plugin-6.0.0-GA.jar
7f0211d61000-7f0211d63000 r--s 00001000 08:01 26482290                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/spring-commons-6.0.0-GA.jar
7f0211d63000-7f0211d65000 r--s 00005000 08:01 26482283                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-jedis-2.x-plugin-6.0.0-GA.jar
7f0211d65000-7f0211d66000 r--s 00005000 08:01 26482297                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-spring-core-patch-6.0.0-GA.jar
7f0211d66000-7f0211d68000 r--s 00007000 08:01 26482316                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-mongodb-2.x-plugin-6.0.0-GA.jar
7f0211d68000-7f0211d69000 r--s 00004000 08:01 26482285                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/tomcat-7.x-8.x-plugin-6.0.0-GA.jar
7f0211d69000-7f0211d6c000 r--s 00009000 08:01 26482312                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-postgresql-8.x-plugin-6.0.0-GA.jar
7f0211d6c000-7f0211d6e000 r--s 00006000 08:01 26482318                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-kafka-v1-plugin-6.0.0-GA.jar
7f0211d6e000-7f0211d6f000 r--s 00003000 08:01 26482302                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-nutz-mvc-annotation-1.x-plugin-6.0.0-GA.jar
7f0211d6f000-7f0211d71000 r--s 00003000 08:01 26482325                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-undertow-2.x-plugin-6.0.0-GA.jar
7f0211d71000-7f0211d73000 r--s 00003000 08:01 26482327                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/dubbo-conflict-patch-6.0.0-GA.jar
7f0211d73000-7f0211d75000 r--s 00002000 08:01 26482288                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-feign-default-http-9.x-plugin-6.0.0-GA.jar
7f0211d75000-7f0211d77000 r--s 00007000 08:01 26482309                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-grpc-1.x-plugin-6.0.0-GA.jar
7f0211d77000-7f0211d79000 r--s 00004000 08:01 26482326                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-rabbitmq-5.x-plugin-6.0.0-GA.jar
7f0211d79000-7f0211d7a000 r--s 00004000 08:01 26482282                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-httpClient-4.x-plugin-6.0.0-GA.jar
7f0211d7a000-7f0211d7c000 r--s 00005000 08:01 26482289                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-okhttp-3.x-plugin-6.0.0-GA.jar
7f0211d7c000-7f0211d7e000 r--s 00002000 08:01 26482280                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-dubbo-plugin-6.0.0-GA.jar
7f0211d7e000-7f0211d80000 r--s 00004000 08:01 26482284                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-redisson-3.x-plugin-6.0.0-GA.jar
7f0211d80000-7f0211d82000 r--s 00004000 08:01 26482308                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-xmemcached-2.x-plugin-6.0.0-GA.jar
7f0211d82000-7f0211d84000 r--s 00003000 08:01 26482322                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/sofa-rpc-plugin-6.0.0-GA.jar
7f0211d84000-7f0211d86000 r--s 00003000 08:01 26482301                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-nutz-http-1.x-plugin-6.0.0-GA.jar
7f0211d86000-7f0211d87000 r--s 00004000 08:01 26482294                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-springmvc-annotation-4.x-plugin-6.0.0-GA.jar
7f0211d87000-7f0211d88000 r--s 00002000 08:01 26482295                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-spring-cloud-feign-1.x-plugin-6.0.0-GA.jar
7f0211d88000-7f0211d8b000 ---p 00000000 00:00 0 
7f0211d8b000-7f0211e89000 rw-p 00000000 00:00 0 
7f0211e89000-7f0211e8b000 r--s 00005000 08:01 26482291                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/plugins/apm-spring-concurrent-util-4.x-plugin-6.0.0-GA.jar
7f0211e8b000-7f0211e9e000 r--s 00346000 08:01 7604073                    /opt/jdk1.8.0_11/jre/lib/resources.jar
7f0211e9e000-7f0211eba000 r--s 00393000 08:01 7604091                    /opt/jdk1.8.0_11/jre/lib/ext/cldrdata.jar
7f0211eba000-7f0211ec4000 r--s 0021b000 08:01 7604097                    /opt/jdk1.8.0_11/jre/lib/ext/localedata.jar
7f0211ec4000-7f021200b000 r--s 00fb5000 08:01 26482278                   /home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/skywalking-agent.jar
7f021200b000-7f021200d000 r--s 00002000 08:01 5902301                    /root/.m2/repository/org/synchronoss/cloud/nio-stream-storage/1.1.3/nio-stream-storage-1.1.3.jar
7f021200d000-7f021200f000 r--s 00009000 08:01 5774706                    /root/.m2/repository/org/slf4j/slf4j-api/1.7.26/slf4j-api-1.7.26.jar
7f021200f000-7f0212011000 r--s 0000c000 08:01 5902284                    /root/.m2/repository/org/synchronoss/cloud/nio-multipart-parser/1.1.0/nio-multipart-parser-1.1.0.jar
7f0212011000-7f0212021000 r--s 000a5000 08:01 5902278                    /root/.m2/repository/org/springframework/spring-webflux/5.1.6.RELEASE/spring-webflux-5.1.6.RELEASE.jar
7f0212021000-7f021202e000 r--s 00098000 08:01 5774672                    /root/.m2/repository/org/springframework/spring-beans/5.1.6.RELEASE/spring-beans-5.1.6.RELEASE.jar
7f021202e000-7f021204a000 r--s 00137000 08:01 5902268                    /root/.m2/repository/org/springframework/spring-web/5.1.6.RELEASE/spring-web-5.1.6.RELEASE.jar
7f021204a000-7f021204c000 r--s 0000f000 08:01 5902280                    /root/.m2/repository/com/fasterxml/classmate/1.4.0/classmate-1.4.0.jar
7f021204c000-7f021204e000 r--s 0000f000 08:01 5902299                    /root/.m2/repository/org/jboss/logging/jboss-logging/3.3.2.Final/jboss-logging-3.3.2.Final.jar
7f021204e000-7f0212053000 r--s 00012000 08:01 5774827                    /root/.m2/repository/javax/validation/validation-api/2.0.1.Final/validation-api-2.0.1.Final.jar
7f0212053000-7f0212070000 r--s 000fe000 08:01 5902289                    /root/.m2/repository/org/hibernate/validator/hibernate-validator/6.0.16.Final/hibernate-validator-6.0.16.Final.jar
7f0212070000-7f0212072000 r--s 00007000 08:01 5902287                    /root/.m2/repository/io/netty/netty-transport-native-unix-common/4.1.34.Final/netty-transport-native-unix-common-4.1.34.Final.jar
7f0212072000-7f0212075000 r--s 00020000 08:01 5902285                    /root/.m2/repository/io/netty/netty-transport-native-epoll/4.1.34.Final/netty-transport-native-epoll-4.1.34.Final-linux-x86_64.jar
7f0212075000-7f0212079000 r--s 0001a000 08:01 5902283                    /root/.m2/repository/io/netty/netty-codec-socks/4.1.34.Final/netty-codec-socks-4.1.34.Final.jar
7f0212079000-7f021207b000 r--s 00004000 08:01 5902281                    /root/.m2/repository/io/netty/netty-handler-proxy/4.1.34.Final/netty-handler-proxy-4.1.34.Final.jar
7f021207b000-7f0212084000 r--s 00060000 08:01 5902279                    /root/.m2/repository/io/netty/netty-handler/4.1.34.Final/netty-handler-4.1.34.Final.jar
7f0212084000-7f021208c000 r--s 0005c000 08:01 5902277                    /root/.m2/repository/io/netty/netty-codec-http2/4.1.34.Final/netty-codec-http2-4.1.34.Final.jar
7f021208c000-7f0212092000 r--s 00048000 08:01 5902275                    /root/.m2/repository/io/netty/netty-codec/4.1.34.Final/netty-codec-4.1.34.Final.jar
7f0212092000-7f0212094000 r--s 00007000 08:01 5902266                    /root/.m2/repository/io/netty/netty-resolver/4.1.34.Final/netty-resolver-4.1.34.Final.jar
7f0212094000-7f021209e000 r--s 00068000 08:01 5902258                    /root/.m2/repository/io/netty/netty-transport/4.1.34.Final/netty-transport-4.1.34.Final.jar
7f021209e000-7f02120a1000 r--s 00041000 08:01 5902255                    /root/.m2/repository/io/netty/netty-buffer/4.1.34.Final/netty-buffer-4.1.34.Final.jar
7f02120a1000-7f02120ad000 r--s 00084000 08:01 5902267                    /root/.m2/repository/io/netty/netty-common/4.1.34.Final/netty-common-4.1.34.Final.jar
7f02120ad000-7f02120b7000 r--s 00080000 08:01 5902265                    /root/.m2/repository/io/netty/netty-codec-http/4.1.34.Final/netty-codec-http-4.1.34.Final.jar
7f02120b7000-7f02120be000 r--s 00060000 08:01 5902263                    /root/.m2/repository/io/projectreactor/netty/reactor-netty/0.8.6.RELEASE/reactor-netty-0.8.6.RELEASE.jar
7f02120be000-7f02120bf000 r--s 00000000 08:01 5902261                    /root/.m2/repository/org/springframework/boot/spring-boot-starter-reactor-netty/2.1.4.RELEASE/spring-boot-starter-reactor-netty-2.1.4.RELEASE.jar
7f02120bf000-7f02120c1000 r--s 00001000 08:01 5902259                    /root/.m2/repository/com/fasterxml/jackson/module/jackson-module-parameter-names/2.9.8/jackson-module-parameter-names-2.9.8.jar
7f02120c1000-7f02120c4000 r--s 00016000 08:01 5902257                    /root/.m2/repository/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.9.8/jackson-datatype-jsr310-2.9.8.jar
7f02120c4000-7f02120c6000 r--s 00007000 08:01 5902256                    /root/.m2/repository/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.9.8/jackson-datatype-jdk8-2.9.8.jar
7f02120c6000-7f02120ca000 r--s 0004c000 08:01 5902325                    /root/.m2/repository/com/fasterxml/jackson/core/jackson-core/2.9.8/jackson-core-2.9.8.jar
7f02120ca000-7f02120cd000 r--s 0000e000 08:01 1447056                    /root/.m2/repository/com/fasterxml/jackson/core/jackson-annotations/2.9.0/jackson-annotations-2.9.0.jar
7f02120cd000-7f02120df000 r--s 00137000 08:01 5902319                    /root/.m2/repository/com/fasterxml/jackson/core/jackson-databind/2.9.8/jackson-databind-2.9.8.jar
7f02120df000-7f02120e0000 r--s 00000000 08:01 5902253                    /root/.m2/repository/org/springframework/boot/spring-boot-starter-json/2.1.4.RELEASE/spring-boot-starter-json-2.1.4.RELEASE.jar
7f02120e0000-7f02120e1000 r--s 00000000 08:01 5902250                    /root/.m2/repository/org/springframework/boot/spring-boot-starter-webflux/2.1.4.RELEASE/spring-boot-starter-webflux-2.1.4.RELEASE.jar
7f02120e1000-7f02120e2000 r--s 00000000 08:01 1443466                    /root/.m2/repository/org/reactivestreams/reactive-streams/1.0.2/reactive-streams-1.0.2.jar
7f02120e2000-7f02120f7000 r--s 00152000 08:01 5902249                    /root/.m2/repository/io/projectreactor/reactor-core/3.2.8.RELEASE/reactor-core-3.2.8.RELEASE.jar
7f02120f7000-7f02120fb000 r--s 00016000 08:01 5902247                    /root/.m2/repository/io/projectreactor/addons/reactor-extra/3.2.2.RELEASE/reactor-extra-3.2.2.RELEASE.jar
7f02120fb000-7f0212104000 r--s 00050000 08:01 5903114                    /root/.m2/repository/org/springframework/cloud/spring-cloud-gateway-core/2.1.0.RELEASE/spring-cloud-gateway-core-2.1.0.RELEASE.jar
7f0212104000-7f021215c000 r--s 003a7000 08:01 5902241                    /root/.m2/repository/org/bouncycastle/bcprov-jdk15on/1.60/bcprov-jdk15on-1.60.jar
7f021215c000-7f021216f000 r--s 000b0000 08:01 5902240                    /root/.m2/repository/org/bouncycastle/bcpkix-jdk15on/1.60/bcpkix-jdk15on-1.60.jar
7f021216f000-7f0212175000 r--s 0001f000 08:01 5903112                    /root/.m2/repository/org/springframework/cloud/spring-cloud-commons/2.1.0.RELEASE/spring-cloud-commons-2.1.0.RELEASE.jar
7f0212175000-7f0212177000 r--s 00010000 08:01 5902236                    /root/.m2/repository/org/springframework/security/spring-security-crypto/5.1.5.RELEASE/spring-security-crypto-5.1.5.RELEASE.jar
7f0212177000-7f021217b000 r--s 00018000 08:01 5903110                    /root/.m2/repository/org/springframework/cloud/spring-cloud-context/2.1.0.RELEASE/spring-cloud-context-2.1.0.RELEASE.jar
7f021217b000-7f0212182000 r--s 00043000 08:01 5774692                    /root/.m2/repository/org/yaml/snakeyaml/1.23/snakeyaml-1.23.jar
7f0212182000-7f021219a000 r--s 00124000 08:01 5774688                    /root/.m2/repository/org/springframework/spring-core/5.1.6.RELEASE/spring-core-5.1.6.RELEASE.jar
7f021219a000-7f021219c000 r--s 00005000 08:01 5774686                    /root/.m2/repository/javax/annotation/javax.annotation-api/1.3.2/javax.annotation-api-1.3.2.jar
7f021219c000-7f021219e000 r--s 00000000 08:01 5774684                    /root/.m2/repository/org/slf4j/jul-to-slf4j/1.7.26/jul-to-slf4j-1.7.26.jar
7f021219e000-7f02121a4000 r--s 0003c000 08:01 5774682                    /root/.m2/repository/org/apache/logging/log4j/log4j-api/2.11.2/log4j-api-2.11.2.jar
7f02121a4000-7f02121c8000 r--s 00111000 08:01 5774676                    /root/.m2/repository/org/springframework/boot/spring-boot-autoconfigure/2.1.4.RELEASE/spring-boot-autoconfigure-2.1.4.RELEASE.jar
7f02121c8000-7f02121cb000 ---p 00000000 00:00 0 
7f02121cb000-7f02122c9000 rw-p 00000000 00:00 0 
7f02122c9000-7f0212c98000 r--p 00000000 08:01 1841817                    /usr/lib/locale/locale-archive
7f0212c98000-7f0212c9b000 ---p 00000000 00:00 0 
7f0212c9b000-7f0212d99000 rw-p 00000000 00:00 0 
7f0212d99000-7f0212d9c000 ---p 00000000 00:00 0 
7f0212d9c000-7f0212e9a000 rw-p 00000000 00:00 0 
7f0212e9a000-7f0212e9b000 ---p 00000000 00:00 0 
7f0212e9b000-7f02138a4000 rw-p 00000000 00:00 0 
7f02138a4000-7f0213a78000 r--s 03c50000 08:01 7604038                    /opt/jdk1.8.0_11/jre/lib/rt.jar
7f0213a78000-7f02184be000 rw-p 00000000 00:00 0 
7f02184be000-7f02184bf000 ---p 00000000 00:00 0 
7f02184bf000-7f02185cb000 rw-p 00000000 00:00 0 
7f02185cb000-7f0218859000 ---p 00000000 00:00 0 
7f0218859000-7f0218865000 rw-p 00000000 00:00 0 
7f0218865000-7f0218af2000 ---p 00000000 00:00 0 
7f0218af2000-7f0218b44000 rw-p 00000000 00:00 0 
7f0218b44000-7f0218c3f000 ---p 00000000 00:00 0 
7f0218c3f000-7f0218c73000 rw-p 00000000 00:00 0 
7f0218c73000-7f0219000000 ---p 00000000 00:00 0 
7f0219000000-7f0219ca0000 rwxp 00000000 00:00 0 
7f0219ca0000-7f0228000000 ---p 00000000 00:00 0 
7f0228000000-7f0228e88000 rw-p 00000000 00:00 0 
7f0228e88000-7f022c000000 ---p 00000000 00:00 0 
7f022c000000-7f022c002000 r--s 00003000 08:01 5774680                    /root/.m2/repository/org/apache/logging/log4j/log4j-to-slf4j/2.11.2/log4j-to-slf4j-2.11.2.jar
7f022c002000-7f022c00d000 r--s 00069000 08:01 2887963                    /root/.m2/repository/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar
7f022c00d000-7f022c013000 r--s 00041000 08:01 2887965                    /root/.m2/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar
7f022c013000-7f022c01d000 r--s 00051000 08:01 5774670                    /root/.m2/repository/org/springframework/spring-aop/5.1.6.RELEASE/spring-aop-5.1.6.RELEASE.jar
7f022c01d000-7f022c037000 r--s 000f3000 08:01 5774668                    /root/.m2/repository/org/springframework/spring-context/5.1.6.RELEASE/spring-context-5.1.6.RELEASE.jar
7f022c037000-7f022c0ad000 rw-p 00000000 00:00 0 
7f022c0ad000-7f022c0ae000 ---p 00000000 00:00 0 
7f022c0ae000-7f022c1ae000 rw-p 00000000 00:00 0 
7f022c1ae000-7f022c1ca000 r-xp 00000000 08:01 7604232                    /opt/jdk1.8.0_11/jre/lib/amd64/libzip.so
7f022c1ca000-7f022c3c9000 ---p 0001c000 08:01 7604232                    /opt/jdk1.8.0_11/jre/lib/amd64/libzip.so
7f022c3c9000-7f022c3ca000 rw-p 0001b000 08:01 7604232                    /opt/jdk1.8.0_11/jre/lib/amd64/libzip.so
7f022c3ca000-7f022c3d5000 r-xp 00000000 08:01 17568476                   /lib/x86_64-linux-gnu/libnss_files-2.27.so
7f022c3d5000-7f022c5d4000 ---p 0000b000 08:01 17568476                   /lib/x86_64-linux-gnu/libnss_files-2.27.so
7f022c5d4000-7f022c5d5000 r--p 0000a000 08:01 17568476                   /lib/x86_64-linux-gnu/libnss_files-2.27.so
7f022c5d5000-7f022c5d6000 rw-p 0000b000 08:01 17568476                   /lib/x86_64-linux-gnu/libnss_files-2.27.so
7f022c5d6000-7f022c5dc000 rw-p 00000000 00:00 0 
7f022c5dc000-7f022c5f3000 r-xp 00000000 08:01 17568470                   /lib/x86_64-linux-gnu/libnsl-2.27.so
7f022c5f3000-7f022c7f2000 ---p 00017000 08:01 17568470                   /lib/x86_64-linux-gnu/libnsl-2.27.so
7f022c7f2000-7f022c7f3000 r--p 00016000 08:01 17568470                   /lib/x86_64-linux-gnu/libnsl-2.27.so
7f022c7f3000-7f022c7f4000 rw-p 00017000 08:01 17568470                   /lib/x86_64-linux-gnu/libnsl-2.27.so
7f022c7f4000-7f022c7f6000 rw-p 00000000 00:00 0 
7f022c7f6000-7f022c801000 r-xp 00000000 08:01 17568487                   /lib/x86_64-linux-gnu/libnss_nis-2.27.so
7f022c801000-7f022ca00000 ---p 0000b000 08:01 17568487                   /lib/x86_64-linux-gnu/libnss_nis-2.27.so
7f022ca00000-7f022ca01000 r--p 0000a000 08:01 17568487                   /lib/x86_64-linux-gnu/libnss_nis-2.27.so
7f022ca01000-7f022ca02000 rw-p 0000b000 08:01 17568487                   /lib/x86_64-linux-gnu/libnss_nis-2.27.so
7f022ca02000-7f022ca0a000 r-xp 00000000 08:01 17568472                   /lib/x86_64-linux-gnu/libnss_compat-2.27.so
7f022ca0a000-7f022cc0a000 ---p 00008000 08:01 17568472                   /lib/x86_64-linux-gnu/libnss_compat-2.27.so
7f022cc0a000-7f022cc0b000 r--p 00008000 08:01 17568472                   /lib/x86_64-linux-gnu/libnss_compat-2.27.so
7f022cc0b000-7f022cc0c000 rw-p 00009000 08:01 17568472                   /lib/x86_64-linux-gnu/libnss_compat-2.27.so
7f022cc0c000-7f022cc16000 r-xp 00000000 08:01 7604241                    /opt/jdk1.8.0_11/jre/lib/amd64/libinstrument.so
7f022cc16000-7f022ce15000 ---p 0000a000 08:01 7604241                    /opt/jdk1.8.0_11/jre/lib/amd64/libinstrument.so
7f022ce15000-7f022ce16000 rw-p 00009000 08:01 7604241                    /opt/jdk1.8.0_11/jre/lib/amd64/libinstrument.so
7f022ce16000-7f022ce40000 r-xp 00000000 08:01 7604227                    /opt/jdk1.8.0_11/jre/lib/amd64/libjava.so
7f022ce40000-7f022d040000 ---p 0002a000 08:01 7604227                    /opt/jdk1.8.0_11/jre/lib/amd64/libjava.so
7f022d040000-7f022d042000 rw-p 0002a000 08:01 7604227                    /opt/jdk1.8.0_11/jre/lib/amd64/libjava.so
7f022d042000-7f022d04f000 r-xp 00000000 08:01 7604231                    /opt/jdk1.8.0_11/jre/lib/amd64/libverify.so
7f022d04f000-7f022d24f000 ---p 0000d000 08:01 7604231                    /opt/jdk1.8.0_11/jre/lib/amd64/libverify.so
7f022d24f000-7f022d251000 rw-p 0000d000 08:01 7604231                    /opt/jdk1.8.0_11/jre/lib/amd64/libverify.so
7f022d251000-7f022d258000 r-xp 00000000 08:01 17568527                   /lib/x86_64-linux-gnu/librt-2.27.so
7f022d258000-7f022d457000 ---p 00007000 08:01 17568527                   /lib/x86_64-linux-gnu/librt-2.27.so
7f022d457000-7f022d458000 r--p 00006000 08:01 17568527                   /lib/x86_64-linux-gnu/librt-2.27.so
7f022d458000-7f022d459000 rw-p 00007000 08:01 17568527                   /lib/x86_64-linux-gnu/librt-2.27.so
7f022d459000-7f022d5f6000 r-xp 00000000 08:01 17568449                   /lib/x86_64-linux-gnu/libm-2.27.so
7f022d5f6000-7f022d7f5000 ---p 0019d000 08:01 17568449                   /lib/x86_64-linux-gnu/libm-2.27.so
7f022d7f5000-7f022d7f6000 r--p 0019c000 08:01 17568449                   /lib/x86_64-linux-gnu/libm-2.27.so
7f022d7f6000-7f022d7f7000 rw-p 0019d000 08:01 17568449                   /lib/x86_64-linux-gnu/libm-2.27.so
7f022d7f7000-7f022e411000 r-xp 00000000 08:01 7604213                    /opt/jdk1.8.0_11/jre/lib/amd64/server/libjvm.so
7f022e411000-7f022e611000 ---p 00c1a000 08:01 7604213                    /opt/jdk1.8.0_11/jre/lib/amd64/server/libjvm.so
7f022e611000-7f022e6da000 rw-p 00c1a000 08:01 7604213                    /opt/jdk1.8.0_11/jre/lib/amd64/server/libjvm.so
7f022e6da000-7f022e71b000 rw-p 00000000 00:00 0 
7f022e71b000-7f022e902000 r-xp 00000000 08:01 17568386                   /lib/x86_64-linux-gnu/libc-2.27.so
7f022e902000-7f022eb02000 ---p 001e7000 08:01 17568386                   /lib/x86_64-linux-gnu/libc-2.27.so
7f022eb02000-7f022eb06000 r--p 001e7000 08:01 17568386                   /lib/x86_64-linux-gnu/libc-2.27.so
7f022eb06000-7f022eb08000 rw-p 001eb000 08:01 17568386                   /lib/x86_64-linux-gnu/libc-2.27.so
7f022eb08000-7f022eb0c000 rw-p 00000000 00:00 0 
7f022eb0c000-7f022eb0f000 r-xp 00000000 08:01 17568409                   /lib/x86_64-linux-gnu/libdl-2.27.so
7f022eb0f000-7f022ed0e000 ---p 00003000 08:01 17568409                   /lib/x86_64-linux-gnu/libdl-2.27.so
7f022ed0e000-7f022ed0f000 r--p 00002000 08:01 17568409                   /lib/x86_64-linux-gnu/libdl-2.27.so
7f022ed0f000-7f022ed10000 rw-p 00003000 08:01 17568409                   /lib/x86_64-linux-gnu/libdl-2.27.so
7f022ed10000-7f022ed27000 r-xp 00000000 08:01 7603854                    /opt/jdk1.8.0_11/lib/amd64/jli/libjli.so
7f022ed27000-7f022ef26000 ---p 00017000 08:01 7603854                    /opt/jdk1.8.0_11/lib/amd64/jli/libjli.so
7f022ef26000-7f022ef27000 rw-p 00016000 08:01 7603854                    /opt/jdk1.8.0_11/lib/amd64/jli/libjli.so
7f022ef27000-7f022ef41000 r-xp 00000000 08:01 17568519                   /lib/x86_64-linux-gnu/libpthread-2.27.so
7f022ef41000-7f022f140000 ---p 0001a000 08:01 17568519                   /lib/x86_64-linux-gnu/libpthread-2.27.so
7f022f140000-7f022f141000 r--p 00019000 08:01 17568519                   /lib/x86_64-linux-gnu/libpthread-2.27.so
7f022f141000-7f022f142000 rw-p 0001a000 08:01 17568519                   /lib/x86_64-linux-gnu/libpthread-2.27.so
7f022f142000-7f022f146000 rw-p 00000000 00:00 0 
7f022f146000-7f022f16d000 r-xp 00000000 08:01 17568358                   /lib/x86_64-linux-gnu/ld-2.27.so
7f022f16d000-7f022f16e000 r--s 00004000 08:01 5902238                    /root/.m2/repository/org/springframework/security/spring-security-rsa/1.0.7.RELEASE/spring-security-rsa-1.0.7.RELEASE.jar
7f022f16e000-7f022f183000 r--s 000d4000 08:01 5774666                    /root/.m2/repository/org/springframework/boot/spring-boot/2.1.4.RELEASE/spring-boot-2.1.4.RELEASE.jar
7f022f183000-7f022f254000 rw-p 00000000 00:00 0 
7f022f254000-7f022f255000 ---p 00000000 00:00 0 
7f022f255000-7f022f258000 ---p 00000000 00:00 0 
7f022f258000-7f022f359000 rw-p 00000000 00:00 0 
7f022f359000-7f022f35a000 r--s 00005000 08:01 5774690                    /root/.m2/repository/org/springframework/spring-jcl/5.1.6.RELEASE/spring-jcl-5.1.6.RELEASE.jar
7f022f35a000-7f022f35b000 r--s 00000000 08:01 5774678                    /root/.m2/repository/org/springframework/boot/spring-boot-starter-logging/2.1.4.RELEASE/spring-boot-starter-logging-2.1.4.RELEASE.jar
7f022f35b000-7f022f360000 r--s 00040000 08:01 5774674                    /root/.m2/repository/org/springframework/spring-expression/5.1.6.RELEASE/spring-expression-5.1.6.RELEASE.jar
7f022f360000-7f022f361000 r--s 00000000 08:01 5774664                    /root/.m2/repository/org/springframework/boot/spring-boot-starter/2.1.4.RELEASE/spring-boot-starter-2.1.4.RELEASE.jar
7f022f361000-7f022f362000 r--s 00000000 08:01 5903108                    /root/.m2/repository/org/springframework/cloud/spring-cloud-starter/2.1.0.RELEASE/spring-cloud-starter-2.1.0.RELEASE.jar
7f022f362000-7f022f363000 r--s 00000000 08:01 5903106                    /root/.m2/repository/org/springframework/cloud/spring-cloud-starter-gateway/2.1.0.RELEASE/spring-cloud-starter-gateway-2.1.0.RELEASE.jar
7f022f363000-7f022f36b000 rw-s 00000000 08:01 18612519                   /tmp/hsperfdata_root/16982
7f022f36b000-7f022f36c000 rw-p 00000000 00:00 0 
7f022f36c000-7f022f36d000 r--p 00000000 00:00 0 
7f022f36d000-7f022f36e000 r--p 00027000 08:01 17568358                   /lib/x86_64-linux-gnu/ld-2.27.so
7f022f36e000-7f022f36f000 rw-p 00028000 08:01 17568358                   /lib/x86_64-linux-gnu/ld-2.27.so
7f022f36f000-7f022f370000 rw-p 00000000 00:00 0 
7ffc79acc000-7ffc79aef000 rw-p 00000000 00:00 0                          [stack]
7ffc79b8a000-7ffc79b8d000 r--p 00000000 00:00 0                          [vvar]
7ffc79b8d000-7ffc79b8f000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]

VM Arguments:
jvm_args: -javaagent:/home/shida/skywalking-procs/6.0.0-GA/apache-skywalking-apm-incubating/agent/skywalking-agent.jar -Dfile.encoding=UTF-8 
java_command: org.shida.SpringGW.SpringGwApplication
java_class_path (initial): /home/shida/sts-space/SpringGW/target/classes:/root/.m2/repository/org/springframework/cloud/spring-cloud-starter-gateway/2.1.0.RELEASE/spring-cloud-starter-gateway-2.1.0.RELEASE.jar:/root/.m2/repository/org/springframework/cloud/spring-cloud-starter/2.1.0.RELEASE/spring-cloud-starter-2.1.0.RELEASE.jar:/root/.m2/repository/org/springframework/boot/spring-boot-starter/2.1.4.RELEASE/spring-boot-starter-2.1.4.RELEASE.jar:/root/.m2/repository/org/springframework/boot/spring-boot/2.1.4.RELEASE/spring-boot-2.1.4.RELEASE.jar:/root/.m2/repository/org/springframework/spring-context/5.1.6.RELEASE/spring-context-5.1.6.RELEASE.jar:/root/.m2/repository/org/springframework/spring-aop/5.1.6.RELEASE/spring-aop-5.1.6.RELEASE.jar:/root/.m2/repository/org/springframework/spring-expression/5.1.6.RELEASE/spring-expression-5.1.6.RELEASE.jar:/root/.m2/repository/org/springframework/boot/spring-boot-autoconfigure/2.1.4.RELEASE/spring-boot-autoconfigure-2.1.4.RELEASE.jar:/root/.m2/repository/org/springframework/boot/spring-boot-starter-logging/2.1.4.RELEASE/spring-boot-starter-logging-2.1.4.RELEASE.jar:/root/.m2/repository/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar:/root/.m2/repository/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar:/root/.m2/repository/org/apache/logging/log4j/log4j-to-slf4j/2.11.2/log4j-to-slf4j-2.11.2.jar:/root/.m2/repository/org/apache/logging/log4j/log4j-api/2.11.2/log4j-api-2.11.2.jar:/root/.m2/repository/org/slf4j/jul-to-slf4j/1.7.26/jul-to-slf4j-1.7.26.jar:/root/.m2/repository/javax/annotation/javax.annotation-api/1.3.2/javax.annotation-api-1.3.2.jar:/root/.m2/repository/org/springframework/spring-core/5.1.6.RELEASE/spring-core-5.1.6.RELEASE.jar:/root/.m2/repository/org/springframework/spring-jcl/5.1.6.RELEASE/spring-jcl-5.1.6.RELEASE.jar:/root/.m2/repository/org/yaml/snakeyaml/1.23/snakeyaml-1.23.jar:/root/.m2/repository/org/springframework/cloud/spring-cloud-context/2.1.0.RELEASE/spring-cloud-context-2.1.0.
Launcher Type: SUN_STANDARD

Environment Variables:
JAVA_HOME=/opt/jdk1.8.0_202
CLASSPATH=.:/opt/jdk1.8.0_202/lib/dt.jar:/opt/jdk1.8.0_202/lib/tools.jar
PATH=.:/opt/go/bin:/home/shida/builder/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:/opt/jdk1.8.0_202/bin:/opt/apache-maven-3.6.1/bin
SHELL=/bin/bash
DISPLAY=:0

Signal Handlers:
SIGSEGV: [libjvm.so+0xa2fde0], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGBUS: [libjvm.so+0xa2fde0], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGFPE: [libjvm.so+0x89b420], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGPIPE: [libjvm.so+0x89b420], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGXFSZ: [libjvm.so+0x89b420], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGILL: [libjvm.so+0x89b420], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGUSR1: SIG_DFL, sa_mask[0]=0x00000000, sa_flags=0x00000000
SIGUSR2: [libjvm.so+0x89cbc0], sa_mask[0]=0x00000004, sa_flags=0x10000004
SIGHUP: [libjvm.so+0x89de70], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGINT: [libjvm.so+0x89de70], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGTERM: [libjvm.so+0x89de70], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004
SIGQUIT: [libjvm.so+0x89de70], sa_mask[0]=0x7ffbfeff, sa_flags=0x10000004


---------------  S Y S T E M  ---------------

OS:DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.2 LTS"

uname:Linux 4.15.0-47-generic #50-Ubuntu SMP Wed Mar 13 10:44:52 UTC 2019 x86_64
libc:glibc 2.27 NPTL 2.27 
rlimit: STACK 8192k, CORE 0k, NPROC 31740, NOFILE 1048576, AS infinity
load average:1.04 0.46 0.53

/proc/meminfo:
MemTotal:        8168760 kB
MemFree:         4277184 kB
MemAvailable:    5413432 kB
Buffers:          104152 kB
Cached:          1226672 kB
SwapCached:            0 kB
Active:          2743648 kB
Inactive:         823632 kB
Active(anon):    2237496 kB
Inactive(anon):    14680 kB
Active(file):     506152 kB
Inactive(file):   808952 kB
Unevictable:          16 kB
Mlocked:              16 kB
SwapTotal:       2097148 kB
SwapFree:        2097148 kB
Dirty:              1568 kB
Writeback:             0 kB
AnonPages:       2236432 kB
Mapped:           346516 kB
Shmem:             15712 kB
Slab:             166472 kB
SReclaimable:      85288 kB
SUnreclaim:        81184 kB
KernelStack:       10800 kB
PageTables:        46072 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     6181528 kB
Committed_AS:    6196212 kB
VmallocTotal:   34359738367 kB
VmallocUsed:           0 kB
VmallocChunk:          0 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:      186240 kB
DirectMap2M:     8202240 kB


CPU:total 2 (1 cores per cpu, 1 threads per core) family 6 model 58 stepping 9, cmov, cx8, fxsr, mmx, sse, sse2, sse3, ssse3, sse4.1, sse4.2, popcnt, avx, aes, clmul, erms, tsc, tscinvbit

/proc/cpuinfo:
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 58
model name	: Intel(R) Core(TM) i5-3380M CPU @ 2.90GHz
stepping	: 9
microcode	: 0x1f
cpu MHz		: 2901.000
cache size	: 3072 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm cpuid_fault pti ibrs ibpb stibp fsgsbase tsc_adjust smep arat arch_capabilities
bugs		: cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf
bogomips	: 5802.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 58
model name	: Intel(R) Core(TM) i5-3380M CPU @ 2.90GHz
stepping	: 9
microcode	: 0x1f
cpu MHz		: 2901.000
cache size	: 3072 KB
physical id	: 2
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm cpuid_fault pti ibrs ibpb stibp fsgsbase tsc_adjust smep arat arch_capabilities
bugs		: cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf
bogomips	: 5802.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management:



Memory: 4k page, physical 8168760k(4277184k free), swap 2097148k(2097148k free)

vm_info: Java HotSpot(TM) 64-Bit Server VM (25.11-b03) for linux-amd64 JRE (1.8.0_11-b12), built on Jun 16 2014 17:29:59 by "java_re" with gcc 4.3.0 20080428 (Red Hat 4.3.0-8)

time: Mon Apr 29 18:02:12 2019
elapsed time: 17 seconds
```

