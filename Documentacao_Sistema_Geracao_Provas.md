# 🧾 Documentação do Projeto — Sistema de Geração de Provas

**Nome do Projeto:** Sistema de Geração de Provas  
**Curso:** Análise e Desenvolvimento de Sistemas - FATEC Guarulhos  
**Autores:** Ryan Vincente, Luiz Pergorari, Guilherme Olivatto, Rafael Narciso, Gustavo Lima
**Orientador:** Jadir

## Sumário

1. Visão geral / Introdução

2. Objetivos e escopo

3. Stakeholders

4. Requisitos Funcionais e Não funcionais

5. Casos de uso / Fluxos do Usuário

6. Modelagem do Sistema (DER / Tabelas)

7. Requisitos de Interface (UI / Wireframes)

8. Arquitetura e diagrama de componentes

9. Implantação / Deployment

10. Teste

11. Fluxos do Sistema (Diagramas de Sequência)



### 1. Visão Geral / Introdução
A elaboração manual de provas demanda tempo e disciplina. O EasyQuiz é um sistema web que facilita a criação, o gerenciamento e a exportação de provas em PDF. Professores poderão cadastrar-se, criar provas com até 20 questões, reutilizar questões do repositório e exportar provas com cabeçalho institucional.
---

## 2. Objetivos e Escopo

### Objetivo Geral
Desenvolver um sistema web que permita que professores criem, gerenciem e exportem provas de forma prática e segura.

### Objetivos Específicos
- Permitir o **cadastro e autenticação de professores** com validação de CPF.  
- Implementar um **painel de controle** para criação, edição e exclusão de provas.   
- Organizar as provas por **matéria**.  
- Permitir **exportação das provas em formato PDF customizável**.  
- Garantir a **segurança de dados e usabilidade** da aplicação.

### Escopo
**Inclui:**
- Cadastro, login e autenticação de usuários.  
- CRUD de provas e questões.  
- Geração de PDF.  

**Não Inclui:**
- Aplicação das provas online.  
- Correção automática ou avaliação de desempenho.  
- Múltiplos perfis de usuário (alunos, coordenadores, etc).

---

## 3. Stakeholders

1. Professor (usuário principal)

2. Administrador do sistema

3. Equipe de desenvolvimento

## 4. Requisitos

### 4.1 Requisitos Funcionais (RF)
| Código | Descrição                                                                                                             |
| ------ | --------------------------------------------------------------------------------------------------------------------- |
| RF01   | O sistema deve permitir o cadastro de professores realizado **apenas por um usuário ADMIN**.                          |
| RF02   | O sistema deve permitir o login por e-mail e senha.                                                                   |
| RF03   | O sistema deve enviar automaticamente uma senha gerada ao e-mail do professor cadastrado.                             |
| RF04   | O sistema deve registrar em log todas as ações de criação, edição e exclusão de usuários realizadas por ADMIN (LogCRUD).          |
| RF05   | O sistema deve permitir o CRUD de disciplinas.                                                                        |
| RF06   | O sistema deve associar professores às disciplinas.                                                                   |
| RF07   | O sistema deve manter um repositório de questões com: enunciado, tipo, dificuldade, autor da questão e alternativas (quando aplicável). |
| RF08   | O professor pode criar provas utilizando questões do repositório.                                                     |
| RF09   | Não tem Limite de questões.                                                                           |
| RF10   | O sistema deve permitir gerar **PDF personalizado** contendo Nome/RA/Data/Logo da instituição.                        |                                  |


### 4.2 Requisitos Não Funcionais (RNF)
| Código | Descrição                                                                                                             |
| ------ | --------------------------------------------------------------------------------------------------------------------- |
| RNF01  | O sistema deve utilizar **SMTP com TLS** para envio de e-mails.          |
| RNF02  | O sistema deve disponibilizar documentação da API via **Springdoc OpenAPI / Swagger**. |
| RNF03  | O sistema deve utilizar MySQL como banco de dados principal.                           |
| RNF04  | A arquitetura deve ser desenvolvida em Spring Boot 3.x.                                |
| RNF05  | A geração de PDF não deve ultrapassar 5 segundos.                                      |


---

## 5. Casos de Uso / Fluxos do Usuário

## UC01 — Efetuar Login

Atores: Administrador, Professor
Descrição: Permite o acesso ao sistema mediante autenticação de credenciais.
Fluxo Principal:

- Usuário acessa a tela de login.

- Informa e-mail e senha.

- O sistema valida as credenciais.

- Usuário é redirecionado ao painel correspondente ao seu perfil (ADMIN ou PROFESSOR).
Fluxo Alternativo:

- Se as credenciais estiverem incorretas, o sistema exibe mensagem de erro.

## UC02 — Cadastrar Professor

Atores: Administrador
Descrição: Permite ao administrador cadastrar novos professores no sistema.
Fluxo Principal:

- Administrador acessa o menu “Usuários”.

- Informa nome, e-mail e tipo de usuário (Professor).

- O sistema gera uma senha temporária e envia por e-mail ao professor.

- O sistema registra a operação em log de auditoria (log_crud).
Fluxo Alternativo:

- Se o e-mail informado já estiver cadastrado, o sistema exibe mensagem de erro.

## UC03 — Gerar PDF da Prova

Atores: Professor ou adminitrador
Descrição: Permite ao professor criar uma prova associada a uma disciplina.
Fluxo Principal:

- Professor acessa o menu “Criar Prova”.

- Preenche as informações da prova.

- Escolhe o conteúdo completo da prova (questões) em campo de texto.

- Clica em Exportar pra PDF.

- O sistema gera o download da prova.
Fluxo Alternativo:

- Se algum campo obrigatório (título ou conteúdo) não for preenchido, o sistema exibe alerta.

## UC04 — Cadastrar Disciplina

Atores: Adminitrador
Descrição: Permite ao Administrador criar uma disciplina.
Fluxo Principal:

- Adminitrador acessa o menu “Disciplinas”.

- Preenche o nome da disciplina.

- Clica em cadastrar.

Fluxo Alternativo:

- Se já existir uma disciplina com este nome, o sistema exibe uma mensagem de alerta.

 ## UC05 — Gerenciar Questões

Atores: Professor ou Adminitrador
Descrição: Permite ao professor ou Administrador gerenciar questões criadas.
Fluxo Principal:

- professor ou administrador acessa o menu "Minhas Questões".

- Preenche o nome da disciplina.

- Clica em cadastrar.

Fluxo Alternativo:

- Se já existir uma disciplina com este nome, o sistema exibe uma mensagem de alerta.

CASO DE USO: <img width="536" height="585" alt="image" src="https://github.com/user-attachments/assets/a69e50a4-e4c6-4d1c-af0e-d671affb41ec" />

---

## 6. Modelagem do sistema (DER)

- **usuario**: Armazena os dados dos usuários do sistema, podendo ser Administrador, Professor ou Usuário Público.
Atributos principais: id, nome, email, senha_hash, tipo, criado_em.

 - **disciplina**: Lista das disciplinas cadastradas no sistema.
Atributos: id, nome.

- **professor_disciplina**: Tabela associativa que estabelece o relacionamento muitos-para-muitos entre professor e disciplina.

- **questao**: Representa uma pergunta cadastrada no sistema, podendo ser de diferentes tipos (Dissertativa, Alternativa, Verdadeiro/Falso) e com níveis de dificuldade.
Atributos: id, titulo, descricao, tipo, dificuldade, disciplina_id, criado_por, data_criacao, data_ultima_modificacao.

- **opcao_resposta**: Armazena as alternativas de questões do tipo objetiva.
Atributos: id, questao_id, texto_resposta, correta.

- **log_crud**: Tabela de auditoria, registrando ações administrativas como criação e exclusão de professores.
Atributos: id, admin_id, acao, registro_afetado, data_hora.

DER: <img width="887" height="526" alt="image" src="https://github.com/user-attachments/assets/c6764280-97cd-47d1-8573-0b25374878e0" />


---

## 7. Requisitos de Interface (UI / Wireframes)

### 7.1 Telas do Sistema

| Tela                                                             | Descrição Corporativa                                                                                                                                                                                            |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tela de Login**                                                | Porta de entrada do sistema. Permite autenticação via e-mail e senha. Em caso de credenciais inválidas, retorna feedback imediato ao usuário.                                                                    |
| **Dashboard**                                                    | Painel gerencial com indicadores-chave: total de questões cadastradas, contagem por tipo (Múltipla Escolha, V/F, Dissertativa) e atalho rápido para ações estratégicas como Criar Questão e Gerar Prova.         |
| **Criar Nova Questão**                                           | Tela transacional destinada ao cadastro de questões. Permite definir enunciado, disciplina, grau de dificuldade, tipo da questão e alternativas (quando aplicável).                                              |
| **Minhas Questões**                                              | Repositório individual das questões cadastradas pelo usuário. Possibilita editar, excluir e visualizar detalhes. Inclui ações rápidas de manutenção.                                                             |
| **Explorar Questões**                                            | Tela estratégica de busca avançada, permitindo filtros por: disciplina, autor da questão, dificuldade e tipo. Viabiliza curadoria eficiente do banco de questões antes da montagem da prova.                     |
| **Gerar Prova**                                                  | Interface de composição da prova. O usuário seleciona as questões desejadas e define o cabeçalho institucional (nome da instituição, curso, disciplina, professor e turma). Gera o PDF finalizado.               |
| **Cadastrar Disciplina (ADMIN)**                                 | Tela administrativa para criação, edição e exclusão de disciplinas. Suporta operação contínua de catalogação do currículo.                                                                                       |
| **Cadastrar Usuário (ADMIN)**                                    | Permite ao administrador criar novos usuários (professores). Inclui nome, e-mail e perfil. A senha é enviada automaticamente por e-mail (conforme política de segurança).                                        |
| **Logs de Auditoria (ADMIN)**                                    | Tela analítica que consolida as ações críticas do sistema: criação, edição e exclusão de usuários, disciplinas e questões. Permite rastreabilidade total para fins de conformidade (LGPD e auditorias internas). |
| **Visualizar Provas Geradas** *(opcional – confirmar se existe)* | Caso exista listagem, permite acesso às provas já geradas e exportações anteriores. *(Se este recurso não existe no sistema, removemos depois.)*                                                                 |




---

## 8. Arquitetura e diagrama de componentes

## 8.1 Arquitetura do Sistema

O sistema EasyQuiz foi desenvolvido seguindo o modelo cliente-servidor com arquitetura em camadas, visando modularidade, escalabilidade e facilidade de manutenção.
A aplicação é composta por três camadas principais:

## 1. Camada de Apresentação (Front-end)

- Responsável pela interface com o usuário (UI).

- Desenvolvida em Next.js com Tailwind CSS para estilização.

- Comunicação com o backend via requisições HTTP (REST API).

- Responsável por renderizar páginas como Login, Dashboard, Criação de Prova e Geração de PDF.

2. Camada de Lógica de Negócio (Back-end)

- Implementada em Java com SpringBoot

- Contém as regras de negócio: cadastro de usuários, controle de acesso, criação e edição de provas, e geração de logs.

- Expõe endpoints RESTful consumidos pelo front-end.

- Faz integração com biblioteca de geração de PDF (PDFMAKE).

- Controla a persistência dos dados no banco.

## 3. Camada de Dados (Banco de Dados)

- Utiliza MySQL como SGBD relacional.

- Responsável pelo armazenamento persistente de usuários, disciplinas, questões e logs.

- Comunicação com o backend via Spring Data JPA.

- Integridade garantida por chaves estrangeiras e constraints definidas no DER.

## 4. Integrações e Serviços de Suporte

- Envio de e-mails (cadastro de professores) via SMTP/TLS.

- Hospedagem em ambiente Railway ou Render, com deploy automatizado.

## 8.2 Diagrama de Componentes
<img width="1178" height="576" alt="image" src="https://github.com/user-attachments/assets/e5061286-c121-4f68-9f8b-087d4756bbe2" />

## 8.3 Tecnologias Utilizadas

| Camada         | Tecnologia                   | Função                                   |
| -------------- | ---------------------------- | ---------------------------------------- |
| Front-end      | **Next.js / Tailwind CSS**   | Interface responsiva e rápida            |
| Back-end       | **Spring Boot 3.x**          | Lógica de negócio e APIs REST            |
| Banco de Dados | **MySQL**                    | Persistência de dados                    |
| E-mail         | **JavaMail / SMTP**          | Envio automático de senha e notificações |
| PDF            | **PDFMAKE **                 | Geração e formatação de provas           |

---

## 9. Implantação / Deployment

## 9.1 Arquitetura de Hospedagem

| Componente     | Serviço                            | Descrição                          |
| -------------- | ---------------------------------- | ---------------------------------- |
| Frontend       | **Vercel (Next.js)**               | Hospedagem do cliente web          |
| Backend        | **Render (Spring Boot em Docker)** | API REST e lógica de negócio       |
| Banco de Dados | **TiDB Cloud (MySQL compatível)**  | Armazenamento relacional escalável |


## 9.2 Banco de Dados em TiDB Cloud

- Instância provisionada na nuvem via painel da TiDB Cloud

- Alta disponibilidade e compatibilidade total com MySQL

- Credenciais protegidas (NÃO estão no repositório)

- Conexão via variáveis de ambiente no Render

  ##Variáveis configuradas no Render:

      DB_URL

      DB_USERNAME

      DB_PASSWORD

## 9.3 Backend em Render (Spring Boot)

- Atualizado para Java 21 no pom.xml

- Aplicação containerizada via Dockerfile

    - Stage 1: Build (Maven + temurin 21)

    - Stage 2: Execução leve (temurin 21 alpine)

- application.properties configurado com placeholders:

    - ${DB_URL}, ${MAIL_USERNAME}, ${MAIL_PASSWORD} etc.

- CORS habilitado apenas para:
   - https://easyquiz-psi.vercel.app
 
 ## 9.4 Frontend na Vercel (Next.js)

- Conectado ao GitHub

- Variável NEXT_PUBLIC_API_URL configurada

- Next.config.ts ajustado para melhorar o build

- API centralizada em /src/services/api.ts

## 9.5 Fluxo de Deploy Contínuo (CI/CD)

- Commits na branch main disparam:

- novo build do backend no Render

- novo build do frontend na Vercel







### Tipos de Testes
- **Unitários:** Testar funções individuais (ex.: validação de CPF).  
- **Integração:** Cadastro, login e fluxo de criação de prova.  
- **Aceitação:** Geração correta do PDF e limite de 20 questões.  

### Casos de Teste Exemplo
| Código | Descrição | Resultado Esperado |
|---------|------------|--------------------|
| CT01 | Cadastro com CPF inválido | Exibir mensagem de erro |
| CT02 | Login com senha incorreta | Bloquear acesso |
| CT03 | Criar prova com 21 questões | Rejeitar última questão |
| CT04 | Gerar PDF | Criar arquivo formatado corretamente |


---
## 10. Fluxos do Sistema (Diagramas de Sequência)
---

## 📚 Referências
- PRESSMAN, Roger S. *Engenharia de Software: uma abordagem profissional.* McGraw Hill, 2016.  
- SOMMERVILLE, Ian. *Engenharia de Software.* 10ª ed. Pearson, 2019.  
- GIL, Antonio Carlos. *Como Elaborar Projetos de Pesquisa.* Atlas, 2018.  
- Documentação do Node.js — https://nodejs.org/  
- Documentação do jsPDF — https://github.com/parallax/jsPDF  
