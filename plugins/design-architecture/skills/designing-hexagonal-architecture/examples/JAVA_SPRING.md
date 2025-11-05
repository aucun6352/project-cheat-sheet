# Java Spring Examples

This document provides concrete examples of implementing Hexagonal Architecture using Java and Spring Framework.

## Pattern 1: Port Interface Definition

In Java, we use interfaces to define ports that establish contracts between domain and infrastructure.

```java
// src/main/java/com/example/common/ports/FeedServicePort.java

package com.example.common.ports;

import java.util.List;

/**
 * Feed service port interface
 * Other domains access Feed domain features through this interface
 */
public interface FeedServicePort {

    /**
     * Find feeds by author ID
     * @param authorId Author ID
     * @return List of feed data
     */
    List<FeedData> findByAuthorId(Long authorId);

    /**
     * Increment feed's comment count
     * @param feedId Feed ID
     */
    void incrementCommentCount(Long feedId);

    /**
     * Decrement feed's comment count
     * @param feedId Feed ID
     */
    void decrementCommentCount(Long feedId);

    /**
     * Data transfer object for the port
     */
    record FeedData(Long id, String content, Integer likeCount, Integer commentCount) {}
}
```

Using traditional class instead of record (Java < 14):

```java
// src/main/java/com/example/common/ports/dto/FeedData.java

package com.example.common.ports.dto;

public class FeedData {
    private final Long id;
    private final String content;
    private final Integer likeCount;
    private final Integer commentCount;

    public FeedData(Long id, String content, Integer likeCount, Integer commentCount) {
        this.id = id;
        this.content = content;
        this.likeCount = likeCount;
        this.commentCount = commentCount;
    }

    public Long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }

    public Integer getLikeCount() {
        return likeCount;
    }

    public Integer getCommentCount() {
        return commentCount;
    }
}
```

## Pattern 2: Adapter Implementation

Adapters implement port interfaces and connect domain logic to infrastructure.

```java
// src/main/java/com/example/feed/adapters/FeedServiceAdapter.java

package com.example.feed.adapters;

import com.example.common.ports.FeedServicePort;
import com.example.feed.service.FeedService;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.stream.Collectors;

/**
 * Feed service adapter
 * Implements FeedServicePort interface to provide Feed features to other domains
 */
@Component("feedServicePort")
public class FeedServiceAdapter implements FeedServicePort {

    private final FeedService feedService;

    public FeedServiceAdapter(FeedService feedService) {
        this.feedService = feedService;
    }

    /**
     * Find feeds by author ID
     * @param authorId Author ID
     * @return List of feeds
     */
    @Override
    public List<FeedData> findByAuthorId(Long authorId) {
        return feedService.findByAuthorId(authorId)
                .stream()
                .map(feed -> new FeedData(
                    feed.getId(),
                    feed.getContent(),
                    feed.getLikeCount(),
                    feed.getCommentCount()
                ))
                .collect(Collectors.toList());
    }

    /**
     * Increment feed's comment count
     * @param feedId Feed ID
     */
    @Override
    public void incrementCommentCount(Long feedId) {
        feedService.incrementCommentCount(feedId);
    }

    /**
     * Decrement feed's comment count
     * @param feedId Feed ID
     */
    @Override
    public void decrementCommentCount(Long feedId) {
        feedService.decrementCommentCount(feedId);
    }
}
```

## Pattern 3: Module Configuration with Dependency Injection

Spring uses `@Configuration` classes and component scanning for dependency injection.

```java
// src/main/java/com/example/feed/FeedModule.java

package com.example.feed;

import com.example.common.ports.FeedServicePort;
import com.example.feed.adapters.FeedServiceAdapter;
import com.example.feed.repository.FeedRepository;
import com.example.feed.service.FeedService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeedModule {

    /**
     * Expose FeedServicePort for other modules
     * This allows dependency injection by interface
     */
    @Bean
    public FeedServicePort feedServicePort(FeedService feedService) {
        return new FeedServiceAdapter(feedService);
    }

    @Bean
    public FeedService feedService(FeedRepository feedRepository) {
        return new FeedService(feedRepository);
    }
}
```

Using component scanning (simpler approach):

```java
// src/main/java/com/example/feed/FeedModule.java

package com.example.feed;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.example.feed")
public class FeedModule {
    // Components are auto-discovered and registered
    // @Component, @Service, @Repository annotations handle registration
}
```

## Pattern 4: Repository Pattern (Infrastructure Layer)

Spring Data JPA provides repository abstraction, but we can add a custom layer.

```java
// src/main/java/com/example/common/repository/BaseRepository.java

package com.example.common.repository;

import java.util.List;
import java.util.Optional;

/**
 * Base repository interface
 * Provides common CRUD operations for all repositories
 * @param <T> Entity type
 * @param <ID> ID type
 */
public interface BaseRepository<T, ID> {

    /**
     * Find entity by ID
     */
    Optional<T> findById(ID id);

    /**
     * Find all entities
     */
    List<T> findAll();

    /**
     * Save entity
     */
    T save(T entity);

    /**
     * Delete entity by ID
     */
    void deleteById(ID id);

    /**
     * Check if entity exists
     */
    boolean existsById(ID id);
}
```

```java
// src/main/java/com/example/feed/repository/FeedRepository.java

package com.example.feed.repository;

import com.example.feed.domain.Feed;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.util.List;

/**
 * Feed repository
 * Handles all database operations for Feed entities
 */
@Repository
public interface FeedRepository extends JpaRepository<Feed, Long> {

    /**
     * Find feeds by author ID
     * @param authorId Author ID
     * @return List of feeds ordered by creation date
     */
    @Query("SELECT f FROM Feed f WHERE f.authorId = :authorId AND f.deletedAt IS NULL ORDER BY f.createdAt DESC")
    List<Feed> findByAuthorId(@Param("authorId") Long authorId);

    /**
     * Find feeds by author ID with photos (fetch join)
     */
    @Query("SELECT DISTINCT f FROM Feed f LEFT JOIN FETCH f.photos WHERE f.authorId = :authorId AND f.deletedAt IS NULL")
    List<Feed> findByAuthorIdWithPhotos(@Param("authorId") Long authorId);
}
```

Custom repository implementation for complex queries:

```java
// src/main/java/com/example/common/repository/BaseRepositoryImpl.java

package com.example.common.repository;

import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;

import java.util.List;
import java.util.Optional;

public abstract class BaseRepositoryImpl<T, ID> implements BaseRepository<T, ID> {

    @PersistenceContext
    protected EntityManager entityManager;

    protected final Class<T> entityClass;

    protected BaseRepositoryImpl(Class<T> entityClass) {
        this.entityClass = entityClass;
    }

    @Override
    public Optional<T> findById(ID id) {
        return Optional.ofNullable(entityManager.find(entityClass, id));
    }

    @Override
    public List<T> findAll() {
        return entityManager
                .createQuery("SELECT e FROM " + entityClass.getSimpleName() + " e", entityClass)
                .getResultList();
    }

    @Override
    public T save(T entity) {
        return entityManager.merge(entity);
    }

    @Override
    public void deleteById(ID id) {
        findById(id).ifPresent(entityManager::remove);
    }

    @Override
    public boolean existsById(ID id) {
        return findById(id).isPresent();
    }
}
```

## Pattern 5: Layered Architecture Example

This example shows how different layers interact in Spring Boot.

```java
// === PRESENTATION LAYER ===
// src/main/java/com/example/store/controller/StoreController.java

package com.example.store.controller;

import com.example.common.security.CurrentUser;
import com.example.store.dto.StoreListResponse;
import com.example.store.service.StoreService;
import com.example.user.domain.User;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;

/**
 * Store REST controller
 * Handles HTTP requests for store management
 */
@RestController
@RequestMapping("/api/stores")
@PreAuthorize("isAuthenticated()")
public class StoreController {

    private final StoreService storeService;

    public StoreController(StoreService storeService) {
        this.storeService = storeService;
    }

    /**
     * Get all stores for current user
     */
    @GetMapping
    public ResponseEntity<StoreListResponse> findAll(@CurrentUser User user) {
        StoreListResponse response = storeService.findByOwnerId(user.getId());
        return ResponseEntity.ok(response);
    }
}
```

```java
// === APPLICATION LAYER ===
// src/main/java/com/example/store/service/StoreService.java

package com.example.store.service;

import com.example.store.domain.Store;
import com.example.store.dto.StoreDto;
import com.example.store.dto.StoreListResponse;
import com.example.store.repository.StoreRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.stream.Collectors;

/**
 * Store service
 * Contains business logic for store management
 */
@Service
@Transactional(readOnly = true)
public class StoreService {

    private final StoreRepository storeRepository;

    public StoreService(StoreRepository storeRepository) {
        this.storeRepository = storeRepository;
    }

    /**
     * Find stores by owner ID
     * Business logic: Only return active stores, sorted by creation date
     */
    public StoreListResponse findByOwnerId(Long ownerId) {
        List<Store> stores = storeRepository.findByOwnerId(ownerId);

        List<StoreDto> storeDtos = stores.stream()
                .map(this::toDto)
                .collect(Collectors.toList());

        return new StoreListResponse(storeDtos, storeDtos.size());
    }

    private StoreDto toDto(Store store) {
        return new StoreDto(
                store.getId(),
                store.getName(),
                store.getAddress(),
                store.getCreatedAt()
        );
    }
}
```

```java
// === DOMAIN LAYER ===
// src/main/java/com/example/store/domain/Store.java

package com.example.store.domain;

import jakarta.persistence.*;
import java.math.BigDecimal;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

/**
 * Store domain entity
 * Contains business rules and domain logic
 */
@Entity
@Table(name = "stores")
public class Store {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    private String address;

    private BigDecimal budget;

    @Column(name = "owner_id", nullable = false)
    private Long ownerId;

    @OneToMany(mappedBy = "store", cascade = CascadeType.ALL)
    private List<EmployeeStore> employeeStores = new ArrayList<>();

    @Column(name = "created_at", nullable = false)
    private LocalDateTime createdAt;

    // Domain logic
    public boolean isWithinBudget(BigDecimal amount) {
        return budget == null || amount.compareTo(budget) <= 0;
    }

    public boolean hasEmployee(Long employeeId) {
        return employeeStores.stream()
                .anyMatch(es -> es.getEmployeeId().equals(employeeId));
    }

    // Getters and setters
    public Long getId() { return id; }
    public String getName() { return name; }
    public String getAddress() { return address; }
    public LocalDateTime getCreatedAt() { return createdAt; }

    // ... other getters/setters
}
```

```java
// === INFRASTRUCTURE LAYER ===
// Already shown in Pattern 4 (StoreRepository)
```

## Using Port in Another Module

This example shows how the Comment module uses the Feed port.

```java
// src/main/java/com/example/comment/controller/CommentController.java

package com.example.comment.controller;

import com.example.common.ports.FeedServicePort;
import com.example.common.ports.FeedServicePort.FeedData;
import com.example.common.security.CurrentUser;
import com.example.user.domain.User;
import com.example.comment.service.CommentService;
import com.example.comment.dto.CreateCommentRequest;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/api/comments")
public class CommentController {

    private final FeedServicePort feedServicePort;
    private final CommentService commentService;

    // Inject via port interface
    public CommentController(
            @Qualifier("feedServicePort") FeedServicePort feedServicePort,
            CommentService commentService) {
        this.feedServicePort = feedServicePort;
        this.commentService = commentService;
    }

    @GetMapping("/feed/{feedId}")
    public ResponseEntity<Map<String, Object>> getCommentsForFeed(
            @PathVariable Long feedId,
            @CurrentUser User user) {

        // Use port to verify feed exists
        // Comment module doesn't depend on Feed module directly
        List<FeedData> feeds = feedServicePort.findByAuthorId(user.getId());
        boolean feedExists = feeds.stream().anyMatch(f -> f.id().equals(feedId));

        if (!feedExists) {
            throw new IllegalArgumentException("Feed not found");
        }

        List<Comment> comments = commentService.findByFeedId(feedId);

        return ResponseEntity.ok(Map.of("comments", comments));
    }

    @PostMapping
    public ResponseEntity<Comment> createComment(
            @RequestBody CreateCommentRequest request,
            @CurrentUser User user) {

        Comment comment = commentService.create(request, user.getId());

        // Increment feed's comment count through port
        feedServicePort.incrementCommentCount(request.feedId());

        return ResponseEntity.ok(comment);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteComment(
            @PathVariable Long id,
            @CurrentUser User user) {

        Comment comment = commentService.findById(id);

        commentService.delete(id, user.getId());

        // Decrement feed's comment count through port
        feedServicePort.decrementCommentCount(comment.getFeedId());

        return ResponseEntity.noContent().build();
    }
}
```

## Testing with Port Mocks

```java
// src/test/java/com/example/employee/controller/EmployeeControllerTest.java

package com.example.employee.controller;

import com.example.common.ports.StoreServicePort;
import com.example.common.ports.StoreServicePort.StoreData;
import com.example.user.domain.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.web.servlet.MockMvc;

import java.util.List;

import static org.mockito.ArgumentMatchers.anyLong;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(EmployeeController.class)
class EmployeeControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private StoreServicePort storeServicePort;

    @Test
    @WithMockUser
    void shouldReturnAvailableStores() throws Exception {
        // Given
        List<StoreData> mockStores = List.of(
                new StoreData(1L, "Store A"),
                new StoreData(2L, "Store B")
        );

        when(storeServicePort.findByOwnerId(anyLong()))
                .thenReturn(mockStores);

        // When & Then
        mockMvc.perform(get("/api/employees/available-stores"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.stores").isArray())
                .andExpect(jsonPath("$.stores.length()").value(2))
                .andExpect(jsonPath("$.stores[0].name").value("Store A"));
    }
}
```

## Benefits Demonstrated

1. **Dependency Inversion**: Controllers depend on `StoreServicePort` (interface), not concrete implementations

2. **Testability**: Easy to mock ports using `@MockBean` without complex setup

3. **Flexibility**: Can swap implementations by changing Spring bean configuration

4. **Layer Separation**: Clear boundaries using `@Controller`, `@Service`, `@Repository`

5. **Spring Integration**: Hexagonal architecture works seamlessly with Spring's DI

## Spring-Specific Considerations

- **Component Scanning**: Use `@Component`, `@Service`, `@Repository` for auto-discovery
- **Qualifier**: Use `@Qualifier` when multiple implementations exist
- **Profiles**: Different adapters for different environments (`@Profile`)
- **Transaction Management**: `@Transactional` on service layer
- **Validation**: Bean Validation at presentation layer
- **Security**: Spring Security for authentication/authorization
