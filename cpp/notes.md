brief synthax of a basic program that finds the largest of the three numbers
********************************************************************************
#include <iostream>
using namespace std;

int main() {
    int a,b,c;
    cout<<"Enter the numbers:"<<endl;
    cin>>a>>b>>c;
    cout<<a<<","<<b<<","<<c<<endl;
    int large=a;
    if(large<b){
        large=b;
    }
    if (large<c){
        large=c;
    }
    cout<<large<<" is the largest numnber";    
    return 0;
}
***************************************************************************************************************************************************
synthax of program to find whether number is positive negative or zero 
************************************************************************
#include <iostream>
using namespace std;

int main() {
    int a;
    cout<<"Enter the numbers:"<<endl;
    cin>>a;
    cout<<a<<endl;
    if(a>0){
        cout<<"Positive";
    }
    if (a<0){
        cout<<"Negative";
    }
    if (a==0){
        cout<<"Zero";
    }
    return 0;
}

**********************************************************************************************************************************************************************
debugging program
**********************
int age = 19;

if (age = 18) {
    cout << "Age is 18" << endl;
} else {
    cout << "Age is not 18" << endl;
}

program after debugging
*****************************
int age = 19;

if (age == 18) {
    cout << "Age is 18" << endl;
} else {
    cout << "Age is not 18" << endl;
}      //here assignment operator is used instead of assigning the value
***********************************************************************************************************************************************************************
A Boolean condition in C++ is a software expression that evaluates to either true or false to decide which path a program should take. A logic gate in a digital circuit is the physical hardware equivalent, using transistors to process electrical signals (high or low voltage) based on Boolean logic. Control flow in a processor is the high-level result of these systems working together, where the CPU determines the execution sequence of instructions based on those hardware and software decision
*********************************************************************************************************************************************************************************
A GPU can often process many elements concurrently, assigning different elements to different threads.
The important distinction is that a loop describes repetition, while parallelism describes work happening concurrently. A loop running on a CPU doesn't automatically become parallel, and a GPU doesn't automatically make every loop faster.
We'll later learn how GPU threads, blocks, memory access, and synchronization affect performance.

******************************************************************************************************************************************************************************
#include <iostream>
using namespace std;

int main()
{
    int n,sum=0;
    cout<<"Enter limit:"<<endl;
    cin>>n;
    for (int i=1;i<=n;i++){
        sum=sum+i;
    }
    cout<<sum;
    return 0;
}

***************************************************************************************
for (int i = 2; i <= 8; i += 2) {
    cout << i << " ";
}
output: 2 4 6 8
************************************************************************************
program to count from 5 to 1
******************************
#include <iostream>
using namespace std;

int main()
{
    int n;
    n=5;
    while (n>=1){
        cout<<n<<endl;
        n-=1;
    }
    return 0;
}

*******************************************************************************************
spot the bug
************
int i = 1;

while (i <= 3) {
    cout << i << endl;
    i--;
}
makes the program a infinite loop 
so maybe we cld change the i-- to i++
****************************************************************************************************

A GPU can often divide that work among many threads so multiple elements are processed concurrently.
Here's the crucial distinction:
- Repetition: Perform an operation for many elements.
- Parallelism: Perform multiple independent operations at the same time.
A loop does not automatically make a program parallel. In later lessons, we'll learn how GPU threads, blocks, memory access, and synchronization allow us to distribute work efficiently.
This is the bridge from beginner C++ to GPU programming
************************************************************************************************************************************************************
Suppose you have 1,000 independent numbers, and you need to multiply every number by two.
Explain in two or three sentences why a GPU might process this workload efficiently.

A GPU is highly efficient for this workload because it is designed for massively parallel processing, allowing it to execute the same instruction across thousands of data points simultaneously. Since multiplying each of the 1,000 numbers by two requires the exact same mathematical operation and the numbers are completely independent, a GPU can process all 1,000 multiplications at the same time in parallel, rather than sequentially like a traditional CPU core
************************************************************************************************************************************************

n % 10 extracts the last digit.
n / 10 removes the last digit using integer division.
Repeat until n becomes 0.

*****************************************************************************************************

Functions:
#include <iostream>
using namespace std;

int sumDigit(int n){
    int sum=0;
    while (n!=0){
        sum+=n%10;
        n/=10;
    }
    return sum;
}

int main()
{
    int n;
    cout<<"Enter number: ";
    cin>>n;
    cout<<sumDigit(n)<<endl;
    return 0;
}
.................................................................................................

Lesson: How does a GPU decide which number each thread processes?
Last time, we learned the following:
- A thread is an execution unit that runs instructions.
- A block groups threads together.
- A grid contains blocks.
- A kernel describes the work that GPU threads should perform.
- CUDA is NVIDIA's programming platform for GPU computing.
Today, we'll connect these concepts to the actual indexing code.
1. Start with a problem
Suppose you have ten numbers:
Input:  [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

You want to square every number.
The output should be:
Output: [4, 9, 16, 25, 36, 49, 64, 81, 100, 121]

We could use a C++ loop:
for (int i = 0; i < 10; i++) {
    output[i] = input[i] * input[i];
}

This loop visits the array indexes one after another.
A GPU kernel can distribute the calculations across many threads instead.
But we need to solve one problem:
How does each thread know which number it is responsible for?
That's what the global index solves.
2. Calculate how many threads we launch
Suppose we choose:
- 3 blocks
- 4 threads per block
The total number of launched threads is:
\[
\text{Total threads}
=
\text{Number of blocks}
\times
\text{Threads per block}
\]
Therefore:
\[
3\times4=\boxed{12\text{ threads}}
\]
We launched 12 threads, even though our array contains only 10 numbers.
That's allowed. We'll see how to handle the extra threads shortly.
Visualize the arrangement
Block	Local thread indexes	Global indexes
Block 0	    0, 1, 2, 3           	0, 1, 2, 3
Block 1	    0, 1, 2, 3          	4, 5, 6, 7
Block 2	    0, 1, 2, 3	            8, 9, 10, 11


Notice that the local indexes restart at zero for every block.
Block 1 has a Thread 0, just like Block 0.
So local thread indexes alone cannot uniquely identify positions across the entire grid.
We need a global index.
3. The global index formula
Here is the formula again:
\[
\boxed{
i=(\text{blockIdx.x}\times\text{blockDim.x})
+\text{threadIdx.x}
}
\]
Let's translate it into ordinary English.
CUDA expression	Meaning
blockIdx.x	Which block am I in?
blockDim.x	How many threads are in each block?
threadIdx.x	Which thread am I within my block?
i	Which array position should I process?


Worked example A
Suppose:
blockIdx.x = 2;
blockDim.x = 4;
threadIdx.x = 1;

First, calculate how many positions come before Block 2:
\[
2\times4=8
\]
Block 2 therefore starts at global index 8.
Our thread is Thread 1, so:
\[
i=8+1=\boxed{9}
\]
That thread processes input[9], the tenth element of the array.
The number 9 comes from the block's starting position plus the thread's local index.
Worked example B
Suppose:
blockIdx.x = 1;
blockDim.x = 4;
threadIdx.x = 2;

Block 1 starts at:
\[
1\times4=4
\]
Add the local thread index:
\[
i=4+2=\boxed{6}
\]
The thread processes input[6].
You're not memorizing random numbers. You're calculating where each thread belongs.
4. Why do we need if (i < n)?
Return to our original problem:
- The array contains 10 elements.
- Valid indexes are 0 through 9.
- We launched 12 threads.
- The global indexes are 0 through 11.
What happens to the threads with indexes 10 and 11?
Those positions don't exist in our array.
This is why CUDA kernels commonly contain a boundary check:
if (i < n) {
    output[i] = input[i] * input[i];
}

Here, n is the number of elements, which is 10.
Let's test the condition.
Global index i	Is i < 10 true?	What happens?
7	Yes	Calculate and store the square
9	Yes	Calculate and store the square
10	No	Skip the calculation
11	No	Skip the calculation


The boundary check prevents invalid array access.
Important distinction: A thread being launched doesn't guarantee that it performs the array calculation. The condition determines whether it does.
5. Put the code together
Here's the complete kernel we've been discussing:
__global__ void squareArray(int* input, int* output, int n) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;    
    if (i < n) {
        output[i] = input[i] * input[i];
    }
}

Read it in this order:
1. __global__ marks the function as a CUDA kernel.
2. input refers to the input array.
3. output refers to the output array.
4. n tells us how many elements exist.
5. The indexing formula determines each thread's global index.
6. The if condition checks whether that index is valid.
7. The multiplication calculates the square and stores it in the output array.
The kernel doesn't contain a for loop because each thread calculates its own index.
The GPU manages the collection of threads executing this work.
A complete CUDA program would also need to allocate appropriate memory, arrange the kernel launch, and handle synchronization and data transfers where necessary.
***********************************************************************************************************************************************************************************


#include <iostream>
using namespace std;
int main(){
    int numbers[5]={1,2,3,4,5};
    int largest = numbers[0];
    for (int i = 1; i < 5; i++) {
        if (numbers[i] > largest) {
            largest = numbers[i];}
    }
    cout<< largest;
}

*****************************************************************************

string word = "parallel";

cout << word[0] << endl;  // p
cout << word[1] << endl;  // a
cout << word.length() << endl;  // 8


- word[0] accesses the first character.
- word.length() gives the number of characters.
- word[i] lets you access a character using a loop index.
If you use cin >> word, it reads only until whitespace. To read a full line such as "GPU engineering", use getline(cin, word).

******************************************************************************************************************


#include <iostream>
#include <string>
using namespace std;


int main()
{
    string word;
    int vowelcount=0;
    cout<<"Enter the word"<<endl;
    cin>>word;
    for (int i=0;i<word.length();i++){
        char ch=word[i];
        if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' || ch == 'A' || ch == 'E'|| ch == 'I' || ch == 'O' || ch == 'U'){
            vowelcount+=1;
        }
    }
    cout<< vowelcount;
    return 0;
}
***********************************************************************************************************************

What happens in a complete CUDA program?
A common pattern is:

CPU memory
Original input data

GPU memory
Input and output arrays

GPU kernel
Threads calculate results

Results
The CPU program can use the answers when ready
*****************************************************************************************************************************
A simplified memory hierarchy:

Registers
Very small, very fast storage used for values a thread is working with.

Cache / shared memory
Fast storage that can reduce repeated trips to larger memory. Shared memory can be explicitly managed by threads in a block.

Global GPU memory
Large memory space used to hold arrays and other data accessible to GPU kernels.
**************************************************************************************************************************************8

Registers: tiny, very fast storage used by an individual GPU thread for values it is currently working with.
Global memory: much larger GPU memory where arrays and other data used by kernels can live. It has higher access latency than registers.

global memory is typically GPU memory such as VRAM, not the CPU's ordinary RAM.

********************************************************************************************************************************
Step 1: CPU memory
One million numbers are stored in the computer's RAM.
Copy input data to GPU memory

Step 2: GPU processing
Many threads process different numbers in parallel.
Copy results back to CPU memory, if needed

Step 3: Results available to the CPU
The program can display or use the answers.

he total time is roughly:


Let's use an illustrative example, not actual benchmark measurements.
Activity	        Time
Copy data to GPU	4 ms
Calculate on GPU	1 ms
Copy results back	4 ms
Total	9 ms
Even though the calculation takes only 1 ms, the entire job takes 9 ms!

**************************************************************************************************************************

#include <iostream>
using namespace std;

int main()
{
    int n,sum=0,pos=0;
    cout<<"Enter the length of the array";
    cin>>n;
    int numbers[n];
    for (int i=0;i<n;i++){
        cout<<"Enter the number: ";
        cin>>numbers[i];
    }
    int largest=numbers[0];
    for (int i=0;i<n;i++){
        if (numbers[i]>largest){
            largest=numbers[i];
        }
        if (numbers[i]%2==0){
            sum+=numbers[i];
        }
        if (numbers[i]>0){
            pos+=1;
        }
    }
    cout<<"largest= "<<largest<<endl;
    cout<<"sum of all even numbers= "<<sum<<endl;
    cout<<"positive count= "<<pos<<endl;
    return 0;
}

***********************************************************************

#include <iostream>
#include <string>
using namespace std;

int main()
{
    string word;
    int check=1;
    cout<<"Enter the word";
    cin>>word;
    int left=0;
    int right=word.length()-1;
    for (left;left<right;left++){
        if (word[left]!=word[right]){
            check=0;            
            break;
        }
        right--;
    }  
    if (check==0){
        cout<<"it is not a palindrome"<<endl;        
    }
    else{
        cout<<"it is a palindrome"<<endl;
    }
    return 0;

}
******************************************************************************************************************************************


































