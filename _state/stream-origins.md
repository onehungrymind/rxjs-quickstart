---
title: Stream Origins
position: 2.4
description: MOAR sequencing!
wistia_id:
right_code: |
  ~~~ typescript
  import { Component, OnInit } from '@angular/core';
  import { fromEvent } from 'rxjs';
  import { map, pairwise, startWith } from 'rxjs/operators';
  import * as $ from 'jquery';

  @Component({
    selector: 'app-stream-origin',
    template: `
    <app-line
      *ngFor="let line of lines" [line]="line">
    </app-line>
    `
  })
  export class StreamOriginComponent implements OnInit {
    lines: any[] = [];
    ngOnInit() {
      const emptyLine: any = { x1: 0, y1: 0, x2: 0, y2: 0 };

      // Observable.fromEvent(document, 'mousemove')
      fromEvent(document, 'click')
        .pipe(
          map((event: MouseEvent) => {
            const offset = $(event.target).offset();
            return {
              x: event.clientX - offset.left,
              y: event.pageY - offset.top
            };
          }),
          pairwise(),
          map(positions => {
            const p1 = positions[0];
            const p2 = positions[1];
            return { x1: p1.x, y1: p1.y, x2: p2.x, y2: p2.y };
          }),
          startWith(emptyLine)
        )
        .subscribe(line => this.lines = [...this.lines, line]);
    }
  }
  ~~~
---