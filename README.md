# 🛡️ PortSwigger SQLi Automation

Este repositório contém Provas de Conceito (PoCs) em Python desenvolvidas para automatizar a exploração de vulnerabilidades de **SQL Injection** nos laboratórios da plataforma PortSwigger Web Security Academy.

O projeto foi criado para demonstrar a capacidade de criar ferramentas customizadas (offensive security tooling) e entender a fundo as lógicas de banco de dados e mecanismos de defesa (WAFs).

## 🛠️ Técnicas Implementadas

*   **Error-Based SQL Injection (`/error_based`):** Extrai dados forçando o banco de dados (PostgreSQL) a gerar erros de conversão (CAST). Utiliza Regex para capturar a senha diretamente da mensagem de erro HTTP 500.
*   **Time-Based Blind SQLi (`/time_based`):** Exploração cega utilizando a função `pg_sleep()` e **Busca Binária (ASCII)** para reduzir o tempo de extração da senha de horas para minutos.
*   **WAF Bypass via XML Encoding (`/waf_bypass_xml`):** Exploração de Union-Based SQLi burlando o firewall da aplicação. O payload é codificado em entidades XML (hexadecimal) para enganar o WAF, enquanto o servidor decodifica e executa a query. Utiliza concatenação (`||`) do PostgreSQL.

## 🧠 Aprendizados e Desafios Técnicos

Durante o desenvolvimento, foram superados desafios reais de infraestrutura e lógica de banco de dados:

*   **Bypass de Otimização de Query:** Entendimento de como o PostgreSQL otimiza queries, evitando falsos positivos em injeções baseadas em tempo.
*   **Manipulação de Tipos de Dados:** Compreensão da diferença de tipos (`void`, `int`, `text`) e como forçar erros de conversão para vazar dados.
*   **Parsing de Erros:** Uso da biblioteca `re` (Regex) do Python para filtrar o "lixo" da tela de erro e extrair apenas a informação sensível.
*   **Bypass de WAF:** Compreensão de como os WAFs analisam assinaturas de texto puro e como a codificação de entidades XML pode evadir filtros de segurança.
*   **Concatenação SQL:** Uso do operador `||` para combinar múltiplas colunas (username e password) em uma única saída de dados.

## ⚙️ Como Utilizar

### Pré-requisitos
*   Python 3.10 ou superior.
*   Biblioteca `requests` (`pip install requests`).

### Execução (Exemplo Error-Based)
Para rodar o script, capture a URL, o cookie `session` e o cookie `TrackingId` usando o Burp Suite:

```bash
python3 error_based/exploit_error.py -u "https://SEU-LAB.web-security-academy.net/" -s "VALOR_DO_SESSION" -t "VALOR_DO_TRACKINGID"
