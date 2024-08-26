# 50+ Perguntas de Java para entrevistas de emprego
[Fonte das questões](https://codeburst.io/review-these-50-questions-to-crack-your-java-programming-interview-69d03d746b7f) /
[Tradução e respostas](https://www.linkedin.com/in/marqsleal/)

## 1 - Como o Java atinge a sua independência de plataforma?

Java é uma linguagem de programação que nos dá independência de plataforma, ou seja, um programa em Java pode ser executado em qualquer plataforma ou sistema operacional. Os 3 fatores chaves para isso são os arquivos de classe, bytecode e a Máquina Virtual Java.  
Java é uma linguagem compilada e interpretada. Quando um programa é compilado, ele gera um arquivo `.class` que é uma coleção de bytecode. Essa coleção de bytecodes não são instruções de máquina, mas são instruções para a Máquina Virtual Java. Como todos os programas Java são executados na JVM, o mesmo bytecode pode ser executado em qualquer plataforma.  
Porém, a JVM é dependente de plataformas, pois converte o bytecode em instrução de nível de máquina que é específico de cada plataforma.  
O bytecode é criado quando você compila um programa Java utilizando o compilador `javac` e o bytecode é executado pela JVM que é criado utilizando o comando `java`. Quando você usa o comando `java`, ele gera a JVM, carrega as classes Main especificadas no comando e chama o Standard Main Method em Java.  
Em suma: a combinação de bytecode e JVM cria a independência de plataforma da linguagem Java.

**Exemplo**:

Arquivo _HelloWorld.java_:
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Compilar o arquivo _HelloWorld.java_ para _HelloWorld_:
```bash
javac HelloWorld.java
```

Executar _HelloWorld_:
```bash
java HelloWorld
```

## 2 - O que é ClassLoader em Java?

Os carregadores de classe (ClassLoader) na linguagem Java são usados para carregar as classes em tempo de execução. O código em Java é compilado em bytecode e executado pela JVM. Então, o ClassLoader carrega as classes de arquivos `.class` no sistema, network ou qualquer outro caminho especificado. ClassLoader funciona em 3 princípios: Delegação, Visibilidade e Singularidade.  
- O princípio de **Delegação** encaminha a requisição do carregamento da classe para o carregamento da classe pai e só carrega a classe se a classe pai não for capaz de encontrar ou carregar a classe.  
- O princípio da **Visibilidade** permite que o carregador da classe filha veja todas as classes carregadas pelo ClassLoader da classe pai, mas o carregador da classe pai não pode ver todas as classes carregadas pela classe filha.  
- O princípio da **Singularidade** permite uma classe ser carregada apenas uma vez, o que é basicamente atingido pelo princípio da Delegação e garante que o ClassLoader da classe filha não recarregue a classe que já foi carregada por uma classe pai.

Existem 3 ClassLoaders padrão usados na linguagem Java: Bootstrap, Extension e System ou Application class loader.  
Todo ClassLoader tem uma localização pré-definida, de onde eles carregam as classes.  
- O **Bootstrap ClassLoader** é responsável por carregar os arquivos de classe padrão da JDK do `jre/lib/rt.jar` e é o pai de todos os ClassLoaders na linguagem Java. É conhecido como o ClassLoader Primordial da linguagem.  
- **Extension ClassLoader** delega a requisição do carregamento da classe para seu pai, Bootstrap, e se não obtiver sucesso, carrega a classe do diretório `jre/lib/ext` ou qualquer outro diretório apontado por `java.ext.dirs` nas propriedades de sistema. Extension ClassLoader na JVM é implementado por `sun.misc.Launcher$ExtClassLoader`.  
- O terceiro ClassLoader padrão usado pela JVM é chamado de **System ou Application ClassLoader** e é responsável por carregar classes específicas da aplicação da variável de ambiente CLASSPATH, `-classpath` ou `-cp` na linha de comando. Esse ClassLoader é filha do Extension ClassLoader e é implementado pela classe `sun.misc.Launcher$AppClassLoader`. 

## 3 - Escreva um programa em Java para dizer se um número é par ou ímpar
```java
import java.util.Scanner;
public class ParOuImpar{
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        System.out.print("Digite um número: ");
        int num = scan.nextInt();

        if(num % 2 == 0)
            System.out.println(num + " é par");
        else
            System.out.println(num + " é ímpar");
    }
}
```

## 4 - Qual a diferença entre ArrayList e HashSet?

Fundamentalmente, `ArrayList` implementa a interface `List` e `HashSet` implementa a interface `Set`. Um `List` dá suporte para coleções ordenadas e permite itens duplicados, enquanto `Set` dá ênfase à singularidade dos itens sem qualquer ordem especificada.

**Diferenças entre ArrayList e HashSet:**

- A primeira e mais importante diferença é que `ArrayList` implementa a interface `List` e `HashSet` implementa a interface `Set`.
- `ArrayList` permite itens duplicados, enquanto `HashSet` não permite.
- `ArrayList` é uma coleção ordenada e mantém a ordem de inserção dos elementos, enquanto `HashSet` não é ordenada e não mantém a ordem.
- `ArrayList` é baseada em um Array, e `HashSet` é baseado em um `HashMap`.
- Os elementos em um `ArrayList` podem ser acessados pelo índice.

**Similaridades entre ArrayList e HashSet:**

- Ambas as classes são coleções não sincronizadas, e não é recomendado que se use elas em ambientes multi-thread. Você pode fazer `ArrayList` e `HashSet` sincronizados utilizando `Collections.synchronizedCollection()`.
- Ambas as classes podem ser atravessadas utilizando um iterador, especialmente quando se deseja realizar operações nos elementos.
- Os iteradores de ambas as classes são fail-fast. Eles irão lançar a exceção `ConcurrentModificationException` se uma das classes for modificada estruturalmente uma vez que o iterador foi criado.

## 5. O que é Double-checked locking em Singleton?

**Singleton** é um padrão de projeto criacional que garante que apenas um objeto desse tipo exista e forneça apenas um ponto de acesso a ele para qualquer outro código. Ele pode ser reconhecido por um método de criação estático (`instance()` ou `getInstance()`), que retorna a instância do mesmo objeto em cache.

**Double-checked locking** é uma solução para retornar a instância do objeto em um ambiente multi-thread, onde se fazem duas checagens de se a instância do objeto já foi iniciada no mesmo método, uma checagem sincronizada e outra não.

O atributo da instância precisa ser declarado como `static` e `volatile`. `volatile` é uma keyword na linguagem que sinaliza que esse atributo pode ser acessado em threads diferentes ao mesmo tempo, a declaração desse atributo faz com que esse método seja thread-safe pelo princípio de *Happens-before Guarantee* (garantia de que acontece antes). Essa particularidade sinaliza para a JVM que leituras e registros de outras variáveis acontecem depois do registro de uma variável marcada com `volatile`. Além disso, quando uma leitura de um atributo marcado com `volatile` acontece, a barreira de memória é renovada.

O construtor da classe precisa ser declarado como `private` para impedir a criação de uma instância fora da classe. Existe uma convenção de que o nome do atributo desse tipo de padrão é nomeado de `_instance`.

A primeira checagem que acontece não é sincronizada, mas a segunda checagem é sincronizada. Exemplo prático:

```java
public class DCLSingleton {
    private static volatile DCLSingleton _instance = null;

    private DCLSingleton() {}

    public static DCLSingleton instance() {
        // Primeira Checagem (fora da thread)
        if (_instance == null) {
            synchronized (DCLSingleton.class) {
                // Segunda checagem (dentro da thread)
                if (_instance == null) {
                    _instance = new DCLSingleton();
                }
            }
        }
        return _instance;
    }
}
```

## 6. Como criar um Singleton thread-safe?

Um **Singleton thread-safe** é uma classe que utiliza o padrão Singleton (apenas um objeto desse tipo exista e forneça apenas um ponto de acesso a ele para qualquer outro código) e garante que a mesma instância seja retornada mesmo em um ambiente multi-thread.

Uma forma de garantir isso é utilizar o padrão Double-checking locking em Singleton (ver questão 5). No entanto, isso também pode ser feito de outras maneiras:

#### Enum como Singleton

Enums em Java são sempre considerados `final` e possuem segurança contra múltiplas instâncias devido à serialização. Se a sua classe Singleton mantém algum estado e contém métodos para alterar esse estado, é preciso garantir que o código evite problemas com serialização e segurança de threads. Exemplo de código:

```java
public enum Singleton {
    INSTANCE;

    public void show() {
        System.out.println("Singleton utilizando Enums em Java");
    }
}

// Você pode acessar o método Singleton com Singleton.INSTANCE e chamar o método dessa forma:
Singleton.INSTANCE.show();
```
### Utilizando campo static na inicialização  

Outra forma é criar um Singleton thread-safe inicializando a instância da classe Singleton durante o carregamento de classes. Campos `static` são inicializados durante o carregamento da classe, e o ClassLoader garante que a instância não estará completamente visível até que esteja completamente criada. A desvantagem dessa abordagem é que a inicialização não é "lazy" (preguiçosa) e a classe Singleton é inicializada antes de qualquer classe que possa chamá-la por um `getInstance()`. Exemplo de código:

```java
public class Singleton {
    private static final Singleton _instance = new Singleton();

    private Singleton() { }

    public static Singleton getInstance() {
        return _instance;
    }

    public void show() {
        System.out.println("Singleton utilizando inicialização estática em Java");
    }
}

// Uma classe Singleton pode ser acessada dessa forma
Singleton.getInstance().show();
```

## 7. Quando utilizar uma variável `volatile` na linguagem Java?

A keyword `volatile` é utilizada para variáveis que podem ser modificadas em um ambiente multi-thread. Ela ajuda a otimizar o compilador e o tempo de execução, como o JIT (Just-in-Time). Ao declarar uma variável como `volatile`, você assegura que seu valor será sempre acessado da **Main Memory** e não de um cache local da thread. 

#### Principais pontos sobre `volatile`:

- **Visibilidade**: Garante que a variável será visível para todas as threads, evitando que uma thread veja um valor obsoleto armazenado em um cache local. Isso se dá pelo princípio de *happens-before*, que significa que outras threads veem a variável com o último valor alterado antes que qualquer thread modifique esse valor.

- **Consistência**: A utilização de `volatile` impede que o compilador JIT reordene operações que envolvam essa variável, garantindo que a ordem de execução das operações seja mantida.

- **Menor Custo**: Embora `volatile` seja menos custoso do que mecanismos de sincronização mais pesados, como blocos `synchronized`, ela não substitui a necessidade de sincronização para garantir a atomicidade e integridade dos dados.

- **Uso Adequado**: Deve ser usada quando você tem variáveis compartilhadas entre threads e deseja garantir que a última atualização seja visível para todas as threads. No entanto, `volatile` não garante a atomicidade de operações compostas, e outras técnicas de sincronização podem ser necessárias para garantir a consistência completa.

**Exemplo de uso:**

```java
private volatile boolean flag = false;

public void updateFlag() {
    flag = true; // Atualiza a flag
}

public boolean checkFlag() {
    return flag; // Lê a flag
}
```

## 8. Quando usar uma variável `transient`?

Uma variável marcada com `transient` é uma variável cujo valor não é serializado durante o processo de serialização. Durante a deserialização, essas variáveis são inicializadas com seus valores padrão.

#### Principais pontos sobre `transient`:

- **Controle e Flexibilidade**: A keyword `transient` permite ao programador mais controle sobre quais propriedades de um objeto devem ser salvas e quais devem ser excluídas durante a serialização. Isso é útil para omitir dados sensíveis ou desnecessários da serialização.

- **Valores Não Serializados**: Variáveis marcadas como `transient` não são incluídas na representação serializada do objeto. Quando o objeto é desserializado, essas variáveis são inicializadas com seus valores padrão (por exemplo, `null` para referências, `0` para tipos numéricos, `false` para booleanos).

- **Exemplos de Uso**:
  - Variáveis que são calculadas com base em outras variáveis e não precisam ser persistidas.
  - Dados temporários ou derivados que não são essenciais para a recuperação do estado do objeto após a desserialização.

**Exemplo de uso:**

```java
import java.io.Serializable;

public class Exemplo implements Serializable {
    private static final long serialVersionUID = 1L;
    private int id;
    private transient String senha; // Não será serializada

    public Exemplo(int id, String senha) {
        this.id = id;
        this.senha = senha;
    }

    // Getters e Setters
}
```

## 9. Qual a diferença entre as palavras-chave `volatile` e `transient` na linguagem Java?

`volatile` e `transient` são modificadores utilizados em contextos totalmente diferentes na linguagem Java:

- **`volatile`**:
  - **Contexto**: Utilizado em ambientes multi-thread.
  - **Função**: Garante que as leituras e gravações de uma variável sejam realizadas diretamente na memória principal (Main Memory) e não em caches de threads individuais. Isso assegura que todas as threads vejam a versão mais recente da variável e evita inconsistências.
  - **Objetivo**: Facilita a visibilidade e a coerência de dados compartilhados entre múltiplas threads. Utiliza a garantia de Happens-before, que garante que as alterações feitas por uma thread sejam visíveis para outras threads de forma segura e consistente.
  - **Exemplo**:
    ```java
    private volatile boolean flag = false;
    ```

- **`transient`**:
  - **Contexto**: Utilizado durante a serialização e desserialização de objetos.
  - **Função**: Indica que a variável não deve ser serializada. Durante a serialização, a variável marcada como `transient` será ignorada, e durante a desserialização, ela será inicializada com seu valor padrão (por exemplo, `null` para referências).
  - **Objetivo**: Controla quais dados de um objeto devem ser persistidos e quais devem ser omitidos durante o processo de serialização.
  - **Exemplo**:
    ```java
    private transient String senha;
    ```

**Resumo**:
- **`volatile`** é usado para garantir a visibilidade de variáveis em ambientes multi-thread.
- **`transient`** é usado para excluir variáveis da serialização.

## 10. Qual a diferença entre `Serializable` e `Externalizable` na linguagem Java?

A serialização em Java é o processo de converter o estado de um objeto em um formato que pode ser armazenado em um arquivo binário (comumente com a extensão `.ser`) e a deserialização é o processo reverso de recriar o estado do objeto a partir desse arquivo. A interface `Externalizable` estende a interface `Serializable`, mas possui comportamentos diferentes. As principais diferenças são:

- **Interface de Marcador**:
  - **`Serializable`**: É uma interface de marcador (marker interface), ou seja, não possui métodos e é usada apenas para sinalizar ao compilador e à JVM que a classe pode ser serializada.
  - **`Externalizable`**: Contém dois métodos obrigatórios para a serialização: `writeExternal()` e `readExternal()`. O programador deve implementar esses métodos para definir como o objeto será serializado e desserializado.

- **Responsabilidade**:
  - **`Serializable`**: A responsabilidade pela serialização é automaticamente gerenciada pela JVM. O processo padrão cuida da serialização do estado do objeto, incluindo o estado da superclasse.
  - **`Externalizable`**: A responsabilidade é totalmente do programador. O programador deve implementar os métodos `writeExternal()` e `readExternal()` para definir como os dados serão escritos e lidos.

- **Performance**:
  - **`Serializable`**: A performance não pode ser otimizada diretamente através dessa interface. O programador pode reduzir a quantidade de dados serializados usando `transient` e outras técnicas, mas o processo padrão não é modificável.
  - **`Externalizable`**: Oferece controle total sobre o processo de serialização, permitindo ao programador otimizar a performance e personalizar o formato de serialização.

- **Manutenção**:
  - **`Serializable`**: Dependente da implementação padrão da JVM, que pode ser sensível a mudanças na estrutura da classe. Alterações nos campos ou na estrutura podem quebrar a compatibilidade com a serialização.
  - **`Externalizable`**: Permite ao programador definir seu próprio formato de serialização e deserialização, proporcionando maior controle e robustez frente a mudanças na classe.

**Resumo**:
- **`Serializable`** oferece uma implementação padrão e é mais fácil de usar, mas com menos controle e possíveis problemas de manutenção.
- **`Externalizable`** oferece controle total sobre a serialização e pode ser mais eficiente, mas exige implementação manual completa e pode ser mais complexo de manter.

## 11. É possível sobrescrever um método privado?

Não, não é possível sobrescrever um método privado em Java. Aqui estão as razões principais:

- **Acesso a Métodos Privados**: Métodos privados são específicos para a classe onde são definidos e não são acessíveis a partir de subclasses. Isso significa que métodos privados não podem ser sobrescritos porque não são visíveis em uma subclasse.

- **Vinculação em Tempo de Compilação**: Métodos privados são vinculados em tempo de compilação, o que significa que o compilador determina qual método será chamado com base no tipo da variável e não no tipo de objeto em tempo de execução. Isso difere dos métodos protegidos e públicos, que são vinculados dinamicamente em tempo de execução.

- **Métodos Privados e Sobrescrita**: Mesmo que você defina um método com o mesmo nome e assinatura em uma subclasse, isso não é considerado sobrescrição, mas sim uma nova definição de método na subclasse. O método privado da classe base não é acessível ou substituído pelo método na subclasse.

**Resumo**: Métodos privados não podem ser sobrescritos porque são específicos da classe onde são definidos e não são visíveis para subclasses. A sobrescrita é possível apenas para métodos protegidos e públicos, que podem ser acessados e modificados por subclasses.

## 12. Qual a diferença entre HashTable e HashMap na linguagem Java?

Ambas as estruturas de dados, `HashTable` e `HashMap`, são baseadas em Hash e implementam a interface `Map`. No entanto, existem diferenças importantes entre elas:

- **Thread-Safety**: 
  - **HashTable**: É thread-safe e sincronizada, o que significa que é segura para uso em um ambiente multi-thread sem necessidade de sincronização externa.
  - **HashMap**: Não é sincronizada e, portanto, não é thread-safe. Se necessário, pode ser sincronizada externamente utilizando `Collections.synchronizedMap()`.

- **Chaves e Valores Nulos**:
  - **HashTable**: Não permite chaves nulas ou valores nulos. Tentativas de inserir um valor nulo resultarão em uma exceção `NullPointerException`.
  - **HashMap**: Permite uma chave nula e valores nulos, tornando-a mais flexível nesse aspecto.

- **Desempenho**:
  - **HashTable**: Pode ser mais lenta devido à sincronização interna, que adiciona overhead. 
  - **HashMap**: Geralmente é mais rápido porque não tem o overhead de sincronização, mas isso a torna menos adequada para ambientes multi-thread sem sincronização externa.

- **Iteradores**:
  - **HashMap**: Usa `Iterator` que é fail-fast. Isso significa que se a estrutura do mapa for modificada estruturalmente após a criação do iterador, ele lançará uma `ConcurrentModificationException`.
  - **HashTable**: Usa `Enumerator`, que não possui a mesma garantia fail-fast. Embora a exceção `ConcurrentModificationException` possa ser lançada se a estrutura for modificada, isso depende da implementação da JVM e não é garantido.

- **Ordem dos Elementos**:
  - **HashMap**: Não garante que a ordem dos elementos seja consistente. A ordem pode mudar com base em inserções e remoções.
  - **HashTable**: Também não garante a ordem dos elementos, mas o comportamento é similar ao de `HashMap` nesse aspecto.

## 13. Qual a diferença entre List e Set na linguagem Java?

`List` e `Set` são interfaces em Java que representam coleções de objetos, mas possuem características e comportamentos diferentes. Aqui estão as principais diferenças:

- **Ordem dos Elementos**:
  - **List**: É uma coleção ordenada onde a ordem dos elementos é mantida. Os elementos são armazenados na sequência em que são inseridos e podem ser acessados por índice.
  - **Set**: É uma coleção sem uma ordem específica dos elementos. A ordem de iteração não é garantida, e a ordem dos elementos pode não ser a mesma em que foram inseridos.

- **Duplicatas**:
  - **List**: Permite elementos duplicados. Você pode adicionar o mesmo elemento várias vezes.
  - **Set**: Não permite elementos duplicados. Adicionar um elemento que já existe no `Set` substituirá o valor antigo.

- **Indexação**:
  - **List**: Os elementos podem ser acessados por índice (por exemplo, `list.get(index)`).
  - **Set**: Não permite acesso por índice. A recuperação de elementos é feita por meio de iteração ou busca direta.

- **Implementações Populares**:
  - **List**: `ArrayList`, `LinkedList`, `Vector`.
  - **Set**: `HashSet`, `TreeSet`, `LinkedHashSet`.

- **Performance**:
  - **List**: Pode sofrer queda de desempenho à medida que a estrutura cresce, especialmente se operações de inserção e remoção forem frequentes.
  - **Set**: Geralmente tem desempenho constante para operações básicas (como inserção e busca) se implementado com `HashSet`, mas pode ser mais lento para `TreeSet` devido à ordenação.

- **Uso de Memória**:
  - **List**: Pode usar mais memória devido ao armazenamento dos elementos e seus índices.
  - **Set**: Pode usar menos memória, especialmente se os elementos são armazenados de forma compacta (por exemplo, em um `HashSet`).

- **Iteração**:
  - **List**: A ordem de iteração pode ser prevista e corresponde à ordem de inserção.
  - **Set**: A ordem de iteração não é prevista e pode variar.

- **Ordenação**:
  - **List**: Suporta ordenação de elementos, especialmente com implementações como `ArrayList` e `LinkedList`.
  - **Set**: `SortedSet` pode manter a ordem dos elementos com base em comparadores ou ordem natural, mas `HashSet` não possui mecanismos embutidos de ordenação.

- **Recuperação**:
  - **List**: A recuperação de elementos específicos é geralmente mais rápida usando índices.
  - **Set**: A recuperação de elementos específicos pode ser mais lenta, especialmente se a busca não for direta.

- **Casos de Uso**:
  - **List**: Use quando a ordem de inserção dos elementos é importante ou quando é necessário permitir duplicatas.
  - **Set**: Use quando a unicidade dos elementos é crucial e a ordem não é relevante.

### Quando usar `List` ou `Set` na linguagem Java?  
Escolha `List` quando você precisa manter a ordem de inserção ou permitir elementos duplicados. Escolha `Set` quando você precisa garantir a unicidade dos elementos e não se importa com a ordem.

## 14. Qual a diferença entre ArrayList e Vector na linguagem Java?

Ambas as coleções derivam de `AbstractList` e implementam a interface `List`. Isso significa que ambas são coleções ordenadas e permitem elementos duplicados. Outra similaridade é que ambas são coleções baseadas em índice e é possível usar o método `get(index)` para recuperar objetos de `Vector` e `ArrayList`. Tendo em vista as similaridades, suas diferenças são:

**Sincronização (Synchronization)**
 - A primeira diferença é que `Vector` é uma coleção sincronizada e thread-safe, enquanto `ArrayList` não é. Se for necessário acessar um `Vector` em um ambiente multi-thread, é possível fazê-lo sem comprometer a integridade dos dados.

**Velocidade**
 - A segunda diferença é a velocidade, que está diretamente relacionada com a sincronização. `Vector`, sendo sincronizado, se torna muito mais lento que o `ArrayList`.

**Classe Legado**
 - `Vector` é uma classe legado e inicialmente não fazia parte do Framework de Coleções da linguagem Java. A partir do Java 1.4, `Vector` passou a implementar a interface `List` e se tornou parte do framework de coleções.

 ## 15. Qual a diferença entre HashTable e ConcurrentHashMap na linguagem Java?

`HashTable` e `ConcurrentHashMap` são duas implementações de mapas em Java, projetadas para serem thread-safe, o que significa que podem ser usadas de forma segura em ambientes concorrentes, onde várias threads estão acessando e modificando os dados ao mesmo tempo. No entanto, existem algumas diferenças importantes entre elas:

### Thread-safety
- **HashTable**: É thread-safe por padrão. Todas as operações no `HashTable` são sincronizadas, o que significa que apenas uma thread pode acessar o `HashTable` em um determinado momento. Isso pode levar a problemas de desempenho em cenários concorrentes de alto tráfego, já que apenas uma thread pode acessar o `HashTable` de cada vez.
- **ConcurrentHashMap**: É projetado para ter um alto desempenho em cenários concorrentes. Ele divide o mapa em várias seções independentes e bloqueia apenas a seção que está sendo modificada, permitindo que várias operações de leitura e gravação ocorram simultaneamente.

### Iteração segura
- **HashTable**: A iteração sobre um `HashTable` não requer sincronização externa, pois é thread-safe. No entanto, se outras threads estiverem modificando o `HashTable` ao mesmo tempo, pode ocorrer uma `ConcurrentModificationException`.
- **ConcurrentHashMap**: Permite iteração segura, mesmo quando outras threads estão modificando o mapa.

### Desempenho
- **HashTable**: Devido à sincronização pesada, o desempenho pode ser comprometido em cenários concorrentes de alto tráfego.
- **ConcurrentHashMap**: É otimizado para desempenho em cenários concorrentes, e cada thread pode trabalhar de forma independente em diferentes partes do mapa, melhorando o throughput em ambientes concorrentes.

### Null values/keys
- **HashTable**: Não permite chaves ou valores nulos.
- **ConcurrentHashMap**: Permite chaves e valores nulos.

**Resumo**: `ConcurrentHashMap` é geralmente preferido em ambientes concorrentes de alto tráfego devido ao seu melhor desempenho e maior flexibilidade em comparação com `HashTable`. No entanto, em algumas situações onde a simplicidade e a segurança de thread são mais importantes, `HashTable` pode ser usado.

## 16. Como ConcurrentHashMap atinge sua escalabilidade?

`ConcurrentHashMap` atinge sua escalabilidade principalmente através de duas técnicas principais: particionamento e bloqueio granular.

### Particionamento
`ConcurrentHashMap` divide internamente seu espaço de armazenamento em várias seções, chamadas de segmentos ou "buckets". Cada segmento é essencialmente um mapa separado, com seu próprio conjunto de entradas. Isso significa que as operações em um segmento não bloqueiam as operações em outros segmentos, permitindo que várias threads realizem operações simultâneas em segmentos diferentes.
- O número de segmentos é fixo durante a criação do `ConcurrentHashMap`. O tamanho padrão é 16, mas você pode especificar um tamanho diferente durante a inicialização.
- Quando uma operação é realizada em um `ConcurrentHashMap`, a chave é usada para determinar em qual segmento a entrada deve ser armazenada ou recuperada. Isso minimiza a contenção de thread, pois diferentes threads podem trabalhar em segmentos diferentes sem bloquear umas às outras.

### Bloqueio granular
Dentro de cada segmento, `ConcurrentHashMap` utiliza técnicas de bloqueio granular para minimizar a contenção de thread.
- Em vez de bloquear todo o mapa durante operações de leitura ou gravação, `ConcurrentHashMap` apenas bloqueia o segmento relevante. Isso significa que, enquanto uma thread está escrevendo em um segmento, outras threads ainda podem ler ou escrever em outros segmentos, melhorando o throughput e a escalabilidade geral do `ConcurrentHashMap`.
- Isso também permite a iteração segura sobre o mapa, mesmo quando outras operações de escrita estão ocorrendo em paralelo.

**Resumo**: `ConcurrentHashMap` atinge escalabilidade por meio da combinação de particionamento e bloqueio granular. Ao dividir o mapa em segmentos e permitir operações simultâneas em segmentos diferentes, ele reduz a contenção de thread e melhora o desempenho em ambientes concorrentes. Essas técnicas tornam o `ConcurrentHashMap` uma escolha popular para aplicativos que requerem alta concorrência e escalabilidade.

## 17. Quais métodos o programador precisa sobrescrever para um objeto ser usado como chave (Key) em um HashMap?

Um `HashMap` em Java é uma estrutura de dados que armazena pares chave-valor, onde cada chave é única. Internamente, um `HashMap` utiliza um array de "buckets" (compartimentos) para armazenar esses pares chave-valor. O índice de cada bucket é calculado usando o método `hashCode()` da chave correspondente.

Quando um par chave-valor é inserido em um `HashMap`, o código hash da chave é calculado e usado para determinar o índice do bucket onde o par deve ser armazenado. Se houver colisões (ou seja, duas chaves têm o mesmo código hash e, portanto, o mesmo índice de bucket), os pares são armazenados em uma estrutura de dados de lista encadeada dentro do bucket. Aqui está como a implementação dos métodos `hashCode()` e `equals()` se relaciona com o funcionamento interno de um `HashMap`:

### `hashCode()`
- O método `hashCode()` é usado para calcular um código hash para a chave. O código hash é usado como um índice para determinar onde o par chave-valor será armazenado no array de buckets. 
- Um bom algoritmo de `hashCode()` distribui os códigos hash de forma uniforme por todo o intervalo de índices do array de buckets, reduzindo assim o número de colisões.

### `equals()`
- O método `equals()` é usado para comparar se duas chaves são consideradas iguais. Durante a busca por uma chave em um `HashMap`, o método `equals()` é usado para verificar se a chave fornecida é igual à chave armazenada no bucket correspondente.
- Se duas chaves tiverem o mesmo código hash, o método `equals()` será chamado para verificar se são iguais. Portanto, é importante que a implementação do método `equals()` seja consistente com a implementação do método `hashCode()`.

**Resumo**: A implementação correta dos métodos `hashCode()` e `equals()` é essencial para o bom funcionamento de um `HashMap`. Eles garantem que as chaves sejam armazenadas e recuperadas corretamente, sem colisões indevidas, e que o `HashMap` possa realizar operações de inserção, recuperação e remoção de forma eficiente e consistente.

## 18. Qual a diferença entre `wait()` e `sleep()` na linguagem Java?

Em Java, as diferenças entre os métodos `wait()` e `sleep()` estão relacionadas às classes em que estão definidos e aos cenários em que são utilizados:

### `wait()`
- **Classe**: O método `wait()` é um método da classe `Object`.
- **Uso**: É usado para colocar uma thread em espera até que outra thread notifique a primeira thread sobre uma mudança de estado ou condição. O método `wait()` deve ser chamado dentro de um bloco sincronizado usando a palavra-chave `synchronized`.
- **Comportamento**: Quando uma thread chama `wait()`, ela libera o monitor (bloqueio) do objeto em que está aguardando e entra em estado de espera. Ela permanece em espera até que outra thread chame `notify()` ou `notifyAll()` no mesmo objeto. O método `wait()` é útil para sincronização de threads e comunicação entre elas.

### `sleep()`
- **Classe**: O método `sleep()` é um método estático da classe `Thread`.
- **Uso**: É usado para suspender a execução da thread atual por um período especificado de tempo. O método `sleep()` não libera o monitor (bloqueio) do objeto.
- **Comportamento**: É comumente utilizado para introduzir atrasos ou pausas controladas no código, mas não tem relação com a sincronização de threads. É importante notar que a precisão do tempo de suspensão pode variar dependendo do sistema operacional e da implementação da máquina virtual Java.

**Resumo**: Enquanto o método `wait()` é usado para sincronização e comunicação entre threads, o método `sleep()` é usado principalmente para introduzir atrasos ou pausas controladas no código. Cada um tem seu próprio propósito e aplicação específica em programação concorrente.

## 19. Qual a diferença entre notify e notifyAll na linguagem Java?

Em Java, `notify()` e `notifyAll()` são métodos da classe `Object` que são usados em conjunto com o método `wait()` para realizar comunicação entre threads em um ambiente multithreaded. Aqui está a diferença entre eles:

### `notify()`
O método `notify()` é usado para notificar uma única thread que está esperando (em estado de espera) em um objeto específico. Quando `notify()` é chamado, o sistema de execução Java escolhe aleatoriamente uma das threads em estado de espera para ser notificada e a move para o estado de pronto. A escolha da thread notificada é não determinística e depende da implementação do JVM. Portanto, não há garantia sobre qual thread será notificada.

### `notifyAll()`
O método `notifyAll()` é usado para notificar todas as threads que estão esperando (em estado de espera) em um objeto específico. Quando `notifyAll()` é chamado, todas as threads em estado de espera no objeto são notificadas e movidas para o estado de pronto. O uso de `notifyAll()` garante que todas as threads esperando tenham a oportunidade de competir pelo acesso ao objeto após a notificação.

### Resumo
- `notify()` notifica uma única thread em espera.
- `notifyAll()` notifica todas as threads em espera.

A escolha entre `notify()` e `notifyAll()` depende dos requisitos específicos do programa. Em geral, é preferível usar `notifyAll()` para evitar problemas de starvation (inanição) e garantir uma distribuição mais justa de recursos entre as threads.

## 20. Por que o programador sobrescreve os métodos equals() e hashCode() na linguagem Java?

É necessário sobrescrever os métodos `equals()` e `hashCode()` em Java por alguns motivos importantes:

### Comparação de Objetos
O método `equals()` é usado para comparar se dois objetos são semanticamente iguais, ou seja, possuem os mesmos valores ou significados. Ao sobrescrever o método `equals()`, o programador pode definir o critério de igualdade de acordo com as necessidades da aplicação.

### Utilização em Estruturas de Dados
Muitas estruturas de dados em Java, como `HashSet`, `HashMap`, e `Hashtable`, dependem da implementação correta dos métodos `equals()` e `hashCode()` para funcionar corretamente. Por exemplo, essas estruturas de dados usam o método `hashCode()` para calcular um índice onde os objetos são armazenados e o método `equals()` para comparar objetos dentro das estruturas para determinar se eles são iguais.

### Contrato entre equals() e hashCode()
Existe um contrato entre os métodos `equals()` e `hashCode()`. Se dois objetos são considerados iguais (retornam `true` quando comparados com `equals()`), eles devem ter o mesmo código hash (retornam o mesmo valor quando `hashCode()` é chamado). Portanto, ao sobrescrever `equals()`, é uma boa prática também sobrescrever `hashCode()` para garantir que o contrato seja mantido.

### Consistência com equals() e hashCode()
Se dois objetos são considerados iguais de acordo com o método `equals()`, então eles devem ter o mesmo código hash. Isso garante que objetos iguais sejam colocados no mesmo bucket em estruturas de dados hash-based, como `HashMap`.

Portanto, sobrescrever os métodos `equals()` e `hashCode()` é fundamental para garantir o comportamento correto de objetos em estruturas de dados e para manter a consistência e a semântica adequada em relação à igualdade de objetos em uma aplicação Java.

## 21. O que o fator de carga de um HashMap significa?

O fator de carga (`load factor`) de um `HashMap` em Java é um parâmetro que determina quando a tabela hash subjacente deve ser redimensionada. Em termos simples, o fator de carga indica a proporção de slots ocupados em relação ao tamanho total da tabela hash.

O fator de carga é definido como:

$$
\text{Fator de carga} = \frac{\text{Número de elementos armazenados}}{\text{Tamanho da tabela Hash}}
$$

Quando os elementos são inseridos em um `HashMap`, ele aumenta dinamicamente o tamanho da tabela hash conforme necessário. Quando o fator de carga atinge um certo limite, geralmente definido como 0,75 (o valor padrão), o `HashMap` redimensiona sua tabela hash, aumentando sua capacidade.

O redimensionamento da tabela hash é um processo computacionalmente custoso, pois envolve a reorganização de todos os elementos existentes em novos buckets. Portanto, manter um fator de carga moderado pode ajudar a minimizar o número de colisões e o tempo de busca.

Ao ajustar o fator de carga, os desenvolvedores podem equilibrar o consumo de memória e o desempenho da aplicação. Um fator de carga baixo significa menos colisões, mas potencialmente mais consumo de memória, enquanto um fator de carga alto pode resultar em mais colisões e, portanto, em uma performance ligeiramente reduzida.

Em geral, o valor padrão de 0,75 para o fator de carga do `HashMap` funciona bem para a maioria dos casos de uso. No entanto, em situações específicas, pode ser necessário ajustar o fator de carga para otimizar o desempenho e o consumo de memória da aplicação.

## 22. Qual a diferença entre ArrayList e LinkedList na linguagem Java?

Em Java, `ArrayList` e `LinkedList` são duas implementações diferentes da interface `List`, cada uma com suas próprias características e uso ideal. Aqui estão as principais diferenças entre `ArrayList` e `LinkedList`:

### Estrutura Interna
- **ArrayList**: Implementado como um array redimensionável. Isso significa que o `ArrayList` aloca um array de elementos e aumenta sua capacidade conforme necessário à medida que mais elementos são adicionados.
- **LinkedList**: Implementado como uma lista duplamente encadeada. Cada elemento em uma `LinkedList` contém uma referência para o próximo elemento e para o elemento anterior.

### Acesso aos Elementos
- **ArrayList**: Acesso direto (\(O(1)\)) aos elementos através de índices. Isso permite acesso rápido a qualquer elemento na lista.
- **LinkedList**: Acesso sequencial aos elementos. Embora a `LinkedList` forneça acesso rápido à cabeça e à cauda da lista, o acesso a elementos arbitrários pode ser mais lento (\(O(n)\)), pois requer a travessia da lista a partir do início ou do fim.

### Inserção e Remoção de Elementos
- **ArrayList**: Inserção e remoção no final da lista são rápidas (\(O(1)\)), mas a inserção e remoção no meio da lista podem ser lentas (\(O(n)\)) devido à necessidade de realocação de elementos.
- **LinkedList**: Inserção e remoção em qualquer posição da lista são relativamente rápidas (\(O(1)\)), pois envolvem apenas ajustes de ponteiros.

### Uso de Memória
- **ArrayList**: Usa menos memória do que `LinkedList` para armazenar os mesmos elementos, pois não requer overhead adicional para armazenar referências para elementos anteriores e seguintes.
- **LinkedList**: Usa mais memória do que `ArrayList` devido à sobrecarga de armazenar referências para elementos anteriores e seguintes.

### Iteração e Manipulação
- **ArrayList**: Melhor desempenho em iterações simples e manipulação de elementos.
- **LinkedList**: Melhor desempenho em operações que envolvem muitas inserções e remoções de elementos no meio da lista.

**Resumo**: Se você precisar de acesso rápido aos elementos por índice e realizar muitas operações de inserção e remoção no final da lista, o `ArrayList` é mais adequado. Por outro lado, se você precisar de inserções e remoções frequentes no meio da lista e não se importar com o acesso lento aos elementos arbitrários, a `LinkedList` pode ser mais apropriada.
