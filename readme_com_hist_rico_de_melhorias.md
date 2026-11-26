# Harmonia Instrumentos Musicais — Acessibilidade e Usabilidade Web

Este repositório contém a evolução completa da página da loja fictícia **Harmonia Instrumentos Musicais**, partindo da correção estrutural de HTML Semântico até a implementação de recursos interativos, validação acessível, animações inclusivas e design responsivo em formato SPA (*Single Page Application*).

---

## Histórico e Ordem das Alterações

As melhorias foram aplicadas de forma incremental, seguindo as diretrizes de acessibilidade da **W3C (WCAG 2.1 AA)** e boas práticas de desenvolvimento web:

### 1. Skip-Links (Links de Salto) e Estrutura Semântica
* **Problema inicial:** Usuários de teclado e leitores de tela precisavam percorrer todo o cabeçalho e menu antes de chegar ao conteúdo principal, além de links de salto quebrados apontando para IDs inexistentes.
* **O que foi feito:**
  * Implementação de links de salto visíveis ao receber foco (`:focus`) no topo da página (`#conteudo` e `#contato`).
  * Substituição de `<div>` genéricas por marcos semânticos (*Landmarks*): `<header>`, `<nav>` com `aria-label`, `<main>`, `<aside>` e `<footer>`.
  * Ajuste na hierarquia de cabeçalhos (`<h1>` a `<h3>`), transformação de listas simuladas com `<br>` em listas reais (`<ul>`/`<li>`), inclusão de `<caption>` e `<th scope>` na tabela comparativa e avisos explícitos `(abre em nova aba)` nos links externos.

### 2. Tema de Cores (Claro e Escuro com Contraste Acessível)
* **Problema inicial:** O alternador de tema não funcionava e os estilos utilizavam cores fixas sem suporte à alternância manual ou preferência do sistema.
* **O que foi feito:**
  * Criação de variáveis CSS (`--bg-pagina`, `--bg-superficie`, `--texto-principal`, etc.) com relação de contraste adequada tanto no modo claro quanto no modo escuro.
  * Implementação do alternador **🌓 Alternar Tema** utilizando `<input type="checkbox" class="sr-only">` e `<label for="btn-tema">` semântico (0 erros no validador W3C e totalmente operável via teclado com `Tab` + `Espaço`).
  * Destaque visual de foco (`outline`) ao navegar pelo botão de tema com o teclado.

### 3. Formulário de Cadastro Completo e Validação Acessível (SPA)
* **Problema inicial:** O formulário da página inicial não possuía `<label>` associados, carecia de agrupamento semântico e exibia mensagens genéricas ("Erro.") sem feedback para tecnologias assistivas.
* **O que foi feito:**
  * **Arquitetura SPA acessível:** Separação entre a vitrine principal (`#view-home`) e a página exclusiva de **Contato e Cadastro** (`#view-contato`), acessível pelo ícone **Cadastro** no cabeçalho e no menu, com gerenciamento de foco, atualização de `<title>`, `aria-current="page"` e região viva (`aria-live="polite"`).
  * **Estrutura do formulário:** Agrupamento lógico com `<fieldset>` e `<legend>`, associação explícita de todos os campos via `<label for="...">`, dicas de formato vinculadas por `aria-describedby`, indicação clara de `(obrigatório)` e campo anti-spam (*honeypot*) oculto para usuários e leitores de tela (`aria-hidden="true"` e `tabindex="-1"`).
  * **Validação JavaScript inclusiva:**
    * Validação em tempo real ao perder o foco (`blur`) e limpeza imediata do erro ao corrigir (`input`/`change`).
    * Atualização dinâmica de `aria-invalid="true"` / `"false"` e mensagens descritivas por campo.
    * Exibição de resumo de erros no topo do formulário (`role="alert"`) com links âncora que levam o foco diretamente ao campo inválido, rolando apenas para o início do formulário sem jogar o usuário de volta ao topo da página.

### 4. Animações Inclusivas (`prefers-reduced-motion`)
* **Problema inicial:** Ausência de feedback visual dinâmico e de controle para usuários sensíveis a movimento (vestibulares/cognitivos).
* **O que foi feito:**
  * **Banner de destaque:** Animação suave de entrada (`fade-in` via `@keyframes`).
  * **Cards de produtos:** Efeito de escala (`transform: scale(1.02)`) tanto no `:hover` do mouse quanto no `:focus-within` da navegação por teclado.
  * **Carrossel acessível:** Transição horizontal suave entre slides, controles por botões (*Anterior* / *Próximo*), indicadores de página com `aria-current="true"`, bloqueio de foco em slides ocultos (`inert`) e anúncios em `aria-live="polite"`.
  * **Respeito ao usuário:** Implementação da media query `@media (prefers-reduced-motion: reduce)`, que desativa animações, transições de escala, rolagem suave e o deslizamento do carrossel quando o usuário ativa a redução de movimento no sistema operacional.

### 5. Responsividade e Reflow (Mobile-First)
* **Problema inicial:** Larguras fixas na barra lateral (`.lateral`), menus sem quebra de linha e tabela comparativa causando estouro horizontal em telas pequenas.
* **O que foi feito:**
  * Tipografia fluida com `clamp()` para adaptação automática de títulos entre telas de `320px` e monitores widescreen.
  * Quebra de linha automática (`flex-wrap: wrap`) no cabeçalho, menu principal e opções de formulário.
  * Empilhamento vertical (`flex-direction: column`) entre o conteúdo principal e a barra lateral em telas menores que `768px`.
  * Envolvimento da tabela comparativa em um contêiner responsivo (`.tabela-responsiva` com `overflow-x: auto` e `tabindex="0"`), garantindo leitura e rolagem horizontal isolada sem quebrar o layout da página.
  * Ajuste de áreas de toque (*touch targets*) em botões e indicadores do carrossel para dispositivos móveis (`<= 480px`).

---

## Como Testar o Projeto

1. **Validação de HTML (W3C):** Submeta o `index.html` no [W3C Markup Validation Service](https://validator.w3.org/nu/) para confirmar ausência de erros de sintaxe e atributos ARIA.
2. **Auditoria Lighthouse (Chrome):** Abra o site em janela anônima (`Ctrl + Shift + N`), acesse o DevTools (`F12` → aba **Lighthouse**) e execute a auditoria de **Accessibility** em **Desktop** e **Mobile** (tanto na Home quanto na rota `#contato`).
3. **Navegação por Teclado:** Percorra toda a página usando apenas `Tab`, `Shift + Tab`, `Enter` e `Espaço`.
4. **Movimento Reduzido:** Ative *Reduzir movimento* / *Desativar efeitos de animação* nas configurações de acessibilidade do seu sistema operacional e recarregue a página.