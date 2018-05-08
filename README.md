# SED
O comando sed, é usado para editar linhas de texto, dentro de arquivos, ou em memória. Sed vem instalado em muitas distribuições linux por default, então é muito provável que sua máquina já tenha este programa.

O programa usa expressões regulares para realizar a substituição de valores dentro de textos. Mas o que são expressões regulares?

## Expressões regulares

Uma expressão regular é uma sequência de caracteres que forma um padrão de pesquisa. Para simplificar, Expressões regulares, ou regex são padrões de busca de dados dentro de arquivos ou textos. Imagine o seguinte cenário: Você tem um arquivo muito grande contendo o cadastro de clientes, e foi pedido para que você altere todos os nomes de JUAO, para JOAO (o estagiário escreveu errado, você precisa consertar). Como fazer isso?

## Uso básico do sed

Antes de nos aprofundar em expressões regulares, vamos ver como o sed funciona.

Você tem o arquivo com os cadastros errados (cadastro.txt) para ser alterado:

```bash
sed 's/JUAO/JOAO/g' cadastro.txt
```
* s: indica substituição
* JUAO: Indica o termo a ser buscado
* JOAO: Indica o que vai ser colocado no lugar do termo buscado
* g: Indica que é globa, ou seja, funciona para todas as linhas do arquivo.

## Uso de sed com regex simples

Muitas vezes a substituição não será tão simples como JUAO/JOAO, e você precisará encontrar padrões para alterar. Se ao invés de apenas JUAO, você também precise buscar JAAO,JBAO,JCAO,etc... ?

Se prestarmos atenção, vamos notar que há um padrão no que pode ter sido escrito:

* Primeira letra: J
* Segunda Letra: Pode ser de A até Z... não sabemos
* Terceira Letra:A
* Quarta Letra:O

Nesse caso podemos usar o seguinte comando:

```bash
sed 's/J[A-Z]AO/JOAO/g' cadastro.txt
```
Perceba que houve apenas uma alteração: No lugar de "U", colocamos "[A-Z]". Isso indica que naquela posição, podem haver todas as letras de A até Z (apenas maiúsculas). se quisermos adicionar também dígitos, podemos incluir dentro dos colchetes outros valores:
``` bash
sed 's/J[A-Z0-9]AO/JOAO/g' cadastro.txt
```

Se quisermos filtrar apenas as vogais (JAAO,JEAO,JIAO,JUAO,JOAO), podemos usar o comando da seguinte forma:

``` bash
sed 's/J[AEIOU]AO/JOAO/g' cadastro.txt
```
Dessa forma, apenas as letras dentro dos colchetes serão usadas.

Além de substituição de padrão, o sed também deleta linhas inteiras, altera linhas inteiras, faz o append antes ou depois de determinada linha.

Exemplos:

```bash
sed 's/antigo/novo/g' arquivo.txt # altera o antigo pelo novo
sed '1,5s/antigo/novo/' arquivo.txt # Altera antigo por novo, nas linhas 1 a 5
sed '1a\novo_texto' arquivo.txt # Adiciona novo_texto após a linha 1
sed '2i\novo_texto' arquivo.txt # Adiciona novo_texto Antes da linha 1
sed '5c\novo_texto' arquivo.txt # Troca a linha 5 por texto_novo
sed '3,5d' arquivo.txt # remove as linhas 3 até 5
sed '3,5!d' arquivo.txt # remove tudo, EXCETO as linhas 3 até 5
sed '/azul/d' arquivo.txt # apaga todas as linhas que contém a palavra azul
sed '/azul/,/verde/d' arquivo.txt # apaga as linhas a partir da linha que tem a palavra azul, até a linha que tem a palavra verde
```

## expressões regulares
Você já deve imaginar que [a-z] é uma expressão regular, então vamos ver outras opções que podem ser usadas para realizar buscas:

* [a-z]
    * todos os caracteres de "a" a "z"
* [a-Z]
    * todos os caracteres de "a" a "z" (maiúsculas e minúsculas)
* [0-9]
    * todos os dígitos
* .
    * Qualquer caracter

Temos também alguns metacaracteres que identificam a quantidade:

* +
    * Uma ou mais vezes
* *
    * Zero ou mais vezes
* ?
    * Zero ou uma vez

E alguns que identificam o ínicio e fim de linha:

* ^
    * Início de linha
* $
    * Final de linha

Experimente as combinações para chegar aos mais diversos resultados. Alguns destes metacaracteres podem precisar que você execute o comando sed com a opção -r, que usa as expressões regulares estendidas.

```bash
sed -r 's/[a-z]+/outraCoisa/g' arquivo.txt
```

## Exercícios
Para os exercícios você vai precisar do arquivo nomes.txt

1. Mude o nome Andre para Fabio
2. Mude o todas as letras maiúsculas para "9"
3. Deixe apenas as iniciais de cada nome (sem espaços)
4. deixe as inciais no formato (Nome Sobrenome => N.S.)
5. Deixe apenas os sobrenomes
6. inclua seu nome e sobrenome na décima linha
7. remova os 3 primeiros nomes
8. Mude os nomes com 4 caracteres para "Marcos" (não alterar sobrenomes)

## Desafio
Coloque a lista de nomes em ordem alfabética, e mostre os nomes no formato: (Nome Sobrenome => N.Sobrenome)