```
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void* (*start_routine(void*), void *arg))
```

```
첫번째 매개변수인 thread는 쓰레드가 성공적으로 생성되었을때 생성된 쓰레드를 식별하기 위해서 사용되는 쓰레드 식별자.

두번째 매개변수인 attr는 쓰레드 특성을 지정하기 위해서 사용하며, 기본 쓰레드 특성을 이용하고자 할때는 NULL을 사용한다.

세번째 매개변수인 start_routine는 분기시켜 실행할 쓰레드 함수이며,

네번째 매개변수인 arg는 위 start_routine 쓰레드 함수의 매개변수로 넘겨진다.

성공적으로 쓰레드가 생성될 경우 pthread_create는 0을 리턴한다.
```

```
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h> 

// 쓰레드 함수
void *t_function(void *data)
{    
    pid_t pid;            // process id    
    pthread_t tid;        // thread id     
    pid = getpid();    
    tid = pthread_self();     
    char* thread_name = (char*)data;    
    int i = 0;     
    
    while (i<3)   // 0,1,2 까지만 loop 돌립니다.    
    {        
        // 넘겨받은 쓰레드 이름과         
        // 현재 process id 와 thread id 를 함께 출력        
        
        printf("[%s] pid:%u, tid:%x --- %d\n", thread_name, (unsigned int)pid, (unsigned int)tid, i);   
             
        i++;        
        
        sleep(1);  // 1초간 대기    
    } 
    
    int main()
    {    
        pthread_t p_thread[2];    
        int thr_id;    
        int status;    
        char p1[] = "thread_1";   // 1번 쓰레드 이름    
        char p2[] = "thread_2";   // 2번 쓰레드 이름    
        char pM[] = "thread_m";   // 메인 쓰레드 이름      
        sleep(1);  // 2초 대기후 쓰레드 생성     
        // ① 1번 쓰레드 생성    
        // 쓰레드 생성시 함수는 t_function    
        // t_function 의 매개변수로 p1 을 넘긴다.      
        
        thr_id = pthread_create(&p_thread[0], NULL, t_function, (void *)p1);     // pthread_create() 으로 성공적으로 쓰레드가 생성되면 0 이 리턴됩니다    
        
        if (thr_id < 0)    
        {        
            perror("thread create error : ");        
            exit(0);    }     // ② 2번 쓰레드 생성    
            thr_id = pthread_create(&p_thread[1], NULL, t_function, (void *)p2);
            
            if (thr_id < 0)    
            {        
                perror("thread create error : ");        
                exit(0);   
            }     // ③ main() 함수에서도 쓰레드에서 돌아가고 있는 동일한 함수 실행    
            
            t_function((void *)pM);     // 쓰레드 종료를 기다린다.     
            
            pthread_join(p_thread[0], (void **)&status);    
            pthread_join(p_thread[1], (void **)&status);     
            printf("언제 종료 될까요?\n");     
            return 0;
        }
    }
```