> A class should have one, and only one, reason to change.

Uma classe deve ter somente um motivo pra mudar, sendo especializada em uma única tarefa.

### Problemas ao não ser seguir o princípio
- Falta de coesão: uma classe não deve assumir responsabilidades que não são suas;
- Alto acoplamento: mais responsabilidades geram um maior nível de dependências, deixando o sistema engessado e frágil para alterações;
- Dificuldades de implementar testes automatizados: é difícil de “mockar” esse tipo de classe;
- Baixo reaproveitamento.

Pode ser aplicado à métodos e funções também, ou seja, tudo que é responsável por executar uma ação, deve ser responsável por apenas aquilo que se propõe a fazer.

O SPR acaba sendo a base para outros princípios e padrões porque aborda temas como acoplamento e coesão, características que todo código orientado a objetos deveria ter.

### Mau Exemplo:
```php
class Order { 
	public function calculateTotalSum(){/*...*/} 
	public function getItems(){/*...*/}
	public function getItemCount(){/*...*/}
	public function addItem($item){/*...*/}
	public function deleteItem($item){/*...*/}
	public function printOrder(){/*...*/}
	public function showOrder(){/*...*/}
	public function load(){/*...*/}
	public function save(){/*...*/}
	public function update(){/*...*/}
	public function delete(){/*...*/}
}
```
### Bom Exemplo:
```php
class Order {
	public function calculateTotalSum(){/*...*/}
	public function getItems(){/*...*/}
	public function getItemCount(){/*...*/}
	public function addItem($item){/*...*/}
	public function deleteItem($item){/*...*/}
} 

class OrderRepository {
	public function load($orderID){/*...*/}
	public function save($order){/*...*/}
	public function update($order){/*...*/}
	public function delete($order){/*...*/}
}

class OrderViewer {
	public function printOrder($order){/*...*/}
	public function showOrder($order){/*...*/}
}
```

### Exemplo Ilustrado:
![[Pasted image 20230105215703.png]]
