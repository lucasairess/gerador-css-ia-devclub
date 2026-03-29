# 🎨 AI CSS Generator — DevClub

> Interface web que transforma descrições em linguagem natural em código HTML + CSS animado, usando a API da Groq (LLaMA 3.3-70b) como motor de geração.

**🔗 [Teste a Demo Online Aqui!](https://lucasairess.github.io/css-ai-generator/)**


---

## 📋 Sumário

- [Descrição](#descrição)
- [Stack Tecnológica](#stack-tecnológica)
- [Como usar](#como-usar)
- [Estrutura de arquivos](#estrutura-de-arquivos)
- [API](#api)
- [Problemas conhecidos e segurança](#problemas-conhecidos-e-segurança)
- [Próximas melhorias](#próximas-melhorias)

---

## Descrição

O **AI CSS Generator** é um projeto educacional do **DevClub** que demonstra, na prática, como conectar uma interface web a uma LLM para geração de código em tempo real.

O usuário descreve o que imagina (ex: *"uma bola vermelha pulsando"*) e a IA retorna código HTML + CSS pronto, exibido simultaneamente em um bloco de código e em um preview ao vivo.

---

## Stack Tecnológica

| Tecnologia | Papel |
|---|---|
| **HTML5** | Estrutura semântica da interface |
| **CSS3** | Estilização, layout (Flexbox/Grid), animações |
| **JavaScript (ES2022)** | Lógica, `fetch` assíncrono, manipulação de DOM |
| **Google Fonts — Inter** | Tipografia principal |
| **Groq API** | Motor de IA — modelo `llama-3.3-70b-versatile` |

Sem frameworks, sem build step. Projeto 100% vanilla, abre direto no browser.

---

## Como usar

1. **Clone ou baixe** o repositório
3. ⚠️ **Configuração da API:**
   - Copie ou renomeie o arquivo `config.example.js` para `config.js`
   - Abra o novo `config.js` e insira sua API Key pessoal obtida em [console.groq.com](https://console.groq.com)
4. Abra o arquivo `index.html` em qualquer navegador moderno (Chrome, Firefox, Edge)
5. Digite uma descrição no campo de texto (ex: *"Botão verde com efeito hover"*)
6. Clique em **Gerar Código ⚡️**
7. Aguarde a resposta da IA — o código aparece à esquerda e o resultado ao vivo à direita

---

## Estrutura de Arquivos

```
css-ai-generator/
├── index.html          # Estrutura HTML da aplicação
├── styles.css          # Estilos globais + tokens CSS + layout
├── scripts.js          # Lógica de integração com a API Groq
├── config.example.js   # Exemplo de configuração da API
├── config.js           # (Você deve criar) Sua API Key protegida
└── README.md           # Esta documentação
```

---

## API

### Endpoint utilizado

```
POST https://api.groq.com/openai/v1/chat/completions
```

### Headers

```json
{
  "Content-Type": "application/json",
  "Authorization": "Bearer {SUA_API_KEY}"
}
```

### Body (simplificado)

```json
{
  "model": "llama-3.3-70b-versatile",
  "messages": [
    {
      "role": "system",
      "content": "Você é um gerador de código HTML e CSS. Responda SOMENTE com código puro..."
    },
    {
      "role": "user",
      "content": "{prompt do usuário}"
    }
  ]
}
```

### Resposta esperada

A API retorna um objeto `choices[]` onde `choices[0].message.content` contém o código HTML+CSS gerado, que é injetado diretamente no `srcdoc` do `<iframe>`.

---

## Problemas conhecidos e segurança

| # | Problema | Severidade | Correção |
|---|----------|------------|---------|
| 1 | **API Key exposta** | 🔴 Crítico | Corrigido: Movido para `config.js` (ignorado no git) |
| 2 | Sem estado de **loading** durante requisição | 🟠 Alto | Implementado na versão atual |
| 3 | Sem **tratamento de erro** da API | 🟠 Alto | Implementado na versão atual |
| 4 | `<p>` usado para bloco de código | 🟡 Médio | Usar `<pre><code>` para semântica correta |
| 5 | `button` sem propriedade `color` | 🟡 Médio | Herdava cor aleatória do browser |
| 6 | `textarea` sem `font-size` explícito | 🟡 Médio | Safari renderizava diferente |
| 7 | Sem botão de **copiar código** | 🟢 Baixo | Implementado na versão atual |

> ⚠️ **NUNCA** publique sua API Key no código-fonte em produção. Em projetos reais, utilize um servidor intermediário (backend/proxy) para fazer as chamadas à API mantendo a chave segura.

---

## Próximas melhorias (roadmap)

- [ ] Histórico de gerações salvo no `localStorage`
- [ ] Modo de edição do código gerado com highlight de sintaxe
- [ ] Suporte a múltiplos modelos de IA (seleção via dropdown)
- [ ] Exportar código gerado como arquivo `.html`
- [ ] Backend (Node.js/Python) para proteger a API Key
- [ ] Autenticação de usuário
