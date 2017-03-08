---
title: Mapping Values
position: 1.1
description: Write your first test with Jasmine
wistia_id: dnslnbw2lo
right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { Observable } from 'rxjs/Observable';
  import 'rxjs/add/observable/fromEvent';
  import 'rxjs/add/operator/filter';
  import 'rxjs/add/operator/map';
  
  @Component({
    selector: 'app-basic-sequence',
    template: `
    <button #btn md-raised-button color="accent">Click me!</button>
    <div class="container">
      <h1>{{message}}</h1>
    </div>
    `
  })
  export class BasicSequenceComponent implements OnInit {
    @ViewChild('btn') btn;
    message: string;
  
    ngOnInit() {
      Observable.fromEvent(this.getNativeElement(this.btn), 'click')
        // .filter(event => event.shiftKey) // Operator stacking
        .map(event => 'Beast Mode Activated!')
        .subscribe(result => this.message = result);
    }
  
    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
---

Bacon ipsum dolor amet chuck short ribs t-bone tenderloin. Meatloaf rump alcatra swine filet mignon corned beef tongue leberkas tail salami shoulder venison strip steak shankle hamburger. Pork loin leberkas brisket, frankfurter pig corned beef tongue beef ribs swine jerky tenderloin. Andouille brisket swine, jowl cow jerky kevin sausage.

