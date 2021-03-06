# LPT - TEIACARD - (Leiaute Integração Players Externos)
 
 `VERSÃO 01.07`

# 1. Introdução

## 1.1. Apresentação do documento

Este manual apresenta um padrão para a troca de informações entre o
TEIACARD e o ERP da empresa contratante. Baseado nas informações
necessárias para a implantação da solução TEIACARD, o padrão define um
conjunto de registros/campos que devem compor o arquivo de troca de
informações.

## 1.2. Estrutura do documento
    
O documento está dividido nos seguintes tópico:

 - INTRODUÇÃO: Apresenta uma visão geral do tipo de serviço disponível e
o contexto em que ocorre, identificando os players de origem e de destino
de cada fluxo de troca de informações.

 - ESTRUTURA DO ARQUIVO: Define a composição do arquivo (header, lotes
   de serviço e trailer), conceituando cada tipo de registro existente e
   especificando a forma de utilização de cada um deles por tipo de serviço, e
   apresenta o layout do header e do trailer de arquivo.

 - SERVIÇO: Apresenta detalhadamente cada serviço disponível e o contexto
   em que ocorrem, identificando os players de origem e de destino de cada
   fluxo de troca de informações para a TRANSAÇÃO DE VENDAS COM
   CARTÕES.
  
   - Vendas crédito a vista rotativo;
   - Vendas crédito parcelado;
   - Vendas débito a vista;
   - Vendas débito pré-datado;
   - Estornos crédito a vista rotativo;
   - Estornos crédito parcelado;
   - Estornos débito a vista;
   - Estornos débito pré-datado;
   - Baixas parcelas vendas crédito a vista rotativo;
   - Baixas parcelas vendas crédito parcelado;
   - Baixas parcelas vendas débito a vista;
   - Baixa parcelas vendas débito pré-datado;
   
 - DESCRIÇÃO DOS CAMPOS: Conceitua todos os campos componentes do
   layout dos registros utilizados em cada um dos tipos de serviço.
   Em cada layout de registro apresentado, é especificado o código da
   descrição de cada campo. Através deste código, deve-se acessar o tópico
   “Descrição dos Campos” e buscar a descrição do campo que se deseja
   consultar. As descrições de campos assinaladas com * antes do código,
   merecem uma atenção especial. 
  

## 1.3. Manutenção das versões do manual

A versão é identificada através de um código com a seguinte composição:

`VV.RR` onde: `VV` (2 dígitos) = Número da versão e `RRR` (3 dígitos) = Número do release.

> Release: Será alterado sempre que ocorrer alteração, exclusão ou inclusão de campos no LEIAUTE.

> Versão de LEIAUTE de arquivo: Será alterado quando ocorrer inclusão ou
  exclusão de serviços (lotes de serviços).

# 2. Estrutura do Arquivo 

## 2.1. COMPOSIÇÃO DO ARQUIVO

O Arquivo de troca de informações entre a Empresa e o TEIACARD é composto
de um registro header de arquivo, um ou mais lotes de Serviços e um registro
trailer de arquivo, conforme ilustra a figura abaixo:

![image](https://github.com/johnathansantos/doc/blob/master/composicao-arquivo.png)

Com a estrutura apresentada, um único arquivo pode conter vários lotes de
Serviços distintos. Este procedimento, que permite com que a Empresa e o
TEIACARD consolidem em um só arquivo todas as informações que desejam
trocar entre si, deve ser previamente acordado entre cada Empresa e o
TEIACARD.

**Lote de Serviço**: Um lote de Serviço é composto de um registro header de lote, um ou mais registros detalhe, e um registro trailer de lote.

> Um Lote de Serviço só pode conter um único tipo de Serviço

Os registros `HEADER (1)` e `TRAILER (3)` de `LOTE` e os de `DETALHE (2)` são
compostos de campos fixos, comuns a todos os tipos de SERVIÇO, e campos
específicos, padrões para cada um dos tipos de Serviço.

**Registros de Detalhe** : Um registro de detalhe é composto de um ou mais
segmentos, dependendo do tipo de Serviço.

Existem vários tipos de segmentos diferentes e cada um deles pode ser utilizado
em um ou mais lotes de Serviço, tanto nos fluxos de Remessa como nos fluxos
de Retorno, conforme discriminados a seguir:


| ID   | Segmento Remessa | Segmento Retorno |
| ---  | --- | --- |
| Vendas Crédito à Vista Rotativo                    | V `Obrigatório`  | V `Obrigatório` </br> P `Obrigatório` |
| Vendas Crédito Parcelado                           | V `Obrigatório`  | V `Obrigatório` </br> P `Obrigatório` |
| Vendas Débito à Vista                              | V `Obrigatório`  |                                       |
| Vendas Débito Pré-Datado                           | V `Obrigatório`  | V `Obrigatório` </br> P `Obrigatório` |
| Estornos Crédito à Vista Rotativo                  | X `Opcional`     | X `Obrigatório`                       |
| Estornos Crédito Parcelado                         | X `Opcional`     | X `Obrigatório`                       |
| Estornos Débito a Vista                            | X `Opcional`     | X `Obrigatório`                       |
| Estornos Débito Pré-Datado                         | X `Opcional`     | X `Obrigatório`                       |
| Ocorrências Crédito à Vista Rotativo               |                  | O `Obrigatório`                       |
| Ocorrências Crédito Parcelado                      |                  | O `Obrigatório`                       |
| Ocorrências Débito à Vista                         |                  | O `Obrigatório`                       |
| Ocorrências Débito Pré-Datado                      |                  | O `Obrigatório`                       |
| Ocorrências Ajustes                                |                  | O `Obrigatório`                       |
| Baixas Parcelas Vendas Crédito à Vista Rotativo    |                  | B `Obrigatório`                       |
| Baixas Parcelas Vendas Crédito Parcelado           |                  | B `Obrigatório`                       |
| Baixas Parcelas Vendas Débito Pré-Datado           |                  | B `Obrigatório`                       |
| Baixas Vendas Débito à Vista                       |                  | B `Obrigatório`                       |

OBSERVAÇÕES

Tamanho do Registro: O Tamanho do Registro é de `300 bytes`

Alinhamento de Campos:
 - `Campos Numéricos (N)` = À direita, preenchidos com zeros à esquerda.
 - `Campos Alfanuméricos (A)` = À esquerda, preenchidos com brancos à direita.

Convenção:
 - `Remessa` = Fluxo de informações da Empresa (ERP) para o TEIACARD.
 - `Retorno` = Fluxo de informações do TEIACARD para a Empresa (ERP).

## 2.2. HEADER E TRAILER DO ARQUIVO

### Cabeçalho do Arquivo.

| ID   | Campo                                      | Dê    | Até   | Qnt.  | Decimal | Formato | Padrão | Descrição |
| ---  | ------                                     | ---:  | ---:  | ---:  | ---:  | :---: | ---     | ---     |
| 01.0 | Código da Empresa                          | 1     | 3     | 3     | -     | N     |         | [GNR.001](#GNR.001) |
| 02.0 | Lote de Serviço                            | 4     | 7     | 4     | -     | N     | 0000    | [GNR.002](#GNR.002) |
| 03.0 | Tipo de Registro                           | 8     | 8     | 1     | -     | N     | 0       | [GNR.003](#GNR.003) |
| 04.0 | Uso Exclusivo Netunna                      | 9     | 17    | 9     | -     | A     |         | [GNR.004](#GNR.004) |
| 05.0 | Tipo de Inscrição da Empresa               | 18    | 18    | 1     | -     | N     |         | [GNR.005](#GNR.005) |
| 06.0 | Número de Inscrição da Empresa             | 19    | 32    | 14    | -     | N     |         | [GNR.006](#GNR.006) |
| 07.0 | Nome da Empresa                            | 33    | 72    | 40    | -     | A     |         | [GNR.007](#GNR.007) |
| 08.0 | Nome da Adquirente                         | 73    | 112   | 40    | -     | A     |         | [GNR.008](#GNR.008) |
| 09.0 | Código de Remessa ou de Retorno            | 113   | 113   | 1     | -     | N     |         | [GNR.009](#GNR.009) |
| 10.0 | Data de Geração do Arquivo                 | 114   | 121   | 8     | -     | A     |         | [GNR.010](#GNR.010) |
| 11.0 | Hora de Geração do Arquivo                 | 122   | 127   | 6     | -     | N     |         | [GNR.011](#GNR.011) |
| 12.0 | Número Sequencial do Arquivo (NSA)         | 128   | 134   | 7     | -     | N     |         | [GNR.012](#GNR.012) |
| 13.0 | Número da Versão do Leiaute do Arquivo     | 135   | 139   | 5     | -     | N     |         | [GNR.013](#GNR.013) |
| 14.0 | Uso Reservado para a Empresa               | 140   | 179   | 40    | -     | A     |         | [GNR.014](#GNR.014) |
| 15.0 | Uso Exclusivo Netunna                      | 180   | 300   | 121   | -     | A     |         | [GNR.004](#GNR.004) |

### Rodapé do Arquivo.

| ID   | Campo                                      | Dê    | Até   | Qnt.  | Decimal | Formato | Padrão | Descrição |
| ---  | ------                                     | ---:  | ---:  | ---:  | ---:  | :---: | ---     | ---     |
| 01.9 | Código da Empresa                          | 1     | 3     | 3     | -     | N     |         | [GNR.001](#GNR.001) |
| 02.9 | Lote de Serviço                            | 4     | 7     | 4     | -     | N     | 9999    | [GNR.002](#GNR.002) |
| 03.9 | Tipo de Registro                           | 8     | 8     | 1     | -     | N     | 9       | [GNR.003](#GNR.003) |
| 04.9 | Uso Exclusivo Netunna                      | 9     | 17    | 9     | -     | A     |         | [GNR.004](#GNR.004) |
| 05.9 | Quantidade de Lotes do Arquivo             | 18    | 23    | 6     | -     | N     |         | [GNR.015](#GNR.015) |
| 06.9 | Quantidade de Registros do Arquivo         | 24    | 29    | 6     | -     | N     |         | [GNR.016](#GNR.016) |
| 07.9 | Uso Exclusivo Netunna                      | 30    | 300   | 271   | -     | A     |         | [GNR.004](#GNR.004) |

# 3. Serviços

## 3.1 VENDAS: crédito (à vista rotativo, parcelado)</br> débito (à vista, pré-datado)

3.1.1 DESCRIÇÃO DO SERVIÇO

Objetivo 

O SERVIÇO VENDAS contempla as modalidades de CRÉDITO À VISTA (ROTATIVO),
CRÉDITO PARCELADO, DÉBITO À VISTA e DÉBITO PRÉ-DATADO e tem por objetivo
a integração de informações relacionadas às vendas diárias realizadas pela
empresa nestas modalidades.
...

### 3.1.2. VENDAS – REMESSA / RETORNO

3. Segmento V

| ID   | Campo                                      | Dê    | Até   | Qnt.  | Decimal | Formato | Padrão | Descrição |
| ---  | ------                                     | ---:  | ---:  | ---:  | ---:  | :---: | ---     | ---     |
| 01.2V | Código da Empresa                         | 1     | 3     | 3     | -     | N     |         | [GNR.001](#GNR.001) |
| 02.2V | Lote de Serviço                           | 4     | 7     | 4     | -     | N     |         | [GNR.002](#GNR.002) |
| 03.2V | Tipo de Registro                          | 8     | 8     | 1     | -     | N     | 2       | [GNR.003](#GNR.003) |
| 04.2V | Tipo de Serviço                           | 9     | 10    | 2     | -     | N     |         | [GNR.017](#GNR.017) |
| 05.2V | Número Sequencial do Registro no Lote     | 11    | 15    | 5     | -     | N     |         | [GNR.020](#GNR.020) |
| 06.2V | Código do Segmento do Registro Detalhe    | 16    | 16    | 1     | -     | A     | V       | [GNR.021](#GNR.021) |
| 07.2V | Uso Exclusivo Netunna                     | 17    | 17    | 1     | -     | A     |         | [GNR.004](#GNR.004) |
| 08.2V | Tipo de Inscrição Empresa                 | 18    | 18    | 1     | -     | N     |         | [GNR.005](#GNR.005) |
| 09.2V | Número de Inscrição da Empresa            | 19    | 34    | 14    | -     | N     |         | [GNR.006](#GNR.006) |
| 10.2V | Código da Loja                            | 33    | 37    | 5     | -     | A     |         | [GNR.018](#GNR.018) |
| 11.2V | Número do Estabelecimento na Adquirente   | 38    | 55    | 18    | -     | A     |         | [GNR.019](#GNR.019) |
| 12.2V | Número do Caixa/Checkout                  | 56    | 61    | 6     | -     | A     |         | [GNR.022](#GNR.022) |
| 13.2V | Número do PDV (Maquineta) na Adquirente   | 62    | 76    | 15    | -     | N     |         | [GNR.023](#GNR.023) |
| 14.2V | Número do Cupom Fiscal / Número do Pedido | 77    | 86    | 10    | -     | N     |         | [GNR.024](#GNR.024) |
| 15.2V | Uso Exclusivo Netunna                     | 87    | 96    | 10    | -     | A     |         | [GNR.004](#GNR.004) |
| 16.2V | Código da Autorização                     | 97    | 106   | 10    | -     | A     |         | [GNR.026](#GNR.026) |
| 17.2V | Data da Venda                             | 107   | 114   | 8     | -     | N     |         | [GNR.027](#GNR.027) |
| 18.2V | Hora da Venda                             | 115   | 120   | 6     | -     | N     |         | [GNR.028](#GNR.028) |
| 19.2V | Valor Bruto da Venda                      | 121   | 130   | 10    | 2     | N     |         | [GNR.029](#GNR.029) |
| 20.2V | Plano (Quantidade de Parcelas)            | 131   | 132   | 2     | -     | N     |         | [GNR.030](#GNR.030) |
| 21.2V | Código da Bandeira                        | 133   | 135   | 3     | -     | N     |         | [GNR.031](#GNR.031) |
| 22.2V | Número do Cartão (Truncado)               | 136   | 154   | 19    | -     | A     |         | [GNR.032](#GNR.032) |
| 23.2V | Nome do Proprietário do Cartão            | 155   | 179   | 25    | -     | A     |         | [GNR.033](#GNR.033) |
| 24.2V | Nome do Operador de Caixa                 | 180   | 204   | 25    | -     | A     |         | [GNR.034](#GNR.034) |
| 25.2V | Indicador Programa Promocional            | 205   | 205   | 1     | -     | A     |         | [GNR.035](#GNR.035) |
| 26.2V | Meio de Captura                           | 206   | 207   | 2     | -     | N     |         | [GNR.036](#GNR.036) |
| 27.2V | Número POS (Maquineta) Adquirente         | 208   | 222   | 15    | -     | N     |         | [GNR.037](#GNR.037) |
| 28.2V | Taxa de Comissão                          | 223   | 226   | 4     | 2     | N     |         | [GNR.038](#GNR.038) |
| 29.2V | Número NSU / CV ou DOC                    | 227   | 246   | 20    | -     | A     |         | [GNR.025](#GNR.025) |
| 30.2V | Uso Exclusivo Netunna                     | 247   | 300   | 54    | -     | A     |         | [GNR.004](#GNR.004) |

# 4. Descrição dos campos

## 4.1. DESCRIÇÃO DE CAMPOS – GENÉRICOS (GNR)

##### <a id="GNR.001">GNR.001</a> - Código da Empresa

Código que identifica a empresa no TEiACARD. Quando exista um  conjunto de empresas iniciar com código `001` e assim por diante consecutivamente.

##### <a id="GNR.002">GNR.002</a> - Lote de Serviço 

Número sequencial para identificar univocamente um lote de serviço. Criado e controlado pelo responsável pela geração magnética dos dados contidos no arquivo.
Preencher com `0001` para o primeiro lote do arquivo. Para os demais: número do lote anterior acrescido de 1. O número não poderá ser repetido dentro do arquivo.  
Se registro for Header do Arquivo preencher com `0000`
Se registro for Trailer do Arquivo preencher com `9999`

#### <a id="GNR.003">GNR.003</a> - Lote de Serviço 

Código adotado pela NETUNNA para identificar o tipo de registro.

Domínio:

| ID   | Campo                  |
| ---  | ---                    |
| 0    | Header de Arquivo      |
| 1    | Header de Lote         |
| 2    | Detalhe (Segmentos)    |
| 3    | Trailer de Lote        |
| 9    | Trailer de Arquivo     |

#### <a id="GNR.004">GNR.004</a> - Uso exclusivo netunna

Texto de observações destinado para uso exclusivo da NETUNNA.
Preencher com Brancos.

#### <a id="GNR.005">GNR.005</a> - Tipo de inscrição da empresa

Código que identifica o tipo de inscrição da Empresa ou Pessoa
Física perante uma Instituição governamental.

Domínio:

| ID   | Campo                  |
| ---  | ---                    |
| 0    | CPF                    |
| 1    | CGC / CNPJ             |

#### <a id="GNR.006">GNR.006</a> - Tipo de inscrição da empresa

Número de inscrição da Empresa ou Pessoa Física perante uma
Instituição governamental.

#### <a id="GNR.007">GNR.007</a> - Nome da Empresa

Nome que identifica a pessoa, física ou jurídica, a qual se quer fazer referência.

#### <a id="GNR.008">GNR.008</a> - Nome da Adquirente

Nome que identifica a Adquirente à qual pertencem as
informações que estão sendo tratadas no arquivo eletrônico.

Ver [Tabela III](#TabelaIII). 

#### <a id="GNR.012">GNR.012</a> - Número sequencial do arquivo (NSA) 

Número sequencial adotado e controlado pelo responsável pela geração do arquivo para ordenar a disposição dos arquivos encaminhados. Evoluir um número sequencial a cada header de arquivo 

#### <a id="GNR.031">GNR.031</a> - Código da Bandeira 

Código que identifica a Bandeira do Cartão de Crédito ou de Débito utilizado para a realização da venda.

Ver [Tabela I](#TabelaI). 

# 5. Tabelas

## 5.1. Objetivo 

Para uma melhor orientação ao usuário criamos neste tópico a lista de tabelas
objeto de identificar exatamente o campo que precise de uma definição mais
adequada.

## 5.2. <a id="TabelaI">Tabela I - Bandeiras</a>

| ID   | Nome | 
| ---:  | ---  |
| 001 | VISA | 
| 002 | MASTERCARD | 
| 003 | AMERICAN EXPRESS | 
| 004 | HIPERCARD | 
| 005 | DINERS | 
| 006 | ELO | 
| 007 | AGIPLAN | 
| 008 | BANESCARD | 
| 009 | CABAL | 
| 010 | CREDSYSTEM | 
| 011 | ESPLANADA | 
| 012 | CREDZ | 
| 013 | SOROCRED | 
| 014 | HIPER | 
| 015 | DISCOVER | 
| 016 | AURORA | 
| 017 | COOPERCRED | 
| 018 | SICRED | 
| 019 | AVISTA | 
| 020 | MAIS! | 
| 021 | UNIONPAY | 
| 022 | ALELO | 
| 023 | GOODCARD | 
| 024 | JCB | 
| 025 | SODEXO | 
| 026 | BONUS CBA | 
| 027 | SAPORE | 
| 028 | POLICARD | 
| 029 | VEROCHEQUE | 
| 030 | GREENCARD | 
| 031 | VALECARD | 

## 5.3. <a id="TabelaII">Tabela II - Meios de Captura</a>

| ID   | Nome | 
| ---: | ---  |
| 001  | POS – POINT OF SALES | 
| 002  | PDV – PONTO DE VENDA OU TEF (TRANSFERENCIA ELETRÔNICA DE FUNDOS | 
| 003  | E-COMMERCE (COMÉRCIO ELETRÔNICO) | 
| 004  | EDI – ELETRONIC DATA INTERCHANGE | 
| 005  | ADP/BSP – EMPRESA CAPTURADORA | 
| 006  | MANUAL | 
| 007  | URA / CVA | 
| 008  | MOBILE | 
| 009  | TO/SenffOffice | 
| 010  | MOEDEIRO ELETRÔNICO EM REDE | 
| 011  | CIELO POS (AMEX) | 
| 012  | CIELO TEF (AMEX) | 
| 013  | CIELO E-COMMERCE (AMEX) | 
| 014  | REDE POS (AMEX) | 
| 015  | REDE TEF (AMEX) | 
| 016  | REDE E-COMMERCE (AMEX) | 
| 017  | GETNET POS (AMEX) | 
| 018  | GETNET TEF (AMEX) | 
| 019  | GETNET E-COMMERCE (AMEX) | 
| 020  | POS ISSO (BIN) | 
| 021  | POS GPRS Mobile (BIN) | 
| 022  | Mobile POS | 
| 023  | POS Desktop (BIN) | 
| 024  | POS Eth + Desktop (BIN) | 
| 025  | POS GPRS Desktop (BIN) | 
| 026  | POS GPRS Dt+Dial+Eth (BIN) | 
| 027  | TEF PINPAD USB (BIN) | 
| 028  | TEF Dial discado (BIN) | 
| 029  | TEF dedicada (BIN) | 
| 030  | TEF IP (BIN) | 
| 031  | E-Commerce Gateway 1 (BIN) | 
| 032  | E-Commerce Gateway 2 (BIN) | 
| 033  | E-Commerce Gateway 3 (BIN) | 
| 034  | E-Commerce Gateway 4 (BIN) | 
| 035  | E-Commerce Gateway 5 (BIN) | 
| 036  | E-Commerce Subadquirente (BIN) | 
| 037  | E-Commerce IPG (BIN) | 

## 5.4. <a id="TabelaIII">Tabela III - Adquirentes</a>

| ID    | Nome | 
| ---:  | ---  |
|       | CIELO | 
|       | REDE | 
|       | AMEX | 
|       | GETNET SANTANDER | 
|       | VERO BANRISUL | 
|       | LOSANGO | 
|       | ELAVON | 
|       | STONE | 
|       | CALCARD | 
|       | ADYEN | 
|       | SENFF | 
|       | FIRST DATA - BIN | 
|       | GLOBAL PAYMENTS | 
|       | SODEXO (BENEFÍCIOS) | 
|       | TICKET (BENEFÍCIOS) | 
|       | VR (BENEFÍCIOS) | 
|       | GOODCARD | 
|       | GREENCARD | 
|       | BIQ (BENEFÍCIOS) | 
|       | PERSONAL (BENEFÍCIO) | 
|       | TRIO (BENEFÍCIOS) | 
|       | VEROCHEQUE | 
|       | VEGA (BENEFÍCIOS) | 
|       | FACISC | 
|       | SINCOMERCIO | 

## 5.5. <a id="TabelaIV">Tabela IV - Motivos de estorno (cancelamento)</a>

| ID   | Nome | 
| ---: | ---  |
| 01   | TROCA | 
| 02   | DEVOLUÇÃO PARCIAL | 
| 03   | DEVOLUÇÃO TOTAL | 
| 04   | FALTA DE MERCADORIA | 
| 05   | CANCELAMENTO DE PEDIDO | 
| 06   | EXTRAVIO / ROUBO | 
| 07   | INDENIZAÇÃO FRETE | 
| 08   | ESTORNO PROMOÇÕES | 
| 09   | VENDA AUTORIZADA PELA ADQUIRENTE EM DUPLICIDADE | 
| 10   | VENDA CANCELADA POR ERRO DE COMUNICAÇÃO NA AUTORIZAÇÃO | 