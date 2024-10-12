# Pagamentos

Os aluguéis são pagos do proprietário da implantação (inquilino) ao provedor através de um mecanismo de depósito e retirada.

## Depósitos de Licitação

Os inquilinos são obrigados a apresentar um depósito ao criar uma implantação. Os aluguéis serão pagos passivamente a partir do saldo deste depósito. A qualquer momento, um fornecedor de leasing pode retirar o saldo devido a eles deste depósito.

Se os fundos disponíveis no depósito chegarem a zero, o prestador pode encerrar o contrato de locação. Um inquilino pode adicionar fundos ao seu depósito a qualquer momento. Quando um desdobramento é encerrado, a parte não gasta do saldo será devolvida ao inquilino.

Fazer uma oferta em um pedido requer que um depósito seja feito. O depósito será devolvido à conta do fornecedor quando a oferta passar para o estado FECHADO.

Os depósitos de licitação são implementados com um módulo de conta escrow. Veja aqui para mais informações.

## Contas Escrow

As contas escrow são um mecanismo que permite pagamentos baseados no tempo de uma conta para outra sem micropagamentos bloco a bloco. Eles também suportam manter fundos em uma conta até que um evento arbitrário ocorra.

### Contas de escrow são necessárias em Akash por duas razões principais:

1. Os arrendamentos em Akash são precificados em blocos - a cada novo bloco, um pagamento

    do inquilino (proprietário da implantação) para o provedor (lease holder)

    é devido. Considerações de desempenho e segurança proíbem o

    abordagem ingênua de transferir tokens em cada bloco.

2. Licitar em um pedido não deve ser gratuito (for various reasons, including performance and security). Akash exige um depósito para cada lance. O depósito é devolvido ao licitante quando a licitação é encerrada.

As contas escrow são criadas com um ID arbitrário, um proprietário e um saldo. O saldo é imediatamente transferido da conta do proprietário para a conta do módulo de custódia. As contas podem ter seu saldo aumentado ao serem depositadas após a criação.

Os pagamentos representam transferências da conta escrow para outra conta. Eles são (atualmente) baseados em blocos - uma certa quantia deve ser transferida para cada bloco. O valor devido ao pagamento da conta escrow é subtraído do saldo da conta escrow através de um processo de liquidação.

Os pagamentos podem ser retirados, transferindo qualquer saldo não desembolsado da conta do módulo para a conta do proprietário do pagamento.

Quando uma conta ou um pagamento é encerrado, qualquer saldo remanescente será transferido para as respectivas contas dos proprietários.

Se em qualquer momento o valor devido aos pagamentos for maior do que o saldo restante da conta, a conta e todos os pagamentos são encerrados com o estado DEVEDOR.

Muitas ações invocam o processo de liquidação e podem fazer com que a conta fique negativa.

Como são calculadas as taxas de gás no Akash?

Akash usa os cálculos básicos de gás do Cosmos para todas as taxas. A documentação do Cosmos sobre gás pode ser encontrada aqui.

## Liquidação de Conta

A liquidação de contas é o processo de atualizar a contabilidade interna dos saldos de pagamentos de uma conta para a altura atual.

Muitas ações acionam o processo de liquidação da conta - isso garante um livro-razão atualizado ao agir na conta.

### A liquidação da conta segue da seguinte forma:

1. Determine blockRate - o valor devido por cada bloco.
2. Determine heightDelta - o número de blocos desde o último assentamento.
3. Determine numFullBlocks - o número de blocos que podem ser pagos integralmente.
4. Transferir o valor de numFullBlocks para pagamentos.
5. Se numFullBlocks for menor que heightDelta (conta estourada), então:
   
   1. Distribua o saldo restante entre os pagamentos, ponderado pela taxa de cada pagamento
   2. Distribua qualquer saldo restante do acima o mais uniformemente possível
   3. Defina a conta e os pagamentos como ESTADO DE SOBREGIRO.

## Modelos

### Conta

| Campo           | Descrição                                             |
| --------------- | ----------------------------------------------------- |
| **ID**          | ID único da conta.                                    |
| **Owner**       | Endereço da conta do proprietário.                    |
| **State**       | Estado da conta.                                      |
| **Balance**     | Valor depositado da conta do proprietário.            |
| **Transferred** | Montante desembolsado da conta através de pagamentos. |
| **SettledAt**   | Último bloco em que os pagamentos foram liquidadas.   |



## **Estado da Conta**
| Name      |
| --------- |
| OPEN      |
| CLOSED    |
| OVERDRAWN |

---

## **Pagamento**
| Campo         | Descrição                                       |
| ------------- | ----------------------------------------------- |
| **AccountID** | ID da Conta Escrow.                             |
| **PaymentID** | ID único (sobre AccountID) do pagamento.        |
| **Owner**     | Endereço da conta do proprietário.              |
| **State**     | Estado do pagamento.                            |
| **Rate**      | Tokens por bloco para transferir.               |
| **Balance**   | Saldo atualmente reservado para o proprietário. |
| **Withdrawn** | Valor já retirado pelo proprietário.            |

---

## **Estado de Pagamento**
| Name      |
| --------- |
| OPEN      |
| CLOSED    |
| OVERDRAWN |

---

## **Métodos**

### **Criar Conta**  
Crie uma conta de escrow. Os fundos são depositados da conta do proprietário para a conta do módulo de custódia.

**Argumentos**:  
| Campo       | Descrição                                  |
| ----------- | ------------------------------------------ |
| **ID**      | ID único da conta.                         |
| **Owner**   | Endereço da conta do proprietário.         |
| **Deposit** | Valor depositado da conta do proprietário. |

---

### **Depósito de Conta**  
Adicionar fundos a uma conta de garantia. Os fundos são transferidos da conta do proprietário para a conta do módulo de custódia.

**Argumentos**:  
| Campo      | Descrição                                  |
| ---------- | ------------------------------------------ |
| **ID**     | ID único da conta.                         |
| **Amount** | Valor depositado da conta do proprietário. |

---

### **Liquidar Conta**  
Recalcule os saldos restantes da conta e dos pagamentos.

**Argumentos**:  
| Campo  | Descrição          |
| ------ | ------------------ |
| **ID** | ID único da conta. |

---

### **Fechar Conta**  
Fechar conta - liquidar e encerrar pagamentos, devolver o saldo restante da conta para a conta do proprietário.

**Argumentos**:  
| Campos | Descrição          |
| ------ | ------------------ |
| **ID** | ID único da conta. |

---

### **Criar Pagamento**  
Criar um novo pagamento. A conta será liquidada primeiro; este método falhará se a conta não puder ser liquidada.

**Argumentos**:  
| Campo         | Descrição                                |
| ------------- | ---------------------------------------- |
| **AccountID** | ID da Conta Escrow.                      |
| **PaymentID** | ID único (sobre AccountID) do pagamento. |
| **Owner**     | Endereço da conta do proprietário.       |
| **Rate**      | Tokens por bloco para transferir.        |

---

## **Invariantes**
- A conta está em estado **OPEN** depois de ser resolvido.
- **ID** é único.
- **Owner** existe.
- **Rate** é diferente de zero, e a conta tem fundos para um bloco.

---

### **Pagamento Retirada**  
Retirar fundos de um saldo de pagamento. A conta será liquidada primeiro.

**Argumentos**:  
| Campo         | Descrição                                |
| ------------- | ---------------------------------------- |
| **AccountID** | ID da Conta Escrow.                      |
| **PaymentID** | ID único (sobre AccountID) do pagamento. |

---

### **Fechamento de Pagamento**  


**Arguments**:  
| Campo         | Descrição                                             |
| ------------- | ----------------------------------------------------- |
| **AccountID** | ID da Conta Escrow.                                   |
| **PaymentID** | Fechar um pagamento. A conta será liquidada primeiro. |

# Gancho

Hooks são callbacks que são registrados pelos usuários do módulo de escrow e que devem ser chamados em eventos específicos.

### Conta Fechada

Sempre que uma conta é encerrada **OnAccountClosed(Conta)** será chamado.

### Pagamento Fechado

Sempre que um pagamento é encerrado, **OnAccountClosed(Conta)** será chamado.
