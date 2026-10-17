# Análise de Redes de Proteínas Associadas à Doença de Alzheimer

Projeto da Unidade 01 de Algoritmos e Estrutura de Dados II (Grafos), UFRN.

## Integrantes

Edson Maciel de Barros Junior

## Vídeo

> **PENDENTE:** colar aqui o link do YouTube (vídeo não listado, até 15 minutos, com a câmera aberta durante todo o vídeo).

https://youtu.be/...

## Problema e pergunta

A doença de Alzheimer está associada a alterações em diversas proteínas. Representar as interações entre essas proteínas como uma rede permite analisar a posição estrutural de cada uma e apontar candidatas a investigação.

**Pergunta de pesquisa:** entre as proteínas associadas à doença de Alzheimer, quais ocupam posições estruturais importantes na rede, atuando como hubs, pontes entre grupos ou integrantes de núcleos densos?

**Quem se beneficia:** pesquisadores que precisam priorizar proteínas-alvo para estudo, a partir de uma leitura estrutural da rede.

## Dados

### Fonte 1: genes associados ao Alzheimer

* **Plataforma:** Open Targets Platform
* **Doença:** Alzheimer disease (`MONDO_0004975`)
* **Página:** https://platform.opentargets.org/disease/MONDO_0004975/associations
* **Data de acesso:** 14/10/2026
* **Critério de seleção:** os 200 genes com maior `Association Score`
* **Arquivo:** `dados/genes_alzheimer.csv` (coluna `symbol` usada como identificador)

### Fonte 2: interações entre proteínas

* **Plataforma:** STRING Database (https://string-db.org/)
* **Obtenção:** API `network` do STRING, espécie humana (`species=9606`), enviando os 200 genes da Fonte 1. O notebook faz o download automaticamente.
* **Limiares de confiança testados** (`required_score`, escala 0 a 1000): 400 (médio), 700 (alto) e 900 (altíssimo).
* **Arquivos gerados:** `dados/string_400.tsv`, `dados/string_700.tsv`, `dados/string_900.tsv`
* **Data de acesso:** 14/10/2026 *(ajustar para a data em que o notebook foi executado)*

### Limpeza e tratamento

* Seleção dos 200 primeiros genes únicos, sem valores vazios.
* Conversão do `score` para número; linhas sem nome de proteína ou score são descartadas.
* O `score` devolvido pela API está na escala 0 a 1 (só o parâmetro `required_score` usa 0 a 1000).
* Autoarestas removidas.
* A rede é construída a partir das arestas, então genes sem nenhuma interação acima do limiar **não entram no grafo**. O número de genes fora da rede em cada limiar está na tabela de resultados.

## Modelagem da rede

| Elemento | Definição |
|---|---|
| **Nó** | proteína correspondente a um gene associado ao Alzheimer |
| **Aresta** | interação funcional entre duas proteínas, segundo o STRING |
| **Peso** | *combined score* do STRING (0 a 1): quanto maior, mais confiável a interação |
| **Direção** | não dirigido (o STRING agrega evidências simétricas) |
| **Custo (para betweenness)** | `1 - score`: interação mais confiável equivale a aresta mais "curta" |

**O que fica de fora:** contexto específico de tecido, distinção entre interação física e funcional, e proteínas fora dos 200 genes selecionados (inclusive as que mediariam interações indiretas).

**Hipótese a destacar:** o *combined score* do STRING inclui evidência de mineração de texto, o que favorece genes muito estudados (ver Limitações).

## Conteúdo do curso utilizado

Todas as referências são ao arquivo `analise.ipynb`.

| Conceito | Semana | Onde aparece | Para que serviu |
|---|---|---|---|
| Grafo não dirigido ponderado, atributos de aresta | 2 | Seção 4 | Representar proteínas e interações com o score como peso |
| Grau e distribuição de grau | 2 | Seções 5, 6 e 7 | Identificar hubs; discutir cauda pesada (escala linear e log-log) |
| Densidade | 2 | Seção 5 e figura de comparação (seção 10) | Medir a conectividade geral e comparar limiares |
| Componentes conexos (GCC) | 4 | Seção 5 | Verificar se a rede é conexa e definir onde calcular as centralidades |
| Clustering e triângulos | 4 | Seção 5 | Avaliar a coesão local de cada proteína |
| Betweenness (ponderada) | 4 | Seções 5, 7, 8 e 10 | Identificar proteínas-ponte entre regiões da rede |
| Centralidade de eigenvector | 4 | Seção 5 | Identificar proteínas ligadas a outras proteínas centrais |
| K-core e core number | 5 | Seções 5 e 10 | Identificar o núcleo denso; colorir o grafo |

## Como executar

```bash
git clone <URL-do-repositório>
cd <pasta-do-repositório>
pip install -r requirements.txt
jupyter notebook analise.ipynb
```

No notebook, use **Reiniciar e executar tudo**. Ele baixa o CSV de genes e as interações do STRING (precisa de internet na primeira execução; depois reaproveita os arquivos de `dados/`) e gera as pastas `resultados/` e `figuras/`.

Também roda no Google Colab: abra o notebook, execute todas as células.

**Estrutura do repositório**

```
README.md
requirements.txt
analise.ipynb
dados/        genes_alzheimer.csv, string_400.tsv, string_700.tsv, string_900.tsv
resultados/   tabelas de métricas, resumo das redes, robustez, validação
figuras/      distribuicao_grau.png, grafo_string_700.png, comparacao_limiares.png
```

## Resultados

> **PENDENTE:** preencher após rodar o notebook com o STRING real. Estrutura sugerida abaixo.

### Estrutura das redes por limiar

*(colar a tabela `resumo_redes`: nós, genes fora da rede, arestas, densidade, componentes, fração na GCC, core máximo)*

![Comparação dos limiares](figuras/comparacao_limiares.png)

### Distribuição de grau

![Distribuição de grau](figuras/distribuicao_grau.png)

*(comentar: há cauda pesada? poucos hubs e muitas proteínas de grau baixo?)*

### Proteínas mais centrais (limiar 700)

*(colar o top 10 por grau, betweenness, eigenvector, clustering e core number)*

### Hubs e pontes são as mesmas proteínas?

*(colar a correlação de Spearman e o cruzamento top 10 grau × top 10 betweenness; comentar quais proteínas coincidem)*

### Núcleo denso

![Rede no limiar 700](figuras/grafo_string_700.png)

*(comentar o que há no core de maior número e como isso se relaciona com os hubs)*

### Robustez ao limiar

*(colar a tabela de Jaccard entre os top 10 e as proteínas presentes nos três limiares)*

### Validação com genes de referência

*(colar a posição de APP, APOE, MAPT e PSEN1 nos rankings e comentar)*

### Resposta à pergunta

*(3 a 5 linhas: quais proteínas se destacam, em que sentido e com que grau de confiança)*

## Limitações e próximos passos

**Limitações**

* **Viés de pesquisa:** genes muito estudados têm mais interações registradas no STRING, e o score combina evidência de mineração de texto. Grau alto pode refletir popularidade, não importância biológica.
* **Topologia, não função:** a rede mostra posição estrutural; não comprova causalidade nem relevância clínica de uma proteína.
* **Rede funcional agregada:** não distingue interação física de associação funcional e não considera tecido (cérebro, por exemplo).
* **Subrede selecionada:** só os 200 genes de maior associação entram; proteínas intermediárias ficam de fora.
* **Genes isolados:** proteínas sem interação acima do limiar saem do grafo, então a densidade não é diretamente comparável entre limiares.
* **Cálculo na GCC:** betweenness e eigenvector são calculados na maior componente conexa; quem está fora dela recebe valor 0.
* **Sensibilidade ao limiar:** os rankings mudam com o limiar, o que é avaliado na seção de robustez.

**Próximos passos**

* Comparar com redes específicas de tecido cerebral ou com interações apenas físicas.
* Detectar comunidades (módulos) e relacioná-las a vias biológicas.
* Simular a remoção de proteínas e medir o dano à conectividade, em vez de usar só centralidades.
* Validar os candidatos contra bases de alvos terapêuticos conhecidos.

## Referências

* Szklarczyk, D. et al. *The STRING database*. https://string-db.org/
* Open Targets Platform. https://platform.opentargets.org/
* Hagberg, A.; Schult, D.; Swart, P. *NetworkX*. https://networkx.org/
* Material da disciplina Algoritmos e Estrutura de Dados II (Grafos), UFRN, semanas 2 a 5.
