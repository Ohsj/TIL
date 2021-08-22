# Object Mapping 라이브러리 란?

개발 시 Controller, Service, Repository 등 레이어 간 데이터를 주고 받을 때나 비즈니스 로직에서, 하나의 객체를 탑이 다른 객체로 형(Type)
변환하거나 여러 객체를 다른 객체로 합지는 일은 매우 빈번하다.

이런 작업을 개발자가 모두 직접 할 때 문제점 몇 가지를 나열하자면

1. 재미가 없고 반복적이고 코드 중복이 발생하기 쉽다.
2. 실수하기 쉽다.
3. 결국 생산성을 떨어뜨린다.
4. 비즈니스 로직에 섞이게 되면 코드가 복잡해진다.

Object Mapping 라이브러리는 이런 문제를 해결해주는 아주 좋은 도구이다.


### MapStruct

```MapStruct```는 자바에서 객체 간 매핑에 대한 코드를 자동으로 생성해주는 매핑 라이브러리다.
```Annotation Processor```를 사용해 컴파일 시 매핑 코드를 생성하는데, 많은 장점이 있다.

1. 컴파일 시 오류를 확인할 수 있다.
2. 리플렉션(Reflection)을 사용하지 않기 때문에 매핑  속도가 빠르다.
3. 디버깅이 쉽다.
4. 생성된 매핑 코드를 눈으로 직접 확인할 수 있다.

```MapStruct``` 말고도 ```Dozer```, ```Orika```, ```ModelMapper```, ```JMapper```등 여러 라이브러리가 존재하며
이 라이브러리들의 [성능 테스트를 한 결과](https://www.baeldung.com/java-performance-mapping-frameworks) 가 있는데 ```MapStruct```와 ```JMapper```가 가장 우수한 성능을 보여주었다.
그 중에서 ```MapStruct```를 사용하게 되었다.

#### MapStruct 기능
1. 서로 다른 속성 매핑
   - 변수 명이 다른 속성들은 ```@Mapping(source = "", target = "")```을 사용해서 매핑하면 서로 다른 속성 매핑이 가능하다.
     ex) Ttt -> Ttttt (name 속성을 userName 속성에 매핑해야 될 경우)
        ```
        public class Ttt {
            private String id;
            private String name;
        }
        ```
        ```
        public class Ttttt {
            private String id;
            private String userName;
        }
        ```
        ```
        @Mapper
        public interface TttMapper {
            @Mapping(source = "name", target = "userName")
            Ttttt toTtttt(Ttt test);
        }
        ```
        
2. 객체 합치기
   - 한 객체에 다른 속성이나 다른 객체의 속성들을 합칠수 있다.
        ```
        public class Ttt {
            private String id;
            private String name;
        }
        ```
        ```
        public class Ttttt {
            private String id;
            private String userName;
            private String email;
            private String phoneNum;
        }
        ```
        ```
        @Mapper
        public interface TttMapper {
            @Mapping(source = "name", target = "userName")
            Ttttt toTtttt(Ttt test, String email, String phoneNum);
        }
        ```
        이런 식으로 속성값 하나씩 합치는 매핑 메서드를 정의할 수 있고
        
        ```
        public class Ttt {
            private String id;
            private String name;
        }
        ```
        ```
        public class Ttttt {
            private String email;
            private String phoneNum;
        }
        ```
        ```
        @Mapper
        public interface TttMapper {
            MergeTtt toMergeTtt(Ttt ttt, Ttttt ttttt);
        }
        ```
        이런식으로 두개의 객체를 합쳐서 하나의 객체로 매핑할 수도 있다.
3. 속성 무시하기
   - 매핑 시 타깃 객체에 필요없는 속성이 있어 이 속성을 빼고 매핑하고 싶다면 ```@target(target="", ignore=boolean)```을 사용해서 무시할 수 있다.
        ```
        public class Ttt {
            private String id;
            private String name;
        }
        ```
        ```
        public class Ttttt {
            private String id;
            private String userName;
        }
        ```
        ```
        @Mapper
        public interface TttMapper {
            @Mapping(target = "userName", ignore = true)
            Ttttt toTtttt(Ttt test);
        }
        ```
4. 직접 구현하기
    - 가끔 MapStruct에서 매핑 코드를 구현하지 못하거나 직접 구현해야될 때가 있다. MapStruct는 ```default``` 메서드를 이용해 매핑 메서드를 직접 구현할 수 있게 해준다.
        ```
        @Mapper
        public interface JsonMapper {
            ObjectMapper OBJECT_MAPPER = new ObjectMapper();
    
            default String toString(Object obj) {
                try {
                    return OBJECT_MAPPER.writeValueAsString(obj);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
        ```

### ref
 - [MapStruct](https://mapstruct.org/)
 - [MapStruct 가이드 문서](https://mapstruct.org/documentation/stable/reference/html/)
 - [Object Mapping 어디까지 해봤니?](https://meetup.toast.com/posts/213)