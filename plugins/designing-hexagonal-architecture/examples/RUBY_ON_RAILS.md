# Ruby on Rails Examples

This document provides concrete examples of implementing Hexagonal Architecture using Ruby on Rails framework.

## Pattern 1: Port Interface Definition

In Ruby, we use modules to define port interfaces, which can be included by adapter classes.

```ruby
# app/ports/feed_service_port.rb

module FeedServicePort
  # Find feeds by author ID
  # @param author_id [Integer] Author ID
  # @return [Array<Hash>] List of feeds with id, content, and counts
  def find_by_author_id(author_id)
    raise NotImplementedError, "#{self.class} must implement find_by_author_id"
  end

  # Increment feed's comment count
  # @param feed_id [Integer] Feed ID
  def increment_comment_count(feed_id)
    raise NotImplementedError, "#{self.class} must implement increment_comment_count"
  end

  # Decrement feed's comment count
  # @param feed_id [Integer] Feed ID
  def decrement_comment_count(feed_id)
    raise NotImplementedError, "#{self.class} must implement decrement_comment_count"
  end
end
```

Alternatively, you can use a more explicit interface pattern:

```ruby
# app/ports/feed_service_port.rb

class FeedServicePort
  # Interface definition
  def find_by_author_id(author_id)
    raise NotImplementedError, "Subclasses must implement find_by_author_id"
  end

  def increment_comment_count(feed_id)
    raise NotImplementedError, "Subclasses must implement increment_comment_count"
  end

  def decrement_comment_count(feed_id)
    raise NotImplementedError, "Subclasses must implement decrement_comment_count"
  end

  # Type definition for feed data
  FeedData = Struct.new(:id, :content, :like_count, :comment_count, keyword_init: true)
end
```

## Pattern 2: Adapter Implementation

Adapters implement the port interface and connect to the actual service.

```ruby
# app/adapters/feed_service_adapter.rb

class FeedServiceAdapter < FeedServicePort
  def initialize(feed_service = FeedService.new)
    @feed_service = feed_service
  end

  # Find feeds by author ID
  # @param author_id [Integer] Author ID
  # @return [Array<FeedData>] List of feeds
  def find_by_author_id(author_id)
    feeds = @feed_service.find_by_author_id(author_id)

    # Map domain models to port DTOs
    feeds.map do |feed|
      FeedServicePort::FeedData.new(
        id: feed.id,
        content: feed.content,
        like_count: feed.like_count,
        comment_count: feed.comment_count
      )
    end
  end

  # Increment feed's comment count
  def increment_comment_count(feed_id)
    @feed_service.increment_comment_count(feed_id)
  end

  # Decrement feed's comment count
  def decrement_comment_count(feed_id)
    @feed_service.decrement_comment_count(feed_id)
  end
end
```

Using module-based interface:

```ruby
# app/adapters/feed_service_adapter.rb

class FeedServiceAdapter
  include FeedServicePort

  def initialize(feed_service = FeedService.new)
    @feed_service = feed_service
  end

  def find_by_author_id(author_id)
    feeds = @feed_service.find_by_author_id(author_id)

    feeds.map do |feed|
      {
        id: feed.id,
        content: feed.content,
        like_count: feed.like_count,
        comment_count: feed.comment_count
      }
    end
  end

  def increment_comment_count(feed_id)
    @feed_service.increment_comment_count(feed_id)
  end

  def decrement_comment_count(feed_id)
    @feed_service.decrement_comment_count(feed_id)
  end
end
```

## Pattern 3: Dependency Injection Configuration

Rails doesn't have built-in DI like NestJS, but you can use gems like `dry-container` or implement your own registry.

```ruby
# config/initializers/ports.rb

module Ports
  class Container
    @registry = {}

    class << self
      def register(name, implementation)
        @registry[name] = implementation
      end

      def resolve(name)
        @registry[name] || raise("Port #{name} not registered")
      end
    end
  end
end

# Register adapters
Ports::Container.register(:feed_service_port, FeedServiceAdapter.new)
```

Or using `dry-container` gem:

```ruby
# config/initializers/container.rb

require 'dry-container'

class ApplicationContainer
  extend Dry::Container::Mixin

  # Register services
  register(:feed_service) { FeedService.new }

  # Register adapters
  register(:feed_service_port) do
    FeedServiceAdapter.new(resolve(:feed_service))
  end
end
```

## Pattern 4: Repository Pattern (Infrastructure Layer)

Rails uses Active Record by default, but we can wrap it in repositories for better separation.

```ruby
# app/repositories/base_repository.rb

class BaseRepository
  def initialize(model_class)
    @model_class = model_class
  end

  # Find entity by ID
  # @param id [Integer] Entity ID
  # @return [ActiveRecord::Base, nil]
  def find_by_id(id)
    @model_class.find_by(id: id)
  end

  # Find multiple entities
  # @param conditions [Hash] Query conditions
  # @return [ActiveRecord::Relation]
  def find_many(conditions = {})
    @model_class.where(conditions)
  end

  # Create new entity
  # @param attributes [Hash] Entity attributes
  # @return [ActiveRecord::Base]
  def create(attributes)
    @model_class.create!(attributes)
  end

  # Update existing entity
  # @param id [Integer] Entity ID
  # @param attributes [Hash] Updated attributes
  # @return [ActiveRecord::Base]
  def update(id, attributes)
    entity = find_by_id(id)
    entity.update!(attributes)
    entity
  end

  # Delete entity
  # @param id [Integer] Entity ID
  # @return [Boolean]
  def delete(id)
    entity = find_by_id(id)
    entity.destroy
  end
end
```

```ruby
# app/repositories/feed_repository.rb

class FeedRepository < BaseRepository
  def initialize
    super(Feed)
  end

  # Find feeds by author ID
  # @param author_id [Integer] Author ID
  # @return [ActiveRecord::Relation]
  def find_by_author_id(author_id)
    find_many(author_id: author_id, deleted_at: nil).order(created_at: :desc)
  end

  # Find feeds with photos
  # @param author_id [Integer] Author ID
  # @return [ActiveRecord::Relation]
  def find_by_author_id_with_photos(author_id)
    @model_class
      .includes(:photos)
      .where(author_id: author_id, deleted_at: nil)
      .order(created_at: :desc)
  end
end
```

## Pattern 5: Layered Architecture Example

This example shows how different layers interact in Rails.

```ruby
# === PRESENTATION LAYER ===
# app/controllers/feeds_controller.rb

class FeedsController < ApplicationController
  before_action :authenticate_user!

  def initialize
    super
    @feed_service = FeedService.new
  end

  # GET /feeds
  def index
    # Delegate to application layer
    result = @feed_service.find_by_author_id(current_user.id)

    render json: {
      feeds: result[:feeds],
      total: result[:total]
    }
  end
end
```

```ruby
# === APPLICATION LAYER ===
# app/services/feed_service.rb

class FeedService
  def initialize(feed_repository = FeedRepository.new)
    @feed_repository = feed_repository
  end

  # Find feeds by author ID
  # Business logic: Only return non-deleted feeds, sorted by creation date
  # @param author_id [Integer] Author ID
  # @return [Hash] Feeds and total count
  def find_by_author_id(author_id)
    feeds = @feed_repository.find_by_author_id(author_id)

    {
      feeds: feeds.map { |feed| serialize_feed(feed) },
      total: feeds.count
    }
  end

  # Increment feed's comment count
  def increment_comment_count(feed_id)
    feed = @feed_repository.find_by_id(feed_id)
    feed.comment_count += 1
    @feed_repository.update(feed_id, comment_count: feed.comment_count)
  end

  # Decrement feed's comment count
  def decrement_comment_count(feed_id)
    feed = @feed_repository.find_by_id(feed_id)
    feed.comment_count = [feed.comment_count - 1, 0].max
    @feed_repository.update(feed_id, comment_count: feed.comment_count)
  end

  private

  def serialize_feed(feed)
    {
      id: feed.id,
      content: feed.content,
      like_count: feed.like_count,
      comment_count: feed.comment_count,
      view_count: feed.view_count,
      created_at: feed.created_at
    }
  end
end
```

```ruby
# === DOMAIN LAYER ===
# app/models/feed.rb

class Feed < ApplicationRecord
  # Associations
  belongs_to :author, class_name: 'User'
  has_many :photos, dependent: :destroy
  has_many :comments, dependent: :destroy

  # Validations
  validates :content, presence: true
  validates :author_id, presence: true

  # Domain logic
  def increase_view_count
    increment!(:view_count)
  end

  def add_like
    increment!(:like_count)
  end

  def remove_like
    decrement!(:like_count) if like_count > 0
  end

  def add_comment
    increment!(:comment_count)
  end

  def remove_comment
    decrement!(:comment_count) if comment_count > 0
  end

  def soft_delete
    update(deleted_at: Time.current)
  end
end
```

```ruby
# === INFRASTRUCTURE LAYER ===
# Already shown in Pattern 4 (FeedRepository)
```

## Using Port in Another Module

This example shows how the Comment module uses the Feed port.

```ruby
# app/controllers/comments_controller.rb

class CommentsController < ApplicationController
  before_action :authenticate_user!

  def initialize
    super
    # Inject via port interface
    @feed_port = Ports::Container.resolve(:feed_service_port)
    @comment_service = CommentService.new
  end

  # GET /comments/feed/:feed_id
  def index
    # Use port to verify feed exists
    # Comment module doesn't depend on Feed module directly
    feeds = @feed_port.find_by_author_id(current_user.id)
    feed_exists = feeds.any? { |f| f.id == params[:feed_id].to_i }

    raise 'Feed not found' unless feed_exists

    comments = @comment_service.find_by_feed_id(params[:feed_id])

    render json: { comments: comments }
  end

  # POST /comments
  def create
    comment = @comment_service.create(comment_params, current_user.id)

    # Increment feed's comment count through port
    @feed_port.increment_comment_count(comment.feed_id)

    render json: { comment: comment }
  end

  # DELETE /comments/:id
  def destroy
    comment = @comment_service.find_by_id(params[:id])

    @comment_service.delete(params[:id], current_user.id)

    # Decrement feed's comment count through port
    @feed_port.decrement_comment_count(comment.feed_id)

    render json: { success: true }
  end

  private

  def comment_params
    params.require(:comment).permit(:feed_id, :content)
  end
end
```

Using dry-auto_inject:

```ruby
# app/controllers/comments_controller.rb

class CommentsController < ApplicationController
  include Dry::AutoInject(ApplicationContainer)

  before_action :authenticate_user!

  # Auto-inject dependencies
  inject[:feed_service_port]

  def create
    comment = CommentService.create(comment_params, current_user.id)

    # Increment feed's comment count through port
    feed_service_port.increment_comment_count(comment.feed_id)

    render json: { comment: comment }
  end
end
```

## Testing with Port Mocks

```ruby
# spec/controllers/comments_controller_spec.rb

require 'rails_helper'

RSpec.describe CommentsController, type: :controller do
  let(:user) { create(:user) }
  let(:mock_feed_port) { instance_double(FeedServiceAdapter) }
  let(:mock_comment_service) { instance_double(CommentService) }

  before do
    sign_in user
    allow(Ports::Container).to receive(:resolve)
      .with(:feed_service_port)
      .and_return(mock_feed_port)
  end

  describe 'GET #index' do
    it 'returns comments for feed via port' do
      feed_id = 1
      expected_feeds = [
        { id: feed_id, content: 'Feed content' }
      ]
      expected_comments = [
        { id: 1, content: 'Comment 1' },
        { id: 2, content: 'Comment 2' }
      ]

      expect(mock_feed_port).to receive(:find_by_author_id)
        .with(user.id)
        .and_return(expected_feeds)

      allow_any_instance_of(CommentService).to receive(:find_by_feed_id)
        .with(feed_id.to_s)
        .and_return(expected_comments)

      get :index, params: { feed_id: feed_id }

      expect(response).to have_http_status(:success)
      expect(JSON.parse(response.body)['comments']).to eq(expected_comments.map(&:stringify_keys))
    end
  end

  describe 'POST #create' do
    it 'increments feed comment count via port' do
      feed_id = 1
      comment = double(:comment, feed_id: feed_id)

      allow_any_instance_of(CommentService).to receive(:create).and_return(comment)
      expect(mock_feed_port).to receive(:increment_comment_count).with(feed_id)

      post :create, params: { comment: { feed_id: feed_id, content: 'New comment' } }

      expect(response).to have_http_status(:success)
    end
  end
end
```

## Benefits Demonstrated

1. **Dependency Inversion**: Controllers depend on ports (abstractions), not concrete services

2. **Testability**: Easy to mock ports in tests without complex setup

3. **Flexibility**: Can swap implementations by changing port registrations

4. **Layer Separation**: Clear boundaries using Services, Repositories, and Models

5. **Rails Integration**: Hexagonal architecture works alongside Rails conventions

## Rails-Specific Considerations

- **Active Record**: Wrap in repositories for better abstraction
- **Service Objects**: Use for application layer business logic
- **Concerns**: Can be used for shared port interfaces
- **Initializers**: Good place for port registrations
- **Gems**: Consider `dry-container`, `dry-auto_inject` for DI
