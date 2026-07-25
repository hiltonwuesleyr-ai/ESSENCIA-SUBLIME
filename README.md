# Essência Sublime — Projeto Completo

Site estático (HTML/CSS/JS puro, sem build) + sistema Node.js de
importação automatizada de fotos do catálogo.

## Estrutura do projeto

```
index.html                    → site (catálogo, quiz, páginas de perfume)
manual-marca.html             → manual de identidade visual
images/
  brand/                      → símbolo, monograma ES, logo, favicon
  perfumes/                   → fotos .webp dos perfumes (geradas pelo script)
    placeholder.svg           → placeholder premium, usado quando falta foto
scripts/
  import-images.js            → script de importação/otimização de imagens
perfumes.json                 → lista de perfumes (nome + arquivo)
image-sources.example.json    → modelo do mapa manual de URLs das fotos
package.json                  → dependências e comando "npm run import-images"
```

## Publicar o site

O site não precisa de build — é HTML puro. Suba a pasta inteira
(mantendo `index.html` na raiz e as pastas `images/` junto) na Vercel,
Netlify, GitHub Pages ou qualquer hospedagem estática.

```bash
# Vercel, a partir da raiz do projeto:
vercel
```

## Importar as fotos dos perfumes

O site já procura automaticamente por `images/perfumes/{id}.webp` para
cada perfume do catálogo (os ids estão em `perfumes.json`). Enquanto a
foto não existir, o site mostra sozinho um placeholder elegante — nada
fica vazio.

**1. Instalar dependências do script**
```bash
npm install
```

**2. Configurar de onde vêm as fotos** — duas opções, veja o passo a
passo completo dentro de `scripts/import-images.js` (função
`getImageUrl`, bem comentada):
- **Mapa manual**: copie `image-sources.example.json` para
  `image-sources.json` e cole a URL de cada foto — mais simples e
  mais confiável.
- **Integração automática**: API de busca de imagens, banco do
  distribuidor, S3 etc. — há um bloco de exemplo comentado pronto
  para adaptar.

**3. Rodar**
```bash
npm run import-images
```

Isso baixa, valida, converte para WebP, redimensiona (máx. 1200px) e
salva cada foto em `images/perfumes/{arquivo}.webp` — no MESMO lugar
de onde o site já lê. Ao final é gerado `import-report.json` com o
resumo completo (encontrados, não encontrados, erros, tempo de
execução), além de um log colorido no terminal.

Pode rodar quantas vezes quiser — reexecutar é seguro, os arquivos
são apenas sobrescritos.

## Nota sobre a estrutura de pastas

O prompt original do sistema de importação usava a convenção
`/public/images/perfumes/`, comum em frameworks como Next.js ou Vite
(onde tudo dentro de `public/` é servido a partir da raiz do site).
Como este projeto é um site estático puro — sem framework, sem build
— essa "raiz servida" já é a própria raiz do projeto. Por isso o
script foi ajustado para salvar direto em `images/perfumes/`, que é
exatamente o caminho que `index.html` já usa. Funcionalmente é a
mesma coisa; só não existe uma pasta `public/` extra e desnecessária
no meio do caminho.
