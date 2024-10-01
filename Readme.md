# Integrantes do Grupo

#### André
#### Felipe Cortez | RM9950
#### Julia Azevedo Lins | RM 98960
#### Luis Gustavo Barreto Garrido | RM99210
#### Victor Hugo Aranda Forte | RM99667

<hr>

# Plataforma de Jogos para Coordenação Motora em Medicina

## Visão Geral

Este projeto desenvolve uma plataforma de jogos em realidade virtual voltada para a prática da coordenação motora de médicos, especificamente em videolaparoscopia. A plataforma inclui um sistema de gerenciamento de dados, utilizando um banco de dados Oracle para armazenar informações sobre o desempenho dos alunos e as notas de suas atividades. Pensando nisso, esse código tem como intuito facilitar a visualização do responsável pelas notas dos alunos que estão utilizando os cursos de nossa plataforma e ver seus respectivos desempenhos.

## Área Cadastro
Onde os usuarios de nosso sistema se cadastram, tanto alunos quanto professores

## Área Professores
Os professores possuem funcionalidades como cadastro, login e análise de desempenho dos alunos.

## Área Alunos
Os alunos também possuem funcionalidades como cadastro, login e verificação de suas notas no sistema.

## Funcionalidades

### Cadastro e Login de Responsáveis

- **Sistema de Cadastro:** Permite o registro de responsáveis com informações básicas.
- **Autenticação:** Sistema de login seguro para acesso à plataforma.

### Análise de Desempenho

- **Cadastro de Notas:** Registro das notas dos alunos em diferentes atividades.
- **Análise de Dados:** Geração de gráficos que visualizam o desempenho dos alunos.
- **Projeções Futuras:** Uso da técnica de Monte Carlo para prever futuras notas da turma.

## Estrutura do Banco de Dados

O banco de dados Oracle é projetado para armazenar e gerenciar as seguintes informações:

- **Usuários e Responsáveis:** Tabelas para cadastro e autenticação.
- **Notas e Desempenho:** Tabelas para o registro de notas e análise de desempenho.

## Uso da Técnica de Monte Carlo

A técnica de Monte Carlo é aplicada para projetar o desempenho futuro dos alunos com base nas notas passadas. Isso inclui:

- **Simulação de Notas:** Geração de cenários futuros para avaliar a precisão das projeções.
- **Análise e Visualização:** Gráficos detalhados com explicações das projeções.

## Criação das Tabelas no Oracle SQL Developer

```sql
CREATE SEQUENCE seq_professor_id
START WITH 1
INCREMENT BY 1;

-- Tabela USUARIOS
CREATE TABLE USUARIOS(
    ID_USUARIO NUMERIC(10) CONSTRAINT PK_USUARIOS PRIMARY KEY NOT NULL,
    NM_USUARIO VARCHAR(100) NOT NULL,
    DS_EMAIL VARCHAR(60) NOT NULL,
    DS_ESPECIALIDADE VARCHAR(30) NOT NULL,
    NR_NIVEL_EXPERIENCIA NUMERIC(10) NOT NULL,
    NR_IDADE NUMERIC(3) NOT NULL,
    DS_LOCALIZACAO VARCHAR(50) NOT NULL,
    DT_NASCIMENTO DATE NOT NULL,
    DT_CADASTRO DATE DEFAULT SYSDATE,
    PS_USUARIO VARCHAR(300) NOT NULL,
    CONSTRAINT VALID_DATA_NASC CHECK ( DT_NASCIMENTO < DT_CADASTRO)
);

-- Criação da tabela Cadastro_Professor
CREATE TABLE Cadastro_Professor (
    ID_PROFESSOR NUMBER(10) CONSTRAINT PK_PROFESSOR PRIMARY KEY NOT NULL,
    NM_PROFESSOR VARCHAR2(100) NOT NULL,
    LOGIN VARCHAR2(50) NOT NULL UNIQUE,
    SENHA VARCHAR2(50) NOT NULL
);

-- Criação da tabela Historico_Notas
CREATE TABLE Historico_Notas (
    DATA DATE,
    NOME_ALUNO VARCHAR2(200),
    MATERIA VARCHAR2(100),
    PERIODO VARCHAR2(10) CONSTRAINT VALIDACAO_PERIODO CHECK (PERIODO IN ('Manhã', 'Noite')),
    MEDIAS_DOS_ALUNOS NUMBER(5, 2)
);

```

## BLOCO 1 - Cadastro de Professor

Este código implementa uma interface para registrar professores em um banco de dados Oracle. Ele usa a biblioteca ipywidgets para criar campos interativos de entrada e um botão de registro. A função register_professor realiza a inserção dos dados fornecidos (nome, usuário e senha) na tabela Cadastro_Professor no banco de dados Oracle.

Quando o botão de registro é clicado, os valores inseridos nos campos são capturados e a função realiza a inserção no banco. Caso o nome de usuário já exista, o sistema exibe uma mensagem de erro apropriada. Caso contrário, o registro é concluído com sucesso.


## BLOCO 2 - Login de Usuário

Este código implementa uma interface de login para o sistema, conectando-se a um banco de dados Oracle. Ele utiliza ipywidgets para criar uma interface interativa com campos para nome de usuário, senha e um botão de login.

A senha é protegida utilizando a função de hash SHA-256 antes de ser enviada ao banco de dados, garantindo que a senha real nunca seja armazenada diretamente. O sistema verifica se o nome de usuário e a senha correspondem a um registro no banco de dados.

Quando o botão de login é clicado, a função login valida as credenciais. Caso as informações estejam corretas, o login é realizado com sucesso; caso contrário, uma mensagem de erro é exibida.

# --------------------- "BLOCO DO PROFESSOR" ---------------------

## BLOCO 3 - Inserção de Dados no Histórico de Notas

Este código permite a inserção de dados na tabela Historico_Notas do banco de dados Oracle, utilizando uma interface gráfica com ipywidgets. Ele recebe as informações do aluno, como data, nome, matéria, período e média, e insere esses dados no banco de dados.

A função insert_historico_notas é responsável por conectar-se ao banco de dados e inserir um novo registro na tabela. O botão de inserção chama a função e verifica se todos os campos estão devidamente preenchidos antes de realizar a operação.

Caso ocorra um erro de banco de dados, uma mensagem de erro será exibida.


## BLOCO 4 - Visualização das Médias dos Alunos

Este código utiliza Matplotlib e Pandas para gerar um gráfico de barras que exibe as médias dos alunos ao longo do tempo, baseado nos dados da tabela Historico_Notas. Ele extrai os dados de DATA e MEDIAS_DOS_ALUNOS do banco de dados Oracle e agrupa as médias por data.

O resultado final é uma visualização gráfica, onde o eixo X mostra as datas e o eixo Y exibe as médias dos alunos.

O gráfico é customizado com rótulos formatados para datas, espaçamento adequado e rotação para melhor legibilidade.


## BLOCO 5 - Comparação de Médias de Alunos (Manhã vs Noite)

Este código extrai os dados da tabela Historico_Notas até a data limite de 01/12/2023 e gera um gráfico comparativo entre os períodos da manhã e noite. As médias das notas dos alunos são agrupadas por data e comparadas visualmente em uma linha de tempo.

A separação entre os períodos é feita utilizando o campo PERIODO da tabela, e o gráfico gerado exibe duas linhas: uma para os alunos do período da manhã e outra para os do período da noite. O gráfico inclui formatação para as datas e uma legenda clara para distinguir os períodos.


## BLOCO 6 - Média das Notas dos Alunos por Matéria ao Longo do Tempo
Este código extrai dados da tabela Historico_Notas para visualizar a média das notas dos alunos agrupadas por matéria ao longo do tempo. A seguir, é gerado um gráfico de linhas onde cada linha representa uma matéria específica, mostrando como a média das notas evolui ao longo das datas disponíveis.

Detalhes:
Conexão com o Banco de Dados: Estabelece uma conexão com o banco de dados Oracle e executa uma consulta SQL para obter as colunas DATA, MEDIAS_DOS_ALUNOS e MATERIA.
Processamento dos Dados: Converte os dados para um DataFrame, garante que a coluna DATA esteja no formato datetime e a coluna MEDIAS_DOS_ALUNOS seja numérica. Linhas com valores NaN são removidas.
Agrupamento e Plotagem: Agrupa os dados por DATA e MATERIA, calcula a média das notas e plota um gráfico de linhas para cada matéria. O gráfico inclui uma legenda para distinguir as diferentes matérias e formata os rótulos das datas para melhorar a legibilidade.

## BLOCO 7 - Teste de campo
Utilizando o protótipo do jogo desenvolvido por nossa equipe, cronometramos o tempo para avaliar nosso desempenho e medir o progresso. Com base nos resultados, calculamos uma média de progresso de aproximadamente 9%. Esse valor foi utilizado para prever as notas dos alunos por meio do nosso sistema.

<video width="600" controls>
  <source src="Jogo.mp4" type="video/mp4">
  Seu navegador não suporta a exibição de vídeos.
</video>



## BLOCO 8 - Previsão das Médias das Notas dos Alunos até 2025 (Simulação de Monte Carlo)
Este código realiza uma simulação de Monte Carlo para prever as médias das notas dos alunos até 2025, baseando-se nos dados de 2022. A análise é dividida em períodos da manhã, noite e geral. A seguir estão os detalhes de cada etapa:

Detalhes:
Conexão e Consulta:

Conecta ao banco de dados Oracle e executa uma consulta SQL para buscar dados das médias das notas de 2022, filtrando por períodos (manhã e noite).
Processamento dos Dados:

Converte os dados para um DataFrame.
Garante que a coluna DATA esteja no formato datetime e que MEDIAS_DOS_ALUNOS seja numérica.
Remove linhas com valores NaN e separa os dados por períodos (manhã, noite e geral).
Simulação de Monte Carlo:

Calcula a média e o desvio padrão das notas para 2022 em cada período.
Realiza simulações de Monte Carlo para prever as médias das notas até 2025. Usa distribuições normais baseadas nas médias e desvios padrões calculados.
Plotagem dos Resultados:

Plota gráficos de linhas para cada período (manhã, noite e geral) mostrando as simulações e a média simulada.
Inclui preenchimento entre os valores mínimos e máximos das simulações para visualizar a faixa de previsões.
Gráficos:
Manhã: Exibe a previsão das médias das notas dos alunos no período da manhã até 2025.
Noite: Exibe a previsão das médias das notas dos alunos no período da noite até 2025.
Geral: Exibe a previsão das médias das notas dos alunos considerando todos os períodos.
Os gráficos ajudam a visualizar a variabilidade das previsões e a tendência esperada das médias das notas dos alunos para os próximos anos.


# ------------------------ BLOCO DO ALUNO ------------------------


## BLOCO 9 - Consulta de Notas dos Alunos
Este bloco de código cria uma interface interativa para consultar a nota de um aluno em um banco de dados Oracle, utilizando widgets para entrada e exibição dos dados. Abaixo estão os detalhes e etapas envolvidas:

Detalhes:
Conexão e Consulta:

Conexão ao Banco de Dados: Estabelece uma conexão com o banco de dados Oracle usando as credenciais fornecidas.
Consulta SQL: Executa uma consulta SQL para buscar a nota (MEDIAS_DOS_ALUNOS) e a data (DATA) associadas ao nome do aluno (NOME_ALUNO).
Processamento dos Dados:

Obtenção dos Resultados: Recupera o resultado da consulta e verifica se há dados disponíveis para o aluno.
Exibição dos Dados: Se a nota for encontrada, exibe a data e a nota. Caso contrário, informa que não há notas para o nome fornecido.
Widgets:

Entrada de Dados: Utiliza um widget de texto para o usuário inserir o nome do aluno.
Botão de Ação: Adiciona um botão para acionar a consulta.
Mensagens de Erro: Inclui um widget de label para exibir mensagens de erro se o campo do nome estiver vazio ou se ocorrer um erro durante a consulta.
Gráficos:
Exibição das Notas:
Interface Interativa: Permite ao usuário consultar e visualizar a nota de um aluno específico, incluindo a data da nota.
Mensagens de Erro: Se o campo de nome do aluno estiver vazio ou se não houver dados disponíveis para o nome fornecido, uma mensagem de erro será exibida.

## BLOCO 10 - Consulta de Descrições dos Jogos
Este bloco de código configura uma interface interativa para consultar e exibir informações sobre jogos a partir de um banco de dados Oracle. Abaixo estão os detalhes e etapas envolvidas:

Detalhes:
Conexão e Consulta:

Conexão ao Banco de Dados: Estabelece uma conexão com o banco de dados Oracle usando as credenciais fornecidas.
Consulta SQL: Executa uma consulta SQL para buscar descrições detalhadas de um jogo específico, identificadas pelo nome (NOME).
Processamento dos Dados:

Obtenção dos Resultados: Recupera todos os resultados da consulta, que incluem o ID do jogo, tipo, dificuldade, data de lançamento, descrição e nome.
Exibição dos Dados: Se o jogo for encontrado, exibe os resultados em formato de tabela HTML utilizando a biblioteca tabulate. Caso contrário, informa que o jogo não foi encontrado.
Widgets:

Entrada de Dados: Utiliza um widget de texto para o usuário inserir o nome do jogo.
Botão de Ação: Adiciona um botão para acionar a consulta.
Mensagens de Erro: Inclui um widget de label para exibir mensagens de erro se o campo do nome estiver vazio ou se ocorrer um erro durante a consulta.
Gráficos:
Exibição das Descrições dos Jogos:
Interface Interativa: Permite ao usuário consultar e visualizar informações detalhadas sobre um jogo específico.
Mensagens de Erro: Se o campo de nome do jogo estiver vazio ou se não houver dados disponíveis para o nome fornecido, uma mensagem de erro será exibida.