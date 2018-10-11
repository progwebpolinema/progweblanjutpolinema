# Basic Selector

Pada modul pertama, kita sudah belajar bahwa Angular menggunakan selector
sebagai penanda component. Kita telah mengenal selector dengan menggunakan tag
element. Akan tetapi ada tipe selector lain yang bisa gunakan. Angular dapat
menggunakan 3 tipe selector, yaitu,

1. Tag element selector
2. Property selector
3. Class selector

> Angular tidak mendukung tipe selector dengan menggunakan ID.

### Tag Element Selector

Perhatikan kode `server.component.ts` dibawah ini,

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-server" /* ini adalah selector tag */,
  templateUrl: "./server.component.html" /* path menuju template html*/,
  styleUrls: ["./server.component.css"] /*path menuju file style*/
})
export class ServerComponent {
  serverName = "server";
  serverStatus = "offline";
}
```

Baris ke-4 merupakan contoh penggunaan selector tag element pada kode TS. Untuk
menggunakan selector pada template, maka element `<app-server></app-server>`
digunakan dalam template. Contohnya ada pada kode `app.component.html` baris
ke-6 dibawah ini,

```html
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <h1> {{ title }} </h1>
      <hr/>
      <app-server></app-server>
    </div>
  </div>
</div>
```

### Property Selector

Seperti namanya, property binding akan memanfaatkan property atau atribut dari
tag HTML sebagai selector. Tag HTML yang digunakan adalah tag HTML biasa (buka
special tag dari angular), namun ditambakan dengan selector property binding.
Perhatikan kode `server.component.ts`

```typescript
import { Component } from "@angular/core";

@Component({
  // selector: 'app-server',
  selector: "[app-server]", // property selector
  templateUrl: "./server.component.html",
  styleUrls: ["./server.component.css"]
})
export class ServerComponent {
  serverName = "server";
  serverStatus = "offline";
}
```

Kemudian untuk menggunakan property selector, perhatikan kode
`app.component.html` baris ke-7

```html
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <h1> {{ title }} </h1>
      <hr/>
      <!-- <app-server></app-server> -->
      <div app-server></div>
    </div>
  </div>
</div>
```

Pada `<div>` ditambahkan property `<div app-server>` sehingga selector dapat
dikenali oleh angular.

### Class Selector

Terakhir kita dapat menggunakan class sebagai selector pada angular. Value dari
selector inilah yang akan menjadi pengenal bagi angular. Perhatikan kode
`server.component.ts`

```typescript
import { Component } from "@angular/core";

@Component({
  // selector: 'app-server',
  // selector: '[app-server]', // property selector
  selector: ".app-server", // class selector
  templateUrl: "./server.component.html",
  styleUrls: ["./server.component.css"]
})
export class ServerComponent {
  serverName = "server";
  serverStatus = "offline";
}
```

Kemudian pada `app.component.html` berikan value `app-server` pada property
class.

```html
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <h1> {{ title }} </h1>
      <hr/>
      <!-- <app-server></app-server> -->
      <!-- <div app-server></div> -->
      <div class="app-server"></div>
    </div>
  </div>
</div>
```

## TUGAS

1. Ubah variabel `serverName` pada `server.component.ts` menjadi `serverID`
   dengan tipe data **number** (20 poin)
2. Modifikasi template component server sehingga menampilkan `Server with ID {{ serverID }} is {{ serverStatus }}` (20 poin)
3. Buatlah component baru dengan menggunakan Angular CLI dengan nama **servers**
   (20 poin)
4. Tampilkan 2 component **server** didalam component **servers** menggunakan
   **tag element selector** (20 poin)
5. Pada `app.component.html` tampilkan selector dari component **servers**
   (hint: sebelumnya adalah selector dari server) (20 poin)
