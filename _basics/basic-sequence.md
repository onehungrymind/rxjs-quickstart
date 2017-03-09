---
title: Basic Sequence
position: 1
description: Create a basic RxJS stream
wistia_id: ktdesnzaz8
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
        .subscribe(result => this.message = 'Beast Mode Activated!');
    }
  
    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
---