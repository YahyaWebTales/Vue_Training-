# Fonctionnalités avancées

## Utilisations de la directive `v-model` sur des composants

### Fonctionnement de `v-model` avec un composant

Lorsque vous utilisez `v-model` sur un composant enfant :

``` bash
<Enfant v-model="uneProp" />
Cela revient en fait exactement à :
```
<Enfant
	:modelValue="uneProp" 
	@update:modelValue="val => uneProp = val" />
Cela signifie que dans le composant enfant il faut émettre un événement update:modelValue et déclarer une prop modelValue :

```bash
<script setup>
defineProps<{
	modelValue: string;
}>();

defineEmits<{
	(e: 'update:modelValue', value: string): void;
}>();
</script>

<template>
	<input
		:value="modelValue"
		@input="$emit('update:modelValue', $event.target.value)"
	/>
</template>
```
### Changer le nom de la prop et de l’événement
Comme nous venons de le voir, par défaut, la prop reçue par le composant enfant est modelValue et l’événement qu’il doit émettre est update:modelValue.

Il est possible de modifier le nom en passant un argument à v-model :

```bash
<Enfant v-model:unNom="uneProp" />
```
Vous pouvez ensuite utiliser ce nom dans le composant enfant :

```bash
<script setup>
defineProps<{
	unNom: string;
}>();

defineEmits<{
	(e: 'update:unNom', value: string): void;
}>();
</script>

<template>
	<input
		type="text"
		:value="unNom"
		@input="$emit('update:unNom', $event.target.value)"
	/>
</template>
```

### Utiliser plusieurs directives v-model
Vous pouvez sans problème utiliser plusieurs directives v-model sur un composant :

```bash 
<Enfant v-model:une-prop="uneProp" 
v-model:une-autre-prop="uneAutreProp" />
```

Par exemple :

```bash
<Modal v-model:prenom="prenom" v-model:nom="nom" />
```

Et nous aurions dans le composant enfant :

```bash 
<template>
	<input
		type="text"
		:value="prenom"
		@input="$emit('update:prenom', $event.target.value)"
	/>
	<input 
		type="text"
		:value="nom"
		@input="$emit('update:nom', $event.target.value)"
	/>
</template>

<script setup lang="ts">
defineProps<{
	prenom: string;	
	nom: string;
}>();

defineEmits<{
	(e: 'update:prenom', value: string): void;
	(e: 'update:nom', value: string): void;
}>();
</script>

<style scoped lang="scss">
/* Ajoutez vos styles ici */
</style>
``` 

# Présentation des Slots

Les slots permettent de passer des parties de template d’un composant parent à un composant enfant.  
Alors que les props servent à passer des valeurs JavaScript, les slots permettent de passer du HTML.

---

## 1. Utilisation de Base des Slots

Pour utiliser les slots, vous devez placer le contenu à passer entre les balises d'ouverture et de fermeture du composant enfant.

### Exemple Parent

```bash
<Enfant>
  J’insère du contenu ici pour le passer au composant enfant.
</Enfant>
``` 
Exemple Composant Enfant
Dans le composant enfant, utilisez la balise <slot> pour indiquer où insérer le contenu :

```bash
<template>
  <slot></slot>
</template>
````

Ce mécanisme permet de projeter du contenu dans le composant enfant. Par exemple, si vous passez du texte "Click Me" dans un composant FancyButton, le rendu final sera :

```bash
<button>
  Click Me
</button>
```

2. Passer du HTML Complexe ou un Autre Composant
Il est possible de passer n'importe quel contenu HTML, par exemple une div :

```bash
<template>
  <Enfant>
    <div>
      J’insère du contenu ici pour le passer au composant enfant.
    </div>
  </Enfant>
</template>
````

Ou même un autre composant :

```bash
<template>
  <Enfant>
    <AutreComposant />
  </Enfant>
</template>
```
Dans ce cas, le composant AutreComposant sera rendu à l’endroit du slot.

3. Portée des Variables
Le contenu déclaré dans le composant parent a accès aux propriétés du composant parent.
Par exemple :

```bash
<template>
  <span>{{ message }}</span>
  <Enfant>
    {{ message }}
  </Enfant>
</template>
```
Ici, le contenu passé dans le slot pourra utiliser la propriété message déclarée dans le parent.
En revanche, ce contenu n’a pas accès par défaut aux propriétés du composant enfant.

4. Les Slots Nommés
Parfois, il est nécessaire de projeter plusieurs contenus à des emplacements précis dans le composant enfant.

Déclaration dans le Parent
Utilisez la balise <template> avec la directive v-slot pour nommer les slots :

```bash
<template>
  <Enfant>
    <template v-slot:header>
      <h1>Titre de mon site</h1>
    </template>
    <template v-slot:footer>
      <p>Qui sommes-nous ?</p>
    </template>
  </Enfant>
</template>
````
La notation raccourcie avec # peut également être utilisée :

```bash
<template>
  <Enfant>
    <template #header>
      <h1>Titre de mon site</h1>
    </template>
    <template #footer>
      <p>Qui sommes-nous ?</p>
    </template>
  </Enfant>
</template>
```

Utilisation dans le Composant Enfant
Dans le composant enfant, indiquez l’emplacement du contenu en utilisant l’attribut name sur la balise <slot> :

```bash
<template>
  <header>
    <slot name="header"></slot>
  </header>
  <footer>
    <slot name="footer"></slot>
  </footer>
</template>
```

5. Slots Dynamiques
Il est aussi possible de créer des identifiants dynamiques pour les slots dans les composants parents.

Syntaxe avec une Variable

```bash
<template>
  <template v-slot:[nomDynamique]>
    Du contenu HTML…
  </template>
</template>
```

Ou en utilisant la notation raccourcie :

```bash
<template>
  <template #[nomDynamique]>
    Du contenu HTML…
  </template>
</template>
```
6. Contenu par Défaut
Si aucun contenu n’est passé pour un slot nommé, vous pouvez définir un contenu par défaut dans le composant enfant :

```bash
<template>
  <slot name="header">Mon titre par défaut si aucun contenu n’est passé</slot>
</template>
```