# Spring AI Elasticsearch Chat Memory Repository Auto Configuration

Auto-configuration for Elasticsearch-based Chat Memory Repository.

## Usage

### 1. Add Dependency

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-autoconfigure-model-chat-memory-repository-elasticsearch</artifactId>
</dependency>

<dependency>
    <groupId>co.elastic.clients</groupId>
    <artifactId>elasticsearch-java</artifactId>
</dependency>
```

### 2. Configure Elasticsearch Client

```java
@Configuration
public class ElasticsearchConfig {
    
    @Bean
    public ElasticsearchClient elasticsearchClient() {
        RestClient restClient = RestClient.builder(
            new HttpHost("localhost", 9200)
        ).build();
        
        return new ElasticsearchClient(
            new RestClientTransport(restClient, new JacksonJsonpMapper())
        );
    }
}
```

### 3. Configure Properties (Optional)

```properties
# application.properties
spring.ai.chat.memory.repository.elasticsearch.index-name=chat_memory
```

Or in `application.yml`:

```yaml
spring:
  ai:
    chat:
      memory:
        repository:
          elasticsearch:
            index-name: chat_memory
```

### 4. Use in Your Application

```java
@Service
public class ChatService {
    
    private final ChatMemoryRepository chatMemoryRepository;
    
    public ChatService(ChatMemoryRepository chatMemoryRepository) {
        this.chatMemoryRepository = chatMemoryRepository;
    }
    
    public void saveConversation(String conversationId, List<Message> messages) {
        chatMemoryRepository.saveAll(conversationId, messages);
    }
    
    public List<Message> getConversation(String conversationId) {
        return chatMemoryRepository.findByConversationId(conversationId);
    }
}
```

## Configuration Properties

| Property | Default | Description |
|----------|---------|-------------|
| `spring.ai.chat.memory.repository.elasticsearch.index-name` | `chat_memory` | Name of the Elasticsearch index to use |

## Features

- Automatic index creation with proper mappings
- Support for all message types (User, Assistant, System, Tool)
- Efficient conversation retrieval with proper ordering
- Metadata support for additional message properties
- Automatic timestamp management

## Requirements

- Spring Boot 3.x
- Elasticsearch 8.x
- Java 17+

