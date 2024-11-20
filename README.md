# Solid_Principles
1. Single Responsibility Principle (SRP)

A class or module should have one and only one reason to change. In Angular, this means ensuring components, services, or directives have a single responsibility.

// Violating SRP
export class UserComponent {
  constructor(private userService: UserService) {}

  saveUser(data: any) {
    this.userService.save(data); // Handles business logic
    console.log('User saved!'); // Handles logging
  }
}

// Following SRP
export class UserComponent {
  constructor(private userService: UserService, private logger: LoggerService) {}

  saveUser(data: any) {
    this.userService.save(data); 
    this.logger.log('User saved!');
  }
}

Real-World Use Case:
Imagine a dashboard component that fetches data, applies filters, and formats dates. These responsibilities can be split into services for fetching, filtering, and formatting.

2. Open/Closed Principle (OCP)

Software entities should be open for extension but closed for modification. Angular’s dependency injection and module system make this principle achievable.

// Open for extension but closed for modification
export abstract class NotificationService {
  abstract notify(message: string): void;
}

export class EmailNotificationService extends NotificationService {
  notify(message: string) {
    console.log(`Email sent: ${message}`);
  }
}

export class SMSNotificationService extends NotificationService {
  notify(message: string) {
    console.log(`SMS sent: ${message}`);
  }
}

// Usage
const notificationService: NotificationService = new EmailNotificationService();
notificationService.notify('Welcome!');

Real-World Use Case:
An e-commerce app supporting multiple payment gateways can use this principle by defining a generic payment interface that is extended by gateway-specific implementations.

3. Liskov Substitution Principle (LSP)

Subtypes must be substitutable for their base types without altering the correctness of the program.
// Violating LSP
class Rectangle {
  setWidth(width: number) { /* ... */ }
  setHeight(height: number) { /* ... */ }
}

class Square extends Rectangle {
  setWidth(width: number) {
    this.setHeight(width); // Breaks substitution
  }
}

// Following LSP
abstract class Shape {
  abstract calculateArea(): number;
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {}
  calculateArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private side: number) {}
  calculateArea() {
    return this.side * this.side;
  }
}

Real-World Use Case:
When creating reusable form controls, ensure that a dropdown or input field can be substituted without breaking the form behavior.

4. Interface Segregation Principle (ISP)

Clients should not be forced to depend on interfaces they do not use. In Angular, break large services into smaller, more focused interfaces.
// Violating ISP
export interface FullReport {
  generatePDF(): void;
  generateCSV(): void;
  generateExcel(): void;
}

// Following ISP
export interface PDFReport {
  generatePDF(): void;
}

export interface CSVReport {
  generateCSV(): void;
}

export class ReportGenerator implements PDFReport, CSVReport {
  generatePDF() { /* ... */ }
  generateCSV() { /* ... */ }
}

Real-World Use Case:
A reporting module may generate multiple file formats. Define separate interfaces for each format rather than a bloated one.

5. Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions. Use Angular's dependency injection to implement this.

// Following DIP
export interface DataService {
  fetchData(): any;
}

@Injectable()
export class ApiService implements DataService {
  fetchData() {
    return fetch('/api/data');
  }
}

@Injectable()
export class Component {
  constructor(private dataService: DataService) {}

  loadData() {
    this.dataService.fetchData();
  }
}

Real-World Use Case:
Switching between a mock service and a real API service during development/testing becomes seamless with DIP.
