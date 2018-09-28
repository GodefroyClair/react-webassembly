# react-webassembly
tutorial to integrate web assembly into a react app

# dependencies

webpack: 4.20.2
webpack-dev-server: 3.1.9
webpack-cli: 3.1.1

babel: @babel/core: 7.1.0, @babel/preset-env: 7.1.0, @babel/preset-react: 7.0.0
babel-loader: 8.0.4

react: 16.5.2
react-dom: 16.5.2


## Steps

TODO:

- emscripten

- webpack + react + babel

## Plans

0) expliquer brièvement le project web assembly & la boite à outils emscripten

1) installer emscripten

2) exemple de compilation, explication concernant l'output, options de compilation

3) creation d'une app react from scratch (projet calculatrice)

4) intégration d'un .asm dans l'app

5) perspectives


## Tuto (reproductible)

1) installation emscripten

Sources:

https://github.com/kripken/emscripten

Site:

http://kripken.github.io/emscripten-site/

Install instructions:

https://kripken.github.io/emscripten-site/docs/getting_started/downloads.html

emsdk:

https://github.com/juj/emsdk

Etapes (pour Unix uniquement, se référer à la doc pour Windows)

git clone : récuper la sdk permettant de compiler avec emcc en web assembly
Pour gérer les mises à jour de emcc et des tools de sa toolchain, emsdk utilise un sys comparable à virtualenv.
Il faut si néccessaire mettre à jour le fichier des updates
```
$ emsdk update
```

puis installer ces mises à jour
```
$ emsdk install latest
```

Puis générer le fichier/env de config (au moins la première, puis à chaque update ou à chaque modif de l'env) on fait
```
$ emsdk activate latest
```
Dans chaque nouveau terminal
```
$ emsdk activate latest
```

Vérifier que emmcc est actif
emcc --version

On est placé dans le précédent dossier et notre ordi est prêt à compiler des sources en  C/C++ avec emcc


La compilation avec emcc

1) hello world
emmc -o

Les options:
d'optimisation
modularize
html
asm vs wasm
emcc utils.c -s WASM=1 -s EXTRA_EXPORTED_RUNTIME_METHODS='["ccall", "cwrap"]' -o utils.html

Transpilation
inputs : sources C / C++ (quelles contraintes?)
output : wasm ou asm files, js pour le binding & .data si l'algo vient avec des données
Aujourd'hui le wasm est bcp + intéressant

Le wasm et le js sont exécutés par deux threads qui peuvent communiquer

Comment se fait la communication entre les deux.

Appeler le wasm à partir du js

1) Tu déclares des fonctions C à la compilation avec les options ccall cwrap

Exemples:
a) compilation en exportant les méthodes ccall & cwrap et en indiquant les fonction à eexporter par l'otption EXPORTED_FUNCTION
En sortie, on a un fichier wasm &
soit un fichier js pour le binding
soit un fichier html pour tester
NB: si on veut utilser le js comme bibli, on doit utiliser l'option de compil Modularize=1

b) utilisation en js:
Comme d'hab on importe le module dans le fichier js
const Module = require('./wasm/utils.js')
// Module est une factory de promesse
const prom = Module() // args: c'est le contxte (canvas, ...)
Module :: (Context) => Prom WasmObj
prom.then((wObj) => wObj.cwrap("function_name", "output_type", ["input_type"])

2) Tu utilises l'injection de dépendances à partir du C++ en créant un handler
a) Dans le C++, on déclare les objets que l'on veut appeler en js avec les objets emscripten de Classe Class_ ou function...

On déclare le type et le nom des class/fonctions/valeur que l'on veut manipuler à partir du js

b) On compile avec l'option --bind pour ajouter bind et l'API emscripten.
On récupère utils.js

c) Comme dans l'exemple prédent le code fournit un constructeur Module de type:
Module :: (Context) => Prom WasmObj

La seule diff est qu'on appelle plus cwrap ou ccall mais directement la fonction ou l'objet qu'on a décrit en C++

Exemple d'appel d'une fonction
Exemple d'instantiation d'une classe

II) Intégration dans un env react + babel + webpack

Le problème:
ON aimerait placer le fichier dans les bibli de notre code et faire un import et récup les méthodes et VOILA
Mais ça ne marche pas car React marche avec babel pour optimiser le code et on output.js n'étant pas du common.js?, webpack & babel ne peut pas le transpiler
De plus, on utilise généralement react avec webpack et webpack ajoute la confusion puisque c'est lui qui décide quoi transpiler et ajoute des opérations sup (fusion ds des dfichiers bundle.js...)
ON va montrer comment on peut facilement avec
1) webpack 4 et babel gérer
2) webpack <=4 et babel

CODER

