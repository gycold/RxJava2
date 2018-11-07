#  RxJava2 操作一览
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
## 四、操作符一览
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
+ [14. map()](#map)
+ [15. flatMap()](#flatmap)
+ [16. concatMap()](#concatmap)
+ [17. buffer()](#buffer)
+ [18. groupBy()](#groupby)
+ [19. scan()](#scan)
+ [20. window()](#window)
+ [21. concat()](#concat)
+ [22. concatArray()](#concatarray)
+ [23. merge()](#merge)
+ [24. concatArrayDelayError() & mergeArrayDelayError()](#concatarraydelayerror)
+ [25. zip()](#zip)
+ [26. combineLatest() & combineLatestDelayError()](#combinelatest)
+ [27. reduce()](#reduce)
+ [28. collect()](#collect)
+ [29. startWith() & startWithArray()](#startwith)
+ [30. count()](#count)
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
+ [52. filter()](#filter)
+ [53. ofType()](#oftype)
+ [54. skip()](#skip)
+ [55. distinct()](#distinct)
+ [56. distinctUntilChanged()](#distinctuntilchanged)
+ [57. take()](#take)
+ [58. debounce()](#debounce)
+ [59. firstElement() && lastElement()](#firstelement)
+ [60. elementAt() & elementAtOrError()](#elementat)
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

### 1. create()
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

### 2. just()
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

### 3. fromArray()
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

### 4. fromCallable()
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

### 5. fromFuture()
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

### 6. fromIterable()
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

### 7. defer()
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

### 8. timer()
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

### 9. interval()
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

### 10. intervalRange()
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

### 11. range()
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

### 12. rangeLong()
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

### 13. empty() & never() & error()
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

### 14. map()
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

### 15. flatMap()
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

### 16. concatMap()
> **方法作用：**<br>concatMap() 和 flatMap() 基本上是一样的，只不过 concatMap() 转发出来的事件是有序的，而 flatMap() 是无序的。<br><br>
> **方法预览：**
```
public final <R> Observable<R> concatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper)
public final <R> Observable<R> concatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper, int prefetch)
```
> **方法使用：**
```
Observable.fromIterable(personList)
        .flatMap(new Function<Person, ObservableSource<Plan>>() {
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

### 17. buffer()
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

### 18. groupBy()
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

### 19. scan()
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

### 20. window()
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

### 21. concat()
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

### 22. concatArray()
> **方法作用：**<br>与 concat() 作用一样，不过 concatArray() 可以发送多于 4 个被观察者。<br><br>
> **方法预览：**
```
public static <T> Observable<T> concatArray(ObservableSource<? extends T>... sources)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="merge">

### 23. merge()
> **方法作用：**<br>这个方法与 concat() 作用基本一样，区别是 concat() 是串行发送事件，而 merge() 是并行发送事件。<br><br>
> **方法预览：**
```
public static <T> Observable<T> merge(ObservableSource<? extends T> source1, ObservableSource<? extends T> source2, ObservableSource<? extends T> source3, ObservableSource<? extends T> source4)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="concatarraydelayerror">

### 24. concatArrayDelayError() & mergeArrayDelayError()
> **方法作用：**<br>在 concatArray() 和 mergeArray() 两个方法当中，如果其中有一个被观察者发送了一个 Error 事件，那么就会停止发送事件，如果你想 onError() 事件延迟到所有被观察者都发送完事件后再执行的话，就可以使用 concatArrayDelayError() 和 mergeArrayDelayError()<br><br>
> **方法预览：**
```
public static <T> Observable<T> concatArrayDelayError(ObservableSource<? extends T>... sources)
public static <T> Observable<T> mergeArrayDelayError(ObservableSource<? extends T>... sources)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="zip">

### 25. zip()
> **方法作用：**<br>会将多个被观察者合并，根据各个被观察者发送事件的顺序一个个结合起来，最终发送的事件数量会与源 Observable 中最少事件的数量一样。<br><br>
> **方法预览：**
```
public static <T1, T2, R> Observable<R> zip(ObservableSource<? extends T1> source1, ObservableSource<? extends T2> source2, BiFunction<? super T1, ? super T2, ? extends R> zipper)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="combinelatest">

### 26. combineLatest() & combineLatestDelayError()
> **方法作用：**<br>combineLatest() 的作用与 zip() 类似，但是 combineLatest() 发送事件的序列是与发送的时间线有关的，当 combineLatest() 中所有的 Observable 都发送了事件，只要其中有一个 Observable 发送事件，这个事件就会和其他 Observable 最近发送的事件结合起来发送。<br><br>
> **方法预览：**
```
public static <T1, T2, R> Observable<R> combineLatest(ObservableSource<? extends T1> source1, ObservableSource<? extends T2> source2, BiFunction<? super T1, ? super T2, ? extends R> combiner)
.......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="reduce">

### 27. reduce()
> **方法作用：**<br>把所有的操作都操作完成之后在调用一次观察者，把数据一次性输出，与scan的区别：scan是每次操作之后先把数据输出，然后在调用scan的回调函数进行第二次操作<br><br>
> **方法预览：**
```
public final Maybe<T> reduce(BiFunction<T, T, T> reducer)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="collect">

### 28. collect()
> **方法作用：**<br>将数据收集到数据结构当中。<br><br>
> **方法预览：**
```
public final <U> Single<U> collect(Callable<? extends U> initialValueSupplier, BiConsumer<? super U, ? super T> collector)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="startwith">

### 29. startWith() & startWithArray()
> **方法作用：**<br>在发送事件之前追加事件，startWith() 追加一个事件，startWithArray() 可以追加多个事件。追加的事件会先发出。<br><br>
> **方法预览：**
```
public final Observable<T> startWith(T item)
public final Observable<T> startWithArray(T... items)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="count">

### 30. count()
> **方法作用：**<br>返回被观察者发送事件的数量。<br><br>
> **方法预览：**
```
public final Single<Long> count()
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```
---
### 功能操作符
<span id="delay">

### 31. delay()
> **方法作用：**<br>延迟一段事件发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> delay(long delay, TimeUnit unit)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="dooneach">

### 32. doOnEach()
> **方法作用：**<br>Observable 每发送一件事件之前都会先回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnEach(final Consumer<? super Notification<T>> onNotification)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doonnext">

### 33. doOnNext()
> **方法作用：**<br>Observable 每发送 onNext() 之前都会先回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnNext(Consumer<? super T> onNext)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doafternext">

### 34. doAfterNext()
> **方法作用：**<br>Observable 每发送 onNext() 之后都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doAfterNext(Consumer<? super T> onAfterNext)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="dooncomplete">

### 35. doOnComplete()
> **方法作用：**<br>Observable 每发送 onComplete() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnComplete(Action onComplete)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doonerror">

### 36. doOnError()
> **方法作用：**<br>Observable 每发送 onError() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnError(Consumer<? super Throwable> onError)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doonsubscribe">

### 37. doOnSubscribe()
> **方法作用：**<br>Observable 每发送 onSubscribe() 之前都会回调这个方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnSubscribe(Consumer<? super Disposable> onSubscribe)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doondispose">

### 38. doOnDispose()
> **方法作用：**<br>当调用 Disposable 的 dispose() 之后回调该方法。<br><br>
> **方法预览：**
```
public final Observable<T> doOnDispose(Action onDispose)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doonlifecycle">

### 39. doOnLifecycle()
> **方法作用：**<br>在回调 onSubscribe 之前回调该方法的第一个参数的回调方法，可以使用该回调方法决定是否取消订阅。<br><br>
> **方法预览：**
```
public final Observable<T> doOnLifecycle(final Consumer<? super Disposable> onSubscribe, final Action onDispose)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="doonterminate">

### 40. doOnTerminate() & doAfterTerminate()
> **方法作用：**<br>doOnTerminate 是在 onError 或者 onComplete 发送之前回调，而 doAfterTerminate 则是 onError 或者 onComplete 发送之后回调。<br><br>
> **方法预览：**
```
public final Observable<T> doOnTerminate(final Action onTerminate)
public final Observable<T> doAfterTerminate(Action onFinally)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="dofinally">

### 41. doFinally()
> **方法作用：**<br>在所有事件发送完毕之后回调该方法。<br><br>
> **方法预览：**
```
public final Observable<T> doFinally(Action onFinally)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="onerrorreturn">

### 42. onErrorReturn()
> **方法作用：**<br>当接受到一个 onError() 事件之后回调，返回的值会回调 onNext() 方法，并正常结束该事件序列。<br><br>
> **方法预览：**
```
public final Observable<T> onErrorReturn(Function<? super Throwable, ? extends T> valueSupplier)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="onerrorresumenext">

### 43. onErrorResumeNext()
> **方法作用：**<br>当接收到 onError() 事件时，返回一个新的 Observable，并正常结束事件序列。<br><br>
> **方法预览：**
```
public final Observable<T> onErrorResumeNext(Function<? super Throwable, ? extends ObservableSource<? extends T>> resumeFunction)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="onexceptionresumenext">

### 44. onExceptionResumeNext()
> **方法作用：**<br>与 onErrorResumeNext() 作用基本一致，但是这个方法只能捕捉 Exception。<br><br>
> **方法预览：**
```
public final Observable<T> onExceptionResumeNext(final ObservableSource<? extends T> next)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="retry">

### 45. retry()
> **方法作用：**<br>如果出现错误事件，则会重新发送所有事件序列。times 是代表重新发的次数。<br><br>
> **方法预览：**
```
public final Observable<T> retry(long times)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="retryuntil">

### 46. retryUntil()
> **方法作用：**<br>出现错误事件之后，可以通过此方法判断是否继续发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> retryUntil(final BooleanSupplier stop)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="retrywhen">

### 47. retryWhen()
> **方法作用：**<br>当被观察者接收到异常或者错误事件时会回调该方法，这个方法会返回一个新的被观察者。如果返回的被观察者发送 Error 事件则之前的被观察者不会继续发送事件，如果发送正常事件则之前的被观察者会继续不断重试发送事件。<br><br>
> **方法预览：**
```
public final void safeSubscribe(Observer<? super T> s)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="repeat">

### 48. repeat()
> **方法作用：**<br>重复发送被观察者的事件，times 为发送次数。<br><br>
> **方法预览：**
```
public final Observable<T> repeat(long times)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="repeatwhen">

### 49. repeatWhen()
> **方法作用：**<br>这个方法可以会返回一个新的被观察者设定一定逻辑来决定是否重复发送事件。<br><br>
> **方法预览：**
```
public final Observable<T> repeatWhen(final Function<? super Observable<Object>, ? extends ObservableSource<?>> handler)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="subscribeon">

### 50. subscribeOn()
> **方法作用：**<br>指定被观察者的线程，要注意的时，如果多次调用此方法，只有第一次有效。<br><br>
> **方法预览：**
```
public final Observable<T> subscribeOn(Scheduler scheduler)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="observeon">

### 51. observeOn()
> **方法作用：**<br>指定观察者的线程，每指定一次就会生效一次。<br><br>
> **方法预览：**
```
public final Observable<T> observeOn(Scheduler scheduler)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```
---
### 过滤操作符
<span id="filter">

### 52. filter()
> **方法作用：**<br>通过一定逻辑来过滤被观察者发送的事件，如果返回 true 则会发送事件，否则不会发送。<br><br>
> **方法预览：**
```
public final Observable<T> filter(Predicate<? super T> predicate)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="oftype">

### 53. ofType()
> **方法作用：**<br>可以过滤不符合该类型事件<br><br>
> **方法预览：**
```
public final <U> Observable<U> ofType(final Class<U> clazz)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="skip">

### 54. skip()
> **方法作用：**<br>跳过正序某些事件，count 代表跳过事件的数量<br><br>
> **方法预览：**
```
public final Observable<T> skip(long count)
.......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="distinct">

### 55. distinct()
> **方法作用：**<br>过滤事件序列中的重复事件。<br><br>
> **方法预览：**
```
public final Observable<T> distinct()
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="distinctuntilchanged">

### 56. distinctUntilChanged()
> **方法作用：**<br>过滤掉连续重复的事件<br><br>
> **方法预览：**
```
public final Observable<T> distinctUntilChanged()
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="take">

### 57. take()
> **方法作用：**<br>控制观察者接收的事件的数量。<br><br>
> **方法预览：**
```
public final Observable<T> take(long count)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="debounce">

### 58. debounce()
> **方法作用：**<br>如果两件事件发送的时间间隔小于设定的时间间隔则前一件事件就不会发送给观察者。<br><br>
> **方法预览：**
```
public final Observable<T> debounce(long timeout, TimeUnit unit)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="firstelement">

### 59. firstElement() && lastElement()
> **方法作用：**<br>firstElement() 取事件序列的第一个元素，lastElement() 取事件序列的最后一个元素。<br><br>
> **方法预览：**
```
public final Maybe<T> firstElement()
public final Maybe<T> lastElement()
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="elementat">

### 60. elementAt() & elementAtOrError()
> **方法作用：**<br>elementAt() 可以指定取出事件序列中事件，但是输入的 index 超出事件序列的总数的话就不会出现任何结果。这种情况下，你想发出异常信息的话就用 elementAtOrError() 。<br><br>
> **方法预览：**
```
public final Maybe<T> elementAt(long index)
public final Single<T> elementAtOrError(long index)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```
---
### 条件操作符
<span id="all">

### 61. all()
> **方法作用：**<br>判断事件序列是否全部满足某个事件，如果都满足则返回 true，反之则返回 false。<br><br>
> **方法预览：**
```
public final Single<Boolean> all(Predicate<? super T> predicate)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="takewhile">

### 62. takeWhile()
> **方法作用：**<br>可以设置条件，当某个数据满足条件时就会发送该数据，反之则不发送。<br><br>
> **方法预览：**
```
public final Observable<T> takeWhile(Predicate<? super T> predicate)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="skipwhile">

### 63. skipWhile()
> **方法作用：**<br>可以设置条件，当某个数据满足条件时不发送该数据，反之则发送。<br><br>
> **方法预览：**
```
public final Observable<T> skipWhile(Predicate<? super T> predicate)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="takeuntil">

### 64. takeUntil()
> **方法作用：**<br>可以设置条件，当事件满足此条件时，下一次的事件就不会被发送了。<br><br>
> **方法预览：**
```
public final Observable<T> takeUntil(Predicate<? super T> stopPredicate
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="skipuntil">

### 65. skipUntil()
> **方法作用：**<br>当 skipUntil() 中的 Observable 发送事件了，原来的 Observable 才会发送事件给观察者。<br><br>
> **方法预览：**
```
public final <U> Observable<T> skipUntil(ObservableSource<U> other)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="sequenceequal">

### 66. sequenceEqual()
> **方法作用：**<br>判断两个 Observable 发送的事件是否相同。<br><br>
> **方法预览：**
```
public static <T> Single<Boolean> sequenceEqual(ObservableSource<? extends T> source1, ObservableSource<? extends T> source2)
......
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="contains">

### 67. contains()
> **方法作用：**<br>判断事件序列中是否含有某个元素，如果有则返回 true，如果没有则返回 false。<br><br>
> **方法预览：**
```
public final Single<Boolean> contains(final Object element)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="isempty">

### 68. isEmpty()
> **方法作用：**<br>判断事件序列是否为空。<br><br>
> **方法预览：**
```
public final Single<Boolean> isEmpty()
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="amb">

### 69. amb()
> **方法作用：**<br>amb() 要传入一个 Observable 集合，但是只会发送最先发送事件的 Observable 中的事件，其余 Observable 将会被丢弃。<br><br>
> **方法预览：**
```
public static <T> Observable<T> amb(Iterable<? extends ObservableSource<? extends T>> sources)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="defaultIfEmpty">

### 70. defaultifempty()
> **方法作用：**<br>如果观察者只发送一个 onComplete() 事件，则可以利用这个方法发送一个值。<br><br>
> **方法预览：**
```
public final Observable<T> defaultIfEmpty(T defaultItem)
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```
