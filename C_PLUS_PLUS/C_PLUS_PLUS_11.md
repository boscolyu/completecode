
# C++11

* C++ 11(C++0x라고도 알려짐)은 [1] ISO가 2011년 8월 12일에 승인한 C++ 프로그래밍 언어의 최신판이다.
 * GCC : v4.7부터 C++11으로 시험 적용함.
 * Visual Studio : Visual Studio 12까지 부분 적용

# keyword
## auto

* auto 키워드는 변수를 초기화할 때 명시적으로 type을 지정하지 않고 auto로 지정할 수 있음.
* C#, Javascript의 var와 비슷함.
* 사용 범위 : 지역 변수에서만 사용 가능, 멤버 변수, 함수 인자, 전역 변수에서는 사용 불가.
* 실행 시점이 아닌 컴파일 시점에 자동으로 형을 결정하는 키워드임.
* pointer, const, reference로 사용 가능함.
* 템플릿 프로그래밍이나 STL 사용시 코드가 더 간단해짐.



```C++
auto NPCname = "BugKing";
auto Number = 1;
auto* pUserMode = &UserMode;
auto* CharInven = new CharacterInvenInfo();
```

## lambda
 
* 람다함수, 이름없는 함수로 부르며 함수 객체와 같음
* C++ 규격에서 람다는 특별한 형을 가지고 있다.
* 'decltype'(C++11에서 새로 생김)과 'sizeof'에서는 사용할 수 없음.
* capture
	* [&] [=] 와 같이 람다 함수 외부 변수들도 참조가 가능함
	* reference가 아닌 value로 복사해서 람다함수 내부에서 수정하면 에러 발생함.
	* mutable 키워드를 사용해야함.
	* value 복사이기 때문에 내부에서 수정을 해도 람다함수를 빠져나오면 예전값으로 돌아옴.

```C++
int main() {
    auto func = [] (int n){std::cout << "Number : " << n << std::endl; };
    func();
    return 0;
}

int main() {
    float func = [] {return 3.14; };
    float func = [] (float f) {return f; };
    float func = [] () -> float {return 3.14; };

    float f1 = func();
    float f2 = func(3.14f);
    float f3 = func();
}
```

example) c++ find_if 

```C++
auto Iter = find_if(User,begin(), User.end(),
                    [](User& tUser) -> bool {return true == uTuser.IsDie(); }
            );
struct FinDieUser
{
    bool operator() (User& tUser) const
    {
        return tUser.IsDie();
    }
};

Iter = find_if (User.begin(), User.end(), FinDieUser());
```

## range based for
* for each를 range based for로 바꾸어서 사용하면 됨








```
https://ko.wikipedia.org/wiki/C%2B%2B11
```
