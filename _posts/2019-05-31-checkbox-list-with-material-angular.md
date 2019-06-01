---
layout: blog
title: Checkbox List With Material Angular
published: true
date: 2019-05-31T08:06:53.250Z
thumbnail: /images/uploads/markus-spiske-109588-unsplash.jpg
rating: 4
---
We make a checkbox list component in angular using material angular MatCheckbox component. When used as model this list will return the list selected checkbox values

![Checkbox List Screenshot](/images/uploads/screenshot-2019-06-01-at-14.53.24.png "Checkbox List")

<!-- more -->

## Implemetation

```ts
import { Component, OnInit, Input, forwardRef } from "@angular/core";
import { ControlValueAccessor, NG_VALUE_ACCESSOR } from '@angular/forms';

@Component({  
   selector: 'db-mat-checkbox-list',
   template: `   
   <div [id]="dbmatcbId" class='cb-list'>
     <div class="cb-warpper" *ngFor="let opt of options; let i = index">
       <mat-checkbox [name]="opt" [value]="opt" [(ngModel)]="selectedModel[opt]" (change)="onChange($event)">{{opt}}</mat-checkbox>
     </div>
   </div>  
   `,
    styles: [`
    .cb-list {
        display: flex;
        padding: 10px;
    }
    .cb-warpper{
        width: 20%;
        min-width: 60px;
    }
    `],
    providers: [{ provide: NG_VALUE_ACCESSOR, useExisting: forwardRef(() => DBMatCheckboxListComponent), multi: true }]
})
export class  DBMatCheckboxListComponent implements OnInit, ControlValueAccessor{
  dbmatcbId = `dbmat-checkbox-list-${idctr++}`;
  @Input() options: string[];

  private _innerValue: string[];

  selectedModel: { [s: string]: boolean; } = {};
  
  ngOnInit(): void {
    // Setting the initial value as false
    this.options.forEach(e => this.selectedModel[e] = false);
  }

  writeValue(obj: any): void {
    this._innerValue = obj;
    if (this._innerValue) {
      this._innerValue.forEach(e => this.selectedModel[e] = true);
      this._controlValueAccessorChangeFn(obj);
    }
  }

  onChange() {
    this._innerValue = Object.keys(this.selectedModel).filter(k =>   this.selectedModel[k]);
    this._controlValueAccessorChangeFn(this._innerValue);
  }
  
  registerOnChange(fn: any): void {
    this._controlValueAccessorChangeFn = fn;
  }

  registerOnTouched(fn: any): void {
    this._onTouched = fn;
  }

  setDisabledState?(isDisabled: boolean): void {
    // throw new Error("Method not implemented.");
  }
  // Defaulf noop 
  private _controlValueAccessorChangeFn: (value: any) => void = () => { };
  private _onTouched: () => any = () => { };

  
}
let idctr = 0;
```

This is component is ready to used array of string and display list of checkboxes and will return only the select list string

## Using new checkbox list

```ts
import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'sample-cb-list',
    template: `
    <div>
        <db-mat-checkbox-list [options]="optionslist" name="cblist" [(ngModel)]="cblist"></db-mat-checkbox-list>
    </div>
    <div><button (click)="display()">Display</button></div>
    <div>{{cblist}}</div>
    `,
    styles: [``]
})
export class SampleCBCListComponent implements OnInit {

    optionslist: string[] = [];

    cblist: string[] = [];

    constructor() {}

    ngOnInit(): void {
        this.optionslist = ['Apple', 'Orange', 'Grapes']
    }


    display() {
      console.log('selected ', this.cblist);
    }
} 
```

#### Enjoy your new checkbox list

<div class="theframe">
  <iframe src="https://angular-4hm6ja.stackblitz.io" width="100%" height="100%"></iframe>
</div>

> App Url @stackblitz.io <https://angular-4hm6ja.stackblitz.io>
