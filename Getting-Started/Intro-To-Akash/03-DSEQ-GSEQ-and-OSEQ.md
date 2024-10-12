
# DSEQ, GSEQ e OSEQ

## GSEQ - Sequência de Grupo

- O **GSEQ** distingue "grupos" de contêineres em uma implantação, permitindo que cada grupo seja alugado de forma independente. Pedidos, ofertas e arrendamentos atuam todos em um único grupo.
- Normalmente, as implantações do Akash usam **GSEQ = 1**, com todos os pods associados à implantação utilizando um único provedor.
- O exemplo abaixo de **SDL (Service Definition Language)** mostra uma configuração com várias seções de colocação (westcoast e eastcoast), solicitando propostas de múltiplos fornecedores:

```yaml
placement:  
  westcoast:    
    attributes:      
      region: us-west    
    pricing:      
      grafana-profile:        
        denom: uakt        
        amount: 10000  
  eastcoast:    
    attributes:      
      region: us-east    
    pricing:      
      postgres-profile:        
        denom: uakt        
        amount: 1000
```

- Neste exemplo:
  - **westcoast** possui o atributo `region: us-west`.
  - **eastcoast** possui o atributo `region: us-east`.

- Quando uma implantação usa múltiplas seções de colocação, o **GSEQ** define ordens únicas para cada grupo.  
  Abaixo, temos um exemplo onde duas ordens são geradas com valores distintos de **GSEQ**.

```json
{"order-created"},{"key":"owner","value":"akash1ggk74pf9avxh3llu30yfhmr345h2yrpf7c2cdu"},{"key":"dseq","value":"9507298"},{"key":"gseq","value":"1"},{"key":"oseq","value":"1"}

{"order-created"},{"key":"owner","value":"akash1ggk74pf9avxh3llu30yfhmr345h2yrpf7c2cdu"},{"key":"dseq","value":"9507298"},{"key":"gseq","value":"2"},{"key":"oseq","value":"1"}
```

---

## DSEQ - Sequência de Implantação

O **DSEQ** é essencial no processo de implantação e gerenciamento na Rede Akash.

### Importância do DSEQ:

- **Identificador Único**: Cada implantação recebe um DSEQ único, permitindo rastreamento preciso.
- **Número de Sequência de Ordem (OSEQ)**: Relaciona-se com a ordem de criação e gestão das implantações.
- **Gestão de Implantação**: Garante organização e clareza no gerenciamento de implantações.
- **Criação de Arrendamento**: O DSEQ é usado ao estabelecer contratos com provedores.
- **Monitoramento**: Permite acompanhar o status e endpoints das implantações.

---

## OSEQ - Sequência de Pedidos

- O **OSEQ** distingue múltiplos pedidos relacionados a uma mesma implantação.
- Normalmente, **OSEQ = 1** é usado, com apenas um pedido vinculado à implantação.
- O OSEQ é incrementado quando:
  - Um contrato é encerrado sem fechar a implantação.
  - Uma nova ordem é gerada mantendo a implantação aberta.

### Exemplo de Uso:

1. Criação de uma implantação:
   ```bash
   provider-services tx deployment create deploy.yml --from $AKASH_KEY_NAME
   ```

2. Após a criação, recebemos os seguintes valores:
   ```json
   {"order-created"},{"key":"owner","value":"akash1ggk74pf9avxh3llu30yfhmr345h2yrpf7c2cdu"},{"key":"dseq","value":"9507524"},{"key":"gseq","value":"1"},{"key":"oseq","value":"1"}]
   ```

3. Para mover a implantação para outro provedor, encerramos o contrato atual:
   ```bash
   provider-services tx market lease close --node $AKASH_NODE --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER --from $AKASH_KEY_NAME
   ```

4. Um novo pedido é gerado, incrementando o OSEQ:
   ```yaml
   bid:
     bid_id:
       dseq: "9507524"
       gseq: 1
       oseq: 2
       owner: akash1ggk74pf9avxh3llu30yfhmr345h2yrpf7c2cdu
       provider: akash1lmaulqyvlj0wwcjm5dgqn5wv5j957g672g20ht
     created_at: "9507559"
   ```

5. Para listar todos os lances:
   ```bash
   provider-services query market bid list --owner=$AKASH_ACCOUNT_ADDRESS --node $AKASH_NODE --dseq $AKASH_DSEQ --gseq 0 --oseq 0
   ```

---

## Provedores

Os **provedores Akash** oferecem recursos de computação para a rede descentralizada. Podem ser indivíduos ou organizações que contribuem com recursos como data centers ou servidores pessoais.

### Componentes Principais:

1. **Software de Nó**:  
   - Permite que provedores se conectem à rede e ofereçam seus recursos.
   - Inclui definição de preços, monitoramento e gerenciamento dos recursos.

2. **Staking**:  
   - Provedores fazem staking de tokens **AKT** como colateral, incentivando serviços confiáveis.

3. **Leilão Reverso**:  
   - A Akash Network utiliza um leilão reverso para automatizar a negociação de recursos.

---

## Resumo

Os provedores Akash são fundamentais para a operação da rede, oferecendo uma infraestrutura descentralizada e acessível. Por meio de componentes como DSEQ, GSEQ e OSEQ, a rede Akash garante um gerenciamento eficiente e seguro de implantações e recursos computacionais.
