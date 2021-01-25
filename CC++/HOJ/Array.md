## 1011 - UNIX ls

http://acm.hit.edu.cn/problemset/1011

### Problem Description

The computer company you work for is introducing a brand new computer line and is developing a new Unix-like operating system to be introduced along with the new computer. Your assignment is to write the formatter for the ls function.

Your program will eventually read input from a pipe (although for now your program will read from the input). Input to your program will consist of a list of (F) filenames that you will sort (ascending based on the ASCII character values) and format into (C) columns based on the length (L) of the longest filename. Filenames will be between 1 and 60 (inclusive) characters in length and will be formatted into left-justified columns. The rightmost column will be the width of the longest filename and all other columns will be the width of the longest filename plus 2. There will be as many columns as will fit in 60 characters. Your program should use as few rows (R) as possible with rows being filled to capacity from left to right.

### Input

The input will contain an indefinite number of lists of filenames. Each list will begin with a line containing a single integer (1 <= N <= 100). There will then be N lines each containing one left-justified filename and the entire line's contents (between 1 and 60 characters) are considered to be part of the filename. Allowable characters are alphanumeric (a to z, A to Z, and 0 to 9) and from the following set { ._- } (not including the curly braces). There will be no illegal characters in any of the filenames and no line will be completely empty.

Immediately following the last filename will be the N for the next set or the end of file. You should read and format all sets in the input.

### Output

For each set of filenames you should print a line of exactly 60 dashes (-) followed by the formatted columns of filenames. The sorted filenames 1 to R will be listed down column 1; filenames R+1 to 2R listed down column 2; etc.

### Sample Input

```
10
tiny
2short4me
very_long_file_name
shorter
size-1
size2
size3
much_longer_name
12345678.123
mid_size_name
12
Weaser
Alfalfa
Stimey
Buckwheat
Porky
Joe
Darla
Cotton
Butch
Froggy
Mrs_Crabapple
P.D.
19
Mr._French
Jody
Buffy
Sissy
Keith
Danny
Lori
Chris
Shirley
Marsha
Jan
Cindy
Carol
Mike
Greg
Peter
Bobby
Alice
Ruben
```

### Sample Output

```
------------------------------------------------------------
12345678.123         size-1
2short4me            size2
mid_size_name        size3
much_longer_name     tiny
shorter              very_long_file_name
------------------------------------------------------------
Alfalfa        Cotton         Joe            Porky
Buckwheat      Darla          Mrs_Crabapple  Stimey
Butch          Froggy         P.D.           Weaser
------------------------------------------------------------
Alice       Chris       Jan         Marsha      Ruben
Bobby       Cindy       Jody        Mike        Shirley
Buffy       Danny       Keith       Mr._French  Sissy
Carol       Greg        Lori        Peter
```

### Solution

```c++
#include<cstdio>
#include<algorithm>
#include<iostream>
#include<cstring>
using namespace std;

string a[200];

int main(void)
{
  int n, m;
  int x, y;
  int maxn;
  int p = 0;
  while(cin>>n){
    printf("------------------------------------------------------------\n");
    for(int i = 0; i < n; ++i) cin>>a[i];
    sort(a, a+n);
    maxn = 0;
    for(int i = 0; i < n; ++i)
      if(maxn < a[i].length())
    maxn = a[i].length();
    x = 62/(maxn+2);
    y = n / x;
    if(n % x) y++;
    for(int i = 0; i < y; ++i){
      for(int j = 0; j < x; ++j){
    m = i+j*y;
    if(m >= n) break;
    else{
      cout<<a[m];
      if(m + y < n)
        for(int k = 0; k < maxn+2-a[m].length(); ++k) cout<<' ';
    }
      }
      cout<<endl;
    }
  }
  return 0;
}
```

## 1012 - Decoding Task

http://acm.hit.edu.cn/problemset/1012

### Problem Description

In the near future any research and publications about cryptography are outlawed throughout the world on the grounds of national security concerns. The reasoning for this is clear and widely accepted by all governments - if cryptography literature is public like in the old times, then everybody (even criminals and terrorists) could easily use it to hide their malicious plans from the national and international security forces. Consequently, public cryptographic algorithms and systems have ceased to exist, and everybody who needs strong protection for their secrets is forced to invent proprietary algorithms.

The ACM Corporation has lots of competitors who are eager to learn its trade secrets. Moreover, the job to protect their secrets is complicated by the fact, that they are forced to use intercontinental communication lines which are easy to eavesdrop on, unlike internal lines of the ACM Corporation which are well guarded. Therefore, the ACM Corporation have invented the Intercontinental Cryptographic Protection Code (ICPC) which they are very proud of, and which is considered unbreakable - nobody has even tried to break it yet, but that is about to change.

The group of hackers was hired by the rival company, which does not disclose its name to them, to break ICPC. As the first step, they have bribed one of the programmers who implemented the software for ICPC and have learned how ICPC works. It turns out, the ICPC uses very long key which is a sequence of bytes generated by some sophisticated and random physical process. This key is changed weekly and is used to encrypt all messages that are sent over intercontinental communication lines during the week. This programmer has also proudly told them, that ICPC is the fastest code in the world, because (having the benefit of highly sophisticated code generation) they simply perform bitwise exclusive OR (XOR) operation between the bytes of the message and the key. That is, the ith byte of the encrypted message Ei = Ki XOR Ci, where Ki is the ith byte of the key and Ci is the ith byte of the original clear-text message.

![img](http://acm.hit.edu.cn/hoj/static/img/pic/1012_a.gif)

Having learned how ICPC works, they have started to look for the way to reliably obtain the key every week, which is the only thing that is still missing to listen for all intercontinental communications of the ACM Corporation (eavesdropping on the intercontinental lines themselves has indeed turned out to be an easy task). An attempt to bribe the security officers who guard and distribute the key has failed, because the security officers (having the profession with one the highest salaries of that time) have turned out to be too expensive to bribe.

During the search for alternative solutions, they have stumbled upon a clerk, who sends weekly newsletters to various employees and departments. Fortunately, these newsletters are being sent just after the change of the key and the messages are usually long enough to recover sufficient portions of the key by studying original newsletters and their encoded forms. However, they could not covertly find anyone who will disclose the newsletter contents on a weekly basis, because all the employees were bound by a Non-Disclosure Agreement (NDA) and the penalty for the disclosure of any corporate message according to this NDA is death.

Yet they were able to convince this clerk (for a small reward) to do a seemingly innocent thing. That is, while sending the copies of newsletter throughout the corporation, he was instructed to insert an extra space character in the beginning of some messages but send other copies in their original form. Now the task to recover the key is straightforward and it is you, who shall create a program for this. The program is given two ICPCed messages where the first message is N bytes, and the second one is N+1 bytes and is the result of encoding the same clear-text messages as the first one, but with one extra space character (represented by the byte with the decimal value of 32) in the beginning. The program shall find the first N+1 bytes of the key that was used to encode the messages.

### Input

The input file consists of two lines. The first line consists of 2N characters and represents the encoded message N bytes long. The second line consists of 2N+2 characters and represents the encoded message N+1 bytes long. Here 1 <= N <= 10000. Each message is written on a single line in a hexadecimal form byte by byte without spaces. Each byte of the message is represented by two characters '0'-'9', 'A'-'F' that represent the hexadecimal value of the corresponding byte.

Process to the end of file.

### Output

Write to the output file a single line that represents N+1 bytes of the recovered key in the same hexadecimal format as in the input file

### Sample Input

```
05262C5269143F314C2A69651A264B
610728413B63072C52222169720B425E
```

### Sample Output

```
41434D2049435043204E454552432732
```

### solution