---
layout: blog
title: Sort Date Correctly with Material Angular Table
published: true
date: 2019-06-03T09:03:37.032Z
thumbnail: /images/uploads/markus-spiske-109588-unsplash.jpg
rating: 4
---
We will try sort a date field which is coming as string in MM/dd/YYYY.

> Using Default Sort for String as date

![Incorrect Sort for date material angular table ](/images/uploads/screenshot-2019-06-05-at-22.07.29.png "Incorrect Sort for date material angular table ")

### Component code

```ts
import { Component, OnInit, ViewChild } from '@angular/core';
import {MatSort} from '@angular/material/sort';
import {MatTableDataSource} from '@angular/material/table';


const DATA = [
  {birthDate: '07/03/1987', name: 'Jon Doe'},
  {birthDate: '07/02/1987', name: 'Leo Yan'},
  {birthDate: '05/18/1987', name: 'Jane Doe'},
  {birthDate: '04/13/1990', name: 'Ron Doug'},
  {birthDate: '08/23/1987', name: 'Kaven Paul'},
  {birthDate: '08/03/1988', name: 'Sean Potson'},
];

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent implements OnInit{
  name = 'Angular';
  dataSource = new MatTableDataSource(DATA);
  displayedColumns = ['birthDate', 'name'];
  @ViewChild(MatSort, {static: true}) sort: MatSort;

  ngOnInit(){
    this.dataSource.sort = this.sort;
  }

}
```

### Template Code

```html
<div class="mat-elevation-z8">
	<table mat-table [dataSource]="dataSource" matSort>
		<ng-container matColumnDef="birthDate">
			<th mat-header-cell *matHeaderCellDef mat-sort-header> Birth Date </th>
			<td mat-cell *matCellDef="let element"> {{element.birthDate}} </td>
		</ng-container>
		<ng-container matColumnDef="name">
			<th mat-header-cell *matHeaderCellDef mat-sort-header> Name </th>
			<td mat-cell *matCellDef="let element"> {{element.name}} </td>
		</ng-container>
    <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
    <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
	</table>
</div>
```

## Solution

We need to tell the mat to compare the Birth date as Date rather then string so the easy way is to convert string to date before sort, for that we'll update the `sortingDataAccessor` of `MatTableDataSource`

#### Update `sortingDataAccessor` on `ngOnInit()`

```ts
  ngOnInit(){
    this.dataSource.sort = this.sort;  
    this.dataSource.sortingDataAccessor = (e, property) => {  
      switch (property) 
      {    
        case 'birthDate': return this.dateTransform(e.birthDate);      
        default: return e[property];  
      }
    };
  }
```

#### dateTransform

```ts
  private dateTransform(val: string) {
    console.log('val', val);
    if (val) {
      try {
        const mval = new Date(val);
        console.log('mval', mval);
        return (+mval);
      } catch (error) {
        console.error(error);
        console.log('error', error);
      }
    }
    return 0;
  }
```

#### Updated Component Code

```ts
import { Component, OnInit, ViewChild } from '@angular/core';
import { MatSort } from '@angular/material/sort';
import { MatTableDataSource } from '@angular/material/table';


const DATA = [
  { birthDate: '07/03/1987', name: 'Jon Doe' },
  { birthDate: '07/02/1987', name: 'Leo Yan' },
  { birthDate: '05/18/1987', name: 'Jane Doe' },
  { birthDate: '04/13/1990', name: 'Ron Doug' },
  { birthDate: '08/23/1987', name: 'Kaven Paul' },
  { birthDate: '08/03/1988', name: 'Sean Potson' },
];

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  name = 'Angular';
  dataSource = new MatTableDataSource(DATA);
  displayedColumns = ['birthDate', 'name'];
  @ViewChild(MatSort, { static: true }) sort: MatSort;

  ngOnInit() {
    this.dataSource.sort = this.sort;
    this.dataSource.sortingDataAccessor = (e, property) => {
      switch (property) {
        case 'birthDate': return this.dateTransform(e.birthDate);
        default: return e[property];
      }
    };
  }

  private dateTransform(val: string) {
    console.log('val', val);
    if (val) {
      try {
        const mval = new Date(val);
        console.log('mval', mval);
        return (+mval);
      } catch (error) {
        console.error(error);
        console.log('error', error);
      }
    }
    return 0;
  }

}
```

> Get Code from @stackblitz.com <https://stackblitz.com/edit/angular-l1dc65>

<div class="theframe" sytle="height:28rem;">
  <iframe src="https://angular-l1dc65.stackblitz.io" width="100%" height="100%"></iframe>
</div>
