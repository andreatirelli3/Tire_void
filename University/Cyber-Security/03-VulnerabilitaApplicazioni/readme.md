# Introduction
> *All software has bugs, latter bugs are what we refer to as **software security vulnerabilities**.*

These **vulnerabilities** are written into the software as **coding error**.

Types of bug:
- Functional;
- Elevated privileges;
- Information leakage;
- Denial of Service.

**Common problem**: when vendors learn of vulnerabilities, they can release patches. Unfortunately, users and administrators frequently **do not implement** the patches, leaving the systems vulnerable.

> ***Poor code quality**, leads to unpredictable behavior.*

- For user: poor usability.
- For attacker: opportunity to stress the system.

Why devs write not secure code? -> Poor training and course on how to write secure/safe code.

# Application vulnerabilities
Some classes of errors:
- [[#Incomplete mediation]];
- [[#Time of Check to Time of Use]];
- Code injection (buffer overflow, Shell code, SQL injection, XSS)

**Major vulnerability**: Validation of missing/incomplete input.
## Incomplete mediation
- Sensitive data, which are effected by some critical operations.
	1. Security; *e.g. authentication*
	2. Application logic.
- Exposed data, not controlled.
	1. Data which are modified by user;
	2. The mediation from the program to data and user is not complete.

> Example: Alter an exam mark written on the paper.

### Real scenario
- Application keep the Intermediate data values in temporal files. ("/tmp")
- Buffer overflow, allow user to modify "status information" of the program.
### Backup measure
- Fix the possible errors;
- Prevent the possibility of errors.
## Time of Check to Time of Use
> Known as: **TOCTTOU**, **TOC/TOU**, **critical races**, **race conditions**.

Usually these type of vulnerability are cause by synchronization problems.
- Multi users and multitasking systems.
![[Schermata del 2024-06-14 15-42-34.png]]
### Backup measure
- Atomic transaction;
- Restructure the code to avoid race conditions.

![[Schermata del 2024-06-14 15-45-04.png]]
## Code injection
> *Inject arbitrary code in an application by exploiting the input control.*

Major technique of code injection:
1. Shell injection via buffer overflow;
2. SQL injection;
3. XSS.

**Major cause**: the input is not validated and sanitized.
- Implicit assumption (and wrong) of the quality of the input given by users;
- Confusion between data and code.
**Backup measure**: sanitized and validate every input.
- Verify size, content, type, etc.
### Buffer overflow (BOF)
> *Writing past the end of an array declared auto in a routine.*

Code that does this is said to **smash the stack.**
- Can cause return from the routine to jump to a random address.

Types of BOF:
- **Stack segment**
- Heap segment
- Format string
- ...
#### Stack segment
**Buffer**, contiguous space in memory which data of the same type.
> **Buffer overflow**, overflow an array with a pool of data that exceed the size of the one.

By default, in the C language there aren't any types of control made by the compiler to check the size limit of array and the reference pointers.
This is because most of the errors are highlighted in runtime; so the developer must take the responsibility of this types of control.