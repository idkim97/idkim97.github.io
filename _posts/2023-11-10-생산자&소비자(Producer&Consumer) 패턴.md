---
permalink: /2023-11-10-생산자&소비자(Producer&Consumer) 패턴/
published: true
title: "[Design Pattern] 생산자&소비자(Producer&Consumer) 패턴 "
date: 2023-11-10 18:00:00
toc: true
toc_sticky: true
toc_label: "생산자&소비자(Producer&Consumer) 패턴"
categories:
- 디자인 패턴
tags:
- 디자인 패턴
- Design Pattern
- 생산자&소비자(Producer&Consumer) 패턴
---
<br><br><br>

## ✅ 생산자 소비자 패턴이란?

생산자-소비자 패턴은 **멀티스레드 환경에서 여러 스레드가 공유 자원에 접근할 때 발생하는 동기화 문제를 해결하기 위한** 디자인 패턴이다.

생산자-소비자 패턴은 멀티스레드 환경에서 **생산자(Producer) 스레드가 데이터를 생성하고, 소비자(Consumer) 스레드가 해당 데이터를 소비**하도록 구성되고, 효율적으로 자원을 관리할 수 있도록 한다.

<br>

<p align="center">
<img src="https://github.com/idkim97/idkim97.github.io/blob/master/img/producerconsumer.png?raw=true">
</p>

<br>

위 그림처럼 작업을 생성하는 **생산자 스레드**와 Queue로 설계된 **공유자원**, 그리고 작업을 수행하는 **소비자 스레드**로 구성되어 있는것이 일반적인 생산자-소비자 패턴의 구조이다.

<br><br><br><br>

## ✅ 생산자 소비자 패턴의 구성 요소

1. **생산자(Producer)**

    데이터를 생성하고 공유자원에 저장하는 역할을 수행한다. 생산자는 새로운 데이터를 생성하여 공유자원에 추가하거나 기존의 데이터를 갱신한다.


2. **소비자(Consumer)**

    공유 자원에서 데이터를 가져와서 처리하는 역할을 수행한다.


3. **공유 자원(Buffer 또는 Queue)**

    생산자와 소비자 간의 데이터 교환을 위한 공유 자원이다. 일반적으로 버퍼(Buffer)나 큐(Queue)로 구현된다.

4. **동기화 메커니즘**

    생산자와 소비자 간의 동기화를 이루기 위한 메커니즘이 존재한다. 생산자측은 공유자원이 가득 차있는지 확인하고 데이터를 추가하기 위해 대기해야 하고, 소비자측은 공유자원이 비어 있는지 확인하고 데이터를 가져오기 위해 대기하는 등의 동작을 수행한다. 일반적으로 **뮤텍스(Mutex), 세마포어(Semaphore), 조건 변수(Condition Variable)** 등이 있다.

<br>

> **뮤텍스 : 상호 배제를 통해 한번에 한 스레드만 공유자원에 접근 가능하도록 함**  
> **세마포어 : 공유 자원에 접근할 수 있는 스레드의 수를 제어**  
> **조건변수 : 특정 조건을 만족할 때 까지 스레드를 대기시킴** 

<br><br><br><br>


## ✅ 생산자 소비자 패턴을 사용하는 이유

1. **자원관리**

    여러 스레드가 공유자원에 동시에 접근하는 상황에서 자원을 효율적으로 관리할 수 있다. 한마디로 자원의 공유 및 동시 접근 문제를 효과적으로 해결할 수 있다.

2. **동기화**

    멀티 스레드 환경에서 생성자와 소비자 간의 데이터 접근을 동기화하여 데이터의 **일관성**과 **안정성**을 보장할 수 있다. 동기화 메커니즘을 통해 생산자 작업을 생성 할 때 소비자가 이를 가져오고, 소비자가 작업을 처리할 때 다른 소비자가 동일한 데이터를 중복으로 처리하지 않도록 보장한다.

3. **효율성**

    생산자-소비자 간의 작업을 비동기적으로 처리하여 시스템의 성능을 향상시킬 수 있다. 생산자는 데이터를 생성하고 버퍼에 추가하는 동안 다른 작업을 수행할 수 있고, 소비자 역시 데이터를 처리하는 동안 다른 작업을 수행할 수 있다.

4. **작업 속도 조절 가능**

    버퍼, 큐의 크기나 생산자의 생산속도를 조절하여 시스템의 작업 속도를 조절할 수 있다. 이를 통해 안정성과 효율성을 최적화 할 수 있다.
    

<br><br><br><br>


## ✅ 생산자 소비자 패턴 적용 상황

1. **스레드 풀 ( Thread Pool )**

    생산자는 작업을 생성하고 버퍼,큐에 추가하며, 소비자는 버퍼에서 작업을 가져와 처리한다. 이를 통해 작업을 비동기적으로 처리하여 시스템의 처리량을 향상시킬 수 있다.

```java
import java.util.concurrent.*;

class Producer implements Runnable {
    private BlockingQueue<Integer> buffer;
    private int data = 0;

    public Producer(BlockingQueue<Integer> buffer) {
        this.buffer = buffer;
    }

    public void run() {
        try {
            while (true) {
                produce(data++);
                Thread.sleep(1000); // 생산 간격을 조절하기 위해 잠시 멈춤
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    private void produce(int i) throws InterruptedException {
        System.out.println("Producing: " + i);
        buffer.put(i); // 데이터를 생산하여 버퍼에 추가
    }
}

class Consumer implements Runnable {
    private BlockingQueue<Integer> buffer;

    public Consumer(BlockingQueue<Integer> buffer) {
        this.buffer = buffer;
    }

    public void run() {
        try {
            while (true) {
                int data = consume();
                Thread.sleep(2000); // 소비 간격을 조절하기 위해 잠시 멈춤
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    private int consume() throws InterruptedException {
        int data = buffer.take(); // 버퍼에서 데이터를 가져옴
        System.out.println("Consuming: " + data);
        return data;
    }
}

public class ThreadPoolProducerConsumer {
    public static void main(String[] args) {
        // 버퍼로 사용할 BlockingQueue 생성
        BlockingQueue<Integer> buffer = new ArrayBlockingQueue<>(10);

        // 스레드 풀 생성
        ExecutorService executor = Executors.newFixedThreadPool(2);

        // 생산자 및 소비자 스레드 생성 및 스레드 풀에 제출
        executor.submit(new Producer(buffer));
        executor.submit(new Consumer(buffer));

        // 스레드 풀 종료
        executor.shutdown();
    }
}
```

위의 예제에서는 BlockingQueue를 사용하여 공유 버퍼를 구현했다. 그리고 Producer 클래스와 Consumer 클래스는 각각 Runnable을 구현하여 생산자 스레드와 소비자 스레드를 정의한다.

생산자는 데이터를 생성하여 BlockingQueue에 추가하고, 소비자는 BlockingQueue에서 데이터를 가져와서 처리한다. 이 두가지 작업은 스레드 풀을 사용하여 병렬적으로 실행된다.

마지막으로 main 메서드에서는 스레드 풀을 생성하고, 생산자 및 소비자 스레드를 스레드 풀에 제출한다. 모든 작업이 완료된 후에는 스레드 풀을 종료한다.
<br><br>



2. **이벤트 기반 시스템 ( Event-Driven Systems )**

    이벤트 발생 시 생산자는 해당 이벤트를 생성하고 큐에 추가하는 역할을 수행하며, 소비자는 큐에서 이벤트를 가져와 처리한다.

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

// 생산자 이벤트 클래스
class ProduceEvent {
    private int data;

    public ProduceEvent(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }
}

// 소비자 이벤트 클래스
class ConsumeEvent {
    private int data;

    public ConsumeEvent(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }
}

// 생산자 클래스
class Producer implements Runnable {
    private BlockingQueue<ProduceEvent> produceQueue;
    private int data = 0;

    public Producer(BlockingQueue<ProduceEvent> produceQueue) {
        this.produceQueue = produceQueue;
    }

    public void run() {
        try {
            while (true) {
                ProduceEvent event = new ProduceEvent(data++);
                produceQueue.put(event);
                System.out.println("Producing: " + event.getData());
                Thread.sleep(1000); // 생산 간격을 조절하기 위해 잠시 멈춤
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

// 소비자 클래스
class Consumer implements Runnable {
    private BlockingQueue<ProduceEvent> produceQueue;
    private BlockingQueue<ConsumeEvent> consumeQueue;

    public Consumer(BlockingQueue<ProduceEvent> produceQueue, BlockingQueue<ConsumeEvent> consumeQueue) {
        this.produceQueue = produceQueue;
        this.consumeQueue = consumeQueue;
    }

    public void run() {
        try {
            while (true) {
                ProduceEvent event = produceQueue.take();
                ConsumeEvent consumeEvent = new ConsumeEvent(event.getData());
                consumeQueue.put(consumeEvent);
                System.out.println("Consuming: " + consumeEvent.getData());
                Thread.sleep(2000); // 소비 간격을 조절하기 위해 잠시 멈춤
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class EventDrivenProducerConsumer {
    public static void main(String[] args) {
        BlockingQueue<ProduceEvent> produceQueue = new LinkedBlockingQueue<>();
        BlockingQueue<ConsumeEvent> consumeQueue = new LinkedBlockingQueue<>();

        // 생산자와 소비자 객체 생성
        Producer producer = new Producer(produceQueue);
        Consumer consumer = new Consumer(produceQueue, consumeQueue);

        // 생산자와 소비자를 각각 스레드로 실행
        Thread producerThread = new Thread(producer);
        Thread consumerThread = new Thread(consumer);
        producerThread.start();
        consumerThread.start();
    }
}
```

생산자가 생산한 데이터를 이벤트 객체로 담아서 큐에 넣고, 소비자는 이벤트를 큐에서 가져와서 처리한다. 

<br><br>


3. **생산자-소비자 문제 해결**

    공유자원에 접근하는 여러 스레드간의 동기화 문제를 해결하기 위해 사용된다. 생산자는 버퍼에 데이터를 추가하고, 소비자는 버퍼에서 데이터를 가져와 처리함으로써 동기화된 데이터 접근을 보장한다.


<br><br><br>

