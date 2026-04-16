# API REST — Node.js + TypeScript

Servidor HTTP minimalista construído com **Express 5** e **TypeScript 6**, servindo como base para o desenvolvimento da API REST do curso CodarSe.

---

## Visão Geral

| Item | Valor |
|---|---|
| Runtime | Node.js |
| Linguagem | TypeScript ^6.0.2 |
| Framework | Express ^5.2.1 |
| Porta padrão | `3333` |
| Rota inicial | `GET /` → `"Olá, DEV!"` |

---

## Estrutura de Pastas

```
.
├── src/
│   └── server/
│       ├── Server.ts       # Cria e configura a instância do Express
│       └── index.ts        # Entry point — sobe o servidor na porta 3333
├── dist/                   # Saída do compilador TypeScript (gerado, não versionar)
├── .gitignore
├── package.json
└── tsconfig.json
```

### Responsabilidade de cada arquivo

| Arquivo | Responsabilidade |
|---|---|
| `src/server/Server.ts` | Instancia o Express, registra a rota `GET /` e exporta o `server` |
| `src/server/index.ts` | Importa o `server` e chama `server.listen(3333)` |
| `tsconfig.json` | Configuração do compilador TS (`target`, `module`, `rootDir`, `outDir`) |
| `package.json` | Dependências e scripts npm |
| `.gitignore` | Exclui `node_modules/` e `dist/` do repositório |

---

## Como Rodar

### Pré-requisitos

- [Node.js](https://nodejs.org/) instalado (recomendado: v18+)
- Gerenciador de pacotes `npm` ou `yarn`

### Passo a passo

**1. Clone o repositório**
```bash
git clone <url-do-repositorio>
cd api-node-typescript
```

**2. Instale as dependências de produção**
```bash
npm install express
# ou
yarn add express
```

**3. Instale as dependências de desenvolvimento**
```bash
npm install -D typescript ts-node-dev @types/express eslint typescript-eslint @eslint/js @eslint/json @eslint/markdown @eslint/css globals
# ou
yarn add -D typescript ts-node-dev @types/express eslint typescript-eslint @eslint/js @eslint/json @eslint/markdown @eslint/css globals
```

> Alternativamente, instale tudo de uma vez a partir do `package.json`:
> ```bash
> npm install
> # ou
> yarn
> ```

**4. Certifique-se de ter o `tsconfig.json` na raiz**  
Se não tiver, gere com:
```bash
npx tsc --init
```
Ajuste `rootDir` para `./src` e `outDir` para `./dist`.

**5. Suba o servidor**
```bash
npm run dev
# ou
yarn dev
```

O servidor sobe em `http://localhost:3333`.  
O `ts-node-dev` monitora alterações nos arquivos `.ts` e reinicia automaticamente.

### Verificar se está funcionando

```bash
curl http://localhost:3333/
# Olá, DEV!
```

---

## Scripts Disponíveis

| Script | Comando | Descrição |
|---|---|---|
| `dev` | `ts-node-dev ./src/server/index.ts` | Sobe o servidor em modo desenvolvimento com hot-reload |

---

## Dependências

### Produção

| Pacote | Versão | Descrição |
|---|---|---|
| `express` | ^5.2.1 | Framework HTTP — roteamento e middlewares |

> O `package.json` declara `"type": "commonjs"`, garantindo que todos os arquivos `.js` gerados sejam tratados como módulos CommonJS pelo Node.js (`require` / `module.exports`).

### Desenvolvimento

| Pacote | Versão | Descrição |
|---|---|---|
| `typescript` | ^6.0.2 | Compilador TypeScript |
| `ts-node-dev` | ^2.0.0 | Executa `.ts` diretamente com hot-reload |
| `@types/express` | ^5.0.6 | Tipagens do Express para o TypeScript |
| `eslint` | ^10.2.0 | Linter para análise estática de código |
| `typescript-eslint` | ^8.58.2 | Integração do ESLint com TypeScript |
| `@eslint/js` | ^10.0.1 | Regras ESLint para JavaScript |
| `@eslint/json` | ^1.2.0 | Linting de arquivos JSON |
| `@eslint/markdown` | ^8.0.1 | Linting de arquivos Markdown |
| `@eslint/css` | ^1.1.0 | Linting de arquivos CSS |
| `globals` | ^17.5.0 | Variáveis globais por ambiente (node, browser etc.) |

---

## Configuração TypeScript

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "strict": true
  },
  "include": ["src"]
}
```

| Opção | Valor | Por que |
|---|---|---|
| `target` | `ES2020` | Async/await nativo, sem polyfills extras |
| `module` | `commonjs` | Formato `require()` — padrão do Node.js |
| `rootDir` | `./src` | Raiz dos arquivos `.ts` |
| `outDir` | `./dist` | Destino dos arquivos `.js` compilados |
| `esModuleInterop` | `true` | Permite `import express from 'express'` |
| `strict` | `true` | Ativa todas as verificações de tipo |
| `include` | `["src"]` | Restringe a compilação ao diretório `src/` |

---

## Observações Técnicas

### 1. Entry point correto

O servidor **não** está em `src/index.ts`. O entry point é:

```
src/server/index.ts
```

O script `dev` no `package.json` aponta para o caminho correto:

```json
"dev": "ts-node-dev ./src/server/index.ts"
```

> Apontar para `./src/index.ts` resulta em erro de módulo não encontrado.

### 2. `tsconfig.json` obrigatório

O `ts-node-dev` depende do `tsconfig.json` para resolver módulos corretamente.  
Sem ele, imports com caminhos relativos falham em tempo de execução.

Gere com:

```bash
npx tsc --init
```

Depois ajuste `rootDir` e `outDir` conforme a estrutura do projeto.

### 3. ESLint configurado com Flat Config

O projeto usa o novo formato **Flat Config** do ESLint (`eslint.config.mts`), que cobre múltiplos tipos de arquivo:

| Arquivos | Plugin | Configuração base |
|---|---|---|
| `.js`, `.ts` e variantes | `@eslint/js` + `typescript-eslint` | `js/recommended` + `tseslint.recommended` |
| `.json` | `@eslint/json` | `json/recommended` |
| `.jsonc` | `@eslint/json` | `json/recommended` |
| `.md` | `@eslint/markdown` | `markdown/recommended` |
| `.css` | `@eslint/css` | `css/recommended` |

**Regra global adicionada:**
```js
{ rules: { semi: ["warn", "always"] } }
```
> Exige ponto e vírgula em todo o código — exibe aviso (`warn`) caso esteja ausente.

**`defineConfig` helper:**  
A configuração usa `defineConfig` importado de `eslint/config`, utilitário que fornece autocomplete e validação de tipos na hora de montar o array de configurações.

**Por que Flat Config?**  
É o formato atual do ESLint (v9+), mais simples e sem arquivos `.eslintrc`. Toda configuração fica em um único `eslint.config.*`.

---

### 4. Express 5 — async nativo

No Express 5, erros lançados dentro de handlers `async` são capturados automaticamente e repassados ao middleware de erro — sem necessidade de `try/catch` em cada rota.

```ts
// Express 5 — erro propagado automaticamente
app.get('/', async (req, res) => {
  const data = await buscarDados(); // se lançar, Express 5 captura
  res.json(data);
});
```
