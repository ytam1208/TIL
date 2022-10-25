## **문제**
### 템플릿를 사용하여 클래스 내부 멤버 함수를 템플릿 멤버함수를 정의하고 빌드를 하던 중 아래와 같은 에러가 발생하였다.

</br>

<img width="655" alt="스크린샷 2022-10-24 오후 8 17 57" src="https://user-images.githubusercontent.com/56625848/197514142-6697c70a-21b1-4646-842b-69b4a265d6d2.png">

</br>

분명히 선언부, 구현부를 정확하게 구별하고 include까지 정확하게 수행했음에도 위 사진과 같이 링크 에러라고 뜨는게 이상했다.

해당 에러를 해결하고자 여기저기 검색을 해본 결과 템플릿 클래스의 정의와 구현은 한 파일 안에 있어야 한다는 점을 발견했다.

컴파일러는 파일 단위로 컴파일을 진행하기 때문에 어떤 헤더파일이 어떤 소스파일과 페어인지 고려하지 않고 컴파일을 한다. 헤더파일도 마찬가지로 따로 컴파일 하지 않고 컴파일 대상은 오로지 소스파일(.c, .cpp) 뿐이다.

즉, 컴파일러는 소스파일에 헤더파일이 include 되어 있다면, 헤더파일을 그대로 복붙하고 컴파일을 진행한다.
어떠한 소스파일에도 포함되지 않는 헤더파일이 존재한다면, 그 헤더파일은 컴파일 되지 않는다.

간단하게 정리하면 만약 소스파일(.c, .cpp)안에서 탬플릿 멤버함수들을 미리 선언하게 될 경우 헤더파일에 있는 .hpp에 선언된 탬플릿 멤버함수들의 선언이 무용지물이 되고 컴파일러는 멤버함수를 찾을 수 없다는 에러를 냅니다.

</br></br>

## 에러 예시코드

temp.hpp
``` 
#include <iostream>

class Data
{
    public:
        template <typename TL> void Go(TL &input, TL &output);
        template <typename TL> void Turn(TL &input, TL &output);        
        
        Data(){};
        ~Data(){};
};

class A
{
    public:
        int x = 100;
        int y = 20;
};
```

temp.cpp
```
#include <C_function/templete.hpp>

template <typename TL>
void Data::Go(TL &input, TL &output)
{
    TL compare;
    compare.x = input.x - output.y;
    std::cout << "compare x = " << compare.x << std::endl;
}

template <typename TL>
void Data::Turn(TL &input, TL &output)
{
    TL compare;
    compare.x = input.x + output.y;
    std::cout << "compare x = " << compare.x << std::endl;
}
```

main.cpp
```
#include <C_function/templete.hpp>

int main()
{
    Data data4;
    A a;
    data4.Go(a, a);
    data4.Turn(a, a);    
}
```

</br></br>

## 제안하는 예시코드

temp.hpp
``` 
#include <iostream>

class Data
{
    public:
        template <typename TL> void Go(TL &input, TL &output);
        template <typename TL> void Turn(TL &input, TL &output);        
        
        Data(){};
        ~Data(){};
};

class A
{
    public:
        int x = 100;
        int y = 20;
};


#include "C_function/templete.tpp"
```
temp.tpp
```
template <typename TL>
void Data::Go(TL &input, TL &output)
{
    TL compare;
    compare.x = input.x - output.y;
    std::cout << "compare x = " << compare.x << std::endl;
}

template <typename TL>
void Data::Turn(TL &input, TL &output)
{
    TL compare;
    compare.x = input.x + output.y;
    std::cout << "compare x = " << compare.x << std::endl;
}
```
main.cpp
```
#include <C_function/templete.hpp>

int main()
{
    Data data4;
    A a;
    data4.Go(a, a);
    data4.Turn(a, a);    
}
```

이와같은 방식으로 템플릿화 되어있는 멤버함수들을 미리 .tpp파일로 작성하여 컴파일러가 헤더파일을 확인할때 동시에 링크되게 꼼수를 쓸 수 있다.

여기서 .tpp 파일은 컴파일러가 빌드를 하지 않고 링크를 할때만 헤더파일안에 템플릿들의 구현이라는 사실을 나타낼때 사용한다고 한다.
