# Akash CLI

### Este guia irá orientá-lo na implantação de uma simples aplicação "Hello World" Next.JS na Rede Akash através do Akash CLI. Este guia é amigável para iniciantes e não requer conhecimento prévio de navegação no Akash CLI ou na Akash Network em geral.

### Tutorial para Implantar "Hello World" SDL usando o Akash CLI

[![Tutorial to Deploy "Hello World" SDL using the Akash CLI](https://img.youtube.com/vi/fYta01WFyVU/0.jpg)](https://www.youtube.com/watch?v=fYta01WFyVU)

### Antes de Começar

Você deve ter o Akash CLI instalado e devidamente configurado, junto com alguns AKTs na sua carteira.(0.5 AKT minimum for a single deployment plus a small amount for transaction fees).

### Siga o ***Guia de Instalação do CLI*** para instalar o Akash CLI, se necessário.

### Verifique seu Saldo de Conta

Verifique se sua conta tem saldo suficiente executando:

```bash
provider-services query bank balances --node $AKASH_NODE $AKASH_ACCOUNT_ADDRESS
```
### Antes de Começar

Você deve ter o Akash CLI instalado e devidamente configurado, junto com alguns AKTs na sua carteira.(0.5 AKT minimum for a single deployment plus a small amount for transaction fees).

Siga o **Guia de Instalação do CLI** para instalar o Akash CLI, se necessário.

### Verifique seu Saldo de Conta

Verifique se sua conta tem saldo suficiente executando:

```bash
provider-services query bank balances --node $AKASH_NODE $AKASH_ACCOUNT_ADDRESS
```

### Você deve ver uma resposta semelhante a:

```yaml
balances:
- amount: "93000637"
  denom: uakt
pagination:
  next_key: null
  total: "0"
```

Se você não tiver saldo, por favor consulte o **guia de financiamento**. Por favor, note que o saldo indicado está denominado em uAKT (AKT x 10^-6), no exemplo acima, a conta tem um saldo de 93 AKT. Estamos agora prontos para implantar.

Nota: Sua conta deve ter um saldo mínimo de 0,5 AKT para criar uma implantação. Este 0,5 AKT financia a conta escrow associada ao lançamento e é usado para pagar o provedor pelos seus serviços. É recomendável que você tenha mais do que este saldo mínimo para pagar as taxas de transação. Para mais informações sobre contas escrow, veja **aqui**

## PASSO 1: Crie sua Configuração

Crie uma configuração de implantação `deploy.yaml` para implantar o `akashlytics/hello-akash-world:0.2.0` para `hello-akash-world   `, que é um simples aplicativo "Hello World" em Next.JS.

Você pode usar o comando CURL para baixar o arquivo SDL:

```bash
curl -s https://raw.githubusercontent.com/maxmaxlabs/hello-akash-world/master/deploy.yml > deploy.yml
```

O arquivo **SDL** é o arquivo de configuração que especifica todos os recursos necessários, como **CPU**, **RAM**, armazenamento, etc. Como esta é uma aplicação simples de "Hello World", os recursos necessários serão muito poucos.


## PASSO 2: Crie sua Implantação

```json
{
  "height":"140325",
  "txhash":"2AF4A01B9C3DE12CC4094A95E9D0474875DFE24FD088BB443238AC06E36D98EA",
  "codespace":"",
  "code":0,
  "data":"0A130A116372656174652D6465706C6F796D656E74",
  "raw_log":"[{\"events\":[{\"type\":\"akash.v1\",\"attributes\":[{\"key\":\"module\",\"value\":\"deployment\"},{\"key\":\"action\",\"value\":\"deployment-created\"},{\"key\":\"version\",\"value\":\"2b86f778de8cc9df415490efa162c58e7a0c297fbac9cdb8d6c6600eda56f17e\"},{\"key\":\"owner\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"},{\"key\":\"dseq\",\"value\":\"140324\"},{\"key\":\"module\",\"value\":\"market\"},{\"key\":\"action\",\"value\":\"order-created\"},{\"key\":\"owner\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"},{\"key\":\"dseq\",\"value\":\"140324\"},{\"key\":\"gseq\",\"value\":\"1\"},{\"key\":\"oseq\",\"value\":\"1\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"create-deployment\"},{\"key\":\"sender\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"},{\"key\":\"sender\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8\"},{\"key\":\"sender\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"},{\"key\":\"amount\",\"value\":\"5000uakt\"},{\"key\":\"recipient\",\"value\":\"akash14pphss726thpwws3yc458hggufynm9x77l4l2u\"},{\"key\":\"sender\",\"value\":\"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj\"},{\"key\":\"amount\",\"value\":\"5000000uakt\"}]}]}]",
  "logs":[
    {
      "msg_index":0,
      "log":"",
      "events":[
        {
          "type":"akash.v1",
          "attributes":[
            {
              "key":"module",
              "value":"deployment"
            },
            {
              "key":"action",
              "value":"deployment-created"
            },
            {
              "key":"version",
              "value":"2b86f778de8cc9df415490efa162c58e7a0c297fbac9cdb8d6c6600eda56f17e"
            },
            {
              "key":"owner",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            },
            {
              "key":"dseq",
              "value":"140324"
            },
            {
              "key":"module",
              "value":"market"
            },
            {
              "key":"action",
              "value":"order-created"
            },
            {
              "key":"owner",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            },
            {
              "key":"dseq",
              "value":"140324"
            },
            {
              "key":"gseq",
              "value":"1"
            },
            {
              "key":"oseq",
              "value":"1"
            }
          ]
        },
        {
          "type":"message",
          "attributes":[
            {
              "key":"action",
              "value":"create-deployment"
            },
            {
              "key":"sender",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            },
            {
              "key":"sender",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            }
          ]
        },
        {
          "type":"transfer",
          "attributes":[
            {
              "key":"recipient",
              "value":"akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8"
            },
            {
              "key":"sender",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            },
            {
              "key":"amount",
              "value":"5000uakt"
            },
            {
              "key":"recipient",
              "value":"akash14pphss726thpwws3yc458hggufynm9x77l4l2u"
            },
            {
              "key":"sender",
              "value":"akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj"
            },
            {
              "key":"amount",
              "value":"5000000uakt"
            }
          ]
        }
      ]
    }
  ],
  "info":"",
  "gas_wanted":"100000",
  "gas_used":"94653",
  "tx":null,
  "timestamp":""
}
```

### Encontre sua Implantação

Encontre a Sequência de Implantação **(DSEQ)** na implantação que você acabou de criar. Por exemplo, na implantação atual, o valor de dseq é 140324, que você pode confirmar na resposta acima.

Em seguida, execute o seguinte comando, certifique-se de copiar e colar o valor DSEQ da sua implantação:

```bash
export AKASH_DSEQ=CHANGETHIS
```

Agora defina a Sequência de Pedido **(OSEQ)** e a Sequência de Grupo **(GSEQ)**. Observe que, se esta for a sua primeira vez implantando no Akash, **OSEQ** e **GSEQ** serão 1.

```bash
AKASH_OSEQ=1
AKASH_GSEQ=1
```

Verifique se os valores preenchidos estão corretos executando:

```bash
echo $AKASH_DSEQ $AKASH_OSEQ $AKASH_GSEQ
```

### PASSO 3: Veja suas Ofertas e escolha um fornecedor

Após um curto período, você deve ver propostas dos provedores para esta implantação com o seguinte comando:

```bash
provider-services query market bid list --owner=$AKASH_ACCOUNT_ADDRESS --node $AKASH_NODE --dseq $AKASH_DSEQ --state=open
```

### Escolha um Provedor

Observe que há propostas de vários fornecedores diferentes. Neste caso, ambos os provedores estão dispostos a aceitar um preço de 1 uAKT. Isso significa que o arrendamento pode ser criado usando 1 uAKT ou 0,000001 AKT por bloco para executar o contêiner. Você deve ver uma resposta semelhante a:

```yaml
bids:
- bid:
    bid_id:
      dseq: "140324"
      gseq: 1
      oseq: 1
      owner: akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj
      provider: akash10cl5rm0cqnpj45knzakpa4cnvn5amzwp4lhcal
    created_at: "140326"
    price:
      amount: "1"
      denom: uakt
    state: open
  escrow_account:
    balance:
      amount: "50000000"
      denom: uakt
    id:
      scope: bid
      xid: akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj/140324/1/1/akash10cl5rm0cqnpj45knzakpa4cnvn5amzwp4lhcal
    owner: akash10cl5rm0cqnpj45knzakpa4cnvn5amzwp4lhcal
    settled_at: "140326"
    state: open
    transferred:
      amount: "0"
      denom: uakt
- bid:
    bid_id:
      dseq: "140324"
      gseq: 1
      oseq: 1
      owner: akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj
      provider: akash1f6gmtjpx4r8qda9nxjwq26fp5mcjyqmaq5m6j7
    created_at: "140326"
    price:
      amount: "1"
      denom: uakt
    state: open
  escrow_account:
    balance:
      amount: "50000000"
      denom: uakt
    id:
      scope: bid
      xid: akash1vn06ycjjnvsvl639fet9lajjctuturrtx7fvuj/140324/1/1/akash1f6gmtjpx4r8qda9nxjwq26fp5mcjyqmaq5m6j7
    owner: akash1f6gmtjpx4r8qda9nxjwq26fp5mcjyqmaq5m6j7
    settled_at: "140326"
    state: open
    transferred:
      amount: "0"
      denom: uakt
```

Para este exemplo, escolheremos o provedor `akash10cl5rm0cqnpj45knzakpa4cnvn5amzwp4lhcal`. Execute o seguinte comando para definir a variável de shell do provedor:

```bash
AKASH_PROVIDER=akash10cl5rm0cqnpj45knzakpa4cnvn5amzwp4lhcal
```

Verifique se temos o valor correto preenchido executando:

```bash
echo $AKASH_PROVIDER
```

## PASSO 4: Criar e confirmar um Contrato de Locação

Crie um contrato de arrendamento para a proposta do fornecedor escolhido acima executando este comando:

```bash
provider-services tx market lease create --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER --from $AKASH_KEY_NAME
```

### Confirmar o Contrato de Locação
Você pode verificar o status do seu contrato de aluguel executando:

consulta de serviços de provedor mercado lista de arrendamentos --proprietário 

```bash
AKASH_ACCOUNT_ADDRESS --nó $AKASH_NODE --dseq $AKASH_DSEQ
```

Observe que os lances serão encerrados automaticamente após 5 minutos, e você pode receber a resposta:

```bash
bid not open
```

Se isso acontecer, feche sua implantação e abra uma nova implantação novamente. Para encerrar sua implantação, execute este comando:

````bash
provider-services tx deployment close --dseq $AKASH_DSEQ  --owner $AKASH_ACCOUNT_ADDRESS --from $AKASH_KEY_NAME
````

Se o seu contrato de arrendamento foi bem-sucedido, você deve ver uma resposta que termina com:

```bash
state: active
```

**Nota:** Por favor, note que uma vez que o contrato de arrendamento é criado, o provedor começará a debitar a conta de custódia da sua implantação, mesmo que você não tenha concluído o processo de implantação ao fazer o upload do manifesto na etapa seguinte.

### Enviar o Manifesto

Carregue o manifesto usando os valores da etapa acima:

```bash
provider-services send-manifest deploy.yml --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER --from $AKASH_KEY_NAME
```

### Confirmar URL

Agora que o manifesto está carregado, sua imagem está implantada. Você pode recuperar os detalhes de acesso executando o seguinte comando:

```bash
provider-services lease-status --dseq $AKASH_DSEQ --from $AKASH_KEY_NAME --provider $AKASH_PROVIDER
```

Você deve ver uma resposta semelhante a:

```json
{
  "services": {
    "web": {
      "name": "web",
      "available": 1,
      "total": 1,
      "uris": [
        "rga3h05jetf9h3p6dbk62m19ck.ingress.ewr1p0.mainnet.akashian.io"
      ],
      "observed_generation": 1,
      "replicas": 1,
      "updated_replicas": 1,
      "ready_replicas": 1,
      "available_replicas": 1
    }
  },
  "forwarded_ports": {}
}
```

Você pode acessar o aplicativo visitando os nomes de host mapeados para sua implantação. Procure um URL/URI e copie-o para o seu navegador.

Então você deve ser capaz de ver a página de aterrissagem do "Hello World" Application.

![Descrição da imagem](https://akash.network/_astro/12.Cn6u_zNB_Z2au6Kl.webp)

### Ver seus registros

Você pode visualizar os logs da sua aplicação para depurar problemas ou acompanhar o progresso assim:

````
provider-services lease-logs \
  --dseq "$AKASH_DSEQ" \
  --provider "$AKASH_PROVIDER" \
  --from "$AKASH_KEY_NAME"
```` 

## PASSO 5: Fechar Implantação

### Fechar a Implantação

Agora, como aprendemos a implantar a aplicação 'Hello World', vamos encerrar a implantação para não desperdiçar nenhum AKT. Após o fechamento do deployment, receberemos o saldo restante como reembolso.

Execute o seguinte comando para encerrar a implantação:

```bash
provider-services tx deployment close --from $AKASH_KEY_NAME
```

### Conclusão

Implantamos uma simples aplicação "Hello World" Next.JS em 5 minutos. O processo de implantar qualquer outra aplicação é idêntico, apenas o arquivo sdl precisa ser alterado de acordo com a configuração adequada e a imagem do contêiner.


