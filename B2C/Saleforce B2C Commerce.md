### BUSINESS MANAGER

1. Introduction

> Business Manager é a ferramenta online do Salesforce B2C Commerce para configurar e gerenciar lojas B2C Commerce. Esta importante ferramenta é o centro de comando para seus recursos de comercialização, administração e desenvolvimento de sites do comércio B2C.

> Business Manager is the Salesforce B2C Commerce online tool for configuring and managing B2C Commerce storefronts. This important tool is the command center for your B2C Commerce merchandising, administration, and site development capabilities.

2. Personas

> Merchandisers configure site data, such as products, images, campaigns, promotions, and search settings.

> Administrators configure B2C Commerce site settings, import and export site data, and roll out code and data changes.

> Developers use Business Manager to access the storefront application directly to debug and troubleshoot problems, and configure development-specific settings.

3. Conceitos Gerais

    - **Cartridges**: 'Módulos que podem conter models, controllers, views ou apenas scripts. Um projeto pode ter várias Cartridges.
    
    - **Storefronts**: Parte visual do site como é vista pelo usuário.
    
    - **Content Assets**: Podem ser arquivos (imagens, vídeos) ou conteúdos em HTML que podem ser exibidos de forma dinâmica no storefront.
    
    - **Content Slots**: Espaços definidos pelo template onde pode ser exibido conteúdo dinâmico como produtos ou content assets.
    
### SFRA (Storefront Reference Architecture)

> **O que é SFRA?** A arquitetura de referência é um projeto que funciona como ponto de partida para o seu storefront, pois fornece as últimas novidades e correções da Commerce Cloud.

> Ela é composta por um cartridge base que contém apenas código comum a maioria dos sites e um módulo server que disponibiliza objetos de requisição, resposta e sessão, bem como o controle de rotas da sua aplicação.

### Criando uma conexão com servidor

> Com a extensão Prophet Debugger*, você poderá se conectar a sua sandbox e também depurar seu projeto. Para configurar a conexão, crie um arquivo dw.json dentro da pasta cartridges que fica localizada na raiz do projeto.

> Este arquivo contém os dados de autenticação e definições da sandbox usada no desenvolvimento, bem como o nome da sua versão de código.

### Conhecendo as Cartridges

> Client: Contém os scripts/estilos de front (devem ser compilados antes de realizar o upload *)

> Controllers: Contém as actions responsáveis pelas rotas de todas as páginas;

> Forms: Contém as definições de formulários xml das páginas (mensagens de campos, campos obrigatórios, tamanhos, etc);

> Models: Contém os objetos do sistema e suas regras de domínio;

> Scripts: Contém os scripts de backend acessíveis às controllers (utils, helpers, etc);

> Static: Contém os arquivos de front compilados e recursos estáticos (imagens, fontes);

> Templates: Contém as views ISML de todas as páginas e seus arquivos de tradução;

### Debugger

> Importante! Você deve compilar seus scripts de front antes de realizar o upload, utilizando um dos comandos abaixo via terminal:


  - npm run compile:scss - Compila todos os arquivos .scss para CSS;
  
  - npm run compile:js - Compila todos os arquivos .js e os agrega;
  
  - npm run compile:fonts - Copia todos os arquivos de fontes necessários. Geralmente este comando é executado uma única vez;
  
  Ou:
  
  - npm run build - Se você deseja executar todos os comandos acima.
  
### Estrutura do Site

> Ao construir seu site, a ordem que você coleta e gera o produto subjacente, conteúdo e informações do qualificador promocional importa. Em geral, você deve primeiro criar seu catálogo, produto e informações de preço e, em seguida, criar um programa de marketing para promover seus produtos.

1. Create the catalog and category structure, which determine site navigation.


2. Create the product information.
   - Import or create product details, including:
      * Product details       
        * Products
        * Product options
        * Variation attributes
        * Product sets
        * Bundles
        
      * Product attributes
      * Category assignments
      * Images
      * Inventory
      
      ... Link: https://documentation.b2c.commercecloud.salesforce.com/DOC1/index.jsp
