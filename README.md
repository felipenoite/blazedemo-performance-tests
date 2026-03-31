# ✈️ BlazeDemo Performance Tests

Projeto de testes de performance para o fluxo de compra de passagem aérea do [BlazeDemo](https://www.blazedemo.com), desenvolvido com **Apache JMeter 5.6.3**.

---

## 📋 Índice

- [Cenário Testado](#-cenário-testado)
- [Critério de Aceitação](#-critério-de-aceitação)
- [Pré-requisitos](#-pré-requisitos)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Executando os Testes](#-executando-os-testes)
- [Pipeline CI/CD](#-pipeline-cicd)

---

## 🎯 Cenário Testado

Fluxo completo de compra de passagem aérea:

| Passo | Método | Endpoint | Descrição |
|-------|--------|----------|-----------|
| 1 | GET | `/` | Página inicial |
| 2 | POST | `/reserve.php` | Seleção do voo (Paris → Buenos Aires) |
| 3 | POST | `/purchase.php` | Confirmação da compra |

---

## ✅ Critério de Aceitação

- **Throughput:** 250 requisições por segundo
- **Tempo de resposta:** P90 inferior a 2000ms

---

## 🛠 Pré-requisitos

- **Java 17+** — [Download](https://adoptium.net/)
- **Apache JMeter 5.6.3** — [Download](https://jmeter.apache.org/download_jmeter.cgi)

---

## 📁 Estrutura do Projeto
```
blazedemo-tests/
├── .github/
│   └── workflows/
│       └── performance.yml        # Pipeline CI/CD
├── scripts/
│   ├── blazedemo-load-test.jmx   # Teste de carga
│   └── blazedemo-spike-test.jmx  # Teste de pico
├── results/
│   ├── load-test-results.csv     # Resultados do teste de carga
│   └── spike-test-results.csv    # Resultados do teste de pico
├── README.md                      # Instruções de configuração e execução
└── REPORT.md                      # Relatório de execução e análise
```

---

## ▶️ Executando os Testes

### Pela interface gráfica do JMeter

1. Abra o JMeter (`bin/jmeter.bat` no Windows)
2. **File → Open** e selecione o arquivo `.jmx` desejado
3. Clique em **Play (▶)** ou pressione **Ctrl + R**
4. Acompanhe os resultados no **Aggregate Report**

### Pela linha de comando (recomendado para CI)
```bash
# Teste de carga
jmeter -n -t scripts/blazedemo-load-test.jmx -l results/load-test-results.csv

# Teste de pico
jmeter -n -t scripts/blazedemo-spike-test.jmx -l results/spike-test-results.csv
```

---

## 🔄 Pipeline CI/CD

O projeto inclui um workflow do **GitHub Actions** em `.github/workflows/performance.yml`.

### Disparadores
- Push para `main` ou `master`
- Pull Request para `main` ou `master`
- Execução manual via `workflow_dispatch`

### O que o pipeline faz
1. Checkout do código
2. Configura Java 17
3. Instala o JMeter 5.6.3
4. Executa o teste de carga
5. Executa o teste de pico
6. Faz upload dos resultados como artefato

---

## 👤 Autor

Desenvolvido como parte do processo seletivo para a vaga de QA na **Mirante**.