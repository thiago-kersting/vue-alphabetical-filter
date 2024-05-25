# alphabetical-filter-vue

## PTBR:

### Instalação:

Para usar essa biblioteca, você deve usar o comando:

```
npm install alphabetical-filter-vue
```

após isso, para sua utilização, deve ser utilizado a sua importação no código com esses comandos:
```
import AlphabeticalFilter from 'alphabetical-filter-vue';
import 'alphabetical-filter-vue/dist/alphabeticalFilterVue.css';
```

### Props do componente:

o componente, deve receber 2 funções e 1 Array:
```
<AlphabeticalFilter :sendKey="scrollToKey" :sendMap="handleUpdateMap" :array-list="array" />
```

scrollToKey é uma função que recebe do componente a key clicada para realizar o scroll na página principal:
```
function scrollToKey(key) {
    const element = document.querySelector(`#${key}`);
    if (element) {
        const rect = element.getBoundingClientRect();
        const offset = 0; // subsitua pelo tamanho do seu header;
        const targetPosition = rect.top + window.scrollY + offset;
        window.scrollTo({
            top: targetPosition,
            behavior: 'smooth'
        });
    }
}
```

handleUpdateMap é uma função que recebe o Map baseado na lista enviada:
```
const mapList = ref(new Map());

const handleUpdateMap = (newMap) => {
    mapList.value = newMap;
}
```

array é um Array que deve ser mandado para o componente possuindo os itens que serão filtrados:
```
const array = [
  "Bruno", "Clara", "Daniel", "Eduarda",
  "Fernando", "Gabriela", "Henrique", "Isabela",
  "Kátia", "Lucas", "Maria", "Nuno", "Olívia",
  "Paulo", "Quiteria", "Rafael", "Sofia", "Tiago",
  "Úrsula", "Vitor", "Wanda", "Xavier", "Yasmin",
  "Zara", "André", "Bianca", "Carlos", "Débora",
  "Elias", "Fátima", "Gustavo", "Helena", "Ícaro",
  "Kleber", "Larissa", "Miguel", "Natália",
  "Otávio", "Pâmela", "Quintino", "Renata", "Samuel",
  "Tatiana", "Ulisses", "Valentina", "William"
]
```

### Exemplo de uso:
Localizado em src/App.vue