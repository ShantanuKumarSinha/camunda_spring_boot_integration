# Camunda_spring_boot_integration

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
# Camunda Spring Boot Integration

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Camunda](https://img.shields.io/badge/Camunda-8.x-orange.svg)](https://camunda.com/)

This project demonstrates the integration of Camunda BPM (Business Process Management) with Spring Boot, providing a production-ready foundation for building workflow and business process automation applications.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [BPMN Process Examples](#bpmn-process-examples)
- [API Endpoints](#api-endpoints)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## 🎯 Overview

This application showcases how to integrate Camunda BPM Platform with Spring Boot to create automated business processes. Camunda is a powerful workflow and decision automation platform that allows you to model, deploy, and monitor business processes using BPMN 2.0 standards.

## ✨ Features

- **Seamless Integration**: Complete integration of Camunda BPM with Spring Boot
- **BPMN 2.0 Support**: Full support for BPMN 2.0 workflow modeling
- **REST API**: RESTful endpoints for process management
- **Process Automation**: Automated execution of business processes
- **Task Management**: Human task management and assignment
- **Process Monitoring**: Real-time process instance monitoring
- **History & Audit**: Complete audit trail of process executions
- **Camunda Cockpit**: Web-based monitoring and operations tool
- **Camunda Tasklist**: User task management interface
- **Database Persistence**: Configurable database for process state persistence

## 🔧 Prerequisites

Before you begin, ensure you have the following installed:

- **Java Development Kit (JDK)**: Version 11 or higher
- **Maven**: Version 3.6 or higher (or use Maven wrapper)
- **Database** (Optional): H2 (embedded), PostgreSQL, MySQL, or other supported database
- **IDE** (Recommended): IntelliJ IDEA, Eclipse, or VS Code with Java extensions

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/ShantanuKumarSinha/camunda_spring_boot_integration.git
   cd camunda_spring_boot_integration
   ```

2. **Build the project**
   ```bash
   mvn clean install
   ```
   
   Or using Gradle:
   ```bash
   ./gradlew build
   ```

3. **Run the application**
   ```bash
   mvn spring-boot:run
   ```
   
   Or using Gradle:
   ```bash
   ./gradlew bootRun
   ```

4. **Access the application**
   - Application: `http://localhost:8080`
   - Camunda Cockpit: `http://localhost:8080/camunda/app/cockpit`
   - Camunda Tasklist: `http://localhost:8080/camunda/app/tasklist`
   - Camunda Admin: `http://localhost:8080/camunda/app/admin`
   
   Default credentials: `demo` / `demo`

## ⚙️ Configuration

### Database Configuration

By default, the application uses an embedded H2 database. To configure a different database, update `application.properties` or `application.yml`:

**PostgreSQL Example:**
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/camunda
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver
```

### Camunda Configuration

Configure Camunda properties in your `application.properties`:

```properties
# Camunda Admin User
camunda.bpm.admin-user.id=admin
camunda.bpm.admin-user.password=admin
camunda.bpm.admin-user.firstName=Admin

# Process Engine
camunda.bpm.auto-deployment-enabled=true
camunda.bpm.history-level=full

# Job Executor
camunda.bpm.job-execution.enabled=true
camunda.bpm.job-execution.max-pool-size=10
```

## 🚀 Usage

### Starting a Process Instance

You can start a process instance programmatically:

```java
@Autowired
private RuntimeService runtimeService;

public void startProcess() {
    ProcessInstance processInstance = runtimeService
        .startProcessInstanceByKey("your-process-key");
    System.out.println("Process started with ID: " + processInstance.getId());
}
```

### Completing a User Task

```java
@Autowired
private TaskService taskService;

public void completeTask(String taskId) {
    Map<String, Object> variables = new HashMap<>();
    variables.put("approved", true);
    taskService.complete(taskId, variables);
}
```

### REST API Example

Start a process via REST API:
```bash
curl -X POST http://localhost:8080/api/process/start \
  -H "Content-Type: application/json" \
  -d '{"processKey": "your-process-key", "variables": {}}'
```

## 📁 Project Structure

```
camunda_spring_boot_integration/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── camunda/
│   │   │               ├── CamundaApplication.java
│   │   │               ├── controller/
│   │   │               ├── service/
│   │   │               ├── delegate/
│   │   │               └── config/
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── processes/
│   │       │   └── *.bpmn
│   │       └── static/
│   └── test/
│       └── java/
├── pom.xml / build.gradle
└── README.md
```

## 🛠️ Technologies Used

- **[Spring Boot](https://spring.io/projects/spring-boot)**: Application framework
- **[Camunda BPM](https://camunda.com/)**: Business Process Management platform
- **[Java](https://www.oracle.com/java/)**: Programming language
- **[Maven](https://maven.apache.org/)** / **[Gradle](https://gradle.org/)**: Build tool
- **[H2 Database](https://www.h2database.com/)**: Embedded database (default)
- **[JUnit](https://junit.org/)**: Testing framework
- **[Mockito](https://site.mockito.org/)**: Mocking framework for tests

## 📊 BPMN Process Examples

The project includes example BPMN processes demonstrating:

- **Simple Process**: Basic start-to-end process flow
- **User Tasks**: Processes requiring human interaction
- **Service Tasks**: Automated tasks calling Java services
- **Gateways**: Decision points and parallel flows
- **Events**: Timer events, message events, and error handling
- **Subprocesses**: Reusable process components

## 🔌 API Endpoints

### Process Management

- `POST /api/process/start` - Start a new process instance
- `GET /api/process/{id}` - Get process instance details
- `DELETE /api/process/{id}` - Delete a process instance

### Task Management

- `GET /api/tasks` - Get all active tasks
- `GET /api/tasks/{id}` - Get task details
- `POST /api/tasks/{id}/complete` - Complete a task
- `POST /api/tasks/{id}/claim` - Claim a task

## 🧪 Testing

Run the test suite:

```bash
mvn test
```

Or using Gradle:
```bash
./gradlew test
```

The project includes:
- **Unit Tests**: Testing individual components
- **Integration Tests**: Testing Camunda process execution
- **Process Tests**: Testing BPMN process flows

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please ensure your code follows the project's coding standards and includes appropriate tests.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📧 Contact

**Shantanu Kumar Sinha**

- GitHub: [@ShantanuKumarSinha](https://github.com/ShantanuKumarSinha)
- Project Link: [https://github.com/ShantanuKumarSinha/camunda_spring_boot_integration](https://github.com/ShantanuKumarSinha/camunda_spring_boot_integration)

## 🙏 Acknowledgments

- [Camunda Documentation](https://docs.camunda.org/)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [BPMN 2.0 Specification](https://www.omg.org/spec/BPMN/2.0/)

## Local setup
-  H2 Database Console: `http://localhost:8080/h2-console`
- Camunda Admin: `http://localhost:8080/camunda/app/admin`
---

⭐ If you find this project helpful, please consider giving it a star!
