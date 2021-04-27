# Blocking, Non-Blocking
 Blocking과 Non-Blocking은 직접 제어할 수 없는 대상을 처리하는 방법에 따라 나눈다. 대표적으로 IO와 멀티쓰레드 동기화가 있다.

### Blocking
 Blocking은 직접 제어할 수 없는 대상의 작업이 끝날 때까지 제어권을 넘겨주지 않는다. 
 
 예를 들어 Java에서 Input을 받는 경우 사용자가 값을 입력하기 전 까지 아무 일도 하지 못하고 기다리고 있는 것이다.

 ### Non-Blocking
 Non-Blocking은 Blocking과 반대되는 개념이다. 직접 제어할 수 없는 대상의 작업 처리 여부와 상관이 없다.
 
 예를 들어  Java에서 Input을 받는 경우 사용자가 값을 입력하는 여부와 상관 없이 자신의 작업을 하는 것이다.

 
# Sync(동기), Async(비동기)

### Synchronous (동기)
 Synchronous는 두 가지 이상의 대상(함수, 애플리케이션)이 서로 시간을 맞춰 행동하는 것이다. 예를 들어 Queue에 작업 A와 B가 들어있을 때
 한 쓰레드에서 작업 A가 끝나는 시간과 작업 B가 시작하는 시간이 같다면 동기이다.

### Asynchronous (비동기)
 비동기는 동기와 반대로 대상이 서로 시간을 맞추지 않는 것을 말한다.
 예를 들면 호출하는 함수가 호출되는 함수에게 작업을 맡겨 놓고 신경쓰지 않고 다음 작업을 계속하는 것이 비동기이다.

### Sync/Async, Blocking/Non-Blocking Code
```
 ExcutorService es = Executors.newCachedThreadPool();
 String res = es.submit(() -> "Hello Async").get();
 ```

위에 간단한 코드에서 동기와 비동기를 모두 볼수 있다.
``` es.submit(() -> "Hello Async") ``` : 비동기
``` .get() ``` : 동기, 블록킹

# Future Interface 란?

오라클 문서에 따르면 Future 란

```
Future 인터페이스는 비동기 처리의 결과를 나타내며, 처리가 완료 되었는지 검사하고 처리가 완료 될 때까지 기다리고 처리 결과를 검색하는 메소드를 제공합니다.

처리 결과는 완료 되었을 때 오직 get() 메소드를 통해서만 검색이 가능하며 필요한 경우 준비가 될 때 까지 차단합니다.

취소는 cancel 메소드로 수행되며 작업이 정상적으로 완료되었는지 또는 취소되었는지 확인하기위한 추가 방법을 제공합니다. 작업이 완료되면 취소 할 수 없습니다.

취소 가능성 때문에 Future를 사용하고 싶지만 결과 값을 얻지 않으려고 할때 Future<?>형식을 선언하고 처리 결과로 Null을 반환 할 수 있습니다.
```

간단히 말하자면 비동기 작업을 처리 할 때 사용하는 인터페이스이며 Javascript의 Promise와 비슷하다고 생각합니다.

# CompletableFuture

Future는 많은 구현체를 가지고 있지만 가장 자주 사용하는 ```CompletableFuture```에 대해 알아보겠습니다.

- ```CompletatbleFuture```는 ```Future```와 ```CompletionStage```를 구현한 클래스 입니다.
      ```Future```이지만 직접 쓰레드를 생성하지 않고 Async로 작업을 처리할 수 있고, 여러 ```CompletableFuture```를 병렬로 처리하거나,
      병합하여 처리할 수 있게 합니다. 또한 Cancel, Error를 처리할 수 있는 방법을 제공합니다.
- ```Future```로 사용하기
    ```
    CompletableFuture<String> future
        = new CompletableFuture<>();
    
    Executors.newCachedThreadPool().submit(() -> {
        Thread.sleep(2000);
        future.complete("End!");
        reture null;
    });
    
    System.out.println(future.get());
    ```
    위의 코드에서는 ```newCachedThreadPool()```으로 직접 쓰레드를 만들었고, 그 쓰레드의 작업이 완료되면 ```complete("Finished")```
    으로 결과를 Future에 저장하였습니다. 결과가 저장되면 ```get()```은 값을 리턴하며 Blocking된 상태에서 빠져나옵니다.
    

- ```Cancel()``` 예외 처리
  
  CompletableFuture는 쓰레드에서 ```cancel()```이 호출 될 수가 있는데 이 때 ```get()```에서 ```CancellationException```이 발생합니다.
  다른 예외들과 마찬가지로 ```try - catch``` 블럭에서 ```CancellationException```으로 예외 처리를 할수 있습니다.



- ```supplyAsync(), runAsync()```
    CompletableFuture는 ```supplyAsync(), runAsync()```를 사용해서 쓰레드를 생성하지 않고 작업을 Async로 처리할 수 있습니다.

    ```
    CompletableFuture<String> future 
        = CompletableFuture.supplyAsync(() -> {
        System.out.println("Start");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "Async Result";
    });
    
    System.out.println(future.get());
    ```
    
    제일 마지막에 호출되는 ```future.get()```은 메인 쓰레드에서 2초동안 Blocking 되고 완료하면 리턴값을 가져옵니다.
    
    ```runAsync()``` 도 사용방법은 같지만 차이점이 있다면 ```supplyAsync()``` 리턴 값이 있지만  ```runAsync()```는 리턴 값이 없습니다.
    따라서  ```runAsync()```를 사용하고 싶다면 ```CompletableFuture<Void>``` 으로 선언해야 하며 결과가 완료될 때까지 ```get()```은 
    Blocking 되지만 ```null```을 리턴합니다.

- Exception handling
  
  ```CompletableFuture```에서 작업을 처리하는 중에 Exception이 발생할 수 있습니다. 이런 경우, ```handle()```로 예외를 처리할 수 있습니다.
  ```
  CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
      String name = null;
      if (name == null) {
          throw new RuntimeException("Computation error!");
      }
      return name;
  }).handle((s, t) -> s != null ? s : "Stranger!");
  
  log(future.get());
   ```
  
- 리턴 값이 있는 작업 수행 ```thenApply()```
  ```
  CompletableFuture<String> future1
        = CompletableFuture.supplyAsync(() -> "Future1");
  
  CompletableFuture<String> future2
        = future1.thenApply(s -> s + " + Future2");
  
  CompletableFuture<String> future3
        = CompletableFuture.supplyAsync(() -> "Future3")
                           .thenApply(s -> s + " + H2");
  
  System.out.println("future1.get(): " + future1.get());
  System.out.println("future2.get(): " + future2.get());
  System.out.println("future3.get(): " + future3.get());
  ```
  위 코드의 결과는<br>
  "Future1"<br>
  "Future1 + Future2"<br>
  "Future3 + H2"<br>
  와 같이 나타납니다.
  

- 리턴 값이 없는 작업 수행 ```thenAccept()```
  ```
  CompletableFuture<String> future1
        = CompletableFuture.supplyAsync(() -> "Hello");
  
  CompletableFuture<String> future2
        = future1.thenAccept(s -> s + " World");
  
  System.out.println("future1.get():" + future1.get());
  System.out.println("future2.get():" + future2.get());
  ```
  위 코드의 결과는<br>
  "Hello"<br>
  null<br>
  와 같이 나타납니다. ```thenAccept```는 리턴값이 없기 때문에 ```CompletableFuture<Void>```를 리턴하게 됩니다.   
  

- 여러 작업을 순차적으로 수행 ```thenCompose()```
  ```
  CompletableFuture<String> future1
        = CompletableFuture.supplyAsync(() -> "Hello")
                           .thenComplose(s -> CompletableFuture.supplyAsync(() -> s + " World"));
                           .thenComplose(s -> CompletableFuture.supplyAsync(() -> s + " !!!"));
  System.out.println(future1.get());
  ```  

  ```thenCompose()```는 체인처럼 세개의 ```CompletableFuture```를 하나로 만들어 줍니다.
  첫번째 결과가 리턴되면 그 결과를 두번째로 전달하며, 순차적으로 작업이 처리됩니다.
  결과값으로는 <br>
  "Hello World !!!"가 리턴 됩니다.


- 여러 작업을 동시에 수행 ```thenCombine()```
  ```
  CompletableFuture<String> future1
        = CompletableFuture.supplyAsync(() -> "Future1")
                           .thenApply((s) -> {
                                System.out.println("Start Future1");
                                try {
                                    Thread.sleep(2000);
                                } catch(InterruptedException e) {
                                    e.printStackTrace();
                                }
                                return s + "!";
                            });
  
  CompletableFuture<String> future2
        = CompletableFuture.supplyAsync(() -> "Future2")
                           .thenApply((s) -> {
                                System.out.println("Start Future2");
                                try {
                                    Thread.sleep(2000);
                                } catch(InterruptedException e) {
                                    e.printStackTrace();
                                }
                                return s + "!";
                            });
  
  future1.thenCombine(future2, (s1, s2) -> s1 + " + " + s2)
         .thenAccept((s) -> System.out.println(s));
  ```
  ```thenCompose()```는 여러 ```CompletableFuture```를 병렬로 처리하도록 만듭니다.
  모든 처리가 완료되고 그 결과를 하나로 합칠 수 있습니다.
  결과를 보면<br>
  "Starting future1"<br>
  "Starting future2"<br>
  "Future1! + Future2!"<br>
  로 리턴되는 것을 볼수 있는데 ```thenApply()```가 같은 쓰레드를 사용해서 순차적으로 처리 된것처럼 보인다.

- 위에 설명 했던 메서드들의 뒤에 Async를 붙이면 동일한 쓰레드가 아닌 다른 쓰레드를 사용하여 처리합니다. 예를 들면 ```thenAccept()``` 대신 ```thenAcceptAsync()```를 사용합니다.
  ```thenAccept()``` 말고도 예제에서 봤던 모든 메소드들도 뒤에 async가 붙은 메소드와 pair로 존재합니다.
  ```
  CompletableFuture<String> future1
        = CompletableFuture.supplyAsync(() -> "Future1")
                           .thenApplyAsync((s) -> {
                                System.out.println("Start Future1");
                                try {
                                    Thread.sleep(2000);
                                } catch(InterruptedException e) {
                                    e.printStackTrace();
                                }
                                return s + "!";
                            });
  
  CompletableFuture<String> future2
        = CompletableFuture.supplyAsync(() -> "Future2")
                           .thenApplyAsync((s) -> {
                                System.out.println("Start Future2");
                                try {
                                    Thread.sleep(2000);
                                } catch(InterruptedException e) {
                                    e.printStackTrace();
                                }
                                return s + "!";
                            });
  
  future1.thenCombine(future2, (s1, s2) -> s1 + " + " + s2)
         .thenAccept((s) -> System.out.println(s));
  ```
  아까와 거의 동일한 코드지만 ```thenApply()``` 대신 ```thenApplyAsync()```를 사용해서 다른쓰레드로 처리되어 병렬로 처리되는 것을 볼수 있다.


- ```anyOf()```는 여러개의 CompletableFuture 중에서 가장 빠른 1개의 결과만 가져온다.
  ```
  CompletableFuture<String> future1 = CompletableFuture
          .supplyAsync(() -> {
              log("starting future1");
              return "Future1";
          });
  
  CompletableFuture<String> future2 = CompletableFuture
          .supplyAsync(() -> {
              log("starting future2");
              return "Future2";
          });
  
  CompletableFuture<String> future3 = CompletableFuture
          .supplyAsync(() -> {
              log("starting future3");
              return "Future3";
          });
          
  CompletableFuture.anyOf(future1, future2, future3)
                   .thenAccept(s -> System.out.println("Result: " + s));
  ```
  위 예제의 결과는<br>
  starting future1<br>
  starting future2<br>
  starting future3<br>
  Result: Future2<br>
  와 같이 가장 빨리 작업이 끝난 Future2번의 결과만 볼수 있다.

  
- ```allOf()```는 여러개의 CompletableFuture 모든 결과를 처리한다.
  ```
  CompletableFuture<String> future1 = CompletableFuture
        .supplyAsync(() -> "Future1");

  CompletableFuture<String> future2 = CompletableFuture
  .supplyAsync(() -> "Future2");
  
  CompletableFuture<String> future3 = CompletableFuture
  .supplyAsync(() -> "Future3");
  
  CompletableFuture<Void> combinedFuture
  = CompletableFuture.allOf(future1, future2, future3);
  
  System.out.println("combinedFuture.get() : " + combinedFuture.get());
  System.out.println("future1.isDone() : " + future1.isDone());
  System.out.println("future2.isDone() : " + future2.isDone());
  System.out.println("future3.isDone() : " + future3.isDone());
  
  String combined = Stream.of(future1, future2, future3)
                          .map(CompletableFuture::join)
                          .collect(Collectors.joining(" + "));
  System.out.println("Combined : " + combined);
  ```
  위 예제의 결과는<br>
  combinedFuture.get() : null<br>
  true<br>
  true<br>
  true<br>
  Combined : Future1 + Future2 + Future3<br>
  와 같이 결과를 얻을수 있다.
  ```allOf()```의 다른 점은 Stream api를 사용해서 결과를 처리해야하고 ```get()```은 null을 리턴한다.

### ref
- [Sync VS Async, Blocking VS Non-Blocking](https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx)
- [Oracle Future Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html)
- [Java - CompletableFuture 사용 방법](https://codechacha.com/ko/java-completable-future/)