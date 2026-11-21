
#include <iostream>
#include <string>
using namespace std;

class Person {
protected:
    string name;
    int age;

public:
    Person(string n, int a) : name(n), age(a) {
        cout << "Person constructor\n";
    }

    ~Person() {
        cout << "Person destructor\n";
    }
};

class Address {
    string city;
    int pin;

public:
    Address(string c, int p) : city(c), pin(p) {
        cout << "Address constructor\n";
    }

    string str() const {
        return city + "-" + to_string(pin);
    }

    ~Address() {
        cout << "Address destructor\n";
    }
};

class Student : public Person {
    long long regNo;
    Address home;

public:
    Student(string n, int a, long long r,
            string c, int p)
        : Person(n, a), regNo(r), home(c, p) {
        cout << "Student constructor\n";
    }

    void print() const {
        cout << regNo << " " << name
             << " (" << age << ") "
             << home.str() << endl;
    }

    ~Student() {
        cout << "Student destructor\n";
    }
};

int main() {
    Student s("Kavya", 19, 192311001,
              "Salem", 636001);

    s.print();

    return 0;
}
