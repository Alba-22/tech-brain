> Make fine grained interfaces that are client specific.

Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar, ou seja, é melhor criar interfaces mais específicas do que ter uma única interface genérica.

### Mau exemplo:
```php
interface Aves {
    public function setLocalizacao($longitude, $latitude);
    public function setAltitude($altitude);
    public function renderizar();
}

class Papagaio implements Aves {
    public function setLocalizacao($longitude, $latitude) { }
    public function setAltitude($altitude) { }
    public function renderizar() { }
}

class Pinguim implements Aves {
    public function setLocalizacao($longitude, $latitude) { }
    // A Interface Aves está forçando a Classe Pinguim a implementar esse método.
    // Isso viola o príncipio ISP
    public function setAltitude($altitude) {
        //Não faz nada...  Pinguins são aves que não voam!
    } 
    public function renderizar() { }
}
```
### Bom exemplo:
```php
interface Aves {
    public function setLocalizacao($longitude, $latitude);
    public function renderizar();
}

interface AvesQueVoam extends Aves {
    public function setAltitude($altitude);
}

class Papagaio implements AvesQueVoam {
    public function setLocalizacao($longitude, $latitude) { }
    public function setAltitude($altitude) { }
    public function renderizar() { }
}

class Pinguim implements Aves {
    public function setLocalizacao($longitude, $latitude) { }
    public function renderizar() { }
}
```

### Exemplo Ilustrado:
![[Pasted image 20230105220425.png]]