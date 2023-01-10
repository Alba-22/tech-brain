> Derived classes must be substitutable for their base classes.

Uma classe derivada deve ser substituível por sua classe base. Foi introduzido por Barbara Liskov, em 1987.

> Se S é um subtipo de T, então os objetos do tipo T, em um programa, podem ser substituídos pelos objetos de tipo S sem que seja necessário alterar as propriedades deste programa.

### Exemplos de Violação do LSP:

-   Sobrescrevendo ou implementando um método que não faz nada;
-   Lançar uma exceção inesperada;
-   Retornar valores de tipos diferentes da classe base;

Para não violar, será necessário usar Injeção de Dependência, além de outros princípios do SOLID.

Permite usar o polimorfismo com maior confiança, usando as classes derivadas referindo-se a sua classe base sem se preocupar com resultados inesperados.

### Exemplo:
```php
class A {
    public function getNome() {
        echo 'Meu nome é A';
    }
}

class B extends A { 
    public function getNome() {
        echo 'Meu nome é B';
    }
}

$objeto1 = new A;
$objeto2 = new B;

function imprimeNome(A $objeto) {
    return $objeto->getNome();
}

imprimeNome($objeto1); // Meu nome é A
imprimeNome($objeto2); // Meu nome é B
```
Estamos passando como parâmetro tanto a classe pai como a classe derivada e o código continua funcionando da forma esperada.

### Exemplo Ilustrado:
![[Pasted image 20230105220152.png]]