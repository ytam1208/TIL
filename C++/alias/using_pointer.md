## **문제**
### 기존에 typedef를 사용하여 콜백함수들을 저장하는 함수 포인터 배열을 만들었다. 이때, 모든 함수들의 매개변수들의 형태는 모두 동일하게 구성되어야 한다는 단점이 있었다. 그렇기 때문에 좀 더 가독성 높은 코드를 구성하기 위해 template 클래스를 사용하여 유동적인 코드를 구현하고자 했다. 하지만 기존에 typedef로는 template 클래스보다 나중에 나온 문법이라 적용이 안된다고 한다. 그래서 이후에 나온 using을 사용하면 typedef와 동일한 기능을 가지고 있지만 template 클래스를 적용할 수 있다고 한다.

</br>

코드는 아래와 같다. 템플릿을 사용하기전에 보통 using을 사용한 함수 포인터의 형식은 10번째 줄처럼 선언을 하지만, 템플릿을 적용시켰을때 13번째 줄처럼 구성할 수 있다. 이후에는 14~17번째 줄처럼 템플릿에 적용시키고 싶은 변수들의 타입을 미리 선언한다. 
예제 코드는 아래와 같다.

</br>

```
#include <iostream>

class Data
{
    public:
        int Mode;
        // typedef void(Data::*FunctionPointer)(int, double);
        // FunctionPointer My_mode[3];

        // using FunctionPointer = void(Data::*)(int, double);
        // FunctionPointer My_mode[3];

        template <typename T, typename T2> using FunctionPointer = void(Data::*)(T, T2);
        FunctionPointer<int, int> My_mode1[3];
        FunctionPointer<int, double> My_mode2[3];
        FunctionPointer<double, int> My_mode3[3];
        FunctionPointer<double, double> My_mode4[3];

        template <typename T, typename T2> void func1(T a, T2 b);
        template <typename T, typename T2> void func2(T a, T2 b);
        void runloop();
        void init_mode();
        Data():Mode(0)
        {
            init_mode();
            runloop();
        };
        ~Data(){};
};

void Data::init_mode()
{
    My_mode1[0] = {&Data::func1};
    My_mode1[1] = {&Data::func2};

    My_mode2[0] = {&Data::func1};
    My_mode2[1] = {&Data::func2};

    My_mode3[0] = {&Data::func1};
    My_mode3[1] = {&Data::func2};

    My_mode4[0] = {&Data::func1};
    My_mode4[1] = {&Data::func2};
}

template <typename T, typename T2>
void Data::func1(T a, T2 b)
{
    std::cout << "(+)Print = " << a + b << std::endl;
}

template <typename T, typename T2>
void Data::func2(T a, T2 b)
{
    std::cout << "(-)Print = " << a - b << std::endl;
}

void Data::runloop()
{
    Mode = 0;
    (this->*My_mode1[Mode])(10.5, 10.5);       
    (this->*My_mode2[Mode])(10.5, 10.5);       
    (this->*My_mode3[Mode])(10.5, 10.5);       
    (this->*My_mode4[Mode])(10.5, 10.5);       

    Mode = 1;
    (this->*My_mode1[Mode])(11.5, 10.5);       
    (this->*My_mode2[Mode])(11.5, 10.5);       
    (this->*My_mode3[Mode])(11.5, 10.5);       
    (this->*My_mode4[Mode])(11.5, 10.5);
}

int main()
{
    Data data;
    return 0;
}
```

적절한 상황에 맞는 가독성 높은 함수 포인터 배열을 사용 할 수 있다.

[아래 코드 실행 결과] 
</br>

<img width="599" alt="스크린샷 2022-10-24 오후 3 16 19" src="https://user-images.githubusercontent.com/56625848/197459990-335f85bc-4cee-4b8e-808c-297ac07a8d00.png">

