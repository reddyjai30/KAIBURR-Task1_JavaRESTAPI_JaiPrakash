# KAIBURR Coding Assignment
##### Name: Jai Prakash Reddy D 
##### R.no: CB.EN.U4CSE20027
###### Mail: reddyjai30@gmail.com



# Task 1 - Java REST API 

## Overview 
 This task involves creating a Java application that provides a REST API for managing "task" objects with specific attributes. It includes endpoints for       searching, creating, and deleting tasks, with data storage in a MongoDB database and functionality demonstrated using tool like Postman.
------  


## Getting Started

### Prerequisites

- **Java Development Kit (JDK)**: Install the latest version of [JDK](https://www.oracle.com/java/technologies/downloads/) to develop Java applications. 
- **Integrated Development Environment (IDE)**: An IDE like IntelliJ [Eclipse](https://www.eclipse.org/downloads/packages/release/luna/sr1/eclipse-ide-java-developers) is recommended for writing and managing your Java code.
- **MongoDB**: As our database, we'll need to install MongoDB (compass). You can download it from the [MongoDB Compass](https://www.mongodb.com/try/download/compass) official website.
- **Maven**: This is a build automation tool used for managing project dependencies and builds. Maven can be downloaded from [Apache Maven](https://maven.apache.org/download.cgi) Project.
- **Spring Boot**: This project will benefit from using Spring Boot for setting up the REST API. We can start with [Spring Initializr](https://start.spring.io) to bootstrap our project.
- **Postman**: For testing our REST API, we used here the tool [Postman](https://www.postman.com/downloads/)


## Setting Up the Project 

### Initialize a Spring Boot Project with Spring Initializr
   Spring Initializr is a convenient tool to bootstrap a new Spring Boot project.
  - Go to [Spring Initializr](https://start.spring.io/).
- Fill in the Project Metadata:
  - **Group**: `com.exampleJaiPrakash`
  - **Artifact**: `taskapi`
  - **Name**: `taskapi`
  - **Description**: A brief description of your project.
  - **Package name**: `com.exampleJaiPrakash.taskapi`
  - **Packaging**: Jar
  - **Java Version**: (Select the latest stable version)
- Add Dependencies:
  - **Spring Web**
  - **Spring Data MongoDB**
  - **Lombok**
- Click on "Generate" to download `ZIP` of the project setup.

<img width="1000" alt="Screenshot 2024-01-08 at 12 09 11 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/fa398474-eaf8-49d6-bb87-c71e8743ef62">

### Eclipse IDE Setup
Import the generated project into your Eclipse IDE
- Open Eclipse.
- Select `File` > `Import`.
- Choose `Existing Maven Projects` .
- Select and import the project folder.
<img width="1000" alt="Screenshot 2024-01-09 at 3 45 09 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/d3bad256-52b0-439d-a71f-a990f5d3f6a0">

### Configuring MongoDB
  #### 1. Install MongoDB Compass
  You can install MongoDB Compass locally as we done in our case by following the below steps:
  - Download MongoDB from the [MongoDB Compass](https://www.mongodb.com/try/download/compass).
  - Choose the correct version for your OS and follow the installation instructions.
  - Start the MongoDB service on your machine.
<img width="1000" alt="Screenshot 2024-01-09 at 3 57 28 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/1fbdb137-903d-4f5b-b5af-ea64a013140d">

  #### 2. Configuring MongoDB in our Application
  After installation, configure your Spring Boot application to connect to MongoDB.
  - Go to our eclipse project and Navigate under the section of `src/main/resources` and you will find `application.properties`.
  - In `application.properties` copy past the below line of code for establishing connectivity of our Spring Boot to MongoDB locally.
    
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/yourDatabase
```
In my case I have created a database in mongoDb called as `JavaApiJai` so mine will be like :
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/JavaApiJai
```

<img width="500" alt="Screenshot 2024-01-12 at 12 13 11 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/b4ff2095-72d1-4903-bf07-ca38c38f82f1"> <img width="500" alt="Screenshot 2024-01-12 at 12 34 32 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/a52b62ff-c432-4fad-9fd7-3777c2111ca5">


### Developing the Application
  #### 1. Create Model Class
  - Create `Task.java` in the model package.
  - Define properties: `name`, `id`, `assignee`, `project`, `startTime`, and a special property `jaiprakashProperty`.
  - Annotate the class with `@Document`, `@Id`, and other relevant JPA annotations.
  #### Task.java
  ```task.java
  package com.exampleJaiPrakash.taskapi.model;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

import java.util.Date;
import java.util.Random;

@Document
public class Task {
    @Id
    private String id;
    private String name;
    private String assignee;
    private String project;
    private Date startTime;
    private String jaiprakashProperty; // The dynamic property

    // Constructors

    public Task() {
        // Generate the jaiprakashProperty for a new Task
        generateJaiprakashProperty();
    }

    public Task(String name, String assignee, String project, Date startTime) {
        this.name = name;
        this.assignee = assignee;
        this.project = project;
        this.startTime = startTime;
        generateJaiprakashProperty();
    }

    // Method to generate jaiprakashProperty
    public void generateJaiprakashProperty() {
        String candidateName = "JaiPrakash";
        Random random = new Random();
        StringBuilder sb = new StringBuilder(5);
        for (int i = 0; i < 5; i++) {
            sb.append(candidateName.charAt(random.nextInt(candidateName.length())));
        }
        this.jaiprakashProperty = sb.toString();
    }

    // Getters and setters for all fields except for jaiprakashProperty (no setter for it)

    // Other methods, getters and setters

 // Getters and Setters
    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAssignee() {
        return assignee;
    }

    public void setAssignee(String assignee) {
        this.assignee = assignee;
    }

    public String getProject() {
        return project;
    }

    public void setProject(String project) {
        this.project = project;
    }

    public Date getStartTime() {
        return startTime;
    }

    public void setStartTime(Date startTime) {
        this.startTime = startTime;
    }

    public String getJaiprakashProperty() {
        return jaiprakashProperty;
    }
    



}
```
<img width="1000" alt="Screenshot 2024-01-12 at 12 44 34 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/ae60dfb7-e250-4c86-af07-50a8a87326c1">

  #### 2. Repository Layer
  - Create `TaskRepository` interface extending `MongoRepository<Task, String>`.
  - This provides basic CRUD operations.

    #### TaskRepository.java
      ```task.java
          package com.exampleJaiPrakash.taskapi.repository;

          import com.exampleJaiPrakash.taskapi.model.Task;
          import org.springframework.data.mongodb.repository.MongoRepository;
          import java.util.List;

          public interface TaskRepository extends MongoRepository<Task, String> {
            List<Task> findByNameContaining(String name);
            List<Task> findFirst10ByAssigneeOrderByStartTime(String assignee);
          }
      ```
<img width="1000" alt="Screenshot 2024-01-12 at 12 51 31 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/17b81bdb-c41c-4f98-99a6-2219e4d421ab">

  #### 3. Service Layer
  - Create a `TaskService` class.
  - Implement business logic for task operations.
  - Include logic to generate a random string for the special property.

#### TaskService.java
```task.java
package com.exampleJaiPrakash.taskapi.service;

import com.exampleJaiPrakash.taskapi.model.Task;
import com.exampleJaiPrakash.taskapi.repository.TaskRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;


@Service
public class TaskService {

    private final TaskRepository taskRepository;

    @Autowired
    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }

    // Get all tasks
    public List<Task> getAllTasks() {
        return taskRepository.findAll();
    }

    // Get a single task by ID
    public Optional<Task> getTaskById(String id) {
        return taskRepository.findById(id);
    }

    // Add or update a task
    public Task saveTask(Task task) {
        // If the task is new (i.e., has no id), or you want to regenerate the custom property every time
        if (task.getId() == null || task.getId().isEmpty()) {
            task.generateJaiprakashProperty(); // This will generate a new custom property
        }
        return taskRepository.save(task);
    }

    // Delete a task
    public void deleteTask(String id) {
        taskRepository.deleteById(id);
    }

    // Find tasks by name containing a string
    public List<Task> findTasksByName(String name) {
        return taskRepository.findByNameContaining(name);
    }

    // Find the first 10 tasks by assignee
    public List<Task> findFirst10TasksByAssignee(String assignee) {
        return taskRepository.findFirst10ByAssigneeOrderByStartTime(assignee);
    }
}

```
<img width="1000" alt="Screenshot 2024-01-12 at 1 00 10 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/f2c40319-b0a1-4ff4-9f85-ae77935ffd07">


  #### 4. Controller Layer
  - Create `TaskController` class.
  - Use `@RestController` and `@RequestMapping`.
  - Define methods for GET, POST, PUT, DELETE requests, corresponding to API endpoints.

    #### TaskController.java
```taskcontroller.java
package com.exampleJaiPrakash.taskapi.controller;

import com.exampleJaiPrakash.taskapi.model.Task;
import com.exampleJaiPrakash.taskapi.service.TaskService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/tasks")
public class TaskController {

    private final TaskService taskService;

    @Autowired
    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }

 // GET /tasks - Retrieve all tasks
    @GetMapping
    public ResponseEntity<List<Task>> getAllTasks() {
        List<Task> tasks = taskService.getAllTasks();
        return ResponseEntity.ok(tasks);
    }

    // GET /tasks/{id} - Retrieve a specific task by ID
    @GetMapping("/{id}")
    public ResponseEntity<Task> getTaskById(@PathVariable String id) {
        Optional<Task> task = taskService.getTaskById(id);
        return task.map(ResponseEntity::ok)
                   .orElse(ResponseEntity.notFound().build());
    }

    // PUT /tasks - Create or update a task
    @PutMapping
    public ResponseEntity<Task> createOrUpdateTask(@RequestBody Task task) {
        Task savedTask = taskService.saveTask(task);
        return ResponseEntity.ok(savedTask);
    }

    // DELETE /tasks/{id} - Delete a task by ID
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteTask(@PathVariable String id) {
        taskService.deleteTask(id);
        return ResponseEntity.ok().build();
    }

 // GET /tasks/search/byName - Find tasks by name
    @GetMapping("/search/byName")
    public ResponseEntity<List<Task>> findTasksByName(@RequestParam String name) {
        List<Task> tasks = taskService.findTasksByName(name);
        if (tasks.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else {
            return ResponseEntity.ok(tasks);
        }
    }

    // GET /tasks/search/byAssignee - Find first 10 tasks by assignee
    @GetMapping("/search/byAssignee")
    public ResponseEntity<List<Task>> findFirst10TasksByAssignee(@RequestParam String assignee) {
        List<Task> tasks = taskService.findFirst10TasksByAssignee(assignee);
        if (tasks.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else {
            return ResponseEntity.ok(tasks);
        }
    }

}

```

<img width="1000" alt="Screenshot 2024-01-12 at 1 03 26 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/f0af99ce-9c73-447d-a1df-088d339a2943">



## Running the Project
Once you've completed the development and build process, the next step is to run your Spring Boot application.
- Locate the TaskapiApplication.java class (under `src/main/java/com/exampleJaiPrakash/taskapi/TaskapiApplication.java`).
- Right-click on the file and choose "Run".
  <img width="1280" alt="Screenshot 2024-01-12 at 1 18 12 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/ada4acec-9261-4545-af31-dc992b57c58f">

  Once the application is running, it should be accessible on the default port, which is typically 8080, unless specified otherwise in the    application.properties file.
  - You can verify that the application is running correctly by opening a web browser and navigating to http://localhost:8080.
    <img width="800" alt="Screenshot 2024-01-12 at 1 24 01 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/baa95953-a3f3-408b-b082-dfcdd5d59fde">

  - Alternatively, you can use Postman to send a request to the application and check the response.
    <img width="500" alt="Screenshot 2024-01-12 at 1 51 12 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/0923e3b4-6373-4a6e-874b-7c50a4095ae6"> <img width="500" alt="Screenshot 2024-01-12 at 1 51 49 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/6b62c85c-bc9e-404f-8237-7632e5c0e581">


  - After sending the request (`PUT`) then the request gets stored in the mongoDb database too.
    <img width="1000" alt="Screenshot 2024-01-12 at 1 52 23 PM" src="https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/7f92cb05-3fab-4dd2-ae22-7c65a1625830">



  


