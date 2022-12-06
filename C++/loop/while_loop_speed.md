### while문 속도를 더 빠르게 돌릴 수 없을까?

```
일전에 for문 속도 문제와 동일한 방식으로 while 문에 적용을 시켜보았다.
while 문이 종료되는 시점은 모든 벡터에 대해 카운트 값을 넣어준 변수가 벡터의 크기보다 커졌을때 종료하였다.
```

```
#include <iostream>
#include <vector>
#include <chrono>
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>

std::vector<int> test_array(10000000000);
pthread_t  p_thread[2];

void *thread1(void *data)
{
    int id;
    id = *((int *)data);
    int count = 0;

    struct timespec  begin, end;
    clock_gettime(CLOCK_MONOTONIC, &begin);
    while(count > test_array.size()){
        count++;
    }
    clock_gettime(CLOCK_MONOTONIC, &end);
    long time = (end.tv_sec - begin.tv_sec) + (end.tv_nsec - begin.tv_nsec);
    printf("Thread id[%d] Time (Nano): %lf\n", id, (double)time);

    pthread_exit(0);
    pthread_detach(p_thread[id]);
}

void *thread2(void *data)
{
    int id;
    id = *((int *)data);
    int count = 0;

    int size = test_array.size();
    struct timespec  begin, end;
    clock_gettime(CLOCK_MONOTONIC, &begin);
    while(count > size){
        count++;
    }
    clock_gettime(CLOCK_MONOTONIC, &end);
    long time = (end.tv_sec - begin.tv_sec) + (end.tv_nsec - begin.tv_nsec);
    printf("Thread id[%d] Time (Nano): %lf\n", id, (double)time);

    pthread_exit(0);
    pthread_detach(p_thread[id]);
}

int main()
{
    int thread_id, thread_id2;
    int status;
    int a = 1;
    int b = 2;
    int count = 0;
    int size = test_array.size();

    thread_id = pthread_create(&p_thread[0], NULL, thread1, (void *)&a);
    if(thread_id != 0)
        std::cout << "thread 1 create fail: " << thread_id << std::endl;

    thread_id2 = pthread_create(&p_thread[1], NULL, thread2, (void *)&b);
    if(thread_id2 != 0)
        std::cout << "thread 2 create fail: " << thread_id2 << std::endl;    

    // struct timespec  begin, end;
    // clock_gettime(CLOCK_MONOTONIC, &begin);
    // for(int i = 0; i < test_array.size(); i++){
    //     count++;
    // }
    // clock_gettime(CLOCK_MONOTONIC, &end);
    // long time = (end.tv_sec - begin.tv_sec) + (end.tv_nsec - begin.tv_nsec);
    // printf("For.size() Time (Nano): %lf\n", (double)time);

    // clock_gettime(CLOCK_MONOTONIC, &begin);
    // for(int i = 0; i < size; i++){
    //     count++;
    // }
    // clock_gettime(CLOCK_MONOTONIC, &end);
    // time = (end.tv_sec - begin.tv_sec) + (end.tv_nsec - begin.tv_nsec);
    // printf("For2 int size Time (Nano): %lf\n", (double)time);


    return 0;
}
```

## 테스트 결과

세컨드, 밀리세컨드에서는 큰 연산 작업이 이루어지지 않기에 비교할 수 없을정도로 값이 작았다. 
나노세컨드로 바꿔서 출력했더니 벡터의 크기가 크지 않을 경우에는 비슷하지만, 벡터의 크기가 클 경우 차이가 크게 발생했다.

### Test1
```
Thread id[1] Time (Nano): 1600.000000           <-- .size()로 매 루프마다 검사했을때 소요된 시간
Thread id[32767] Time (Nano): 900.000000        <-- size의 값을 가지는 변수를 미리 선언했을 때 매 루프마다 검사했을때 소요된 시간
```
### Test2
```
Thread id[32767] Time (Nano): 1000.000000
Thread id[1] Time (Nano): 1400.000000
```
### Test3
```
Thread id[1] Time (Nano): 800.000000
Thread id[32767] Time (Nano): 700.000000
```
### Test4
```
Thread id[1] Time (Nano): 800.000000
Thread id[32767] Time (Nano): 700.000000
```

### Test5
```
Thread id[1] Time (Nano): 1700.000000
Thread id[32767] Time (Nano): 1000.000000
```