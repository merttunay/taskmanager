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
- Git / GitHub

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

### Controller

`controller` katmanı, dışarıdan gelen HTTP isteklerini karşılar.

Bu projede iki controller bulunmaktadır:

- `HelloController`: Projenin çalışıp çalışmadığını test etmek için kullanılır.
- `TaskController`: Görevlerle ilgili API endpointlerini içerir.

### Entity

`entity` katmanı, veritabanındaki tabloları Java sınıfı olarak temsil eder.

Bu projede `Task` entity sınıfı bulunmaktadır.

`Task` sınıfında şu alanlar vardır:

- `id`: Görevin benzersiz kimliği
- `title`: Görev başlığı
- `description`: Görev açıklaması
- `completed`: Görevin tamamlanıp tamamlanmadığını belirtir

### Repository

`repository` katmanı, veritabanı işlemlerini yapar.

`TaskRepository`, `JpaRepository` sınıfını genişleterek temel veritabanı işlemlerini otomatik olarak sağlar.

Örneğin:

- Görevleri listeleme
- ID ile görev bulma
- Görev kaydetme
- Görev silme
- Başlığa göre arama
- Tamamlanma durumuna göre filtreleme

### Service

`service` katmanı, uygulamanın iş mantığını içerir.

Controller doğrudan repository ile konuşmaz. Bunun yerine controller, service katmanını çağırır.

Genel akış şu şekildedir:

```txt
Controller → Service → Repository → Database
```

### DTO

`dto` katmanı, dışarıdan gelen verileri taşımak ve kontrol etmek için kullanılır.

Bu projede `TaskRequest` DTO sınıfı bulunmaktadır.

`TaskRequest`, görev oluşturma ve görev güncelleme işlemlerinde kullanılır.

### Exception

`exception` katmanı, hata yönetimini düzenlemek için kullanılır.

Bu projede `GlobalExceptionHandler` sınıfı ile validation hataları daha anlaşılır JSON formatında döndürülür.

## Endpointler

## Test Endpoint

### Spring Boot çalışıyor mu kontrol etme

```http
GET /hello
```

Örnek cevap:

```txt
Merhaba, Spring Boot calisiyor!
```

---

## Task CRUD Endpointleri

### Tüm görevleri listeleme

```http
GET /tasks
```

Bu endpoint, sistemde kayıtlı tüm görevleri listeler.

Örnek cevap:

```json
[
  {
    "id": 1,
    "title": "Java çalış",
    "description": "Spring Boot öğren",
    "completed": false
  }
]
```

---

### ID ile görev getirme

```http
GET /tasks/{id}
```

Örnek:

```http
GET /tasks/1
```

Bu endpoint, belirtilen ID değerine sahip görevi getirir.

---

### Yeni görev oluşturma

```http
POST /tasks
```

Örnek body:

```json
{
  "title": "Java çalış",
  "description": "Spring Boot Controller öğren",
  "completed": false
}
```

Örnek cevap:

```json
{
  "id": 1,
  "title": "Java çalış",
  "description": "Spring Boot Controller öğren",
  "completed": false
}
```

---

### Görev güncelleme

```http
PUT /tasks/{id}
```

Örnek:

```http
PUT /tasks/1
```

Örnek body:

```json
{
  "title": "Java tekrar",
  "description": "CRUD endpointleri test edildi",
  "completed": true
}
```

Bu endpoint, belirtilen ID değerine sahip görevi günceller.

---

### Görev silme

```http
DELETE /tasks/{id}
```

Örnek:

```http
DELETE /tasks/1
```

Örnek cevap:

```txt
Görev silindi.
```

---

## Gelişmiş Endpointler

### Tamamlanma durumuna göre görevleri getirme

```http
GET /tasks/completed/{completed}
```

Tamamlanan görevleri getirmek için:

```http
GET /tasks/completed/true
```

Tamamlanmayan görevleri getirmek için:

```http
GET /tasks/completed/false
```

---

### Başlığa göre arama

```http
GET /tasks/search?title=java
```

Bu endpoint, başlığında belirli bir kelime geçen görevleri getirir.

Örnek:

```http
GET /tasks/search?title=java
```

---

### Başlık ve tamamlanma durumuna göre filtreleme

```http
GET /tasks/filter?title=spring&completed=true
```

Bu endpoint, hem başlığa hem de tamamlanma durumuna göre görevleri filtreler.

Örnek:

```http
GET /tasks/filter?title=spring&completed=true
```

---

### Tamamlanma durumuna göre görev sayısı

```http
GET /tasks/count?completed=true
```

Tamamlanan görevlerin sayısını verir.

```http
GET /tasks/count?completed=false
```

Tamamlanmayan görevlerin sayısını verir.

Örnek cevap:

```json
2
```

---

### Başlığa göre görev var mı kontrolü

```http
GET /tasks/exists?title=Java çalış
```

Bu endpoint, belirtilen başlığa sahip bir görev olup olmadığını kontrol eder.

Örnek cevap:

```json
true
```

veya:

```json
false
```

---

### Son 5 görevi getirme

```http
GET /tasks/latest
```

Bu endpoint, sisteme son eklenen 5 görevi getirir.

---

## Validation Kuralları

`TaskRequest` sınıfında validation kuralları uygulanmıştır.

### Başlık alanı

```java
@NotBlank(message = "Başlık boş bırakılamaz.")
@Size(min = 3, max = 100, message = "Başlık 3 ile 100 karakter arasında olmalıdır.")
private String title;
```

Başlık alanı:

- Boş bırakılamaz.
- En az 3 karakter olmalıdır.
- En fazla 100 karakter olmalıdır.

### Açıklama alanı

```java
@Size(max = 500, message = "Açıklama en fazla 500 karakter olabilir.")
private String description;
```

Açıklama alanı en fazla 500 karakter olabilir.

---

## Validation Hata Cevabı

Boş veya geçersiz veri gönderildiğinde uygulama `400 Bad Request` döndürür.

Örnek hatalı body:

```json
{
  "title": "",
  "description": "Deneme açıklama",
  "completed": false
}
```

Örnek hata cevabı:

```json
{
  "messages": {
    "title": "Başlık 3 ile 100 karakter arasında olmalıdır."
  },
  "error": "Validation Error",
  "timestamp": "2026-05-18T00:26:38.8994702",
  "status": 400
}
```

---

## Projeyi Çalıştırma

Projeyi çalıştırmak için IntelliJ IDEA üzerinden `TaskmanagerApplication.java` dosyasındaki `main` metodu çalıştırılır.

Alternatif olarak terminalden şu komut kullanılabilir:

```bash
mvn spring-boot:run
```

Uygulama varsayılan olarak şu adreste çalışır:

```txt
http://localhost:8080
```

---

## Postman ile Test

API testleri Postman üzerinden yapılmıştır.

Örnek test adresleri:

```http
GET http://localhost:8080/tasks
```

```http
POST http://localhost:8080/tasks
```

```http
GET http://localhost:8080/tasks/completed/true
```

```http
GET http://localhost:8080/tasks/search?title=java
```

---

## Git Commit Süreci

Proje geliştirme sürecinde aşamalı commit mantığı kullanılmıştır.

Commit süreci genel olarak şu adımlardan oluşur:

```bash
git status
git add .
git commit -m "Commit mesajı"
git push
```

Bu projede kullanılan commit aşamaları:

- Spring Boot projesi oluşturuldu
- Hello endpoint eklendi
- Task entity, repository ve service eklendi
- Task CRUD endpointleri eklendi
- DTO validation ve global hata yönetimi eklendi
- Görev arama, filtreleme ve sayma endpointleri eklendi
- README dosyası güncellendi

---

## Genel Proje Akışı

Bu projede isteklerin çalışma mantığı şu şekildedir:

```txt
Kullanıcı / Postman
        ↓
Controller
        ↓
Service
        ↓
Repository
        ↓
H2 Database
```

Örneğin yeni görev oluşturma işlemi:

```txt
POST /tasks
        ↓
TaskController
        ↓
TaskService
        ↓
TaskRepository
        ↓
H2 Database
```

---

## Sonuç

Bu proje ile Java Spring Boot kullanılarak katmanlı mimariye sahip basit bir REST API geliştirilmiştir.

Proje içerisinde CRUD işlemleri, DTO kullanımı, validation, global hata yönetimi ve özel repository sorguları uygulanmıştır.
