# Directive 2 - The Advanced

## Bahan Praktikum

1. Download starter-kit project di url
2. ng serve

## Memisahkan Komponen

Sebelum kita masuk pada topik Data Binding Advance, sebelumnya kita akan
melakukan pemisahan komponen dari start-project. Jalankan terlebih dahulu
starter-project dengan perintah `ng serve`. Perhatikan hasil project!
Terdapat bagian untuk **Add Server** dan **Add Blueprint** dengan hasil yang
berbeda. Pertama-tama kita akan memisahkan kedua bagian tersebut menjadi 2
komponen yang berbeda. Perhatikan kode template komponen **app**

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
  </div>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <div
        class="panel panel-default"
        *ngFor="let element of serverElements">
        <div class="panel-heading">{{ element.name }}</div>
        <div class="panel-body">
          <p>
            <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
            <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
          </p>
        </div>
      </div>
    </div>
  </div>
</div>
```

Kemudian, buatlah komponen baru dengan menggunakan Angular CLI. Komponen pertama
bernama **cockpit**, komponen ke dua bernama **server-element**.

```cli
$ ng g c cockpit --spec false
$ ng g c server-element --spec false
```

Cut baris ke-2 hingga ke-17 dari kode template app ke kode template cockpit,
sehingga menjadi seperti dibawah ini.

```html
<div class="row">
    <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
  </div>
```

Akan ada underliner berwarna merah yang menandakan ada error yang terjadi.
Untuk mengatasi masalah tersebut, cut fungsi **onAddServer()** dan
**onAddBluePrint()** pada kode TS komponen app ke **kode TS komponen cockpit**.
Sehingga kode TS cockpit menjadi seperti berikut,

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

}
```

Terdapat error pada variabel serverElements, newServerName, dan
newServerContent. Mengapa terjadi error ? Karena kita belum melakukan deklarasi
terhadap variabel tersebut. Perhatikan kode TS komponen app **setelah** kita
pindahkan fungsi onAddServer() dan onAddBlueprint() kedalam kode TS komponen
cockpit.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [];
  newServerName = '';
  newServerContent = '';


}
```

Variabel serverElements, newServerName, dan newServerContent masih tertinggal
didalam kode TS app. Pada materi ini kita hanya akan memindahkan newServerName
dan newServerContent kedalam kode TS cockipt. Array serverElements[] nantinya
akan kita akses dari komponen lain, dimana secara default tidak dilakukan oleh
Angular. Sampai tahap ini, kode TS dari cockpit dan app adalah sebagai berikut,

**cockpit.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

}
```

**app.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [];

}
```

Selanjutnya, kembali ke file template komponen app, pindahkan atau cut baris
ke-6 sampai ke-16 kedalam file template server-element. Sehingga file template
server-element dan app menjadi seperti berikut,<br/><br/>

**server-element.component.html**

```html
<div class="panel panel-default" *ngFor="let element of serverElements">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <p>
      <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
      <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
    </p>
  </div>
</div>
```

**app.component.html**

```html
<div class="container">

  <hr>
  <div class="row">
    <div class="col-xs-12">

    </div>
  </div>
</div>
```

Selanjutnya, karena kita telah memiliki komponen server-elemen secara individu
dan terpisan dari komponen app, maka kita tidak memerlukan directive ngFor pada
template server-element. Pada tahap selanjutnya, komponen server-element lah
yang akan kita looping menggunakan ngFor, bukan variabel array serverElements[].
Kode template dari komponen server-element akan berubah menjadi seperti berikut,

```html
<div class="panel panel-default">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <p>
      <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
      <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
    </p>
  </div>
</div>
```

Terdapat error pada variabel element, akan kita selesaikan pada pembahasan
selanjutnya. Langkah terakhir pada topik memisahkan komponen ini adalah, rubah
kode template komponen app, dengan memanggil komponen server-element didalam div
dengan class "col-xs-12". Perhatikan kode template app berikut,

```html
<div class="container">

  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element *ngFor="let serverElement of serverElements"></app-server-element>
    </div>
  </div>
</div>
```

Coba jalankan perintah `ng serve`. Maka akan terdapat pesan error seperti pada
kode dibawah. Hal ini karena kita tidak memiliki variabel serverElements pada
komponen cockpit. **SECARA DEFAULT VARIABEL DALAM SEBUAH KOMPONEN HANYA DAPAT
DIAKSES OLEH KOMPONEN TERSEBUT**.

```
ERROR in src/app/cockpit/cockpit.component.ts(18,10): error TS2339: Property 'serverElements' does not exist on type 'CockpitComponent'.
src/app/cockpit/cockpit.component.ts(26,10): error TS2339: Property 'serverElements' doesnot exist on type 'CockpitComponent'.
```

## Custom Properties

Pada file TS cockpit, untuk menghindari error, silahkan jadikan komentar kode
yang ada didalam fungsi onAddServer dan onAddBlueprint. Perhatikan kode TS
cockpit dibawah ini

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    // this.serverElements.push({
    //   type: 'server',
    //   name: this.newServerName,
    //   content: this.newServerContent
    // });
  }

  onAddBlueprint() {
    // this.serverElements.push({
    //   type: 'blueprint',
    //   name: this.newServerName,
    //   content: this.newServerContent
    // });
  }

}
```

Selanjutnya, perhatikan terlebih dahulu kode yang ada pada template
server-element. Didalam template tersebut, kita mengakses variabel **element**,
dimana variabel element merupakan sebuah obyek ditandai dengan dapat diaksesnya
variabel yang ada didalamnya seperti **element.name**. Sehingga kita perlu
membuat sebuah obyek bernama **element** didalam kode TS server-element.
Perhatikan kode server-element dibawah ini,

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css']
})
export class ServerElementComponent implements OnInit {
  element: {type: string, name: string, content:string}

  constructor() { }

  ngOnInit() {
  }

}
```

element merupakan obyek yang secara default hanya dapat diakses oleh
server-element komponen. Pada kasus saat ini, kita menginginkan untuk mengakses
obyek tersebut dari komponen yang lain. Sekarang kita buat obyek didalam array
serverElements pada file TS app.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [{type: 'server', name: 'Testserver', content:'Just a test'}];


}
```

Selanjutnya kita akan melakukan property binding pada obyek element didalam file
template app.

```html
<div class="container">

  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [element]=serverElement></app-server-element>
    </div>
  </div>
</div>
```

Jika server Angular dijalankan, maka hanya akan muncul tampilan putih, bertanda
ada error yang harus diselesaikan. Bila kita melakukan inspect element pada
browser, maka akan ditemukan pesan error,

```
Template parse errors:
Can't bind to 'element' since it isn't a known property of 'app-server-element'.
1. If 'app-server-element' is an Angular component and it has 'element' input, then verify that it is part of this module.
2. If 'app-server-element' is a Web Component then add 'CUSTOM_ELEMENTS_SCHEMA' to the '@NgModule.schemas' of this component to suppress this message.
3. To allow any property add 'NO_ERRORS_SCHEMA' to the '@NgModule.schemas' of this component. ("
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [ERROR ->][element]=serverElement></app-server-element>
    </div>
  </div>
"): ng:///AppModule/AppComponent.html@7:8
```

Pesan error ini muncul karena variabel obyek element tidak dapat diakses pada
komponen app meski sudah di definisikan di komponen server-element.<br/> Untuk
itu kita memerlukan **decoration** bernama **@Input()** pada variabel element.
Pertama ubah kode TS server-element menjadi,

```typescript
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css']
})
export class ServerElementComponent implements OnInit {
  @Input() element: {type: string, name: string, content:string}

  constructor() { }

  ngOnInit() {
  }

}
```

_**Decorator @Input() menjadikan variable element pada komponen server-element
dapat diakses oleh komponen app**_ sehingga variabel array dari serverElements
pada komponen app dapat menampilkan obyek element. <br/> Langkah terakhir,
masukkan komponen cockpit pada template app, sehingga menjadi,

```html
<div class="container">
  <app-cockpit></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [element]=serverElement></app-server-element>
    </div>
  </div>
</div>
```

### Custom Properties Alias

Pada decorator @Input(), secara default, nama variabel-lah yang dapat kita
binding dengan menggunakan property binding. Contoh pada bagian sebelumnya
adalah, pada template app

```html
[element] = serverElement
```

dan pada decorator @Input didalam file TS server-element adalah,

```typescript
@Input() element: {type: string, name: string, content:string}
```

Namun kita dapat menggunakan **alias** sehingga nama dari property binding
element atau [element] dapat kita ganti sesuai dengan kebutuhan. Ubah kode
template app menjadi,

```html
<div class="container">
  <app-cockpit></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]=serverElement></app-server-element>
    </div>
  </div>
</div>
```

Perhatikan baris ke-8, property binding [element] kita ubah menjadi
[srvElement]. Tentunya dalam IDE akan mentrigger underline warna merah yang
menandakan sebuah error. Hal ini terjadi karena **srvElement** tidak dikenali
oleh template app. Untuk mengatasi permasalahan tersebut, kita ubah kode TS
server-element menjadi,

```typescript
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css']
})
export class ServerElementComponent implements OnInit {
  @Input('srvElement') element: {type: string, name: string, content:string}

  constructor() { }

  ngOnInit() {
  }

}
```

Perhatikan baris ke-9. Pada decorator @Input() kita berikan argment berupa
string **srvElement** yang merupakan alias dari variabel **element**. Pada
bagian ini diharapkan Anda sudah memahami bagaimana cara untuk melakukan akses
variabel atau property dari komponen lain. Selanjutnya, bagaimana caranya kita
akan memasukkan data dari komponen satu ke komponen yang lain ? Jawabnnya adalah
custom events!

## Custom Events

Pada bagian ini kita akan melakukan _passing data_ atau mengirim sebuah data
dari satu komponen ke komponen lain. Perhatikan apa yang kita lakukan pada
tahapan sebelumnya! Komponen cockpit merupakan form yang digunakan untuk
menambahkan server atau blueprint. Akan tetapi, hasil dari server atau blueprint
akan disimpan oleh komponen app dan ditampilkan secara satu persatu oleh
server-element. Bagimana cara kita melakukan hal tersebut ? Pertama tambahkan
fungsi onServerAdded() dan onBlueprintAdded() pada file TS komponen app.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [{type: 'server', name: 'Testserver', content:'Just a test'}];

  onServerAdded(serverData: {serverName: string, serverContent:string}) {
    console.log(serverData);
    this.serverElements.push({
      type: 'server',
      name: serverData.serverName,
      content: serverData.serverContent
    });
  }

  onBlueprintAdded(blueprintData: {blueprintName: string, blueprintContent:string}) {
    this.serverElements.push({
      type: 'blueprint',
      name: blueprintData.blueprintName,
      content: blueprintData.blueprintContent
    });
  }
}
```
Perhatikan argumen pada onServerAdded() dan onBlueprintAdded(). Argumen kedua fungsi tersebut merupakan sebuah obyek. Selanjutnya kita akan membuat event binding pada tempalate app. Lebih tepatnya pada selector ```<app-cockpit>```. Perhatikan kode template app dibawah ini,
```html
<div class="container">
  <app-cockpit
    (serverCreated)="onServerAdded($event)"
    (blueprintCreated)="onBlueprintAdded($event)"></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]=serverElement></app-server-element>
    </div>
  </div>
</div>
```
Pada baris ke-3 dan ke-4 kita membuat event binding untuk event bernama **serverCreated** dan **blueprintCreated**. **Event tersebut bukanlan event standar pada JS, maupun Angular, melainkan custom event!**. Selanjutnya kita perlu membuat variable atau lebih tepatnya sebuah property dari serverCreated dan blueprintCreated. Dimana kita akan membuat keduanya ? Bukan didalam file TS app, melainkan di file TS cockpit app. Masih ingat bahasan sebelumnya tentang decorator @Input() ?
<br/>
Modifikasi file TS cockpit menjadi,
```typescript
import { Component, OnInit, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  serverCreated = new EventEmitter<{serverName: string, serverContent:string}>();
  blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent
    })
  }

  onAddBlueprint() {
    this.blueprintCreated.emit({
      blueprintName: this.newServerName,
      blueprintContent: this.newServerContent
    })
  }

}
```
Perhatikan baris ke-9 dan ke-10. serverCreated dan blueprintCreated menginisiasi kelas EventEmitter dimana berfungsi untuk meng-**_emmit_** atau membroadcast variable atau property atau sebuah fungsi dari satu komponen ke komponen lain. Dalam kasus ini ```<app-cockpit>``` dipanggil didalam template komponen app (**cek template komponen app!**). Pada fungsi onAddServer() dan onAddBlueprint kita menggunakan fungsi **emit()** agar obyek yang kita buat dalam fungsi tersebut dapat dibaca oleh komponen lain. Perhatikan baris ke 1 bagian import! EventEmitter harus kita import dari @angular/core sebelum dapat kita gunakan. <br/>
Hingga tahapan ini, kita belum sepenuhnya dapat melakukan passing data ke komponen yang lain, kita masih memerlukan satu decorator, yaitu @Output(). Decorator @Output menjadikan variabel atau property yang kita buat dapat digunakan oleh komponen lain. Modifikasi kembali kode TS pada cockpit sehingga menjadi,
```typescript
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent:string}>();
  @Output() blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverCreated.emit({
      serverName: this.newServerName,
      serverContent: this.newServerContent
    })
  }

  onAddBlueprint() {
    this.blueprintCreated.emit({
      blueprintName: this.newServerName,
      blueprintContent: this.newServerContent
    })
  }

}
```
Decorator @Output kita tambahkan pada variabel atau property serverCreated dan blueprintCreated pada baris ke-9 dan ke-10. Jangan lupa, kita juga harus melakukan import kelas Output dari @angular/core. Perhatikan baris ke-1. Jalankan server Angular dan coba tambahkan server baru dan blueprint baru. Apa yang terjadi ?

### Aliasing Custom Event
Sama seperti @Input() kita juga dapat melakukan aliasing pada property custom event, yaitu pada @Output. Ubah baris ke-10 file TS cockpit menjadi,
```typescript
// Dari
@Output() blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
// Menjadi
@Output('bpCreated') blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
```
Kemudian kita juga harus merubah event binding pada template app menjadi,
```html
<div class="container">
  <app-cockpit
    (serverCreated)="onServerAdded($event)"
    (bpCreated)="onBlueprintAdded($event)"></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]=serverElement></app-server-element>
    </div>
  </div>
</div>
```
