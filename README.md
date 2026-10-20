# simulador-reserva-frota
Simulador de reserva para renovação de frota, feito em planilha

🚚 Simulador de Reserva para Frota

Ferramenta em planilha que simula quanto uma transportadora acumula guardando um valor todo mês para renovar a frota, e quantos veículos esse dinheiro consegue comprar.

Projeto desenvolvido no Desafio de Projeto de Excel da DIO, adaptando o simulador de fundos imobiliários do Expert Felipe Aguiar para a área de logística.

Ferramenta construída no Google Sheets e exportada para Excel (.xlsx).

Tabela de apoio com os percentuais de cada perfil Mostrar Imagem

❓ As 5 perguntas que a ferramenta responde
Pergunta	Onde aparece
Quanto guardar por mês?	Reserva mensal (R$), informada pelo usuário
Por quantos anos?	Prazo (anos), informado pelo usuário
Qual o rendimento mensal da aplicação?	Rendimento mensal (%), informado pelo usuário
Quanto vai ter acumulado?	Total acumulado (R$), calculado
Quantos veículos esse valor compra?	Veículos que dá para comprar, calculado

Além disso, a ferramenta:

projeta o acumulado em cenários de 2, 5, 10, 20 e 30 anos;
divide a reserva mensal entre 6 destinos, conforme o perfil da empresa escolhido numa lista.

Código de cores: células em lilás são para preencher, células em cinza são calculadas e não devem ser editadas.

⚙️ Como funciona por dentro
Função VF: o total acumulado
=VF(taxa_mensal; prazo*12; -reserva)
taxa_mensal: o rendimento de cada mês.
prazo*12: o prazo é informado em anos, mas a taxa é mensal, então converti para meses.
-reserva: o sinal negativo indica que o dinheiro sai do caixa da empresa para a reserva. Sem ele, o resultado sairia negativo.

Exemplo: guardando R$ 3.000 por 5 anos, a empresa deposita R$ 180.000 e acumula R$ 229.871,60. A diferença de quase R$ 50 mil são os juros.

Veículos que dá para comprar
=ARREDONDAR.PARA.BAIXO(total_acumulado / preco_veiculo; 0)

Arredondo para baixo porque não dá para comprar um veículo pela metade.

Cenários

A mesma lógica do VF, repetida para 2, 5, 10, 20 e 30 anos. A taxa e a reserva são fixas (intervalos nomeados), enquanto o prazo vem da própria linha, para a fórmula funcionar ao ser arrastada.

PROCV com chave composta: divisão por perfil

Na aba Perfis, cada destino aparece 3 vezes (uma por perfil). Para o PROCV achar a linha certa, criei uma coluna Chave que junta perfil e destino:

=B2&C2   →   "ConservadorVeículos novos"

Essa combinação existe uma única vez na tabela. No simulador, a busca fica assim:

=PROCV(perfil&B15; tabela_perfis; 4; 0)
perfil&B15: monta a mesma chave (perfil escolhido + destino da linha).
tabela_perfis: a tabela de apoio.
4: a coluna do percentual.
0: busca exata.

O valor em reais de cada destino é o percentual vezes a reserva mensal.

🏷️ Intervalos nomeados

Nome	O que guarda
reserva	Reserva mensal informada
prazo	Prazo em anos
taxa_mensal	Rendimento mensal da aplicação
faturamento	Faturamento mensal da empresa (aba Config)
perc_reserva	% do faturamento sugerido para a reserva (aba Config)
reserva_sugerida	Faturamento × % sugerido (aba Config)
preco_veiculo	Preço médio de um veículo (aba Config)
perfil	Perfil escolhido na lista
tabela_perfis	Tabela de apoio com chave, perfil, destino e %

Com os nomes, a fórmula =VF($C$7;$C$6*12;-$C$5) virou =VF(taxa_mensal;prazo*12;-reserva), que dá para ler como uma frase.

📊 Perfis e percentuais

Destino	Conservador	Equilibrado	Expansão
Veículos novos	20%	35%	50%
Manutenção preventiva	25%	20%	10%
Tecnologia e rastreamento	10%	15%	15%
Armazém e estrutura	10%	10%	15%
Treinamento de motoristas	10%	10%	5%
Fundo de emergência	25%	10%	5%
Total	100%	100%	100%

Os percentuais são exemplos criados para o projeto, não uma recomendação. A lógica foi:

Conservador: prioriza manter a frota atual rodando (manutenção) e ter caixa para imprevistos.
Equilibrado: divide entre renovar a frota e cuidar da operação.
Expansão: concentra a reserva em comprar veículos novos para crescer.

🔄 O que mudei em relação ao projeto do Expert

Projeto original	Minha versão
Investimento em fundos imobiliários	Reserva de uma transportadora para renovar a frota
Patrimônio acumulado	Total acumulado da reserva
Dividendos mensais	Quantidade de veículos que a reserva compra
Salário e sugestão de 30% para aporte	Faturamento e sugestão de 10% para reserva
6 tipos de fundo imobiliário	6 destinos da reserva (veículos, manutenção, tecnologia...)
Perfis de investidor	Perfis de empresa: Conservador, Equilibrado e Expansão

Também adicionei um gráfico de rosca que muda junto com o perfil escolhido, mostrando a divisão da reserva em reais e em percentual.

🛠️ Técnicas utilizadas

VF · PROCV · chave composta com & · ARREDONDAR.PARA.BAIXO · SOMA · referências absolutas · intervalos nomeados · validação de dados · formatação de interface (sem linhas de grade, código de cores) · gráfico de rosca

Todos os valores da planilha (faturamento, preço do veículo, reserva) são de exemplo.
