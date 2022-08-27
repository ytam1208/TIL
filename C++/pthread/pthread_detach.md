**pthread_detach**

일반적으로 thread가 종료되고 난 이후에 모든 자원들은 자동으로 해제되지 않는다. 
반면에 pthread join을 사용하면 쓰레드가 정상 동작이 마무리되고 난 후 알아서 해제를 해준다.
pthread_detach는 join을 사용하지 않고도 쓰레드의 모든 자원을 해제해주는 놈이다.

```
int pthread_detach(pthread_t th);
```

첫번째 매개변수인 thread는 분리시킬 쓰레드 식별자.

아래는 join을 쓰지 않고 프로그램이 pause 상태에 들어가며 쓰레드 두개가 생성되어 동작하는 코드이다.

그러면서 쓰레드내 a++ 가 수행되며, a > 10이 될 경우에 pthread exit가 되며, detach를 하여 쓰레드 종료, 메모리 해제등을 실시한다.

예제코드

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
      a++;
      i++;
      sleep(1);

      if(a > 10){
         pthread_exit(0);
         pthread_detach(p_thread[id]);
      }  
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
      a++;
      i++;
      sleep(1);

      if(a > 10){
         pthread_exit(0);
         pthread_detach(p_thread[id]);
      }  
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


   // thread_id = pthread_join(p_thread[0], NULL);
   // if(thread_id != 0)
   //    std::cout << "thread 1 Join fail : " << thread_id << std::endl;

   // thread_id = pthread_join(p_thread[1], NULL);
   // if(thread_id != 0)
   //    std::cout << "thread 2 Join fail : " << thread_id << std::endl;

   pause();
   return 0;
}
```
