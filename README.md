# 🚀 Desafio Técnico: Automação de Matrículas (4blue)

Este repositório contém a solução desenvolvida para o desafio técnico de automação utilizando **n8n**. O fluxo orquestra o recebimento de dados de novos alunos, realiza o tratamento e higienização das informações, roteia de acordo com regras de negócio, salva em banco de dados e dispara comunicações automatizadas.

## 📺 Apresentação e Demonstração
Para uma explicação detalhada da arquitetura e das decisões de engenharia tomadas, assista ao vídeo de demonstração:
👉 **(https://youtu.be/e_q_aLzEk0E)**

---

## ⚙️ Arquitetura do Fluxo

O workflow foi estruturado nos seguintes nós:

1. **Webhook (Trigger):** Recebe o payload (método POST) com os dados brutos do aluno.
2. **Edit Fields (Tratamento de Dados):**
   - Padronização de strings: Nome convertido para iniciais maiúsculas.
   - Higienização: Remoção de caracteres especiais do campo `telefone`.
   - Conversão de Tipo: Data de nascimento formatada para o padrão ISO (`YYYY-MM-DD`).
3. **Switch (Roteamento):** Encaminha os dados do aluno para as tabelas correspondentes baseadas na variável `curso` (Tech ou Business).
4. **Data Table / PostgreSQL:** Inserção dos registros higienizados no banco de dados da plataforma.
5. **Send Email (SMTP):** Disparo de e-mail de boas-vindas customizado, ativado apenas após o sucesso da transação no banco de dados.

---

## 🕵️ Resolução de Problemas e "Desafio Oculto"

Durante o desenvolvimento, foquei fortemente na **integridade dos dados** e na resiliência do sistema, aplicando duas correções fundamentais em relação à documentação inicial:

- **Correção de Nomenclatura (Desafio Oculto):** A especificação exigia o uso da variável `email_aluno`, porém o payload recebido fornecia a chave `email`. Para evitar falhas de comunicação, a variável foi mapeada e padronizada logo no nó de tratamento, garantindo que o fluxo não quebrasse.
- **Prevenção de Duplicidade (Upsert):** Um fluxo linear de inserção causaria duplicidade de cadastros caso o Webhook fosse acionado múltiplas vezes para o mesmo aluno. Para mitigar esse erro operacional grave, o nó de banco de dados foi configurado com a operação de **Upsert** utilizando o e-mail como chave única de verificação.

---

## 🛠️ Como importar este projeto
Se desejar visualizar o fluxo em sua própria instância do n8n:
1. Faça o download do arquivo `desafio-automacao-4blue.json` presente neste repositório.
2. Em seu n8n, vá em *Workflows* > *Import from File* e selecione o arquivo.

---
Desenvolvido por **Maurício Viégas** [LinkedIn]www.linkedin.com/in/maurícioviegas | Estudante de Ciência da Computação
