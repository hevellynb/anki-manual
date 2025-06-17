# Substituições de campo

## Substituições básicas

O modelo mais simples é parecido com isso:

    {{Frente}}

Quando você coloca texto entre chaves, Anki procura por um campo com esse nome e substitui o texto com o conteúdo daquele campo.

Nomes de campo diferenciam maiúsculas de minúsculas. Se você tem um campo nomeado ‘Frente’, escrever ‘\\{{frente}}\\’ não irá funcionar corretamente.

Seus modelos não estão limitados a uma lista de campos. Você também pode incluir textos arbitrários no seus modelos. 
Por exemplo, se você está estudando capitais, e você criou um tipo de nota com um campo ‘País’, você pode criar um modelo de frente como esse:

    Qual a capital de {{País}}?

O modelo padrão do verso do cartão pode ser algo assim:

    {{FrontSide}}

    <hr id=resposta>

    {{Verso}}

Isso significa “me mostre o texto que está na frente do cartão, depois uma linha divisora, e então o campo Verso”.

A parte ‘id=resposta’ fala para Anki onde o divisor está entre a questão e a resposta. Isso permite que Anki automaticamente vá para o lugar
onde a resposta começa quando você aperta ‘mostrar resposta’ em um cartão grande (útil especialmente em aparelhos móveis com pequenas telas).
Se você não quer uma linha horizontal no começo da resposta, você pode utilizar outro elemento HTML como o parágrafo ou div.

## Novas linhas

Modelos de cartões são como páginas web, o que significa que um comando especial é necessário para criar uma nova linha.
Por exemplo, se você escreveu o seguinte no modelo:

    um
    dois

Na pré-visualização você verá:

    um dois

Para adicionar uma nova linha, você precisa adicionar uma marcação &lt;br&gt; no final de uma linha, assim:

    um<br>
    dois

A marcação br indica a quebra de linha.

O mesmo se aplica para campos. Se você quer mostrar dois campos, um em cada linha, você usaria

    {{Campo 1}}<br>
    {{Campo 2}}

## Texto para Fala

Esse recurso requer que você esteja utilizando Anki 2.1.20 ou AnkiMobile 2.0.56. AnkiDroid não dispõe atualmente desse recurso.

Para que Anki leia o campo da Frente em Inglês dos EUA, você pode colocar o seguinte no seu modelo de cartão:

    {{tts en_US:Frente}}

No Windows, MacOS e iOS, Anki usará as vozes embutidas do sistema operacional. No Linux não há vozes embutidas,
mas vozes podem ser fornecidas por extensões, [como essa](https://ankiweb.net/shared/info/391644525).

Para ver uma lista de todas as linguagens/vozes disponíveis, coloque o seguinte no seu modelo de cartão:

    {{tts-voices:}}

Se houver múltiplas vozes disponíveis para o idioma escolhido, você pode especificar as vozes desejadas em uma lista,
Anki irá escolher a primeira voz disponível. Por exemplo:

    {{tts ja_JP voices=Apple_Otoya,Microsoft_Haruka:Campo}}

Isso faria com que o Anki usasse Otoya em um dispositivo Apple e Haruka em um computador com Windows.

Especificar uma velocidade diferente também é possível em algumas implementações de texto para fala:

    {{tts fr_FR speed=0.8:AlgumCampo}}

Tanto a velocidade quanto a voz são opcionais, mas o idioma deve ser incluído.

Em um Mac, você pode customizar as vozes disponíveis:

- Abra a tela de Ajustes do Sistema.

- Clique em Acessibilidade.

- Clique em Fala.

- Clique no menu suspenso de voz do sistema e escolha Personalizar.

Algumas vozes soam melhor que outras, então experimente para escolher sua preferida. Observe que a voz da Siri
só pode ser usada em aplicativos Apple. Quando você tiver instalado novas vozes, será necessário reiniciar Anki
para que as novas vozes se tornem disponíveis.  

## Campos especiais

Existem alguns campos especiais que você pode incluir em seus modelos:

    As etiquetas da nota: {{Etiquetas}}

    O tipo de nota: {{Tipo}}

    O baralho dos cartões: {{Baralho}}

    O sub-baralho dos cartões: {{SubBaralho}}

    O tipo de cartão (“Avançar”, etc): {{Cartão}}

    O conteúdo do modelo da frente
    (só válido no modelo do verso): {{FrontSide}}

FrontSide não irá tocar automaticamente nenhum áudio que estava no lado da frente do cartão. Se você deseja que o mesmo áudio
seja tocado automaticamente tanto na frente quanto no verso do cartão, você terá que manualmente incluir os campos de áudio
no verso também.

Assim como em outros campos, o nome dos campos especiais diferenciam maiúsculas de minúsculas - você deve usar {{Etiquetas}}
ao invés de {{etiquetas}} por exemplo.

## Campos de dica

É possível adicionar um campo na frente ou verso de um cartão que continua escondido até que você escolha mostrá-lo.
Chamamos isso de ‘campo de dica’. Antes de adicionar uma dica, por favor tenha em mente que quanto mais fácil você deixar
uma questão no Anki, mais difícil será para você lembrá-la em situações do cotidiano. Por favor leia sobre o ‘princípio de informação mínimo’
em <http://www.supermemo.com/articles/20rules.htm> antes de continuar.

Primeiro, você terá que adicionar um campo para guardar a dica, caso você já não o tenha. Veja a seção de [campos](../editing.md#customizing-fields)
se não souber como fazer isso.

Assumindo que você criou um campo chamado MeuCampo, você pode fazer com que Anki o inclua no cartão mas deixe-o escondido
por padrão adicionando o seguinte no seu modelo:

    {{dica:MeuCampo}}

Isso irá mostrar um link “mostrar dica”; quando clicado, o conteúdo do campo será visível no cartão.
(Caso MeuCampo esteja vazio, nada irá aparecer).

Se você mostrar dica em uma questão e depois revelar a resposta, a dica será escondida novamente. Se você deseja que a
dica sempre seja revelada após a resposta ser mostrada, você terá que remover \\{{FrontSide}} do seu modelo de verso e
manualmente adicionar os campos que você deseja que apareça.

No momento não é possível utilizar campos de dica para áudios — o áudio irá tocar independentemente do link de dica ter sido apertado.

Se você deseja customizar a aparência ou comportamento, será necessário implementar o campo de dica você mesmo. Não podemos
fornecer nenhum suporte nesse caso, mas o código abaixo pode ajudar a começar:

    {{#Verso}}
    <a class=hint href="#"
    onclick="this.style.display='none';document.getElementById('hint4753594160').
    style.display='inline-block';return false;">
    Mostrar verso</a><div id="hint4753594160" 
    class=hint style="display: none">{{Back}}</div>
    {{/Verso}}

## Links de dicionário

Você pode utilizar substituições de campo para criar links de dicionário. Imagine que você esteja estudando um idioma
e o seu dicionário online favorito permita que você procure por texto utilizando uma URL web como:

    http://exemplo.com/search?q=minhapalavra

Você pode adicionar um link automático fazendo o seguinte:

    {{Expressao}}

    <a href="http://exemplo.com/search?q={{Expressao}}">veja no dicionário</a>

O modelo acima permite que você pesquise a expressão de cada nota clicando no link enquanto revisa. No entanto,
há uma ressalva, então consulte a próxima sessão.

## Remoção de formatação em HTML

Assim como modelos, campos são armazenados em HTML. No exemplo de link de dicionário acima, caso a expressão contenha a
palavra “minhapalavra” sem nenhuma formatação, então o HTML seria o mesmo: “minhapalavra”. Mas quando você inclui formatação em seus campos,
HTML extra será incluído. Se “minhapalavra” estivesse em negrito, o HTML seria "&lt;b&gt;minhapalavra&lt;/b&gt;".

Isso pode acabar trazendo problemas em campos como os links de dicionário. No exemplo acima, o link de dicionário ficaria assim:

    <a href="http://exemplo.com/search?q=<b>minhapalavra</b>">veja no dicionário</a>

Os caracteres extra no link confundiria o site de dicionário que provavelmente não encontraria resultados.

Para resolver isso, Anki oferece a capacidade de remover a formatação dos campos quando eles são substituídos. Se você prefixar
um nome de campo com texto, Anki não incluirá nenhuma formatação. Então um link de dicionário ficaria:

    <a href="http://exemplo.com/search?q={{text:Expressao}}">veja no dicionário</a>

## Texto da direita para esquerda

Se você está utilizando um idioma cuja leitura é feita da direita para a esquerda, você precisará ajustar o modelo dessa forma:

    <div dir=rtl>{{CampoQuePossuiTextoDireitaParaEsquerda}}</div>

## Mídia e LaTeX

Anki não percebe referências à mídia em modelos porque isso leva tempo. Isso significa que você precisa tomar ações extras para
incluir mídia no seu modelo.

### Sons/imagens estáticos

Se você deseja incluir imagens ou sons em seus cartões que são os mesmos para qualquer cartão (ex: a logo de uma empresa no topo de cada cartão):

1. Renomeie o arquivo para que comece com um subtraço (underline), como por exemplo “\_logo.png”. O subtraço diz para o Anki
que o arquivo é usado pelo o modelo e que deve ser exportado quando o baralho é compartilhado.

2. Adicione uma referência para a mídia na frente ou verso do seu modelo, como:

<!-- -->

    <img src="_logo.jpg">

### Referências para campos

Referências de mídia para campos não estão disponíveis. Eles podem ou não serem exibidos durante a revisão e não funcionarão
ao procurar por mídia não utilizada, importação/exportação e assim por diante. Exemplos que não funcionam:

    <img src="{{Expressao}}.jpg">

    [sound:{{Palavra}}]

    [latex]{{Campo 1}}[/latex]

Ao invés disso, você deve incluir as referências à mídia no campo. Veja a seção de importação para mais informações.

## Verificando sua resposta

Você pode assistir [um vídeo sobre essa função](http://www.youtube.com/watch?v=5tYObQ3ocrw&yt:cc=on) no YouTube.

A maneira mais fácil de verificar sua resposta é clicar no topo à esquerda da tela de adição do cartão e selecionar “Básico (digite a resposta)”.

Se você tiver baixado um baralho compartilhado e gostaria de digitar a resposta nele você pode modificar o modelo dos cartões. Se o modelo for assim:

    {{Palavra Nativa}}

    {{FrontSide}}

    <hr id=resposta>

    {{Palavra Estrangeira}}

Para digitar a palavra estrangeira e verificar que está correta, você precisa editar o modelo da frente para que se pareça com isso:

    {{Palavra Nativa}}
    {{type: Palavra Estrangeira}}

Note que adicionamos ‘type:’ (digite) na frente do campo que desejamos comparar. Já que FrontSide está no verso no cartão, a caixa
de digitação da resposta aparecerá no verso também.

Quando revisando, Anki irá mostrar uma caixa de texto onde você pode digitar a resposta, quando enter ou o botão de mostrar resposta
é apertado, Anki irá mostrar quais partes você acertou e quais partes você errou. O tamanho da fonte da caixa de texto será o tamanho
que você configurou para aquele campo (no botão de “Campos” durante a edição).

Essa função não muda como os cartões são respondidos, então, ainda é você que decide o quão bem você lembrou ou não uma resposta.

Somente uma comparação por digitação pode ser usada por cartão. Se você adicionar o texto acima múltiplas vezes não irá funcionar.
Também só é possível escrever em uma única linha, então essa função acaba não sendo útil ao comparar um campo composto por múltiplas linhas.

Anki utiliza uma fonte monoespaçada para a comparação da resposta para que as seções “providas” e “corretas” sejam alinhadas.
Se você deseja substituir a fonte para a comparação de respostas, você pode colocar o seguinte no fim da sua seção de estilo:

    code#typeans { font-family: "minhafonte"; }

O que irá afetar o HTML seguinte para a comparação de respostas:

    <code id=typeans>...</code>

Usuários mais avançados podem substituir as core padrões da digitação/resposta com as classes de css ‘typeGood’ (boa),
’typeBad’ (ruim) e 'typeMissed’ (faltando). AnkiMobile suporta  ‘typeGood’ (boa) e ’typeBad’ (ruim) mas não  'typeMissed’ (faltando).

Se você deseja alterar o tamanho da caixa de digitação e não quer mudar a fonte na caixa de diálogo Campos,
você pode mudar o estilo padrão embutido utilizando `!important`, assim:

    #typeans { font-size: 50px !important; }

Também é possível utilizar o método de digitação de resposta em cartões de exclusão cloze (teste onde partes de um texto
são removidas e o usuário deve preencher as lacunas). Para fazer isso, adicione \\{\\{type:cloze:Text}} tanto no modelo da frente
quanto do verso. O verso deverá parecer com isso:

    {{cloze:Text}}
    {{type:cloze:Text}}
    {{Extra}}

Note que o tipo cloze não utiliza FrontSide, portanto é necessário adicionar o trecho acima em ambos os lados de uma nota cloze.

Se houver múltiplas seções omitidas, você pode separar suas respostas na caixa de texto utilizando vírgula.

Caixas de digitação de resposta não irão aparecer na [caixa de "pré-visualização"](intro.md) no navegador. Quando você revisa ou
vê a pré-visualização na janela de tipos de cartão, elas aparecerão.
