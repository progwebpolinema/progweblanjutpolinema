# Directive 1 - The Basic

## Apa itu directive ?

Directive adalah perintah dalam Angular yang digunakan untuk memanipulasi
document object model atau (DOM). Dalam pembelajaran sebelumnya, kita telah
menggunakan directive tanpa kita menyadarinya. Component dalam Angular merupakan
salah satu contoh directive, dimana elemen-elemen dapat dimanipulasi dengan
menggunakan component. Pada modul ini, kita akan belajar bagaimana cara
menggunakan **built-in directive** atau directive yang sudah ada secara default
pada Angular. Sedangkan materi yang lebih mendalam tentang directive akan dibaha
pada modul selanjutnya.

### Persiapan

Modul ini merupakan modul lanjutan dari pembelajaran pada Minggu ke-3. Untuk
itu, silahkan salin project Angular pada direktory Minggu ke-3 ke direktori
Minggu ke-4. Pastikan project Angular sudah memiliki,

1. Component server dan servers.
2. Sudah mengimplementasi data binding yang dipelajari sebelumnya.

### Tambahan

> Jika dalam contoh kode terdapat sebuah komentar / comment, maka tidak wajib
> untuk ditulis. Commented code hanya sebagai bahan referensi dari apa yang telah
> dikerjakan sebelumnya.

## Conditional output dengan ngIf

Untuk membuat sebuah kondisi dalam Angular, kita dapat memanfaatkan built-in
directive **ngIf**. Perhatikan langkah langkah berikut untuk mengetahui cara
kerja **ngIf**

Ubah file TS component servers menjadi seperti kode dibawah ini,

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
  serverCreated = false;

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

  onCreationStatus() {
    this.serverCreated = true;
    this.serverCreationStatus = 'Server telah dibuat!';
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }

}
```

Pada baris ke-12 kita nembahkan variabel **serverCreated** denga value boolean
false. Variabel ini akan digunakan untuk pengecekan kondisi dengan menggunakan
directive **ngIf**. Pada baris ke-25 didalam fungsi **onCreationStatus()** kita
merubah value dari **serverCreated** menjadi true. Kemudian ubah template
component servers menjadi dibawah ini,

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
<!-- <p>{{ serverCreationStatus }}</p> -->
<p *ngIf="serverCreated">Server telah dibuat, nama server adalah {{ serverName }}</p>
<app-server></app-server>
<app-server></app-server>
```

Pada baris ke-16, dilakukan modifikasi pada pesan ketika tombol **Add Server**
ditekan. Perhatikan property didalam elemen p. **ngIf** merupakan deklarasi
dari directive ngIf. Penggunaan tanda asteriks (*) menunjukkan bahwa ngIf
merupakan structur directive. Structure directive adalah jenis directive yang
mampu mengubah struktur DOM pada saat itu juga. Tanda asteriks didepan nama
struture directive wajib dilakukan. **serverCreated** merupakan value dari
variabel yang telah kita buat pada file TS. Ketika tombol **Add Server**
ditekan, maka akan mengubah value dari serverCreated menjadi true, sehingga
pesan akan ditampilkan.

### Menggunakan kondisi else pada ngIf

Terkadang kita juga membutuhkan kondisi alternatif dalam conditional statement.
Penggunaan else dalam Angular dapat dilakukan dengan cara berikut,

Ubah template component servers menjadi,

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
<!-- <p>{{ serverCreationStatus }}</p> -->
<p *ngIf="serverCreated; else noServer">Server telah dibuat, nama server adalah {{ serverName }}</p>
<ng-template #noServer>
  <p>No server was created</p>
</ng-template>
<app-server></app-server>
<app-server></app-server>
```

Perhatikan baris ke-17 sampang ke-19. Kode tersebut digunakan untuk membuat
**local reference** yang akan digunakan pada kondisi else. Local reference yang
dibuat diberikan nama **noServer** ditandai dengan tanda **#**. Untuk sementara
anggap local reference sebagai marker, kita akan membahasnya lebih dalam pada
modul selanjutnya. Pada barus ke-16, kondisi else digunakan dengan mengacu pada
local reference noServer. **Perhatikan pada yang terjadi pada aplikasi
Angular**!

### Styling dengan ngStyle

ngStyle merupakan attribute direcive yang digunakan untuk mengubah atribute HTML
secara dinamis. Untuk mengetahui cara kerjanya, ubah file TS componen server
menjadi seperti dibawah ini.

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

    constructor(){
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
    }

    getServerStatus() {
        return this.serverStatus;
    }

    getColor() {
        return this.serverStatus === 'online' ? 'green' : 'red';
    }
}
```

Terdapat tambahan sebuah constructor pada class ServerComponent. Didalam
constructor, kita akan menggenerate angka random dimana jika angka yang muncul
lebih dari 0.5 maka value dari **serverStatus** menjadi online. Kemudian
terdapat fungsi getColor() yang akan digunakan untuk mengubah warna backgroud
pada template sesuai dengan status dari server. Modifkpasi template component
server menjadi,

```html
<p [ngStyle]="{'background-color': getColor()}">Server with ID {{ serverID }} is {{ serverStatus }}</p>
<!-- Atau dapat menggunakan kode dibawah ini -->
<!-- <p [ngStyle]="{backgroundColor: getColor()}">Server with ID {{ serverID }} is {{ serverStatus }}</p> -->

<!-- <p>Server with ID {{ serverID }} is {{ getServerStatus() }}</p> -->
<!-- <p>Server with ID {{ '15' }} is {{ 'online' }}</p> -->
```

Perhatikan baris ke-1. Pada baris tersebut digunakan ngStyle didalam sebuah
property binding untuk melakukan styleing pada elemen P. **background-color**
merupakan property CSS biasa yang kemudian mengambil value dari fungsi
getColor() yang sudah dibuat dalam file TS.

### Styling dinamis dengan ngClass

ngClass dapat digunakan untuk memanipulasi atribute class pada DOM. Dalam kasus
ini kita akan mencoba untuk memberikan atribute class atau tidak pada elemen.
Modifikasi kode TS component server menjadi seperti berikut,

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-server',
    // selector: '[app-server]', // property selector
    // selector: '.app-server', // class selector
    templateUrl: './server.component.html',
    // styleUrls: ['./server.component.css']
    styles:[`
        .online {
            color:  white;
        }
    `]
})

export class ServerComponent {
    serverID = 10;
    serverStatus = 'offline';

    constructor(){
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
    }

    getServerStatus() {
        return this.serverStatus;
    }

    getColor() {
        return this.serverStatus === 'online' ? 'green' : 'red';
    }
}
```

Pada baris ke-9 sampai ke-13 kita menggunakan inline styling untuk membuat
styling class bernama **.online**. Kemudian pada template component server,
rubah kode sehingga menjadi,

```html
<!-- <p [ngStyle]="{'background-color': getColor()}">Server with ID {{ serverID }} is {{ serverStatus }}</p> -->
<!-- Atau dapat menggunakan kode dibawah ini -->
<p
    [ngStyle]="{backgroundColor: getColor()}"
    [ngClass]="{online: serverStatus === 'online' }">
    Server with ID {{ serverID }} is {{ serverStatus }}</p>

<!-- <p>Server with ID {{ serverID }} is {{ getServerStatus() }}</p> -->
<!-- <p>Server with ID {{ '15' }} is {{ 'online' }}</p> -->
```

Perhatikan apa yang terjadi pada aplikasi Angular!

## Perulangan dengan ngFor

ngFor juga termasuk kedalam structure directive. Hal ini dikarenakan ngFor juga
dapat menipulasi elemen dengan menambahkan elemen baru kedalam sebuah DOM. Ubah
kode TS pada component servers menjadi seperti dibawah ini,

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
  serverCreated = false;
  servers = ['TestServer', 'TestServer 2'];

  constructor() {
    // () => {} adalah arrow function atau lamda --> fitur ES6 javascript
    setTimeout(()=> {
      this.allowNewServer = true;
    }, 2000)
  }

  ngOnInit() {
  }

  onCreationStatus() {
    this.serverCreated = true;
    this.servers.push(this.serverName);
    this.serverCreationStatus = 'Server telah dibuat!';
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }

}
```

Kemudian kita akan menerapkan *ngFor pada template servers. Ubah kode template
servers menjadi seperti dibawah ini,

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
<!-- <p>{{ serverCreationStatus }}</p> -->
<!-- <p *ngIf="serverCreated; else noServer">Server telah dibuat, nama server adalah {{ serverName }}</p>
<ng-template #noServer>
  <p>No server was created</p>
</ng-template> -->
<p *ngIf="serverCreated">Server telah dibuat, nama server adalah {{ serverName }}</p>
<!-- <app-server></app-server> -->
<app-server *ngFor="let server of servers"></app-server>
```

Pada baris ke-22 kita melakukan perulangan dengan menggunakan directive ngFor
pada selector component **app-server**

### TUGAS

1. Buatlah component baru dengan nama tugas3. (10 poin)
2. Pada component tersebut, buatlah sebuah tombol dengan tulisan '**Tampilkan
   Detail**'. (15 poin)
3. Tampilkan kalimat bebas (contoh : 'POLITEKNIK NEGERI MALANG') ketika tombol
   ditekan dengan menggunakan paragraf. (15 poin)
4. Tampilkan simpan **log** berapa kali tombol di tekan **kedalam sebuah array**
   dan tampilkan log tersebut dibawah kalimat bebas yang dibuat pada poin 3. (30
   poin)
5. Mulai log ke-5, berikan warna background biru pada log dengan menggunakan
   ngStyle dan tulisan berwarna putih dengan menggunakan ngClass. (30 poin)

### SOLUSI TUGAS
_**Will be updated as soon as possible**_
