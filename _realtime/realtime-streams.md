---
title: Realtime Streams
position: 3
description: Combining the power of RxJS and Firebase
wistia_id: tpcztytfjb
right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { AngularFire } from 'angularfire2';
  
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
  
    constructor(private af: AngularFire) {}
  
    ngOnInit() {
      const remote$ = this.af.database.object('clicker/');
  
      remote$
        .subscribe(result => this.count = result.ticker)
      ;
    }
  }
  ~~~
  {: title="Client"}
  ~~~typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { Observable } from 'rxjs/Observable';
  import { AngularFire } from 'angularfire2';
  import 'rxjs/add/observable/fromEvent';
  import 'rxjs/add/operator/do';
  import 'rxjs/add/operator/map';
  import 'rxjs/add/operator/startWith';
  
  interface Ticker {
    ticker: number
  }
  
  @Component({
    selector: 'app-counter-master',
    template: `
    <button #btn md-raised-button color="accent">Click me!</button>
    `
  })
  export class CounterMasterComponent implements OnInit {
    @ViewChild('btn') btn;
  
    constructor(private af: AngularFire) {}
  
    ngOnInit() {
      const remote$ = this.af.database.object('clicker/');
  
      Observable.fromEvent(this.getNativeElement(this.btn), 'click')
        .startWith({ticker: 0})
        .scan((acc: Ticker, curr) => { return { ticker: acc.ticker + 1 }; })
        .subscribe(event => remote$.update(event));
    }
  
    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
  {: title="Master"}
---

Bacon ipsum dolor amet chuck short ribs t-bone tenderloin. Meatloaf rump alcatra swine filet mignon corned beef tongue leberkas tail salami shoulder venison strip steak shankle hamburger. Pork loin leberkas brisket, frankfurter pig corned beef tongue beef ribs swine jerky tenderloin. Andouille brisket swine, jowl cow jerky kevin sausage.
