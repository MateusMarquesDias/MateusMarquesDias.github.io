# Prototype

## Definição

Em padrões de projeto na programação orientada a objetos, o termo "prototype" refere-se a um padrão que se concentra na criação de objetos por meio da clonagem de um objeto existente, conhecido como "protótipo". 

## Intenção

O padrão Prototype tem como objetivo criar novos objetos a partir de objetos existentes, conhecidos como protótipos, em vez de criar novos objetos do zero. Isso é útil quando a criação de um objeto é custosa em termos de desempenho ou quando os objetos têm uma estrutura complexa.

O padrão Prototype permite que um objeto seja clonado para criar um novo objeto. O objeto original, chamado de protótipo, serve como modelo para a criação de cópias. A clonagem pode ser superficial (apenas copiando as propriedades básicas) ou profunda (copiando todas as propriedades e objetos internos).

## Motivação

Em um sistema de modelagem 3D (exemplo), a criação de novos objetos tridimensionais a partir do zero é demorado devido a sua complexidade. Suponhamos que temos que criar vários objetos (Círculos, Triângulos, etc.) e cada objeto distinto venha levar 30 minutos para ser criado, depois de criado, precisaremos criar mais 10 objetos do mesmo tipo por uma necessidade especifica, isso levaria muito tempo, certo? Então, como solucionar esse problema?
Podemos solucionar esse problema copiando a parti de um objeto existente, outros objetos do mesmo tipo com o padrão chamado Prototype.

![Prototype](SoWkIImgAStDuShCAqajIajCJbM0i_oJib9ByerTkBWmX1Ii5BGLd7Foyr8LD1GqkO9IInBpqajpyj74dJEBaZ550qiJKueIKz25GueoiHh2nUMSavaAT862hXscux2upQP6LrS1xYHS2a1kmoCDTIHEURXhkHnIyrA08GW0.png)

## Aplicabilidade

Use o prototype, quando: <br/> 
<br/> 
Em situações em que você deseja criar cópias de objetos existentes de maneira eficiente e personalizá-las conforme necessário. Aqui estão algumas situações específicas em que o padrão Prototype é útil em Java:

1.	Criação de objetos complexos
2.	Variações de objetos
3.	Preservação de estados
4.	Reciclagem de objetos
5.	Evitar hierarquias de subclasses

É importante lembrar que o uso do padrão Prototype em Java é mais eficaz quando você deseja criar cópias de objetos existentes, especialmente quando a criação desses objetos é um processo caro.

## Estrutura - Implementação Básica
![Prototype](TL7D2i8m3BxdANBS61teKKG6FNaJV8BQPL2ehRJPGV3XhQ0MQRobtmzV-cNAR1AlLm_7m9GMOmdTPpgbo97lDmBjJSwelQF2GIl07Gw5Ze6mwykZEa77O1FnZrRXuiZF6v4Si4MxxzZ_yuZXv_LYNwiBuLkH7B2YKhZiaSFVTQ4wI9KA9UuVuzX-7YfIXKgrYm9C5EPoooS0.png)
    
## Estrutura - Implementação do Registo do Prototype
![Prototype](ZPB1IiKm44Nt-OfPliSZFr14Absvq-9EN8HcB84qaMIoAFZnGekP44rqrtjpoEbCEqPIWT9cXN64uLqj24_1awVz0yLYLOoSPrnDfB0BaIiOu0QJzGxX0bSO6AwO5UcHS8ijZ6-70IIOWtrz-nEzmOFM_x-vyo8pad9hilx0E5qoScMcFJSpVTraml8D7S-LYiyR8YPwckUaT53.png)

É uma forma fácil de acessar protótipos que você usa de forma frequente, ele salva um conjunto de protótipos pré construído que estavam prontos para serem copiado.

## Participantes

* Prototype (Protótipo): Define a interface para clonar objetos e geralmente inclui um método "clone".
* ConcretePrototype (Protótipo Concreto): Implementa a interface Prototype e fornece a implementação específica para clonar a si mesmo.
* Client (Cliente): Cria novos objetos clonando um protótipo existente, em vez de criar novos objetos do zero.

## Colaborações

*	O padrão Prototype é um padrão de design de software que faz parte do grupo de padrões de criação. Ele é usado para criar novos objetos duplicando um objeto existente, conhecido como protótipo, em vez de criar um novo objeto do zero. Isso pode ser útil quando a criação de um objeto é mais custosa em termos de desempenho ou recursos do que a clonagem de um objeto existente.

*	A colaboração básica envolve o cliente solicitando à fábrica de protótipos um novo objeto, especificando qual protótipo deseja. A fábrica de protótipos, por sua vez, cria uma cópia do protótipo solicitado e a retorna ao cliente. O cliente pode então usar o objeto clonado conforme necessário.

## Consequências

### __Vantagens__

*	Você pode clonar objetos sem acoplá-los a suas classes concretas.
*	Você pode se livrar de códigos de inicialização repetidos em troca de clonar protótipos pré-construídos.
*	Você pode produzir objetos complexos mais convenientemente.
*	Você tem uma alternativa para herança quando lidar com configurações pré determinadas para objetos complexos.

### __Desvantagem__

*	Clonar objetos complexos que têm referências circulares pode ser bem complicado.


_Exemplo de referências circulares:_

* __Dificuldade na criação de clones:__<br/>
O padrão Prototype pressupõe que você pode criar cópias profundas dos objetos existentes, o que significa criar novos objetos que são independentes do objeto original. No entanto, devido à dependência circular entre `Departamento` e `Funcionario`, criar cópias profundas dessas classes pode ser complexo. Se você tentar criar um clone de um `Departamento`, ele também terá que criar clones de todos os `Funcionario` associados a ele, e vice-versa. Isso pode levar a um código complicado e propenso a erros.

```java

class Departamento {
private String nome;
    private List<Funcionario> funcionarios;
    public Departamento(String nome) {
        this.nome = nome;
        this.funcionarios = new ArrayList<>();
    }
    public void adicionarFuncionario(Funcionario funcionario) {
        funcionarios.add(funcionario);
    }
}

```
* __Possível recursão infinita:__ <br/>
Se não for tratado corretamente, a dependência circular pode levar a uma recursão infinita durante o processo de clonagem. Por exemplo, ao tentar clonar um `Departamento`, ele pode tentar clonar todos os `Funcionario` associados, que por sua vez tentarão clonar o `Departamento` novamente, e assim por diante.

```java

class Funcionario {
    private String nome;
    private Departamento departamento;
    public Funcionario(String nome, Departamento departamento) {
        this.nome = nome;
        this.departamento = departamento;
    }
}

```
* __Dificuldade na manutenção:__ <br/>
A dependência circular torna o código mais difícil de entender e manter. Qualquer alteração em uma classe pode afetar a outra, e você precisa ter cuidado para garantir que a clonagem seja feita corretamente e que não haja efeitos colaterais indesejados.

## Implementações

A implementação do padrão Prototype envolve a criação de uma classe que atua como protótipo e permite a clonagem de objetos com base nesse protótipo. O Java fornece uma interface `Cloneable` e o método `clone()` para facilitar a criação de objetos clonáveis. 

*	Criamos uma interface Prototype marcada com a interface Cloneable.
*	Implementamos a classe ConcretePrototype, que implementa a interface Prototype e fornece uma implementação do método `clone()`. Este método cria uma cópia profunda do objeto, criando uma nova instância com os mesmos valores dos atributos.
*	No main(), criamos um objeto protótipo e, em seguida, clonamos esse protótipo para criar uma cópia. Modificamos a cópia e verificamos que as modificações não afetam o objeto original.

O padrão Prototype permite criar cópias de objetos de maneira flexível e eficiente, evitando a necessidade de criar novas instâncias a partir de classes concretas. Isso é especialmente útil quando a configuração de objetos é complexa ou quando você deseja evitar a duplicação de código de inicialização.

## Exemplo de Código - Criando Carro - Com Cloneable

```java

class Carro implements Cloneable {
    private String marca;
    private String modelo;

    public Carro(String marca, String modelo) {
        this.marca = marca;
        this.modelo = modelo;
    }

    public void exibirInformacoes() {
        System.out.println("Marca: " + marca);
        System.out.println("Modelo: " + modelo);
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class ExemploCloneCarro {
    public static void main(String[] args) {

        Carro original = new Carro("Toyota", "Corolla");

        try {
            Carro clone = (Carro) original.clone();

            clone.marca = "Honda";

            System.out.println("Carro Original:");
            original.exibirInformacoes();

            System.out.println("\nCarro Clone:");
            clone.exibirInformacoes();

        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}

```

## Exemplo de Código - Criando Carro - Class Abstract

```java

abstract class CarroPrototype {
    private String marca;
    private String modelo;

    public CarroPrototype(String marca, String modelo) {
        this.marca = marca;
        this.modelo = modelo;
    }

    public abstract CarroPrototype clone();

    public void exibirInformacoes() {
        System.out.println("Marca: " + marca);
        System.out.println("Modelo: " + modelo);
    }
}

class Carro extends CarroPrototype {
    public Carro(String marca, String modelo) {
        super(marca, modelo);
    }

    @Override
    public CarroPrototype clone() {
        return new Carro(this.getMarca(), this.getModelo());
    }
}

public class ExemploCloneCarro {
    public static void main(String[] args) {
       
        CarroPrototype original = new Carro("Toyota", "Corolla");

        CarroPrototype clone = original.clone();

        ((Carro) clone).setMarca("Honda");

        System.out.println("Carro Original:");
        original.exibirInformacoes();

        System.out.println("\nCarro Clone:");
        clone.exibirInformacoes();
    }
}

```
## Exemplo de Código - Criando Carro

```java

class Carro {
    private String marca;
    private String modelo;

    public Carro(String marca, String modelo) {
        this.marca = marca;
        this.modelo = modelo;
    }

    public Carro clonar() {
        return new Carro(this.marca, this.modelo);
    }

    public void setMarca(String marca) {
        this.marca = marca;
    }

    public void exibirInformacoes() {
        System.out.println("Marca: " + marca);
        System.out.println("Modelo: " + modelo);
    }
}

public class ExemploCloneCarro {
    public static void main(String[] args) {
       
        Carro original = new Carro("Toyota", "Corolla");

        Carro clone = original.clonar();

        clone.setMarca("Honda");

        System.out.println("Carro Original:");
        original.exibirInformacoes();

        System.out.println("\nCarro Clone:");
        clone.exibirInformacoes();
    }
}

```

## Usos conhecidos

Talvez o primeiro exemplo do padrão Prototype se encontre no sistema Sketchpad de Ivan Sutherland [Sut63]. A primeira aplicação amplamente conhecida do padrão numa linguagem orientada a objeto foi em ThingLab, na qual os usuários poderiam formar um objeto composto e então promovê-lo a um protótipo pela sua instalação numa biblioteca de objetos reutilizáveis [Bor81]. 

Goldberg e Robson mencionam protótipos como um padrão [GR83], mas Coplien [Cop92] fornece uma descrição muito mais completa. Ele descreve idiomas relacionados ao padrão prototype para C++ e dá muitos exemplos e variações. O Etgdb é um depurador (debugger) de front-end, baseado em ET++, que fornece uma interface de apontar e clicar para diferentes depuradores orientados a linhas. Cada depurador tem uma subclasse DebuggerAdaptor correspondente. Por exemplo, GdbAdaptor adapta o etgdb à sintaxe dos comandos do gdb de GNU, enquanto que SunDbxAdaptor adapta o etgdb ao depurador da Sun. 

O Etgdb não tem um conjunto de classes DebuggerAdaptor codificadas rigidamente nele próprio. Em vez disso, lê o nome do adaptor a ser usado de uma variável fornecida pelo ambiente, procura um protótipo com o nome especificado em uma tabela global e, então, clona o protótipo. 
Novos depuradores podem ser acrescentados ao etgdb ligando-o ao DebuggerAdaptor que funciona para um depurador novo. 

A “biblioteca de técnicas de interações”, no ModeComposer, armazena protótipos de objetos que suportam várias técnicas de interação [Sha90]. Qualquer técnica de interação criada pelo Mode Composer pode ser usada como um protótipo colocando-a nesta biblioteca. O padrão Prototype permite ao Mode Composer suportar um conjunto ilimitado de técnicas de interação. O exemplo do editor musical discutido anteriormente se baseia no framework para desenhos do Unidraw [VL90].

## Padrões Relacionados

Prototype e Abstract Factory (95) são padrões que competem entre si em várias situações, como discutimos no final deste capítulo. Porém, eles também podem ser usados em conjunto. Um Abstract Factory pode armazenar um conjunto de protótipos a partir dos quais podem ser clonados e retornados objetos-produto.

## Referencias

Gamma Erich - Padrões de Projetos - Soluções Reutilizáveis <br/>
Mergulho nos Padrões de Projeto
