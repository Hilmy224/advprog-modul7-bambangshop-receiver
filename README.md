# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1.In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust) in this BambangShop case, or a single Model `struct` is enough?
+ Not nessecarily, we don't  need to implement an interface since all Subscribers exhibit identical behavior That negates the main purpose of traits, which is to introduce and distinguish various behaviors for Subscribers. However, if there's a plan in the future to have Subscribers with distinct behaviors, it's probably a good idea to implement an interface.

2.`id` in Program and `url` in `Subscriber` is intended to be unique. Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary) like we currently use is necessary for this case?
+ Since `ids` and `urls` are meant to be unique, implementing keys for their identifiers makes DashMaps a more suitable choice, offering better operational efficiency compared to a single list/Vec. This is because Vec is more suited for smaller datasets with a fixed order and it doesn't perform well with datasets containing unique identifiers like keys, resulting in a linear lookup time as it needs to iterate through the list. On the contrary, DashMaps resemble dictionaries or hashmaps/maps, operating based on key-value pairs in datasets which makes it a more apporipate option.

3.When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (`SUBSCRIBERS`) static variable, we used the `DashMap` external library for `thread safe HashMap`. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
+ It depends for example, using a Singleton pattern controls access to a shared instance of a data structure, ensuring its uniqueness throughout the program's life and simplifying dependencies. However, employing a library like DashMap offers a more efficient solution for managing shared data structures in a multithreaded environment. Also, Singleton is crucial to ensure that multiple threads access only one instance of the SUBSCRIBER instance from DashMap.

#### Reflection Publisher-2
1.In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
+ By separating the "Service" and "Repository" from the Model, we adhere to the principle of Separation of Concerns. This means each component has a specific responsibility.The Model focuses on representing data, while the Service handles business logic, like processing data, and the Repository deals with data storage and retrieval. This separation makes the codebase easier to understand, maintain, and test, as each component focuses on a single aspect of the application's functionality. It also promotes code reusability and scalability, as changes to one component are less likely to impact others.
2.What happens if we only use the Model? Explain your imagination on how the interactions between each model (`Program, Subscriber, Notification`) affect the code complexity for each model?
+ Overall, relying solely on the Model layer without separating concerns will overly complicate the code leading to a codebase that is harder to understand, maintain and test and also each module relying eachother in a single layer will drastically increase code complexity and high risk of making it tightly coupled.
3.Have you explored more about `Postman`? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
+ Postman is like a special toolbox for testing and developing software, especially when you're working with APIs. It helps you check if your APIs are doing what they're supposed to by sending requests and getting responses back. You can also automate these tests, so you don't have to do them manually every time. Postman lets you set up different environments, like for testing and for the real deal, so you can see how your APIs behave in different situations. Plus, it has this cool feature where you can create pretend versions of your APIs called mock servers, which are handy when you're still building the real thing. And if you need to show others how to use your APIs, Postman can even help you create documentation to explain everything clearly. Overall, Postman makes it easier to test, develop, and share your APIs, which is should be super helpful for any future software projects.

#### Reflection Publisher-3
1.Observer Pattern has two variations: `Push model` (publisher pushes data to subscribers) and `Pull model` (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
+ The Observer Pattern comes in two variations: the Push model and the Pull model. In the Push model, the publisher actively sends updates to its subscribers. Conversely, in the Pull model, subscribers fetch updates from the publisher when needed. In this specific tutorial scenario, the Push Model is employed. We see this in action when any new request, such as creating, deleting, or publishing a Product, triggers the NotificationService to automatically dispatch update notifications to all Subscribers who have subscribed to that particular product type.

2.What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
+ The advange on implementing the Pull model in this scenario would make the Subscriber to proactively request data from the publisher, granting them control over when they receive updates. 
+ However, this approach has its disadvantages as it renders the notification system redundant. Instead of automated notifications, Subscribers would need to possess knowledge of the data structure they wish to retrieve, essentially transforming the system into a manual, pull-based process. This not only complicates the Subscriber's interaction with the system but also undermines the purpose of having a notification service in the first place.

3.Explain what will happen to the program if we decide to not use multi-threading in the notification process.
+ If we were to decide to not use multi-threading, the program would dispatch notifications sequentially, one after another, potentially resulting in significant delays, especially when dealing with multiple Subscribers. This sequential approach would notably slow down the process, as each notification must wait for the previous one to complete before being sent.
