# chatbot_whith_ai_alura
Curso Alura IA 2024 2

No nosso curso de construção de um chatbot com Python e OpenAI, vamos aprender a utilizar as APIs da OpenAI para conectar com o front-end em Flask, usando Python. Vamos abordar também como utilizar um contexto dinâmico para selecionar personas e documentos de forma adequada, com base nas respostas das pessoas usuárias.

Aprenderemos a gerenciar o histórico, utilizando threads (linhas de execução) e assistentes, com os novos recursos que a OpenAI tem apresentado. Além disso, veremos como utilizar functions calling para trazer para dentro da nossa aplicação desvios de processos que, naturalmente, o modelo da OpenAI seria incapaz de realizar.

Por fim, teremos um exercício prático que vai envolver a parte de visão computacional, para garantir que o nosso chatbot seja capaz de interpretar imagens enviadas pelas pessoas usuárias.

## Entendendo o FrontEnd do projeto

Apesar de este curso ter como foco a implementação do chatbot do ponto de vista de back-end, utilizando Python e Flask, disponibilizamos este material para você que quer entender melhor a integração com o front-end, principalmente no que diz respeito ao código JavaScript responsável pela exibição das respostas fornecidas pela API do chatbot.

Para começar, vamos considerar o arquivo index.js que se encontra no caminho static/js do projeto. Segue abaixo o código completo da função que vamos explorar:

A função enviarMensagem() é disparada quando clicamos no botão de enviar do chat ou pressionamos a tecla Enter. Ela é responsável por atualizar o chat para mostrar o prompt que acabamos de enviar e depois a resposta recebida da API.

Agora vamos entender por partes o que acontece na função quando enviamos a mensagem.

1. Tratamento da entrada
No trecho abaixo verificamos se a mensagem enviada é vazia:

```js
    if(input.value == "" || input.value == null) return;
    let mensagem = input.value;
    input.value = "";
```
Se isso for verdade e a mensagem é vazia, simplesmente retornamos da função pois não há nada a fazer. Caso contrário, capturamos a mensagem que foi digitada no campo input do chat e guardamos na variável mensagem.

Se tiver curiosidade, você pode abrir o arquivo templates/index.html para verificar essa amarração entre o campo de entrada e o código JavaScript. Lá você vai encontrar a seguinte linha:

```html
<input type="text" class="entrada__input" placeholder="Enviar uma mensagem" id="input">
```

Esse é o código em HTML para a definição de um campo de entrada do tipo texto com o id “input”. Esse id é o nome que acessamos no código JavaScript para pegar o valor que foi digitado pela pessoa usuária do chat.

Por fim, apagamos o texto que foi digitado em preparação para que a pessoa usuária possa enviar novas mensagens.

2. Exibição da mensagem que foi digitada na conversa
Com o código abaixo estamos criando uma nova bolha no histórico da conversa para exibir a mensagem que acabamos de enviar:

```js
    let novaBolha = criaBolhaUsuario();
    novaBolha.innerHTML = mensagem;
    chat.appendChild(novaBolha);
```

A função criaBolhaUsuario() vai criar um novo elemento HTML que é armazenado temporariamente na variável novaBolha. A partir dessa variável conseguimos acessar uma propriedade chamada innerHTML. Essa propriedade permite a edição do conteúdo que vai dentro da bolha, isto é, a mensagem que queremos exibir dentro dela. Por fim, invocamos o método appendChild no chat para que esse novo elemento HTML seja inserido de fato na página e seja exibido logo abaixo do histórico de mensagens já existente.

3. Exibição da mensagem de resposta que vem da API
O primeiro trecho de código que vamos considerar é o seguinte:

```js
    let novaBolhaBot = criaBolhaBot();
    chat.appendChild(novaBolhaBot);
    vaiParaFinalDoChat();
    novaBolhaBot.innerHTML = "Analisando ...";
```

Ele é bastante similar ao código para criar a bolha com a mensagem do usuário, mas agora estamos chamando uma função diferente para que a bolha seja criada do lado esquerdo, representando uma resposta do chatbot. Também fazemos uma chamada adicional para a função vaiParaFinalDoChat(), que é responsável por fazer a rolagem da tela para que a mensagem do chatbot fique visível na tela, principalmente quando for mais extensa. Por fim, garantindo que a nova bolha tenha em seu conteúdo o texto "Analisando …" até que a resposta do bot seja carregada.

Perceba que a bolha do chatbot está na interface, mas ainda precisamos preencher com o conteúdo recebido do chatbot. Para entender como isso é feito, vamos analisar o trecho seguinte código:

```js
// Envia requisição com a mensagem para a API do chatbot
   const resposta = await fetch("http://127.0.0.1:5000/chat", {
        method: "POST",
        headers: {
        "Content-Type": "application/json",
        },
        body: JSON.stringify({'msg':mensagem}),
    });
```

Neste trecho estamos fazendo a requisição para o endpoint /chat do nosso back-end em Flask. Especificamos que o método da requisição é um POST e indicamos no cabeçalho que o tipo de dado que estamos enviando é um JSON. Logo em seguida, descrevemos o corpo da requisição (body) e montamos um JSON com um único campo msg, cujo conteúdo é a variável mensagem que capturamos do usuário logo no início da função enviarMensagem().

Por fim, acessamos a variável que representa a bolha do chat (novaBolhaBot) e de forma similar ao que fizemos com a bolha do usuário, acessamos a propriedade innerHTML para modificar o conteúdo da bolha com a textoDaResposta que temos.

Isso encerra o código de integração com o back-end. Podemos perceber que, de certa forma, o código lembra um pouco aquele que existe no back-end em Python.

## 📚 Referências de Leitura

- [Documentação Whisper](https://openai.com/research/whisper)
- [Documentação Dall-E](https://openai.com/research/dall-e)
- [Preços OpenAI](https://openai.com/pricing)
- [Áudios Longos](https://platform.openai.com/docs/guides/speech-to-text/prompting)
