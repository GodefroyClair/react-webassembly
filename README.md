# react-webassembly
tutorial to integrate web assembly into a react app

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

2) react + webpack
