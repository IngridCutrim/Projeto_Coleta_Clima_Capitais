# Projeto de Coleta: Clima nas 27 capitais brasileiras
**Autora:** Ingrid Cutrim | **Disciplina:** D1INT, Introdução à Ciência de Dados
**Data da coleta:** 09/11/2026 15:44
**Período dos dados:** 2026-10-10 a 2026-11-08

## Pergunta
O que acompanha mais a temperatura média das capitais: a latitude ou a altitude?

## Fontes (todas via API REST, sem chave)
| Fonte | URL | Dados |
|---|---|---|
| IBGE Localidades | tabela local (APIs indisponíveis) | estado e região |
| Open-Meteo Geocoding | https://geocoding-api.open-meteo.com/v1/search | coordenadas, altitude, população |
| Open-Meteo Forecast | https://api.open-meteo.com/v1/forecast | temperatura, chuva e umidade diárias |

## Arquivos gerados
- `dataset_clima_capitais.csv`: 27 linhas x 20 colunas (3.8 KB), uma linha por capital
- `dataset_clima_diario.csv`: 810 registros diários limpos
- `dados_brutos/`: respostas originais das APIs (JSON)
- `graficos/`: gráficos em PNG

## Limpeza aplicada
Conversão de tipos (to_datetime / to_numeric), remoção do dia atual (incompleto), remoção de
duplicatas por capital+dia, remoção de dias sem temperatura, chuva ausente = 0 mm, filtros de
consistência (-10 a 50 °C, máxima >= mínima, chuva >= 0).

## Dicionário de dados
| Coluna | Descrição |
|---|---|
| uf | Sigla da unidade da federação (IBGE) |
| estado | Nome do estado (IBGE) |
| regiao | Região geográfica (IBGE) |
| capital | Nome da capital |
| latitude / longitude | Coordenadas da cidade em graus (Open-Meteo Geocoding) |
| dist_equador_graus | Distância do Equador = valor absoluto da latitude (calculada) |
| altitude_m | Altitude da cidade em metros (Open-Meteo Geocoding) |
| populacao | População informada pelo Geocoding (GeoNames); pode estar desatualizada |
| temp_media_c | Média das temperaturas médias diárias no período (°C) |
| temp_max_media_c / temp_min_media_c | Média das máximas / mínimas diárias (°C) |
| amplitude_termica_c | temp_max_media_c - temp_min_media_c (°C) |
| chuva_total_mm | Chuva acumulada no período (mm) |
| dias_com_chuva | Dias com 1 mm de chuva ou mais |
| umidade_media_pct | Umidade relativa média (%) |
| dias_observados | Quantidade de dias válidos após a limpeza |
| periodo_inicio / periodo_fim | Primeiro e último dia analisados |
| data_coleta | Data em que o notebook foi executado |

## Insights
1. A capital mais quente no período foi Teresina-PI (31.9 °C) e a mais fria foi Curitiba-PR (17.1 °C): diferença de 14.8 °C.
2. A região com maior temperatura média foi a Norte (28.8 °C).
3. A correlação entre altitude e temperatura foi moderada e negativa (r = -0.51); entre distância do Equador e temperatura foi forte e negativa (r = -0.90). Neste período, a distância do Equador acompanhou melhor a variação de temperatura.
4. Rio Branco-AC foi a capital mais chuvosa (251 mm em 30 dias).
5. A maior amplitude térmica (diferença média entre máxima e mínima) foi em Teresina-PI: 13.3 °C.

## Limitações
- São só 30 dias: o resultado reflete a estação atual, não o clima do ano todo.
- Os dados diários vêm de modelos meteorológicos (Open-Meteo), não diretamente de estações.
- Uma coordenada representa a cidade inteira; bairros podem ter clima diferente.
- Correlação não prova causa; latitude e altitude também se misturam com distância do mar.
