> You should be able to extend a classes behavior, without modifying it.

Objetos ou entidades devem estar abertos para extensão mas fechados pra modificação, ou seja, quando novos comportamentos e recursos precisam ser adicionados no software, devemos estender e não alterar o código fonte original.

### Como adicionar um novo comportamento sem alterar o código já existente:

> Separate extensible behavior behind an interface, and flip the dependencies.

O que devemos fazer é concentrar nos aspectos essenciais do contexto, abstraindo-os para uma interface. Se as abstrações são bem definidas, logo o software estará aberto para extensão.

### Mau Exemplo:
```php
class ContratoClt {
    public function salario() {}
}

class Estagio {
    public function bolsaAuxilio() {}
}

class FolhaDePagamento {
    protected $saldo;
    
    public function calcular($funcionario) {
        if ( $funcionario instanceof ContratoClt ) {
            $this->saldo = $funcionario->salario();
        }
				else if ( $funcionario instanceof Estagio) {
            $this->saldo = $funcionario->bolsaAuxilio();
        }
    }
}
```
### Bom Exemplo:
```php
interface Remuneravel {
    public function remuneracao();
}

class ContratoClt implements Remuneravel {
    public function remuneracao() {}
}

class Estagio implements Remuneravel {
    public function remuneracao() {}
}

class FolhaDePagamento {
    protected $saldo;
    public function calcular(Remuneravel $funcionario) {
        $this->saldo = $funcionario->remuneracao();
    }
}
```

### Exemplo Ilustrado:
![[Pasted image 20230105215924.png]]
