# Trabalho-WebScraping-Coleta-
**Disciplina:** Coleta, Preparação e Análise de Dados  

---

##  Objetivo

Aplicar técnicas de web scraping em dois cenários:

1. **Tarefa 1 — Ambiente controlado (Wikipedia):** Exploração de links e conexões entre artigos 
2. **Tarefa 2 — Ambiente real (IMDb):** Análise de dados dos filmes com maior avaliação *Em desenvolvimento*

---

## 📋 Pré-requisitos

- Python **3.8+**
- pip

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

```bash
pip install requests beautifulsoup4 pandas notebook
```

---

## Tarefa 1 — Wikipedia

### O que faz

O notebook realiza as seguintes etapas:

1. **Crawler inicial:** baixa o HTML da página da Wikipedia escolhida (Lionel Messi) e salva localmente.
2. **Extração de links:** identifica todos os links internos (`/wiki/`) da página principal e baixa os HTMLs correspondentes.
3. **Scraping dos HTMLs:** extrai de cada página salva o título, o primeiro parágrafo, as URLs das imagens e os links internos.
4. **Exportação:** salva os dados em dois arquivos CSV com timestamp da coleta.

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

Devido ao tamanho dos sv gerandos nao serem suportados pelo github estaremos enviando separadamente.
---

##  Tarefa 2 — IMDb — Em Desenvolvimento

> **Esta seção ainda está em desenvolvimento e não está funcional.**

### O que será implementado

1. Scraping da lista **Top 250 filmes** do IMDb (`https://www.imdb.com/chart/top/`).
2. Scraping das páginas individuais de cada filme para extrair:
   - Título
   - Ano de lançamento
   - URL do pôster
   - Imagem do pôster
   - Nota IMDb
   - Lista de gêneros
   - Lista de diretores
3. Exportação de todos os dados para um arquivo **JSON**.

### Estrutura prevista de saída

```
data/
└── imdb_top250.json    # Dados dos 250 filmes (a ser gerado)
```

---
