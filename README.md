# hw1, Notifications

You will create a C++ program that will combine notifications (using linked lists).
There are many notifications in a text file. Each notification includes person to which an email to which is sent to, a subject, and a message.
The notifications need to be combined to make more efficient both the sending, and the actions by the person receiving them.

## Input

The input is one or more plain-text files. In an input file, each line is terminated with an end-of-line character.
Each line will have a fixed set of data, separated by the `tab` character (that is, `\t`).
Lines that start with the symbol `#` should be considered comments and therefore skipped. There may be empty lines in the input file.

Example 1:

    Meredith   Invoice  PARK-1203
    Jackson    Approve-reservation  7634
    Amelia     Approve-reservation  7658
    Jackson    Invoice  DISC-7303
    Callie     Approve-travel    TR-N208-TX
    Arizona    Invoice  DISC-7304(Cancelled)
    # this line is a comment because it starts with #
    Owen       Approve-purchase-order  D-9076
    Miranda    Purchase-order-revise   D-9076
    Arizona    Invoice  DISC-7304(Cancelled)
    Meredith   Invoice  PARK-1205
    Cristina   Invoice  TECH-3030
    Owen       Approve-purchase-order  D-9077
    Callie     Invoice    DISC-7294
    Callie     Invoice    DISC-7294
    Amelia     Approve-reservation  7664
    Callie     Invoice    DISC-7294
    Cristina   Invoice  TECH-2980(Cancelled)
    Jackson    Invoice  DISC-7343
    Callie     Approve-travel    TR-N209-TX
    Meredith   Invoice  TECH-3034
    Owen       Approve-purchase-order  D-9078
    Amelia     Approve-reservation  7669
    Miranda    Purchase-order-cancellation   D-9076
    Amelia     Approve-reservation(Cancelled)  7669
    Callie     Review-invoice    DISC-7304

Each line that is not a comment (or empty line) will always have three items. The first is the person receiving the notification. The second item is the subject line of the email. The third is the 'body' of the email message.

For the purpose of the assignment, we will pretend that another part of a notification system will replace the name for the actual email, and there will be a nicely formatted email message created automatically from the other information in each line.

## Program specification

The main program should be called `notify` and the syntax in which it will be tested is as follows:

`./notify "input=FILENAME"`

The parameter `input` specifies the name of the input file.

Example of program calls:

`./notify "input=uh-gray.1.txt"`

The source code will be compiled as follows:

`g++ -std=c++11 -o notify -I ./ *.cpp`

Rules to combine notifications:

  1. When the same person is going to receive more than one email having the same subject, then the contents of the message need to be combined (separated by commas) into only one notification.
  2. Duplicate notifications may happen and they need to be combined into one. See example of `Callie Invoice DISC-7294`
  3. Any notification `Approve-reservation` that is followed (either immediately or after one or more lines) by a corresponding `Approve-reservation(Cancelled)` will require that both notifications be eliminated. See example of `Amelia Approve-reservation 7669`. The ID has to match.
  4. Any notification `Approve-purchase-order` that is followed (either immediately or after one or more lines) by a corresponding `Approve-purchase-order-cancellation` will require that the first notification be eliminated. See example of `Miranda Purchase-order-cancellation D-9076`. The ID has to match. Any intermediate notification `Purchase-order-revise` with a matching ID also needs to be eliminated.

Rules to combine input files:

  1. The input file will always have the substring `.1.txt`, which is used to attempt to open the next file, file 2, if exists. Then the program will open file 3, if it exists, and so on up to and including file 9. In the example listed, the input file name `uh-gray.1.txt` is file 1. File 2 would be: `uh-gray.2.txt`, which is not specified to your program. Instead, your program needs to see if file 2 exists, and if so, open it and process it.
  2. Each input file is processed independently of the others. That is, the notifications of each input file are **combined** and will stay independently of other input files (if any).
  3. Once each input file has had its notifications combined (independently of the other input files), a global list of notifications will be created by taking one notification at a time of each of the input files.

    Example:

    File 1 has notifications A B C D

    File 2 has notifications G H K

    File 3 has notifications M N

    File 4 has notifications Q R T S

    The global list would be: A G M Q B H N R C K T D S

  4. The final step is to combine the global list of notifications. For example, if file 1 has a notification `Alex Invoice PEDS-765`, and file 4 has a notification `Alex Invoice PSYCH-3654`, then such notifications need to be combined into only one notification: `Alex Invoice PEDS-765, PSYCH-3654`

## Output

Your program will output to the console (such as via `cout`) with the results of combining notifications.
Your program must follow the output format exactly to facilitate automated grading (and to avoid failing test cases due to things such as output of an empty line at the end).

Output for the input example 1.

    Meredith   Invoice  PARK-1203, PARK-1205, TECH-3034
    Jackson    Approve-reservation  7634
    Amelia     Approve-reservation  7658, 7664
    Jackson    Invoice  DISC-7303, DISC-7343
    Callie     Approve-travel    TR-N208-TX, TR-N209-TX
    Arizona    Invoice  DISC-7304(Cancelled),
    Cristina   Invoice  TECH-3030, TECH-2980(Cancelled)
    Owen       Approve-purchase-order  D-9077, D-9078
    Callie     Invoice    DISC-7294
    Miranda    Purchase-order-cancellation   D-9076
    Callie     Review-invoice    DISC-7304

## Assumptions

* Each input file can fit in main memory (not larger than 10kb).

## Requirements

* Must utilize linked-lists to do the combining of operations (either within a file, or when combining various files into a single list of notifications). Failure to do so will result in a grade of 2 points (out of 100).
* Place your codes in a folder in your home directory, named: `hw1` (Failure to do so will cause your program to have a zero grade due to inability for doing automated grading).
* Your program should not produce any unexpected output when it is doing intermediate calculations because doing so will interfere with automated grading and some test cases will fail.
* You can not assume a maximum number of lines in the input file.

## Miscellaneous

* A good explanation on [how to use c++ parameters from the command-line](http://www.site.uottawa.ca/~lucia/courses/2131-05/labs/Lab3/CommandLineArguments.html)
