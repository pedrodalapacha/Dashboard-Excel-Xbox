<h1 align="center">📊 Dashboard de Vendas: Xbox Game Pass</h1>

<p align="center">
  <strong>Dashboard de vendas de assinaturas construído 100% no Excel, com tabelas dinâmicas, segmentação de dados e gráfico dinâmico.</strong><br>
  Projeto desenvolvido no bootcamp <em>Santander – Excel com IA (Claude)</em> da <a href="https://www.dio.me/">DIO</a>.
</p>

<p align="center">
  <img alt="Excel" src="https://img.shields.io/badge/Microsoft%20Excel-0074C5?style=for-the-badge&logo=microsoftexcel&logoColor=white">
  <img alt="Tabelas dinâmicas" src="https://img.shields.io/badge/Tabelas%20Din%C3%A2micas-014E90?style=for-the-badge">
  <img alt="Dashboard" src="https://img.shields.io/badge/Dashboard-FF460A?style=for-the-badge">
  <img alt="DIO" src="https://img.shields.io/badge/DIO-Santander-03588C?style=for-the-badge">
</p>

---

## 📌 Sumário

- [Sobre o projeto](#-sobre-o-projeto)
- [O desafio](#-o-desafio)
- [Prévia](#-prévia)
- [Estrutura do arquivo](#-estrutura-do-arquivo)
- [Dados utilizados](#-dados-utilizados)
- [Perguntas de negócio](#-perguntas-de-negócio)
- [Componentes do dashboard](#-componentes-do-dashboard)
- [Identidade visual](#-identidade-visual)
- [Como reproduzir passo a passo](#-como-reproduzir-passo-a-passo)
- [Como usar o dashboard](#-como-usar-o-dashboard)
- [Principais insights](#-principais-insights)
- [Recursos do Excel utilizados](#-recursos-do-excel-utilizados)
- [Autor](#-autor)

---

## 💡 Sobre o projeto

Este projeto transforma uma base de **295 assinaturas do Xbox Game Pass** (ano de 2024) em um **dashboard interativo** que responde perguntas de negócio sobre faturamento:

- quanto foi faturado em assinaturas;
- quanto veio de clientes **com** e **sem** renovação automática;
- quanto foi vendido dos complementos **EA Play Season Pass** e **Minecraft Season Pass**.

Com um clique na **segmentação de dados** (Mensal, Trimestral ou Anual), todos os indicadores e o gráfico são atualizados ao mesmo tempo.

---

## 🎯 O desafio

> O objetivo deste desafio é criar um dashboard de vendas, com foco na organização e visualização de dados. O objetivo é transformar dados brutos em informações visuais claras e úteis, permitindo uma análise eficaz do desempenho de vendas e a tomada de decisões baseadas em dados.

| Requisito | Como foi atendido |
|---|---|
| Organizar os dados | Base formatada como **Tabela do Excel** (`Tabela1`), fonte única de todos os cálculos |
| Transformar dados brutos em informação | 3 **tabelas dinâmicas** respondem às perguntas de negócio |
| Visualização clara | 2 cards de KPI + 1 **gráfico dinâmico** de barras, em layout limpo |
| Análise e tomada de decisão | **Segmentação de dados** conectada às 3 tabelas dinâmicas filtra todo o painel |
| Projeto com identidade própria | Paleta personalizada em azul e laranja aplicada no painel, no gráfico, na segmentação e no tema do arquivo |

---

## 🖼️ Prévia

![Dashboard](imagens/dashboard.png)

> Prévia gerada a partir do arquivo, com o filtro **Monthly** selecionado.

---

## 🗂️ Estrutura do arquivo

O arquivo **`Dashboard_Vendas_XboxGamePass.xlsx`** segue o fluxo **Assets → Bases → Cálculos → Dashboard**:

```
Dashboard_Vendas_XboxGamePass.xlsx
├── A̳ssets      (oculta) → paleta de cores, logos e ícones usados no painel
├── B̳ases       (oculta) → dados brutos formatados como Tabela1 (295 linhas × 13 colunas)
├── C̳álculos    (oculta) → perguntas de negócio, tabelas dinâmicas e INFOPIVOTDADOS
└── D̳ashboard   (visível) → painel final para o usuário
```

> As letras sublinhadas nos nomes das abas (A̳, B̳, C̳, D̳) são um detalhe visual que deixa a ordem das abas explícita.

Somente o **Dashboard** fica visível. As demais abas ficam ocultas para que o usuário final veja só o painel. Para reexibir: botão direito em uma guia → **Reexibir**.

---

## 🧾 Dados utilizados

### Visão geral

| Item | Valor |
|---|---|
| Registros | **295 assinaturas** (1 linha = 1 assinante) |
| Período | **01/01/2024 a 16/12/2024** (campo `Start Date`) |
| Colunas | 13 |
| Faturamento total da base | **R$ 7.633,00** |
| Cupons concedidos | R$ 2.122,00 |
| Origem | Base fictícia fornecida pelo bootcamp DIO |

### Dicionário de dados

| Coluna | Tipo | Descrição | Valores |
|---|---|---|---|
| `Subscriber ID` | Número | Identificador do assinante | 3231 em diante |
| `Name` | Texto | Nome do assinante | — |
| `Plan` | Texto | Plano contratado | `Core` (101), `Standard` (96), `Ultimate` (98) |
| `Start Date` | Data | Início da assinatura | 2024 |
| `Auto Renewal` | Texto | Renovação automática ativa? | `Yes` (148), `No` (147) |
| `Subscription Price` | Moeda | Preço do plano | Core R$ 5 · Standard R$ 10 · Ultimate R$ 15 |
| `Subscription Type` | Texto | Periodicidade | `Monthly` (139), `Quarterly` (85), `Annual` (71) |
| `EA Play Season Pass` | Texto | Comprou o EA Play? | `Yes` (98, todos Ultimate), `No` (197) |
| `EA Play Season Pass Price` | Moeda | Valor do EA Play | R$ 30 ou `-` |
| `Minecraft Season Pass` | Texto | Comprou o passe do Minecraft? | `Yes` (194), `No` (101) |
| `Minecraft Season Pass Price` | Moeda | Valor do passe | R$ 20 ou R$ 0 |
| `Coupon Value` | Moeda | Desconto aplicado | R$ 0 a R$ 20 |
| `Total Value` | Moeda | Valor total da assinatura | ver regra abaixo |

### Regra do valor total

Conferida em **100% das linhas**:

```
Total Value = Subscription Price + EA Play Season Pass Price + Minecraft Season Pass Price − Coupon Value
```

Exemplo (João Silva): `15 + 30 + 20 − 5 = R$ 60,00`.

### Observações sobre a qualidade dos dados

- Na coluna `EA Play Season Pass Price`, quem não comprou aparece com **`-` (texto)**, e não com 0. As tabelas dinâmicas ignoram o texto na soma, então o resultado não muda.
- O cabeçalho dessa coluna tem uma **quebra de linha** (`EA Play Season Pass⏎Price`). Por isso a fórmula `INFOPIVOTDADOS` precisa usar o nome exatamente igual.
- O **EA Play** só aparece para assinantes **Ultimate**, e o **Minecraft Season Pass** só para **Standard** e **Ultimate**.

---

## ❓ Perguntas de negócio

Na aba **Cálculos**, cada pergunta é respondida por uma tabela dinâmica alimentada pela `Tabela1`:

| # | Pergunta | Tabela dinâmica | Linhas | Valores | Onde aparece |
|---|---|---|---|---|---|
| 1 | Qual o faturamento total de vendas de assinaturas, com todas as assinaturas agregadas? | `tbl_annual_total` (B11) | — (Total Geral) | Soma de `Total Value` | Barra **Total Result** do gráfico |
| 2 | Qual o faturamento total separado por renovação automática ou não? | `tbl_annual_total` (B11) | `Auto Renewal` | Soma de `Total Value` | Barras **Yes / No** do gráfico |
| 3 | Qual o total de vendas do **EA Play Season Pass**? | `tbl_easeasonpass_total` (B21) | `Plan` | Soma de `EA Play Season Pass Price` | Card **EA Play** (célula `E25`) |
| 4 | Qual o total de vendas do **Minecraft Season Pass**? | `Tabela dinâmica9` (B32) | `Plan` | Soma de `Minecraft Season Pass Price` | Card **Minecraft** (célula `E36`) |

As 3 tabelas dinâmicas têm o filtro **`Subscription Type`**, controlado pela segmentação de dados do painel. Os cards usam **INFOPIVOTDADOS** (`GETPIVOTDATA`):

```excel
E25: =INFOPIVOTDADOS("EA Play Season Pass
Price";$B$21)

E36: =INFOPIVOTDADOS("Minecraft Season Pass Price";$B$32)
```

### Respostas por tipo de assinatura

| Indicador | Monthly | Quarterly | Annual | **Total** |
|---|---:|---:|---:|---:|
| Faturamento total (P1) | R$ 3.571,00 | R$ 2.308,00 | R$ 1.754,00 | **R$ 7.633,00** |
| ↳ com renovação automática (P2) | R$ 747,00 | R$ 1.502,00 | R$ 1.537,00 | R$ 3.786,00 |
| ↳ sem renovação automática (P2) | R$ 2.824,00 | R$ 806,00 | R$ 217,00 | R$ 3.847,00 |
| EA Play Season Pass (P3) | R$ 1.350,00 | R$ 990,00 | R$ 600,00 | **R$ 2.940,00** |
| Minecraft Season Pass (P4) | R$ 1.800,00 | R$ 1.140,00 | R$ 940,00 | **R$ 3.880,00** |

> Para responder à pergunta 1 especificamente para **planos anuais**, basta selecionar **Annual** na segmentação: R$ 1.754,00 (R$ 1.537,00 com renovação e R$ 217,00 sem).

---

## 🧩 Componentes do dashboard

```
┌───────────────┬────────────────────────────────────────────────────────────┐
│  [avatar]     │ [logo]  XBOX GAME PASS SUBSCRIPTIONS SALES                 │
│ > bem-vindo,  │ Calculation period: 01/01/2024 - 31/12/2024 | Update date  │
│   Pedro       ├────────────────────────────┬───────────────────────────────┤
│               │ TOTAL SUBSCRIPTIONS        │ TOTAL SUBSCRIPTIONS           │
│ Subscription  │ EA PLAY SEASON PASS        │ MINECRAFT SEASON PASS         │
│ Type          │ [EA Play]   R$ 1.350,00    │ [Minecraft]   R$ 1.800,00     │
│ [ Annual    ] ├────────────────────────────┴───────────────────────────────┤
│ [■Monthly   ] │ TOTAL SUBSCRIPTIONS XBOX GAME PASS                         │
│ [ Quarterly ] │ Total Result ███████████████████████████  R$ 3.571,00      │
│               │ Yes          ██████  R$ 747,00                             │
│ SYSTEM BY     │ No           ██████████████████████  R$ 2.824,00           │
│ PEDRO CHRISTO │                                                            │
└───────────────┴────────────────────────────────────────────────────────────┘
```

| Componente | Tipo | Fonte |
|---|---|---|
| Menu lateral (avatar, saudação e assinatura com link para o LinkedIn) | Imagem e formas | Aba Assets |
| Segmentação **Subscription Type** | Segmentação de dados conectada às 3 tabelas dinâmicas | `Tabela1[Subscription Type]` |
| Título + linha laranja | Célula formatada (borda inferior espessa) | — |
| Período e data de atualização | Caixa de texto | — |
| Card **EA Play Season Pass** | Forma com texto vinculado a célula | `=Cálculos!E25` |
| Card **Minecraft Season Pass** | Forma com texto vinculado a célula | `=Cálculos!E36` |
| Gráfico **Total Subscriptions Xbox Game Pass** | Gráfico dinâmico de barras horizontais | `tbl_annual_total` |

---

## 🎨 Identidade visual

Paleta **"Post Instagram Dia do Programador Moderno"**, documentada na aba **Assets**:

| Cor | Hex | Uso no projeto |
|---|---|---|
| 🔵 Azul principal | `#0074C5` | Cabeçalhos dos cards, barras do gráfico, título, item selecionado na segmentação |
| 🔵 Azul-marinho | `#014E90` | Menu lateral, fundo da segmentação, rótulos de dados do gráfico |
| 🔵 Azul-petróleo | `#03588C` | Cor de apoio (tema do arquivo) |
| 🔵 Azul-claro | `#049DD9` | Cor de apoio (tema do arquivo) |
| 🟠 Laranja | `#FF460A` | Valores dos KPIs e linha sob o título (destaque) |
| ⬜ Fundo ("negative zone") | `#EAF1F8` | Área de fundo do painel, para os cards brancos se destacarem |

A paleta também foi aplicada às **cores de destaque do tema** do arquivo (Layout da Página → Cores). Assim, novas tabelas dinâmicas e gráficos já nascem com as mesmas cores.

---

## 🛠️ Como reproduzir passo a passo

> Testado no **Excel para Microsoft 365** (Windows). Funciona no Excel 2013 ou mais recente. Segmentação de dados em tabelas dinâmicas exige Excel 2010+. Os nomes dos comandos estão em português, com o equivalente em inglês entre parênteses.

### 1. Preparar as abas
1. Crie 4 abas: **Assets**, **Bases**, **Cálculos** e **Dashboard**.
2. Em **Assets**, registre os códigos HEX da paleta e cole os logos e ícones (Xbox, EA Play, Minecraft e avatar).

### 2. Organizar a base
1. Cole os dados brutos em **Bases**, a partir de `A1`.
2. Selecione qualquer célula → **Inserir → Tabela** (`Ctrl + Alt + T`) → marque *Minha tabela tem cabeçalhos*.
3. Formate `Start Date` como data e as colunas de preço como **Contábil (R$)**.

### 3. Criar as tabelas dinâmicas (aba Cálculos)
1. Escreva as perguntas de negócio no topo da aba.
2. **Inserir → Tabela Dinâmica → Da Tabela/Intervalo** → `Tabela1` → *Planilha existente* → `Cálculos!B11`:
   - **Filtros:** `Subscription Type` · **Linhas:** `Auto Renewal` · **Valores:** Soma de `Total Value`.
   - Renomeie a tabela dinâmica para `tbl_annual_total` (*Analisar Tabela Dinâmica → Nome*).
3. Repita em `B21`: Filtro `Subscription Type` · Linhas `Plan` · Valores Soma de `EA Play Season Pass Price` → `tbl_easeasonpass_total`.
4. Repita em `B32`: Filtro `Subscription Type` · Linhas `Plan` · Valores Soma de `Minecraft Season Pass Price`.
5. Em `E25` e `E36`, digite `=` e clique na célula do **Total Geral** de cada tabela dinâmica. O Excel gera a fórmula `INFOPIVOTDADOS` automaticamente.
6. Formate os valores como moeda (`R$`).

### 4. Montar o layout do Dashboard
1. **Exibir** → desmarque *Linhas de Grade* e *Títulos*.
2. Coluna **A** larga (≈ 27) com preenchimento `#014E90` (menu); coluna **B** estreita (≈ 3,5) como respiro.
3. Preencha a área do painel com `#EAF1F8`.
4. Título em `C2` (Segoe UI 15, negrito, `#0074C5`) com **borda inferior espessa** laranja `#FF460A` até a coluna S.
5. Insira o logo, a caixa de texto com o período e, no menu, o avatar (recortado em elipse) e a saudação.

### 5. Criar os cards de KPI
1. **Inserir → Formas → Retângulo: Cantos Arredondados** branco (o card).
2. Sobre ele, um **Retângulo: Cantos Superiores Arredondados** `#0074C5` com o título em branco.
3. Insira uma caixa de texto → com ela selecionada, digite na **barra de fórmulas** `=Cálculos!E25` e pressione Enter. O texto passa a acompanhar o valor da célula.
4. Fonte do valor: 36 pt, cor `#FF460A`. Adicione o logo do produto.
5. Agrupe tudo (`Ctrl + G`) e duplique para o card do Minecraft (`=Cálculos!E36`).

### 6. Criar o gráfico dinâmico
1. Clique em `tbl_annual_total` → **Analisar Tabela Dinâmica → Gráfico Dinâmico → Barras Agrupadas**.
2. Recorte o gráfico (`Ctrl + X`) e cole no Dashboard, sobre um card grande com o cabeçalho *TOTAL SUBSCRIPTIONS XBOX GAME PASS*.
3. Remova título, legenda, eixo horizontal e linhas de grade. Oculte os botões de campo (*Analisar Gráfico Dinâmico → Botões de Campo → Ocultar Tudo*).
4. Barras em `#0074C5`, **rótulos de dados** em `#014E90` e fundo sem preenchimento.

### 7. Criar a segmentação de dados
1. Clique em qualquer tabela dinâmica → **Analisar Tabela Dinâmica → Inserir Segmentação de Dados** → marque `Subscription Type`.
2. Com a segmentação selecionada → **Segmentação → Conexões de Relatório** → marque as **3 tabelas dinâmicas**.
3. Mova a segmentação para o menu lateral. Para o estilo: **Segmentação → Estilos → Novo Estilo de Segmentação**:
   - *Segmentação inteira*: preenchimento `#014E90`, fonte branca;
   - *Item selecionado com dados*: preenchimento `#0074C5`, fonte branca em negrito;
   - *Item não selecionado com dados*: preenchimento branco, fonte cinza;
   - *Item focalizado*: tons claros de laranja.

### 8. Finalizar
1. Oculte as abas **Assets**, **Bases** e **Cálculos** (botão direito → *Ocultar*).
2. (Opcional) Em *Arquivo → Opções → Avançado*, oculte a barra de rolagem e as guias para um visual de aplicativo.
3. Salve como `.xlsx`.

### Atualizando com novos dados
1. Cole as novas linhas **logo abaixo** da `Tabela1` em **Bases**. A tabela se expande sozinha.
2. **Dados → Atualizar Tudo** (`Ctrl + Alt + F5`). Tabelas dinâmicas, cards e gráfico são atualizados.

---

## 🖱️ Como usar o dashboard

1. Abra `Dashboard_Vendas_XboxGamePass.xlsx`.
2. No menu lateral, clique em **Annual**, **Monthly** ou **Quarterly** na segmentação.
3. Para comparar mais de um tipo, segure `Ctrl` e clique em vários. Para remover o filtro, clique no ícone de funil com **X** no canto da segmentação.
4. Os cards e o gráfico se atualizam na hora.

---

## 📈 Principais insights

- **Mensal é o carro-chefe:** com R$ 3.571,00, os planos mensais são **47%** do faturamento total (R$ 7.633,00).
- **Renovação automática quase não aparece no mensal:** entre os mensais, só **21%** do faturamento (R$ 747,00) vem de clientes com renovação automática. Nos anuais, essa fatia sobe para **88%** (R$ 1.537,00). Incentivar a renovação automática no plano mensal é uma oportunidade clara de receita recorrente.
- **Complementos pesam:** EA Play (R$ 2.940,00) e Minecraft Season Pass (R$ 3.880,00) somam **R$ 6.820,00**, valor próximo ao faturamento total. Isso mostra o peso dos passes na composição do ticket.
- **Ultimate concentra a receita:** o plano Ultimate gera **R$ 5.388,00 (71%)** do total, pois é o único com EA Play.

---

## 🧰 Recursos do Excel utilizados

- Tabela do Excel (`Tabela1`) como fonte de dados
- Tabelas Dinâmicas com filtro de relatório e nomes personalizados
- `INFOPIVOTDADOS` (`GETPIVOTDATA`) para levar resultados ao painel
- Segmentação de Dados conectada a várias tabelas dinâmicas (Conexões de Relatório)
- Estilo de segmentação personalizado
- Gráfico Dinâmico de barras
- Formas com texto vinculado a célula (cards de KPI dinâmicos)
- Imagens, recorte em elipse e agrupamento de objetos
- Tema de cores personalizado
- Abas ocultas e linhas de grade e títulos ocultos para visual de aplicativo

---

## 👤 Autor

<table>
  <tr>
    <td>
      <strong>Pedro Christo</strong><br>
      Projeto do bootcamp Santander – Excel com IA (Claude) · DIO<br><br>
      <a href="https://www.linkedin.com/in/pedro-christo-9001b62b4/">
        <img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-Pedro%20Christo-0074C5?style=for-the-badge&logo=linkedin&logoColor=white">
      </a>
    </td>
  </tr>
</table>

> Os dados são fictícios e usados apenas para fins educacionais. Xbox, Game Pass, EA Play e Minecraft são marcas de seus respectivos proprietários.
