# Trabalho-WebScraping-Coleta-

**Disciplina:** Coleta, Preparação e Análise de Dados

---

## Objetivo

Aplicar técnicas de web scraping em dois cenários:

1. **Tarefa 1 — Ambiente controlado (Wikipedia):** Exploração de links e conexões entre artigos
2. **Tarefa 2 — Ambiente real (IMDb):** Análise de dados dos filmes com maior avaliação

---

## 📋 Pré-requisitos

- Python **3.8+**
- pip
- Google Chrome instalado (necessário para a Tarefa 2)

---

## ⚙️ Instalação

### 1. Clone ou baixe o projeto

```bash
git clone <url-do-repositorio>
cd <pasta-do-projeto>
```

### 2. (Opcional) Crie um ambiente virtual

```bash
python -m venv ambiente
source ambiente/bin/activate        # Linux/macOS
ambiente\Scripts\activate           # Windows
```

### 3. Instale as dependências

**Tarefa 1 — Wikipedia:**
```bash
pip install requests beautifulsoup4 pandas notebook
```

**Tarefa 2 — IMDb (requer dependências adicionais):**
```bash
pip install requests beautifulsoup4 pandas notebook selenium
```

> O Selenium utiliza o ChromeDriver para controlar o navegador. Na maioria das versões recentes do Selenium (4.6+), o ChromeDriver é baixado automaticamente. Certifique-se de ter o Google Chrome instalado na máquina.

---

## Tarefa 1 — Wikipedia

### O que faz

O notebook realiza as seguintes etapas:

1. **Crawler inicial:** baixa o HTML da página da Wikipedia escolhida (Lionel Messi) e salva localmente.
2. **Extração de links:** identifica todos os links internos (`/wiki/`) da página principal e baixa os HTMLs correspondentes.
3. **Scraping dos HTMLs:** extrai de cada página salva o título, o primeiro parágrafo, as URLs das imagens e os links internos.
4. **Exportação:** salva os dados em dois arquivos CSV com timestamp da coleta.

> ⚠️ **AVISO:** Devido ao tamanho dos arquivos CSV gerados, eles não são suportados pelo GitHub e serão enviados separadamente.

### Como executar

```bash
jupyter notebook
```

Abra o arquivo `messi.ipynb` e execute as células em ordem com **Shift + Enter**, ou clique em **"Run All"**.

### Estrutura gerada

```
data/
├── html/
│   ├── main.html       # HTML da página principal (Lionel Messi)
│   ├── page_0.html
│   ├── page_1.html
│   └── ...             # Uma página para cada link encontrado
├── pages.csv           # Dados de cada página coletada
└── images.csv          # URLs de imagens encontradas
```

### Saída dos CSVs

**`pages.csv`**

| Coluna | Descrição |
|---|---|
| `title` | Título da página (tag `<h1>`) |
| `first_paragraph` | Primeiro parágrafo com texto |
| `link_principal` | URL original da página |
| `internal_links` | Lista de links internos encontrados |
| `timestamp` | Data e hora do download |

**`images.csv`**

| Coluna | Descrição |
|---|---|
| `image_url` | URL de cada imagem encontrada nas páginas |

### ⚠️ Avisos importantes

**Tempo de execução:** o script baixa ~2000 páginas com `time.sleep(1)` entre requisições — aproximadamente **35–40 minutos** no total.

Para **testar rapidamente**, edite o loop no segundo bloco do notebook:

```python
# Baixa apenas os 10 primeiros links:
for i, link in enumerate(links[:10]):
```

A pasta `data/html/` é **apagada e recriada** a cada execução para evitar duplicações.

---

## Tarefa 2 — IMDb

### O que faz

O notebook utiliza **Selenium** para navegar pelo IMDb com um navegador real (Chrome), contornando o bloqueio de requisições simples. As etapas são:

1. **Coleta do Top 250:** acessa `https://www.imdb.com/chart/top/`, faz scroll até o fim da página para garantir que todos os 250 itens sejam carregados e extrai título, nota e URL de cada filme.
2. **Scraping individual:** acessa a página de cada um dos 250 filmes e extrai:
   - Título
   - Ano de lançamento
   - Nota IMDb
   - Lista de gêneros
   - Lista de diretores
   - URL do pôster (maior resolução disponível via `srcset`)
   - Imagem do pôster em **base64** (embutida no JSON)
3. **Exportação:** salva todos os dados em um único arquivo JSON.

### Como executar

```bash
jupyter notebook
```

Abra o arquivo `imdb.ipynb` e execute as células em ordem com **Shift + Enter**, ou clique em **"Run All"**.

> O Chrome abrirá automaticamente durante a execução. Não feche a janela do navegador.

### Estrutura gerada

```
data/
└── imdb_top250.json    # Dados completos dos 250 filmes
```

### Estrutura do JSON

Cada entrada no arquivo `imdb_top250.json` segue o formato abaixo:

```json
{
  "rank": 1,
  "title": "The Shawshank Redemption",
  "year": 1994,
  "imdb_rating": 9.3,
  "url": "https://www.imdb.com/title/tt0111161",
  "genres": ["Drama", "Prison Drama"],
  "directors": ["Frank Darabont"],
  "poster_url": "https://m.media-amazon.com/images/...",
  "poster_b64": "data:image/jpeg;base64,..."
}
```

Campos ausentes (quando não encontrados na página) são salvos como `null` ou lista vazia `[]`.

### ⚠️ Avisos importantes

**Tempo de execução:** o script processa 250 páginas com `time.sleep(0.5)` entre cada uma — aproximadamente **25–30 minutos** no total.

**Tamanho do arquivo:** o `imdb_top250.json` pode ser grande (~50–100 MB) devido às imagens em base64. Por isso, não será versionado no GitHub e será enviado separadamente.

**Resumo da última execução:**

| Métrica | Resultado |
|---|---|
| Total processados | 250 |
| Com nota IMDb | 250 |
| Sem gêneros | 0 |
| Sem diretores | 1 |
| Sem imagem do pôster | 0 |
| Tempo total | ~1546s (~26 min) |

---
