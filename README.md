# Documentação Técnica: Slide

>Este projeto consiste em um carrossel robusto, desenvolvido em JavaScript puro (ES6+), focado em performance, acessibilidade e responsividade.

---

### 1. Arquitetura de Classes (Herança)

O projeto é dividido em duas camadas para respeitar o princípio de responsabilidade única:

>Classe Slide (Core): Gerencia a física, cálculos de distância, eventos de ponteiro (mouse/touch) e o movimento real do DOM.

>Classe SlideNav (Interface): Estende a classe base para adicionar controles visuais como setas de navegação e paginação dinâmica (bullets).

--- 

### 2. O Motor de Movimento (Physics & UX)

A fluidez do arraste é baseada em um sistema de coordenadas que sincroniza a posição do mouse com a aceleração do hardware.

>Diferenciação de Pointer: Suporte nativo para mousedown (Desktop) e touchstart (Mobile).

>Cálculo de Aceleração: Multiplicamos o movimento por um fator de 1.6 para que o slide acompanhe o ritmo natural do dedo/mouse.

>Threshold de Decisão: Implementamos um limiar de 120px. Se o usuário soltar o slide antes disso, ele retorna ao centro (reset); se passar disso, ele navega para o próximo item.

---

### 3. Responsividade e Performance
Diferente de slides comuns que recalculam a posição a todo momento, este framework trabalha com um Mapa de Posições.

>Slides Config: Ao iniciar ou redimensionar a tela, a classe mapeia o offsetLeft de cada item e calcula a margem necessária para centralizá-lo perfeitamente no wrapper.

>Debounce no Resize: Utilizamos um utilitário de debounce para garantir que o recálculo de posições só ocorra 200ms após o usuário terminar de redimensionar a janela.

>Aceleração de Hardware: Todo o movimento é feito via translate3d, movendo a renderização da CPU para a GPU.

---

### 4. Sincronização de Estado (Custom Events)

  Neste slide é usado Eventos Customizados para manter a interface atualizada sem criar dependências rígidas.

```JavaScript
this.changeEvent = new Event('changeEvent');
// Disparado no final de cada transição
this.wrapper.dispatchEvent(this.changeEvent);
Sempre que o slide muda, ele emite um sinal. A paginação (bolinhas) apenas "ouve" esse sinal e atualiza a classe .active no item correspondente ao índice atual.
```

---

### 5. Como Implementar (Guia de Uso)

```JavaScript
import SlideNav from './js/slide.js';

const slide = new SlideNav('.slide', '.wrapper');
slide.init();

// Adiciona as setas se necessário
slide.addArrow('.prev', '.next');

// Cria ou vincula uma paginação
slide.addControl('.custom-control');
```
