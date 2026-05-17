# Task Manager

Java Spring Boot ile geliştirilmiş basit bir görev yönetimi REST API projesidir.

Bu projede görev ekleme, listeleme, güncelleme, silme, arama, filtreleme ve sayma işlemleri yapılabilmektedir.

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
```

## Katmanların Görevleri

- `controller`: HTTP isteklerini karşılar.
- `service`: İş mantığının bulunduğu katmandır.
- `repository`: Veritabanı işlemlerini yapar.
- `entity`: Veritabanı tablosunu temsil eder.
- `dto`: Dışarıdan gelen verileri kontrol etmek için kullanılır.
- `exception`: Hataları düzenli şekilde döndürür.

## Endpointler

### Test

```http
GET /hello
```

### CRUD İşlemleri

```http
GET /tasks
GET /tasks/{id}
POST /tasks
PUT /tasks/{id}
DELETE /tasks/{id}
```

### Gelişmiş İşlemler

```http
GET /tasks/completed/{completed}
GET /tasks/search?title=java
GET /tasks/filter?title=spring&completed=true
GET /tasks/count?completed=true
GET /tasks/exists?title=Java çalış
GET /tasks/latest
```

## Örnek Görev Oluşturma

```http
POST /tasks
```

Örnek JSON body:

```json
{
  "title": "Java çalış",
  "description": "Spring Boot öğren",
  "completed": false
}
```

## Validation

`TaskRequest` sınıfında başlık ve açıklama alanları için validation kuralları uygulanmıştır.

- `title` boş bırakılamaz.
- `title` 3 ile 100 karakter arasında olmalıdır.
- `description` en fazla 500 karakter olabilir.

Geçersiz veri gönderildiğinde uygulama `400 Bad Request` döndürür.

## Projeyi Çalıştırma

IntelliJ IDEA üzerinden `TaskmanagerApplication.java` dosyasındaki `main` metodu çalıştırılır.

Alternatif olarak terminalden:

```bash
mvn spring-boot:run
```

Uygulama varsayılan olarak şu adreste çalışır:

```txt
http://localhost:8080
```

## Test

API testleri Postman üzerinden yapılmıştır.
