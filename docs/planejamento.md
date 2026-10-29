# instruções
- Objetivo: criar funções de sincronização para garantir o embarque dos vagões de forma ordenada
- um carro/trem possui apenas um vagão
- vagão é uma thread
- passageiro é outra thread
- criar struct station com os atributos necessários para o funcionamento do sistema

# ideia
- fazer o fluxo do metrorec, com o trem passando nas estações dos ramais, com a quantidade de pessoas estimadas nas estações e de acordo com o horario
- Recife -> Jaboatão; Recife -> Camaragibe; Recife -> Cajueiro Seco

# fluxo
- chegou o trem na estação -> chama `station_load_car(struct station *station, int count)` OBS: count é a qunatidade de assentos disponíveis no trem/carro
- station_load_car so retorna quando todos os assentos estiverem preenchidos/todos os passageiros sentados e ou vagão cheio (capaciadade maxima) ou todos os que estavam esperando vão entrar
- quando o passageiro chega na estação -> `station_wait_for_car(struct station* station)`
- station_wait_for_car fica esperando o retorno de station_load_car, dizendo que o trem chegou na estação e retorna a quantidade de assentos disponiveis (não pode busy waiting)
- passageiro entra no trem
- OBS: nao se preocupar na função de colocar o passageiro pra sentar nos assentos
- quando o passageiro entrar -> `station_on_board(struct station *station)`
- segue para a proxima estação até o fim/terminal


# funções e structs
- struct station -> char *nome, int passagers, struct train
- station_load_car(struct station *station, int count)
- station_wait_for_car(struct station* station)
- station_on_board(struct station *station)
- struct train -> struct vagao, char *destino
- struct vagao -> int assentosDisponiveis
- station init (): inicia a estação com o trem vazio e assentos vazios (começo da viagem)

## funções de sync e threads
- pthread_mutex_init 
- pthread_mutex_lock 
- pthread_mutex_unlock 
- pthread_cond_init 
- pthread_cond_signal 
- pthread_cond_wait
- pthread_cond_broadcast 

# regras
- não pode usar mais que um unico lock em cada station
- nunca vai ter mais de um trem na estação
- todos os carros e passageiros estão indo para o mesmo destino
- passageiros podem embarcar ao mesmo tempo (parte da sincronização para não sobrescrever)

# duvidas
- o sistema vai funcionar como uma simulação (sem o usuário interagir)? mostra um numero x de pessoas que vão embarcar nas estações e na saída só vai mostrar os logs?
- precisa de sistema de desembarque?
- o usuário precisa interagir com algo? (solicitar algo/selecionar alguma opção)
- o número de assentos nos vagões e passageiros nas estações será definida no código ou será pelo usuário?