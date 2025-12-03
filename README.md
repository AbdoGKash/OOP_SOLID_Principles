# 🚀 Welcome to the OOP_SOLID_Principles repository!

The Smart Ahwa Manager is a Flutter application designed to help a coffee shop owner in Cairo efficiently manage operations. The app allows adding new customer orders with name, drink type, and special instructions; marking orders as completed; displaying a dashboard of pending orders; and generating a daily report of top-selling drinks and total orders served.

# Dependencies

* flutter_riverpod for state management

* Flutter core libraries (material)

  
# 🔧 OOP Principles Applied : 

**Encapsulation**

* We grouped related data into a single class, and made _isCompleted private, controlling access to it through a getter and a setter method.


```dart
class Order {
  final String customerName;
  final String drinkType;
  final String specialInstructions;
  bool _isCompleted;

  Order({
    required this.customerName,
    required this.drinkType,
    required this.specialInstructions,
    bool isCompleted = false,
  }) : _isCompleted = isCompleted;

  bool get isCompleted => _isCompleted;

  void completeOrder() {
    _isCompleted = true;
  }
}
```

**Inheritance & Polymorphism & Abstraction**
  
* IReportGenerator is the abstraction. DailyReportGenerator is one implementation. Any other generator (e.g., WeeklyReportGenerator) can replace it without changing client code.


```dart
abstract class IReportGenerator {
  String generateReport(List<Order> orders);
}
```

```dart
class DailyReportGenerator implements IReportGenerator {
  @override
  String generateReport(List<Order> orders) {
    final totalOrders = orders.length;
    final drinksCount = <String, int>{};

    for (var order in orders) {
      drinksCount[order.drinkType] = (drinksCount[order.drinkType] ?? 0) + 1;
    }

    final topDrink = drinksCount.entries.isNotEmpty
        ? drinksCount.entries.reduce((a, b) => a.value > b.value ? a : b).key
        : 'No orders yet';

    return 'Total Orders: $totalOrders\nTop Drink: $topDrink';
  }
}
```
```dart
// This demonstrates the Polymorphism principle since we can replace
// DailyReportGenerator with any other implementation (e.g., WeeklyReportGenerator)
// without affecting the rest of the code.
final reportGeneratorProvider = Provider<IReportGenerator>((ref) {
  return DailyReportGenerator();   
});

final reportProvider = Provider.family<String, List<Order>>((ref, orders) {
  final reportGenerator = ref.read(reportGeneratorProvider);
  return reportGenerator.generateReport(orders);
});
```


# SOLID Principles Applied

**Single Responsibility Principle (SRP)**

* Each class has one responsibility:

* Order → represents order data.

* OrderManager → manages adding/completing orders.

* DailyReportGenerator → generates reports.



**Open/Closed Principle (OCP)**

* System is open to extension but closed to modification:

* Add WeeklyReportGenerator without changing existing code.



**Liskov Substitution Principle (LSP)**

* Any class implementing IReportGenerator can replace another without breaking behavior.



**Interface Segregation Principle (ISP)**

* IReportGenerator is small and focused, avoiding unnecessary methods.



**Dependency Inversion Principle (DIP)**

* High-level modules depend on abstractions, not implementations:
* The app depends on IReportGenerator (abstraction), not directly on DailyReportGenerator.

```dart
final reportGeneratorProvider = Provider<IReportGenerator>((ref) {
  return DailyReportGenerator();   
});
```


# 🎨 UI 

<img src="https://github.com/user-attachments/assets/f45f806d-415e-4a6b-a5d0-7d141e5d2919" width="200">
<img src="https://github.com/user-attachments/assets/2f45e486-d6d5-4790-bc3d-9d8e081c3f77" width="200">
<img src="https://github.com/user-attachments/assets/38056268-adfe-455c-ace7-3e76e2b70a0c" width="200">
<img src="https://github.com/user-attachments/assets/ab5c13ea-971b-4f68-bbf8-906011bfd432" width="200">

