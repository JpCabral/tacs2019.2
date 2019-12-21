# Guia do desenvolvedor 1.0

## Introdução

Este guia usará como base a linguagem Java que por ser muito utilizada no ambiente acadêmico acaba por sofrer de diversos vícios ao se codificar, dificultando bastante a compreensão do produto final. Diante disso, é recomendado desenvolver códigos de maneira limpa, de modo a possibilitar maior manutenibilidade do software, afinal, programa-se também para outras pessoas e não apenas para computadores. Este breve guia fornece ao desenvolvedor dicas de como escrever códigos de maneira mais limpa tendo como base o livro "Código Limpo", de Robert C. Martin. 

## Nomes autoexplicativos

A seguir, serão dadas algumas dicas quanto à nomeação de variáveis, classes e métodos.

### Nomes que revelem seu objetivo

O nome de uma variável, função ou classe necessita de responder todas às principais questões de um código, devendo dizer por que existe, o que faz e como é utilizado. Caso este necessite de um comentário para ser explicado, então não é adequado.

``` Java
int i; // contador do laço de repetição
```
No exemplo anterior, o nome "i" não revela muita coisa. Por isso, deve ser escolhido um nome que possa especificar seu uso para controlar as iterações de um laço de repetição:

``` Java
int contador;
```
### Distinções significativas

Em algumas ocasiões, pode ser necessário referir-se a duas variáveis diferentes no mesmo escopo. Como estas não podem possuir o mesmo nome, geralmente opta-se por adicionar números ou letras sequenciais em nomes. Ainda que isso não gere confusão, não oferece informações ou dicas sobre a intenção do programador.

``` Java
public static void copiarNumeros(int n1[], int n2[]) {
    for (int i = 0; i < n1.length; i++) {
        n2[i] = ni[i];   
    }
}
```

Tal método pode ser lido mais facilmente caso o nome de seus parâmetros sejam "vetorOrigem" e "vetorDestino", por exemplo.

### Nomes pronunciáveis
```Java
class CerimEm2019 {
       private Date datInYmdhsm;
       private Date datFYmdhsm;
       private int anoCerim = 2019;
}
```
Em um diálogo entre desenvolvedores, soa um tanto incoerente pronunciar os nomes das classes e atributos do exemplo, já que estes são uma junção de abreviações e siglas. Para proporcionar um diálogo inteligível, é preferível utilizar termos existentes do idioma:

```Java
class EmmyAwards {
       private Date dataInicalTimestamp;
       private Date dataFinalTimestamp;
       private int anoCerimonia = 2019;
}
```

### Nomes de Classes e métodos

Classes e objetos devem ser denominadas com substantivos, propriamente ditos, por exemplo, Estudante, SalaDeAula, AcademyAwards. Palavras genéricas como Gerente, Processador, Dados devem ser evitadas, além de não poderem ser utilizados verbos para nomeá-las.

Em relação aos nomes dos métodos, estes devem conter verbos, como atualizarEstudante, removerSalaDeAula, adicionarDiscente. É preciso nomear métodos de acesso, alteração e autenticação de acordo com seus valores, adicionando prefixos *get*, *set* ou *is*, segundo o padrão *javabean*.

```Java
String nome = estudante.getNome();
turma.setTurma("Técnicas Avançadas de Programação");
if (oscar.isEmpty()) ...
```

## Funções

Serão comentados, a seguir, os principais pontos aos se considerar funções no código.

### Tamanho

A primeira regra sobre funções é que elas devem ser pequenas. Como exemplo, segue uma função que calcula, dentre dois números, o maior destes.

```Java
public static int obtemMaiorNumero(int valorInicial, int valorFinal) {
    if (valorInicial > valorFinal)
        return valorInicial;
    return valorFinal;
}
```

### Coesão

As funções devem se limitar a fazer o mínimo de ações possíveis, ou seja, elas devem realizar se possível apenas uma tarefa. O exemplo a seguir mostra uma função que realiza diversas tarefas.

```Java
public static int verificaNumero(int valorInicial, int valorFinal, int[] vetor) {
    int maiorValor, quantidadeDeOcorrencias = 0;
    if (valorInicial > valorFinal) {
        maiorValor = valorInicial;
    } else {
        maiorValor = valorFinal;
    }
    for (int i = 0; i < vetor.length; i++) {
        if (vetor[i] == maiorValor) {
            quantidadeDeOcorrencias++;
        }
    }
    return quantidadeDeOcorrencias;
}
```
A partir disso, observe que o método mostrado pode ser dividido em dois outros:

```Java
public static int obtemMaiorNumero(int valorInicial, int valorFinal) {
    if (valorInicial > valorFinal)
        return valorInicial;
    return valorFinal;
}

public static int verificaOcorrenciaPorVetor(int numero, int[] vetor) {
    int quantidadeDeOcorrencias = 0;
    for (int i = 0; i < vetor.length; i++)
        if (vetor[i] == numero)
            quantidadeDeOcorrencias++;
    return quantidadeDeOcorrencias;
}
```

### Parâmetros

A quantidade ideal de parâmetros em uma função é zero, e a máxima são dois. Deve-se evitar, sempre que possível, o uso de três ou mais parâmetros. Isso porque eles podem requerer determinados conceitos.

```Java
public static void verificaNumero(Scanner scanner, int valorExistente) {
    System.out.println("Entre com um valor: ");
    int valorLido = scanner.nextInt();
    if (valorLido > valorExistente) {
        System.out.println("Valor lido é maior que o existente");
    } else {
        System.out.println("Valor lido é menor que o existente");
    }
}
```

No exemplo, como o parâmetro *scanner* não foi declarado e instanciado dentro do próprio método, os leitores do método podem ter que interpretá-lo toda vez que acessarem a função.

Por outro lado, uma grande quantidade de parâmetros pode tornar mais complexa a atividade de testes. Pode haver maior dificuldade ao se escrever todos os casos de teste para se certificar de que as diversas combinações de parâmetros funcionem adequadamente. 

## Comentários

A verdade é que, se as linguagens de programação fossem expressivas o suficiente ou se os desenvolvedores as manipulassem com eficácia para expressar suas reais intenções, não se precisaria de muitos comentários, provavelmente nenhum. 
A seguir, alguns exemplos de bons e maus comentários:

### Bons comentários

#### Comentário TODO

```Java
public static void gerarArquivoHtml() {
    // TODO Finalizar função. 
}
```

#### Javadocs em APIs públicas

```Java
/**
 * Retorna o número correspondente a um dia da semana, de 0 a 6, a partir de uma data.
 * 
 * @param data Uma data no formato yyyy-mm-dd.
 * @return O dia da semana correspondente à data, de 0 a 6, ou -1 em casos inválidos.
 */
public int calculaNumeroDiaDaSemana(int data) ...
```

### Maus comentários

#### Comentários redundantes

```Java
// Método recebe um número, o qual é dividido por 2. Caso o resto for zero, a função
// retorna true, caso contrário, retorna falso.
public static boolean verificaNumeroPar(int numero) {
    if (numero % 2 == 0)
        return true;
    return false;
}
```

## Formatação

A formatação de um código é bastante importante. Ela serve como uma comunicação, devendo ser até mesmo a primeira regra em detrimento a "fazer o código funcionar". Isso deve ser considerado, pois a funcionalidade criada tem grandes chances de ser alterada posteriormente, mas a legibilidade, através da formatação, persistirá, tendo efeito em todas as mudanças posteriores.


Retirando-se os espaçamentos, durante uma leitura, os olhos só parariam após o término do código, o que prejudicaria bastante a legibilidade.

```Java
package br.ufg.inf.es.awards;
import br.ufg.inf.es.person.Actor;
import java.util.List;
public class AcademyAwards {
    private int year;
    private String localCeremony;
    public int getYear() {
        return this.year;
    }
    public void setYear(int year) {
        this.year = year;
    } 
}
```
## Referências

MARTIN, Robert C. Código Limpo: Habilidades Práticas do Agile Software. 1ª edição. Rio de Janeiro: Editora Alta Books, 2011.


