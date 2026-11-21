
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

class Cart {
private:
    string items[10];
    double prices[10];
    int n = 0;
    double discount = 0;

public:
    Cart& add(string item, double price) {
        if (n < 10 && price >= 0) {
            items[n] = item;
            prices[n] = price;
            n++;
        }

        return *this;
    }

    Cart& setDiscount(double discount) {
        this->discount = discount;
        return *this;
    }

    double total() const {
        double t = 0;

        for (int i = 0; i < n; i++) {
            t += prices[i];
        }

        return t * (100 - discount) / 100;
    }
};

int main() {
    // 1. Direct object
    Cart c;
    c.add("Rice", 60).add("Dal", 120);
    cout << "Direct: " << c.total() << endl;

    // 2. Pointer to object
    Cart* p = &c;
    p->add("Oil", 180);
    cout << "Pointer: " << p->total() << endl;

    // 3. Reference to object
    Cart& r = c;
    r.setDiscount(10);
    cout << "Reference: " << r.total() << endl;

    // 4. Dynamic object
    Cart* d = new Cart();
    d->add("Milk", 50);
    cout << "Dynamic: " << d->total() << endl;
    delete d;

    // Complete Code from PDF
    Cart cart;
    cart.add("Rice", 60)
        .add("Dal", 120)
        .setDiscount(10);

    Cart* ptr = &cart;
    ptr->add("Oil", 180);

    cout << fixed << setprecision(2);
    cout << "PDF output: " << ptr->total() << endl;

    // Highest total using array of pointers
    Cart c1, c2, c3;

    c1.add("Book", 100);
    c2.add("Laptop", 500);
    c3.add("Pen", 20);

    Cart* carts[] = {&c1, &c2, &c3};

    Cart* highest = carts[0];

    for (int i = 1; i < 3; i++) {
        if (carts[i]->total() > highest->total()) {
            highest = carts[i];
        }
    }

    cout << "Highest cart: "
         << highest->total() << endl;

    return 0;
}
