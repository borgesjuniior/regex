# Regex

Qualquer Regex precisa ser interpretada por meio de uma Regex engine. Esse motor é uma parte de software que processa a expressão, tentando achar o padrão dentro da string dada, devolvendo os resultados. Normalmente a Regex engine faz parte da aplicação maior, para executar uma validação ou busca de uma string.

```javascript
  var regexObject = new RegExp(textoPattern, 'g');
 ```

No construtor passamos o pattern a expressão regular a ser processada, a letra g é uma flag do mundo JavaScript e significa global, para aplicar a regex na string inteira (e não parar no primeiro match).

 ```javascript
  while (result = regexObject.exec(textoTarget)) {
    //codigo omitido
  } 
 ```

O método exec devolve um resultado que possui a string match *e outras informações, como a posição/index. *Repare que estamos fazendo um laço sempre pedindo o próximo resultado.

 ## Meta-char e quantifiers

- ``\d`` significa qualquer dígito (atalho para [0-9])
- ``\s+`` corresponde a um ou mais caracteres de espaços em branco (classe white-spaces), atalho para [ \t\r\n\f]
- ``\w`` significa word char e é uma atalho para [A-Za-z0-9_]
- ``.`` significa qualquer char
- ``*`` serve para definir uma quantidade de chars, zero ou mais vezes 
- ``+`` uma ou mais vezes
- ``{}`` as chaves que servem para definir uma quantidade de caracteres específicas que é desejado encontrar (ex: ``a{1, 3}`` - letra ``a`` 1 até 3 vezes || ``\d* ``um dígito zero ou mais vezes)
- ``?`` sigifica que o caractere anterior é opcional

Lembrando também, se quisermos procurar pelo ``*`` ou ``.`` literalmente (sem significado especial), devemos utilizar o caractere ``\``

## Classes de caracteres

Classe de caractere é uma maneira de dizer à Regex Engine que queremos pegar um conjunto específico de caracteres. Basicamente, é como se definíssemos que o que pode ou não aparecer naquele trecho (ou em toda) da expressão, uma classe pré definida no Regex é o ``\d`` que é um atalho para ``[1-9]`` mas nada impede de criarmos a nossa própria classe.

Exemplos de classes
- ``\d`` é um atalho para [1-9]
- ``[A-Z]`` de A até Z em letras maiúsculas 
- ``[a-z]`` de mesma forma em low-case (ex: ``[A-Z][a-z]{5}`` - procura uma palavra com a primeira letra em Upper-case e as 5 seguintes em low-case)

### Como criar minha classe?

Podemos declarar com a classe: ``[.-]``. Perceba também que não escapamos o ponto ``.``, pois dentro dos colchetes o ponto já possui o seu valor literal. Importante também é que a ordem não importa, [.-] e [-.] são a mesma coisa. (ex: ``\d{3}[.-]?\d{3}[.-]?\d{3}[.-]?\d{2}``)


## Legibilidade 

Dada a sequinte expressão: ``[0123]?\d\s+de\s+[A-Z][a-zç]{1,8}\s+de\s+[12]\d{3}``

Uma forma de deixar a expressão mais legível é usar algumas váriaveis auxiliares, como por exemplo:

```javascript
  var DIA  = "[0123]?\d"; 
  var _DE_ = "\s+de\s+";
  var MES  = "[A-Za-z][a-zç]{1,8}";
  var ANO  = "[12]\d{3}";
  //Cada variável representa uma parte da regex. Depois disso é só concatenar essas variáveis para ter a expressão final
  var stringRegex = DIA + _DE_ +  MES + _DE_ + ANO;
  //Essa string é passada para dentro do Regex Engine
  var objetoRegex  = new RegExp(stringRegex, 'g');
```

## Âncoras 

O que queremos na verdade é encontrar o início e o fim de uma palavra, no geral, âncoras servem para que você delimite um começo e um fim em uma expressão regular. Basicamente você diz que a correspondência dessa expressão deve começar e terminar daquele jeito, e caso não corresponda, já seja descartada.

> denise teste, 987.654.321-00,28 de Dezembro de 1991,(31)45562712,SCS Qd. 8 Bl. B-50,11,70333-900,Rio Grande 

Regex

```nano 
  \bde\b
``` 

Repare que usamos o ``\b ``no início e no fim, ou seja, queremos buscar exatamente a preposição ``de``. Testando isso no nosso formulário, agora sim só selecionamos apenas a palavra ``de``, existe a inversão do \b, o non-word-boundary: \B (B maiúsculo), em outras palavras, a silaba por deve aparecer dentro de uma palavra, nunca no inicio ou fim.

existem vários tipos de âncoras mas as mais comuns são: ``^, $ e \b``

- ``^`` avalia a string inteira e garante que nenhuma expressão venha antes, ou seja, é a primeira expressão.
- ``$`` de modo similiar ao ``^``, garante que a expressão seja a ultima da string. 

> file:///Users/nico/Downloads/regex/index.html

Regex

```nano
  ^file.+html$
```

Ou seja, tem que começar com file e terminar com html, caso qualquer expressão seja diferente disso ela será descartada.

## Grupos

Grupos de caracteres em expressões regulares são formas de agrupar expressões. Com eles, podemos selecionar o que queremos ou não receber nos resultados.

Até agora, estamos definindo expressões sem grupo e quando temos um match, é no resultado inteiro da expressão. Mas… e se quisermos pegar só um pedaço daquela expressão? Só uma parte em específico e o resto podemos até descartar? observe o exemplo abaixo:

- queremos buscar o texto que está dentro das tags
```html
  <h1>Júnior</h1>
```
- regex busca o h1 e qualquer que seja o texto que tenha dentro
```javascript
  ^<h1>.+</h1>$
```
 Porém queremos buscar apenas o texto dentro da tag, para isso usamos os ``( )``, para envolver a expressão que queremos agrupar:

 ```javascript
  ^<h1>(.+)</h1>$
```

### Non-capturing group
Como vimos no nosso exemplo, agora estamos pegando os valores dos grupos. Mas como solicitar à Regex Engine para que NÃO traga o valor de algum grupo? Para faze-lo, basta no início do grupo digitar (?:)
