#  RxJava2 操作一览
---
### 一、基本概念
> 被观察者（Observable）<br>
> 观察者（Observer）<br>
> 订阅（subscribe）
---
### 二、使用步骤
1. 创建被观察者：
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
2. 创建观察者：
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
3. 订阅:
```
observable.subscribe(observer);
```
---
### 三、被观察者发送的事件
| 事件种类 | 作用 |
| :--- | :--- |
| onNext() | 发送该事件时，观察者会回调 onNext() 方法 |
| onError() | 发送该事件时，观察者会回调 onError() 方法，当发送该事件之后，其他事件将不会继续发送|
| onComplete() | 发送该事件时，观察者会回调 onComplete() 方法，当发送该事件之后，其他事件将不会继续发送 |
---
### 四、操作符一览
#### 创建操作符
1. <br>
> 方法：&#160;&#160;&#160;&#160;&#160;&#160;&#160;`create()`<br>
> 作用：&#160;&#160;&#160;&#160;&#160;&#160;&#160;**创建一个被观察者**<br>
> 方法预览：```public static <T> Observable<T> create(ObservableOnSubscribe<T> source)```<br>
> 方法使用：
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
> 打印结果：<br>
```
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onNext Hello Observer
11-06 11:40:59.411 20983-20983/com.css.rxjava D/RxJava: =============onComplete
```
2. <br>
> 方法：&#160;&#160;&#160;&#160;&#160;&#160;&#160;`s`<br>
> 作用：&#160;&#160;&#160;&#160;&#160;&#160;&#160;**s**<br>
> 方法预览：```s```<br>
> 方法使用：
```
s
```
3. <br>
> 方法：&#160;&#160;&#160;&#160;&#160;&#160;&#160;`s`<br>
> 作用：&#160;&#160;&#160;&#160;&#160;&#160;&#160;**s**<br>
> 方法预览：```s```<br>
> 方法使用：
```
s
```
4. <br>
> 方法：&#160;&#160;&#160;&#160;&#160;&#160;&#160;`s`<br>
> 作用：&#160;&#160;&#160;&#160;&#160;&#160;&#160;**s**<br>
> 方法预览：```s```<br>
> 方法使用：
```
s
```
