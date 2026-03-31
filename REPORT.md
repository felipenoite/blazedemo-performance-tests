# 📊 Relatório de Execução — BlazeDemo Performance Tests

Ferramenta: **Apache JMeter 5.6.3**

---

## 🧪 Configuração dos Testes

| Parâmetro | Teste de Carga | Teste de Pico |
|-----------|---------------|---------------|
| Usuários (Threads) | 250 | 250 |
| Ramp-up | 10 segundos | 1 segundo |
| Loop Count | 1 | 1 |

---

## 📈 Resultados — Execução Local (Windows)

Data de execução: **31/03/2026**

### Teste de Carga

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 525 | 723 | 1094 | 0.00% | 24.5/sec |
| POST - Reserve | 250 | 299 | 349 | 364 | 0.00% | 27.5/sec |
| POST - Purchase | 250 | 300 | 355 | 383 | 0.00% | 27.4/sec |
| **TOTAL** | **750** | **375** | **482** | **550** | **0.00%** | **69.3/sec** |

### Teste de Pico

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 706 | 961 | 1094 | 0.00% | 90.0/sec |
| POST - Reserve | 250 | 618 | 909 | 1167 | 0.00% | 84.6/sec |
| POST - Purchase | 250 | 546 | 873 | 1086 | 0.00% | 85.0/sec |
| **TOTAL** | **750** | **623** | **924** | **1112** | **0.00%** | **203.4/sec** |

---

## 📈 Resultados — Execução em CI (GitHub Actions / Ubuntu)

### Teste de Carga

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 301.83 | 374.70 | 719.60 | 0.00% | 24.74/sec |
| POST - Reserve | 250 | 202.18 | 237.90 | 261.35 | 0.00% | 26.88/sec |
| POST - Purchase | 250 | 205.43 | 245.00 | 271.45 | 0.00% | 26.93/sec |
| **TOTAL** | **750** | **236.48** | **285.00** | **326.80** | **0.00%** | **71.42/sec** |

### Teste de Pico

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 1754.80 | 2281.00 | 2501.45 | 0.00% | 74.47/sec |
| POST - Reserve | 250 | 802.49 | 1309.70 | 1449.00 | 0.00% | 101.30/sec |
| POST - Purchase | 250 | 628.26 | 1223.90 | 1324.35 | 0.00% | 103.56/sec |
| **TOTAL** | **750** | **1061.85** | **1953.90** | **2179.80** | **0.00%** | **163.15/sec** |

---

## 🔍 Análise dos Critérios de Aceitação

### Critério 1 — P90 < 2000ms

#### Execução Local
| Teste | P90 Total | Limite | Resultado |
|-------|-----------|--------|-----------|
| Carga | 482ms | 2000ms | ✅ 76% abaixo do limite |
| Pico | 924ms | 2000ms | ✅ 54% abaixo do limite |

#### Execução CI
| Teste | P90 Total | Limite | Resultado |
|-------|-----------|--------|-----------|
| Carga | 285ms | 2000ms | ✅ 86% abaixo do limite |
| Pico | 1953.90ms | 2000ms | ⚠️ 46ms abaixo do limite |

> ⚠️ No teste de pico em CI, o endpoint **GET - Home** atingiu P90 de **2281ms**, ultrapassando o limite de 2000ms.

### Critério 2 — Throughput de 250 req/s

| Ambiente | Teste | Throughput Obtido | Meta | Resultado |
|----------|-------|------------------|------|-----------|
| Local | Carga | 69.3 req/s | 250 req/s | ⚠️ Não atingido |
| Local | Pico | 203.4 req/s | 250 req/s | ⚠️ Não atingido |
| CI | Carga | 71.42 req/s | 250 req/s | ⚠️ Não atingido |
| CI | Pico | 163.15 req/s | 250 req/s | ⚠️ Não atingido |

---

## 💡 Conclusão

### Critério de P90
O critério de P90 < 2000ms foi **aprovado na execução local** nos dois cenários e **aprovado no CI para o teste de carga**. Porém, no **teste de pico em CI**, o P90 total ficou em 1953ms — apenas 46ms abaixo do limite — e o endpoint **GET - Home** ultrapassou o limite com 2281ms.

Essa divergência entre os ambientes evidencia que o teste de pico é sensível à latência de rede e capacidade do servidor. Em ambiente local a conexão é mais estável, enquanto no CI a carga simultânea de 250 usuários com ramp-up de 1 segundo pressionou mais o servidor do BlazeDemo, que é um ambiente de demonstração sem garantias de SLA.

### Critério de Throughput
O critério de 250 req/s **não foi atingido** em nenhum ambiente ou cenário. Isso ocorre porque o throughput é diretamente influenciado pelo tempo de resposta do servidor — quanto mais lenta a resposta, menos requisições por segundo são processadas. Com 250 usuários executando 1 loop cada, o volume total de requisições se esgota antes de sustentar 250 req/s de forma contínua.

Para atingir 250 req/s de forma sustentada seria necessário:
- Aumentar o número de usuários simultâneos, ou
- Aumentar o número de loops para manter o fluxo contínuo, ou
- Combinar as duas abordagens

### Consideração Final
Apesar do critério de throughput não ter sido atingido, o resultado mais crítico — o **P90** — foi satisfeito na grande maioria dos cenários, com o servidor demonstrando boa resiliência com **0.00% de erros** em todas as execuções.