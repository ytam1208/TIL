### for문 속도를 더 빠르게 돌릴 수 없을까?

```
최근 최적화 알고리즘을 사용하여 연산을 하던 중에 너무 많은 데이터들이 들어가 프로그램의 속도가 터무니없이 느려지는 현상이 있어 혹시 for문 속도를 더 빠르게 할 수 있는 방법이 있을까 해서 찾아봤다.
```

아래의 코드는 총 3가지 테스트를 진행해보았다.

우선 내가 많이 사용했던 방법 for문에 벡터의 사이즈만큼 도는 방식

두번째는 벡터의 사이즈를 변수로 넣어 상수처리를 하는 것 

세번째는 벡터의 사이즈를 변수로 넣고 반대로 탐색하는 방법

```
#include <iostream>
#include <vector>
#include <chrono>

int main()
{
    std::vector<int> test_array(10000000000);

    int count = 0;
    clock_t st1 = clock();
    for(int i = 0; i < test_array.size(); i++){
        count++;
    }
    clock_t end1 = clock();
    std::cout << end1 - st1 << std::endl;

    clock_t st2 = clock();
    int len = test_array.size();
    for(int i = 0; i < len; i++){
        count++;
    }
    clock_t end2 = clock();
    std::cout << end2 - st2 << std::endl;


    clock_t st3 = clock();
    for(int i = test_array.size(); i >= 0; i--){
        count++;
    }
    clock_t end3 = clock();
    std::cout << end3 - st3 << std::endl;

    return 0;
}

```

이 결과 충격적으로 내가 많이 사용했던 이 방법이 터무니없이 다른 방법들에 비해 느린 결과를 알 수 있었다.

```
    for(int i = 0; i < test_array.size(); i++){
        count++;
    }
```

반면에 다른 방법들은 비교적 빠른 속도를 낼 수 있었는데 그 이유는 for문이 한번 돌 때마다 계속해서 벡터의 크기를 계산해주기 때문에 느릴 수 밖에 없다고 한다.

그렇다면 마지막 방법 벡터를 거꾸로 탐색하는 방법?
이 방법이 2번째 방법과 별 차이가 없을 수도 있지만 조금 더 빠른 결과를 낼 수 있었다.