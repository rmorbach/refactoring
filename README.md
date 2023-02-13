- [Técnicas de refatoração](#técnicas-de-refatoração)
- [Princípios de refatoração](#princípios-de-refatoração)
- [Por que refatorar?](#por-que-refatorar)
- [Quando refatorar](#quando-refatorar)
- [Quando não refatorar](#quando-não-refatorar)
- [Métodos para auxiliar na refatoração](#métodos-para-auxiliar-na-refatoração)
  - [Refatorações básicas](#refatorações-básicas)
    - [Extrair função](#extrair-função)
    - [Internalizar função](#internalizar-função)
    - [Extrair variável](#extrair-variável)
    - [Renomear variáveis](#renomear-variáveis)
    - [Introduzir objeto de parâmetros](#introduzir-objeto-de-parâmetros)
  - [Encapsulamento](#encapsulamento)
    - [Encapsular coleções](#encapsular-coleções)
    - [Substitur variável temporária por consulta](#substitur-variável-temporária-por-consulta)
    - [Ocultar delegação](#ocultar-delegação)
  - [Movendo recursos](#movendo-recursos)
    - [Dividir laço](#dividir-laço)
    - [Substituir laço por pipeline](#substituir-laço-por-pipeline)
    - [Remover código morto](#remover-código-morto)
  - [Organizando dados](#organizando-dados)
    - [Mudar referência para valor](#mudar-referência-para-valor)
  - [Simplificando lógicas condicionais](#simplificando-lógicas-condicionais)
    - [Substituir cláusulas aninhadas por instruções de guarda](#substituir-cláusulas-aninhadas-por-instruções-de-guarda)
    - [Substituir condicional por polimorfismo](#substituir-condicional-por-polimorfismo)

# Técnicas de refatoração

Este é um repositório de estudo para documentar algumas das técnicas de refatoração de projetos de software descritos por Martin Fowler e Kent Beck no livro [Refatoração: Aperfeiçoando o Design de Códigos Existentes](https://www.amazon.com.br/Refatora%C3%A7%C3%A3o-Aperfei%C3%A7oando-Design-C%C3%B3digos-Existentes/dp/8575227246).

# Princípios de refatoração

É importante destacar que a refatoração não é bala de prata. Ela pode sim, juntamente com um bom design arquitetural e todos os princípios que governam o [SOLID](https://blog.cleancoder.com/uncle-bob/2020/10/18/Solid-Relevance.html) conduzir a um código limpo que seja fácil de manter e evoluir.

Se meu aplicativo está funcionando, por quê refatorar? Aqui cabe aquela velha máxima que muitos desenvolvedores já ouviram, mas poucos praticam. "Não escrevemos código para máquina, escrevemos código para outros desenvolvedores". Por mais clichê que possa parecer, no contexto atual é mais verdade do que nunca. Com o mercado de tecnologia muito aquecido, a rotatividade de profissionais é muito alta. Um código bem estruturado e fácil de entender diminui a curva de aprendizado de novos membros do time e o tempo para que este novo profissional possa produzir na mesma velocidade que os demais integrantes.

A refatoração não irá melhorar a performance da sua aplicação. Este não é o objetivo. A refatoração pode, inclusive, diminuir a performance da sua aplicação até certo ponto. A questão é que é muito mais fácil tratar questões de performance em um código bem estruturado do que em um código ininteligível.

Outro ponto importante destacado pelos autores é que o processo de refatoração deve manter o código no mesmo estado que estava antes de iniciar o processo. Para isso é fundamental a existência de testes unitários que deixem o desenvolvedor confiante de que a sua refatoração não irá introduzir novos bugs. Isso é o que os autores chamam de **comportamento observável**. 

Por isso, a refatoração deve ser composta de pequenos passos individuais, onde cada alteração é checada por testes de unidade.

Uma sugestão dos autores é tratar a refatoração de modo independente das demais atividades de desenvolvimento: novas funcionalidades, resolução de bugs, melhorias de performance, etc. O desenvolvedor pode até acabar resolvendo potenciais bugs ainda não identificados durante o processo de refatoração, mas esse não é o objetivo.

# Por que refatorar?

Olhando da perspectiva de um desenvolvedor essa pergunta parece ter uma resposta óbvia. No mundo real não cabe somente ao desenvolvedor decidir se o código deve ser refatorado e quando a refatoração deve acontecer. Ele precisa argumentar e defender a refatoração sob uma perspectiva de melhoria do produto e do projeto para poder convencer todas as partes interessadas.

Os autores destacam alguns pontos onde a refatoração gera resultados mensuráveis.

* **Refatoração para melhoria de design**: esse tipo de refatoração é importante para manter o código e a arquitetura do projeto sempre atualizados com as novas tecnologias e evoluções de plataforma. Sem esse tipo de refatoração o software "tende a entrar em decadência".

* **Refatoração para melhorar a legibilidade**: importante para deixar o código mais fácil de entender para todos os desenvolvedores que eventualmente precisarão atuar no código.

* **Refatoração para aumento de produtividade**: os dois pontos anteriores devem resultar neste terceiro, que é o que de fato interessa para todos os envolvidos. Quando o software é fácil de entender e está bem estruturado, sua manutenabilidade é baixa e sua evolução simples.

# Quando refatorar

A refatoração não é bala de prata e por isso é preciso ponderar sobre o melhor momento e método a ser utilizado para empregá-la.

Os autores apresentam diversas oportunidades e abordagens para aplicar a refatoração em código, destaco as seguintes:

* **Refatoração preparatória**: momento antes de iniciar o desenvolvimento de uma nova funcionalidade. O objetivo é "preparar o terreno" para não encontrar surpresas em tempo de cruzeiro. Neste ponto já existe conhecimento prévio sobre o código que será a base para a nova funcionalidade e que ele precisa ser modificado para receber novas evoluções.

* **Refatoração para compreensão**: aplicação da justificativa "para melhorar a legibilidade". Esse tipo de refatoração pode ser feita durante a resolução de um bug ou desenvolvimento de uma funcionalidade. Em geral não é preciso reservar um tempo exclusivo para realizar esse tipo de refatoração.

* **Refatoração para coleta de lixo**: variação da refatoração acima, no entanto, aplicado quando o desenvolvedor encontrar uma código com potencial de gerar bug, seja por fluxos não cobertos ou por lógicas incompletas.

* **Refatorações planejadas e oportunistas**: as refatorações mencionadas anteriormente são todas oportunistas, isto é, podem ser realizadas durante o desenvolvimento de uma nova funcionalidade ou resolução de um bug. Refatorações planejadas, por outro lado, exigem um tempo dedicado para atuação e normalmente são feitas de modo exclusivo. Esses tipos de refatorações devem ser muito bem analisadas e alinhadas com todos os envolvidos do projeto, a fim de definir expectativas do que será entregue após a sua realização (quais serão os ganhos reais para o projeto/produto).

# Quando não refatorar

Nem todo código "feio" precisa ser refatorado. Se o código existe há algum tempo e não precisa ser modificado, pois não prevê evolução, não é necessário refatorar. "A refatoração me trará alguma vantagem somente se eu precisa entender como o código funciona".

Outro caso onde não é aconselhado a refatoração é quando é mais fácil reescrever o código. Um exemplo prático é uma mudança de linguagem de programação dentro de um projeto. O mundo mobile tem proporcionado essa oportunidade com os não recentes suportes às linguages Kotlin para Android e Swift para os dispositivos da Apple. Muito além de simplesmente reescrever a mesma estrutura só que em uma linguagem diferente, essa é uma oportunidade de analisar o que pode ser melhorado e também explorar o potencial da nova linguagem empregada.

# Métodos para auxiliar na refatoração

Abaixo sumarizei, com exemplos do livro, os principais métodos de refatoração elencados pelos autores. Fica com *disclaimer* que mesmo os códigos refatorados podem possuir problemas. O objetivo não é ter um código perfeito ao final de cada refatoração mas sim,colocar em termos práticos os métodos de refatoração.

São mais de 60 métodos de refatoração com exemplos no livro, com antes e depois, e também com passos demonstrando como fazer cada refatoração. Não faria sentido replicar o que está no livro, para isso recomendo a leitura. Selecionei alguns que julgo serem relevantes e corriqueiros no dia-a-dia do desenvolvedor.

Outro ponto é que o autor divide as refatorações em tópicos, por exemplo: Primeiras refatorações; Encapsulamento; Refatorando API; Lidando com herança; Movendo recursos; Organizando dados e Simplificando lógicas condicionais.
Vale destacar também que muitos métodos de refatoração possuem etapas que utilizam outros métodos.

## Refatorações básicas

### Extrair função

Objetivo: extrair trecho de código em uma função ou método que seja coeso e autocontido.

Código a ser refatorado:

```swift 
public func printOwing(invoice: Invoice) {

    print("********************************")
    print("******* Customer Owns **********")
    print("********************************")

    var outstanding: Double = .zero
    for order in invoice.orders {
        outstanding += order.amount
    }
        
    print("name: \(invoice.customer)")
    print("amount: \(outstanding)")
}
```

Após a refatoração:

```swift 
public func printOwing(invoice: Invoice) {
    
    func calculateOutstanding() -> Double {
        return invoice.orders.reduce(0) { result, order -> Double in
            return result + order.amount
        }
    }
    
    func printDetails(outstanding: Double) {
        print("name: \(invoice.customer)")
        print("amount: \(outstanding)")
    }

    func printBanner() {
        print("********************************")
        print("******* Customer Owes **********")
        print("********************************")
    }

    let outstanding = calculateOutstanding()
    printBanner()
    printDetails(outstanding: outstanding)
}

```

### Internalizar função

Objetivo: remover acessos indiretos desnecessários.

Em geral é útil quando há muitos métodos que simplesmente chamam outros métodos. Essa "linkagem" de métodos ou funções só aumenta a carga cognitiva do desenvolvedor e dificulta o entendimento geral do programa.

Antes:

```swift
func getRating(for driver: Driver) -> Int {
    return moreThanFiveLateDeliveries(driver) ? 2 : 1
}

func moreThanFiveLateDeliveries(_ driver: Driver) -> Bool {
    return driver.numberOfLateDeliveries > 5
}
```

Depois:

```swift
func getRating(for driver: Driver) -> Int {
    return (driver.numberOfLateDeliveries > 5) ? 2 : 1
}
```

### Extrair variável

Objetivo: diminuir complexidade e melhorar legibilidade de expressões.

Sistemas podem demandar cálculos que compreendam vários passos e que possuem fórmulas padrões. Definir operandos como variáveis ajuda a entender os cálculos realizados e auxilia no processo de depuração e debug.

Antes: exemplo de cálculo usando uma entidade order.

```swift
func calculateFinalPrice(for order: Order) -> Double {
    return order.quantity * order.itemPrice - max(0, order.quantity - 500) * order.itemPrice * 0.06 + min(order.quantity * order.itemPrice * 0.1, 100)
}
```

Depois:

```swift
func calculateFinalPrice(for order: Order) -> Double {
    let basePrice = order.quantity * order.itemPrice
    let quantityDiscount = max(0, order.quantity - 500) * order.itemPrice * 0.06
    let shipping = min(order.quantity * order.itemPrice * 0.1, 100)
    let result = basePrice - quantityDiscount + shipping
    return result
}
```

### Renomear variáveis

Objetivo: facilitar a identificação de elementos envolvidos no código.

Definir bons nomes para variáveis é um desafio diário para o desenvolvedor. Este é também um dos principais pilares do Clean Code.
A maioria dos analisadores estáticos de código previnem o desenvolvedor de nomear variáveis com *X* e *bla*, mas isso não quer dizer que os nomes utilizados sejam bons para o contexto em que estão sendo utilizados.

Antes:

```swift
let usrNmStr = getUsername()
```

Depois:

```swift
let username: String = getUsername()
```


### Introduzir objeto de parâmetros

Objetivo: evitar repetição de parâmetros em diferentes métodos e diminuir o número de argumentos.

É comum que métodos ganhem novos argumentos à medida que novas features vão sendo desenvolvidas. Boas práticas sugerem um número pequeno de argumentos, no máximo 5.

Para resolver o problema de métodos com argumentos repetidos ou com muitos argumentos é útil criar estruturas de dados efêmeras, que fazem sentido somente para encapsular propriedades relacionadas, um **objeto de parâmetros**.

Antes:

```swift
func getOrdersSent(startDate: Date, endDate: Date) -> [Order]
func getOrdersCancelled(startDate: Date, endDate: Date) -> [Order]
func getOrdersPending(startDate: Date, endDate: Date) -> [Order]
```

Depois:

```swift
enum OrderStatus { case pending, sent, cancelled }

struct OrderRequest { 
    let status: OrderStatus
    let startDate: Date
    let endDate: Date
}

func getOrders(request: OrderRequest) -> [Order]

```

## Encapsulamento

### Encapsular coleções

### Substitur variável temporária por consulta

### Ocultar delegação

## Movendo recursos

### Dividir laço

### Substituir laço por pipeline

### Remover código morto

## Organizando dados

### Mudar referência para valor

## Simplificando lógicas condicionais

### Substituir cláusulas aninhadas por instruções de guarda

### Substituir condicional por polimorfismo


