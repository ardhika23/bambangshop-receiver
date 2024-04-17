# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [✔️] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✔️] Commit: `Create Notification model struct.`
    -   [✔️] Commit: `Create SubscriberRequest model struct.`
    -   [✔️] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [✔️] Commit: `Implement add function in Notification repository.`
    -   [✔️] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [✔️] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✔️] Commit: `Create Notification service struct skeleton.`
    -   [✔️] Commit: `Implement subscribe function in Notification service.`
    -   [✔️] Commit: `Implement subscribe function in Notification controller.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification service.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✔️] Commit: `Implement receive_notification function in Notification service.`
    -   [✔️] Commit: `Implement receive function in Notification controller.`
    -   [✔️] Commit: `Implement list_messages function in Notification service.`
    -   [✔️] Commit: `Implement list function in Notification controller.`
    -   [✔️] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. 'RwLock<>' is necessary in this case to control access to the 'Vec' of Notifications in a way that allows multiple readers simultaneously while ensuring exclusive access for writers. This is important because it's common to have many reads and relatively few writes for a collection of notifications. 'RwLock<>' makes the data accessible to any number of readers until a writer needs exclusive access, which maximizes concurrent access and efficiency.
A 'Mutex<>', on the other hand, would enforce exclusive access for both reading and writing operations, which can lead to unnecessary blocking. If a thread holds a 'Mutex<>' for reading, other threads that want to read would have to wait, even though they would not interfere with each other. Therefore, using 'RwLock<>' instead of 'Mutex<>' allows for more fine-grained control of concurrency, improving performance in situations where data is mostly read rather than written.

2. Rust takes a more conservative approach to concurrency and memory safety than Java. In Rust, the content of a static variable is immutable by default because Rust's ownership and borrowing rules are designed to prevent data races at compile time. Data races can occur when two or more threads access the same memory location concurrently and at least one thread is writing.
Mutable static variables could introduce unsafe access patterns that are difficult to manage, as any part of the code could potentially change the variable's state, leading to unpredictable behavior. The 'lazy_static' library allows us to safely declare and initialize static variables that are lazily evaluated and can include mutable data types like 'Vec' and 'DashMap', but we use synchronization primitives like 'Mutex' or 'RwLock' to safely manage access to their contents. This enforces thread safety by ensuring that mutable statics are not accessed concurrently from multiple threads without proper synchronization, aligning with Rust’s guarantees of memory safety.

#### Reflection Subscriber-2
1. Exploring 'src/lib.rs' provided a deeper understanding of how Rust organizes library crates and modules. By looking at the root of the library crate, I learned about module declaration and how public interfaces are exposed. It was particularly insightful to see the application's entry point and how different components are wired together, providing a holistic view of the app's architecture beyond the step-by-step instructions.

2. The Observer pattern inherently supports scalability by decoupling subjects (publishers) from observers (subscribers). By following this pattern, adding new subscribers is straightforward, as they simply need to be registered with the publisher. The publisher maintains a list of subscribers and notifies them without being concerned with their specific implementation. This decoupling makes the system flexible and eases the addition of new instances of the Receiver app.

3. I have not yet ventured into writing my own tests or enhancing the Postman documentation, as my focus was on grasping the core tutorial concepts and ensuring that the implementation was correct. As I become more comfortable with these foundational elements, I plan to expand my skills to include these practices, which are undoubtedly valuable for ensuring code quality and maintainability.