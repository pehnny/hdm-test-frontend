# Solution au test
## TODO : TodoPage

Il est demandé au début du fichier d'implémenter **create** et **delete** avec une mention pour **update**. Cependant, la signature de la fonction **handleSave()** ne possède pas de paramètre pour transférer des données et les inputs associés aux tâches ne semblent pas fonctionner (aucune mise à jour visuelle quand j'écris). J'ai donc décidé de ne pas travailler dessus. A la place, j'ai ajouté un input pour envoyer une tâche directement avec un nom.

Les deux fonctions à implémenter sont simples. La première, ***delete**, consiste en un simple appel vers l'API et une mise à jour de la liste côté frontend. Pour ce faire, on comprend que le hook personnalisé `useFetch()` est d'une grande aide. L'implémentation donne :

```ts
await useFetch().delete(`/tasks/${id}`, {id: id});

setTasks(tasks.filter(task => task.id != id));
```

L'implémentation de la fonction **create** est un peu plus longue puisque j'ai ajouté un input :

```ts
let name = "";

if (inputRef.current?.value.trim()) {
    name = inputRef.current.value.trim()
    inputRef.current.value = "";
}

const newTask = await useFetch().post("/tasks", {name});

setTasks([...tasks, newTask]);
```

Je commence par utiliser un *hook* `useRef` que j'ai implémenté au début du *component* `const inputRef = useRef<HTMLInputElement>(null);` afin de récupérer la valeur de texte dans le champ au moment de cliquer sur le bouton. Si celle-ci est vide, j'envoie une tâche avec un nom correspondant à une string vide `""`. Sinon, j'envoie la tâche avec son nom et je prends soin de vider l'input. Ensuite, je fais l'appel vers l'API et je mets à jour la liste côté frontend comme pour l'autre fonction.
