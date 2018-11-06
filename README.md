#  <font color=#2EB5AA size=6>RxJava2 操作一览</font>
---
### <font color=#2EB5AA size=5>一、基本概念</font>
> 被观察者（Observable）<br>
> 观察者（Observer）<br>
> 订阅（subscribe）
---
### <font color=#2EB5AA size=5>二、使用步骤</font>
1. <font color=#2EB5AA size=4>创建被观察者：</font>
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
2. <font color=#2EB5AA size=4>创建观察者：</font>
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
3. <font color=#2EB5AA size=4>订阅:</font>
```
observable.subscribe(observer);
```
---
### <font color=#2EB5AA size=5>三、被观察者发送的事件</font>
| 事件种类 | 作用 |
| :--- | :--- |
| onNext() | 发送该事件时，观察者会回调 onNext() 方法 |
| onError() | 发送该事件时，观察者会回调 onError() 方法，当发送该事件之后，其他事件将不会继续发送|
| onComplete() | 发送该事件时，观察者会回调 onComplete() 方法，当发送该事件之后，其他事件将不会继续发送 |
---
### <font color=#2EB5AA size=5>四、操作符一览</font>
#### <font color=#2EB5AA size=4>创建操作符</font>
1. <font color=#2EB5AA size=4>create()</font><br>
> <font color=#2EB5AA size=3>作用&#160;&#160;&#160;&#160;&#160;&#160;&#8201;：创建一个被观察者</font><br>
> <font color=#2EB5AA size=3>方法预览：</font>```public static <T> Observable<T> create(ObservableOnSubscribe<T> source)```<br>
> <font color=#2EB5AA size=3>方法使用：</font>
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
        Log.d("chan","=============onNext " + s);
    }
    @Override
    public void onError(Throwable e) {
    }
    @Override
    public void onComplete() {
        Log.d("chan","=============onComplete ");
    }
};
observable.subscribe(observer);
```
> <font color=#2EB5AA size=3>打印结果：</font><br>
```
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onNext Hello Observer
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onComplete
```
2. `just()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
3. `fromArray()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
4. `fromCallable()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
5. `fromFuture()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
6. `fromIterable()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
7. `defer()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
8. `timer()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
9. `interval()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
10. `intervalRange()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
11. `range()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
12. `rangeLong()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
13. `empty() & never() & error()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
14. `range()`<br>
> **作用&#160;&#160;&#160;&#160;&#160;&#160;&#160;：**<br>
> **方法预览：**```s```<br>
> **方法使用：**
```
s
```
> **打印结果：**<br>
```
s
```
