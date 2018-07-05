---
title: Combining Streams
position: 2.6
description: Running multiple streams in parallel and capturing the results, much like "Promise.all".
wistia_id:
right_code: |
  ~~~ typescript
  import { Component } from '@angular/core';
  import { AnimalService } from './animal.service';
  import { forkJoin } from 'rxjs/index';
  import { tap } from 'rxjs/internal/operators';
  import { DomSanitizer } from '@angular/platform-browser';

  @Component({
    selector: 'app-combining-streams',
    template: `
      <button (click)="randomize()" mat-raised-button color="accent">Randomize</button>
      <hr>
      <div class="images">
        <video controls *ngIf="isVideo(dog); else dogImage;" [src]="dog.url"></video>
        <ng-template #dogImage>
          <img *ngIf="dog" [src]="dog.url" alt="Random Dog">
        </ng-template>
        <img *ngIf="cat" [src]="cat" alt="Random Cat">
      </div>
    `,
    styles: [`
      .images {
        display: flex;
        align-items: center;
      }
      img {
        max-width: 500px;
      }
      img:not(:first-child) {
        margin-left: 15px;
      }
    `],
    providers: [AnimalService]
  })

  export class CombiningStreamsComponent {
    dog;
    cat;

    constructor(private animalService: AnimalService, private sanitizer: DomSanitizer) {}

    randomize() {
      forkJoin(this.animalService.getDog(), this.animalService.getCat())
        .pipe(tap(([dog, cat]) => {
          this.dog = dog;
          this.cat = this.sanitizer.bypassSecurityTrustResourceUrl(URL.createObjectURL(cat));
        })).subscribe();
    }

    isVideo(dog) {
      return dog && dog.url.includes('mp4');
    }
  }
  ~~~
  {: title="Component"}
  ~~~ typescript
  import { Injectable } from '@angular/core';
  import { HttpClient } from '@angular/common/http';

  @Injectable()
  export class AnimalService {

    constructor(private http: HttpClient) {}

    getDog() {
      return this.http.get(`https://random.dog/woof.json`);
    }

    getCat() {
      return this.http.get(`https://cataas.com/cat`, {responseType: 'blob'});
    }
  }
  ~~~
  {: title="Service"}
---