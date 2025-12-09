# 940Basic
BASIC for the SDS 940, as modified at NOAA Computer Services Division in Boulder, Colorado.

## Introduction

This BASIC was developed as part of project Genie at the University of California,
Berkeley.
It is for a time-sharing system.
Direct access to hardware is not appropriate for a time-sharing system.
This BASIC was modeled after Dartmouth BASIC.

I modified this system to add output formatting, multiple I/O file access, and string operations.
This work started about 1970.
Before my work, NOAA added matrix operations.

This implementation comprises the command and compiler component and the runtime component.
The SDS-940 operating system permits user programs to modify their virtual memory by specifying
the mapping of 8 2048-word pages in the 16,384 word address space.
940 BASIC maps out the compiler page (pages?) and maps in the runtime page (pages?) when program
execution starts, and switches back to the compiler memory configuration when execution stops.
940 BASIC code is compiled and exected directly, rather than interpreted.
Scalar arithmetic is executed by the floating-point SYSPOPs, which are instructions that invoke code
in the operating system.
Involved operations, such as computing the sine or cosine functions, are executed as POPs,
which are instructions that invoke BASIC operations based on a 64 word transfer table.
These capabilities are described in available SDS-940 documentation.

### Matrix Operations History

This history is based on conversations with Tom Grey (@7TBG).
Two other people worked with Tom, and for some reason I didn't talk about this with either of them.
Jim Winkelman modified the compiler to recognize and compile the matrix operations.
Ralph Slutz modified the run time system to execute the POPs that implement the matrix operations.
Tom was the integrator.
One of {Jim, Ralph} worked during the day; the other worked at night.
Tom accepted the changes from each and tested the assembly and execution.
He left status and action messages in files.
I don't remember, nor remember hearing, if Jim and Ralph worked in their own accounts
or worked in the system BASIC account, @4BASIC.

\[N.B. Looking back, I wish I had talked with Jim about the compiler;
I suspect I spent a lot of time figuring it out when Jim had already
done so and an hour or two of conversation would have saved weeks of work.\]

#### Individual Account Workflow

The 940 "shell" had the "GFD" command, meaning "get file directory".
It allowed Tom, logged in as @4BASIC, to change the working directory to any other
account's directory.
Tom could GFD to Ralph's account and leave messages for him; ditto for Jim.
Ditto for messages from Ralph or Jim to Tom, and for the software changes.

#### Work in @4BASIC

The problem here is that Jim, Ralph, and Tom would all need conventions to not log in
at the same time.
Then messages could be left as files like "TOJIM" and "TORLPH".
I think there was a 6 character file name limit.

## Compiler Overview

940 BASIC allocated 286 variables, named A, A0, A1, ..., Z7, Z8, Z9.
The compiler tokenized the statements.
It pushed variables to one stack and operators to another stack.
When the precedence of the operator in hand compared appropriately
with the precedence of the operator on the top of the operator stack,
the compiler emitted the necessary code.
Matrix operations are in statements identified by the MAT keyword,
this set a flag so that + and * operations, for instance, would compile
to the matrix add or multiply POPs instead of the floating add or multiply SYSPOPs.

In reviewing this code, I noticed the garbage collector.
It was a surprise to me that BASIC would have a garbage collector.
I don't remember noticing the garbage collector before,
which seems odd since I practically memorized the garbage collector of the SDS-940 Snobol3 system.
The garbage collector code seems to be original, not added by me for string storage.
I believe the code for deleted or modified statements became the garbage that was collected as necessary.

## Compiler Details

I'm going to leave these for the future since they are lost in the mists of time.

# Code History

Somehow the BCC-500 group at the University of Hawaii heard that NOAA in Boulder had
a BASIC system with the features I've outlined here.
The BCC-500 was a multi-processor computer that executed 940 code.
One processor executed user code.
Other processors ran operating system code or handled peripherals.
When the BCC-500 group requested the source code, NOAA was happy to send
them a tape.

I was given an account on the BCC-500 in Hawaii, and I logged into that computer
from my office in Boulder using the ARPANet and ALOHANet.
(The NOAA machine room hosted an ARPANet TIP and the modems.)
I verified that it was the NOAA BASIC by triggering an obscure and low-priority bug.

Recently my Boulder colleague Mark Emmer let me know that Lars Brinkhoff had a repository with
the BASIC code from the BCC-500.
I worked with them to copy the code and get permission to create this repository.

# Code Future

TODO

### My history

I was an intern at NOAA in the summer of 1969.
I found and fixed a killer garbage collector bug in the SDS-940 Snobol3 system.
I was going into my senior year of high school.

In March of 1970, I was hired as a permanent employee.
I worked on language and operating systems and applications at NOAA.
My code ran on the CDC-3800, the SDS-940, the CDC-6600, and the MODCOMP-II and -IV computers.
I resigned in early 1979 prior to joining Bell Telephone Labs where I was a developer for the 5ESS.
