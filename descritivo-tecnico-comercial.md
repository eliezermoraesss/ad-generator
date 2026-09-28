# 🚀 Plataforma de Automação para Afiliados — Compra Inteligente

## Visão geral

O **Compra Inteligente** é uma plataforma de automação desenvolvida para transformar links de produtos de marketplaces em **ofertas prontas para divulgação**, reduzindo drasticamente o trabalho manual necessário para pesquisar produtos, coletar informações, identificar descontos, preparar imagens e criar mensagens para WhatsApp e Telegram.

O sistema foi projetado para atuar desde a **entrada do link do produto até a publicação da oferta**, utilizando automação, web scraping, processamento de dados e Inteligência Artificial.

A solução permite que o afiliado deixe de executar manualmente tarefas repetitivas e concentre seu trabalho na **curadoria de produtos, estratégia de divulgação e crescimento das vendas**.

---

# 💰 Onde o sistema gera economia

Tradicionalmente, para publicar uma oferta, o afiliado precisa:

1. Receber ou encontrar o produto.
2. Abrir o link.
3. Acessar o marketplace.
4. Localizar o nome do produto.
5. Identificar o preço atual.
6. Procurar o preço anterior.
7. Copiar a imagem.
8. Verificar se existe desconto.
9. Identificar eventual cupom.
10. Copiar o link de afiliado.
11. Criar manualmente o texto da oferta.
12. Aplicar formatação.
13. Enviar a mensagem para WhatsApp/Telegram.
14. Repetir todo o processo para cada produto.

Com a plataforma, grande parte desse processo é automatizada.

### Fluxo automatizado

**LINK → SCRAPER → DADOS DO PRODUTO → PROCESSAMENTO → IA → OFERTA FORMATADA → WHATSAPP/TELEGRAM**

Isso transforma uma operação manual e repetitiva em um processo automatizado e escalável.

---

# ⚡ Economia de tempo operacional

O principal ganho da solução está na **redução do trabalho operacional por oferta**.

Em vez de o afiliado ficar alternando entre navegador, marketplace, planilha, editor de texto e WhatsApp, o sistema centraliza o processo.

### Antes

**Produto → pesquisa manual → copiar informações → montar texto → editar → copiar → WhatsApp**

### Depois

**Link → processamento automático → oferta pronta → publicação**

Isso permite que o afiliado processe **diversos produtos em sequência**, aumentando a quantidade de ofertas que pode trabalhar sem aumentar proporcionalmente o esforço operacional.

---

# 🤖 Inteligência Artificial aplicada às ofertas

A plataforma utiliza Inteligência Artificial para transformar os dados coletados em mensagens comerciais estruturadas.

A IA recebe os dados reais do produto, como:

- Nome;
- Preço atual;
- Preço anterior;
- Link;
- Cupom;
- Informações promocionais.

A partir dessas informações, gera uma mensagem padronizada para divulgação.

O sistema possui regras para evitar que a IA invente informações que não estejam presentes nos dados coletados.

Isso é especialmente importante em um ambiente de afiliados, onde **preço, desconto, cupom e link precisam corresponder ao produto real**.

---

# 🛒 Integração com marketplaces

A arquitetura foi preparada para trabalhar com diferentes plataformas de comércio eletrônico.

Atualmente, o projeto possui tratamento específico para:

- **Mercado Livre**
- **Amazon**

O sistema identifica a origem do link e direciona automaticamente para o processo de coleta adequado.

Também existe tratamento para diferentes formatos de URL, incluindo links de afiliados e links encurtados.

---

# 🔎 Web Scraping automatizado

O núcleo de coleta utiliza **Python + FastAPI + Playwright**.

O scraper acessa a página do produto e extrai automaticamente informações relevantes.

### Dados coletados

```text
Título do produto
Preço atual
Preço anterior
Imagem principal
URL
Status da operação
Checkpoint de execução
```

A arquitetura também possui mecanismos de controle de erro e checkpoints.

Por exemplo:

```text
start
playwright_started
browser_launched
before_goto
after_goto
mercado_livre_detected
mercado_livre_domcontentloaded
mercado_livre_before_wait_title
mercado_livre_after_wait_title
```

Isso facilita a identificação de falhas durante o processamento.

---

# 🖼️ Tratamento automático de imagens

Um dos problemas comuns em automações de afiliados é receber imagens em formatos que não são adequados para determinados canais de comunicação.

A plataforma possui uma etapa específica para **normalização das imagens**, convertendo formatos incompatíveis para formatos adequados à publicação.

Exemplo:

```text
WEBP
JPEG
PNG
   ↓
NORMALIZAÇÃO
   ↓
JPG
```

Isso permite que a imagem coletada pelo scraper seja preparada automaticamente antes do envio.

---

# 🎟️ Detecção de cupons

O sistema também pode identificar automaticamente informações de cupom presentes na mensagem recebida.

Exemplo:

```text
Cupom VESTIR BEM
```

é transformado em um atributo estruturado:

```json
{
  "cupom": "VESTIR BEM"
}
```

Isso permite que o cupom seja posteriormente utilizado pela IA na construção da oferta.

---

# 📦 Processamento de múltiplos produtos

A arquitetura também permite evoluir o processo de uma oferta individual para **lotes de produtos**.

Em vez de:

```text
1 link
↓
1 scraping
↓
1 IA
↓
1 mensagem
```

o sistema pode trabalhar com:

```text
Lista de links
      ↓
Processamento individual
      ↓
Scraping
      ↓
Dados estruturados
      ↓
Agregação
      ↓
Uma única requisição para IA
      ↓
Várias ofertas formatadas
      ↓
Separação das mensagens
      ↓
Envio individual
```

Essa arquitetura reduz chamadas desnecessárias à IA e melhora a eficiência do processamento.

---

# ⏱️ Controle de requisições

Para operações envolvendo marketplaces, a arquitetura pode utilizar **intervalos controlados entre requisições**.

Exemplo:

```text
Produto 1
   ↓
aguarda
   ↓
Produto 2
   ↓
aguarda
   ↓
Produto 3
```

Essa abordagem reduz a concentração de requisições em um curto período e torna o processamento de múltiplos produtos mais controlado.

---

# 📲 Automação de WhatsApp

Após o processamento, o sistema pode enviar automaticamente as ofertas para o WhatsApp através da **Evolution API**.

A arquitetura permite separar o envio de:

### 🖼️ Oferta com imagem

```text
Imagem
+
Mensagem formatada
```

### 💬 Oferta somente texto

```text
Mensagem formatada
```

Assim, o sistema pode decidir o fluxo de envio de acordo com o resultado do processamento.

---

# 🔄 Arquitetura do fluxo

A arquitetura atual utiliza uma abordagem baseada em **n8n**, permitindo construir um pipeline visual de automação.

Exemplo conceitual:

```text
                 ┌──────────────┐
                 │   WhatsApp   │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   Webhook    │
                 └──────┬───────┘
                        │
                        ▼
                ┌────────────────┐
                │ Extrair Links  │
                └───────┬────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ Validar Marketplace│
              └─────────┬──────────┘
                        │
                        ▼
               ┌─────────────────┐
               │    Scraper      │
               │ Python/Playwright│
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ Normalizar dados│
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ Detectar cupom  │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │   Agregador     │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ Inteligência    │
               │    Artificial   │
               └────────┬────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ Separar ofertas    │
              └─────────┬──────────┘
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
        ┌───────────┐       ┌───────────┐
        │  Imagem   │       │   Texto   │
        └─────┬─────┘       └─────┬─────┘
              │                   │
              └─────────┬─────────┘
                        ▼
                 ┌──────────────┐
                 │   WhatsApp   │
                 └──────────────┘
```

---

# 📈 Escalabilidade

Um dos principais objetivos da arquitetura é permitir que o crescimento da quantidade de ofertas **não exija um crescimento proporcional da equipe ou do trabalho manual**.

Por exemplo, uma operação que precisa publicar:

```text
10 ofertas
```

pode utilizar praticamente o mesmo fluxo estrutural de uma operação que deseja trabalhar com:

```text
50 ofertas
100 ofertas
500 ofertas
```

O que muda principalmente é a quantidade de itens processados.

A automação transforma o trabalho do afiliado de uma atividade predominantemente **operacional** para uma atividade mais focada em:

- Encontrar boas oportunidades;
- Selecionar produtos;
- Criar campanhas;
- Administrar canais;
- Acompanhar resultados;
- Otimizar conversões.

---

# 💵 Impacto para o afiliado

O valor da plataforma não está apenas em "raspar produtos".

O principal benefício é criar uma **linha de produção automatizada de ofertas**.

O afiliado pode aumentar sua capacidade operacional sem precisar executar manualmente cada etapa.

### O sistema automatiza:

✅ Coleta de produtos  
✅ Identificação de marketplace  
✅ Extração de informações  
✅ Coleta de preços  
✅ Coleta de imagens  
✅ Tratamento de imagens  
✅ Identificação de cupons  
✅ Validação de URLs  
✅ Formatação das ofertas  
✅ Geração de texto com IA  
✅ Organização das ofertas  
✅ Envio automatizado  
✅ Processamento em lote  

---

# 🧠 Diferencial estratégico

A grande diferença está na integração de várias tecnologias em um único fluxo:

**n8n + Python + FastAPI + Playwright + IA + Evolution API + Redis + WhatsApp**

Não se trata simplesmente de um scraper.

É uma **plataforma de automação de marketing de afiliados**.

O scraper coleta os dados.

O backend processa.

O n8n orquestra.

A IA transforma os dados em conteúdo.

A automação de WhatsApp distribui.

E o afiliado concentra seus esforços naquilo que realmente gera valor: **selecionar boas ofertas e alcançar compradores**.

---

# 🚀 Resultado esperado

A plataforma foi projetada para proporcionar:

### ⏱️ Economia de tempo

Menos tarefas manuais repetitivas para cada produto.

### 📦 Maior capacidade operacional

Possibilidade de processar vários produtos através de um único fluxo.

### 🤖 Automação

Redução da necessidade de intervenção humana durante o processo.

### 📱 Maior velocidade de publicação

Do link recebido até a oferta pronta para divulgação.

### 📊 Padronização

Todas as ofertas seguem uma estrutura definida.

### 🔗 Redução de erros operacionais

Menos necessidade de copiar e colar manualmente preços, links, títulos e informações.

### 📈 Escalabilidade

A operação pode crescer sem depender proporcionalmente de mais trabalho manual.

---

# 💡 Em uma frase

> **O Compra Inteligente transforma links de produtos em ofertas prontas para venda, automatizando coleta, análise, formatação e distribuição — permitindo que afiliados publiquem mais ofertas, com menos trabalho operacional.**

---

# 🏆 Proposta de valor

**Antes:**

> "Eu preciso pesquisar, copiar, formatar e publicar cada oferta."

**Com o sistema:**

> "Eu forneço os links e a plataforma executa o processo."

Esse é o ponto central da solução: **automatizar a operação de afiliados do início ao fim**, transformando uma sequência de tarefas manuais em um pipeline automatizado, escalável e orientado a produtividade.
