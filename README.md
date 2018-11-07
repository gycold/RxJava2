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
> **方法使用：**<br><br>以下代码将 Integer 类型的数据转换成 String。
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="groupby">

### 18. groupBy()
> **方法作用：**<br><br>
> **方法预览：**
```
s
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="scan">

### 19. scan()
> **方法作用：**<br><br>
> **方法预览：**
```
s
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="window">

### 20. window()
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
### 组合操作符
<span id="concat">

### 21. concat()
> **方法作用：**<br><br>
> **方法预览：**
```
s
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```

<span id="concatarray">

### 22. concatArray()
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
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
> **方法作用：**<br><br>
> **方法预览：**
```
s
```
> **方法使用：**
```
s
```
> **打印结果：**
```
s
```
