# Miniprojeto_Rooseveltfreire_t8
Analise de dado da  Base Varejo . csv

 Relatório de Problemas Tratados, Decisões e Justificativas

 Problema Encontrado  Decisão Adotada  Justificativa Técnica  Regra de Negócio  Status
 Colunas extras nulas (Unnamed: 10 a 13) Remoção das colunas Eram colunas corrompidas, importadas sem nome e preenchidas com 100\% de valores vazios. Não continham nenhuma informação útil.  Tratado 
 Inconsistência textual (#N/D na categoria de produto PR_CAT) | Substituição por NAO DEFINIDO O termo #N/D é um erro de exportação padrão (geralmente do Excel). Substituir por um rótulo textual limpo como NAO DEFINIDO preserva a linha na tabela e permite uma análise sem quebrar filtros de classificação futuros. Tratado 
 Espaços nas bordas e letras minúsculas (CL_GENERO, CL_SEG, PR_CAT, PR_NOME)  Remoção de espaços (strip) e conversão para maiúsculo (upper) Inconsistências de digitação (ex: "alimentos" vs "Alimentos " vs "ALIMENTOS") criam categorias fantasmas no agrupamento do pandas. A padronização une registros idênticos sob um único padrão legível. Tratado 
 Linhas Duplicadas Absolutas (96.553 ocorrências) Remoção completa via `drop_duplicates() Linhas onde todos os valores (incluindo IDs, datas e produtos) são idênticos são redundâncias operacionais geradas por erros de sistema ou duplicação de lote. Sua permanência distorce médias e volumes reais de venda. Tratado 
 Compras repetidas em momentos diferentes (Mesmo cliente / Produto distinto) Mantidos no DataFrame Se um mesmo cliente realiza compras diferentes de produtos diferentes ou em datas diferentes, são transações autênticas de consumo e representam a realidade do negócio. Não foram descartadas.  Mantido (Justificado)

 Explicação das Métricas e Medidas

Para interpretar corretamente estes agrupamentos, é fundamental não confundir as diferentes entidades de nossa base de dados:

 O que cada agrupamento acima mede?
  Agrupamento por Categoria: Mede a quantidade absoluta de registros de itens (linhas na tabela) que foram levados nas cestas destas famílias. Por exemplo, se uma linha indica ALIMENTOS, significa que um item pertencente a alimentos foi adicionado em alguma compra.
  Agrupamento por Gênero: Mede o volume de itens associados a cadastros de clientes daquele gênero específico dentro deste grupo filtrado.

Diferenciando as Entidades para Evitar Confusões:
  Registros de Itens (Linhas): Cada linha representa a inclusão de um produto em uma compra.O número total obtido nas tabelas acima (ex: Qtd_Registros_Itens) representa esse volume de itens faturados, e não o número de compras ou clientes individuais.
  Compras (Transações / CO_ID): Uma única compra (CO_ID) pode conter múltiplos itens (várias linhas no nosso dataset). Para contar o número de transações reais, precisaríamos contar os IDs de compra únicos (.nunique()).
  Clientes (CL_ID): Um cliente cadastrado pode realizar várias compras ao longo do tempo. Para sabermos quantos consumidores únicos com filhos temos na base, deveríamos contar os CL_ID exclusivos.
  Faturamento: Como nossa base não contém colunas de preços ou valores monetários monetizados das compras nesta tabela, não podemos estimar o faturamento financeiro diretamente aqui, apenas o volume físico de movimentação de produtos.

 Explicação e Interpretação dos Resultados

 Unidade Analisada: A unidade de análise desta coluna é a quantidade de pessoas por dependentes que compõem o núcleo familiar do cliente cadastrado.

 Explicação de 2 Resultados Obtidos:
   Média 1,15: O valor médio de aproximadamente 1,15 indica que, na média geral da base de clientes ativos, cada cadastro possui entre 1 e 2 dependentes familiares associados. Isso sugere que o público-alvo principal do varejo possui famílias de pequeno a médio porte.
  
   Mínimo 0 e Máximo 4: O valor mínimo mostra a presença de clientes com perfil individual (sem filhos/dependentes registrados na base), enquanto o limite superior máximo é de até 4 dependentes por núcleo familiar nesta amostra, indicando um teto claro nos registros de segmentação da empresa.
