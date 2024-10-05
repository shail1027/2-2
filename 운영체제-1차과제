#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_QUEUE_SIZE 100
#define NULL_PROCESS_ID -1

// 프로세스 구조체
typedef struct {
    int id;       // 프로세스 ID
    int repeat;   // 프로세스가 실행될 반복 횟수
} Process;

// 큐 구조체 정의
typedef struct {
    Process queue[MAX_QUEUE_SIZE];
    int front;
    int rear;
} SchedulingQueue;

// 큐 초기화
void initQueue(SchedulingQueue* q) {
    q->front = 0;
    q->rear = 0;
}

// 큐가 비었는지 확인
int isEmpty(SchedulingQueue* q) {
    return q->front == q->rear;
}

// 큐가 가득 찼는지 확인
int isFull(SchedulingQueue* q) {
    return (q->rear + 1) % MAX_QUEUE_SIZE == q->front;
}

// 프로세스 삽입
int insert(SchedulingQueue* q, int id, int repeat) {
    if (isFull(q)) {
        printf("Queue is full!\n");
        return 0;
    }
    q->queue[q->rear].id = id;
    q->queue[q->rear].repeat = repeat;
    q->rear = (q->rear + 1) % MAX_QUEUE_SIZE;
    return 1;
}

// 특정 ID를 가진 프로세스 삭제 후 리턴
Process deleteProcessById(SchedulingQueue* q, int id) {
    Process p;
    p.id = NULL_PROCESS_ID;  // 기본값으로 NULL ID 설정

    if (isEmpty(q)) {
        printf("Queue is empty!\n");
        return p;
    }

    int found = 0;
    int index = q->front;

    // 큐에서 해당 ID의 프로세스를 찾음
    while (index != q->rear) {
        if (q->queue[index].id == id) {
            p = q->queue[index];  // 프로세스를 찾음
            found = 1;
            break;
        }
        index = (index + 1) % MAX_QUEUE_SIZE;
    }

    if (found) {
        // 해당 프로세스 삭제 및 큐 재정렬
        while (index != q->rear) {
            int next = (index + 1) % MAX_QUEUE_SIZE;
            q->queue[index] = q->queue[next];
            index = next;
        }
        q->rear = (q->rear - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
    }

    return p;
}

// 큐의 상태를 출력
void printQueue(SchedulingQueue* q) {
    printf("Queue state: [ ");
    int i = q->front;
    while (i != q->rear) {
        printf("{ID: %d, Repeat: %d} ", q->queue[i].id, q->queue[i].repeat);
        i = (i + 1) % MAX_QUEUE_SIZE;
    }
    printf("]\n");
}

int main() {
    SchedulingQueue queue;
    initQueue(&queue);

    // 1부터 10까지의 프로세스를 큐에 삽입
    for (int i = 1; i <= 10; i++) {
        int repeat = rand() % 5 + 1;  // 각 프로세스의 반복 횟수는 1~5 사이로 랜덤 설정
        insert(&queue, i, repeat);
    }

    // 큐의 상태 출력
    printQueue(&queue);

    srand(time(NULL));  // 랜덤 시드 설정

    // 큐가 비워질 때까지 반복
    while (!isEmpty(&queue)) {
        // 큐에서 랜덤 프로세스를 선택
        int random_index = rand() % (queue.rear - queue.front) + queue.front;
        int random_id = queue.queue[random_index % MAX_QUEUE_SIZE].id;

        // 선택한 프로세스 출력
        printf("선택한 프로세스: %d\n", random_id);

        // 해당 ID의 프로세스 삭제
        Process selected_process = deleteProcessById(&queue, random_id);

        if (selected_process.id != NULL_PROCESS_ID) {
            printf("삭제 ID: %d, 남은 반복: %d\n", selected_process.id, selected_process.repeat);
            // repeat 감소 및 다시 삽입
            selected_process.repeat--;
            if (selected_process.repeat > 0) {
                insert(&queue, selected_process.id, selected_process.repeat);
                printf("재삽입 ID: %d, 남은 반복: %d\n\n", selected_process.id, selected_process.repeat);
            }
        }
        else {
            printf("Process with ID %d not found!\n", random_id);
        }

        // 큐 상태 출력
        printQueue(&queue);

        // 큐가 비워졌다면 종료
        if (isEmpty(&queue)) {
            printf("Queue is now empty.\n");
            break;
        }
    }

    getchar();  
    return 0;
}
