# Basisrezepte

In diesem Kapitel befinden sich Basisrezepte die in weitere Kapitel des Buches benötigt werden. Der Code von den Rezepten die hier gezeigt werden, kann später kopiert und angepasst werden, um weiter Rezepte zu implementieren. Ziel des Kapitels ist es dem Leser den schnellen Einstieg in die Basics von Angular 2 zu ermöglichen.

## Angular 2 Anwendung {#c02-angular-app}

### Problem

Ich möchte von Null auf eine Angular 2 Anwendung implementieren.

### Zutaten
* Leeres Verzeichnis für unsere Anwendung
* index.html-Datei, um die nötige Bibliotheken zu laden und die Anwendung zu starten
* main.ts-Datei mit der Komponentendefinition
* Webserver, um die index.html-Datei und die Angular Anwendung zu laden

### Lösung

{title="main.ts", lang=js}
```
import {bootstrap, Component, View} from 'angular2/angular2';

@Component({
  selector: 'my-app'
})
@View({
  template: '<div>Hello World!</div>'
})
class MyApp {}

bootstrap(MyApp);
```

Erklärung:

Diese Datei definiert die Haupt- und in dem Fall einzige Komponente unserer Anwendung. Sie ist ein ES6-Modul.
Die Hauptkomponente erkennt man dadurch, dass die der bootstrap-Funktion als Parameter übergeben wird (Zeile 11).

* Zeile 1: Hier importieren wir die nötige Abhängigkeiten aus dem angular2-Paket. Dafür nutzen wir eine ES6/ES2015 [import-Anweisung](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* Zeile 3-5: Hier definieren wir eine Komponente mittels [TypeScript-Decorator](#gl-decorator) und sagen Angular, dass unser Komponente im my-app-Tag gerendert werden soll
* Zeile 6-8: Definiert die View die zu der Komponente gehört. Das HTML im template-Attribut wird später zwischen <my-tag> und </my-tag> hinzugefügt
* Zeile 9: Ist die dazugehörige Klasse. In diesem Fall ist die Klasse leer da unsere Komponente keine Logic und keine Daten hat
* Zeile 11: Die Anwendung wird initialisiert (bootstrap)

{title="index.html", lang=html}
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Angular 2 Starter</title>
  <script src="https://code.angularjs.org/tools/system.js"></script>
  <script src="https://code.angularjs.org/tools/typescript.js"></script>
  <script src="https://code.angularjs.org/2.0.0-alpha.48/angular2.dev.js"></script>
  <script>
    System.config({
      transpiler: 'typescript',
        typescriptOptions: {
        emitDecoratorMetadata: true
      }
      package: {app: {defaultExtension: 'ts'}}
    });
    System.import('./app/main');
  </script>
</head>
<body>
  <my-app>Loading...</my-app>
</body>
</html>
```

Erklärung:

In dieser Datei laden wir unsere Abhängigkeiten und laden mittels SystemJS das Hauptmodul unserer Anwendung (main.ts).

* Zeile 6: Lade SystemJS. Wird benötigt um das Hauptmodul und weitere Module wie z. B. Angular 2 zu laden.
* Zeile 7: Lade den TypeScript-Compiler. Der wird benötigt, um on-the-fly die main.ts-Datei in JavaScript umzuwandeln, so dass der Browser sie lesen kann
* Zeile 8: Lade die Entwicklungsversion von Angular 2
* Zeile 10-15: Konfiguriere SystemJS, so dass es TypeScript-Dateien on-the-fly umwandeln kann
* Zeile 16: Lade unser Hauptmodul
* Zeile 20: Hier wird unsere Hauptkomponente gerendert. Initial wird "Loading..." angezeigt bis Angular initialisiert wird

Jetzt brauchen wir noch ein Webserver, um unsere Anwendung zu testen. Das Angular-Team empfiehlt den [live-server](https://www.npmjs.com/package/live-server) der automatisch die Seite im Browser Neuladen kann bei Änderungen. Wer kein live-reload mag kann auch den [http-server](https://www.npmjs.com/package/http-server) nutzen. Beide Webserver sind über npm installierbar.

Nach der Installation und Start des Webservers, können wir unsere Anwendung testen. Wenn im Browser Fenster "Hello World!" steht, ist alles gut gelaufen. Wenn das nicht der Fall ist, muss man wahrscheinlich noch das [es6-shim](https://www.npmjs.com/package/es6-shim) installieren und in der index.html-Datei laden. Angular 2 braucht gewisse ES6-Features die in ältere Browser, darunter auch Internet Explorer 11, nicht vorhanden sind.

### Diskussion

TypeScript on-the-fly in JavaScript umzuwandeln ist auf Dauer keine Option, da mit wachsende Datei Anzahl der Kompiliervorgan langsamer wird. Wir werden in spätere Rezepte sehen wie wir die TypeScript-Dateien vor kompilieren können.

Bei der Definition einer Komponente, darf sich kein Code zwischen @Component(), @View() und class befinden. Da sind nur Kommentare und/oder Leerzeilen erlaubt. Falls sich da Code befindet, werden wir folgenden Fehler in der Konsole sehen:

```text
No Directive annotation found on MyApp
```

Wobei "MyApp" der Namen der Komponenten ist. Das gilt für alle Komponenten unabhängig davon, ob sie Haupt- oder normale Komponenten sind.

### Code

Code auf Github: [02-Basic\_Recipes/01-Angular\_App](https://github.com/jsperts/angular2_kochbuch_code/tree/master/02-Basic_Recipes/01-Angular_App)

### Weitere Ressourcen

* Informationen über ES6/ES2015 [Module](http://exploringjs.com/es6/ch_modules.html)
* Mehr Informationen über Decorators in TypeScript und JavaScript (wahrscheinlich in ES7/ES2016) gibt es [hier](https://github.com/wycats/javascript-decorators)

## Komponente definieren {#c02-component-definition}

### Problem

Ich möchte weitere Komponenten nutzen, um meine Anwendung modularer zu gestalten.

### Zutaten
* [Angular 2 Anwendung](#c02-angular-app) (index.html-Datei und eine Hauptkomponente)
* Datei für die neue Komponente
* Anpassungen an der Hauptkomponente die wir im Rezepte Angular 2 Anwendung definiert haben

### Lösung

{title="component.ts", lang=js}
```
import {Component, View} from 'angular2/angular2';

@Component({
    selector: 'my-component'
})
@View({
    template: '<div>My Name is ...</div>'
})
class MyComponent {}

export default MyComponent;
```

Erklärung:

Diese Datei ist auch ein ES6-Modul ähnlich zu der main.ts-Datei die wir im [Angular 2 Anwendung Rezept](#c02-angular-app) gesehen haben.
Es gibt zwei Hauptunterschiede. Erstens rufen wir hier die bootstrap-Funktion nicht auf.
Zweitens müssen wir die Komponente exportieren damit wir sie in der Hauptkomponente nutzen können. Das sehen wir auf Zeile 11. Wir nutzen eine [ES6 export-Anweisung](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

Anpassungen an der Hauptkomponente

{title="main.ts", lang=js}
```
import {bootstrap, Component, View} from 'angular2/angular2';
import MyComponent from './component.ts';

@Component({
  selector: 'my-app'
})
@View({
  template: '<div>Hello World!</div><my-component></my-component>',
  directives: [MyComponent]
})
class MyApp {}

bootstrap(MyApp);
```

Erklärung:

Zeile 2: Hier importieren wir unsere Komponente
Zeile 8: Wir haben den Tag <my-component></my-component> an das Template hinzugefügt. Zu beachten ist, dass dieser Tag gleich zu dem Selektor in Zeile 4 von component.ts sein muss
Zeile 9: Mit dem Attribute "directives", definieren wir welche Direktiven im Template benutzt werden können

### Diskussion

Es ist sehr wichtig, dass wir alle Komponenten die wir im HTML-Template nutzen auch in dem directives-Array definieren. Tags die zu keiner Komponente gehören werden von Angular ignoriert und bleiben leer.

Die Komponente "MyComponent" ist jetzt ein Kindelement von unsere Hauptkomponente. In dem wir Komponenten importieren und dann in der View nutzen, können wir beliebig große Komponentenbäume erzeugen. Tatsächlich ist eine Angular 2 Anwendung nur ein Baum von Komponenten, mit der Hauptkomponente an der Spitze und beliebig viele Kindelemente.

### Code

Code auf Github: [02-Basic\_Recipes/02-Define\_Component](https://github.com/jsperts/angular2_kochbuch_code/tree/master/02-Basic_Recipes/02-Define_Component)
