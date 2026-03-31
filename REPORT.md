# 📊 Relatório de Execução — BlazeDemo Performance Tests

Data de execução: **31/03/2026**
Ferramenta: **Apache JMeter 5.6.3**
Ambiente: **Local (Windows)**

---

## 🧪 Configuração dos Testes

| Parâmetro | Teste de Carga | Teste de Pico |
|-----------|---------------|---------------|
| Usuários (Threads) | 250 | 250 |
| Ramp-up | 10 segundos | 1 segundo |
| Loop Count | 1 | 1 |

---

## 📈 Resultados

### Teste de Carga
Configuração: **250 usuários | Ramp-up: 10 segundos | 1 loop**

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 525 | 723 | 1094 | 0.00% | 24.5/sec |
| POST - Reserve | 250 | 299 | 349 | 364 | 0.00% | 27.5/sec |
| POST - Purchase | 250 | 300 | 355 | 383 | 0.00% | 27.4/sec |
| **TOTAL** | **750** | **375** | **482** | **550** | **0.00%** | **69.3/sec** |

### Teste de Pico
Configuração: **250 usuários | Ramp-up: 1 segundo | 1 loop**

| Requisição | Amostras | Média (ms) | P90 (ms) | P95 (ms) | Erro % | Throughput |
|-----------|----------|------------|----------|----------|--------|------------|
| GET - Home | 250 | 706 | 961 | 1094 | 0.00% | 90.0/sec |
| POST - Reserve | 250 | 618 | 909 | 1167 | 0.00% | 84.6/sec |
| POST - Purchase | 250 | 546 | 873 | 1086 | 0.00% | 85.0/sec |
| **TOTAL** | **750** | **623** | **924** | **1112** | **0.00%** | **203.4/sec** |

---

## 🔍 Análise dos Critérios de Aceitação

### Critério 1 — P90 < 2000ms
✅ **Aprovado** nos dois testes:

| Teste | P90 Total | Limite | Resultado |
|-------|-----------|--------|-----------|
| Carga | 482ms | 2000ms | ✅ 76% abaixo do limite |
| Pico | 924ms | 2000ms | ✅ 54% abaixo do limite |

### Critério 2 — Throughput de 250 req/s
⚠️ **Não atingido** em nenhum dos testes:

| Teste | Throughput Obtido | Meta | Resultado |
|-------|------------------|------|-----------|
| Carga | 69.3 req/s | 250 req/s | ⚠️ 72% abaixo da meta |
| Pico | 203.4 req/s | 250 req/s | ⚠️ 19% abaixo da meta |

---

## 💡 Conclusão

O critério de **P90 abaixo de 2000ms foi amplamente satisfeito** nos dois cenários, demonstrando que o servidor do BlazeDemo responde de forma estável e dentro do tempo esperado mesmo sob carga simultânea de 250 usuários.

O critério de **throughput de 250 req/s não foi atingido**. Isso ocorre porque o throughput é diretamente influenciado pelo tempo de resposta do servidor — quanto mais tempo o servidor leva para responder, menos requisições por segundo são processadas. Com 250 usuários executando 1 loop cada, o volume total de requisições se esgota antes de sustentar 250 req/s de forma contínua.

Para atingir 250 req/s de forma sustentada seria necessário:
- Aumentar o número de usuários simultâneos, ou
- Aumentar o número de loops para manter o fluxo contínuo de requisições, ou
- Combinar as duas abordagens

Vale destacar que o **teste de pico chegou a 203.4 req/s** — próximo da meta — mesmo com o servidor sob pressão máxima de ramp-up de 1 segundo, e ainda assim sem nenhum erro registrado (Error % = 0.00%), o que é um indicativo positivo de resiliência da aplicação.