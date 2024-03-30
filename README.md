# Testing and Patching of Reference monitor

This project is devided into three parts

**Part 1**

This project involves implementing a reference monitor using the security layer functionality in Repy V2. The goal is to create a defense monitor that oversees file operations, particularly focusing on enhancing the default Repy behavior by adding an undo functionality for writes. This undo capability allows reverting the last write operation, mimicking common document editors' behavior.

The project specifications include:

Incorporating standard file operation methods along with an additional "undo" method.
Delaying the write operation until either a subsequent valid write operation occurs or the file is closed.
Allowing only the most recent write operation to be undone.
Ensuring that all file operations, except for the undo functionality, behave similarly to the underlying RepyV2 API.
Adhering to design paradigms of accuracy, efficiency, and security.

**Part 2**

Part two involves testing reference monitors designed for the Seattle testbed to ensure they are robust against potential attacks. As a tester, the goal is to assess the security layers by attempting various scenarios to circumvent them. These scenarios include writing and appending invalid data, with the aim of identifying any vulnerabilities in the security layers.

The project emphasizes three key design paradigms: **accuracy, efficiency, and security**. Test cases should focus on testing for flaws in these areas, such as ensuring that the security layer only blocks specific actions, uses minimal resources, and remains resilient against tampering or circumvention by attackers.

I created a series of test cases designed to evaluate the security layers thoroughly. Each test case focused on a specific scenario or functionality and was designed to test hypotheses related to security, performance, and accuracy.

Through this project i gained practical experience in testing security layers and understanding security paradigms in a functional way. By identifying potential vulnerabilities and weaknesses in reference monitors, I contributed to strengthen the security of systems deployed on the Seattle testbed.

**Part 3**

Part three involves fixing bugs identified in the reference monitor created in Part One of the project. A set of test cases attacking the reference monitor was a major resource that helped in identifying areas where the security layer fails. The goal is to analyze these bugs and make necessary fixes to ensure that attackers cannot circumvent the security layer.

Common bugs i encountered while fixing the security layer were:

**SeekPastEndOfFileError:**
Most of the test cases that bypassed my security layer were testing the proper functioning of End of File
counter. In order to maintain efficiency. I computed and maintained EOF inside the code. I used to modify it
only if there was a valid write at. This caused error when the write that is pending has more characters than the
actual EOF and its subsequent write used a bigger offset which is the within the length of previous write but
more than the actual EOF of the file in Disk.
To fix this. I modified EOF logic to increase EOF even for pending write and reduce it back on undo.

**LockDoubleReleaseError:**
This was caused because of the wrong implementation of lock in the security layer.
I was creating a new lock every time i had to acquire it. This made no difference as none of the threads were
waiting for the lock to release.
I fixed it by creating a single lock at the starting of file creation. And acquiring the same lock when required.

**FileClosedError:**
I faced this error because i was not checking if file is closed at the starting of writeat. This error was supposed
to raise first in case of multiple exception. I fixed it by putting file closed error validation at first.

**RepyArgumentError:**
I faced this error because i did not implement lock properly in security layer.
I fixed it by removing the lock implementation in all the unnecessary places and placing it only when the actual
write is happening.

Overall, this project gave me hands-on experience in identifying and resolving security vulnerabilities in reference monitors, reinforcing concepts related to access control and security mechanisms.
