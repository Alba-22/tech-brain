#backend
## Introdução
Criar uma aplicação com Spring Framework exigia fazer muitas configurações. Com isso, foi criar o **SpringBoot** que já possui diversas integrações e configurações feitas por padrão, além de já ter um servidor embutido(Tomcat ou Netty) por padrão também.

## Spring MVC
Baseado no estilo arquitetural Model-View-Controller, já traz uma série de anotações para que se possa criar endpoints de forma rápida para uma API REST.
Todo controller é decorado com *@RestController("/")*, no qual é indicado o nome do recurso que aquele controller trata.
Cada método é decorado com *@GetMapping*, *@PostMapping*, *@PutMapping*, etc. para indicar o método HTTP daquele endpoint. Nessa anotação também é passado a String com o path do endpoint que aquele método corresponde.
Nos argumentos do método, pode-se usar *@PathVariable* e *@RequestBody* para pegar os dados da requisição pelo caminho ou pelo corpo, respectivamente.
```kotlin
@RestController   
@RequestMapping("/accounts")  
class AccountController(private val service: AccountService) {  
  
    @PostMapping  
    @ResponseStatus(HttpStatus.CREATED)  
    fun create(@RequestBody account: Account): Account {  
        // ...
    }
    
    @GetMapping  
    fun getAll(): List<Account> {  
        // ...
    }

    @GetMapping("/{id}")  
    fun getById(@PathVariable id: Long): ResponseEntity<Account> {  
        // ...
    }
    
    @PutMapping("/{id}")  
    fun update(@PathVariable id: Long, @RequestBody account: Account): ResponseEntity<Account> {
	    // ...
    }
    
    @DeleteMapping("/{id}")  
    fun delete(@PathVariable id: Long): ResponseEntity<Unit> {  
        // ...
    }
}
```

## Spring Security
É a estrutura de autenticação e autorização do Spring Framework, que é muito poderosa e já inclui proteção contra diversos tipos de ataques, como Session Fixation, Clickjacking, Cross Site Request Forgery.

## Spring Data
Parte do Spring Framework responsável por interagir com as bases de dados, possuindo um projeto especifico para cada banco de dados Além disso, possui abstrações de mapeamento de objetos e tem um repositório robusto.
#### Spring JPA(Java Persistance API)
Usando a anotação *@Repository* e extendendo a classe *JpaRepository<Entidade, TipoDoId>* é possível criar um repositório de acesso a dados, que já conta com uma  série de métodos para manipular os dados de um banco de dados.
Além disso, pode-se criar a assinatura de diversos métodos para fazer filtros sobre os dados a serem manipulados.
```kotlin
@Repository
interface AccountRepository : JpaRepository<Account, Long> {  
}
```

## Spring Testing
Possui suporte para **testes unitários**, **testes de intergração** e conta com o **MockMVC**, que serve para simular requisições à um controller.