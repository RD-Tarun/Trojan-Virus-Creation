# Crafting a Trojan Virus in 5 Minutes

## Introduction
In this guide, I’ll walk you through creating a Trojan in just five minutes. We’ll use a familiar application, Putty (an SSH client for Windows), to trigger a calculator as a demonstration. You can replace the calculator with any payload you want — this is just for testing and learning purposes.

## What is a Trojan?
A Trojan, or Trojan horse, is malicious software disguised as legitimate code. It can seize control of your system, designed to harm, disrupt, steal data, or cause other damaging effects on your device or network.  
*Source: [us.norton.com](https://us.norton.com)*

> This tutorial draws from the Sektor7 RTO Essentials Course:  
> *RED TEAM Operator: Malware Development Essentials*  
> Learn ethical hacking, penetration testing, and red teaming through malware development.  
> *Source: [institute.sektor7.net](https://institute.sektor7.net)*

![image](https://github.com/user-attachments/assets/7bb04256-c447-434c-b2c3-ae9f495914a0)

## Backdooring an EXE
Here’s how to embed a backdoor in an executable file:

### 1. Locate a Code Cave
### 2. Insert a Memory Jump
### 3. Inject Your Shellcode

First, determine whether the target process is 32-bit or 64-bit. You can use Process Hacker to check this:

- **Download Process Hacker:**  
  A free, versatile tool for monitoring system resources, debugging, and detecting issues.  
  *Source: [processhacker.sourceforge.io](https://processhacker.sourceforge.io)*

Open `Putty.exe` in Process Hacker to inspect it:

- Select `Putty.exe`
- Confirm it’s a 32-bit process

Since it’s 32-bit, download `x32dbg` debugger (use `x64dbg` for 64-bit processes):

- **GitHub - x32dbg/x32dbg**  
  An open-source debugger for Windows, ideal for malware analysis and reverse engineering.  
  *Source: [github.com](https://github.com/x64dbg/x64dbg)*

Launch x32dbg:

- Go to **File -> Open -> Select Putty.exe**

---

### Step 1: Find a Code Cave
Scroll through the memory in x32dbg to locate a block of empty memory (addresses filled with `00 00`). This is your **code cave**.

- Right-click the first empty memory address
- Choose **Copy -> Address**, and save it in Notepad
- Set a breakpoint: **Right-click -> Breakpoint -> Toggle**
- Navigate to the **Breakpoints** tab and select the **Entry breakpoint**

---

### Step 2: Create a Memory Jump
Modify the assembler instruction at the entry point:

- Right-click -> Assemble
- Enter:

```assembly
jmp 0xFirst0000Address
```
*(Replace with your saved address)*

This jump redirects the program’s execution to your shellcode in the empty memory when it starts.

Next, modify the assembler instructions for the first two empty memory addresses.

---

### Step 3: Inject Your Shellcode
You’ll need hexadecimal executable code for your payload.  
For this example, we use the calculator’s hex code:

- **calc32.hex**  
  *Source: [drive.google.com](https://drive.google.com)*

- Select a large range of empty memory addresses in x32dbg
- Right-click -> **Binary -> Edit** -> Paste the hex code (Ctrl+V)

Your shellcode is now embedded.

- Right-click -> **Patches -> Select All -> Patch File**

Run the modified executable, and the calculator should launch.

---

### Restoring Program Flow
To ensure Putty still functions, add a jump back to the original program flow after your shellcode executes.

1. Copy the memory address immediately following the entry point (increment the entry point address by one).
2. Locate the last memory address used by your shellcode.
3. Add an assembler instruction to jump back to the address from step 1.

Run the executable again. The calculator should launch, followed by Putty’s normal operation.

---

## Conclusion
This tutorial demonstrates how hackers craft Trojans.  
I hope you found it insightful and educational.  
Thanks for reading!

- Malforge Group
---
