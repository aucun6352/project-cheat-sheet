# TypeScript/NestJS Examples

This document provides concrete examples of implementing Hexagonal Architecture using TypeScript and NestJS framework.

## Pattern 1: Port Interface Definition

Ports define the contract between the domain and external systems. In TypeScript, we use interfaces to declare these contracts.

```typescript
// src/modules/common/ports/feed/IFeedService.port.ts

/**
 * Feed service port interface
 * Other domains access Feed domain features through this interface
 */
export interface IFeedServicePort {
  /**
   * Find feeds by author ID
   * @param authorId Author ID
   * @returns List of feeds
   */
  findByAuthorId(authorId: bigint): Promise<FeedData[]>;

  /**
   * Increment feed's comment count
   * @param feedId Feed ID
   */
  incrementCommentCount(feedId: bigint): Promise<void>;

  /**
   * Decrement feed's comment count
   * @param feedId Feed ID
   */
  decrementCommentCount(feedId: bigint): Promise<void>;
}

// Data transfer object for the port
export interface FeedData {
  id: bigint;
  content: string;
  likeCount: number;
  commentCount: number;
}
```

## Pattern 2: Adapter Implementation

Adapters implement the port interfaces and connect the domain to the infrastructure.

```typescript
// src/modules/feed/adapters/feed.service.adapter.ts

import { Injectable } from '@nestjs/common';
import { IFeedServicePort, FeedData } from '../../common/ports/feed/IFeedService.port';
import { FeedService } from '../feed.service';

/**
 * Feed service adapter
 * Implements IFeedServicePort interface to provide Feed features to other domains
 */
@Injectable()
export class FeedServiceAdapter implements IFeedServicePort {
  constructor(private readonly feedService: FeedService) {}

  /**
   * Find feeds by author ID
   * @param authorId Author ID
   * @returns List of feeds (only ID, content, and counts)
   */
  async findByAuthorId(authorId: bigint): Promise<FeedData[]> {
    const feeds = await this.feedService.findByAuthorId(authorId);

    // Map domain entities to port DTOs
    return feeds.map((feed) => ({
      id: feed.id,
      content: feed.content,
      likeCount: feed.likeCount,
      commentCount: feed.commentCount,
    }));
  }

  /**
   * Increment feed's comment count
   * @param feedId Feed ID
   */
  async incrementCommentCount(feedId: bigint): Promise<void> {
    await this.feedService.incrementCommentCount(feedId);
  }

  /**
   * Decrement feed's comment count
   * @param feedId Feed ID
   */
  async decrementCommentCount(feedId: bigint): Promise<void> {
    await this.feedService.decrementCommentCount(feedId);
  }
}
```

## Pattern 3: Module Configuration with Dependency Injection

NestJS modules wire together ports and adapters using dependency injection.

```typescript
// src/modules/feed/feed.module.ts

import { Module, forwardRef } from '@nestjs/common';
import { FeedController } from './feed.controller';
import { FeedService } from './feed.service';
import { FeedRepository } from './repositories/feed.repository';
import { FeedServiceAdapter } from './adapters/feed.service.adapter';
import { CommentModule } from '../comment/comment.module';

@Module({
  imports: [
    forwardRef(() => CommentModule), // Resolve circular dependencies
  ],
  controllers: [FeedController],
  providers: [
    FeedService,
    FeedRepository,
    FeedServiceAdapter,
    {
      provide: 'IFeedServicePort', // Register port with DI container
      useClass: FeedServiceAdapter,
    },
  ],
  exports: [
    FeedService,
    'IFeedServicePort', // Export port for other modules
  ],
})
export class FeedModule {}
```

## Pattern 4: Repository Pattern (Infrastructure Layer)

Repositories abstract data access, isolating the domain from ORM details.

```typescript
// src/modules/common/repositories/base.repository.ts

import { PrismaService } from '../prisma.service';

/**
 * Base repository providing common CRUD operations
 * Wraps Prisma ORM for better testability and abstraction
 */
export abstract class BaseRepository<T> {
  constructor(
    protected readonly prisma: PrismaService,
    protected readonly modelName: string,
  ) {}

  /**
   * Find entity by ID
   */
  async findById(id: bigint): Promise<T | null> {
    return this.getModel().findUnique({ where: { id } });
  }

  /**
   * Find multiple entities
   */
  async findMany(args?: any): Promise<T[]> {
    return this.getModel().findMany(args);
  }

  /**
   * Create new entity
   */
  async create(data: any): Promise<T> {
    return this.getModel().create({ data });
  }

  /**
   * Update existing entity
   */
  async update(id: bigint, data: any): Promise<T> {
    return this.getModel().update({
      where: { id },
      data,
    });
  }

  /**
   * Delete entity
   */
  async delete(id: bigint): Promise<T> {
    return this.getModel().delete({ where: { id } });
  }

  /**
   * Get Prisma model delegate
   */
  protected getModel() {
    return this.prisma[this.modelName];
  }
}
```

```typescript
// src/modules/feed/repositories/feed.repository.ts

import { Injectable } from '@nestjs/common';
import { BaseRepository } from '../../common/repositories/base.repository';
import { PrismaService } from '../../common/prisma.service';
import { Feed } from '@prisma/client';

/**
 * Feed repository
 * Handles all database operations for Feed entities
 */
@Injectable()
export class FeedRepository extends BaseRepository<Feed> {
  constructor(prisma: PrismaService) {
    super(prisma, 'feed');
  }

  /**
   * Find feeds by author ID
   */
  async findByAuthorId(authorId: bigint): Promise<Feed[]> {
    return this.findMany({
      where: { authorId, deletedAt: null },
      orderBy: { createdAt: 'desc' },
    });
  }

  /**
   * Find feeds by author ID with photos (eager loading)
   */
  async findByAuthorIdWithPhotos(authorId: bigint): Promise<Feed[]> {
    return this.findMany({
      where: { authorId, deletedAt: null },
      include: { photos: true },
      orderBy: { createdAt: 'desc' },
    });
  }
}
```

## Pattern 5: Layered Architecture Example

This example shows how different layers interact in a complete request flow.

```typescript
// === PRESENTATION LAYER ===
// src/modules/feed/feed.controller.ts

import { Controller, Get, UseGuards } from '@nestjs/common';
import { Auth } from '../common/decorators/auth.decorator';
import { GetUser } from '../common/decorators/user.decorator';
import { FeedService } from './feed.service';
import { FeedListResponseDto } from './dto/feed-list-response.dto';
import { User } from '@prisma/client';

@Controller('feeds')
@Auth() // JWT authentication guard
export class FeedController {
  constructor(private readonly feedService: FeedService) {}

  @Get()
  async findAll(
    @GetUser() user: User, // Inject authenticated user
  ): Promise<FeedListResponseDto> {
    return this.feedService.findByAuthorId(user.id);
  }
}
```

```typescript
// === APPLICATION LAYER ===
// src/modules/feed/feed.service.ts

import { Injectable } from '@nestjs/common';
import { FeedRepository } from './repositories/feed.repository';
import { FeedListResponseDto } from './dto/feed-list-response.dto';

/**
 * Feed service
 * Contains business logic for feed management
 */
@Injectable()
export class FeedService {
  constructor(private readonly feedRepository: FeedRepository) {}

  /**
   * Find feeds by author ID
   * Business logic: Only return non-deleted feeds, sorted by creation date
   */
  async findByAuthorId(authorId: bigint): Promise<FeedListResponseDto> {
    const feeds = await this.feedRepository.findByAuthorId(authorId);

    return {
      feeds: feeds.map(feed => ({
        id: feed.id.toString(), // Convert BigInt to string for API
        content: feed.content,
        likeCount: feed.likeCount,
        commentCount: feed.commentCount,
        viewCount: feed.viewCount,
        createdAt: feed.createdAt,
      })),
      total: feeds.length,
    };
  }

  /**
   * Increment feed's comment count
   */
  async incrementCommentCount(feedId: bigint): Promise<void> {
    const feed = await this.feedRepository.findById(feedId);
    if (!feed) {
      throw new Error('Feed not found');
    }

    feed.commentCount += 1;
    await this.feedRepository.save(feed);
  }

  /**
   * Decrement feed's comment count
   */
  async decrementCommentCount(feedId: bigint): Promise<void> {
    const feed = await this.feedRepository.findById(feedId);
    if (!feed) {
      throw new Error('Feed not found');
    }

    if (feed.commentCount > 0) {
      feed.commentCount -= 1;
      await this.feedRepository.save(feed);
    }
  }
}
```

```typescript
// === INFRASTRUCTURE LAYER ===
// Already shown in Pattern 4 (FeedRepository)
```

## Using Port in Another Module

This example shows how the Comment module uses the Feed port to access feed functionality.

```typescript
// src/modules/comment/comment.controller.ts

import { Controller, Get, Post, Delete, Body, Param, Inject } from '@nestjs/common';
import { Auth } from '../common/decorators/auth.decorator';
import { GetUser } from '../common/decorators/user.decorator';
import { IFeedServicePort } from '../common/ports/feed/IFeedService.port';
import { User } from '@prisma/client';
import { CommentService } from './comment.service';
import { CreateCommentDto } from './dto/create-comment.dto';

@Controller('comments')
@Auth()
export class CommentController {
  constructor(
    @Inject('IFeedServicePort') // Inject via port interface
    private readonly feedPort: IFeedServicePort,
    private readonly commentService: CommentService,
  ) {}

  @Get('feed/:feedId')
  async getCommentsForFeed(
    @Param('feedId') feedId: string,
    @GetUser() user: User
  ) {
    // Use port to verify feed exists
    // Comment module doesn't depend on Feed module directly
    const feeds = await this.feedPort.findByAuthorId(user.id);
    const feedExists = feeds.some(f => f.id.toString() === feedId);

    if (!feedExists) {
      throw new Error('Feed not found');
    }

    const comments = await this.commentService.findByFeedId(BigInt(feedId));

    return { comments };
  }

  @Post()
  async createComment(
    @Body() dto: CreateCommentDto,
    @GetUser() user: User
  ) {
    const comment = await this.commentService.create(dto, user.id);

    // Increment feed's comment count through port
    await this.feedPort.incrementCommentCount(dto.feedId);

    return { comment };
  }

  @Delete(':id')
  async deleteComment(
    @Param('id') id: string,
    @GetUser() user: User
  ) {
    const comment = await this.commentService.findById(BigInt(id));

    await this.commentService.delete(BigInt(id), user.id);

    // Decrement feed's comment count through port
    await this.feedPort.decrementCommentCount(comment.feedId);

    return { success: true };
  }
}
```

## Benefits Demonstrated

1. **Dependency Inversion**: CommentController depends on `IFeedServicePort` (abstraction), not `FeedService` (implementation)

2. **Testability**: Can easily mock `IFeedServicePort` in tests without touching the database

3. **Flexibility**: Can replace `FeedServiceAdapter` with a different implementation without changing CommentController

4. **Layer Separation**: Clear boundaries between Presentation, Application, and Infrastructure layers

5. **Module Independence**: Modules communicate through well-defined ports, making them independently deployable
