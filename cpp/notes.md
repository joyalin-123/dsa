brief synthax of a basic program that finds the largest of the three numbers

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

synthax of program to find whether number is positive negative or zero 

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


debugging program

int age = 19;

if (age = 18) {
    cout << "Age is 18" << endl;
} else {
    cout << "Age is not 18" << endl;
}

program after debugging

int age = 19;

if (age == 18) {
    cout << "Age is 18" << endl;
} else {
    cout << "Age is not 18" << endl;
}      //here assignment operator is used instead of assigning the value

A Boolean condition in C++ is a software expression that evaluates to either true or false to decide which path a program should take. A logic gate in a digital circuit is the physical hardware equivalent, using transistors to process electrical signals (high or low voltage) based on Boolean logic. Control flow in a processor is the high-level result of these systems working together, where the CPU determines the execution sequence of instructions based on those hardware and software decision
