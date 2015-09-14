# maquinaderefrigerante
Projeto em VHDL, em grupo, com o objetivo de implementar uma máquina de refrigerante em uma FPGA Altera Ciclone 3.

Máquina de Refrigerantes
Vinícius Roratto
Denyson Mendes Grellert
#Granduandos em engenharia da computação
1viniciusroratto@gmail.com
denysonjurgen@gmail.com

Keywords— Circuitos digitais, laboratório, aula prática, circuitos aritiméticos, VHDL, Quartus, Altera, Cliclone, FSM, máquina de estados finitos, trabalho final

Resumo
Este relatório consiste na apresentação teórica do trabalho final em que os alunos utilizaram os conhecimentos adquiridos na disciplina de Circuitos Digitais para desenvolver uma máquina de refrigerantes. Aqui é apresentado os problemas encontrados, algumas possíveis soluções, o código do trabalho e as conclusões.  

Introdução teórica

	
	Para implementação da máquina de refrigerantes, usamos o conceito de FSM, máquina de estados finitos. Para entender melhor este conceito, deve-se ter em conta que nem todos circuitos eletrônicos tem a saída controlada somente pelas entradas. Em muitos casos, a saída vai mudar de acordo com valores calculados anteriormente, também chamados de estado da máquina. Assim, dependendo de qual o estado que a máquina está ela pode ir para diferentes caminhos, de acordo com as entradas, e se o estado for outro, esses caminhos podem ser diferentes.
	Este conceito exige um circuito capaz de armazenar valores que foram calculados antes. Circuitos sequenciais são os responsáveis por essa parte da lógica. Há dois tipos de registradores que armazenam um bit, o latch e o flip-flop. A diferença entre eles é que o latch é sensível a borda, o que significa que, enquanto o sinal de habilitação estiver ativo, qualquer mudança no sinal de entrada é armazenado. O flip-flop é sensível à borda, ou seja, ao sinal que estiver na entrada durante a transição do sinal de ativação que será armazenado.
Outro conceito importante, o mais básico desta área, é o de transístor: 
Reis (2008) destaca que transístores associados podem ser utilizados para a criação de estruturas e equações lógicas, “Um transístor pode ser visto como uma chave controlada pelo sinal de sua grade” (p.57).   A compreensão destes conceitos básicos é vital para a criação de lógicas complexas utilizadas na microeletrônica. A associação de múltiplos transístores pode resultar em lógicas simples como as operações básicas OR/AND/NOT. Já estas operações, combinadas de diferentes maneiras podem resultar em circuitos simples, mas úteis, como um contador, circuitos um pouco mais complexos, como uma máquina de refrigerantes, ou até em poderosos microprocessadores.

	

Materiais Utilizados
Durante o trabalho foi utilizada a seguinte lista de materiais:
Placa FPGA Ciclone III. 
Programa Simulador Quartus II, web edition.

Descrição das Atividades

Aqui descrevemos todo o processo de criação, das primeiras ideias de como fazer funcionar a máquina, até sua implementação final. O processo se iniciou com a escolha do tema, a Máquina de Refrigerante, que foi escolhido devido à aplicabilidade em situações reais, o grupo se interessou por construir uma máquina usada no dia a dia. Além dosso, havia também a possibilidade de aprender uma linguagem de descrição de hardware, o que foi definitivo para a escolha do tema.
O primeiro passo no processo foi pesquisar um pouco sobre as máquinas de refrigerante com o objetivo de descobrir funcionalidades aplicáveis que pudessem interessar aos integrantes do grupo. A partir de uma lista com aplicações criadas para máquinas de refrigerante, o grupo escolheu as que seriam implementáveis com o conhecimento teórico desenvolvido durante a disciplina e a experiência com a linguagem VHDL desenvolvida nos laboratórios práticos. 
Com o conhecimento do grupo, achou-se possível desenvolver duas aplicações básicas para a máquina de refrigerante: a rotina tradicional de controle de troco, em que o usuário escolhe as moedas que vai colocar, recebe um refrigerante e, se houver, recebe o troco, e uma rotina de controle de estoque que libera ou não a compra do produto, conforme o armazenamento. 
Observando as máquinas deste tipo tradicionais podemos encontrar as seguintes entradas e saídas:
Entradas: moedas e seleção do produto
Saídas: Troco e o produto.
O grupo sugeriu também que a saída apresentasse a situação do estoque com relação a um produto, então o ideal seria “avisar” ao usuário da existência ou não de um refrigerante no momento da compra. Outra saída interessante (dessa vez para o dono da máquina, não o cliente) seria o valor total de dinheiro na máquina. 
Uma vez escolhidas as funcionalidades da máquina, foi necessário refletir sobre como seria possível a implementação do circuito. O primeiro passo foi criar rascunhos de possíveis soluções, o resultado foi o aparecimento de uma dúvida: criar do zero um circuito deste tipo é a melhor solução? Não seria possível encontrar outras implementações interessantes, basear-se em boas ideias e evitar construções pouco otimizadas? Era necessário, então, pesquisar o que já foi produzido sobre o assunto.
Entende-se aqui que a busca por soluções para o nosso problema não envolve a ideia de copiar o trabalho dos outros, mas compreender melhor o que já foi feito, para que o resultado final aproveite o conhecimento que já foi construído sobre circuitos de máquinas de refrigerante, buscando soluções eficientes e evitando erros que já foram resolvidos. Para estas pequisas buscou-se na internet por diversos tipos de circuitos teóricos envolvidos na criação de “vending machines”. Algumas imagens abaixo mostram como o processo de pesquisa evoluiu o pensamento do grupo (os links com os endereços da onde foram tiradas as imagens estão no final do relatório)[4]


Imagem 01: O primeiro exemplo mostra o funcionamento de uma vending machine clássica, recebe o valor, entrega o o produto e o troco.
Apesar desta imagem já ser uma ideia básica de como deveria funcionar a máquina de refrigerante, a proposta ainda é tanto superficial. Seguimos então com: 



Imagem 2: As duas imagens dão uma ideia de como pode ser o processo que controla o pagamento do produto.
As imagens acima mostram como seria possível tratar o recebimento de valores pela máquina. Através de uma máquina de estados é possível receber moedas até que o usuário chegue a um valor x, quando encontrado este valor, segue-se para um estado diferente em que, no caso, ele recebe o produto. Estas propostas nos colocaram no caminho de como montar este sistema, mas, ao mesmo tempo nos proporcionaram uma dúvida:
Como o grupo estava interessado em usar valores reais com moedas de baixos e altos valores e diversos refrigerantes com custos variados, uma implementação com uma simples máquina de estados poderia se tornar inviável. Uma vez que um refrigerante pode custar 3,50 e existem moedas de 0,05, seriam necessários pelo menos 71 estados para fazer operar a máquina. Este tipo de dúvida de implementação voltará a ser abordada mais adiante no relatório.


Imagem 3: Formato parecido com árvore nos ajuda a entender o processo de compra de vários produtos.

Uma última observação nos fez encontrar um exemplo de como funcionaria uma máquina em que vários produtos poderiam ser escolhido. Este formato parecido com árvore também nos agradou, mostrando, a cada produto escolhido, o processo segue por caminhos diferentes. Outra ideia que surgiu através de observações é que a máquina precisa ser um ciclo que permita que o usuário compre vários refrigerantes, portanto, é sempre importante voltar ao início, depois de todo o processo terminado.
Com ideias de como proceder com pagamentos, escolhas de produto e o funcionamento geral da máquina e a possibilidade de incluir um diferencial na máquina (controle de estoque), o grupo buscou nos slides de aula um caminho para a esquematização da máquina de estados finitos
O primeiro passo é contruir um diagrama de estados. Para facilitar os primeiros passos, decidimos implementar uma FSM (finite state machine) com um contador que exibiria, em sequência, os produtos disponíveis no estoque da máquina. Segue abaixo a primeira parte da máquina de refrigerante:

Figura 4: Diagrama de estados mostra funcionamento de contador que exibe produtos do estoque.

Os próximos passos para implementação da máquina de estados finitos utilizando flip flops do tipo D (escolhidos pela facilidade de síntese, em relação ao JK) envolvem a criação de uma tabela de estados, a utilização de mapas de Karnaugh para a simplificação de equação de cada entrada e cada saída, após isso é necessário desenhar os blocos do esquema lógico do circuito. Repete-se a operação, não apenas para este circuito inicial, mas para a máquina de estados inteira, com todas suas possíveis entradas e possíveis saídas.
É possível entender que o tamanho do problema aqui cresce de maneira que, se resolvidos na “força bruta”, através destas tabelas e mapas, certamente implicaria na perda de muitas horas de trabalho para tratar todos os possíveis erros humanos. Foi necessário, novamente, procurar uma solução de mais alto nível. 
O uso da FSM não foi descartada, muito pelo contrário, foi fundamental na esquematização do processo que seguiria, então, com o uso de uma linguagem de descrição de hardware, o VHDL. Segue abaixo o esquema com o resto das tabelas de estado, mostrando o resto da composição do circuito.



Figura 5: Máquina de estados com o circuito, do início ao fim,

A concepção da máquina é a seguinte:
 Inicia-se preso no loop 1, onde o usuário observa quais produtos estão disponíveis, e aperta um botão para sair daquele estado. (este processo sofreu uma modificação e foi retirado no final do projeto).
O estado 2 é o estado de seleção. Aqui, através do switchs da FPGA, o usuário seleciona um produto e recebe seu valor. Quando aperta um botão, o produto é selecionado.
O estado R é igual (de 1 a 5), aqui o usuario coloca moedas através dos switches e dos botões. Quando a soma total das moedas igualar ou passar o valor do produto, ele é enviado para o próximo estado.
O estado T calcula se os usuários recebem troco, e se é possível dar troco.
Ao final mostra-se o valor total recebido pela máquina até agora.
Para melhor entender o processo de pagamento, explicamos melhor aumentando o número de estados entre R e o Total:







Figura 7: Esquema mostra como funciona os estados em que a máquina dá o troco.
Os estados acima mostram como funciona o processo de decisões entre o recebimento do valor inicial e a finalização do ciclo da máquina. O grupo desenvolveu em VHDL uma máquina que, apesar ter sofrido algumas mudanças durante o processo de produção, segue a lógica desenvolvida acima.

Resultados
	
	A partir da descrição do trabalho feita acima, escrevemos o seguinte código (que também vai em arquivo anexo, para facilitar a visualização no editor de texto de preferência do leitor):

library ieee;
use IEEE.std_logic_1164.all;
use IEEE.numeric_std.all;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

ENTITY Maquina_de_Refri is
PORT(
	clock 	:in std_logic;
	button1	:in std_logic;
	button2	:in std_LOGIC;
	button3	:in std_logic;
	switch	:in std_logic_vector (9 downto 0);
	leds		:out std_logic_vector (9 downto 0);
	seg_1    :out std_logic_vector (6 downto 0);
	seg_2    :out std_logic_vector (6 downto 0);
	seg_3    :out std_logic_vector (6 downto 0);
	seg_4    :out std_logic_vector (6 downto 0)
);

END ENTITY Maquina_de_Refri;


ARCHITECTURE arq OF Maquina_de_Refri IS

COMPONENT dec7seg is
port (
		bin : in std_logic_vector(3 downto 0);  --BCD input
      seg : out std_logic_vector(6 downto 0)  -- 7 bit decoded output.
    );
END COMPONENT;

COMPONENT debounce is
GENERIC(
    counter_size  :  INTEGER := 19); --counter size (19 bits gives 10.5ms with 50MHz clock)
  PORT(
    clk     : IN  STD_LOGIC;  --input clock
    button  : IN  STD_LOGIC;  --input signal to be debounced
    result  : OUT STD_LOGIC); --debounced signal
END COMPONENT;

TYPE stateB is (state6, state7, state8, state9, state10, state11, state12, state13);

SIGNAL t_r_1, t_r_2, t_r_3, t_r_4, t_r_5: std_logic := '1';

SIGNAL total		:  integer range 0 to 1000 := 0;
SIGNAL preco		:  integer range 0 to 350;

SIGNAL counter : integer range 0 to 50000000;
SIGNAL pr_state2, nx_state2 :stateB := state6;
SIGNAL soda		: std_logic_vector (2 downto 0) := "111";
SIGNAL reset	: std_logic;
SIGNAL bounce1	: std_logic;
SIGNAL bounce2	: std_LOGIC;
SIGNAL bounce3	: std_logic;

SIGNAL clock1	: std_LOGIC;

SIGNAL bit_n1 	: std_logic_vector (3 downto 0):= "1111";
SIGNAL bit_n2 	: std_logic_vector (3 downto 0):= "1111";
SIGNAL bit_n3	: std_logic_vector (3 downto 0):= "1111";
SIGNAL bit_n4  : std_logic_vector (3 downto 0):= "1111";

SIGNAL dec1		: integer range 0 to 9 := 0;
SIGNAL dec2		: integer range 0 to 9 := 0;
SIGNAL dec3		: integer range 0 to 9 := 0;
SIGNAL dec4		: integer range 0 to 9 := 0;

CONSTANT preco1: std_logic_vector (11 downto 0):= "000001010000";
CONSTANT preco2: std_logic_vector (11 downto 0):= "000100000000";
CONSTANT preco3: std_logic_vector (11 downto 0):= "000101010000";
CONSTANT preco4: std_logic_vector (11 downto 0):= "001001110101";
CONSTANT preco5: std_logic_vector (11 downto 0):= "001101010000";

BEGIN

leds(0) <= t_r_1;
leds(1) <= t_r_2;
leds(2) <= t_r_3;
leds(3) <= t_r_4;
leds(4) <= t_r_5;
reset <= bounce3;

PROCESS(clock, reset)
	BEGIN
		if(reset = '1') then
			clock1 <= '0';
			counter <= 0;
		elsif(rising_edge(clock)) then
			if(counter = 5000000) then
				clock1 <= not(clock1);
				counter <= 0;
			else
				counter <= counter + 1;
			end if;
		end if;
	END PROCESS;

PROCESS (clock1)
	BEGIN
		if(rising_edge(clock)) then
			pr_state2 <= nx_state2;
		end if;
END PROCESS;
	
PROCESS(pr_state2, reset)

VARIABLE coin		: std_logic_vector (3 downto 0) := "1111";
VARIABLE valoratual :  integer range 0 to 1000 := 0;

	BEGIN
		if(reset = '0') then
			valoratual := 0; -- zera valor atual.
			total <= 0;
			
			t_r_1 <= '1';
			t_r_2 <= '1';
			t_r_3 <= '1';
			t_r_4 <= '1';
			t_r_5 <= '1';
			
			leds(9 downto 7) <= (others => '0');
			
			nx_state2 <= state6;
		
		else
			case pr_state2 is
				when state6 =>
					leds(9)<= '1';
					leds(8 downto 7) <= "00";
					-- Aqui precisamos receber qual refri foi escolhido pelo usuário.
					-- Fica preso aqui até receber o valor.
					-- Esse valor vai inflenciar o resto do caminho. 
					-- atrelado a este valor também está o custo do refri. Precisamos de uma variável para isso.
					-- para escolhar o valor é necessário selecionar o switch E apertar o botão.
					valoratual := 0;
					
					if(switch(0) = '1') then
						soda <= "000";
					elsif(switch(1) = '1') then
						soda <= "001";
					elsif(switch(2) = '1') then
						soda <= "010";
					elsif(switch(3) = '1') then
						soda <= "011";
					elsif(switch(4) = '1') then
						soda <= "100";
					else
						soda <= "111";
					end if;			
					
					if (soda = "000") then -- arrumar entrada.
						if t_r_1 = '1' then -- testa se tem refri no estoque.
							bit_n1 <= preco1(3 downto 0);
							bit_n2 <= preco1(7 downto 4);
							bit_n3 <= preco1(11 downto 8);
							nx_state2 <= state6; -- segue para o próximo estado.
							if(bounce1 = '0') then
								nx_state2 <= state7;
								preco <= 50;
							end if;					
						else
							bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
							bit_n2 <= "1111";
							bit_n3 <= "1111";
							bit_n4 <= "1111";
							nx_state2 <= state6; -- continua preso no S6.
						end if;
					
					elsif (soda = "001") then -- arrumar entrada.
						if t_r_2 = '1' then -- testa se tem refri no estoque.
							bit_n1 <= preco2(3 downto 0);
							bit_n2 <= preco2(7 downto 4);
							bit_n3 <= preco2(11 downto 8);
							nx_state2 <= state6; -- segue para o próximo estado.
							if(bounce1 = '0') then
								nx_state2 <= state7;
								preco <= 100;
							end if;					
						else
							bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
							bit_n2 <= "1111";
							bit_n3 <= "1111";
							bit_n4 <= "1111";
							nx_state2 <= state6; -- continua preso no S6.
						end if;
						
					elsif (soda = "010") then -- arrumar entrada.
						if t_r_3 = '1' then -- testa se tem refri no estoque.
							bit_n1 <= preco3(3 downto 0);
							bit_n2 <= preco3(7 downto 4);
							bit_n3 <= preco3(11 downto 8);
							nx_state2 <= state6; -- segue para o próximo estado.
							if(bounce1 = '0') then
								nx_state2 <= state7;
								preco <= 150;
							end if;					
						else
							bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
							bit_n2 <= "1111";
							bit_n3 <= "1111";
							bit_n4 <= "1111";
							nx_state2 <= state6; -- continua preso no S6.
						end if;	
							
					elsif (soda = "011") then -- arrumar entrada.
						if t_r_1 = '1' then -- testa se tem refri no estoque.
							bit_n1 <= preco4(3 downto 0);
							bit_n2 <= preco4(7 downto 4);
							bit_n3 <= preco4(11 downto 8);
							nx_state2 <= state6; -- segue para o próximo estado.
							if(bounce1 = '0') then
								nx_state2 <= state7;
								preco <= 275;
							end if;					
						else
							bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
							bit_n2 <= "1111";
							bit_n3 <= "1111";
							bit_n4 <= "1111";
							nx_state2 <= state6; -- continua preso no S6.
						end if;
						
					elsif (soda = "100") then -- arrumar entrada.
						if t_r_1 = '1' then -- testa se tem refri no estoque.
							bit_n1 <= preco5(3 downto 0);
							bit_n2 <= preco5(7 downto 4);
							bit_n3 <= preco5(11 downto 8);
							nx_state2 <= state6; -- segue para o próximo estado.
							if(bounce1 = '0') then
								nx_state2 <= state7;
								preco <= 350;
							end if;					
						else
							bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
							bit_n2 <= "1111";
							bit_n3 <= "1111";
							bit_n4 <= "1111";
							nx_state2 <= state6; -- continua preso no S6.
						end if;
						
					else
						bit_n1 <= "0000"; -- se não tem refri continua recebendo zero.
						bit_n2 <= "1111";
						bit_n3 <= "1111";
						bit_n4 <= "1111";
						nx_state2 <= state6; -- continua preso no S6.
					end if;	
					
				when state7 =>
					leds(9)<= '0';
					leds(8)<= '1';
					-- Pagamento é aqui. 
					-- valor atual vai recebendo moedas e fica preso no estado até igualar/passar valor do refri.
					-- valor atual precisa ser apresentado a cada moeda.
					dec1 <= valoratual mod 10;
					dec2 <= valoratual mod 100;
					dec3 <= valoratual mod 1000;
					dec4 <= valoratual mod 10000;
					
					bit_n1 <= std_logic_vector(to_unsigned(dec1, 4));
					bit_n2 <= std_logic_vector(to_unsigned(dec2, 4));
					bit_n3 <= std_logic_vector(to_unsigned(dec3, 4));
					bit_n4 <= std_logic_vector(to_unsigned(dec4, 4));	
					
					if(switch(0) = '1') then -- converte valor do switch para a moeda.
						coin := "0000";
					elsif(switch(1) = '1') then
						coin := "0001";
					elsif(switch(2) = '1') then
						coin := "0010";
					elsif(switch(3) = '1') then
						coin := "0011";
					elsif(switch(4) = '1') then
						coin := "0100";
					elsif(switch(5)= '1') then
						coin := "0101";
					elsif(switch(6)= '1') then
						coin := "0111";
					else
						coin := "1111";
					end if;				
					
					-- modelo:
					-- if entrada == 1, soma 5, testa se valoratual >= preco. if valoratual<preco, proximoestado <= state7, else proximoestado <= state8.
					-- elsif entrada == 2, soma 10, testa se valoratual >= preco. if valoratual<preco, proximoestado <= state7, else proximoestado <= state8.
					-- elsif entrada == 3, soma 25, testa se valoratual >= preco. if valoratual<preco, proximoestado <= state7, else proximoestado <= state8.
					
					if coin = "0000" and bounce2 = '0' then -- pegar o valor do switch, E apertar o botão. 
						valoratual := 5 + valoratual; -- soma valor da moeda ao valoratual. cinco centavos
						if valoratual < preco then -- enquanto valor atual é menor que o preco, segue no mesmo estado.
							nx_state2 <= state7; 
						else
							nx_state2 <= state8;
						end if;
						
						elsif coin = "0001" and bounce2 = '0' then
							valoratual := 10 + valoratual; -- 10 centavos
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;
							
						elsif coin = "0010" and bounce2 = '0' then
							valoratual := 25 + valoratual; -- 25 centavos
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;
							
						elsif coin = "0011" and bounce2 = '0' then
							valoratual := 50 + valoratual; -- 50 centavos
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;
						
						elsif coin = "0100" and bounce2 = '0' then
							valoratual := 100 + valoratual; -- 100 centavos
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;					
						
						elsif coin = "0101" and bounce2 = '0' then
							valoratual := 200 + valoratual; -- 200 centavos
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;
							
						elsif switch = "0110" and bounce2 = '0' then
							valoratual := 500 + valoratual;
							if valoratual < preco then
								nx_state2 <= state7;
							else
								nx_state2 <= state8;
							end if;
						else
							nx_state2 <= state7; -- enquanto nada mais acontece, fica preso por ai.
						end if;
						
				when state8 => 
					leds(8)<= '0';
					leds(7)<= '1';
					--if valoratual == preco segue para o 9.
					if (valoratual = preco) then
						nx_state2 <= state9;
					-- if valoratual > preco, segue para o 12.
					else
						nx_state2 <= state12;
					end if; 
					
				when state9 =>
					leds(9 downto 7) <= "000";
					-- add valoratual ao TOTAL.
					total <= total + valoratual;
					-- segue para o 10.
					nx_state2 <= state10;
					
				when state10 =>
				leds(9 downto 7) <= "000";
					-- exibe TOTAL.
					dec1 <= total mod 10;
					dec2 <= total mod 100;
					dec3 <= total mod 1000;
					dec4 <= total mod 10000;
					
					bit_n1 <= std_logic_vector(to_unsigned(dec1, 4));
					bit_n2 <= std_logic_vector(to_unsigned(dec2, 4));
					bit_n3 <= std_logic_vector(to_unsigned(dec3, 4));
					bit_n4 <= std_logic_vector(to_unsigned(dec4, 4));	
					
					-- segue para estado 11
					if(bounce1 = '0') then
						nx_state2 <= state11;
					else
						nx_state2 <= state10;
					end if;
					
				when state11 =>
		leds(9 downto 7) <= "000";		
					-- retira refri x do estoque.
					if (soda = "0000") then -- sequencia equivalente a um switch recebe o número do refri e zera t_r_x
						t_r_1 <= '0';
					elsif (soda = "0001") then
						t_r_2 <= '0';
					elsif (soda = "0010") then
						t_r_3 <= '0';
					elsif (soda = "0011") then
						t_r_4 <= '0';
					else
						t_r_5 <= '0';
					end if;
					
					-- volta para estado 1.
					nx_state2 <= state6;
						
				when state12 =>
				leds(9 downto 7) <= "000";
					total <= total + valoratual;
					-- if troco > total, segue para 14.
					--if (valoratual - preco) > total then
						--nx_state2 <= state14;
					-- if troco <= total, segue para 13.
					--else
					nx_state2 <= state13;
					--end if;
					
				when state13 => 
				leds(9 downto 7) <= "000";
					-- apresenta preco - dinheiro.
					dec1 <= (valoratual - preco) mod 10;
					dec2 <= (valoratual - preco) mod 100;
					dec3 <= (valoratual - preco) mod 1000;
					dec4 <= (valoratual - preco) mod 10000;
					
					bit_n1 <= std_logic_vector(to_unsigned(dec1, 4));
					bit_n2 <= std_logic_vector(to_unsigned(dec2, 4));
					bit_n3 <= std_logic_vector(to_unsigned(dec3, 4));
					bit_n4 <= std_logic_vector(to_unsigned(dec4, 4));	
					
					total <= total - (valoratual - preco);
					-- segue para estado 10.
					if(bounce1 = '0') then
						nx_state2 <= state10;
					else
						nx_state2 <= state13;
					end if;
					
			end case;
		end if;
	END PROCESS;
	
		seg1: dec7seg PORT MAP (bin => bit_n1, seg => seg_1);
		seg2: dec7seg PORT MAP (bin => bit_n2, seg => seg_2);
		seg3: dec7seg PORT MAP (bin => bit_n3, seg => seg_3);
		seg4: dec7seg PORT MAP (bin => bit_n4, seg => seg_4);
		
		button_1: debounce PORT MAP (clk=> clock, button => button1, result => bounce1);
		button_2: debounce PORT MAP (clk=> clock, button => button2, result => bounce2);
		button_3: debounce PORT MAP (clk=> clock, button => button3, result => bounce3);	
		
END ARCHITECTURE arq;	
O maior desafio em relação ao código foi entender o funcionamento do VHDL. Como os participantes do grupo tem algum conhecimento de lógica de programação,  foi possível entender a sintaxe com alguma facilidade, mas a lógica de funcionamento da FPGA, junto com o VHDL, mostrou-se um pouco mais difícil. A parte teórica da disciplina também ajudou a entender alguns conceitos, mas a verdade é que muitos dos problemas foram descobertos pela primeira vez quando testamos o funcionamento na FPGA.
Entre asdificuldades que enfrentamos, a impossibilidade de usar sinais em mais de um processo, definiu uma mudança grande no projeto. Inicialmente, a ideia era usar uma velocidade de clock lenta para que o usuário conseguisse observar o contador (que mostrava o estoque) e outra (mais rápida) para os estados onde os comandos eram acionados pelo usuário. Para isso foram criados dois processos, mas como alguns valores (principalmente os do estoque) precisavam ser utilizados nos dois processos, o programa falhou. Nesse aspecto, o grupo teve dificuldade em proceder com uma solução que mantivesse o contador, então optou-se por uma solução mais fácil: ao invés do contador, usamos leds para representar a existência ou não de produtos no estoque.
Infelizmente não (como um código muito amplo e estados com muita lógica interna) não foram possíveis resolver alguns problemas, e o funcionamento final da máquina ficou bastante prejudicado. Mesmo com algumas tardes de tentativas, alguns objetivos não foram atingidos, então a apresentação final sofreu com alguns bugs. Entre eles, o pior, era o funcionamento inesperado da máquina quando alcançados alguns estados e a dificuldade em realizar o pagamento. 


Conclusão:
Mesmo com as dificuldades finais, o resultado foi positivo de alguma maneira. Foi possível projetar e implementar uma máquina de estados finitos com algumas utilidades, de maneira que conseguimos aprender uma nova linguagem e resolver um problema através do uso de conceitos teóricos de microeletrônica.
O trabalho final foi a peça pedagógica que melhor fez a ligação entre os conteúdo teórico, apresentado nas aulas expositivas, e o conteúdo prático dos laboratórios. Com a necessidade de pensar sozinhos, pesquisar soluções e enfrentar problemas “reais”, o grupo conseguiu (relativo ao pouco conhecimento que tinha) desenvolver bastante seus conhecimentos de microeletrônica.
 Enquanto no início do semestre, a única possibilidade de criação de circuitos conhecidos pelos alunos era operações lógicas básicas com and/or/nor, no final do semestre foi possível uma série de processos envolvendo síntese de máquinas de estados em uma complexidade que ainda não era compreendida. No final das contas, a disciplina de circuitos digitais serviu como  um primeiro passo no caminho da utilização, e quem sabe, um dia, produção, de ferramentas de design de microcircuitos assistidas por computadores (computer aided design, ou CAD).



Referências Bibliográficas
REIS, R. A. L., Concepção de Circuitos Integrados, Bookman, 2009.
[2] Materiais de aula.
[3] WAGNER, Flavio Rech, Fundamentos de circuitos digitais- Porto Alegre, Bookman, Instituto de Informática da UFRGS, 2008.
[4]Imagens:
http://www.cybernadian.net/fsm.html
http://bits.citrusbyte.com/state-design-pattern-with-ruby/
http://pt.slideshare.net/pratikpatilee/fsm-based-vending-machine-pratik-patil
http://cecs.wright.edu/~pmateti/Courses/7370/Lectures/Actors+Akka+Scala/dining-philosophers-akka-fsm.html
http://embsys.technikum-wien.at/projects/decs/verification/formalmethods.php
