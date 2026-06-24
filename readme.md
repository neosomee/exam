# Ответы на задания

## 1. Теоретические вопросы

### SQL-запрос

```sql
SELECT name
FROM users
WHERE age > 18 AND city = 'Москва';
```

### Что делает `@RestController`?

`@RestController` используется для создания REST API. Это комбинация `@Controller` и `@ResponseBody`, поэтому методы возвращают данные напрямую в HTTP-ответ. `@Controller` обычно возвращает представления (HTML-страницы).

### Что такое `JpaRepository`?

`JpaRepository` — интерфейс Spring Data JPA для работы с БД. Предоставляет готовые CRUD-операции: `save`, `findById`, `findAll`, `delete`, `count`, а также пагинацию и сортировку.

### Для чего используется Liquibase?

Liquibase используется для управления миграциями базы данных: позволяет версионировать изменения схемы и автоматически применять их при запуске приложения.

### Что выведет код?

Результат:

```text
ANNA
ALEX
```

Сначала `filter` оставляет строки, начинающиеся на `"A"`, затем `map` переводит их в верхний регистр, а `forEach` выводит результат.

---

## 2. OrderService

```java
public class OrderService {

    public BigDecimal getPaidOrdersTotal(List<Order> orders) {
        return orders.stream()
                .filter(Order::isPaid)
                .map(Order::getAmount)
                .reduce(BigDecimal.ZERO, BigDecimal::add);
    }

    public Map<String, List<Order>> getOrdersByUser(List<Order> orders) {
        return orders.stream()
                .collect(Collectors.groupingBy(Order::getUserName));
    }
}
```

---

## 3. RestController для заметок

```java
@RestController
@RequestMapping("/notes")
public class NoteController {

    private final List<NoteResponse> notes = new ArrayList<>();
    private final AtomicLong idGenerator = new AtomicLong(1);

    @PostMapping
    public NoteResponse create(@RequestBody Note note) {
        NoteResponse response = new NoteResponse(
                idGenerator.getAndIncrement(),
                note.title(),
                note.text()
        );

        notes.add(response);
        return response;
    }

    @GetMapping
    public List<NoteResponse> getAll() {
        return notes;
    }

    public record NoteResponse(
            Long id,
            String title,
            String text
    ) {
    }
}
```
