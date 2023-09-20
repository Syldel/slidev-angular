---
theme: the-unnamed #seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Angular
  Presentation for developers.
drawings:
  persist: false
transition: slide-left
title: Angular

themeConfig:
  default-headingBg: var(--slidev-theme-center-headingBg)
  default-headingColor: var(--slidev-theme-center-headingColor)
---

# Angular

Presentation for developers

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
transition: fade-out
---

# What is Angular?

Angular est un framework de Google, construite sur TypeScript qui permet de faire des SPA « Single Page Applications ».
<br>
<br>

---
transition: fade-out
---


# Table of contents

<Toc columns="3"></Toc>

---
layout: default
---

# Single Page Applications

Une single page application (SPA) est une application web constituée d'un document HTML unique. On parle d'application monopage : tout le contenu est codé sur une seule page, pour éviter de charger une nouvelle page à chaque interaction de l'utilisateur.
<br>
<br>

---
transition: fade-out
---

# AngularJS

AngularJS a été créé par Google en 2009.
En 2016 Google lance Angular 2, ils en ont profité pour réécrire intégralement le Framework. Ce qui fait que la première version est devenue « obsolète » et Google a décidé de la rebaptiser AngularJS afin de marquer une différenciation avec l’écosystème actuel.

Aujourd'hui (septembre 2023) nous sommes à la version 16 !

---
transition: fade-out
---

# MVC

Angular est basé sur une architecture du type MVC (Model-View-Controller) et permet donc de séparer les données, le visuel et les actions pour une meilleure gestion des responsabilités.

https://guide-angular.wishtack.io/angular/composants/lapproche-mvc

---
transition: fade-out
---

# Component

The following is a minimal Angular component.

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
```

To use this component, you write the following in a template:

```html
<hello-world></hello-world>
```

---
layout: default
---

# La programmation réactive ?

La programmation réactive se base sur le concept d'observateur. Si vous n'êtes pas familier avec ce principe, le principe est tout simplement que l'on définit des observables et des observateurs. Les observables vont émettre des événements qui seront interceptés par les observateurs.

```ts
const myObservable = Observable.of(42);

myObservable.subscribe((value) => {
  console.log(value);
}, (error) => {
  console.log(error);
});
```

https://makina-corpus.com/front-end/mise-en-pratique-rxjs-angular

---
layout: default
---

# Injection de dépendances (définition)

L'injection de dépendances (DI) est un modèle de conception logicielle qui permet de supprimer les dépendances codées en dur dans notre application.

L'injection de dépendances est utilisée pour passer une instance d'une classe à un objet dépendant. Par exemple, si nous avons une variable, par exemple client, dans notre composant, Angular se chargera de fournir un objet client réel au composant.

Angular utilise une classe appelée Injector pour faire du DI. Injector est une instance singleton dont le travail consiste à créer des instances de classes. L'injecteur utilise un ensemble de règles pour répondre à la demande de dépendance.

---
layout: two-cols
---

# Injection de dépendances

```ts
@Injectable()
export class DataService {
  constructor(protected http: HttpClient) {}

  public getArticles(): Observable<Article[]> {
    return this.http.get('http://localhost:3000/articles/').pipe(
      map(
        (jsonArray: Object[]) => jsonArray.map(jsonItem => Article.fromJson(jsonItem))
      )
    );
  }
}
```

::right::

```ts
@Component({
  selector: 'app-articles-list',
  templateUrl: './articles-list.component.html',
  styleUrls: ['./articles-list.component.scss']
})
export class ArticleListComponent {

  public articles: Article[];

  constructor(private data: DataService) { }

  ngOnInit() {
    this.data.getArticles().pipe(take(1)).subscribe(
      articles => this.articles = articles
    );
  }
}
```

---
layout: two-cols
---

# Utiliser @Input

Passer des données dans le composant

```ts
import {Component, Input} from '@angular/core';

@Component({
  selector: 'navbar',
  template: '<p>Bienvenue {{name}}</p>'
})
export class NavbarComponent {

  @Input() name: string;

}
```

::right::

Pour utiliser le composant dans un template :

```html
<navbar name="Sam"></navbar>
````

Si nous voulons passer des variables, l'écriture sera légèrement différente :

```ts
@Component({
  selector: 'app',
  template: '<navbar [name]="person.name"></navbar> <p>Hello World</p>'
})
```

---
layout: two-cols
---

# @Output et EventEmitter

```ts
import {Component, Input} from '@angular/core';

@Component({
  selector: 'count',
  template: `
    <p>{{n}}</p>
    <button (click)="up()"></button>
  `
})
export class CountComponent {
  n: number = 0;

  up() {
    this.n++;
  }
}
```

::right::

Envoyer la valeur en dehors du composant

```ts
@Component({
  selector: 'count',
  template: `
    <p>{{n}}</p>
    <button (click)="up()">Up</button>
  `
})
export class CountComponent {
  n: number = 0;
  @Output() numberChange: EventEmitter<number> = new EventEmitter();

  up() {
    this.n++;
    this.numberChange.emit(this.n);
  }
}
```

---
layout: default
---

# Cycle de vie dans les composants

Le cycle de vie d'un composant Angular désigne l'ensemble des étapes que suit le composant depuis sa création jusqu'à sa destruction.

- ngOnChanges : appelée chaque fois que des modifications sont apportées aux propriétés de l'objet d'entrée du composant.
- ngOnInit : appelée une seule fois après l'initialisation du composant.
- ngOnDestroy : appelée avant la destruction du composant.


---
layout: two-cols
---

# Les directives
- ngIf: Afficher et désafficher un élément

```ts
import {Component} from '@angular/core';

@Component({
  selector: 'app',
  template: `
    <p>{{result}}</p>
    <button (click)="up()">Up</button>
    <p *ngIf="result%2==0">Pair</p>
  `
})
export class AppComponent {
  result: number = 0

  up() {
    this.result++;
  }
}
```

::right::

- ngFor: Répéter un affichage
```ts 
import {Component} from '@angular/core';

@Component({
  selector: 'app',
  template: `
    <p *ngFor="let user of users">{{user.name}} : {{user.age}}</p>
  `
})
export class AppComponent {
  users: any[] = [
    {name: 'Sam', age: 45},
    {name: 'Jim', age: 33},
    {name: 'Ana', age: 17},
    {name: 'Lou', age: 4},
  ]
}
```

---
layout: default
---

# Routing

Créer un simple routeur

```ts
import { RouterModule, Routes } from '@angular/router';

import {OtherComponent} from './other.component';
import {MainComponent} from './main.component';

const routes: Routes = [
  { path: '', component: MainComponent },
  { path: 'other', component: OtherComponent }
];

export const routing = RouterModule.forRoot(routes);
```

---
layout: default
---

# Guard

```ts
import { inject } from "@angular/core";
import { Router } from "@angular/router";

export const AuthGuard = () => {
  return true
}
```

```ts
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [AuthGuard], // Utilisation du guard
  }
];
```

---
layout: two-cols
---

# Authentification

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private isAuthenticated = false;

  login() {
    this.isAuthenticated = true;
  }

  logout() {
    this.isAuthenticated = false;
  }

  isAuthenticated(): boolean {
    return this.isAuthenticated;
  }
}

```

::right::

```ts
import { inject } from "@angular/core";
import { Router } from "@angular/router";
import { AuthService } from "./auth.service";

export const AuthGuard = () => {
    const auth = inject(AuthService);
    const router = inject(Router);

    if(!auth.isAuthenticated()) {
        router.navigateByUrl('/login')
        return false
    }
    return true
}
```

---
layout: two-cols
---

# Les intercepteurs

```ts
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth-interceptor.service';

@NgModule({
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    }
  ]
})
export class AppModule { }
```

::right::

```ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(
    request: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    // Récupération du token d'authentification (à remplacer par votre code)
    const token = 'mon-token-d-authentification';

    // Ajout du token dans les entêtes de la requête
    const authReq = request.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`
      }
    });

    // Envoi de la requête avec les nouvelles entêtes
    return next.handle(authReq);
  }
}
```

---
transition: fade-out
---

# Les pipes

Comment utiliser le pipe async ?

Le pipe async est un opérateur RxJS qui est utilisé dans les templates Angular pour faciliter la gestion des Observables. L'opérateur async permet de souscrire à un Observable dans un template, de transformer les valeurs émises par l'Observable en valeurs synchronisées utilisables dans le template, et de se désabonner automatiquement lorsque le composant est détruit.

---
transition: fade-out
---

# Les pipes (exemples)

```html
<h1>Compteur : {{ counter$ | async }}</h1>

<ul>
  <li *ngFor="let user of users$ | async">
    {{ user.name }} ({{ user.email }})
  </li>
</ul>
```

```ts
this.users$ = this.http.get<User[]>('https://jsonplaceholder.typicode.com/users').pipe(
  // Transformation des données reçues en un tableau d'utilisateurs
  map(data => data.map(item => new User(item)))
);
```

```html
{{ objet | json }}
```

```html
<p>{{ 'HELLO WORLD' | lowercase }}</p>
<p>{{ 'hello world' | uppercase }}</p>
```
