#  RxJava2.0 操作一览
---
## 一、基本元素
+ 被观察者（Observable）
+ 观察者（Observer）
+ 订阅（subscribe）
---
## 二、使用步骤
#### 1. 创建被观察者：
```
Observable observable = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        Log.d(TAG, "=========================currentThread name: " + Thread.currentThread().getName());
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
});
```
#### 2. 创建观察者：
```
Observer observer = new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {
        Log.d(TAG, "======================onSubscribe");
    }
    @Override
    public void onNext(Integer integer) {
        Log.d(TAG, "======================onNext " + integer);
    }
    @Override
    public void onError(Throwable e) {
        Log.d(TAG, "======================onError");
    }
    @Override
    public void onComplete() {
        Log.d(TAG, "======================onComplete");
    }
};
```
#### 3. 订阅:
```
observable.subscribe(observer);
```
---
## 三、被观察者发送的事件
| 事件种类 | 方法作用 |
| :--- | :--- |
| onNext() | 发送该事件时，观察者会回调 onNext() 方法 |
| onError() | 发送该事件时，观察者会回调 onError() 方法，当发送该事件之后，其他事件将不会继续发送|
| onComplete() | 发送该事件时，观察者会回调 onComplete() 方法，当发送该事件之后，其他事件将不会继续发送 |
---

<span id="目录">

## 四、操作符一览
+ [创建操作符](#创建操作符)
  + [1. create()](#create)
  + [2. just()](#just)
  + [3. fromArray()](#fromarray)
  + [4. fromCallable()](#fromcallable)
  + [5. fromFuture()](#fromfuture)
  + [6. fromIterable()](#fromiterable)
  + [7. defer()](#defer)
  + [8. timer()](#timer)
  + [9. interval()](#interval)
  + [10. intervalRange()](#intervalrange)
  + [11. range()](#range)
  + [12. rangeLong()](#rangelong)
  + [13. empty() & never() & error()](#empty)
+ [转换操作符](#转换操作符)
  + [14. map()](#map)
  + [15. flatMap()](#flatmap)
  + [16. concatMap()](#concatmap)
  + [17. buffer()](#buffer)
  + [18. groupBy()](#groupby)
  + [19. scan()](#scan)
  + [20. window()](#window)
+ [组合操作符](#组合操作符)
  + [21. concat()](#concat)
  + [22. concatArray()](#concatarray)
  + [23. merge() & mergeArray()](#merge)
  + [24. concatArrayDelayError() & mergeArrayDelayError()](#concatarraydelayerror)
  + [25. zip()](#zip)
  + [26. combineLatest() & combineLatestDelayError()](#combinelatest)
  + [27. reduce()](#reduce)
  + [28. collect()](#collect)
  + [29. startWith() & startWithArray()](#startwith)
  + [30. count()](#count)
+ [功能操作符](#功能操作符)
  + [31. delay()](#delay)
  + [32. doOnEach()](#dooneach)
  + [33. doOnNext()](#doonnext)
  + [34. doAfterNext()](#doafternext)
  + [35. doOnComplete()](#dooncomplete)
  + [36. doOnError()](#doonerror)
  + [37. doOnSubscribe()](#doonsubscribe)
  + [38. doOnDispose()](#doondispose)
  + [39. doOnLifecycle()](#doonlifecycle)
  + [40. doOnTerminate() & doAfterTerminate()](#doonterminate)
  + [41. doFinally()](#dofinally)
  + [42. onErrorReturn()](#onerrorreturn)
  + [43. onErrorResumeNext()](#onerrorresumenext)
  + [44. onExceptionResumeNext()](#onexceptionresumenext)
  + [45. retry()](#retry)
  + [46. retryUntil()](#retryuntil)
  + [47. retryWhen()](#retrywhen)
  + [48. repeat()](#repeat)
  + [49. repeatWhen()](#repeatwhen)
  + [50. subscribeOn()](#subscribeon)
  + [51. observeOn()](#observeon)
+ [过滤操作符](#过滤操作符)
  + [52. filter()](#filter)
  + [53. ofType()](#oftype)
  + [54. skip()](#skip)
  + [55. distinct()](#distinct)
  + [56. distinctUntilChanged()](#distinctuntilchanged)
  + [57. take()](#take)
  + [58. debounce()](#debounce)
  + [59. firstElement() && lastElement()](#firstelement)
  + [60. elementAt() & elementAtOrError()](#elementat)
+ [条件操作符](#条件操作符)
  + [61. all()](#all)
  + [62. takeWhile()](#takewhile)
  + [63. skipWhile()](#skipwhile)
  + [64. takeUntil()](#takeuntil)
  + [65. skipUntil()](#skipuntil)
  + [66. sequenceEqual()](#sequenceequal)
  + [67. contains()](#contains)
  + [68. isEmpty()](#isempty)
  + [69. amb()](#amb)
  + [70. defaultIfEmpty()](#defaultifempty)
---
### 创建操作符

<span id="create">

### 1. create() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>创建一个被观察者<br><br>
> **方法预览：**
```
public static <T> Observable<T> create(ObservableOnSubscribe<T> source)
```
> **方法使用：**
```
Observable<String> observable = Observable.create(new ObservableOnSubscribe<String>() {
    @Override
    public void subscribe(ObservableEmitter<String> e) throws Exception {
        e.onNext("Hello Observer");
        e.onComplete();
    }
});
Observer<String> observer = new Observer<String>() {
    @Override
    public void onSubscribe(Disposable d) {
    }

    @Override
    public void onNext(String s) {
        Log.d("chan", "=============onNext " + s);
    }

    @Override
    public void onError(Throwable e) {
    }

    @Override
    public void onComplete() {
        Log.d("chan", "=============onComplete ");
    }
};
observable.subscribe(observer);
```
> **打印结果：**
```
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onNext Hello Observer
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onComplete
```

<span id="just">

### 2. just() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>创建一个被观察者，并发送事件，发送的事件不可以超过10个以上。<br><br>
> **方法预览：**
```
public static <T> Observable<T> just(T item)
......
public static <T> Observable<T> just(T item1, T item2, T item3, T item4, T item5, T item6, T item7, T item8, T item9, T item10)
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "=================onSubscribe");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "=================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "=================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "=================onComplete ");
            }
        });

```
> **打印结果：**
```
11-06 16:07:24.291 25045-25045/com.css.rxjava D/RxJava: =================onSubscribe
11-06 16:07:24.291 25045-25045/com.css.rxjava D/RxJava: =================onNext 1
11-06 16:07:24.291 25045-25045/com.css.rxjava D/RxJava: =================onNext 2
11-06 16:07:24.291 25045-25045/com.css.rxjava D/RxJava: =================onNext 3
11-06 16:07:24.291 25045-25045/com.css.rxjava D/RxJava: =================onComplete
```

<span id="fromarray">

### 3. fromArray() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这个方法和 just() 类似，只不过 fromArray 可以传入多于10个的变量，并且可以传入一个数组。<br><br>
> **方法预览：**
```
public static <T> Observable<T> fromArray(T... items)
```
> **方法使用：**
```
Observable.fromArray(array)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "=================onSubscribe");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "=================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "=================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "=================onComplete ");
            }
        });

```
> **打印结果：**
```
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onSubscribe
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onNext 1
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onNext 2
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onNext 3
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onNext 4
11-06 16:08:56.401 26163-26163/com.css.rxjava D/RxJava: =================onComplete
```

<span id="fromcallable">

### 4. fromCallable() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这里的 Callable 是 java.util.concurrent 中的 Callable，Callable 和 Runnable 的用法基本一致，只是它会返回一个结果值，这个结果值就是发给观察者的。<br><br>
> **方法预览：**
```
public static <T> Observable<T> fromCallable(Callable<? extends T> supplier)
```
> **方法使用：**
```
Observable.fromCallable(new Callable<Integer>() {
    @Override
    public Integer call() throws Exception {
        return 1;
    }
})
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "================accept " + integer);
            }
        });
```
> **打印结果：**
```
11-06 17:56:40.001 27747-27747/com.css.rxjava D/RxJava: ================accept 1
```

<span id="fromfuture">

### 5. fromFuture() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>参数中的 Future 是 java.util.concurrent 中的 Future，Future 的作用是增加了 cancel() 等方法操作 Callable，它可以通过 get() 方法来获取 Callable 返回的值。<br><br>
> **方法预览：**
```
public static <T> Observable<T> fromFuture(Future<? extends T> future)
```
> **方法使用：**
```
FutureTask<String> futureTask = new FutureTask<>(new Callable<String>() {
            @Override
            public String call() throws Exception {
                Log.d(TAG, "CallableDemo is Running");
                return "返回结果";
            }
        });
Observable.fromFuture(futureTask)
        .doOnSubscribe(new Consumer<Disposable>() {
            @Override
            public void accept(Disposable disposable) throws Exception {
                futureTask.run();
            }
        })
        .subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                Log.d(TAG, "================accept " + s);
            }
        });
```
> **打印结果：**
```
11-06 17:58:41.181 29584-29584/com.css.rxjava D/RxJava: CallableDemo is Running
11-06 17:58:41.181 29584-29584/com.css.rxjava D/RxJava: ================accept 返回结果
```

<span id="fromiterable">

### 6. fromIterable() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>直接发送一个 List 集合数据给观察者<br><br>
> **方法预览：**
```
public static <T> Observable<T> fromIterable(Iterable<? extends T> source)
```
> **方法使用：**
```
List<Integer> list = new ArrayList<>();
        list.add(0);
        list.add(1);
        list.add(2);
        list.add(3);
Observable.fromIterable(list)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "=================onSubscribe");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "=================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "=================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "=================onComplete ");
            }
        });
```
> **打印结果：**
```
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onSubscribe
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onNext 0
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onNext 1
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onNext 2
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onNext 3
11-06 18:03:29.701 30092-30092/com.css.rxjava D/RxJava: =================onComplete
```

<span id="defer">

### 7. defer() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这个方法的方法作用就是直到被观察者被订阅后才会创建被观察者。<br><br>
> **方法预览：**
```
public static <T> Observable<T> defer(Callable<? extends ObservableSource<? extends T>> supplier)
```
> **方法使用：**
```
// i 要定义为成员变量
Integer i = 100;
Observable<Integer> observable = Observable.defer(new Callable<ObservableSource<? extends Integer>>() {
    @Override
    public ObservableSource<? extends Integer> call() throws Exception {
        return Observable.just(i);
    }
});
i = 200;
Observer observer = new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {
    }

    @Override
    public void onNext(Integer integer) {
        Log.d(TAG, "================onNext " + integer);
    }

    @Override
    public void onError(Throwable e) {
    }

    @Override
    public void onComplete() {
    }
};
observable.subscribe(observer);
i = 300;
observable.subscribe(observer);
```
> **打印结果：**
```
11-06 18:05:42.101 30425-30425/com.css.rxjava D/RxJava: ================onNext 200
11-06 18:05:42.101 30425-30425/com.css.rxjava D/RxJava: ================onNext 300
```

<span id="timer">

### 8. timer() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当到指定时间后就会发送一个 0L 的值给观察者。<br><br>
> **方法预览：**
```
public static Observable<Long> timer(long delay, TimeUnit unit)
......
```
> **方法使用：**
```
Observable.timer(2, TimeUnit.SECONDS)
        .subscribe(new Observer<Long>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(Long aLong) {
                Log.d(TAG, "===============onNext " + aLong);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-06 18:13:16.091 30836-30906/com.css.rxjava D/RxJava: ===============onNext 0
```

<span id="interval">

### 9. interval() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>每隔一段时间就会发送一个事件，这个事件是从0开始，不断增1的数字。<br><br>
> **方法预览：**
```
public static Observable<Long> interval(long period, TimeUnit unit)
public static Observable<Long> interval(long initialDelay, long period, TimeUnit unit)
......
```
> **方法使用：**
```
Observable.interval(4, TimeUnit.SECONDS)
        .subscribe(new Observer<Long>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==============onSubscribe ");
            }

            @Override
            public void onNext(Long aLong) {
                Log.d(TAG, "==============onNext " + aLong);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-06 18:15:40.301 31087-31114/com.css.rxjava D/RxJava: ==============onNext 0
11-06 18:15:44.301 31087-31114/com.css.rxjava D/RxJava: ==============onNext 1
11-06 18:15:48.301 31087-31114/com.css.rxjava D/RxJava: ==============onNext 2
11-06 18:15:52.301 31087-31114/com.css.rxjava D/RxJava: ==============onNext 3
```

<span id="intervalrange">

### 10. intervalRange() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以指定发送事件的开始值和数量，其他与 interval() 的功能一样。<br><br>
> **方法预览：**
```
public static Observable<Long> intervalRange(long start, long count, long initialDelay, long period, TimeUnit unit)
public static Observable<Long> intervalRange(long start, long count, long initialDelay, long period, TimeUnit unit, Scheduler scheduler)
```
> **方法使用：**
```
Observable.intervalRange(2, 5, 2, 1, TimeUnit.SECONDS)
        .subscribe(new Observer<Long>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==============onSubscribe ");
            }

            @Override
            public void onNext(Long aLong) {
                Log.d(TAG, "==============onNext " + aLong);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-06 18:18:09.341 31266-31266/com.css.rxjava D/RxJava: ==============onSubscribe
11-06 18:18:11.341 31266-31360/com.css.rxjava D/RxJava: ==============onNext 2
11-06 18:18:12.341 31266-31360/com.css.rxjava D/RxJava: ==============onNext 3
11-06 18:18:13.341 31266-31360/com.css.rxjava D/RxJava: ==============onNext 4
11-06 18:18:14.341 31266-31360/com.css.rxjava D/RxJava: ==============onNext 5
11-06 18:18:15.341 31266-31360/com.css.rxjava D/RxJava: ==============onNext 6
```

<span id="range">

### 11. range() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>同时发送一定范围的事件序列。<br><br>
> **方法预览：**
```
public static Observable<Integer> range(final int start, final int count)
```
> **方法使用：**
```
Observable.range(2, 5)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==============onSubscribe ");
            }

            @Override
            public void onNext(Integer aLong) {
                Log.d(TAG, "==============onNext " + aLong);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onSubscribe
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onNext 2
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onNext 3
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onNext 4
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onNext 5
11-06 18:19:45.541 31540-31540/com.css.rxjava D/RxJava: ==============onNext 6
```

<span id="rangelong">

### 12. rangeLong() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>方法作用与 range() 一样，只是数据类型为 Long<br><br>
> **方法预览：**
```
public static Observable<Long> rangeLong(long start, long count)
```
> **方法使用：**
```
同上
```
> **打印结果：**
```
参照上条
```

<span id="empty">

### 13. empty() & never() & error() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>
+ empty() ：直接发送 onComplete() 事件
+ never()：不发送任何事件
+ error()：发送 onError() 事件<br><br>
> **方法预览：**
```
public static <T> Observable<T> empty()
public static <T> Observable<T> never()
public static <T> Observable<T> error(final Throwable exception)
```
> **方法使用：**
```
Observable.empty()
        .subscribe(new Observer<Object>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe");
            }

            @Override
            public void onNext(Object o) {
                Log.d(TAG, "==================onNext");
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError " + e);
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete");
            }
        });
```
> **打印结果：**
```
11-06 18:37:05.331 32077-32077/com.css.rxjava D/RxJava: ==================onSubscribe
11-06 18:37:05.331 32077-32077/com.css.rxjava D/RxJava: ==================onComplete
```
---
### 转换操作符
<span id="map">

### 14. map() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>map 可以将被观察者发送的数据类型转变成其他的类型<br><br>
> **方法预览：**
```
public final <R> Observable<R> map(Function<? super T, ? extends R> mapper)
```
> **方法使用：**<br>以下代码将 Integer 类型的数据转换成 String。
```
Observable.just(1, 2, 3)
        .map(new Function<Integer, String>() {
            @Override
            public String apply(Integer integer) throws Exception {
                return "I'm " + integer;
            }
        })
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.e(TAG, "===================onSubscribe");
            }

            @Override
            public void onNext(String s) {
                Log.e(TAG, "===================onNext " + s);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-07 10:43:17.761 7161-7161/com.css.rxjava E/RxJava: ===================onSubscribe
11-07 10:43:17.761 7161-7161/com.css.rxjava E/RxJava: ===================onNext I'm 1
11-07 10:43:17.761 7161-7161/com.css.rxjava E/RxJava: ===================onNext I'm 2
11-07 10:43:17.761 7161-7161/com.css.rxjava E/RxJava: ===================onNext I'm 3
```

<span id="flatmap">

### 15. flatMap() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这个方法可以将事件序列中的元素进行整合加工，返回一个新的被观察者。<br><br>
> **方法预览：**
```
public final <R> Observable<R> flatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper)
......
```
> **方法使用：**<br>flatMap() 其实与 map() 类似，但是 flatMap() 返回的是一个 Observerable。
```
public class Person {
    private String name;
    private List<Plan> planList = new ArrayList<>();
    public Person(String name, List<Plan> planList) {
        this.name = name;
        this.planList = planList;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public List<Plan> getPlanList() {
        return planList;
    }
    public void setPlanList(List<Plan> planList) {
        this.planList = planList;
    }
}
```
```
public class Plan {
    private String time;
    private String content;
    private List<String> actionList = new ArrayList<>();
    public Plan(String time, String content) {
        this.time = time;
        this.content = content;
    }
    public String getTime() {
        return time;
    }
    public void setTime(String time) {
        this.time = time;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public List<String> getActionList() {
        return actionList;
    }
    public void setActionList(List<String> actionList) {
        this.actionList = actionList;
    }
}
```
```
Observable.fromIterable(personList)
        .flatMap(new Function<Person, ObservableSource<Plan>>() {
            @Override
            public ObservableSource<Plan> apply(Person person) {
                return Observable.fromIterable(person.getPlanList());
            }
        })
        .flatMap(new Function<Plan, ObservableSource<String>>() {
            @Override
            public ObservableSource<String> apply(Plan plan) throws Exception {
                return Observable.fromIterable(plan.getActionList());
            }
        })
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(String s) {
                Log.d(TAG, "==================action: " + s);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
会依次打印出Plan中的actionList：
11-07 11:00:22.191 8533-8533/com.css.rxjava D/RxJava: ==================action: aaaaa
11-07 11:00:22.191 8533-8533/com.css.rxjava D/RxJava: ==================action: bbbbb
```

<span id="concatmap">

### 16. concatMap() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>concatMap() 和 flatMap() 基本上是一样的，只不过 concatMap() 转发出来的事件是有序的，而 flatMap() 是无序的。<br><br>
> **方法预览：**
```
public final <R> Observable<R> concatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper)
public final <R> Observable<R> concatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper, int prefetch)
```
> **方法使用：**
```
Observable.fromIterable(personList)
        .concatMap(new Function<Person, ObservableSource<Plan>>() {
            @Override
            public ObservableSource<Plan> apply(Person person) {
                if ("chan".equals(person.getName())) {
                    return Observable.fromIterable(person.getPlanList()).delay(10, TimeUnit.MILLISECONDS);
                }
                return Observable.fromIterable(person.getPlanList());
            }
        })
        .subscribe(new Observer<Plan>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(Plan plan) {
                Log.d(TAG, "==================plan " + plan.getContent());
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-07 11:06:03.941 8764-8764/com.css.rxjava D/RxJava: ==================plan 撸代码
11-07 11:06:03.941 8764-8764/com.css.rxjava D/RxJava: ==================plan 代码审核
11-07 11:06:03.941 8764-8764/com.css.rxjava D/RxJava: ==================plan 吃饭
11-07 11:06:03.941 8764-8764/com.css.rxjava D/RxJava: ==================plan 干活
```

<span id="buffer">

### 17. buffer() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>从需要发送的事件当中获取一定数量的事件，并将这些事件放到缓冲区当中一并发出。<br><br>
> **方法预览：**
```
public final Observable<List<T>> buffer(int count, int skip)
......
```
> **方法使用：**<br>buffer 有两个参数，一个是 count，另一个 skip。count 缓冲区元素的数量，skip 就代表缓冲区满了之后，发送下一次事件序列的时候要跳过多少元素。
```
Observable.just(1, 2, 3, 4, 5)
        .buffer(2, 1)
        .subscribe(new Observer<List<Integer>>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(List<Integer> integers) {
                Log.d(TAG, "================缓冲区大小： " + integers.size());
                for (Integer i : integers) {
                    Log.d(TAG, "================元素： " + i);
                }
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
================缓冲区大小： 2
================元素： 1
================元素： 2
================缓冲区大小： 2
================元素： 2
================元素： 3
================缓冲区大小： 2
================元素： 3
================元素： 4
================缓冲区大小： 2
================元素： 4
================元素： 5
================缓冲区大小： 1
================元素： 5
```

<span id="groupby">

### 18. groupBy() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>将发送的数据进行分组，每个分组都会返回一个被观察者。<br><br>
> **方法预览：**
```
public final <K> Observable<GroupedObservable<K, T>> groupBy(Function<? super T, ? extends K> keySelector)
```
> **方法使用：**
```
Observable.just(5, 2, 3, 4, 1, 6, 8, 9, 7, 10)
        .groupBy(new Function<Integer, Integer>() {
            @Override
            public Integer apply(Integer integer) throws Exception {
                return integer % 3;
            }
        })
        .subscribe(new Observer<GroupedObservable<Integer, Integer>>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "====================onSubscribe ");
            }

            @Override
            public void onNext(final GroupedObservable<Integer, Integer> integerIntegerGroupedObservable) {
                Log.d(TAG, "====================onNext ");
                integerIntegerGroupedObservable.subscribe(new Observer<Integer>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "====================GroupedObservable onSubscribe ");
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "====================GroupedObservable onNext  groupName: " + integerIntegerGroupedObservable.getKey() + " value: " + integer);
                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "====================GroupedObservable onError ");
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "====================GroupedObservable onComplete ");
                    }
                });
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "====================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "====================onComplete ");
            }
        });
```
> **打印结果：**<br>在 groupBy() 方法返回的参数是分组的名字，每返回一个值，那就代表会创建一个组，以上的代码就是将1~10的数据分成3组，可以看到返回的结果中是有3个组的(0、1、2三组，已存在的组会直接发射数据)。
```
====================onSubscribe
====================onNext
====================GroupedObservable onSubscribe
====================GroupedObservable onNext  groupName: 2 value: 5
====================GroupedObservable onNext  groupName: 2 value: 2
====================onNext
====================GroupedObservable onSubscribe
====================GroupedObservable onNext  groupName: 0 value: 3
====================onNext
====================GroupedObservable onSubscribe
====================GroupedObservable onNext  groupName: 1 value: 4
====================GroupedObservable onNext  groupName: 1 value: 1
====================GroupedObservable onNext  groupName: 0 value: 6
====================GroupedObservable onNext  groupName: 2 value: 8
====================GroupedObservable onNext  groupName: 0 value: 9
====================GroupedObservable onNext  groupName: 1 value: 7
====================GroupedObservable onNext  groupName: 1 value: 10
====================GroupedObservable onComplete
====================GroupedObservable onComplete
====================GroupedObservable onComplete
====================onComplete
```

<span id="scan">

### 19. scan() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>将Observable的结果在BiFunction扫描一遍后交给Observer使用，scan最大的功用是在BiFunction的apply里面做一次计算，有条件、有筛选的输出最终结果<br><br>
> **方法预览：**
```
public final Observable<T> scan(BiFunction<T, T, T> accumulator)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4, 5)
        .scan(new BiFunction<Integer, Integer, Integer>() {
            @Override
            public Integer apply(Integer integer, Integer integer2) throws Exception {
                Log.d(TAG, "====================apply ");
                Log.d(TAG, "====================integer " + integer);
                Log.d(TAG, "====================integer2 " + integer2);
                return integer + integer2;
            }
        })
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "====================accept " + integer);
            }
        });
```
> **打印结果：**<br>由于在第一次scan时候，源输入队列中的数据只有“1”，就只返回一个“1”，后面又成对的数据源，则成对的扫描。
```
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================accept 1
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================apply
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================i 1[这是accept结果]
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================j 2
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================accept 3
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================apply
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================i 3[这是accept结果]
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================j 3
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================accept 6
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================apply
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================i 6[这是accept结果]
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================j 4
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================accept 10
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================apply
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================i 10[这是accept结果]
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================j 5
11-07 14:16:34.391 14689-14689/com.css.rxjava D/RxJava: ====================accept 15
```

<span id="window">

### 20. window() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>发送指定数量的事件时，就将这些事件分为一组。window 中的 count 的参数就是代表指定的数量，例如将 count 指定为2，那么每发2个数据就会将这2个数据分成一组。<br><br>
> **方法预览：**
```
public final Observable<Observable<T>> window(long count)
......
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4, 5)
        .window(2)
        .subscribe(new Observer<Observable<Integer>>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "=====================onSubscribe ");
            }

            @Override
            public void onNext(Observable<Integer> integerObservable) {
                integerObservable.subscribe(new Observer<Integer>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "=====================integerObservable onSubscribe ");
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "=====================integerObservable onNext " + integer);
                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "=====================integerObservable onError ");
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "=====================integerObservable onComplete ");
                    }
                });
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "=====================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "=====================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================onSubscribe
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onSubscribe
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onNext 1
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onNext 2
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onComplete
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onSubscribe
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onNext 3
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onNext 4
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onComplete
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onSubscribe
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onNext 5
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================integerObservable onComplete
11-07 14:31:34.171 15219-15219/com.css.rxjava D/RxJava: =====================onComplete
```
---
### 组合操作符
<span id="concat">

### 21. concat() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以将多个观察者组合在一起，然后按照之前发送顺序发送事件。需要注意的是，concat() 最多只可以发送4个事件。<br><br>
> **方法预览：**
```
public static <T> Observable<T> concat(ObservableSource<? extends T> source1, ObservableSource<? extends T> source2, ObservableSource<? extends T> source3, ObservableSource<? extends T> source4)
......
```
> **方法使用：**
```
Observable.concat(Observable.just(1, 2),
        Observable.just(3, 4),
        Observable.just(5, 6),
        Observable.just(7, 8))
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 1
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 2
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 3
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 4
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 5
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 6
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 7
11-07 14:33:52.221 15589-15589/com.css.rxjava D/RxJava: ================onNext 8
```

<span id="concatarray">

### 22. concatArray() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>与 concat() 作用一样，不过 concatArray() 可以发送多于 4 个被观察者。<br><br>
> **方法预览：**
```
public static <T> Observable<T> concatArray(ObservableSource<? extends T>... sources)
```
> **方法使用：**
```
Observable.concatArray(Observable.just(1, 2),
        Observable.just(3, 4),
        Observable.just(5, 6),
        Observable.just(7, 8),
        Observable.just(9, 10))
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
================onNext 1
================onNext 2
================onNext 3
================onNext 4
================onNext 5
================onNext 6
================onNext 7
================onNext 8
================onNext 9
================onNext 10
```

<span id="merge">

### 23. merge() & mergeArray() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这个方法与 concat() 作用基本一样，区别是 concat() 是串行发送事件，而 merge() 是并行发送事件。mergeArray() 与 merge() 的作用是一样的，只是它可以发送4个以上的被观察者，这里就不再赘述了。<br><br>
> **方法预览：**
```
public static <T> Observable<T> merge(ObservableSource<? extends T> source1, ObservableSource<? extends T> source2, ObservableSource<? extends T> source3, ObservableSource<? extends T> source4)
......
```
> **方法使用：**
```
Observable.merge(
        Observable.interval(1, TimeUnit.SECONDS).map(new Function<Long, String>() {
            @Override
            public String apply(Long aLong) throws Exception {
                return "A" + aLong;
            }
        }),
        Observable.interval(1, TimeUnit.SECONDS).map(new Function<Long, String>() {
            @Override
            public String apply(Long aLong) throws Exception {
                return "B" + aLong;
            }
        }))
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(String s) {
                Log.d(TAG, "=====================onNext " + s);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-07 15:09:15.771 16325-16385/com.css.rxjava D/RxJava: =====================onNext A0
11-07 15:09:15.771 16325-16386/com.css.rxjava D/RxJava: =====================onNext B0
11-07 15:09:16.771 16325-16385/com.css.rxjava D/RxJava: =====================onNext A1
11-07 15:09:16.771 16325-16386/com.css.rxjava D/RxJava: =====================onNext B1
11-07 15:09:17.771 16325-16385/com.css.rxjava D/RxJava: =====================onNext A2
11-07 15:09:17.771 16325-16386/com.css.rxjava D/RxJava: =====================onNext B2
......
```

<span id="concatarraydelayerror">

### 24. concatArrayDelayError() & mergeArrayDelayError() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>在 concatArray() 和 mergeArray() 两个方法当中，如果其中有一个被观察者发送了一个 Error 事件，那么就会停止发送事件，如果你想 onError() 事件延迟到所有被观察者都发送完事件后再执行的话，就可以使用 concatArrayDelayError() 和 mergeArrayDelayError()<br><br>
> **方法预览：**
```
public static <T> Observable<T> concatArrayDelayError(ObservableSource<? extends T>... sources)
public static <T> Observable<T> mergeArrayDelayError(ObservableSource<? extends T>... sources)
```
> **方法使用：**
```
Observable.concatArray(
        Observable.create(new ObservableOnSubscribe<Integer>() {
            @Override
            public void subscribe(ObservableEmitter<Integer> e) throws Exception {
                e.onNext(1);
                e.onError(new NumberFormatException());
            }
        }), Observable.just(2, 3, 4))
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "===================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "===================onError ");
            }

            @Override
            public void onComplete() {
            }
        });
```
> **打印结果：**
```
11-07 15:14:07.001 17348-17348/com.css.rxjava D/RxJava: ===================onNext 1
11-07 15:14:07.001 17348-17348/com.css.rxjava D/RxJava: ===================onError
```

<span id="zip">

### 25. zip() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>会将多个被观察者合并，根据各个被观察者发送事件的顺序一个个结合起来，最终发送的事件数量会与源 Observable 中最少事件的数量一样。<br><br>
> **方法预览：**
```
public static <T1, T2, R> Observable<R> zip(ObservableSource<? extends T1> source1, ObservableSource<? extends T2> source2, BiFunction<? super T1, ? super T2, ? extends R> zipper)
......
```
> **方法使用：**
```
Observable.zip(Observable.intervalRange(1, 5, 1, 1, TimeUnit.SECONDS)
                .map(new Function<Long, String>() {
                    @Override
                    public String apply(Long aLong) throws Exception {
                        String s1 = "A" + aLong;
                        Log.d(TAG, "===================A 发送的事件 " + s1);
                        return s1;
                    }
                }),
        Observable.intervalRange(1, 6, 1, 1, TimeUnit.SECONDS)
                .map(new Function<Long, String>() {
                    @Override
                    public String apply(Long aLong) throws Exception {
                        String s2 = "B" + aLong;
                        Log.d(TAG, "===================B 发送的事件 " + s2);
                        return s2;
                    }
                }),
        new BiFunction<String, String, String>() {
            @Override
            public String apply(String s, String s2) throws Exception {
                String res = s + s2;
                return res;
            }
        })
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "===================onSubscribe ");
            }

            @Override
            public void onNext(String s) {
                Log.d(TAG, "===================onNext " + s);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "===================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "===================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:15:25.241 17578-17605/com.css.rxjava D/RxJava: ===================A 发送的事件 A1
11-07 15:15:25.241 17578-17606/com.css.rxjava D/RxJava: ===================B 发送的事件 B1
11-07 15:15:25.251 17578-17606/com.css.rxjava D/RxJava: ===================onNext A1B1
11-07 15:15:26.241 17578-17605/com.css.rxjava D/RxJava: ===================A 发送的事件 A2
11-07 15:15:26.241 17578-17606/com.css.rxjava D/RxJava: ===================B 发送的事件 B2
11-07 15:15:26.251 17578-17606/com.css.rxjava D/RxJava: ===================onNext A2B2
11-07 15:15:27.241 17578-17605/com.css.rxjava D/RxJava: ===================A 发送的事件 A3
11-07 15:15:27.241 17578-17606/com.css.rxjava D/RxJava: ===================B 发送的事件 B3
11-07 15:15:27.241 17578-17606/com.css.rxjava D/RxJava: ===================onNext A3B3
11-07 15:15:28.251 17578-17605/com.css.rxjava D/RxJava: ===================A 发送的事件 A4
11-07 15:15:28.251 17578-17606/com.css.rxjava D/RxJava: ===================B 发送的事件 B4
11-07 15:15:28.251 17578-17606/com.css.rxjava D/RxJava: ===================onNext A4B4
11-07 15:15:29.241 17578-17605/com.css.rxjava D/RxJava: ===================A 发送的事件 A5
11-07 15:15:29.241 17578-17606/com.css.rxjava D/RxJava: ===================B 发送的事件 B5
11-07 15:15:29.251 17578-17606/com.css.rxjava D/RxJava: ===================onNext A5B5
11-07 15:15:29.251 17578-17606/com.css.rxjava D/RxJava: ===================onComplete
```

<span id="combinelatest">

### 26. combineLatest() & combineLatestDelayError() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>combineLatest() 的作用与 zip() 类似，但是 combineLatest() 发送事件的序列是与发送的时间线有关的，当 combineLatest() 中所有的 Observable 都发送了事件，只要其中有一个 Observable 发送事件，这个事件就会和其他 Observable 最近发送的事件结合起来发送。<br><br>
> **方法预览：**
```
public static <T1, T2, R> Observable<R> combineLatest(ObservableSource<? extends T1> source1, ObservableSource<? extends T2> source2, BiFunction<? super T1, ? super T2, ? extends R> combiner)
.......
```
> **方法使用：**
```
Observable.combineLatest(
        Observable.intervalRange(1, 4, 1, 1, TimeUnit.SECONDS)
                .map(new Function<Long, String>() {
                    @Override
                    public String apply(Long aLong) throws Exception {
                        String s1 = "A" + aLong;
                        Log.d(TAG, "===================A 发送的事件 " + s1);
                        return s1;
                    }
                }),
        Observable.intervalRange(1, 5, 2, 2, TimeUnit.SECONDS)
                .map(new Function<Long, String>() {
                    @Override
                    public String apply(Long aLong) throws Exception {
                        String s2 = "B" + aLong;
                        Log.d(TAG, "===================B 发送的事件 " + s2);
                        return s2;
                    }
                }),
        new BiFunction<String, String, String>() {
            @Override
            public String apply(String s, String s2) throws Exception {
                String res = s + s2;
                return res;
            }
        })
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "===================onSubscribe ");
            }

            @Override
            public void onNext(String s) {
                Log.d(TAG, "===================最终接收到的事件 " + s);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "===================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "===================onComplete ");
            }
        });
```
> **打印结果：**<br>当发送 A1 事件之后，因为 B 并没有发送任何事件，所以根本不会发生结合。当 B 发送了 B1 事件之后，就会与 A 最近发送的事件 A2 结合成 A2B1，这样只要后面一有被观察者发送事件，这个事件就会与其他被观察者最近发送的事件结合起来。
```
11-07 15:16:42.661 17793-17820/com.css.rxjava D/RxJava: ===================A 发送的事件 A1
11-07 15:16:43.651 17793-17820/com.css.rxjava D/RxJava: ===================A 发送的事件 A2
11-07 15:16:43.661 17793-17821/com.css.rxjava D/RxJava: ===================B 发送的事件 B1
11-07 15:16:43.661 17793-17821/com.css.rxjava D/RxJava: ===================最终接收到的事件 A2B1
11-07 15:16:44.651 17793-17820/com.css.rxjava D/RxJava: ===================A 发送的事件 A3
11-07 15:16:44.661 17793-17820/com.css.rxjava D/RxJava: ===================最终接收到的事件 A3B1
11-07 15:16:45.661 17793-17820/com.css.rxjava D/RxJava: ===================A 发送的事件 A4
11-07 15:16:45.661 17793-17820/com.css.rxjava D/RxJava: ===================最终接收到的事件 A4B1
11-07 15:16:45.661 17793-17821/com.css.rxjava D/RxJava: ===================B 发送的事件 B2
11-07 15:16:45.661 17793-17821/com.css.rxjava D/RxJava: ===================最终接收到的事件 A4B2
11-07 15:16:47.661 17793-17821/com.css.rxjava D/RxJava: ===================B 发送的事件 B3
11-07 15:16:47.661 17793-17821/com.css.rxjava D/RxJava: ===================最终接收到的事件 A4B3
11-07 15:16:49.661 17793-17821/com.css.rxjava D/RxJava: ===================B 发送的事件 B4
11-07 15:16:49.661 17793-17821/com.css.rxjava D/RxJava: ===================最终接收到的事件 A4B4
11-07 15:16:51.661 17793-17821/com.css.rxjava D/RxJava: ===================B 发送的事件 B5
11-07 15:16:51.661 17793-17821/com.css.rxjava D/RxJava: ===================最终接收到的事件 A4B5
11-07 15:16:51.661 17793-17821/com.css.rxjava D/RxJava: ===================onComplete
```

<span id="reduce">

### 27. reduce() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>把所有的操作都操作完成之后再调用一次观察者，把数据一次性输出，与scan的区别：scan是每次操作之后先把数据输出，然后在调用scan的回调函数进行第二次操作<br><br>
> **方法预览：**
```
public final Maybe<T> reduce(BiFunction<T, T, T> reducer)
```
> **方法使用：**
```
Observable.just(0, 1, 2, 3)
        .reduce(new BiFunction<Integer, Integer, Integer>() {
            @Override
            public Integer apply(Integer i, Integer j) throws Exception {
                int res = i + j;
                Log.d(TAG, "====================i " + i);
                Log.d(TAG, "====================j " + j);
                Log.d(TAG, "====================res " + res);
                return res;
            }
        })
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "==================accept " + integer);
            }
        });
```
> **打印结果：**<br>从结果可以看到，其实就是前2个数据聚合之后，然后再与后1个数据进行聚合，一直到没有数据为止。
```
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================i 0
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================j 1
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================res 1
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================i 1
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================j 2
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================res 3
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================i 3
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================j 3
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ====================res 6
11-07 15:22:15.311 18629-18629/com.css.rxjava D/RxJava: ==================accept 6
```

<span id="collect">

### 28. collect() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>将数据收集到数据结构当中。<br><br>
> **方法预览：**
```
public final <U> Single<U> collect(Callable<? extends U> initialValueSupplier, BiConsumer<? super U, ? super T> collector)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
                .collect(new Callable<ArrayList<Integer>>() {
                             @Override
                             public ArrayList<Integer> call() throws Exception {
                                 return new ArrayList<>();
                             }
                         },
                        new BiConsumer<ArrayList<Integer>, Integer>() {
                            @Override
                            public void accept(ArrayList<Integer> integers, Integer integer) throws Exception {
                                integers.add(integer);
                            }
                        })
                .subscribe(new Consumer<ArrayList<Integer>>() {
                    @Override
                    public void accept(ArrayList<Integer> integers) throws Exception {
                        Log.d(TAG, "===============accept " + integers);
                    }
                });
```
> **打印结果：**
```
11-07 15:24:43.761 18864-18864/com.css.rxjava D/RxJava: ===============accept [1, 2, 3, 4]
```

<span id="startwith">

### 29. startWith() & startWithArray() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>在发送事件之前追加事件，startWith() 追加一个事件，startWithArray() 可以追加多个事件。追加的事件会先发出。<br><br>
> **方法预览：**
```
public final Observable<T> startWith(T item)
public final Observable<T> startWithArray(T... items)
```
> **方法使用：**
```
Observable.just(5, 6, 7)
        .startWithArray(2, 3, 4)
        .startWith(1)
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "================accept " + integer);
            }
        });
```
> **打印结果：**
```
================accept 1
================accept 2
================accept 3
================accept 4
================accept 5
================accept 6
================accept 7
```

<span id="count">

### 30. count() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>返回被观察者发送事件的数量。<br><br>
> **方法预览：**
```
public final Single<Long> count()
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .count()
        .subscribe(new Consumer<Long>() {
            @Override
            public void accept(Long aLong) throws Exception {
                Log.d(TAG, "=======================aLong " + aLong);
            }
        });
```
> **打印结果：**
```
=======================aLong 3
```
---
### 功能操作符
<span id="delay">

### 31. delay() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>延迟一段事件发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> delay(long delay, TimeUnit unit)
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .delay(2, TimeUnit.SECONDS)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "=======================onSubscribe");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "=======================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "=======================onSubscribe");
            }
        });
```
> **打印结果：**
```
=======================onSubscribe
=======================onNext 1
=======================onNext 2
=======================onNext 3
=======================onSubscribe
```

<span id="dooneach">

### 32. doOnEach() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送一件事件之前都会先回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnEach(final Consumer<? super Notification<T>> onNotification)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        //      e.onError(new NumberFormatException());
        e.onComplete();
    }
})
        .doOnEach(new Consumer<Notification<Integer>>() {
            @Override
            public void accept(Notification<Integer> integerNotification) throws Exception {
                Log.d(TAG, "==================doOnEach " + integerNotification.getValue());
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================doOnEach 1
==================onNext 1
==================doOnEach 2
==================onNext 2
==================doOnEach 3
==================onNext 3
==================doOnEach null
==================onComplete
```

<span id="doonnext">

### 33. doOnNext() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送 onNext() 之前都会先回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnNext(Consumer<? super T> onNext)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doOnNext(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "==================doOnNext " + integer);
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================doOnNext 1
==================onNext 1
==================doOnNext 2
==================onNext 2
==================doOnNext 3
==================onNext 3
==================onComplete
```

<span id="doafternext">

### 34. doAfterNext() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送 onNext() 之后都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doAfterNext(Consumer<? super T> onAfterNext)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doAfterNext(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "==================doAfterNext " + integer);
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================doAfterNext 1
==================onNext 2
==================doAfterNext 2
==================onNext 3
==================doAfterNext 3
==================onComplete
```

<span id="dooncomplete">

### 35. doOnComplete() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送 onComplete() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnComplete(Action onComplete)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doOnComplete(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doOnComplete ");
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================onNext 2
==================onNext 3
==================doOnComplete
==================onComplete
```

<span id="doonerror">

### 36. doOnError() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送 onError() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnError(Consumer<? super Throwable> onError)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new NullPointerException());
    }
})
        .doOnError(new Consumer<Throwable>() {
            @Override
            public void accept(Throwable throwable) throws Exception {
                Log.d(TAG, "==================doOnError " + throwable);
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================onNext 2
==================onNext 3
==================doOnError java.lang.NullPointerException
==================onError
```

<span id="doonsubscribe">

### 37. doOnSubscribe() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>Observable 每发送 onSubscribe() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnSubscribe(Consumer<? super Disposable> onSubscribe)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doOnSubscribe(new Consumer<Disposable>() {
            @Override
            public void accept(Disposable disposable) throws Exception {
                Log.d(TAG, "==================doOnSubscribe ");
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================doOnSubscribe
==================onSubscribe
==================onNext 1
==================onNext 2
==================onNext 3
==================onComplete
```

<span id="doondispose">

### 38. doOnDispose() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当调用 Disposable 的 dispose() 之后回调该方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnDispose(Action onDispose)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doOnDispose(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doOnDispose ");
            }
        })
        .subscribe(new Observer<Integer>() {
            private Disposable d;

            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
                this.d = d;
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
                d.dispose();
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================doOnDispose
```

<span id="doonlifecycle">

### 39. doOnLifecycle() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>在回调 onSubscribe 之前回调该方法的第一个参数的回调方法，可以使用该回调方法决定是否取消订阅。<br><br>
> **方法预览：**
```
public final Observable<T> doOnLifecycle(final Consumer<? super Disposable> onSubscribe, final Action onDispose)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doOnLifecycle(new Consumer<Disposable>() {
            @Override
            public void accept(Disposable disposable) throws Exception {
                Log.d(TAG, "==================doOnLifecycle accept");
            }
        }, new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doOnLifecycle Action");
            }
        })
        .doOnDispose(
                new Action() {
                    @Override
                    public void run() throws Exception {
                        Log.d(TAG, "==================doOnDispose Action");
                    }
                })
        .subscribe(new Observer<Integer>() {
            private Disposable d;

            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
                this.d = d;
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
                d.dispose();
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:43:09.281 19313-19313/com.css.rxjava D/RxJava: ==================doOnLifecycle accept
11-07 15:43:09.281 19313-19313/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:43:09.281 19313-19313/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:43:09.281 19313-19313/com.css.rxjava D/RxJava: ==================doOnDispose Action
11-07 15:43:09.281 19313-19313/com.css.rxjava D/RxJava: ==================doOnLifecycle Action
```
可以看到当在 onNext() 方法进行取消订阅操作后，doOnDispose() 和 doOnLifecycle() 都会被回调。如果在 doOnLifecycle 内部的accept方法内进行取消订阅disposable.dispose()，则打印结果：
```
11-07 15:44:24.811 19503-19503/com.css.rxjava D/RxJava: ==================doOnLifecycle accept
11-07 15:44:24.811 19503-19503/com.css.rxjava D/RxJava: ==================onSubscribe
```

<span id="doonterminate">

### 40. doOnTerminate() & doAfterTerminate() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>doOnTerminate 是在 onError 或者 onComplete 发送之前回调，而 doAfterTerminate 则是 onError 或者 onComplete 发送之后回调。<br><br>
> **方法预览：**
```
public final Observable<T> doOnTerminate(final Action onTerminate)
public final Observable<T> doAfterTerminate(Action onFinally)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
//      e.onError(new NullPointerException());
        e.onComplete();
    }
})
        .doOnTerminate(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doOnTerminate ");
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================doOnTerminate
11-07 15:46:19.931 19741-19741/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="dofinally">

### 41. doFinally() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>在所有事件发送完毕之后回调该方法。doFinally() 和 doAfterTerminate() 到底有什么区别？区别就是在于取消订阅，如果取消订阅之后 doAfterTerminate() 就不会被回调，而 doFinally() 无论怎么样都会被回调，且都会在事件序列的最后。<br><br>
> **方法预览：**
```
public final Observable<T> doFinally(Action onFinally)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .doFinally(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doFinally ");
            }
        })
        .doOnDispose(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doOnDispose ");
            }
        })
        .doAfterTerminate(new Action() {
            @Override
            public void run() throws Exception {
                Log.d(TAG, "==================doAfterTerminate ");
            }
        })
        .subscribe(new Observer<Integer>() {
            private Disposable d;

            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
                this.d = d;
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
                d.dispose();
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================doOnDispose
==================doFinally
```

<span id="onerrorreturn">

### 42. onErrorReturn() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当接受到一个 onError() 事件之后回调，返回的值会回调 onNext() 方法，并正常结束该事件序列。<br><br>
> **方法预览：**
```
public final Observable<T> onErrorReturn(Function<? super Throwable, ? extends T> valueSupplier)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new NullPointerException());
    }
})
        .onErrorReturn(new Function<Throwable, Integer>() {
            @Override
            public Integer apply(Throwable throwable) throws Exception {
                Log.d(TAG, "==================onErrorReturn " + throwable);
                return 404;
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onErrorReturn java.lang.NullPointerException
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onNext 404
11-07 15:48:45.291 19964-19964/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="onerrorresumenext">

### 43. onErrorResumeNext() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当接收到 onError() 事件时，返回一个新的 Observable，并正常结束事件序列。<br><br>
> **方法预览：**
```
public final Observable<T> onErrorResumeNext(Function<? super Throwable, ? extends ObservableSource<? extends T>> resumeFunction)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new NullPointerException());
    }
})
        .onErrorResumeNext(new Function<Throwable, ObservableSource<? extends Integer>>() {
            @Override
            public ObservableSource<? extends Integer> apply(Throwable throwable) throws Exception {
                Log.d(TAG, "==================onErrorResumeNext " + throwable);
                return Observable.just(4, 5, 6);
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onErrorResumeNext java.lang.NullPointerException
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 4
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 5
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onNext 6
11-07 15:49:37.131 20160-20160/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="onexceptionresumenext">

### 44. onExceptionResumeNext() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>与 onErrorResumeNext() 作用基本一致，但是这个方法只能捕捉 Exception。<br><br>
> **方法预览：**
```
public final Observable<T> onExceptionResumeNext(final ObservableSource<? extends T> next)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new Error("404"));
    }
})
        .onExceptionResumeNext(new Observable<Integer>() {
            @Override
            protected void subscribeActual(Observer<? super Integer> observer) {
                observer.onNext(333);
                observer.onComplete();
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:50:29.171 20419-20419/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:50:29.171 20419-20419/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:50:29.171 20419-20419/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:50:29.171 20419-20419/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:50:29.171 20419-20419/com.css.rxjava D/RxJava: ==================onError
```
从打印结果可以知道，观察者收到 onError() 事件，证明 onErrorResumeNext() 不能捕捉 Error 事件。<br>
将被观察者的 e.onError(new Error("404")) 改为 e.onError(new Exception("404"))，从打印结果可以知道，这个方法成功捕获 Exception 事件：
```
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onNext 333
11-07 15:52:54.421 20670-20670/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="retry">

### 45. retry() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>如果出现错误事件，则会重新发送所有事件序列。times 是代表重新发的次数。<br><br>
> **方法预览：**
```
public final Observable<T> retry(long times)
......
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new Exception("404"));
    }
})
        .retry(2)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
==================onSubscribe
==================onNext 1
==================onNext 2
==================onNext 3
==================onNext 1
==================onNext 2
==================onNext 3
==================onNext 1
==================onNext 2
==================onNext 3
==================onError
```

<span id="retryuntil">

### 46. retryUntil() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>出现错误事件之后，可以通过此方法判断是否继续发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> retryUntil(final BooleanSupplier stop)
```
> **方法使用：**
```
int i = 0;//类内部变量
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onError(new Exception("404"));
    }
})
        .retryUntil(new BooleanSupplier() {
            @Override
            public boolean getAsBoolean() throws Exception {
                if (i == 6) {
                    return true;
                }
                return false;
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 15:59:18.361 21020-21020/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 15:59:18.361 21020-21020/com.css.rxjava D/RxJava: ==================onNext 1
11-07 15:59:18.361 21020-21020/com.css.rxjava D/RxJava: ==================onNext 2
11-07 15:59:18.361 21020-21020/com.css.rxjava D/RxJava: ==================onNext 3
11-07 15:59:18.361 21020-21020/com.css.rxjava D/RxJava: ==================onError
```

<span id="retrywhen">

### 47. retryWhen() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当被观察者接收到异常或者错误事件时会回调该方法，这个方法会返回一个新的被观察者。如果返回的被观察者发送 Error 事件则之前的被观察者不会继续发送事件，如果发送正常事件则之前的被观察者会继续不断重试发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> retryWhen(final Function<? super Observable<Throwable>, ? extends ObservableSource<?>> handler)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<String>() {
            @Override
            public void subscribe(ObservableEmitter<String> e) throws Exception {
                e.onNext("g");
                e.onNext("y");
                e.onNext("c");
                e.onError(new Exception("404"));
                e.onNext("haha");
            }
        })
                .retryWhen(new Function<Observable<Throwable>, ObservableSource<?>>() {
                    @Override
                    public ObservableSource<?> apply(Observable<Throwable> throwableObservable) throws Exception {
                        return throwableObservable.flatMap(new Function<Throwable, ObservableSource<?>>() {
                            @Override
                            public ObservableSource<?> apply(Throwable throwable) throws Exception {
                                if (!throwable.toString().equals("java.lang.Exception: 404")) {
                                    return Observable.just("可以忽略的异常");
                                } else {
                                    return Observable.error(new Throwable("终止啦"));
                                }
                            }
                        });
                    }
                })
                .subscribe(new Observer<String>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "==================onSubscribe ");
                    }

                    @Override
                    public void onNext(String s) {
                        Log.d(TAG, "==================onNext " + s);
                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "==================onError " + e.toString());
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "==================onComplete ");
                    }
                });
```
> **打印结果：**
```
11-07 16:07:56.681 21417-21417/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:07:56.691 21417-21417/com.css.rxjava D/RxJava: ==================onNext g
11-07 16:07:56.691 21417-21417/com.css.rxjava D/RxJava: ==================onNext y
11-07 16:07:56.691 21417-21417/com.css.rxjava D/RxJava: ==================onNext c
11-07 16:07:56.691 21417-21417/com.css.rxjava D/RxJava: ==================onError java.lang.Throwable: 终止啦
```
将 onError(new Exception("404")) 改为 onError(new Exception("303")) 打印结果：
```
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext g
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext y
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext c
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext c
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext g
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext y
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext y
11-07 16:14:59.081 21997-21997/com.css.rxjava D/RxJava: ==================onNext g
```

<span id="repeat">

### 48. repeat() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>重复发送被观察者的事件，times 为发送次数。<br><br>
> **方法预览：**
```
public final Observable<T> repeat(long times)
......
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .repeat(2)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "===================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "===================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "===================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onSubscribe
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 1
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 2
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 3
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 1
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 2
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onNext 3
11-07 16:18:49.631 22353-22353/com.css.rxjava D/RxJava: ===================onComplete
```

<span id="repeatwhen">

### 49. repeatWhen() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>这个方法可以会返回一个新的被观察者设定一定逻辑来决定是否重复发送事件。这里分三种情况，如果新的被观察者返回 onComplete 或者 onError 事件，则旧的被观察者不会继续发送事件。如果被观察者返回其他事件，则会重复发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> repeatWhen(final Function<? super Observable<Object>, ? extends ObservableSource<?>> handler)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .repeatWhen(new Function<Observable<Object>, ObservableSource<?>>() {
            @Override
            public ObservableSource<?> apply(Observable<Object> objectObservable) throws Exception {
                return Observable.empty();
                //  return Observable.error(new Exception("404"));
                //  return Observable.just(4); null;
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "===================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "===================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "===================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "===================onComplete ");
            }
        });
```
> **打印结果：**<br>return Observable.empty();
```
05-24 11:44:33.486 9379-9379/com.example.rxjavademo D/chan: ===================onSubscribe
05-24 11:44:33.487 9379-9379/com.example.rxjavademo D/chan: ===================onComplete
```
发送 onError 打印结果：return Observable.error(new Exception("404"));
```
11-07 16:23:38.781 23165-23165/com.css.rxjava D/RxJava: ===================onSubscribe
11-07 16:23:38.781 23165-23165/com.css.rxjava D/RxJava: ===================onError
```
发送其他事件的打印结果：return Observable.just(4);
```
11-07 16:24:16.581 23351-23351/com.css.rxjava D/RxJava: ===================onSubscribe
11-07 16:24:16.581 23351-23351/com.css.rxjava D/RxJava: ===================onNext 1
11-07 16:24:16.581 23351-23351/com.css.rxjava D/RxJava: ===================onNext 2
11-07 16:24:16.581 23351-23351/com.css.rxjava D/RxJava: ===================onNext 3
11-07 16:24:16.581 23351-23351/com.css.rxjava D/RxJava: ===================onComplete
```

<span id="subscribeon">

### 50. subscribeOn() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>指定被观察者的线程，需要注意的是，如果多次调用此方法，只有第一次有效。<br><br>
> **方法预览：**
```
public final Observable<T> subscribeOn(Scheduler scheduler)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        Log.d(TAG, "=========================currentThread name: " + Thread.currentThread().getName());
        e.onNext(1);
        e.onNext(2);
        e.onNext(3);
        e.onComplete();
    }
})
        .subscribeOn(Schedulers.computation())
        .subscribeOn(Schedulers.newThread())
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "======================onSubscribe");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "======================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "======================onError");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "======================onComplete");
            }
        });
```
> **打印结果：**<br>
可以看到第二次调动的 subscribeOn(Schedulers.newThread()) 并没有效果。
```
11-07 16:28:52.411 23846-23846/com.css.rxjava D/RxJava: ======================onSubscribe
11-07 16:28:52.421 23846-24000/com.css.rxjava D/RxJava: =========================currentThread name: RxComputationThreadPool-2
11-07 16:28:52.421 23846-24000/com.css.rxjava D/RxJava: ======================onNext 1
11-07 16:28:52.421 23846-24000/com.css.rxjava D/RxJava: ======================onNext 2
11-07 16:28:52.421 23846-24000/com.css.rxjava D/RxJava: ======================onNext 3
11-07 16:28:52.421 23846-24000/com.css.rxjava D/RxJava: ======================onComplete
```

<span id="observeon">

### 51. observeOn() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>指定观察者的线程，每指定一次就会生效一次。<br><br>
> **方法预览：**
```
public final Observable<T> observeOn(Scheduler scheduler)
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .observeOn(Schedulers.newThread())
        .flatMap(new Function<Integer, ObservableSource<String>>() {
            @Override
            public ObservableSource<String> apply(Integer integer) throws Exception {
                Log.d(TAG, "======================flatMap Thread name " + Thread.currentThread().getName());
                return Observable.just("chan" + integer);
            }
        })
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "======================onSubscribe");
            }

            @Override
            public void onNext(String s) {
                Log.d(TAG, "======================onNext Thread name " + Thread.currentThread().getName());
                Log.d(TAG, "======================onNext " + s);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "======================onError");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "======================onComplete");
            }
        });
```
> **打印结果：**
```
11-07 16:30:20.821 24169-24169/com.css.rxjava D/RxJava: ======================onSubscribe
11-07 16:30:20.831 24169-24209/com.css.rxjava D/RxJava: ======================flatMap Thread name RxNewThreadScheduler-1
11-07 16:30:20.831 24169-24209/com.css.rxjava D/RxJava: ======================flatMap Thread name RxNewThreadScheduler-1
11-07 16:30:20.831 24169-24209/com.css.rxjava D/RxJava: ======================flatMap Thread name RxNewThreadScheduler-1
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext Thread name main
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext chan1
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext Thread name main
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext chan2
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext Thread name main
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onNext chan3
11-07 16:30:20.861 24169-24169/com.css.rxjava D/RxJava: ======================onComplete
```
<br>
下表总结了 RxJava 中的调度器：<br>

| 调度器 | 作用 |
| :--- | :--- |
| Schedulers.computation() | 用于使用计算任务，如事件循环和回调处理 |
| Schedulers.immediate() | 当前线程 |
| Schedulers.io() | 用于 IO 密集型任务，如果异步阻塞 IO 操作 |
| Schedulers.newThread() | 创建一个新的线程 |
| AndroidSchedulers.mainThread() | Android 的 UI 线程，用于操作 UI |
---
### 过滤操作符
<span id="filter">

### 52. filter() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>通过一定逻辑来过滤被观察者发送的事件，如果返回 true 则会发送事件，否则不会发送。<br><br>
> **方法预览：**
```
public final Observable<T> filter(Predicate<? super T> predicate)
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .filter(new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) throws Exception {
                return integer < 2;
            }
        })
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:37:38.901 24636-24636/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:37:38.901 24636-24636/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:37:38.901 24636-24636/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="oftype">

### 53. ofType() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以过滤不符合该类型事件<br><br>
> **方法预览：**
```
public final <U> Observable<U> ofType(final Class<U> clazz)
```
> **方法使用：**
```
Observable.just(1, 2, 3, "chan", "zhide")
        .ofType(Integer.class)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:38:15.821 24881-24881/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:38:15.821 24881-24881/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:38:15.821 24881-24881/com.css.rxjava D/RxJava: ==================onNext 2
11-07 16:38:15.821 24881-24881/com.css.rxjava D/RxJava: ==================onNext 3
11-07 16:38:15.821 24881-24881/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="skip">

### 54. skip() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>跳过正序某些事件，count 代表跳过事件的数量<br><br>
> **方法预览：**
```
public final Observable<T> skip(long count)
.......
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .skip(2)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:38:42.141 25111-25111/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:38:42.141 25111-25111/com.css.rxjava D/RxJava: ==================onNext 3
11-07 16:38:42.141 25111-25111/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="distinct">

### 55. distinct() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>过滤事件序列中的重复事件。<br><br>
> **方法预览：**
```
public final Observable<T> distinct()
```
> **方法使用：**
```
Observable.just(1, 2, 3, 3, 2, 1)
        .distinct()
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:39:43.401 25285-25285/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:39:43.401 25285-25285/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:39:43.401 25285-25285/com.css.rxjava D/RxJava: ==================onNext 2
11-07 16:39:43.401 25285-25285/com.css.rxjava D/RxJava: ==================onNext 3
11-07 16:39:43.401 25285-25285/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="distinctuntilchanged">

### 56. distinctUntilChanged() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>过滤掉连续重复的事件<br><br>
> **方法预览：**
```
public final Observable<T> distinctUntilChanged()
```
> **方法使用：**
```
Observable.just(1, 2, 3, 3, 2, 1)
        .distinctUntilChanged()
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onNext 2
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onNext 3
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onNext 2
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:40:13.711 25478-25478/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="take">

### 57. take() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>控制观察者接收的事件的数量。<br><br>
> **方法预览：**
```
public final Observable<T> take(long count)
......
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4, 5)
        .take(3)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "==================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                i += integer;
                Log.d(TAG, "==================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "==================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "==================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:41:00.111 25705-25705/com.css.rxjava D/RxJava: ==================onSubscribe
11-07 16:41:00.111 25705-25705/com.css.rxjava D/RxJava: ==================onNext 1
11-07 16:41:00.111 25705-25705/com.css.rxjava D/RxJava: ==================onNext 2
11-07 16:41:00.111 25705-25705/com.css.rxjava D/RxJava: ==================onNext 3
11-07 16:41:00.111 25705-25705/com.css.rxjava D/RxJava: ==================onComplete
```

<span id="debounce">

### 58. debounce() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>如果两件事件发送的时间间隔小于设定的时间间隔则前一件事件就不会发送给观察者。<br><br>
> **方法预览：**
```
public final Observable<T> debounce(long timeout, TimeUnit unit)
......
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onNext(1);
        Thread.sleep(900);
        e.onNext(2);
    }
})
        .debounce(1, TimeUnit.SECONDS)
        .subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "===================onSubscribe ");
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "===================onNext " + integer);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "===================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "===================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:41:33.041 25992-25992/com.css.rxjava D/RxJava: ===================onSubscribe
11-07 16:41:34.941 25992-26019/com.css.rxjava D/RxJava: ===================onNext 2
```

<span id="firstelement">

### 59. firstElement() && lastElement() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>firstElement() 取事件序列的第一个元素，lastElement() 取事件序列的最后一个元素。<br><br>
> **方法预览：**
```
public final Maybe<T> firstElement()
public final Maybe<T> lastElement()
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
        .lastElement()
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "====================lastElement " + integer);
            }
        });
```
> **打印结果：**
```
====================firstElement 1
====================lastElement 4
```

<span id="elementat">

### 60. elementAt() & elementAtOrError() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>elementAt() 可以指定取出事件序列中事件，但是输入的 index 超出事件序列的总数的话就不会出现任何结果。这种情况下，你想发出异常信息的话就用 elementAtOrError() 。<br><br>
> **方法预览：**
```
public final Maybe<T> elementAt(long index)
public final Single<T> elementAtOrError(long index)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
        .firstElement()
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "====================firstElement " + integer);
            }
        });
Observable.just(1, 2, 3, 4)
        .lastElement()
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "====================lastElement " + integer);
            }
        });
```
> **打印结果：**
```
====================firstElement 1
====================lastElement 4
```
---
### 条件操作符
<span id="all">

### 61. all() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>判断事件序列是否全部满足某个事件，如果都满足则返回 true，反之则返回 false。<br><br>
> **方法预览：**
```
public final Single<Boolean> all(Predicate<? super T> predicate)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
        .all(new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) throws Exception {
                return integer < 5;
            }
        })
        .subscribe(new Consumer<Boolean>() {
            @Override
            public void accept(Boolean aBoolean) throws Exception {
                Log.d(TAG, "==================aBoolean " + aBoolean);
            }
        });
```
> **打印结果：**
```
==================aBoolean true
```

<span id="takewhile">

### 62. takeWhile() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以设置条件，当某个数据满足条件时就会发送该数据，反之则不发送。<br><br>
> **方法预览：**
```
public final Observable<T> takeWhile(Predicate<? super T> predicate)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
        .takeWhile(new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) throws Exception {
                return integer < 3;
            }
        })
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "========================integer " + integer);
            }
        });
```
> **打印结果：**
```
========================integer 1
========================integer 2
```

<span id="skipwhile">

### 63. skipWhile() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以设置条件，当某个数据满足条件时不发送该数据，反之则发送。<br><br>
> **方法预览：**
```
public final Observable<T> skipWhile(Predicate<? super T> predicate)
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4)
        .skipWhile(new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) throws Exception {
                return integer < 3;
            }
        })
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "========================integer " + integer);
            }
        });
```
> **打印结果：**
```
========================integer 3
========================integer 4
```

<span id="takeuntil">

### 64. takeUntil() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>可以设置条件，当事件满足此条件时，下一次的事件就不会被发送了。<br><br>
> **方法预览：**
```
public final Observable<T> takeUntil(Predicate<? super T> stopPredicate
```
> **方法使用：**
```
Observable.just(1, 2, 3, 4, 5, 6)
        .takeUntil(new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) throws Exception {
                return integer > 3;
            }
        })
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "========================integer " + integer);
            }
        });
```
> **打印结果：**
```
11-07 16:47:29.151 26631-26631/com.css.rxjava D/RxJava: ========================integer 1
11-07 16:47:29.151 26631-26631/com.css.rxjava D/RxJava: ========================integer 2
11-07 16:47:29.151 26631-26631/com.css.rxjava D/RxJava: ========================integer 3
11-07 16:47:29.151 26631-26631/com.css.rxjava D/RxJava: ========================integer 4
```

<span id="skipuntil">

### 65. skipUntil() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>当 skipUntil() 中的 Observable 发送事件了，原来的 Observable 才会发送事件给观察者。<br><br>
> **方法预览：**
```
public final <U> Observable<T> skipUntil(ObservableSource<U> other)
```
> **方法使用：**
```
Observable.intervalRange(1, 5, 0, 1, TimeUnit.SECONDS)
        .skipUntil(Observable.intervalRange(6, 5, 3, 1, TimeUnit.SECONDS))
        .subscribe(new Observer<Long>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.d(TAG, "========================onSubscribe ");
            }

            @Override
            public void onNext(Long along) {
                Log.d(TAG, "========================onNext " + along);
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "========================onError ");
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "========================onComplete ");
            }
        });
```
> **打印结果：**
```
11-07 16:48:46.211 27680-27726/com.css.rxjava D/RxJava: ========================onNext 4
11-07 16:48:47.211 27680-27726/com.css.rxjava D/RxJava: ========================onNext 5
11-07 16:48:47.211 27680-27726/com.css.rxjava D/RxJava: ========================onComplete
```

<span id="sequenceequal">

### 66. sequenceEqual() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>判断两个 Observable 发送的事件是否相同。<br><br>
> **方法预览：**
```
public static <T> Single<Boolean> sequenceEqual(ObservableSource<? extends T> source1, ObservableSource<? extends T> source2)
......
```
> **方法使用：**
```
Observable.sequenceEqual(Observable.just(1, 2, 3),
        Observable.just(1, 2, 3))
        .subscribe(new Consumer<Boolean>() {
            @Override
            public void accept(Boolean aBoolean) throws Exception {
                Log.d(TAG, "========================onNext " + aBoolean);
            }
        });
```
> **打印结果：**
```
========================onNext true
```

<span id="contains">

### 67. contains() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>判断事件序列中是否含有某个元素，如果有则返回 true，如果没有则返回 false。<br><br>
> **方法预览：**
```
public final Single<Boolean> contains(final Object element)
```
> **方法使用：**
```
Observable.just(1, 2, 3)
        .contains(3)
        .subscribe(new Consumer<Boolean>() {
            @Override
            public void accept(Boolean aBoolean) throws Exception {
                Log.d(TAG, "========================onNext " + aBoolean);
            }
        });
```
> **打印结果：**
```
========================onNext true
```

<span id="isempty">

### 68. isEmpty() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>判断事件序列是否为空。<br><br>
> **方法预览：**
```
public final Single<Boolean> isEmpty()
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onComplete();
    }
})
        .isEmpty()
        .subscribe(new Consumer<Boolean>() {
            @Override
            public void accept(Boolean aBoolean) throws Exception {
                Log.d(TAG, "========================onNext " + aBoolean);
            }
        });
```
> **打印结果：**
```
========================onNext true
```

<span id="amb">

### 69. amb() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>amb() 要传入一个 Observable 集合，但是只会发送最先发送事件的 Observable 中的事件，其余 Observable 将会被丢弃。<br><br>
> **方法预览：**
```
public static <T> Observable<T> amb(Iterable<? extends ObservableSource<? extends T>> sources)
```
> **方法使用：**
```
ArrayList<Observable<Long>> list = new ArrayList<>();
        list.add(Observable.intervalRange(1, 5, 2, 1, TimeUnit.SECONDS));
        list.add(Observable.intervalRange(6, 5, 0, 1, TimeUnit.SECONDS));
        Observable.amb(list)
                .subscribe(new Consumer<Long>() {
                    @Override
                    public void accept(Long aLong) throws Exception {
                        Log.d(TAG, "========================aLong " + aLong);
                    }
                });
```
> **打印结果：**
```
11-07 16:51:59.351 28077-28108/com.css.rxjava D/RxJava: ========================aLong 6
11-07 16:52:00.351 28077-28108/com.css.rxjava D/RxJava: ========================aLong 7
11-07 16:52:01.351 28077-28108/com.css.rxjava D/RxJava: ========================aLong 8
11-07 16:52:02.351 28077-28108/com.css.rxjava D/RxJava: ========================aLong 9
11-07 16:52:03.351 28077-28108/com.css.rxjava D/RxJava: ========================aLong 10
```

<span id="defaultifempty">

### 70. defaultIfEmpty() &#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;&#8195;[回到目录](#目录)
> **方法作用：**<br>如果观察者只发送一个 onComplete() 事件，则可以利用这个方法发送一个值。<br><br>
> **方法预览：**
```
public final Observable<T> defaultIfEmpty(T defaultItem)
```
> **方法使用：**
```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> e) throws Exception {
        e.onComplete();
    }
})
        .defaultIfEmpty(666)
        .subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.d(TAG, "========================onNext " + integer);
            }
        });
```
> **打印结果：**
```
11-07 16:54:56.571 28303-28303/com.css.rxjava D/RxJava: ========================onNext 666
```
