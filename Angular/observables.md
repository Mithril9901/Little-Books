## Observables

[Observables](https://github.com/tc39/proposal-observable) 是一個未來 ECMAScript 的新型別。

這種概念是從一個串流的 `subscribe` 事件方式傳入一個 Generator (產生器)，不過重要的部分是 Iterator (迭代器)。

除了 Generators 其他非同步設計的主題有 Promises 和 Async/Await。

### 基本應用

首先透過 `RxJS` 載入 `Observable`
```ts
import { Observable } from 'rxjs/Observable';
```

```ts
import { Component } from '@angular/core';
import { Observable } from 'rxjs/Observable';

@Component({
  selector: 'app-root',
  template: `
   <button (click)="init()">初始化</button>
   <div *ngFor="let value of values">{{ value }}</div>
   <div>{{ finished }}</div>
  `
})
export class AppComponent {
  private data: Observable<Array<string>>;
  private values: string[] = [];
  private finished: string;

  init() {
    this.data = new Observable(observer => {
      setTimeout(() => observer.next('開始'), 0);
      setTimeout(() => observer.next('第一個'), 1000);
      setTimeout(() => observer.next('第二個'), 2000);
      setTimeout(() => observer.next('第三個'), 3000);
      setTimeout(() => observer.complete(), 4000);
    });

    let subscription = this.data.subscribe(
      value => this.values.push(value),
      () => new Error('初始化失敗'),
      () => this.finished = '完成'
    );
  }
}
```
