
#include <iostream>
#include <string>
using namespace std;

// Abstract base class
class Vehicle {
protected:
    string regNo;
    double dailyRate;
    bool rented;

public:
    Vehicle(string r, double rate)
        : regNo(r), dailyRate(rate), rented(false) {}

    virtual double charge(int days) const = 0;

    virtual void display() const = 0;

    void rent() {
        if (!rented) {
            rented = true;
            cout << regNo << " rented successfully\n";
        }
        else {
            cout << "Already rented\n";
        }
    }

    void returnVehicle(int days) {
        if (rented) {
            double amount = charge(days);

            if (days > 3) {
                amount += (days - 3) * 100;
            }

            rented = false;

            cout << regNo << " returned\n";
            cout << "Total charge: Rs " << amount << endl;
        }
        else {
            cout << "Vehicle was not rented\n";
        }
    }

    virtual ~Vehicle() = default;
};

// Bicycle
class Bicycle : public Vehicle {
public:
    Bicycle(string r, double rate)
        : Vehicle(r, rate) {}

    double charge(int days) const override {
        return dailyRate * days;
    }

    void display() const override {
        cout << "Bicycle: " << regNo << endl;
    }
};

// Two wheeler
class TwoWheeler : public Vehicle {
private:
    double fuelLevel;
    string serviceDate;

public:
    TwoWheeler(string r, double rate,
               double fuel, string date)
        : Vehicle(r, rate),
          fuelLevel(fuel), serviceDate(date) {}

    double charge(int days) const override {
        return dailyRate * days + 50;
    }

    void display() const override {
        cout << "Two Wheeler: " << regNo
             << " Fuel: " << fuelLevel
             << " Service: " << serviceDate << endl;
    }
};

// Car
class Car : public Vehicle {
private:
    int seats;
    double fuelLevel;
    string serviceDate;

public:
    Car(string r, double rate, int s,
        double fuel, string date)
        : Vehicle(r, rate), seats(s),
          fuelLevel(fuel), serviceDate(date) {}

    double charge(int days) const override {
        return dailyRate * days + 200;
    }

    void display() const override {
        cout << "Car: " << regNo
             << " Seats: " << seats
             << " Fuel: " << fuelLevel
             << " Service: " << serviceDate << endl;
    }
};

// Electric car
class ElectricCar : public Vehicle {
private:
    double batteryCapacity;
    double chargeLevel;

public:
    ElectricCar(string r, double rate,
                double capacity, double level)
        : Vehicle(r, rate),
          batteryCapacity(capacity), chargeLevel(level) {}

    double charge(int days) const override {
        return dailyRate * days + batteryCapacity * 5;
    }

    void display() const override {
        cout << "Electric Car: " << regNo
             << " Battery: " << batteryCapacity
             << " kWh" << endl;
    }
};

// New vehicle type: Electric Bus
class ElectricBus : public Vehicle {
private:
    double batteryCapacity;

public:
    ElectricBus(string r, double rate, double capacity)
        : Vehicle(r, rate), batteryCapacity(capacity) {}

    double charge(int days) const override {
        return dailyRate * days + batteryCapacity * 3;
    }

    void display() const override {
        cout << "Electric Bus: " << regNo << endl;
    }
};

int main() {
    Bicycle b("B101", 100);
    TwoWheeler t("T102", 200, 5, "2026-12-10");
    Car c("C103", 1000, 5, 10, "2026-12-15");
    ElectricCar e("E104", 1500, 50, 80);

    // Array of base-class pointers
    Vehicle* fleet[5] = {
        &b, &t, &c, &e, nullptr
    };

    // Rent all four vehicles
    for (int i = 0; i < 4; i++) {
        fleet[i]->display();
        fleet[i]->rent();
    }

    cout << "\nReturning vehicles:\n";

    for (int i = 0; i < 4; i++) {
        fleet[i]->returnVehicle(4);
    }

    // Add a new vehicle type
    ElectricBus bus("EB105", 2000, 100);

    fleet[4] = &bus;

    cout << "\nNew vehicle:\n";
    fleet[4]->display();
    fleet[4]->rent();
    fleet[4]->returnVehicle(2);

    return 0;
}
