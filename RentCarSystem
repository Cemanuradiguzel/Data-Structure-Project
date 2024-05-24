#include <iostream>
#include <fstream>
#include <sstream>
#include <forward_list>
#include <vector>
using namespace std;

struct RentalHistory {
	string carBrand;
	string carModel;
};
struct Customer {
	string name;
	string surname;
	char gender;
	vector<RentalHistory> rentalHistory;
};
struct Car {
	string type;
	string brand;
	string model;
	string year;
	bool isAvailable = true;
	Customer rentCustomer;
};

class EliteDrive {
private:
	forward_list<Car> cars;
	forward_list<Customer> customers;
	
	int numberOfCars = 0, numberOfCustomer = 0, numberOfRentals = 0;
public:
	void carRentedMenu() {
		cout << "\nCar Rented Menu" << endl;
		cout << "1. Display Available Cars" << endl;
		cout << "2. Rent a Car" << endl;
		cout << "3. Return a Car" << endl;
		cout << "4. Display Customer Information" << endl;
		cout << "5. Register New Customer" << endl;
		cout << "6. View Customer Accounts" << endl;
		cout << "7. Exit" << endl;
	}

	void addCars() {
		ifstream inpfile("cars.txt");

		if (!inpfile.is_open()) {
			std::cerr << "Cannot open the file!" << std::endl;
			return;
		}

		string line;
		while (getline(inpfile, line)) {
			Car car;
			stringstream ss(line);
			//separate each type of element with commas
			getline(ss, car.type, ',');
			getline(ss, car.brand, ',');
			getline(ss, car.model, ',');
			getline(ss, car.year, ',');
			getline(ss, line);

			cars.push_front(car);
			numberOfCars++;
		}
		inpfile.close();
	}

	void displayCars() {
		if (cars.empty()) {
			cout << "No cars found!" << endl;
			return;
		}

		for (const Car& car : cars) {
			cout << "Type: " << car.type << endl;
			cout << "Brand: " << car.brand << endl;
			cout << "Model: " << car.model << endl;
			cout << "Year: " << car.year << endl << endl;
		}
	}

	void rentCar(string carType, string carBrand, string carModel, string carYear, const Customer& customer) {
		
		for (auto& car : cars) {
			if (carType == car.type && carBrand == car.brand && carModel == car.model && carYear == car.year && car.isAvailable) {
				cout << "The selected car is available." << endl;

				car.rentCustomer = customer;

				car.isAvailable = false;


				cout << "Car rented successfully to " << customer.name << " " << customer.surname << endl;
				return;
			}
		}
	}

	void returnCar(string brand, string model) {
		for (auto& car : cars) {
			if (car.brand == brand && car.model == model && !car.isAvailable) {
				car.isAvailable = true;
				cout << "Car returned successfully." << endl;
				return;
			}
		}
	}

	void displayCustomerInfo(string name, string surname) {
		for (const Customer& customer : customers) {
			if (customer.name == name && customer.surname == surname) {
				cout << "Customer Name: " << customer.name << " " << customer.surname << endl;
				cout << "Gender: " << customer.gender << endl;
				cout << "Number of Rentals: " << customer.rentalHistory.size() << endl;

				if (!customer.rentalHistory.empty()) {
					cout << "\nRental History:" << endl;
					for (const RentalHistory& rental : customer.rentalHistory) {
						cout << " - " << rental.carBrand << " " << rental.carModel << endl;
					}
				}
				else {
					cout << "No rental history found." << endl;
				}

				return;
			}
		}
		cout << "Customer not found." << endl;
	}
	void registerNewCustomer() {
		Customer customer;

		cout << "Enter customer name: ";
		cin >> customer.name;
		cout << "Enter customer surname: ";
		cin >> customer.surname;
		cout << "Enter customer gender (M/F): ";
		cin >> customer.gender;

		customers.push_front(customer);
		numberOfCustomer++;

		cout << "Customer registered successfully." << endl;
	}
	void viewCustomerAccounts() {
		string name, surname;

		cout << "Do you want to view all accounts (A) or a specific account (S)?" << endl;
		char choice;
		cin >> choice;

		if (choice == 'A') {
			// Display all accounts
			for (const Customer& customer : customers) {
				cout << "Customer Name: " << customer.name << " " << customer.surname << endl;
				cout << "Number of Rentals: " << customer.rentalHistory.size() << endl << endl;
			}
		}
		else if (choice == 'S') {
			// Display specific account
			cout << "Enter customer name: ";
			cin >> name;
			cout << "Enter customer surname: ";
			cin >> surname;

			displayCustomerInfo(name, surname);
		}
		else {
			cout << "Invalid choice." << endl;
		}
	}
};

int main() {
	char choice;
	EliteDrive eliteDrive;
	Customer customer;
	string type, brand, model, year;
	try {
		eliteDrive.addCars();
	}
	catch (const runtime_error& e) {
		cerr << e.what() << endl;
	}

	do {
	    eliteDrive.carRentedMenu();
		cout << "Enter your choice: ";
		cin >> choice;
		if (choice == '1') {
			eliteDrive.displayCars();
		}
		else if (choice == '2') {
			cout << "Enter car type: ";
			cin >> type;
			cout << "Enter car brand: ";
			cin >> brand;
			cout << "Enter car model: ";
			cin >> model;
			cout << "Enter car year: ";
			cin >> year;
			cout << "Enter your name: ";
			cin >> customer.name;
			cout << "Enter your surname: ";
			cin >> customer.surname;
			cout << "Enter your gender (M/F): ";
			cin >> customer.gender;
			eliteDrive.rentCar(type, brand, model, year, customer);
		}
		else if (choice == '3') {
			cout << "Enter car brand: ";
			cin >> brand;
			cout << "Enter car model: ";
			cin >> model;
			eliteDrive.returnCar(brand, model);
		}
		else if (choice == '4') {
			string name, surname;
			cout << "Enter customer name: ";
			cin >> name;
			cout << "Enter customer surname: ";
			cin >> surname;
			eliteDrive.displayCustomerInfo(name, surname);
		}
		else if (choice == '5') {
			eliteDrive.registerNewCustomer();
		}
		else if (choice == '6') {
			eliteDrive.viewCustomerAccounts();
		}
		else if (choice == '7') {
			cout << "Exiting the application...";
		}
		else {
			cout << "Invalid input! Please try again.\n";
		}
	} while (choice != '7');
	return 0;
}
