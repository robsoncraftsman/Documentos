# Dicas de TypeScript

## Configurações do Compilador (tsconfig.json)

https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
https://www.typescriptlang.org/docs/handbook/compiler-options.html

### Não compilar em caso de erro

> noEmitOnError=true

### Habilitar a depuração de código TS no browser

> sourceMap=true

### Selecionar o diretório de output dos arquivos compilados

> outDir="./dist"

### Selecionar o diretório de origem dos arquivos fontes (TS)

> rootDir="./src"

## Comandos

### Instalar o TypeScript

> npm install --save-dev typescript ts-node @types/node

### Rodar compilador do TypeScript

> npx tsc

### Iniciar o TS no projeto (criar arquivo de configurações)

> npx tsc --init

### Verificar a versão

> npx tsc --version

### Monitorar e compilar arquivos .ts automaticamente

> npx tsc -w

### ESLint para TypeScript + Prettier

> npm install --save-dev eslint eslint-plugin-import eslint-plugin-node eslint-plugin-promise @typescript-eslint/eslint-plugin @typescript-eslint/parser prettier eslint-plugin-prettier eslint-config-prettier
