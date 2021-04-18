---
layout: post
title: Python 의 multithreading 과 multiprocessing 알아보기
comments: true
categories: [programming, python]
---

안녕하세요 한헌종입니다.
오늘은 python 에서의 병렬 처리에 대해 공부해보려 해요.
사실 업무에서 병렬처리를 해야 할 일이 생겼는데, 처음 해보는 거라 잘 모르겠더라구요.
이번 기회에 공부하면서 정리해보려 하니, 관심 있으신 분들은 참고해 보시면 좋을 것 같아요!

---
이번 글은 많은 글들을 참조해서 작성했습니다.
주로 Gennaro S. Rodrigues 가 작년에 작성한 글에서 따와서 번역해가며 적어보려 해요: [링크](https://towardsdatascience.com/multithreading-vs-multiprocessing-in-python-3afeb73e105f)

---

이 글은 먼저 두 개념을 비교합니다: **multithreading** vs **multiprocessing**
이 둘의 차이는 **thread** 와 **process** 의 차이에 기인합니다.
또한 이것이 **concurrent execution** 와 **paralle execution** 의 차이를 만듭니다.
병렬 처리란 건 다 똑같은 줄 알았는데, 대체 어떤 차이가 있는 걸까요?

### Thread vs Process
이 개념을 이해할 때 원문의 CPU 그림이 매우 도움이 많이 됐습니다.
그대로 가져오긴 좀 그래서, 제가 이해한대로 다시 그려볼게요.

| ![-]({{"/assets/210418/figure1.png"| relative_url}}) | 
|:--:| 
| *Parallel 하게 처리되고 있는 process 들. thread 들은 concurrent 하게 실행되고 있습니다* |

즉, CPU 하나에서 진행되는 작업은 process 라 하며, 하나의 process 는 여러개의 thread 를 가질 수 있습니다.
한 process 의 thread 들은 서로 메모리를 공유합니다.

한 process 가 여러 thread 를 실행하는 것을 **Concurrent execution** 이라 하고,
여러 process 를 동시에 실행시키는 것을 **Parallel execution** 이라 합니다.

이에 따르면, **multithreading** 은 여러 thread 를 동시에 실행하는 것을 말하구요 (concurrency),
**multiprocessing** 은 여러 process 를 동시에 실행하는 것을 말합니다 (parallelism).

### 둘 중 무엇을 써야 할까?
이는 현재 작업이 어떤 것이냐에 따라 다릅니다.
본문에 따르면 multithreading 은 **I/O intensive tasks** 에 써야 하고,
그리고 multiprocessing 은 **CPU intensive tasks** 에 써야 한다고 하네요.
그 이유가 뭔지 한번 공부해봅시다.

**Case 1. 여러 작업을 실행하고 싶은데, 이들이 CPU 를 많이 잡아먹지는 않는데 I/O, 즉 읽고 쓰는 작업이 많을 때**
이런 경우에는 각 작업들을 thread 로 묶으면 됩니다.
아마 아래와 같은 작업이 될 거에요.

| ![-]({{"/assets/210418/figure2.png"| relative_url}}) | 

예를 들어 읽어야 할 파일이 10개 쯤 되는데, 이를 빠르게 읽고 싶다면 multithreading 을 해야 합니다.
물론 multiprocessing 으로도 가능하겠지만, multithreading 이 더 유리합니다.
그리고 읽은 파일들의 데이터를 모두 모아 작업해야 한다면, memory 를 공유하는 multithreading 이 더 빠르고 안전하겠죠?
이 때, thread 를 2, 3, ... 으로 계속 늘릴 수 있지만 속도가 항상 2배, 3배, ... 로 늘어나지는 않습니다.

**Case 2. 여러 작업을 실행하고 싶은데, CPU 를 많이 잡아먹는 일, 즉 계산량이 많은 작업일 때**
어떤 게산을 여러번 해야할 때는 multiprocessing 에서 이득을 얻을 수 있습니다.
multiprocessing 은 말 그대로 새로운 process 를 만들어서 계산할 작업을 나누는 것인데요.
다음과 같은 방식으로 작업이 실행될 겁니다.

| ![-]({{"/assets/210418/figure3.png"| relative_url}}) | 

이 때, process 를 2개, 3개 ... 계속 늘릴 수 있는데요, 이 때도 역시 속도가 바로 2배, 3배 ... 가 되지는 않습니다.
설명에 따르면 process 관리에 비용이 들어가기 때문이라고 하는데요, process 초기화 비용 등을 이야기하는 거겠죠?
그리고 아까 위에서 말한대로 메모리를 공유하지 않기 때문에 해당 데이터를 넘기고 다시 받는 데에 시간이 걸릴 것입니다.

---
*참고로 위 본문의 링크를 따라가면 multiprocessing 으로 I/O intensive tasks 를 할 수 있지 않을까? 라며 실험해본 결과가 있습니다.
결과는 multiprocessing 도 속도를 높여주긴 한다! 였습니다.
다만, **여전히 multithreading 이 더욱 유리한 이유**가 있습니다.
1. multithreading 이 결과적으로 더 빠릅니다.
1. multiprocessing 은 초기화 비용이 많이 듭니다.
1. multiprocessing 은 기기의 코어 수 만큼만 병렬처리가 가능합니다.

---

### 간단한 예제들
이제 아래에서는 정말 간단한 예제 몇개를 찾아보려 합니다.
어떻게 작동시키는지만 확인하는 코드라 실제 효율을 체크해보진 않았어요.
실제로 얼마나 효율이 좋은지는 원문의 jupyter notebook 으로 된 예제를 한번 살펴보시는걸 추천합니다!

병렬 처리를 할 수 있는 python module 이 여러가지가 있습니다.
여기서는 
1. concurrent.features 모듈의 Executor
2. multithreading 모듈
3. multiprocess 모듈

이 세 가지를 확인해보려 합니다.

---
### 예제 1: concurrent.features 모듈
concurrent.features 모듈은 여러 작업을 비동기로 실행할 수 있는 인터페이스를 제공해줍니다.
이를 통해 multi-threading 및 multi-processing 을 진행할 수 있죠.
다음 문서를 참고해보세요: [문서 링크](https://docs.python.org/3/library/concurrent.futures.html)

아래 예제는 3초 기다렸다가 두 숫자의 합을 출력하는 print_sum 이라는 함수를 병렬처리하는 예제입니다.
먼저 multi-threading 으로 진행 해보았습니다.

<pre><code>import time
from concurrent.futures import ThreadPoolExecutor

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    with ThreadPoolExecutor(max_workers=3) as executor:
        executor.submit(print_sum, 1, 2)
        executor.submit(print_sum, 2, 3)
        executor.submit(print_sum, 3, 4)
    print("done!")

if __name__ == "__main__":
    main()</code></pre>

executor 라는 오브젝트를 만들고, 각 함수를 submit 이라는 메서드로 실행했습니다.
print_sum 함수를 세 번 실행하고, 각각의 실행마다 현재 시간을 같이 출력하게 했습니다.
만약 병렬처리가 정말로 잘 이루어졌다면 이 세 번의 함수 실행이 거의 동일한 시간을 출력해주겠죠?
병렬처리가 안되고 순서대로 출력된다면 서로 3초의 시간 간격이 있을 거에요.
결과는 이렇습니다:
<pre><code><b>$ python3 threading1.py</b>
3 Sun Apr 18 14:30:27 2021
5 Sun Apr 18 14:30:27 2021
7 Sun Apr 18 14:30:27 2021
done!</code></pre>
출력된 시간을 보니 초단위가 같군요. 함수가 병렬로 실행됐음을 확인할 수 있습니다.

똑같은 작업을 multi-processing 으로 진행해봤습니다.

<pre><code>import time
from concurrent.futures import ProcessPoolExecutor

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    with ProcessPoolExecutor(max_workers=3) as executor:
        executor.submit(print_sum, 1, 2)
        executor.submit(print_sum, 2, 3)
        executor.submit(print_sum, 3, 4)
    print("done!")

if __name__ == "__main__":
    main()</code></pre>

실행 결과입니다:
<pre><code><b>$ python3 processing1.py</b>
3 Sun Apr 18 14:35:48 2021
5 Sun Apr 18 14:35:48 2021
7 Sun Apr 18 14:35:48 2021
done!</code></pre>

multiprocessing 역시 동시에 잘 진행되는 것을 확인했습니다.
이제 여러 작업이 있을 때 IO-bound tasks 인지 CPU-bound tasks 인지에 따라 선택해서 작업하면 되겠군요.

---
### 예제 2: threading 모듈

다음은 threading 이라는 모듈을 사용해 진행하는 예제입니다.
threading library 에 대한 원본 문서는 [이 링크](https://docs.python.org/3.8/library/threading.html) 를 참조하시면 되구요,
다음 링크에 훨씬 더 많은 예제와 설명이 되어 있으니 참조해보세요! [문서 링크](https://pymotw.com/2/threading/)

Thread 오브젝트를 만든 뒤, start() 메서드를 사용하면 해당 작업이 실행됩니다.
이 역시 병렬처리가 제대로 되었는지 확인하기 위해 시간을 출력해보았습니다.

<pre><code>import threading
import time

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    thread1 = threading.Thread(target=print_sum, args=(1, 2))
    thread2 = threading.Thread(target=print_sum, args=(2, 3))
    thread3 = threading.Thread(target=print_sum, args=(3, 4))

    thread1.start()
    thread2.start()
    thread3.start()
    print("done!")

if __name__ == "__main__":
    main()</code></pre>
실행 결과입니다:
<pre><code>$ python3 threading2.py
done!
3 Sun Apr 18 15:15:59 2021
5 Sun Apr 18 15:15:59 2021
7 Sun Apr 18 15:15:59 2021</code></pre>
똑같은 시간이 출력된 걸 보니 병렬처리가 잘 이루어졌군요.
그리고 맨 아래에 출력하게 한 "done1" 부분이 가장 먼저 출력된 것을 보니, start()를 하면 thread 하나가 독립적으로 작업을 시작하나 봅니다.

이 예제를 보시면 main thread 가 실행되고, 각각의 sub thread 가 실행되죠.
main thread 는 내부적으로 모든 thread 가 다 끝날때까지 기다립니다.
그래서 맨 아래의 "done" 이 출력됐지만 main 이 끝나지 않고 다른 thread 들의 작업을 기다린 걸 보실 수 있습니다.
이 thread 를 main 의 종료에 상관 없는 thread 로 만들 수도 있습니다. 이를 "daemon" 이라 합니다.
보통은 이럴 일이 잘 없지만, 만약 이렇게 되면 main 이 끝나기 전 반드시 join() 이라는 메서드로 종료될 때까지 기다려줘야 합니다.

**main thread 가 끝나기 전, 병렬 처리로 진행된 모든 thread 가 잘 끝났는지 확인하려면 어떻게 해야할까요?**
이는 enumerate 메서드로 확인이 가능합니다.
각 thread 를 iterative 하게 받아서 join 으로 끝나기까지 기다리면 되는거죠.
이 때, main thread 자체는 join 할 수 없기 때문에 넘어가야 합니다.

예제를 한번 살펴보겠습니다:
<pre><code>import threading
import time

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    thread1 = threading.Thread(target=print_sum, args=(1, 2))
    thread2 = threading.Thread(target=print_sum, args=(2, 3))
    thread3 = threading.Thread(target=print_sum, args=(3, 4))

    thread1.start()
    thread2.start()
    thread3.start()

    main_thread = threading.currentThread()
    for thread in threading.enumerate():
        if thread is main_thread:
            continue
        thread.join()
        print(thread.name, thread.isAlive())

    print("done!")

    for thread in threading.enumerate():
        print(thread.name, thread.isAlive())

if __name__ == "__main__":
    main()</code></pre>
먼저 현재 가지고 있는 모든 thread 를 threading.enumerate() 로 얻어옵니다.
그 다음, 각 thread 작업을 join() 메서드로 종료를 기다립니다.
이 때, main thread 는 넘어가도록 합니다.
각 thread 의 이름을 확인하기 위해 thread.name 으로 출력하고, 각 thread 가 잘 종료되었는지 (아직 살아있는지) isAlive() 로 확인합니다.
마지막으로, join 으로 모두 끝낸 다음 (print("done!") 까지 한 다음) thread 가 어떤 것이 남아있는지 출력해서 확인했습니다.

다음은 실행 결과입니다:
<pre><code><b>$ python3 threading3.py</b>
3 Sun Apr 18 16:31:41 2021
5 Sun Apr 18 16:31:41 2021
7 Sun Apr 18 16:31:41 2021
Thread-1 False
Thread-2 False
Thread-3 False
done!
MainThread True</code></pre>
병렬처리는 잘 진행됐습니다.
join() 을 하고 나니 모든 thread 가 잘 종료되었군요.
그리고 "done!" 이 출력된 이후 나머지 thread 가 뭐가 있는지 확인해보니 MainThread 말고는 없군요.
이렇게 안전하게 multi-threading 을 진행하면 될 것 같습니다.

---
### 예제 3: multiprocessing 모듈
이번엔 multiprocessing 모듈로 간단하게 병렬처리를 확인해보겠습니다.
원본 문서는 다음 링크를 참조해주세요: [문서 링크](https://docs.python.org/3.8/library/multiprocessing.html)
설명 글에는 대부분의 API 가 threading 모듈과 비슷하게 작성되었다고 하네요.

multiprocessing 으로 작업하는 방법이 두 가지 있습니다.
하나는 Pool 이라는 클래스를 이용하는 것입니다.
Pool 은 a pool of worker processes 를 나타낸다고 하네요.
Pool 에 있는 다양한 메서드로 병렬처리가 가능합니다. map, apply 등등..
여기서는 starmap 이라는 걸 사용해서 진행해보도록 하겠습니다.

<pre><code>import multiprocessing
import time

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    with multiprocessing.Pool(3) as pool:
        pool.starmap(print_sum, [(1, 2), (2, 3), (3, 4)])

if __name__ == "__main__":
    main()</code></pre>

starmap 은 해당 함수에 인자를 전달해주는데, map 과 다른 점은 여러 개의 인자를 한번에 전달할 수 있다는 것입니다.
여기서 Pool(3) 은 총 세 개 까지의 worker process 를 사용한다는 뜻입니다.
즉 process 를 최대 3개 까지만 사용한다는 거죠.
다음은 실행 결과입니다.
<pre><code><b>$ python3 processing1.py</b>
3 Sun Apr 18 16:54:27 2021
5 Sun Apr 18 16:54:27 2021
7 Sun Apr 18 16:54:27 2021</code></pre>
동시에 잘 진행된 것 같네요.

이번엔 Pool 이 아닌 **Process** 클래스를 사용해볼게요.
이 작업에서는 위에서 봤던 threading 과 비슷한 메서드들이 많이 나옵니다.

<pre><code>import multiprocessing
import time


def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())


def main():
    process1 = multiprocessing.Process(target=print_sum, args=(1, 2))
    process2 = multiprocessing.Process(target=print_sum, args=(2, 3))
    process3 = multiprocessing.Process(target=print_sum, args=(3, 4))

    process1.start()
    process2.start()
    process3.start()

    print("done!")


if __name__ == "__main__":
    main()</code></pre>

아까 threading 에서 본 코드와 매우 유사하죠?
실행 결과도 다음과 같이 거의 같습니다:
<pre><code><b>$ python3 processing2.py</b>
done!
3 Sun Apr 18 16:59:00 2021
5 Sun Apr 18 16:59:00 2021
7 Sun Apr 18 16:59:00 2021</code></pre>
아까처럼 "done!" 이 가장 먼저 출력됐네요.


**main process 가 끝나기 전, 병렬 처리로 진행된 모든 process 가 잘 끝났는지 확인하려면 어떻게 해야할까요?**
아까와 같은 식으로 확인해야 합니다.
다만 multiprocessing 에는 threading 의 enumerate 메서드 대신 active_children() 이 있습니다.
이걸로 아까처럼 모든 child process 를 join() 으로 종료해보도록 할게요.

<pre><code>import multiprocessing
import time

def print_sum(num1, num2):
    time.sleep(3)
    print(num1 + num2, time.ctime())

def main():
    process1 = multiprocessing.Process(target=print_sum, args=(1, 2))
    process2 = multiprocessing.Process(target=print_sum, args=(2, 3))
    process3 = multiprocessing.Process(target=print_sum, args=(3, 4))

    process1.start()
    process2.start()
    process3.start()

    for process in multiprocessing.active_children():
        process.join()
        print(process.name, process.pid, process.is_alive())

    print("done!")

    for process in multiprocessing.active_children():
        print(process.name, process.is_alive())

if __name__ == "__main__":
    main()</code></pre>
threading 이랑 좀 다른 점이, threading 에서는 isAlive() 였는데 여기서는 is_alive() 네요. 왜 다르게 했을까...
다음은 실행 결과입니다:
<pre><code><b>$ python3 processing3.py</b>
5 Sun Apr 18 17:17:21 2021
3 Sun Apr 18 17:17:21 2021
7 Sun Apr 18 17:17:21 2021
Process-3 55166 False
Process-2 55165 False
Process-1 55164 False
done!</code></pre>

아까 threading 이랑의 차이점이라 하면, threading 은 '모든 thread' 를 가져오는 메서드가 있다면 여기서는 '모든 active children process' 를 가져오는 메서드라는 것이죠.
따라서 여기서 main process 인지 검사할 필요가 없습니다.
active_children 으로 가져오는 process 는 순서가 상관 없나 봅니다. process-3 부터 출력되네요.
이렇게 모든 process 를 정상적으로 종료할 수 있었습니다.

---

네, 오늘은 multithreading 과 multiprocessing 을 공부해봤습니다.
작성하고 나니 너무 겉핥기만 한 것 같긴 하네요.
해당 개념은 여기서 이렇게 짧게 다룬 것 외에도 무척 방대한 양의 개념이 있습니다.
또한 여기서 다룬 모듈에도 이 예제에 쓰인 것 말고도 무척 다양한 메서드가 있으니, 꼭 원본 문서를 참조해보세요.
그래도 이 글을 통해 병렬 처리의 기본 개념을 쌓는 데 도움이 되시길 바랍니다.

그럼 다음 시간에 만나요!