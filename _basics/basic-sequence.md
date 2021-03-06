---
title: Basic Sequence
position: 1
description: Create a basic RxJS stream
wistia_id:
right_code: |
  ~~~ typescript
  import { Component, OnInit, ViewChild } from '@angular/core';
  import { fromEvent } from 'rxjs';
  import { filter } from 'rxjs/operators';
  import { map } from 'rxjs/operators';

  @Component({
    selector: 'app-basic-sequence',
    template: `
    <button #btn mat-raised-button color="accent">Click me!</button>
    <div class="container">
      <h1>{{message}}</h1>
    </div>
    `
  })
  export class BasicSequenceComponent implements OnInit {
    @ViewChild('btn') btn;
    message: string;

    ngOnInit() {
      fromEvent(this.getNativeElement(this.btn), 'click')
        .subscribe(result => this.message = 'Beast Mode Activated!');
    }

    getNativeElement(element) {
      return element._elementRef.nativeElement;
    }
  }
  ~~~
---