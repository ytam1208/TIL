**pthread_exit**

현재 실행중인 쓰레드를 종료시킵니다.

매개변수 retval는 pthread_join()에서 받아 쓸 수 있습니다. 하지만, 이 방법은 단순히 쓰레드만 종료시킬뿐 자원을 해제하지는 않습니다.

쓰레드를 종료시킬때 자원등을 반납해야 되는 등의 clean 하는 프로세스를 해야 할 필요가 있으면 pthread_cleanup_push() 를 사용하여 콜백함수 등록을 해두면 됩니다.

```
void pthread_exit(void *retval);  
```

첫번째 매개변수인 thread는 분리시킬 쓰레드 식별자.


예제 코드
```
#include <iostream>
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

pthread_t  p_thread[2];   //쓰레드 아이디
int a = 0;

void *thread1(void *data)
{
   int id;
   int i = 0;
   id = *((int *)data);
   while(1)
   {
      printf("Thread1 id[%d] a = %d i = %d \n", id, a, i);
      pthread_exit(0);
 
   }
}

void *thread2(void *data)
{
   int id;
   int i = 0;
   id = *((int *)data);
   while(1)
   {
      printf("Thread2 id[%d] a = %d i = %d \n", id, a, i);
      pthread_exit(0);
   }
}

int main()
{

   int thread_id;
   int status;
   int a = 1;
   int b = 2;

   thread_id = pthread_create(&p_thread[0], NULL, thread1, (void *)&a);
   if(thread_id != 0)
      std::cout << "thread 1 create fail: " << thread_id << std::endl;

   thread_id = pthread_create(&p_thread[1], NULL, thread2, (void *)&b);
   if(thread_id != 0)
      std::cout << "thread 2 create fail: " << thread_id << std::endl;


   thread_id = pthread_join(p_thread[0], NULL);
   if(thread_id != 0)
      std::cout << "thread 1 Join fail : " << thread_id << std::endl;

   thread_id = pthread_join(p_thread[1], NULL);
   if(thread_id != 0)
      std::cout << "thread 2 Join fail : " << thread_id << std::endl;

   // pause();
   return 0;
}
```