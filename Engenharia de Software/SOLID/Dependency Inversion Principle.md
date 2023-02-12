#solid
> Depend on abstractions, not on concretions.

Devemos depender de abstrações, e não de implementações

> Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender da abstração.

> Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.

### Mau Exemplo:
```php
use MySQLConnection;

class PasswordReminder {
    private $dbConnection;
    
    public function __construct() {       
        $this->dbConnection = new MySQLConnection();           
    }
}
```
Há alto nível de acoplamento, pois a classe PasswordReminder tem a responsabilidade de criar uma instância da classe MySQLConnection.
### Exemplo Melhorado:
```php
use MySQLConnection;

class PasswordReminder {
    private $dbConnection;
    
    public function __construct(MySQLConnection $dbConnection) {       
        $this->dbConnection = $dbConnection;           
    }
}
```
A classe de conexão com o banco de dados virou uma dependência que deve ser injetada via método construtor → _**Injeção de Depêndencia**_.

Porém, ainda há uma violação do DIP, pois estamos dependendo de uma implementação e não uma abstração. Além de que em uma mudança de banco de dados escolhido, iria-se violar o OCP
### Bom exemplo:
```php
interface DBConnectionInterface {
    public function connect();
}


class MySQLConnection implements DBConnectionInterface {
    public function connect()  { }
}

class OracleConnection implements DBConnectionInterface {
    public function connect() { }
}

class PasswordReminder {
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}
```
Com a criação da interface, a classe PasswordReminder não tem a mínima ideia de qual banco de dados a aplicação irá utilizar. Assim, ambas as classes estão desacopladas e dependendo de uma abstração.

### Exemplo Ilustrado:
![[Pasted image 20230105220652.png]]