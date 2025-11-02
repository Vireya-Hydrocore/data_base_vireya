Sistema de Gestão de ETAs e Processos de Tratamento de Água

Este projeto contém o script completo para criação e populamento de um banco de dados PostgreSQL que gerencia ETAs (Estações de Tratamento de Água), funcionários, processos, estoque de produtos químicos e relatórios.

1. Estrutura do Banco

O banco é composto pelas seguintes tabelas principais:

Tabela	Descrição
cargo	Lista de cargos e níveis de acesso dos funcionários.
endereco	Endereços das ETAs.
eta	Estaçōes de tratamento de água, com capacidade e endereço.
funcionario	Funcionários, com cargos, vinculação a ETA e hierarquia (supervisor).
eta_admin	Relação de administradores de cada ETA.
comentario_gerente	Comentários dos gerentes sobre ETAs.
prioridade	Níveis de prioridade para tarefas e avisos.
status	Status de tarefas e avisos.
tarefa	Tarefas atribuídas aos funcionários.
avisos	Avisos e alertas das ETAs.
processo	Processos de tratamento de água.
relatorio	Relatórios finais de cada processo.
unidade_medida	Unidades de medida de produtos.
produto	Produtos químicos e equipamentos utilizados.
uso_produto	Uso de produtos nos processos.
estoque	Controle de estoque por ETA.
plano	Planos de pagamento para serviços.
pagamento	Pagamentos realizados pelos planos.
log_geral	Logs automáticos de alterações em tabelas importantes.
2. Funções e Procedures

O banco possui funções e procedures para automatizar regras de negócio:

2.1 Funções

log_automatico(): Registra inserções, atualizações e deleções na tabela log_geral.

calcular_data_validade(): Calcula automaticamente a validade do plano no momento do pagamento.

atualizar_estoque(): Atualiza o estoque de produtos ao registrar seu uso em processos.

relatorio_funcionarios(p_id_funcionario): Retorna dados e tarefas de um funcionário ou de todos.

resumo_tarefas_usuario(p_id_funcionario): Retorna contagem de tarefas de um funcionário.

resumo_tarefas_eta(p_id_eta): Retorna contagem de tarefas de uma ETA.

gerar_relatorio_mes(mes, ano): Retorna relatório mensal por ETA, incluindo volume tratado e comentários.

produtos_usados_mes(mes, ano): Retorna a quantidade de cada produto usado por ETA no mês.

organograma_eta(p_id_eta): Retorna hierarquia de funcionários de uma ETA.

verificar_relatorio_processo(): Garante que a data do relatório não seja anterior à data do processo.

relatorio_comentario_gerente(p_id_eta, mes, ano): Retorna relatórios de uma ETA com comentários do gerente.

data_relatorio_eta(p_id_eta): Retorna datas de relatórios distintos de uma ETA.

2.2 Procedures

adicionar_estoque(p_id_produto, p_id_eta, p_quantidade): Adiciona produtos ao estoque de uma ETA.

tirar_estoque(p_id_produto, p_id_eta, p_quantidade): Remove produtos do estoque, validando quantidade disponível.

adicionar_tarefa(p_descricao, p_id_prioridade, p_id_funcionario, p_id_status): Adiciona uma tarefa para um funcionário.

3. Triggers

Atualizam estoque automaticamente ao usar produtos (trg_atualizar_estoque).

Calculam validade do plano ao inserir pagamento (trg_calcular_data_validade).

Verificam datas de relatórios (trg_verificar_relatorio_processo).

Log automático para todas as tabelas críticas (trg_log_automatico_*).

4. Índices

Índices criados para otimizar consultas:

cargo(nome)

funcionario(email)

relatorio(data_processo_final)

estoque(quantidade)

funcionario(id_pai)

Sugestão de índices adicionais para performance em grandes volumes: uso_produto(id_processo), uso_produto(id_produto), eta_admin(id_eta), eta_admin(id_funcionario), processo(id_eta).
