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