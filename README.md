#  RxJava2 操作一览
---
## 一、基本概念
+ 被观察者（Observable）<br>
+ 观察者（Observer）<br>
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
| 事件种类 | 作用 |
| :--- | :--- |
| onNext() | 发送该事件时，观察者会回调 onNext() 方法 |
| onError() | 发送该事件时，观察者会回调 onError() 方法，当发送该事件之后，其他事件将不会继续发送|
| onComplete() | 发送该事件时，观察者会回调 onComplete() 方法，当发送该事件之后，其他事件将不会继续发送 |
---
## 四、操作符一览
### 创建操作符
* [1. create()](#create)
### 1. create()<span id=”create”>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：创建一个被观察者**<br>
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
### 2. just()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：创建一个被观察者，并发送事件，发送的事件不可以超过10个以上。**<br>
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
### 3. fromArray()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**
```
public static <T> Observable<T> fromArray(T... items)
```
> **方法使用：这个方法和 just() 类似，只不过 fromArray 可以传入多于10个的变量，并且可以传入一个数组。**
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
### 4. fromCallable()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 5. fromFuture()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 6. fromIterable()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 7. defer()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 8. timer()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 9. interval()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 10. intervalRange()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 11. range()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 12. rangeLong()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 13. empty() & never() & error()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
### 14. range()
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
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
