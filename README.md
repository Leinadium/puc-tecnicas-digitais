# Trabalho para G2

* Daniel Guimarães

* Mariana Barreto

## Utilização

Para utilizar o circuito, primeiro confira se os contadores estão resetados.
	De vez em quando, o contador de quantidade não reseta ao resetar a simulação,
	se isso acontecer, envie um pulso invertido através da chave de reset, acima do texto
	escrito "para debug", à direita do quadrado de inputs.

Para inserir algum número, aperto no teclado à esquerda do quadrado de inputs. A operação de detectar necessita de 
um pulso completo de clock mais a varredura, por isso segure o botão por tempo suficiente. Para verificar se o botão foi registrado, há um display hexadecimal chamado "quantidade", que mostra a quantidade atual de números na fila.

Para ler um número, utilize a chave indicada dentro do quadrado de inputs. A chave só funciona se houver algo na fila para ler.

## Funcionamento interno

Foi utilizado o teclado por varredura do laboratório 8, porém ele antes havia um problema de nem sempre armazenar o 
valor correto. Agora esse problema foi corrigido (era um problema muito escondido).

Para o funcionamento da memória, tudo foi separado em duas etapas, basicamente controladas pelo clock.
Foi implementado dessa maneira para ter certeza de que não houvesse nenhuma condição de corrida.

```text
# lerTecla up:
	Armazena em um flipflop que o modo é leitura (por default, é escrita)

# Clock up
	Armazena em um flipflop que foi feita uma leitura ou escrita
	Faz a leitura ou escrita
	Acende ou não o LED de Overrun
	Apaga o LED de DDisponivel (se foi uma leitura)
	
# Clock down:
	Reseta o flipflop que diz se foi feita uma leitura ou escrita
	Atualiza os contadores de inicio e quantidade
	Acende ou não o LED de DDisponivel
	Apaga ou não o LED de Overrun
	Reseta o modo para escrita, se estivesse em leitura.
```

Para esse funcionamento especificado acima, foram utilizados 5 flipflops:

	* Dois para controlar o flipflop de leitura e escrita (o primeiro garantia que era feito no clockup, e o outro armazenava)
	
	* Um para o LED de DDisponivel

	* Um para o LED de Overrun
	
	* Um para o modo atual de funcionamento (leitura ou escrita).

Também foram utilizados 4 flipflops para servir como registrador.

Para evitar uma condição de corrida na hora da escrita (pois há o endereço muda, e é feito uma leitura na memória),
foi utilizado um bit a mais na memória, para saber quando que a leitura foi executada. É possível que isso não era necessário, mas foi deixado no circuito como garantia de funcionamento.