# Angular Cheat Sheet

## Structure de projet

Une structure typique d'un projet Angular peut ressembler à ceci :

```bash
src
├── app
│   ├── app-routing.module.ts
│   ├── app.component.html
│   ├── app.component.css
│   ├── app.component.ts
│   ├── app.module.ts
│   ├── components
│   │   ├── component1
│   │   │   ├── component1.component.html
│   │   │   ├── component1.component.css
│   │   │   ├── component1.component.ts
│   │   │   └── component1.component.spec.ts
│   │   ├── component2
│   │   │   ├── component2.component.html
│   │   │   ├── component2.component.css
│   │   │   ├── component2.component.ts
│   │   │   └── component2.component.spec.ts
│   │   └── component3
│   │       ├── component3.component.html
│   │       ├── component3.component.css
│   │       ├── component3.component.ts
│   │       └── component3.component.spec.ts
│   ├── services
│   │   ├── service1.service.ts
│   │   ├── service1.service.spec.ts
│   │   ├── service2.service.ts
│   │   ├── service2.service.spec.ts
│   │   ├── service3.service.ts
│   │   └── service3.service.spec.ts
│   └── models
│       ├── model1.model.ts
│       ├── model2.model.ts
│       └── model3.model.ts
├── assets
├── environments
├── index.html
├── main.ts
├── server.ts
├── styles.css
└── main.server.ts
```

### Description des fichiers

app : Contient les composants, services et modèles de l'application.
assets : Contient les fichiers statiques comme les images, les polices et les fichiers JSON.
environments : Contient les fichiers de configuration pour différents environnements (développement, production).
index.html : Le fichier HTML principal de l'application.
main.ts : Le point d'entrée de l'application Angular.
server.ts : Le point d'entrée pour le rendu côté serveur (SSR).
styles.css : Le fichier CSS principal de l'application.

### Notions Angular

- **Composants** : Un composant est une partie autonome de l'interface utilisateur. Chaque composant a son propre template, styles et logique. Le template est le code HTML qui définit l'apparence du composant, tandis que les styles définissent son apparence visuelle, et la logique est le code TypeScript qui gère le comportement du composant. Les fichiers spec.ts sont utilisés pour les tests unitaires des composants. Il est possible d'utiliser des services dans les composants pour partager des données et des fonctionnalités entre eux.
- **Services** : Un service est une classe qui encapsule la logique métier et les opérations de données. Les services sont généralement utilisés pour effectuer des appels HTTP, gérer l'état de l'application ou partager des données entre plusieurs composants. Ils sont injectés dans les composants via le système d'injection de dépendances d'Angular. L'interet d'utiliser des services est de séparer la logique métier de la logique de présentation, ce qui rend le code plus modulaire et réutilisable.
- **Modèles** : Un modèle est une interface ou une classe qui définit la structure des données utilisées dans l'application. Les modèles sont utilisés pour typer les données et faciliter la validation et la manipulation des données. Ils sont souvent utilisés en conjonction avec des services pour gérer les données de l'application. Les modèles permettent de s'assurer que les données respectent une certaine structure, ce qui facilite la maintenance et la compréhension du code. Exemple de modèle :

    ```typescript
    export interface User {
    id: number;
    name: string;
    email: string;
    }
    ```

- **Modules** : Un module est un conteneur pour un groupe de composants, services et autres éléments Angular. Chaque application Angular a au moins un module, le module racine (AppModule). Les modules permettent d'organiser le code et de le rendre plus modulaire et réutilisable. Ils facilitent également la gestion des dépendances et le chargement paresseux des fonctionnalités.
- **Routage** : Le routage permet de naviguer entre différentes vues ou composants dans une application Angular. Il est géré par le module Router d'Angular, qui permet de définir des routes, de gérer la navigation et de charger des composants en fonction de l'URL. Le routage est essentiel pour créer des applications monopages (SPA) où le contenu change dynamiquement sans recharger la page entière.
- **Directives** : Une directive est une classe qui modifie le comportement ou l'apparence d'un élément DOM. Il existe trois types de directives : les directives d'attribut, les directives structurelles et les directives personnalisées. Les directives permettent de réutiliser du code et d'appliquer des comportements spécifiques à des éléments HTML. Exemple de directive : ngIf, ngFor, *ngSwitch.
- **Pipes** : Un pipe est une fonction qui transforme les données avant de les afficher dans le template. Les pipes sont utilisés pour formater les données, effectuer des calculs ou filtrer des listes. Angular fournit plusieurs pipes intégrés, mais il est également possible de créer des pipes personnalisés. exemple les dates.

### Commandes Angular CLI

- `ng new nom-du-projet` : Crée un nouveau projet Angular.
- `ng serve` : Démarre le serveur de développement et ouvre l'application dans le navigateur.
- `ng generate component nom-du-composant` : Génère un nouveau composant.
- `ng generate service nom-du-service` : Génère un nouveau service.
- `ng generate module nom-du-module` : Génère un nouveau module.
- `ng generate directive nom-de-la-directive` : Génère une nouvelle directive.
- `ng generate pipe nom-du-pipe` : Génère un nouveau pipe.
- `ng build` : Compile l'application pour la production.
- `ng test` : Exécute les tests unitaires de l'application.

### Exemple de composant de base

```typescript
// les imports nécessaires
import { Component } from '@angular/core';
import { User } from '../models/user.model';
import { UserService } from '../services/user.service';
import { Router } from '@angular/router';

// le décorateur @Component permet de définir le composant
// selector : le nom du composant utilisé dans le template
// templateUrl : le chemin vers le fichier HTML du composant
// styleUrls : le chemin vers le fichier CSS du composant
// imports : les modules Angular nécessaires au composant
@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css'],
  imports: [CommonModule],
})

// la classe du composant
// elle doit implémenter l'interface OnInit pour utiliser la méthode ngOnInit
export class UserListComponent implements OnInit {
  // les propriétés du composant
  // users : un tableau d'utilisateurs
  users: User[] = [];

    // le constructeur du composant
    // ici on injecte le service UserService et le routeur Router
  constructor(private userService: UserService, private router: Router) {}

    // la méthode ngOnInit est appelée après la création du composant
    // elle est utilisée pour initialiser les données du composant
  ngOnInit() {
    // on appelle la méthode getUsers du service UserService pour récupérer la liste des utilisateurs
    // on s'abonne à l'observable retourné par la méthode getUsers = subscribe sert à écouter les changements de données
    // lorsque les données sont récupérées, on les assigne à la propriété users
    this.userService.getUsers().subscribe((data: User[]) => {
      this.users = data;
    });
  }

    // la méthode navigateToUserDetails est utilisée pour naviguer vers la page de détails d'un utilisateur
  // elle prend en paramètre l'identifiant de l'utilisateur
  // on utilise le routeur Router pour naviguer vers la route '/user/:id'
  navigateToUserDetails(userId: number) {
    this.router.navigate(['/user', userId]);
  }
}
```

### Exemple de service de base

```typescript
import { Injectable } from '@angular/core';

// Le décorateur @Injectable permet d'injecter ce service dans d'autres classes (composants, autres services...)
@Injectable({
  providedIn: 'root' // Rend le service disponible dans toute l'application sans devoir l'ajouter à un module manuellement
})
export class UserService {

  // Propriété pour stocker un utilisateur fictif
  private user = {
    id: 1,
    name: 'Alice',
    email: 'alice@example.com'
  };

  // Constructeur vide ici (on pourrait injecter d'autres services si besoin)
  constructor() { }

  // Méthode pour récupérer les informations de l'utilisateur
  getUser() {
    return this.user;
  }

  // Méthode pour mettre à jour les informations de l'utilisateur
  updateUser(name: string, email: string) {
    this.user.name = name;
    this.user.email = email;
  }

  // Méthode pour réinitialiser les informations de l'utilisateur
  resetUser() {
    this.user = {
      id: 0,
      name: '',
      email: ''
    };
  }
}```

### Exemple de modèle de base

```typescript
// Le modèle User définit la structure d'un utilisateur
export interface User {
  id: number; // Identifiant unique de l'utilisateur
  name: string; // Nom de l'utilisateur
  email: string; // Adresse e-mail de l'utilisateur
}
```

### Exemple de module de base

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser'; // Nécessaire pour toutes les applications web Angular
import { FormsModule } from '@angular/forms'; // Pour utiliser les formulaires avec ngModel, etc.

import { AppComponent } from './app.component'; // Composant principal de l'application
import { UserComponent } from './user/user.component'; // Un autre composant, exemple

@NgModule({
  declarations: [
    AppComponent,  // On déclare les composants qui appartiennent à ce module
    UserComponent
  ],
  imports: [
    BrowserModule, // On importe d'autres modules nécessaires ici
    FormsModule    // Exemple : gestion de formulaires
  ],
  providers: [
    // Ici, on pourrait ajouter des services si on veut les rendre disponibles uniquement pour ce module
  ],
  bootstrap: [AppComponent] // Composant principal à démarrer en premier (le root component)
})
export class AppModule { }
```

### Exemple de routeur de base

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router'; // Importation des outils de routage

import { HomeComponent } from './home/home.component'; // Composant de la page d'accueil
import { UserComponent } from './user/user.component'; // Composant de la page utilisateur
import { PageNotFoundComponent } from './page-not-found/page-not-found.component'; // Composant pour la page 404

// Définition des routes : chemin (path) + composant à afficher
const routes: Routes = [
  { path: '', component: HomeComponent }, // Chemin racine ('') → HomeComponent
  { path: 'user', component: UserComponent }, // Chemin '/user' → UserComponent
  { path: '**', component: PageNotFoundComponent } // Route "catch-all" → PageNotFoundComponent (404)
];

@NgModule({
  imports: [RouterModule.forRoot(routes)], // Configuration du router au niveau de l'application
  exports: [RouterModule] // Exportation pour pouvoir utiliser <router-outlet> et directives de routing dans l'app
})
export class AppRoutingModule { }
```

### Exemple de pipe de base

```typescript
import { Pipe, PipeTransform } from '@angular/core';

// Le décorateur @Pipe permet à Angular de reconnaître cette classe comme un Pipe
@Pipe({
  name: 'capitalize' // Le nom du pipe, utilisé dans le template : {{ valeur | capitalize }}
})
export class CapitalizePipe implements PipeTransform {

  // La méthode transform est obligatoire : elle définit ce que fait le pipe
  transform(value: string): string {
    if (!value) return '';
    
    // Transforme la première lettre en majuscule, garde le reste en minuscule
    return value.charAt(0).toUpperCase() + value.slice(1).toLowerCase();
  }
}
```

il existe aussi des pipes intégrés comme `DatePipe`, `CurrencyPipe`, `DecimalPipe`, etc. qui permettent de formater les dates, les devises et les nombres qui s'intègrent directement dans les templates.

```html
<p>{{ 'hello world' | capitalize }}</p> <!-- Affiche "Hello world" -->
<p>{{ today | date:'fullDate' }}</p> <!-- Affiche la date complète -->
<p>{{ price | currency:'EUR' }}</p> <!-- Affiche le prix formaté en euros -->
```

### Exemple de directive de base

```typescript
import { Directive, ElementRef, Renderer2, HostListener } from '@angular/core';

// Le décorateur @Directive indique à Angular que cette classe est une directive
@Directive({
  selector: '[appHighlight]' // Le sélecteur utilisé dans le template HTML : <div appHighlight></div>
})
export class HighlightDirective {

  constructor(private el: ElementRef, private renderer: Renderer2) {
    // ElementRef donne accès à l'élément DOM
    // Renderer2 est utilisé pour modifier le DOM de manière sécurisée
  }

  // Écoute l'événement "mouseenter" sur l'élément
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow'); // Appelle la fonction highlight avec la couleur jaune
  }

  // Écoute l'événement "mouseleave" sur l'élément
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(''); // Remet la couleur de fond d'origine
  }

  // Méthode privée pour changer la couleur de fond de l'élément
  private highlight(color: string) {
    this.renderer.setStyle(this.el.nativeElement, 'backgroundColor', color);
  }
}
```

et voici un exemple de directive dans le template HTML :

```html
<div appHighlight>
  Passez la souris sur moi pour voir l'effet de surbrillance !
</div>
```

Il existe des directives structurelles comme `*ngIf`, `*ngFor`, et `*ngSwitch` qui modifient la structure du DOM en ajoutant ou supprimant des éléments. Par exemple, `*ngIf` affiche un élément seulement si une condition est vraie, et `*ngFor` répète un élément pour chaque élément d'une liste.

exemple de `*ngIf` :

```html
<div *ngIf="isLoggedIn">
  Bienvenue, utilisateur !
</div>
```

exemple de `*ngFor` :

```html
<ul>
  <li *ngFor="let user of users">
    {{ user.name }}
  </li>
</ul>
```

## Recette : Construire un fichier de test Angular pas à pas

> **Objectif :** disposer d’une checklist simple pour écrire, exécuter et maintenir des tests unitaires/environnementaux pour vos composants **et** services Angular.
>
> **Techno utilisées :** Jasmine (ou Jest), TestBed, HttpClientTestingModule, HttpTestingController.

---

### 🗺️ Vue d’ensemble du flux AAA

| Étape       | But                                     | API principale                                       |
|-------------|-----------------------------------------|------------------------------------------------------|
| **Arrange** | Préparer l’environnement et les données | `TestBed`, `ComponentFixture`, stubs/spies           |
| **Act**     | Lancer l’action à tester                | `detectChanges()`, appels de méthode, événements DOM |
| **Assert**  | Vérifier le résultat attendu            | `expect()` + matchers Jasmine/Jest                   |

Gardez ce triptyque en tête : il structure **chaque** `it()` que vous écrirez.

---

### 1️⃣ Préparer l’environnement

```ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule } from '@angular/common/http/testing';
```

1. **Choisissez le type de test**
   - *Composant* ? Vous aurez besoin de `ComponentFixture`.
   - *Service HTTP* ? Ajoutez `HttpClientTestingModule`.
2. **Imports/Declarations/Providers**
   - Composant : `declarations: [MyComponent]` (ou `imports: [MyStandaloneComponent]`).
   - Service : `providers: [MyService]`.
3. **compileComponents()** si votre composant a un template externe.

```ts
beforeEach(async () => {
  await TestBed.configureTestingModule({
    imports: [HttpClientTestingModule],
    providers: [SearchService]
  }).compileComponents();
});
```

---

### 2️⃣ Créer l’instance / la fixture

#### Composant
```ts
let fixture: ComponentFixture<MyComponent>;
let component: MyComponent;

fixture  = TestBed.createComponent(MyComponent);
component = fixture.componentInstance;
```

#### Service
```ts
let service: SearchService;
service = TestBed.inject(SearchService);
```

> **Astuce** : pour un service HTTP, récupérez aussi le contrôleur :
> ```ts
> const httpMock = TestBed.inject(HttpTestingController);
> ```

---

### 3️⃣ Arrange – préparer les données

- Renseignez les `@Input` ou les propriétés publiques :
  ```ts
  component.tag = { id: '1', name: 'Tag', slug: 'tag' };
  ```
- Mettez en place vos spies/mocks (`spyOn(service, 'method').and.returnValue(...)`).
- Pour les services HTTP : préparez un tableau de réponses factices.

---

### 4️⃣ Act – déclencher l’action

| Cas              | Action typique                                          |
|------------------|---------------------------------------------------------|
| Changer un input | `component.inputProp = value; fixture.detectChanges();` |
| Méthode service  | `service.myMethod(arg).subscribe(...)`                  |
| Événement DOM    | `button.click(); fixture.detectChanges();`              |

---

### 5️⃣ Assert – vérifier le résultat

#### DOM / Composant
```ts
const span: HTMLSpanElement = fixture.nativeElement.querySelector('span');
expect(span.textContent.trim()).toBe('Tag');
```

#### Service HTTP
```ts
const req = httpMock.expectOne(r => r.url.endsWith('/api/search') && r.params.get('q') === 'angular');
req.flush(mockData);
expect(received).toEqual(mockData);
```

N’oubliez pas **`httpMock.verify()`** dans un `afterEach` pour détecter les requêtes non traitées.

---

### 6️⃣ Nettoyage et bonnes pratiques

- `afterEach(() => httpMock.verify());` pour les tests HTTP.
- Utilisez `fdescribe`/`fit` pour focaliser pendant le dev, jamais en CI.
- Paramétrez vos cas répétitifs :
  ```ts
  const cases: [string, string][] = [
    ['red',   'bg-red-100'],
    ['green', 'bg-green-100']
  ];
  cases.forEach(([color, cls]) => { /* … */ });
  ```
- Pensez à `fakeAsync`/`tick()` si v