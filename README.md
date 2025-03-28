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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

> In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

In this tutorial, we use RwLock<> to synchronize access to the Vec of notifications because read operations are more frequent than write operations. RwLock<> is a better choice than Mutex<> in this case because it allows multiple readers to access the data concurrently, while still ensuring that only one writer can modify the data at a time. This improves performance since multiple threads can read notifications without blocking each other.

If we used Mutex<> instead, it would lock the entire data structure for both reading and writing, meaning that even read operations would block each other. This would lead to unnecessary contention and reduce efficiency, especially in a scenario where reading notifications is much more common than writing them.

> In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Rust does not allow direct mutation of a static variable to ensure thread safety and prevent race conditions, especially in multi-threaded applications. Unlike Java, where a static variable can be modified via a static function, Rust enforces stricter safety guarantees by restricting direct mutable access to static variables. Instead, we use the lazy_static library to define mutable static variables in a controlled manner. By wrapping them in synchronization primitives like Mutex or RwLock, we ensure that modifications are safe and properly synchronized. This approach helps prevent data corruption and concurrency issues, making Rust’s memory safety model more robust.

#### Reflection Subscriber-2

> Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Yes, I have explored src/lib.rs a bit. From my exploration, this file contains several helper functions that assist in tasks such as loading information from .env files and handling custom error responses. Additionally, I also looked into the use of RwLock<> and Mutex<> for managing concurrent access to shared data. Understanding when to use RwLock<> (for read-heavy cases) and when to use Mutex<> (for exclusive access) is crucial for ensuring efficient and safe data handling. This exploration helped me gain deeper insights into Rust’s concurrency mechanisms and how they apply to real-world scenarios.

> Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

The Observer Pattern makes it easy to add new subscribers because we can simply register them without modifying the publisher's code. This works because the publisher only interacts with subscribers through a generic Observer interface, without needing to know their specific implementations. This flexibility allows for seamless scalability when adding more subscribers.

However, if we spawn multiple instances of the Main app, things become more complex. For example, if there are multiple publishers, but a subscriber is only connected to one of them, we need to ensure that notifications from other publishers still reach the appropriate subscribers. This would require additional logic to manage cross-instance communication, which could introduce potential challenges or bugs in the system.

> Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Yes, I have added several tests in Postman to validate the endpoints I created. These tests focus on checking the subscription feature in the publisher application, particularly by testing different variations of product types. I found this feature extremely useful in ensuring that my endpoints work correctly and behave as expected.

Additionally, I have improved the documentation in my Postman collection to make it easier for me to understand the purpose of each test later on. This has helped me maintain clarity in my testing process. Overall, I believe that Postman is a valuable tool for API testing, and I expect it will continue to be helpful in future tutorials and group projects.
