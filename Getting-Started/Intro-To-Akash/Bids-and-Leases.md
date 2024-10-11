# Lances e Arrendamentos

# Como funciona o Marketplace?

O Akash Marketplace gira em torno das Implantações, que descrevem completamente os recursos que um inquilino está solicitando da rede. As Implantações contêm Grupos, que são uma agrupação de recursos destinados a serem alugados juntos de um único provedor.

# Implantar aplicações na Akash envolve dois tipos de usuários:

1. O Inquilino: a entidade que implanta a aplicação.
2. O Provedor: a entidade que hospeda a aplicação

# O que é um Leilão Reverso?

Akash usa um leilão reverso. Os inquilinos definem o preço e os termos de sua implantação, e os provedores de nuvem fazem lances nas implantações.

# Em um leilão reverso muito simples:

1. Um inquilino cria pedidos.
2. Os fornecedores fazem lances nos pedidos.
3. Os inquilinos escolhem as propostas vencedoras e criam os contratos de arrendamento.

# Ciclo de Vida de Implantação do Akash

# O ciclo de vida de uma implantação típica de aplicativo é o seguinte:

1. O inquilino cria um arquivo.yml composto por marcação SDL que descreve os recursos necessários para executar sua aplicação. Isso é conhecido como uma implantação. 
2. O inquilino submete essa definição à blockchain. 
3. A submissão deles gera um pedido no mercado. 
4. Os fornecedores que gostariam de cumprir esse pedido fazem uma oferta. 
5. Após algum período de tempo, uma proposta vencedora para o pedido é escolhida e um contrato de arrendamento é criado. 
6. Uma vez que um contrato de locação tenha sido criado, o inquilino envia um manifesto ao provedor. 
7. O provedor executa as cargas de trabalho conforme instruído pelo manifesto. 
8. A carga de trabalho está em execução - se for uma aplicação web, pode ser visitada 
9. O prestador ou inquilino eventualmente encerra o contrato de locação, encerrando a carga de trabalho. 

# Pagamentos

Os aluguéis são pagos do proprietário da implantação (inquilino) para o provedor através de um mecanismo de depósito e retirada. 

Os inquilinos são obrigados a apresentar um depósito ao criar uma implantação. Os alugueres serão pagos passivamente a partir do saldo deste depósito. A qualquer momento, um fornecedor de leasing pode retirar o saldo devido a eles deste depósito. 

Se os fundos disponíveis no depósito chegarem à zero, um prestador pode encerrar o contrato de locação, portanto, um inquilino que deseja manter o contrato ativo deve adicionar fundos ao seu depósito, pois isso garante que seu contrato não termine prematuramente. Quando um desdobramento é encerrado, a parte não gasta do saldo será devolvida ao inquilino. 

# Contas Escrow

As contas escrow são um mecanismo que permite pagamentos baseados no tempo de uma conta para outra sem micropagamentos bloco a bloco. Eles também suportam manter fundos em uma conta até que um evento arbitrário ocorra. 

# As contas escrow são necessárias em Akash por dois motivos principais: 

1. Os arrendamentos no Akash são precificados em blocos - a cada novo bloco, um pagamento do inquilino (proprietário da implantação) ao provedor (detentor do arrendamento) é devido. Considerações de desempenho e segurança proíbem a abordagem ingênua de transferir tokens em cada bloco. 
2. Licitar em um pedido não deve ser gratuito (for various reasons, including performance and security). Akash exige um depósito para cada lance. O depósito é devolvido ao licitante quando a licitação é encerrada. 

# Depósitos de Licitação

Fazer uma oferta em um pedido requer o pagamento de um depósito. O depósito será devolvido à conta do fornecedor quando a oferta passar para o estado FECHADO. 
Os depósitos de licitação são implementados com um módulo de conta escrow. Veja aqui para mais informações. 

# Atributos Auditados 

Atributos auditados permitem que os usuários que implantam aplicativos sejam mais seletivos sobre quais provedores podem executar seus aplicativos. Qualquer pessoa na Akash Network pode atribuir esses atributos aos Provedores através de uma transação on-chain.


## Parâmetros On-Chain

| Nome                  | Valor Inicial | Descrição                                    |
|-----------------------|---------------|----------------------------------------------|
| deployment_min_deposit | 0.5akt       | Depósito mínimo para realizar implantações. Alvo: ~US$2,2 |
| bid_min_deposit       | 0.5akt       | Quantia de depósito necessária para dar um lance. Alvo: ~US$2,2 |

## Transações

### DeploymentCreate
Cria uma implantação, abrindo grupos e ordens para ela.

**Parâmetros**
| Nome           | Descrição                                    |
|----------------|----------------------------------------------|
| DeploymentID   | ID da Implantação.                           |
| DepositAmount  | Quantia do depósito. Deve ser maior que deployment_min_deposit. |
| Version        | Hash do manifesto enviado para os provedores.|
| Groups         | Uma lista de descrições de grupo.            |

### DeploymentDeposit
Adiciona fundos ao saldo de uma implantação.

**Parâmetros**
| Nome           | Descrição                                    |
|----------------|----------------------------------------------|
| DeploymentID   | ID da Implantação.                           |
| DepositAmount  | Quantia do depósito. Deve ser maior que deployment_min_deposit. |

### GroupClose
Fecha um grupo e quaisquer ordens relacionadas. Enviado pelo locatário.

**Parâmetros**
| Nome | Descrição    |
|------|--------------|
| ID   | ID do Grupo. |

### GroupPause
Coloca o grupo no estado PAUSED e fecha quaisquer ordens relacionadas. Enviado pelo locatário.

**Parâmetros**
| Nome | Descrição    |
|------|--------------|
| ID   | ID do Grupo. |

### GroupStart
Muda o estado de um grupo de PAUSED para OPEN. Enviado pelo locatário.

**Parâmetros**
| Nome | Descrição    |
|------|--------------|
| ID   | ID do Grupo. |

### BidCreate
Enviado por um provedor para fazer um lance em uma ordem aberta. O depósito será devolvido quando o lance mudar para o estado CLOSED.

**Parâmetros**
| Nome     | Descrição                                    |
|----------|----------------------------------------------|
| OrderID  | ID da Ordem                                  |
| TTL      | Número de blocos para os quais este lance é válido |
| Deposit  | Quantia do depósito. bid_min_deposit se vazio.|

### BidClose
Enviado pelo provedor para fechar um lance ou uma locação de um lance existente.

Ao fechar uma locação, o grupo do lance será colocado no estado PAUSED.

**Parâmetros**
| Nome   | Descrição    |
|--------|--------------|
| BidID  | ID do Lance  |

## Transições de Estado

| Objeto | Estado Anterior | Novo Estado |
|--------|------------------|-------------|
| Bid    | ACTIVE           | CLOSED      |
| Lease  | ACTIVE           | CLOSED      |
| Order  | ACTIVE           | CLOSED      |
| Group  | OPEN             | PAUSED      |

### LeaseCreate
Enviado pelo locatário para criar uma locação. Cria uma locação a partir do lance dado. Define todos os lances não vencedores para o estado CLOSED (depósito devolvido).

**Parâmetros**
| Nome  | Descrição                |
|-------|--------------------------|
| BidID | Lance para criar uma locação a partir de |

### MarketWithdraw
Retira os saldos ganhos fornecendo locações e depósitos de lances que expiraram.

**Parâmetros**
| Nome   | Descrição                           |
|--------|-------------------------------------|
| Owner  | ID do provedor para o qual retirar fundos. |

## Modelos

### Deployment

| Nome        | Descrição                                    |
|-------------|----------------------------------------------|
| ID.Owner    | Endereço da conta do locatário               |
| ID.DSeq     | Número de sequência arbitrário que identifica a implantação. Padrão: altura do bloco. |
| State       | Estado da implantação.                       |
| Version     | Hash do manifesto enviado aos provedores.    |

**Estado**
| Nome   | Descrição              |
|--------|------------------------|
| OPEN   | Ordens podem ser criadas. |
| CLOSED | Todos os grupos estão fechados. Terminal. |

### Group

| Nome             | Descrição                                    |
|------------------|----------------------------------------------|
| ID.DeploymentID  | ID de Implantação do grupo.                  |
| ID.GSeq          | Número de sequência arbitrário. Incrementado internamente, começando em 1. |
| State            | Estado do grupo.                             |

**Estado**
| Nome   | Descrição                           |
|--------|-------------------------------------|
| OPEN   | Tem uma ordem aberta ou ativa.      |
| PAUSED | Lance fechado pelo provedor. Pode ser reiniciado. |
| CLOSED | Nenhuma ordem aberta ou ativa. Terminal. |

### Order

| Nome          | Descrição                                    |
|---------------|----------------------------------------------|
| ID.GroupID    | ID do Grupo do grupo.                        |
| ID.OSeq       | Número de sequência arbitrário. Incrementado internamente, começando em 1. |
| State         | Estado da ordem.                             |

**Estado**
| Nome   | Descrição                           |
|--------|-------------------------------------|
| OPEN   | Aceitando lances.                   |
| ACTIVE | Locação aberta foi criada.          |
| CLOSED | Sem locações ativas e não aceitando lances. Terminal. |

### Bid

| Nome         | Descrição                                    |
|--------------|----------------------------------------------|
| ID.OrderID   | ID da Ordem do grupo.                        |
| ID.Provider  | Endereço da conta do provedor.               |
| State        | Estado do lance.                             |
| EndsOn       | Altura em que o lance termina, se não for emparelhado. |
| Price        | Preço do lance - valor a ser pago a cada bloco. |

**Estado**
| Nome   | Descrição                           |
|--------|-------------------------------------|
| OPEN   | Aguardando emparelhamento.          |
| ACTIVE | Lance para uma locação ativa (vencedor). |
| CLOSED | Sem locações ativas para este lance. Terminal. |

### Lease

| Nome | Descrição                                    |
|------|----------------------------------------------|
| ID   | O mesmo que o ID do lance para a locação.    |
| State | Estado do lance.                            |

**Estado**
| Nome   | Descrição                           |
|--------|-------------------------------------|
| ACTIVE | Locação ativa - locatário paga provedor a cada bloco. |
| CLOSED | Nenhum pagamento sendo feito. Terminal.  |


