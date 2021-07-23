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

 ## Meta-char
- ``\d`` significa qualquer dígito
- ``.`` significa qualquer char
- ``*`` serve para definir uma quantidade de chars, zero ou mais vezes 
- ``{}`` as chaves que servem para definir uma quantidade de caracteres específicas que é desejado encontrar (ex: a{3} - letra a 3 vezes ou até \d* um DIGITO zero ou mais vezes)
- ()

Lembrando também, se quisermos procurar pelo ``*`` ou ``.`` literalmente (sem significado especial), devemos utilizar o caractere ``\``

