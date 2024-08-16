---
title: Todo Controller
description: 2024년 KDT 3차 프로젝트
author: hyeonyeong
date: 2024-08-15 16:16:00 +0800
categories: [Project, Do-tori]
tags: [project, do-tori, todo, feat]
pin: false
math: true
mermaid: true
---


# Spring Boot에서 Todo API 구현하기

## 할 일
Todo 항목들을 카테고리별로 그룹화하고 동적으로 처리하기

## 기본 구조
`TodoController`의 기본 구조 -> controller에서 가져오는 모든 todo들은 카테고리별로 나눠지지 않았음


```java
@RestController
@RequestMapping("/api/todos")
public class TodoController {
    private final TodoService todoService;

    @Autowired
    public TodoController(TodoService todoService) {
        this.todoService = todoService;
    }

    @GetMapping
    public ResponseEntity<List<TodoDTO>> getAllTodos() {
        try{
            return ResponseEntity.ok(todoService.getAllTodo());
        }catch (Exception e){
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(null);
        }
    }
    // 이 외 것들..
}
```

## Todo 항목 가저오는 코드 수정
모든 Todo 항목을 가져오는 API 메소드를 카테고리별로 분리

```java
@GetMapping
public ResponseEntity<Map<String, List<TodoDTO>>> getAllTodos() {
    try {
        List<TodoDTO> todos = todoService.getAllTodo();

        Map<String, List<TodoDTO>> todoCategory = todos.stream()
                .collect(Collectors.groupingBy(TodoDTO::getCategory));

        Map<String, List<TodoDTO>> sortedTodoCategory = new TreeMap<>(todoCategory);  // 다른 자료구조도 많으나, 정렬을 위해 TreeMap 사용
        
        return ResponseEntity.ok(sortedTodoCategory);
    } catch (Exception e) {
        logger.error("Error while fetching todos", e);
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(null);
    }
}
```

## 주요 포인트 설명

1. **동적 카테고리 처리**: 
   -  `Collectors.groupingBy()`를 사용하여 Todo 항목들을 카테고리별로 자동으로 그룹화.
   - 새로운 카테고리가 추가되거나 기존 카테고리가 제거되어도 코드 수정 없이 유연하게 대응 가능

2. **정렬**:
   - `TreeMap`을 사용하여 카테고리를 알파벳 순으로 정렬.
   - 정렬이 필요 없다면 `new TreeMap<>(todoCategory)` 대신 그냥 `todoCategory`를 사용할 수 있다.
   - 아쉬운 점 : 우선순위를 사용자가 원하는 대로 맞출 수 없을 것 같음 (추후 구현 사항)

3. **예외 처리**:
   - try-catch 블록을 사용하여 예외를 잡고 로깅.
   - 오류 발생 시 적절한 HTTP 상태 코드와 함께 응답 반환.

## 장점

- **유연성**: 카테고리 구조가 변경되어도 코드 수정이 필요 없다.
- **간결성**: 카테고리 순서를 명시적으로 관리할 필요가 없어 코드가 간결
- **동적 대응**: 실제 데이터에 존재하는 카테고리만 결과에 포함됨.


# api 명세서

- get mapping이라 input 없음

# Output

# Output Sample

```json
{
    "5789ddd": [
        {
            "id": 28,
            "category": "5789ddd",
            "content": "456wss",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "Schedule": [
        {
            "id": 34,
            "category": "Schedule",
            "content": "dd",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "Study": [
        {
            "id": 35,
            "category": "Study",
            "content": "ㅇㅇㅇ",
            "done": true,
            "todoDate": "2024-08-09"
        },
        {
            "id": 36,
            "category": "Study",
            "content": "4555",
            "done": false,
            "todoDate": "2024-08-21"
        }
    ],
    "dljdl": [
        {
            "id": 31,
            "category": "dljdl",
            "content": "dma~",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "난끌려가": [
        {
            "id": 29,
            "category": "난끌려가",
            "content": "title..............2",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "습관": [
        {
            "id": 33,
            "category": "습관",
            "content": "습관이 중요해요",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "아오..": [
        {
            "id": 30,
            "category": "아오..",
            "content": "이렇게 하면",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ],
    "일정": [
        {
            "id": 32,
            "category": "일정",
            "content": "54654",
            "done": false,
            "todoDate": "2024-08-03"
        }
    ]
}
```
