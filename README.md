# Análise de Redes de Proteínas Associadas à Doença de Alzheimer

## Integrantes

Edson Maciel Júnior

## Vídeo

*Link do vídeo será adicionado após a gravação.*

## Problema e pergunta

A doença de Alzheimer está associada a alterações em diferentes proteínas. A teoria dos grafos permite representar as interações entre essas proteínas como uma rede e analisar suas posições estruturais.

**Pergunta de pesquisa:** Entre as proteínas associadas à doença de Alzheimer, quais ocupam posições estruturais importantes na rede, atuando como hubs, pontes entre grupos ou integrantes de núcleos densos?

## Dados

Os dados serão obtidos a partir de fontes públicas de associações entre genes e doença e de interações funcionais entre proteínas.

### Fonte 1 — Genes associados à doença de Alzheimer

* **Plataforma:** Open Targets Platform.
* **Doença:** Alzheimer disease.
* **Identificador da doença:** `MONDO_0004975`.
* **Página dos dados:** https://platform.opentargets.org/disease/MONDO_0004975/associations
* **Data de acesso:** 14/10/2026.
* **Critério de seleção:** seleção dos 200 genes com os maiores valores de `Association Score`.

O conjunto selecionado será salvo no arquivo `dados/genes_alzheimer.csv`.

### Fonte 2 — Interações entre proteínas

* **Plataforma:** STRING Database.
* **Página da fonte:** https://string-db.org/
* **Finalidade:** obter as interações funcionais entre as proteínas correspondentes aos genes selecionados.

Os procedimentos de coleta, os critérios de filtragem e as etapas de limpeza serão documentados durante o desenvolvimento do projeto.

## Modelagem da rede

* **Nós:** proteínas associadas à doença de Alzheimer.
* **Arestas:** interações funcionais entre proteínas, conforme os dados do STRING.
* **Peso:** pontuação de confiança da interação, disponibilizada pelo STRING.
* **Direção:** grafo não dirigido.
* **O que fica de fora:** contexto específico de tecidos, distinção detalhada entre tipos de interação e proteínas não incluídas no conjunto selecionado.

## Conteúdo do curso utilizado

| Conceito                    | Semana | Onde aparece | Para que serviu                                                 |
| --------------------------- | ------ | ------------ | --------------------------------------------------------------- |
| Grau e distribuição de grau | 2      | A definir    | Identificar proteínas com muitas conexões                       |
| Densidade                   | 2      | A definir    | Avaliar a conectividade geral da rede                           |
| Componentes conexos         | 4      | A definir    | Verificar a conectividade da rede                               |
| Betweenness                 | 4      | A definir    | Identificar proteínas que conectam diferentes regiões           |
| Clustering e triângulos     | 4      | A definir    | Avaliar a coesão local                                          |
| Centralidade de eigenvector | 4      | A definir    | Identificar proteínas conectadas a outras proteínas importantes |
| K-core e core number        | 5      | A definir    | Identificar regiões densamente conectadas                       |

## Como executar

As instruções de instalação e execução serão adicionadas após a implementação e os testes do notebook.

## Resultados

Os resultados serão apresentados após a coleta dos dados e a análise da rede.

## Limitações e próximos passos

A rede representa as interações registradas nas fontes utilizadas e não comprova, por si só, relações causais ou a importância clínica de uma proteína.

Também será avaliado como diferentes limiares de confiança das interações alteram a estrutura da rede e os resultados das centralidades.

## Referências

* STRING Database: https://string-db.org/
* Open Targets Platform: https://platform.opentargets.org/
