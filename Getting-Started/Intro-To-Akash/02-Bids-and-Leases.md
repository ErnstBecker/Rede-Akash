# Lances e Arrendamentos  

## Como funciona o Marketplace?  

O **Akash Marketplace** é estruturado em torno das **Implantações**, que descrevem todos os recursos que um inquilino solicita da rede. Cada implantação é composta por **Grupos**, que são conjuntos de recursos alugados de um único provedor.  

## Tipos de Usuários no Marketplace  

1. **Inquilino**: A entidade que realiza a implantação da aplicação.  
2. **Provedor**: A entidade que hospeda a aplicação para o inquilino.  

## O que é um Leilão Reverso?  

A Akash utiliza um **leilão reverso**, no qual os inquilinos definem os preços e termos de suas implantações, e os provedores competem entre si, fazendo lances para oferecer seus serviços.  

### Etapas Básicas de um Leilão Reverso  

1. O inquilino cria pedidos no marketplace.  
2. Os provedores enviam lances para atender aos pedidos.  
3. O inquilino seleciona a proposta vencedora e cria um **contrato de arrendamento**.  

## Ciclo de Vida de uma Implantação  

1. O inquilino cria um arquivo `.yml` utilizando **SDL (Service Definition Language)** para descrever os recursos necessários.  
2. A definição é enviada à **blockchain**, gerando um pedido no marketplace.  
3. Provedores interessados fazem lances para atender o pedido.  
4. Após um período, uma proposta vencedora é escolhida e um **contrato de arrendamento** é criado.  
5. O inquilino envia um **manifesto** ao provedor com instruções para a execução da aplicação.  
6. O provedor executa a aplicação conforme as instruções recebidas.  
7. A aplicação entra em operação (se for uma aplicação web, pode ser acessada via navegador).  
8. O inquilino ou provedor encerra o contrato de arrendamento, finalizando a execução.  

---

## Pagamentos  

Os pagamentos fluem do **inquilino** para o **provedor** por meio de um mecanismo de **depósito e retirada**.  

- **Depósito Inicial**: O inquilino deve depositar fundos ao criar uma implantação.  
- **Pagamentos Automáticos**: Os pagamentos são deduzidos automaticamente do saldo do depósito.  
- **Manutenção do Saldo**: O inquilino deve garantir saldo suficiente para evitar a interrupção do serviço.  
- **Reembolso**: Ao encerrar a implantação, o saldo não utilizado é devolvido ao inquilino.  

---

## Contas Escrow  

As **contas escrow** garantem a transferência segura de fundos entre as partes, eliminando a necessidade de micropagamentos bloco a bloco.  

### Finalidades das Contas Escrow  

1. **Pagamentos em Blocos**: Cada bloco gera um pagamento do inquilino para o provedor, evitando transferências individuais para cada bloco.  
2. **Depósitos de Lances**: Ao enviar um lance, os provedores precisam fazer um depósito. O depósito é devolvido quando o lance é encerrado.  

---

## Depósitos de Licitação  

Os provedores devem realizar um depósito ao enviar um lance.  
- **Devolução**: O depósito é devolvido quando o lance entra no estado **CLOSED**.  
- **Módulo Escrow**: A funcionalidade é implementada por meio de um **módulo de conta escrow**.  

---

## Atributos Auditados  

Os **atributos auditados** permitem que os inquilinos escolham provedores com critérios específicos.  
- Qualquer usuário da rede pode atribuir atributos aos provedores por meio de transações on-chain.  

---

## Parâmetros On-Chain  

| Nome                     | Valor Inicial | Descrição                                                   |
| ------------------------ | ------------- | ----------------------------------------------------------- |
| `deployment_min_deposit` | 0.5 AKT       | Depósito mínimo para implantações. Aproximadamente US$2,20. |
| `bid_min_deposit`        | 0.5 AKT       | Depósito mínimo para lances. Aproximadamente US$2,20.       |

---

## Transações  

### **DeploymentCreate**  
Cria uma implantação, abrindo grupos e ordens.  

| Parâmetro       | Descrição                                      |
| --------------- | ---------------------------------------------- |
| `DeploymentID`  | ID da Implantação.                             |
| `DepositAmount` | Depósito inicial (≥ `deployment_min_deposit`). |
| `Version`       | Hash do manifesto enviado aos provedores.      |
| `Groups`        | Lista de descrições de grupo.                  |

### **DeploymentDeposit**  
Adiciona fundos ao saldo de uma implantação.  

| Parâmetro       | Descrição                       |
| --------------- | ------------------------------- |
| `DeploymentID`  | ID da Implantação.              |
| `DepositAmount` | Quantia adicionada ao depósito. |

### **GroupClose**  
Fecha um grupo e suas ordens associadas.  

| Parâmetro | Descrição    |
| --------- | ------------ |
| `ID`      | ID do Grupo. |

### **BidCreate**  
Enviado por um provedor para fazer um lance.  

| Parâmetro | Descrição                                        |
| --------- | ------------------------------------------------ |
| `OrderID` | ID da Ordem.                                     |
| `TTL`     | Número de blocos para os quais o lance é válido. |
| `Deposit` | Depósito do lance (≥ `bid_min_deposit`).         |

### **BidClose**  
Fecha um lance ou contrato de locação.  

| Parâmetro | Descrição    |
| --------- | ------------ |
| `BidID`   | ID do Lance. |

---

## Transições de Estado  

| Objeto | Estado Anterior | Novo Estado |
| ------ | --------------- | ----------- |
| Bid    | ACTIVE          | CLOSED      |
| Lease  | ACTIVE          | CLOSED      |
| Order  | ACTIVE          | CLOSED      |
| Group  | OPEN            | PAUSED      |

---

## Modelos  

### **Deployment**  

| Campo      | Descrição                                        |
| ---------- | ------------------------------------------------ |
| `ID.Owner` | Endereço da conta do inquilino.                  |
| `ID.DSeq`  | Número sequencial (por padrão, altura do bloco). |
| `State`    | Estado da implantação.                           |
| `Version`  | Hash do manifesto enviado aos provedores.        |

**Estados**  
- **OPEN**: Ordens podem ser criadas.  
- **CLOSED**: Implantação encerrada.  

### **Order**  

| Campo        | Descrição        |
| ------------ | ---------------- |
| `ID.GroupID` | ID do Grupo.     |
| `State`      | Estado da ordem. |

**Estados**  
- **OPEN**: Aceitando lances.  
- **ACTIVE**: Locação ativa criada.  
- **CLOSED**: Ordem encerrada.  
