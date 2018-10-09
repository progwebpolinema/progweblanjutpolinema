# Data Binding

Data binding dapat diartikan sebagai cara berkomunikasi antara bussiness logic
pada file TS dengan Template HTML. Gunakan starter-kit pada direktori Week3
untuk mempelajari modul ini.

Angular mengenal 4 jenis data binding, yaitu
1. String interpolation
2. Property binding
3. Event binding
4. Two-way binding

### String Interpolation

String interpolation digunakan untuk menampilkan data dari file TS (business
logic) ke template (HTML) dengan menggunakan syntanx `{{ data }}`. Seluruh
data yang ditampilkan oleh string interpolation didalam template akan dirubah
menjadi tipe data string.

Perhatikan kode `server.component.html` dibawah ini

```html
<p>Server with ID {{ serverID }} is {{ serverStatus }}</p>
```

{{ serverID }} dan {{ serverStatus }} merupakan data binding dengan tipe string
interpolation. Variabel yang dipanggil didalam string interpolation harus sudah
didefinisikan didalam kode TS, atau dalam hal ini ada pada
`server.component.ts`. String interpolation akan mengubah semua tipe data dalam
kode TS menjadi sebuah string pada template. Value string juga dapat dilakukan
dengan cara hardcoding value string didalam string interpolation.

Coba tambahkan `<p>Server with {{ '15' }} is {{ 'online' }}</p>` di dalam
file template component server. Perhatikan apa yang terjadi! Setelah memahami
apa yang telah terjadi, silahkan hapus atau jadikan komentar pada kode
`<p>Server with {{ '15' }} is {{ 'online' }}</p>`.

Selain menampilkan variable dan hardcoded string, kita juga dapat memanggil
sebuah fungsi (function) didalam string interpolation. Fungsi tersebut harus
memberikan nilai kembalian (return value). Perhatikan kode `server.component.ts`
dibawah ini

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    // selector: '[app-server]', // property selector
    // selector: '.app-server', // class selector
    templateUrl: './server.component.html',
    styleUrls: ['./server.component.css']
})

export class ServerComponent {
    serverID = 10;
    serverStatus = 'offline';

    getServerStatus() {
        return this.serverStatus;
    }
}
```

Fungsi `getServerStatus()` akan memberikan nilai kembalian
`serverStatus`. Untuk memanggil fungsi tersebut dalam template, cukup
masukkan nama fungsi kedalam string interpolation, sehingga menjadi,

```html
<p>Server with ID {{ serverID }} is {{ getServerStatus() }}</p>
```

### Propety Binding

Pada template component **servers** tambahkan sebuah button di bagian paling
atas sehingga template menjadi,

```html
<button class="btn btn-primary" disabled>Add Server</button>
<app-server></app-server>
<app-server></app-server>
```

Property **disabled** pada elemen button akan menjadikan button tidak dapat
menerima aksi klik. Kita akan melakukan modifikasi pada file TS component
servers, sehingga dalam rentang waktu tertentu, status disable akan berubah,
sehingga user dapat melakukan aksi klik pada button. Perhatikan file TS
`servers.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

}
```

Fungsi JS setTimeout digunakan sebagai timer untuk merubah nilai
**allowNewServer** menjadi **true**. Kemudian ubah kode template component
servers menjadi,

```html
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer">Add Server</button>
<app-server></app-server>
<app-server></app-server>
```

Perhatikan baris ke-3! Square brackets atau kurung siku -> **[]** merupakan
penanda bahwa property tersebut digunakan sebagai fungsi property binding. Value
dari property binding ditelakkan didalam tanda pentik, dalam hal ini adalah
**"!allowNewServer"**. Tanda seru berarti **not**. Sehingga, jika
**allowNewServer** memiliki nilai **false**, maka **!allowNewServer** memiliki
nilai **true**.

### Event Binding

Event binding digunakan untuk merespon aksi yang dilakukan oleh user didalam
sebuah template. Contoh dari aksi user adalah aksi menekan (klik) sebuah tombol.
Tentunya kita harus merespon aksi tersebut. Kita dapat menggunakan **event
binding** untuk melakukan hal tersebut. Lakukan modifikasi pada file TS
component servers sehingga menjadi seperti dibawah ini,

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = 'Tidak ada server baru yang telah dibuat!';

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

  onCreationStatus() {
    this.serverCreationStatus = 'Server telah dibuat!';
  }

}
```
Kemudian ubah template component servers menjadi,

```html
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreationStatus()">Add Server</button>
<p>{{ serverCreationStatus }}</p>
<app-server></app-server>
<app-server></app-server>
```

Pada baris ke-4, kita melakukan event binding untuk event bernama **click**
ditandai dengan event click didalam tanda kurung. Tanda kurung atau parenthesis
merupakan penanda untuk event binding pada Angular. Sedangkan value yang kita
gunakan dalam event binding click adalah fungsi **onCreationStatus()**. Fungsi
onCreationStatus() mengubah nilai variabel **serverCreationStatus** menjadi
'**Server telah dibuat**'.

#### Passing Data Using Event Binding

Selanjutnya kita akan mencoba menangkap inputan dari user dengan menggunakan
event binding. Buat sebuah input text pada template components servers, sehingga
menjadi,

```html
<label>Server Name</label>
<input
  type="text"
  class="form-control"
  (input)="onUpdateServer($event)">
<p>{{ serverName }}</p>
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreationStatus()">Add Server</button>
<p>{{ serverCreationStatus }}</p>
<app-server></app-server>
<app-server></app-server>
```
Perhatikan kode baris ke-5. Event binding input akan menjalankan fungsi
**onUpdateServerName()** dengan input paramater **$event**. Parameter $event
digunakan untuk menangkap event yang terjadi. Event tersebut akan kita tangkap
pada file TS, atau lebih tepatnya pada fungsi **onUpdateServerName()**. String
interpolation {{ serverName }} pada baris ke-6 digunakan untuk menapilkan value
dari input text. Langkah selajutnya adalah, membuat fungsi
**onUpdateServerName()** pada file TS component servers sehingga menjadi seperti
berikut,

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = 'Tidak ada server baru yang telah dibuat!';
  serverName = '';

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

  onCreationStatus() {
    this.serverCreationStatus = 'Server telah dibuat!';
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }

}
```

Pada baris ke-28, value dari **serverName** akan diganti sesuai dengan apa yang
diinputkan pada input text. Kode casting `(<HTMLInputElement>event.target)`
digunakan untuk memastikan bahwa event yang terjadi adalah event pada element
HTML input. Perhatikan apa yang terjadi pada aplikasi Angular!

### Two-way binding

Two-way binding membutuhkan modul **FormsModule** dari package
**@angular/forms**, sehingga sebelum dapat menerapkan two-way binding pada
aplikasi Angular kita, terlebih dahulu kita harus mengimport modul tersebut.
Ubah file `app.module.ts` menjadi,

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { ServerComponent } from '../app/server/server.component';
import { ServersComponent } from './servers/servers.component';

@NgModule({
  declarations: [
    AppComponent,
    ServerComponent,
    ServersComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Perhatikan baris ke-3 dan ke-17. Baris ke-3 digunakan untuk melakukan import
modul **FormsModule**. Kemudian kita harus memberi tahu Angular bahwa kita akan
menggunakan modul tersebut dengan nembahkannya pada array import didalam
decorator @NgModule, baris ke-17.

Two-way binding merupakan kombinasi antara property binding dengan event
binding. Dengan cara ini, kita dapat melakukan komunasikasi dua arah, baik dari
files TS ke Template, ataupun sebaliknya. Modifikasi file template servers
sehingga menjadi,

```html
<label>Server Name</label>
<!-- <input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)"> -->
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName">
<p>{{ serverName }}</p>
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreationStatus()">Add Server</button>
<p>{{ serverCreationStatus }}</p>
<app-server></app-server>
<app-server></app-server>
```

Kode [(ngModel)] pada baris ke-9 merupakan perintah _**directive**_ yang
digunakan untuk two-way binding. Apa itu _**directive**_ akan kita bahas pada
bagian selanjutnya. Kemudian pada file TS component servers, berika value pada
variabel `serverName` sehingga menjadi,

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = 'Tidak ada server baru yang telah dibuat!';
  serverName = 'TestServer';

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

  onCreationStatus() {
    this.serverCreationStatus = 'Server telah dibuat!';
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }

}
```

Lihat apa yang terjadi pada aplikasi Angular!

### TUGAS
1. Buat sebuah component dengan nama **tugas**
2. Tampilkan component tugas pada template app (silahkan `app-servers`
   dijakdikan komenter)
3. Buatlah sebuah inputan untuk `username` dengan menggunakan two-way
   binding (15 poin)
4. Tampilkan `username` yang diketikkan oleh user pada element `<p>`
   dengan menggunakan string interpolation (15 poin)
5. Buatlah sebuah tombol yang HANYA BISA DI TEKAN ATAU KLIK KETIKA INPUT TIDAK
   KOSONG (40 poin)
6. Ketika tombol pada point 5 ditekan, maka input text akan di reset ulang
   kedalam keadaan kosong (30 poin)

