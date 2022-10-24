## **문제**
### 최근 콜백함수를 제작하기 위해 함수 포인터를 저장하는 배열을 찾아봤다. 가장 오래된 방식 중에 하나는 typedef를 활용하여 함수의 형태를 정의해주면 보다 깔끔하게 배열에 함수의 주소값을 저장하는 형식으로 사용할 수 있다고 한다.

</br>
코드는 아래와 같다.
typedef로 void 타입의 (int, double)을 가지는 함수의 형태를 미리 정의하여 My_mode라는 배열을 만들어준다. 이후에는 Data 클래스가 생성되는 동시에 미리 정의해둔 같은 타입의 함수들을 배열에 저장시켜 배열에 콜백함수를 저장하여 원하는 시기마다 스위치하며 사용하는 구조로 만들었다.

</br>
```
#include <iostream>

class Data
{
    private:
        int Mode;
        typedef void(Data::*FunctionPointer)(int, double);
        FunctionPointer My_mode[3];

    public:
        void func1(int a, double b);
        void func2(int a, double b);
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
    My_mode[0] = {&Data::func1};
    My_mode[1] = {&Data::func2};
}

void Data::func1(int a, double b)
{
    std::cout << "(+)Print = " << a + b << std::endl;
}

void Data::func2(int a, double b)
{
    std::cout << "(-)Print = " << a - b << std::endl;
}

void Data::runloop()
{
    int a;
    double b;
    while(1)
    {
        std::cout << "a =";
        std::cin >> a;
        std::cout << "b =";
        std::cin >> b;

        this->Mode = a;
        (this->*My_mode[Mode])(a, b);
    }
}

int main()
{
    Data data;
    return 0;
}
```