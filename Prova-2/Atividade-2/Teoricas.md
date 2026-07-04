## Exercício 45

### Explique por que os documentos da coleção conteudos podem ter campos diferentes.

> ---
>
> - Como os documentos podem ser de diferentes tipode de obra (séries, filmes, etc) pode implicar em atributos diferentes, como o número de temporadas por exemplo.
>
> ---

## Exercício 50

### Diferença entre manter as informações separadas em várias coleções e armazenar tudo em um único documento

> ---
>
> R: A principal diferença está na forma como o banco acessa e gerencia os dados:
>
> 1. **Várias coleções**: Os dados são divididos e conectados por IDs (como uma chave estrangeira em SQL). Isso evita duplicação de dados, mas exige que o banco faça múltiplas buscas ou cruzamentos para juntar todas as informações.
> 2. **Único documento**: Todas as informações relacionadas ficam dentro de um único "super documento". A leitura é extremamente rápida porque o banco só precisa fazer uma única busca para trazer tudo o que a aplicação precisa.
>
> ---

## Exercício 51

### Uma vantagem e uma desvantagem de usar documentos aninhados no MongoDB

> ---
>
> R: O MongoDB, apesar de ser muito eficiente em alguns casos, pode se tornar incômodo em outros, seguem algumas vantagens e desvantagens:
>
> 1. **Vantagens**: é notável a maleabilidade dos dados de bancos não relacionais, como o MongoDB. Além disso, por representar os dados diretamente como os objetos são estruturados o "mongo" facilita o trabalho do programa na hora de tratar os dados (escrita e leitura), por exemplo em contextos web, as informações são passadas por _APIs_ via _json_ (referências de PSS), e no mongo os dados já são tratados assim.
> 2. **Desvantagens**: ao mesmo tempo que deixa mais fácil de ler e escrever os dados, o mongo acaba por ter uma repetição dos mesmos, e também a falta dos conceitos do ACID deixa-os menos confiáveis em certas situações, sendo um _trade-off_ entre desempenho e segurança.
>
> ---

## Exercício 52

### Situações onde seria melhor usar referência entre coleções

> ---
>
> R: Você deve usar referências (coleções separadas) quando:
>
> 1. **Relacionamentos complexos**: por exemplo, usuários e filmes. Um usuário assiste a vários filmes, e um filme é assistido por vários usuários.
> 2. **Os dados relacionados mudam com muita frequência**: se a informação muda o tempo todo, é mais fácil atualizá-la em um único lugar centralizado do que procurar em milhares de documentos aninhados.
> 3. **Arrays ilimitados**: quando a quantidade de itens relacionados pode crescer sem limite.
>
> ---

## Exercício 53

### Situações onde seria melhor usar dados incorporados no mesmo documento

> ---
>
> R: Dados aninhados devem ser usados quando:
>
> 1. Os dados são lidos juntos a maior parte do tempo: se toda vez que você busca um usuário você também precisa do endereço dele, aninhe o endereço (como foi feito com a Ana Souza e o Carlos Lima na sua base).
> 2. Relacionamentos pequenos: por exemplo, incluir as formas de contato de uma pessoa ou o diretor de um filme. O filme Matrix tem diretores limitados, então é seguro aninhar essa informação.
> 3. Os dados aninhados não mudam com frequência: informações estáticas ou de raras mudanças são candidatas perfeitas para aninhamento.
>
> ---
