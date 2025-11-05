# Kotlin Spring Examples

This document provides concrete examples of implementing Hexagonal Architecture using Kotlin and Spring Framework.

## Pattern 1: Port Interface Definition

In Kotlin, we use interfaces to define ports that establish contracts between domain and infrastructure.

```kotlin
// src/main/kotlin/com/example/common/port/FeedPort.kt

package com.example.common.port

import com.example.common.entity.Feed

/**
 * Feed service port interface
 * Other domains access Feed domain features through this interface
 */
interface FeedPort {

    /**
     * Find feed by ID
     * @param id Feed ID
     * @return Feed entity
     */
    fun findById(id: Long): Feed

    /**
     * Check if feed exists
     * @param id Feed ID
     * @return true if exists
     */
    fun existsById(id: Long): Boolean

    /**
     * Increment feed's comment count
     * @param feedId Feed ID
     */
    fun incrementCommentCount(feedId: Long)

    /**
     * Decrement feed's comment count
     * @param feedId Feed ID
     */
    fun decrementCommentCount(feedId: Long)

    /**
     * Increment feed's like count
     * @param feedId Feed ID
     */
    fun incrementLikeCount(feedId: Long)

    /**
     * Decrement feed's like count
     * @param feedId Feed ID
     */
    fun decrementLikeCount(feedId: Long)

    /**
     * Save feed
     * @param feed Feed entity
     * @return Saved feed
     */
    fun save(feed: Feed): Feed
}
```

Simple port example:

```kotlin
// src/main/kotlin/com/example/common/port/UserPort.kt

package com.example.common.port

import com.example.common.entity.User

/**
 * User service port interface
 * Other domains access User domain features through this interface
 */
interface UserPort {

    /**
     * Find user by ID
     * @param id User ID
     * @return User entity
     */
    fun findById(id: Long): User

    /**
     * Check if user exists
     * @param id User ID
     * @return true if exists
     */
    fun existsById(id: Long): Boolean
}
```

## Pattern 2: Adapter Implementation

Adapters implement port interfaces and connect domain logic to infrastructure.

```kotlin
// src/main/kotlin/com/example/feed/adapter/FeedAdapter.kt

package com.example.feed.adapter

import com.example.common.entity.Feed
import com.example.common.port.FeedPort
import com.example.feed.repository.FeedRepository
import org.springframework.stereotype.Component
import org.springframework.transaction.annotation.Transactional

/**
 * Feed service adapter
 * Implements FeedPort interface to provide Feed features to other domains
 */
@Component
@Transactional
class FeedAdapter(
    private val feedRepository: FeedRepository
) : FeedPort {

    /**
     * Find feed by ID
     * @param id Feed ID
     * @return Feed entity
     */
    override fun findById(id: Long): Feed {
        return feedRepository.findByIdAndDeletedAtIsNull(id)
            ?: throw IllegalArgumentException("Feed not found: $id")
    }

    /**
     * Check if feed exists
     * @param id Feed ID
     * @return true if exists
     */
    override fun existsById(id: Long): Boolean {
        return feedRepository.findByIdAndDeletedAtIsNull(id) != null
    }

    /**
     * Increment feed's comment count
     */
    override fun incrementCommentCount(feedId: Long) {
        val feed = findById(feedId)
        feed.addComment()
        feedRepository.save(feed)
    }

    /**
     * Decrement feed's comment count
     */
    override fun decrementCommentCount(feedId: Long) {
        val feed = findById(feedId)
        feed.removeComment()
        feedRepository.save(feed)
    }

    /**
     * Increment feed's like count
     */
    override fun incrementLikeCount(feedId: Long) {
        val feed = findById(feedId)
        feed.addLike()
        feedRepository.save(feed)
    }

    /**
     * Decrement feed's like count
     */
    override fun decrementLikeCount(feedId: Long) {
        val feed = findById(feedId)
        feed.removeLike()
        feedRepository.save(feed)
    }

    /**
     * Save feed
     */
    override fun save(feed: Feed): Feed {
        return feedRepository.save(feed)
    }
}
```

```kotlin
// src/main/kotlin/com/example/user/adapter/UserAdapter.kt

package com.example.user.adapter

import com.example.common.entity.User
import com.example.common.port.UserPort
import com.example.user.repository.UserRepository
import org.springframework.stereotype.Component

/**
 * User service adapter
 * Implements UserPort interface to provide User features to other domains
 */
@Component
class UserAdapter(
    private val userRepository: UserRepository
) : UserPort {

    /**
     * Find user by ID
     */
    override fun findById(id: Long): User {
        return userRepository.findById(id).orElseThrow {
            IllegalArgumentException("User not found: $id")
        }
    }

    /**
     * Check if user exists
     */
    override fun existsById(id: Long): Boolean {
        return userRepository.existsById(id)
    }
}
```

## Pattern 3: Module Configuration with Dependency Injection

Kotlin Spring uses constructor-based dependency injection with `@Component` annotation.

```kotlin
// src/main/kotlin/com/example/feed/FeedConfig.kt

package com.example.feed

import com.example.common.port.FeedPort
import com.example.feed.adapter.FeedAdapter
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration

@Configuration
class FeedConfig {

    /**
     * Expose FeedPort for other modules
     * This allows dependency injection by interface
     */
    @Bean
    fun feedPort(feedAdapter: FeedAdapter): FeedPort {
        return feedAdapter
    }
}
```

Using component scanning (simpler approach):

```kotlin
// src/main/kotlin/com/example/feed/FeedModule.kt

package com.example.feed

import org.springframework.context.annotation.ComponentScan
import org.springframework.context.annotation.Configuration

@Configuration
@ComponentScan(basePackages = ["com.example.feed"])
class FeedModule {
    // Components are auto-discovered and registered
    // @Component annotation handles registration
}
```

## Pattern 4: Repository Pattern (Infrastructure Layer)

Spring Data JPA provides repository abstraction for Kotlin.

```kotlin
// src/main/kotlin/com/example/common/repository/BaseRepository.kt

package com.example.common.repository

import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.data.repository.NoRepositoryBean
import java.util.*

/**
 * Base repository interface
 * Provides common CRUD operations for all repositories
 */
@NoRepositoryBean
interface BaseRepository<T, ID> : JpaRepository<T, ID> {

    /**
     * Find entity by ID
     */
    override fun findById(id: ID): Optional<T>

    /**
     * Find all entities
     */
    override fun findAll(): List<T>

    /**
     * Save entity
     */
    override fun <S : T> save(entity: S): S

    /**
     * Delete entity by ID
     */
    override fun deleteById(id: ID)

    /**
     * Check if entity exists
     */
    override fun existsById(id: ID): Boolean
}
```

```kotlin
// src/main/kotlin/com/example/feed/repository/FeedRepository.kt

package com.example.feed.repository

import com.example.common.entity.Feed
import org.springframework.data.jpa.repository.JpaRepository
import org.springframework.data.jpa.repository.Query
import org.springframework.data.repository.query.Param
import org.springframework.stereotype.Repository

/**
 * Feed repository
 * Handles all database operations for Feed entities
 */
@Repository
interface FeedRepository : JpaRepository<Feed, Long> {

    /**
     * Find feed by ID excluding soft-deleted records
     * @param id Feed ID
     * @return Feed entity or null
     */
    fun findByIdAndDeletedAtIsNull(id: Long): Feed?

    /**
     * Find feeds by author ID
     * @param authorId Author ID
     * @return List of feeds ordered by creation date
     */
    @Query("SELECT f FROM Feed f WHERE f.author.id = :authorId AND f.deletedAt IS NULL ORDER BY f.createdAt DESC")
    fun findByAuthorId(@Param("authorId") authorId: Long): List<Feed>

    /**
     * Find feeds by author ID with photos (fetch join)
     */
    @Query("SELECT DISTINCT f FROM Feed f LEFT JOIN FETCH f.photos WHERE f.author.id = :authorId AND f.deletedAt IS NULL")
    fun findByAuthorIdWithPhotos(@Param("authorId") authorId: Long): List<Feed>
}
```

## Pattern 5: Layered Architecture Example

This example shows how different layers interact in Kotlin Spring Boot.

```kotlin
// === PRESENTATION LAYER ===
// src/main/kotlin/com/example/feed/controller/FeedController.kt

package com.example.feed.controller

import com.example.common.security.CurrentUser
import com.example.feed.dto.FeedListResponse
import com.example.feed.service.FeedService
import com.example.common.entity.User
import org.springframework.http.ResponseEntity
import org.springframework.security.access.prepost.PreAuthorize
import org.springframework.web.bind.annotation.*

/**
 * Feed REST controller
 * Handles HTTP requests for feed management
 */
@RestController
@RequestMapping("/api/feeds")
@PreAuthorize("isAuthenticated()")
class FeedController(
    private val feedService: FeedService
) {

    /**
     * Get all feeds for current user
     */
    @GetMapping
    fun findAll(@CurrentUser user: User): ResponseEntity<FeedListResponse> {
        val response = feedService.findByAuthorId(user.id!!)
        return ResponseEntity.ok(response)
    }
}
```

```kotlin
// === APPLICATION LAYER ===
// src/main/kotlin/com/example/feed/service/FeedService.kt

package com.example.feed.service

import com.example.feed.repository.FeedRepository
import com.example.feed.dto.FeedDto
import com.example.feed.dto.FeedListResponse
import com.example.common.port.UserPort
import org.springframework.stereotype.Service
import org.springframework.transaction.annotation.Transactional

/**
 * Feed service
 * Contains business logic for feed management
 */
@Service
@Transactional(readOnly = true)
class FeedService(
    private val feedRepository: FeedRepository,
    private val userPort: UserPort
) {

    /**
     * Find feeds by author ID
     * Business logic: Only return non-deleted feeds, sorted by creation date
     */
    fun findByAuthorId(authorId: Long): FeedListResponse {
        // Verify author exists through port
        userPort.existsById(authorId)

        val feeds = feedRepository.findByAuthorId(authorId)

        val feedDtos = feeds.map { feed ->
            FeedDto(
                id = feed.id!!,
                content = feed.content,
                likeCount = feed.likeCount,
                commentCount = feed.commentCount,
                viewCount = feed.viewCount,
                createdAt = feed.createdAt
            )
        }

        return FeedListResponse(feedDtos, feedDtos.size)
    }
}
```

```kotlin
// === DOMAIN LAYER ===
// src/main/kotlin/com/example/common/entity/Feed.kt

package com.example.common.entity

import com.example.common.constant.VISIBILITY
import jakarta.persistence.*
import org.hibernate.annotations.Comment

/**
 * Feed domain entity
 * Contains business rules and domain logic
 */
@Entity
@Table(name = "feeds")
class Feed() : BaseEntity() {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long? = null

    @Column(length = 2000)
    @Comment("Feed content")
    var content: String? = null

    @Column
    @Comment("Like count")
    var likeCount: Int = 0

    @Column
    @Comment("Comment count")
    var commentCount: Int = 0

    @Column
    @Comment("View count")
    var viewCount: Int = 0

    @Enumerated(EnumType.STRING)
    @Column
    @Comment("Visibility")
    var visibility: VISIBILITY = VISIBILITY.PUBLIC

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "author_id", nullable = false)
    @Comment("Author")
    var author: User? = null

    @OneToMany(mappedBy = "feed", cascade = [CascadeType.ALL], orphanRemoval = true)
    @OrderBy("position ASC")
    var photos: MutableList<Photo> = mutableListOf()

    // Domain logic
    fun increaseViewCount() {
        this.viewCount++
    }

    fun addLike() {
        this.likeCount++
    }

    fun removeLike() {
        if (this.likeCount > 0) {
            this.likeCount--
        }
    }

    fun addComment() {
        this.commentCount++
    }

    fun removeComment() {
        if (this.commentCount > 0) {
            this.commentCount--
        }
    }

    fun update(content: String?, visibility: VISIBILITY) {
        this.content = content
        this.visibility = visibility
    }
}
```

```kotlin
// === INFRASTRUCTURE LAYER ===
// Already shown in Pattern 4 (FeedRepository)
```

## Using Port in Another Module

This example shows how the Comment module uses the Feed port.

```kotlin
// src/main/kotlin/com/example/comment/controller/CommentController.kt

package com.example.comment.controller

import com.example.common.port.FeedPort
import com.example.common.security.CurrentUser
import com.example.common.entity.User
import com.example.comment.service.CommentService
import org.springframework.http.ResponseEntity
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/api/comments")
class CommentController(
    private val commentService: CommentService,
    private val feedPort: FeedPort  // Inject via port interface
) {

    @GetMapping("/feed/{feedId}")
    fun getCommentsForFeed(
        @PathVariable feedId: Long,
        @CurrentUser user: User
    ): ResponseEntity<Map<String, Any>> {

        // Use port to verify feed exists
        // Comment module doesn't depend on Feed module directly
        if (!feedPort.existsById(feedId)) {
            throw IllegalArgumentException("Feed not found")
        }

        val comments = commentService.findByFeedId(feedId)

        return ResponseEntity.ok(mapOf("comments" to comments))
    }

    @PostMapping
    fun createComment(
        @RequestBody request: CreateCommentRequest,
        @CurrentUser user: User
    ): ResponseEntity<CommentResponse> {
        val comment = commentService.create(request, user.id!!)

        // Increment feed's comment count through port
        feedPort.incrementCommentCount(request.feedId)

        return ResponseEntity.ok(CommentResponse.from(comment))
    }

    @DeleteMapping("/{id}")
    fun deleteComment(
        @PathVariable id: Long,
        @CurrentUser user: User
    ): ResponseEntity<Void> {
        val comment = commentService.findById(id)

        commentService.delete(id, user.id!!)

        // Decrement feed's comment count through port
        feedPort.decrementCommentCount(comment.feedId)

        return ResponseEntity.noContent().build()
    }
}
```

## Testing with Port Mocks

```kotlin
// src/test/kotlin/com/example/comment/controller/CommentControllerTest.kt

package com.example.comment.controller

import com.example.common.port.FeedPort
import com.example.common.entity.User
import com.example.comment.service.CommentService
import com.ninjasquad.springmockk.MockkBean
import io.mockk.every
import io.mockk.verify
import org.junit.jupiter.api.Test
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest
import org.springframework.security.test.context.support.WithMockUser
import org.springframework.test.web.servlet.MockMvc
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get
import org.springframework.test.web.servlet.result.MockMvcResultMatchers.*

@WebMvcTest(CommentController::class)
class CommentControllerTest {

    @Autowired
    private lateinit var mockMvc: MockMvc

    @MockkBean
    private lateinit var feedPort: FeedPort

    @MockkBean
    private lateinit var commentService: CommentService

    @Test
    @WithMockUser
    fun `should return comments for feed`() {
        // Given
        val feedId = 1L
        every { feedPort.existsById(feedId) } returns true
        every { commentService.findByFeedId(feedId) } returns emptyList()

        // When & Then
        mockMvc.perform(get("/api/comments/feed/$feedId"))
            .andExpect(status().isOk)
            .andExpect(jsonPath("$.comments").isArray)

        verify { feedPort.existsById(feedId) }
    }

    @Test
    @WithMockUser
    fun `should throw exception when feed not found`() {
        // Given
        val feedId = 999L
        every { feedPort.existsById(feedId) } returns false

        // When & Then
        mockMvc.perform(get("/api/comments/feed/$feedId"))
            .andExpect(status().isBadRequest)

        verify { feedPort.existsById(feedId) }
    }
}
```

## Benefits Demonstrated

1. **Dependency Inversion**: Controllers depend on `FeedPort` (interface), not concrete implementations

2. **Testability**: Easy to mock ports using MockK without complex setup

3. **Flexibility**: Can swap implementations by changing Spring bean configuration

4. **Layer Separation**: Clear boundaries using `@Controller`, `@Service`, `@Repository`

5. **Kotlin Integration**: Hexagonal architecture works seamlessly with Kotlin's language features (data classes, null safety, etc.)

## Kotlin-Specific Considerations

- **Null Safety**: Leverage Kotlin's null safety features in port definitions and implementations
- **Data Classes**: Use data classes for DTOs and simple value objects
- **Extension Functions**: Can be used to add port-specific behavior without modifying interfaces
- **Coroutines**: For asynchronous operations, ports can return `suspend` functions
- **Smart Casts**: Kotlin's type system reduces boilerplate in adapter implementations
- **Constructor Injection**: Primary constructor injection is idiomatic in Kotlin Spring
