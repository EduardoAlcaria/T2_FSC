Trabalho de Depuracao - Jogo da Velha em Assembly RISC-V
Fundamentos de Sistemas Computacionais - 98800-04

Integrantes: Eduardo Alcaria, Thiago Tarantino, Luca Callegari, Renan Bertoletti 
 
===================================================================== 
ERROS ENCONTRADOS E CORRECOES
=====================================================================

ERRO 1 - Interpretacao da entrada (coluna)
  Linha original: addi t2, t2, -48
  Linha corrigida: addi t2, t2, -49
  Motivo: O enunciado especifica coluna = digito - '1'. O caractere '1'
  tem codigo ASCII 49, nao 48. Com -48, a entrada '1' vira 1 (deveria
  ser 0), '2' vira 2 (deveria ser 1), e '3' vira 3 que excede o limite
  0..2, tornando a coluna 3 sempre invalida.

ERRO 2 - Calculo do indice da celula
  Linha original: slli t3, t1, 1  /  add t3, t3, t2
  Linha corrigida: slli t3, t1, 1  /  add t3, t3, t1  /  add t3, t3, t2
  Motivo: O indice correto e linha*3 + coluna. O shift left de 1 bit
  multiplica por 2, nao por 3. Faltava um "add t3, t3, t1" para
  completar a multiplicacao por 3 antes de somar a coluna.

ERRO 3 - Controle de turno
  Linha original: sub t6, t6, s0  /  j loop
  Linha corrigida: sub s0, t6, s0  /  j loop
  Motivo: O resultado de (3 - jogador_atual) era calculado em t6 mas
  nunca salvo em s0. O registrador s0 ficava sempre igual a 1 (X),
  fazendo o jogo nunca alternar para o jogador O.

ERRO 4 - Verificacao de vitoria
  Linha original: li t2, 7
  Linha corrigida: li t2, 8
  Motivo: Ha 8 combinacoes vencedoras (3 linhas + 3 colunas + 2
  diagonais). O loop so verificava 7, nunca testando a diagonal
  anti-principal (indices 2, 4, 6).

ERRO 5 - Impressao do tabuleiro
  Linha original: li a0, 88 (para celula==2) / li a0, 79 (para celula==1)
  Linha corrigida: li a0, 79 (para celula==2) / li a0, 88 (para celula==1)
  Motivo: Os simbolos estavam trocados. Marca 1 = X (ASCII 88) e marca
  2 = O (ASCII 79), mas o codigo imprimia X para celulas de O e vice-versa.
