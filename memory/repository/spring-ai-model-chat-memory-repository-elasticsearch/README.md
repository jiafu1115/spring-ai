# Spring AI Elasticsearch Chat Memory Repository

An Elasticsearch-based implementation of `ChatMemoryRepository` for persisting conversation history.

## Features

- Store and retrieve chat messages by conversation ID
- Support for all message types (User, Assistant, System, Tool)
- Automatic index creation with proper mappings
- Efficient querying with conversation-based partitioning

## Usage

```java
// Create Elasticsearch client
ElasticsearchClient client = // ... configure your client

// Create configuration
ElasticSearchChatMemoryRepositoryConfig config = ElasticSearchChatMemoryRepositoryConfig.builder()
    .withClient(client)
    .withIndexName("chat_memory")  // optional, defaults to "chat_memory"
    .build();

// Create repository
ElasticSearchChatMemoryRepository repository = ElasticSearchChatMemoryRepository.create(config);

// Save messages
List<Message> messages = List.of(
    new UserMessage("Hello"),
    new AssistantMessage("Hi! How can I help you?")
);
repository.saveAll("conversation-123", messages);

// Retrieve messages
List<Message> history = repository.findByConversationId("conversation-123");

// Find all conversation IDs
List<String> conversationIds = repository.findConversationIds();

// Delete conversation
repository.deleteByConversationId("conversation-123");
```

## Dependencies

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-model-chat-memory-repository-elasticsearch</artifactId>
</dependency>

<dependency>
    <groupId>co.elastic.clients</groupId>
    <artifactId>elasticsearch-java</artifactId>
</dependency>
```


