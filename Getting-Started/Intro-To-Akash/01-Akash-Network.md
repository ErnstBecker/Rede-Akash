# Rede Akash

### Mercado de Computação Descentralizada

Akash é uma rede aberta que facilita a compra e venda seguras e eficientes de recursos computacionais. Construída especificamente para utilidade pública, é totalmente de código aberto e conta com uma comunidade ativa de colaboradores.

### Perguntas Comuns

#### Como usar o Akash?
Você pode começar diretamente pelo terminal ou usar o aplicativo web:
- **Defina sua imagem Docker, CPU, memória e armazenamento** em um arquivo `deploy.yaml`.
- **Defina seu preço**, receba propostas de fornecedores em segundos e selecione o menor preço.
- **Implemente sua aplicação** sem precisar configurar ou gerenciar servidores.
- **Escale sua aplicação** de um único contêiner para centenas de implantações.

#### O que é o Akash Compute Marketplace?
O Akash Compute Marketplace é onde os usuários alugam recursos de computação de provedores de nuvem antes de implantar um contêiner Docker na Akash Container Platform. O marketplace armazena registros on-chain de solicitações, lances, locações e pagamentos de liquidação usando o Akash Token (AKT). A blockchain do Akash é uma aplicação baseada em Tendermint construída no Cosmos SDK.

#### O que é a Plataforma de Contêineres Akash?
A Akash Container Platform é uma plataforma de implantação para hospedar e gerenciar contêineres, permitindo que os usuários executem qualquer aplicativo Cloud-Native. Akash é construída com um conjunto de serviços de gerenciamento de nuvem, incluindo Kubernetes, para orquestrar e gerenciar contêineres.

#### Qual é o custo para usar o Akash?
O custo de hospedar sua aplicação usando Akash é cerca de um terço do custo do Amazon AWS, Google Cloud Platform (GCP) e Microsoft Azure. Você pode verificar os preços ao vivo usando a ferramenta de comparação de preços.

#### Como eu uso o Akash?
Se você é novo no Akash, comece com nossos guias de implantação e siga a partir daí. A comunidade de Akash escreveu vários guias mais avançados para aprender sobre Akash: um guia para operadores de nós, um guia para validadores, um guia para provedores de nuvem e vários guias de implantação para rodar vários aplicativos no Akash.

#### Por que o Akash é diferente das outras plataformas de nuvem?
A nuvem descentralizada é uma mudança dos recursos de computação sendo de propriedade e operados pelas três grandes empresas de nuvem (Amazon, Google e Microsoft) para uma rede descentralizada de provedores de nuvem que utilizam software de código aberto desenvolvido por uma comunidade, levando ao surgimento de um novo modelo de computação em nuvem. Este modelo assume a forma de um mercado aberto composto por muitos fornecedores, todos competindo para fornecer recursos.

Como o Airbnb para hospedagem de servidores, o Akash é um mercado que lhe dá controle sobre o preço que você paga e as comodidades incluídas (chamamos isso de atributos). Akash oferece aos desenvolvedores de aplicativos uma ferramenta de linha de comando para alugar e implantar aplicativos diretamente de um terminal. Akash aproveita o enorme mercado de recursos subutilizados que estão parados nos estimados 8,4 milhões de data centers em todo o mundo. Qualquer aplicação em contêineres que esteja rodando na nuvem centralizada pode rodar mais rápido e a um custo menor na nuvem descentralizada Akash.

#### Por que o Akash é diferente das outras plataformas descentralizadas?
Akash hospeda contêineres onde os usuários podem executar qualquer aplicativo nativo da nuvem. Não há necessidade de reescrever toda a internet em uma nova linguagem proprietária, e não há dependência de fornecedor que impeça você de mudar de provedor de nuvem. O arquivo de implantação é transferido por meio de uma rede privada ponto a ponto isolada da blockchain. A transferência de ativos ocorre off-chain via mTLS para fornecer a segurança e o desempenho exigidos por aplicações críticas em execução na nuvem.

#### O que é a Linguagem de Definição de Pilha (SDL)?
Você pode definir os serviços de implantação, data centers, requisitos e parâmetros de preços em um arquivo "manifesto" (`deploy.yaml`), que efetivamente atua como um formulário para solicitar recursos da rede. O arquivo está escrito em uma linguagem declarativa chamada Stack Definition Language (SDL). SDL é um padrão de dados amigável para humanos para declarar atributos de implantação. SDL é compatível com o padrão YAML e semelhante aos arquivos Docker Compose.

#### Como configuro a rede para o meu contêiner?
A rede, permitindo a conectividade para e entre cargas de trabalho, pode ser configurada através do arquivo Stack Definition Language (SDL) para uma implantação. Por padrão, as cargas de trabalho em um grupo de implantação são isoladas - nada mais é permitido se conectar a elas. Esta restrição pode ser relaxada.

#### Preciso fechar e recriar meu deployment se eu quiser atualizar o deployment?
Não. Você pode atualizar sua implantação. No entanto, apenas alguns campos no arquivo de definição da pilha Akash são mutáveis. A imagem, o comando, os argumentos e o ambiente podem ser modificados, mas os recursos de computação e os critérios de colocação não podem.



