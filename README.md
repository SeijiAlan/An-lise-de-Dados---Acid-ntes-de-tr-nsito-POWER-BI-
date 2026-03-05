# Análise de Dados - Acidentes de trânsito (RENAEST).
Este projeto tem como objetivo analisar dados públicos de acidentes de trânsito disponibilizados pela Secretaria Nacional de Trânsito (SENATRAN), por meio da base do Registro Nacional de Acidentes e Estatísticas de Trânsito (RENAEST).

A análise foi desenvolvida no Power BI com foco na construção de indicadores confiáveis e na validação da consistência dos dados antes da geração de insights.

🎯 Objetivo
Calcular o total de acidentes no período de 2018 a 2025,
Identificar o total de óbitos,
Identificar o total de feridos/ilesos,
Calcular a taxa média de mortalidade por ocorrência,
Validar a integridade da base antes da análise.

💻 Ferramentas Utilizadas
Power BI,
DAX (Data Analysis Expressions),
Base pública do RENAEST.

🧠Etapa de Validação dos Dados.
1.Antes de criar os indicadores, foi feita uma verificação da estrutura da base:
2.Confirmou-se que cada linha representa uma ocorrência individual
3.Atestou-se que a coluna qtde_acidente possui valor máximo igual a 1
4.Utilizou-se DISTINCTCOUNT no identificador do acidente para evitar duplicidades
5.Verificada a ausência de valores nulos relevantes na chave da ocorrência
6.Essa etapa foi essencial para garantir que os resultados não estivessem inflados por duplicações ou agregações indevidas.

📌 Métricas Desenvolvidas
O Arquivo utilizado está disponível no endereço:https://dados.transportes.gov.br/dataset/renaest? e o arquivo utilizado foi o "RENAEST-Mensal-10-2025". 

Sobre a tabela "Vitimas_DadosAbertos_20260212", a coluna relacionada a chave da ocorrência foi nomeada para "Código_acidente"
Foram utilizadas as seguintes medidas (Consultas DAX):

1.Total de ocorrências:
Quantidade de ocorrências =
DISTINCTCOUNT(Acidentes_DadosAbertos_20260212[Código_acidente])

2.Cálcular o Estado com maior Nº de ocorrências (Estático):
Estado com maior Nº de acidentes = 
VAR TabelaResumo =
    SUMMARIZE(
        ALL(Vitimas_DadosAbertos_20260212),
        Vitimas_DadosAbertos_20260212[uf_acidente],
        "TotalAcidentes",
        DISTINCTCOUNT(Vitimas_DadosAbertos_20260212[Código_acidente])
    )
VAR Maior =
    TOPN(
        1,
        TabelaResumo,
        [TotalAcidentes],
        DESC
    )
RETURN
    MAXX(Maior, Vitimas_DadosAbertos_20260212[uf_acidente])

3.Estado com maior Nº de Frota circulante:
Estado c/ maior frota circulante = 
VAR TabelaResumo =
    SUMMARIZE(
        ALL(Localidade_DadosAbertos_20260212),
        Localidade_DadosAbertos_20260212[uf],
        "TotalFrota",
        SUM(Localidade_DadosAbertos_20260212[frota_circulante])
    )
VAR Maior =
    TOPN(
        1,
        TabelaResumo,
        [TotalFrota],
        DESC
    )
RETURN
    MAXX(Maior, Localidade_DadosAbertos_20260212[uf])

  4.Quantidade de ocorrências = 
DISTINCTCOUNT(Acidentes_DadosAbertos_20260212[Código_acidente])

Além disso, devido a granda quantidade de dados presentes nas tabelas, utilizou-se a seguinte medida para confirmar que cada ocorrência está relacionada a 1 acidente: 

Valor Máximo Qtde =
MAX(Acidentes_DadosAbertos_20260212[qtde_acidente])


📈 Conclusão
O volume de acidentes no período analisado ultrapassa 8 milhões de registros, a média de vítimas por acidente é de aproximadamente 1,46 pessoas, sendo que a taxa média de mortalidade ficou próxima de 2% (2 óbitos a cada 100 casos).O Estado de Sâo Paulo é o que possui maior quantidade de frota circulante, enquanto que o Estado de Minas Gerais é o que apresenta maior quantidade de ocorrências. A validação da granularidade foi um ponto importante do processo para garantir confiabilidade nos indicadores.A maioria das ocorrências foram sexta-feira à noite sendo 47% dos casos envolvendo automóveis (Conforme definição pelo Código de trânsito brasileiro),o período soma 187 mil óbitos no total sendo a maioria dos registros dando como causa desconhecida enquanto que a Colisão aparece como a 2º maior causa conhecida, a maioria dos óbitos são de vítimas do sexo masculino (Pouco mais de 150 mil).


📚 Aprendizados do Projeto

Durante o desenvolvimento deste dashboard, foi possível aplicar conceitos de Modelagem básica de dados , Construção de medidas em DAX, Validação de consistência de base e interpretação de métricas e indicadores. Este projeto faz parte do meu processo de evolução na área de Análise e Desenvolvimento de Sistemas, com foco em análise de dados e construção de dashboards.


