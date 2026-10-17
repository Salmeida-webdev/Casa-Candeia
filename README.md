# Casa Candeia

Site institucional demonstrativo para portfólio, feito com SvelteKit, Svelte 5, TypeScript e Vite. A publicação é estática via `@sveltejs/adapter-static` e Netlify.

## Rodar localmente

```powershell
npm install
npm run dev
```

Abra o endereço mostrado pelo Vite no terminal.

## Verificações e build

```powershell
npm run check
npm run build
```

O resultado estático é criado em `build/`. O arquivo `netlify.toml` configura `npm run build` e `build` como diretório de publicação.

## Formulário

O formulário usa marcação compatível com Netlify Forms. A captação depende de publicar o site na Netlify com detecção de formulários habilitada; depois, envie um teste pelo domínio publicado e confira o painel de Forms. A confirmação local demonstra somente o fluxo da interface. Não há calendário, disponibilidade, preço ou reserva automática.

## Idiomas

O seletor alterna as cópias PT-BR e EN no cliente e memoriza a preferência local. Esta versão mantém a mesma URL nos idiomas; se a estratégia de SEO exigir páginas indexáveis separadas por idioma, publicar rotas dedicadas `/pt/` e `/en/` antes da indexação.

## Conteúdo e ativos

A Casa Candeia é uma hospedaria fictícia. O mapa usa o Parque Capivari, em Campos do Jordão (SP), como referência geográfica pública real na Serra da Mantiqueira; esse ponto **não é o endereço da pousada**. A acomodação e todas as imagens são conceituais. Não há depoimentos, preços, serviços ou avaliações apresentados como fatos de uma empresa real.

As imagens foram geradas para este projeto e otimizadas em WebP. A hero tem arquivos e enquadramentos próprios para desktop e celular. Na galeria, o mosaico muda a proporção dos quadros entre telas grandes e pequenas, com `object-position` ajustado por cena para preservar os assuntos principais.

A direção tipográfica usa Cormorant Garamond para títulos, DM Sans para leitura e IBM Plex Mono nas legendas técnicas. As fontes são empacotadas localmente e não dependem de um servidor de terceiros. A capa desktop e a capa mobile usam imagens com enquadramentos próprios; a galeria muda a composição em telas estreitas.

### Arquivos de imagem

- `static/images/hero-casa-candeia.webp` — paisagem principal.
- `static/images/hero-casa-candeia-mobile.webp` — versão vertical com enquadramento próprio para celular.
- `static/images/acomodacao-refugio.webp` — acomodação ilustrativa.
- `static/images/experiencia-trilha.webp` — trilha e serra.
- `static/images/galeria-cafe.webp` — detalhe de café da manhã.
- `static/images/serra-nevoa-manha.webp` — camadas da serra na luz da manhã.
- `static/images/recanto-janela.webp` — canto de leitura conceitual junto à janela.
- `static/images/caminho-da-serra.webp` — caminho em vegetação de montanha.
- `static/brand/` — logo horizontal e símbolo em SVG/PNG, versões escura e clara, favicon e notas de ativos.

## Windows PowerShell

Depois de salvar o ZIP em `Downloads`, extraia a pasta e instale as dependências:

```powershell
$downloads = Join-Path $env:USERPROFILE 'Downloads'
Expand-Archive -LiteralPath (Join-Path $downloads 'casa-candeia-institucional-v1.2.0.zip') -DestinationPath $downloads
Set-Location (Join-Path $downloads 'casa-candeia-v1.2.0')
npm install
npm run check
npm run dev
```

## Publicação

1. Crie um repositório Git para o projeto e envie a pasta.
2. Importe o repositório na Netlify (base directory na raiz do projeto).
3. Confirme build `npm run build` e publish directory `build`.
4. Teste as páginas, a troca de idioma e o formulário no domínio publicado.

Antes de usar para um negócio real, substitua os dados conceituais, defina política de privacidade e confirme consentimento, destinatário, retenção e operação do formulário.
