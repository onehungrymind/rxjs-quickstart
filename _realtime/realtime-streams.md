---
title: Realtime Streams
position: 3
description: Combining the power of RxJS and Firebase
wistia_id: tpcztytfjb
right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { AngularFireDatabase } from 'angularfire2/database';

  @Component({
    selector: 'app-counter-client',
    template: `
    <div>
      <h2>Beast Mode Activated</h2>
      <strong>{{count}} times!</strong>
    </div>
    `
  })
  export class CounterClientComponent implements OnInit {
    count: number;

    constructor(private db: AngularFireDatabase) {}

    ngOnInit() {
      const remote$ = this.db.object('clicker/').valueChanges();

      remote$
        .subscribe((result: any) => this.count = result.ticker)
      ;
    }
  }
  ~~~
  {: title="Client"}
  ~~~typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { fromEvent } from 'rxjs';
  import { scan, startWith } from 'rxjs/operators';
  import { AngularFireDatabase } from 'angularfire2/database';

  interface Ticker {
    ticker: number;
  }

  @Component({
    selector: 'app-counter-master',
    template: `
    <button #btn mat-raised-button color="accent">Click me!</button>
    `
  })
  export class CounterMasterComponent implements OnInit {
    @ViewChild('btn') btn;

    constructor(private db: AngularFireDatabase) {}

    ngOnInit() {
      const remoteRef = this.db.object('clicker/');

      fromEvent(this.getNativeElement(this.btn), 'click')
        .pipe(
          startWith({ticker: 0}),
          scan((acc: Ticker, curr) => ({ ticker: acc.ticker + 1 }))
        )
        .subscribe(event => remoteRef.update(event));
    }

    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
  {: title="Master"}
---