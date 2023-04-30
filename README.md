Download Link: https://assignmentchef.com/product/solved-icsi404-assignment-10-stack-it-up
<br>
Our computer so far can implement all the instructions that can go inside a function (think about C).

What it cannot do is call functions, pass parameters, return values and return to where we called from. With this assignment, we will implement the last 4 assembly instructions that we need in order to do these things.

The Stack

CPUs typically have a stack implementation built in. Remember that a stack is like a stack of cards. The last thing in is the first thing out (LIFO). With a stack, we can push and pop values.

Consider, for a moment, how this C program might work:

int add (int a, int b) { return a+b;

} void main() {     int c = add(3,4);

}

Of course, it does not output anything, but that’s not our emphasis here. Compilers would typically output something like this in assembly:

add:       pop R15 // stack:  3 4 pop R1 // stack: 3 pop R2 // stack: empty add R1 R2 R3 push R3 // stack: 7 push R15 // stack: 7 RN return // after this, stack: 7

main:     move 3 R1 // stack: empty

move 4 R2

push R1   // stack:3 push R2 // stack: 3 4 call add // stack 3 4 RN

RN:         return // stack: 7

The parameters to the function are pushed onto the stack from registers. Inside the function they are popped off of the stack into registers. There are two other new instructions here – call and return. Call is just like jump, but it pushes the address of the NEXT instruction after the call onto the stack. When return (in add) happens, it pops the return address from the stack and moves it into PC.

For this assignment, you will need to create these four instructions (call, return, push, pop). The opcode for all four instructions will be: 0110.

Push will look like this:

0110 0000 0000 RRRR // R = register bit Pop will look like this:

0110 0100 0000 RRRR // R = register bit

Call will look like this:

0110 10AA AAAA AAAA // A = address bit. These are like jump – absolute and not an offset

Return will look like this:

0110 1100 0000 0000 // No variable bits

You will need to create a stack pointer in your CPU. This is a longword that points to an address in memory that will indicate the NEXT place to write the stack. You should call this SP (stack pointer). It should be pre-initialized to 1020. Why? We are going to start at the end of memory (remember that we have 1024 bytes, so memory ranges from 0 – 1023). We read/write 4 bytes (32 bits) because that’s the size of a register. So the first push would go into bytes 1020, 1021, 1022, 1023. We would subtract 4 from the SP, giving 1016. The next push would go into bytes 1016, 1017, 1018, 1019. Pop would add 4 to the SP.

Call is a combination of push and jump. Return is a combination of pop and jump. Push doesn’t change the register that it is copying from.

You must add these instructions to your assembler.

You must test these new instructions using your assembler (as we did before). Create a small program to test these instructions and place it in cpu_test3.