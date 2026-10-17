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

























