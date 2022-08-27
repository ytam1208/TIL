**pthred_join** 

쓰레드 생성 이후에 쓰레드의 사용을 다했음을 알고, 더 이상 프로그램이 정상 종료가 될때 혹은 쓰레드의 메모리를 해제할때까지 기다리게 해주는 함수이다.

그러니까 만일 join을 안쓰고 pthread_create만 하게 되면 프로그램이 실행되자마자 쓰레드가 생성되기전에 프로그램이 종료 될 수 있다.

아래 예시 코드처럼 전역변수 i가 쓰레드 생성이 되기도 전에 종료되어 0이 출력된다.

쓰레드가 정상 동작하며 프로그램이 기다리게 해주기 위해서는 join을 꼭 사용해야한다.

```
int pthread_join(pthread_t th, void **thread_return);
```

첫번째 매개변수인 pthread_t 해당 쓰레드가 성공적으로 생성되었을때 생성된 쓰레드를 식별하기 위해서 사용되는 쓰레드 식별자.

두번째 매개변수인 thread_return는 종료 및 대기의 리턴 값을 받는다. 이중 포인터인 이유는 리턴 형식이 void* 이기 때문에 이 리턴 형식을

call by referene 형식으로 받아 적절한 타입 캐스팅을 통해 개발자가 원하는 형태로 값을 받기 위해서 이다.

pthread_join return 값: 성공시 0, 실패시 1을 반환한다.

예제 코드
```
#include <iostream>
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int i = 0;

void *thread1(void *data)
{
   int id;
   id = *((int *)data);
   while(1)
   {
      printf("Thread1 id[%d] i = %d \n", id, i);
      i++;
      sleep(1);
   }
}

int main()
{

   pthread_t  p_thread[1];   //쓰레드 아이디
   int thread_id;
   int status;
   int a = 1;

   thread_id = pthread_create(&p_thread[0], NULL, thread1, (void *)&a);
   if(thread_id != 0)
      std::cout << "thread 1 create fail: " << thread_id << std::endl;

   <!-- thread_id = pthread_join(p_thread[0], NULL);
   if(thread_id != 0)
      std::cout << "thread 1 Join fail : " << thread_id << std::endl; -->

   std::cout << i << std::endl;

   return 0;
}

```