# Laboratorio: Git y GitHub

**Nombre:** ___________________________
**Compañero/a:** ___________________________

---

## Antes de empezar

### Herramientas necesarias

- **Git** instalado localmente
  - Linux: `sudo apt install git`
  - macOS: `xcode-select --install`
  - Windows: https://git-scm.com/download/win
- Cuenta en **GitHub**
- GCC y Make (para compilar el proyecto)

Verificá que git esté instalado:

```bash
git --version
```

### Cloná el repositorio

Una vez que aceptaste el assignment en GitHub Classroom, GitHub crea una copia del repositorio para vos. Copiá la URL de tu repo (botón verde **Code → HTTPS**) y ejecutá:

```bash
git clone <URL-de-tu-repo>
cd <nombre-del-repo>
```

> El nombre del directorio clonado es el nombre de tu repo en GitHub Classroom (ej: `lab-github-juan-perez`), no necesariamente `lab-github`.

Verificá el estado inicial:

```bash
git status
git log --oneline
```

Compilá el proyecto para asegurarte de que el estado inicial funciona:

```bash
make
./calculadora
```

Vas a ver que `multiplicar` devuelve 0 — eso es lo esperado, es lo que vas a implementar.

---

## Qué vas a aprender

| Concepto | Dónde lo practicás |
|---|---|
| Crear y cambiar branches | Parte I |
| Commits atómicos y mensajes claros | Parte I |
| Push y Pull Requests | Parte I |
| Agregar colaboradores | Parte II |
| Revisar y aprobar un PR | Parte II |
| Revertir un commit | Parte III |
| Resolver un conflicto de merge | Parte IV |

---

## Parte I — Tu primera branch y tu primer PR

### ¿Qué es una branch?

Una branch (rama) es una línea de desarrollo paralela. Permite trabajar en algo nuevo sin tocar el código que ya funciona en `main`. Cuando terminás, integrás los cambios con un Pull Request.

```
main     ──●──────────────────────────●──▶
            \                        /
feature      ●── ●── ●── ●── ●──●──/
```

**Regla de oro:** nunca trabajar directamente en `main`. Todo cambio va en una branch propia.

---

### Paso 1 — Crear la branch

```bash
git switch -c feature/mi-funcion
```

`git switch -c` crea la branch y te mueve a ella en un solo comando. Verificá en qué branch estás:

```bash
git branch
```

Deberías ver `* feature/mi-funcion`.

---

### Paso 2 — Implementar `multiplicar`

Abrí `operaciones.c`. Encontrá la función `multiplicar` y reemplazá el cuerpo:

```c
int multiplicar(int a, int b) {
    return a * b;
}

```

Acordate de sacar el `(void)a; (void)b;` también, ya no hace falta.

Compilá para verificar que no hay errores:

```bash
make
./calculadora
```

Deberías ver `3 * 5    = 15`.

---

### Paso 3 — El staging area y el primer commit

Antes de commitear, Git te pide que elijas explícitamente qué cambios incluir. Esto se llama **staging area** (área de preparación).

```
Working directory  →  git add  →  Staging area  →  git commit  →  Historial
```

Mirá qué cambió:

```bash
git diff
```

Agregá el archivo al staging:

```bash
git add operaciones.c
```

Verificá qué está en staging:

```bash
git status
```

Commiteá:

```bash
git commit -m "Implementa multiplicar con operador *"
```

**¿Qué es un buen mensaje de commit?**

Un mensaje de commit debe explicar **qué hace** el cambio, no *cómo* lo hace. Tiene que ser legible para un compañero que ve el historial sin ver el código.

| ❌ Mal | ✅ Bien |
|---|---|
| `fix` | `Corrige bug en esPar para negativos` |
| `cambios` | `Implementa multiplicar con operador *` |
| `wip` | `Agrega validación de entrada en sumar` |
| `asdfjkl` | `Refactoriza multiplicar: extrae variable resultado` |

---

### Paso 4 — Segundo commit

Un commit debe ser **atómico**: contener un solo cambio lógico, ni más ni menos. Si hiciste dos cosas distintas, deberían ser dos commits distintos.

Agregá un comentario en `operaciones.c` encima de `multiplicar` explicando brevemente cómo funciona (una línea). Commitealo por separado:

```bash
git add operaciones.c
git commit -m "Agrega comentario explicativo en multiplicar"
```

Mirá el historial:

```bash
git log --oneline
```

Deberías ver tus dos commits encima del commit inicial.

---

### Paso 5 — Push de la branch

Subí la branch a GitHub:

```bash
git push -u origin feature/mi-funcion
```

`-u origin feature/mi-funcion` configura el tracking: la próxima vez alcanza con `git push`.

---

### Paso 6 — Abrir un Pull Request

Un **Pull Request (PR)** es una propuesta para integrar los cambios de una branch a otra. Es el momento de revisión: antes de que el código entre a `main`, alguien puede leerlo, comentarlo y aprobarlo.

En GitHub:

1. Entrá a tu repositorio
2. Vas a ver un banner amarillo: **"feature/mi-funcion had recent pushes"** → hacé click en **Compare & pull request**
3. Asegurate de que la base sea `main` y el compare sea `feature/mi-funcion`
4. Escribí un título descriptivo: `Implementa función multiplicar`
5. En la descripción explicá brevemente qué hiciste y cómo verificaste que funciona
6. Hacé click en **Create pull request**

---

### Paso 7 — Mergear el PR

Como es tu propio repo, podés mergearlo vos mismo:

1. En el PR, hacé click en **Merge pull request → Confirm merge**
2. Hacé click en **Delete branch** (mantener el repo limpio)

Ahora traete los cambios a tu copia local:

```bash
git switch main
git pull
git log --oneline
```

Deberías ver tus commits en `main`.

```
PARTE_I_COMPLETA=
```
_(escribí SI cuando el PR esté mergeado)_

---

## Parte II — Colaboración con un compañero/a

Para esta parte necesitás coordinarte con alguien. Uno de ustedes va a ser el **owner** (dueño del repo) y el otro va a ser el **colaborador**.

---

### Paso 8 — Agregar al compañero como colaborador

El **owner** hace esto en GitHub:

1. Entrá a tu repositorio
2. Hacé click en **Settings** (pestaña superior derecha)
3. En el menú lateral, hacé click en **Collaborators**
4. Hacé click en **Add people**
5. Buscá el usuario de GitHub del compañero y agregalo
6. El compañero va a recibir un email con una invitación — debe aceptarla

---

### Paso 9 — El compañero clona y crea su branch

El **compañero** hace estos pasos:

```bash
git clone <URL-del-repo-del-owner>
cd <nombre-del-repo>
git switch -c sugerencia/<tu-nombre>
```

Por ejemplo: `sugerencia/maria` o `sugerencia/juan`.

---

### Paso 10 — El compañero hace un cambio

El **compañero** hace una mejora pequeña a `operaciones.c`. Puede ser cualquiera de estas:

- Agregar `const` a los parámetros que no se modifican (ej: `int sumar(const int a, const int b)`)
- Agregar un comentario que explique el algoritmo de `multiplicar`
- Mejorar el comentario existente

Después de hacer el cambio:

```bash
git add operaciones.c
git commit -m "Mejora: <descripción breve de lo que cambió>"
git push -u origin sugerencia/<tu-nombre>
```

---

### Paso 11 — El compañero abre un PR

El **compañero** abre un PR en el repo del owner:

1. Entrá al repo del owner en GitHub
2. Creá un PR desde `sugerencia/<nombre>` → `main`
3. Título: `Sugerencia: <qué cambiaste>`
4. Descripción: explicá el cambio y por qué lo propusiste

---

### Paso 12 — El owner revisa el PR

El **owner** revisa el PR:

1. En el PR, hacé click en la pestaña **Files changed**
2. Leé los cambios línea por línea
3. Hacé click en el ícono **+** al lado de una línea para dejar un comentario
4. Pedí al menos un cambio o hacé al menos una pregunta sobre el código
5. Hacé click en **Review changes → Request changes** y escribí un resumen

---

### Paso 13 — El compañero atiende el comentario

El **compañero** ve el comentario, hace el cambio pedido y lo pushea:

```bash
# (hace el cambio en el archivo)
git add operaciones.c
git commit -m "Atiende review: <qué se cambió>"
git push
```

El nuevo commit aparece automáticamente en el PR.

---

### Paso 14 — El owner aprueba y mergea

El **owner** vuelve al PR:

1. Revisá el nuevo commit en **Files changed**
2. Hacé click en **Review changes → Approve → Submit review**
3. Mergeá el PR: **Merge pull request → Confirm merge**
4. **Delete branch**

Traete los cambios:

```bash
git switch main
git pull
```

```
PARTE_II_COMPLETA=
```
_(escribí SI cuando el PR del compañero esté aprobado y mergeado)_

---

## Parte III — Revertir un error

En el día a día es común commitear algo que no debería estar. Git permite deshacerlo sin borrar la historia.

Antes de arrancar, asegurate de estar en `main`:

```bash
git switch main
```

---

### Paso 15 — Hacer el commit "malo"

Agregá esta función al final de `operaciones.c`:

```c
int dividir(int a, int b) {
    return a - b; /* bug intencional */
}
```

Commiteala con **exactamente** este mensaje:

```bash
git add operaciones.c
git commit -m "wip: experimento roto"
git push
```

---

### Paso 16 — Revertirlo con `git revert`

`git revert` crea un nuevo commit que deshace los cambios del commit anterior. A diferencia de `git reset`, no borra historia — es seguro en ramas compartidas.

> Si nunca usaste vim, configurá nano como editor antes de correr el revert:
> ```bash
> git config --global core.editor nano
> ```

```bash
git revert HEAD
```

Git abre un editor con el mensaje del commit de revert. Guardá y cerrá sin cambiar nada (en nano: `Ctrl+X → Y → Enter`, en vim: `:wq`).

Pusheá:

```bash
git push
```

Verificá el historial:

```bash
git log --oneline
```

Deberías ver el commit `wip: experimento roto` seguido del `Revert "wip: experimento roto"`.

**P1** — ¿Por qué `git revert` es preferible a `git reset --hard` cuando ya hiciste push de los cambios?

> R:

```
PARTE_III_COMPLETA=
```
_(escribí SI cuando el revert esté pusheado)_

---

## Parte IV — Resolver un conflicto

En esta parte vamos a forzar un conflicto de forma controlada para practicar su resolución.

> Importante: para que exista conflicto, ambas ramas deben editar la **misma línea** desde una base común.

El repositorio ya tiene una branch `feature/conflicto-demo` que implementa `esPar` de forma diferente a `main`. Cuando intentes mergearla, Git no va a poder resolver la diferencia solo — vas a tener que hacerlo vos.

---

### ¿Por qué ocurren los conflictos?

Un conflicto ocurre cuando dos branches modificaron la misma línea del mismo archivo. Git no sabe cuál versión es la correcta — esa decisión la tiene que tomar un humano.

```
main                    →  esPar: return (n % 2) == 0; /* version main */
feature/conflicto-demo  →  esPar: return (n & 1) == 0;
```

Ambas implementaciones son correctas. Tenés que elegir cuál conservar (o combinarlas).

---

### Paso 17 — Modificar función esPar
Primero asegurate de estar en `main` y actualizado:

```bash
git switch main
git pull
```

Editá la misma función para que quede así:

```c
int esPar(int n) {
    return (n % 2) == 0; /* version main */
}
```

Commit en `main`:

```bash
git add operaciones.c
git commit -m "Ajusta esPar en main con comentario de version"
```

### Paso 18 — Mergear desde origin y provocar el conflicto
Traé referencias remotas y mergeá la branch preparada:

```bash
git fetch origin
git merge origin/feature/conflicto-demo
```

Git debería reportar un conflicto:

```
CONFLICT (content): Merge conflict in operaciones.c
Automatic merge failed; fix conflicts and then commit the result.
```

---

### Paso 19 — Abrir el archivo y entender los markers

Abrí `operaciones.c`. Vas a ver algo así:

```c
int esPar(int n) {
<<<<<<< HEAD
    return (n % 2) == 0; /* version main */
=======
    return (n & 1) == 0;
>>>>>>> origin/feature/conflicto-demo
}
```

- `<<<<<<< HEAD` marca el inicio de **tu versión** (la de main)
- `=======` separa las dos versiones
- `>>>>>>> origin/feature/conflicto-demo` marca el fin de **la versión entrante**

---

### Paso 20 — Resolver el conflicto

Editá el archivo para que quede con **una sola versión limpia**, sin los markers. Podés elegir cualquiera de las dos, o combinarlas con un comentario:

```c
int esPar(int n) {
    return (n % 2) == 0;
}
```

Asegurate de que no quede ningún `<<<<<<<`, `=======` ni `>>>>>>>` en el archivo.

Compilá para verificar:

```bash
make
./calculadora
```

---

### Paso 21 — Commitear y pushear la resolución

```bash
git add operaciones.c
git commit -m "Resuelve conflicto en esPar: conserva version con operador %"
git push
```

**P2** — Describí con tus palabras qué diferencia hay entre las dos implementaciones de `esPar` que conflictuaban. ¿En qué caso podría importar elegir una sobre la otra?

> R:

```
PARTE_IV_COMPLETA=
```
_(escribí SI cuando el conflicto esté resuelto y pusheado)_

---

## Preguntas de reflexión

**P3** — Un compañero te dice: "yo hago un solo commit al final del día con todo lo que hice". ¿Qué problemas puede traer esa práctica? ¿Qué le dirías?

> R:

**P4** — ¿Cuál es la diferencia entre `git fetch` y `git pull`? ¿Cuándo preferirías usar uno sobre el otro?

> R:

**P5** — ¿Qué información debería tener la descripción de un Pull Request para que sea útil para quien lo revisa?

> R:

---

## Entrega

- `feature/mi-funcion` mergeada a `main` vía PR
- PR del compañero revisado, aprobado y mergeado
- Commit `wip: experimento roto` y su revert en el historial
- Conflicto de `esPar` resuelto en `main`
- Preguntas P1–P5 respondidas
- Push a `main`

El CI corre automáticamente. En la pestaña **Actions** podés ver qué checks pasan.
