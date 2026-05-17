# Task Manager

Bu proje, Java Spring Boot kullanılarak geliştirilmiş basit bir görev yönetimi REST API uygulamasıdır.

Proje kapsamında görev ekleme, listeleme, güncelleme, silme, arama, filtreleme ve sayma işlemleri yapılabilmektedir.

## Kullanılan Teknolojiler

- Java 17
- Spring Boot
- Spring Web
- Spring Data JPA
- H2 Database
- Jakarta Validation
- Maven
- Postman

## Proje Yapısı

```txt
src/main/java/org.example.taskmanager
 ├── controller
 │   ├── HelloController.java
 │   └── TaskController.java
 ├── dto
 │   └── TaskRequest.java
 ├── entity
 │   └── Task.java
 ├── exception
 │   └── GlobalExceptionHandler.java
 ├── repository
 │   └── TaskRepository.java
 ├── service
 │   └── TaskService.java
 └── TaskmanagerApplication.java