# Analise de Imoveis - Site Estatico

[![Site](https://img.shields.io/badge/SITE-Analise%20de%20Imoveis-blue?style=for-the-badge)](https://jeferson100.github.io/Analise-imoveis/)

Painel web para visualizacao e analise de imoveis a venda e aluguel em 11 cidades brasileiras.

## Cidades disponiveis

| Cidade | Estado | Imoveis | Aluguel | Predicao de Preco |
|--------|--------|---------|---------|-------------------|
| Joinville | SC | ~24k | ✅ | ✅ |
| Florianopolis | SC | ~50k | - | - |
| Blumenau | SC | ~15k | - | - |
| Balneario Camboriu | SC | ~16k | ✅ | ✅ |
| Balneario Picarras | SC | ~5k | - | - |
| Itai | SC | ~12k | - | - |
| Itapema | SC | ~12k | - | - |
| Itapoa | SC | ~4k | - | - |
| Jaragua do Sul | SC | ~4k | - | - |
| Curitiba | PR | ~39k | - | - |
| Sao Paulo | SP | ~106k | - | - |

## Funcionalidades

### Modo Venda
- Metricas gerais: preco por m2, valor do imovel, dimensoes
- Filtros: bairro, tipo, quartos, banheiros, vagas, metragem
- Remocao de outliers (IQR e percentil 99.6%)
- Tabela ordenavel com todos os imoveis
- Mapa com geolocalizacao (Leaflet)
- Graficos interativos (Plotly.js)

### Modo Aluguel
- Joinville e Balneario Camboriu possuem dados de aluguel
- Metricas: valor do aluguel, condominio, IPTU

### Predicao de Preco (Joinville e Balneario Camboriu)
- Formulario para prever o valor de um imovel
- Modelo: GradientBoostingRegressor (200 arvores)
- Inferencia 100% em JavaScript (sem servidor)
- Intervalo de predicao via conformal prediction

## Como funciona

### Estrutura de arquivos
```
site_estatico/
├── index.html              # Pagina principal
├── css/style.css           # Estilos
├── js/
│   ├── app.js              # Logica principal (filtros, tabela, mapa, graficos)
│   └── predicao.js         # Inferencia de preco em JS puro
└── data/
    ├── dados_{cidade}.js          # Dados dos imoveis (carregado via <script>)
    ├── stats_{cidade}.js          # Estatisticas por bairro
    ├── bairro_stats_{cidade}.json # Stats detalhados
    ├── imoveis_{cidade}.json      # Dados completos
    ├── modelo_{cidade}.json       # Modelo de predicao (arvores + preprocessing)
    └── config_aluguel.json        # Cidades com dados de aluguel
```

### Carregamento de dados
- Os dados sao carregados via tags `<script>` dinamicas
- Funciona com `file://` protocol (sem servidor)
- Cada cidade tem seus proprios arquivos JS/JSON

### Predicao de preco
- O modelo e exportado como JSON (arvores de decisao + parametros de preprocessing)
- Pre-processamento em JS: imputacao -> Yeo-Johnson -> StandardScaler -> RobustScaler
- One-Hot Encoding para variaveis categoricas
- Predicao via GradientBoostingRegressor puro em JavaScript

## Tecnologias

- **HTML/CSS/JS** puro (sem frameworks)
- **Plotly.js** — graficos interativos
- **Leaflet.js** — mapas com OpenStreetMap
- **GradientBoostingRegressor** — modelo de predicao (scikit-learn)
- **JSON** — exportacao do modelo para inferencia client-side

## Atualizacao dos dados

Os dados sao atualizados automaticamente via GitHub Actions:
1. Scraping de sites de imoveis (ZAP, VivaReal, OLX, Chave na Mao)
2. Limpeza e processamento
3. Treinamento de modelos
4. Exportacao para JSON/JS
5. Deploy via GitHub Pages
