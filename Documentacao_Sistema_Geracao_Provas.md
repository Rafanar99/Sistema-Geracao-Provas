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

9. APIs

10. Regras de negócio detalhadas

11. Validações e constraints (ex.: CPF, limite 20 questões)

12. Geração de PDF (detalhes técnicos)

13. Segurança e conformidade (LGPD)

14. Testes (unitários, integração, aceitação)

15. Implantação / DevOps / backup



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
| RF04   | O sistema deve registrar em log todas as ações de criação, edição e exclusão realizadas por ADMIN (LogCRUD).          |
| RF05   | O sistema deve permitir o CRUD de disciplinas.                                                                        |
| RF06   | O sistema deve associar professores às disciplinas.                                                                   |
| RF07   | O sistema deve manter um repositório de questões com: enunciado, tipo, dificuldade e alternativas (quando aplicável). |
| RF08   | O professor pode criar provas utilizando questões do repositório.                                                     |
| RF09   | Não tem Limite de questões.                                                                           |
| RF10   | O sistema deve permitir gerar **PDF personalizado** contendo Nome/RA/Data/Logo da instituição.                        |                                  |


### 4.2 Requisitos Não Funcionais (RNF)
| Código | Descrição                                                                              |
| ------ | -------------------------------------------------------------------------------------- |
| RNF01  | Senhas devem ser armazenadas com hash seguro (bcrypt/argon2).                          |
| RNF02  | O sistema deve utilizar autenticação baseada em tokens (JWT).                          |
| RNF03  | O sistema deve utilizar **SMTP com TLS** para envio de e-mails transacionais.          |
| RNF04  | O sistema deve disponibilizar documentação da API via **Springdoc OpenAPI / Swagger**. |
| RNF05  | O sistema deve utilizar MySQL como banco de dados principal.                           |
| RNF06  | A arquitetura deve ser desenvolvida em Spring Boot 3.x.                                |
| RNF07  | A geração de PDF não deve ultrapassar 5 segundos.                                      |
| RNF08  | Deve estar em conformidade com padrões mínimos da LGPD.                                |


---

## 5. Casos de Uso / Fluxos do Usuário

UC01 — Efetuar Login

Atores: Administrador, Professor
Descrição: Permite o acesso ao sistema mediante autenticação de credenciais.
Fluxo Principal:

Usuário acessa a tela de login.

Informa e-mail e senha.

O sistema valida as credenciais.

Usuário é redirecionado ao painel correspondente ao seu perfil (ADMIN ou PROFESSOR).
Fluxo Alternativo:

Se as credenciais estiverem incorretas, o sistema exibe mensagem de erro.

UC02 — Cadastrar Professor

Atores: Administrador
Descrição: Permite ao administrador cadastrar novos professores no sistema.
Fluxo Principal:

Administrador acessa o menu “Usuários”.

Informa nome, e-mail e tipo de usuário (Professor).

O sistema gera uma senha temporária e envia por e-mail ao professor.

O sistema registra a operação em log de auditoria (log_crud).
Fluxo Alternativo:

Se o e-mail informado já estiver cadastrado, o sistema exibe mensagem de erro.

UC03 — Criar Prova

Atores: Professor
Descrição: Permite ao professor criar uma prova associada a uma disciplina.
Fluxo Principal:

Professor acessa o menu “Criar Prova”.

Seleciona a disciplina.

Informa o título da prova.

Digita o conteúdo completo da prova (questões e cabeçalho) em campo de texto.

Clica em Salvar.

O sistema grava a prova no banco e exibe mensagem de sucesso.
Fluxo Alternativo:

Se algum campo obrigatório (título ou conteúdo) não for preenchido, o sistema exibe alerta.

UC04 — Listar / Editar / Excluir Provas

Atores: Professor
Descrição: Permite ao professor visualizar todas as provas criadas, editar conteúdo ou excluir.
Fluxo Principal:

Professor acessa o menu “Minhas Provas”.

O sistema exibe lista de provas criadas, com título, disciplina e data.

Professor pode:

Clicar em Editar para alterar o conteúdo.

Clicar em Excluir para remover a prova.
Fluxo Alternativo:

Se não houver provas cadastradas, o sistema exibe mensagem “Nenhuma prova criada”.

UC05 — Gerar PDF da Prova

Atores: Professor
Descrição: Permite gerar um arquivo PDF formatado da prova criada.
Fluxo Principal:

Professor seleciona uma prova existente.

Clica em Gerar PDF.

O sistema formata o conteúdo e adiciona cabeçalho (nome do professor, disciplina, data).

O sistema disponibiliza o arquivo para download ou impressão.
Fluxo Alternativo:

Se a prova estiver vazia, o sistema exibe alerta e bloqueia a geração do PDF.


---

## 6. Modelagem do sistema (DER)

usuario: Armazena os dados dos usuários do sistema, podendo ser Administrador, Professor ou Usuário Público.
Atributos principais: id, nome, email, senha_hash, tipo, criado_em.

disciplina: Lista das disciplinas cadastradas no sistema.
Atributos: id, nome.

professor_disciplina: Tabela associativa que estabelece o relacionamento muitos-para-muitos entre professor e disciplina.

questao: Representa uma pergunta cadastrada no sistema, podendo ser de diferentes tipos (Dissertativa, Alternativa, Verdadeiro/Falso) e com níveis de dificuldade.
Atributos: id, titulo, descricao, tipo, dificuldade, disciplina_id, criado_por, data_criacao, data_ultima_modificacao.

opcao_resposta: Armazena as alternativas de questões do tipo objetiva.
Atributos: id, questao_id, texto_resposta, correta.

log_crud: Tabela de auditoria, registrando ações administrativas como criação e exclusão de professores.
Atributos: id, admin_id, acao, registro_afetado, data_hora.

DER: https://dbdiagram.io/d/EasyQuiz-69136e556735e111704da191

---

## 7. Arquitetura do Sistema

### Tecnologias
- **Front-end:** HTML5, CSS3, JavaScript (ou React.js)  
- **Back-end:** Node.js / Express (ou Python Flask/Django)  
- **Banco de Dados:** MySQL ou PostgreSQL  
- **PDF Generator:** jsPDF / wkhtmltopdf / PDFKit  
- **Hospedagem:** Render / Vercel / Railway  

### Diagrama Simplificado
```
[Interface Web]
     ↓
[API REST - Backend]
     ↓
[Banco de Dados]
     ↓
[Geração de PDF]
```

---

## 8. Regras de Negócio
- Cada professor só pode editar ou excluir provas criadas por ele.  
- O limite máximo de **20 questões por prova** é obrigatório.  
- É necessário informar **pelo menos uma alternativa** por questão.  
- CPF deve ser **único e válido**.  
- PDF deve conter **nome do professor, disciplina e data**.

---

## 9. Segurança e LGPD
- Senhas criptografadas (ex.: bcrypt).  
- Comunicação via HTTPS.  
- Proteção contra SQL Injection e XSS.  
- Consentimento explícito para uso de dados pessoais (nome, e-mail, CPF).  
- Possibilidade de exclusão de conta a pedido do usuário.  

---

## 10. Testes
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

## 11. Conclusão e Próximos Passos
O sistema proposto contribui para otimizar o processo de elaboração de provas, reduzindo tempo e padronizando formatos.  
Como próximos passos, o projeto pode ser ampliado com:
- Banco de questões reutilizáveis;  
- Compartilhamento de provas entre professores;  
- Aplicação online com correção automática.

---

## 📚 Referências
- PRESSMAN, Roger S. *Engenharia de Software: uma abordagem profissional.* McGraw Hill, 2016.  
- SOMMERVILLE, Ian. *Engenharia de Software.* 10ª ed. Pearson, 2019.  
- GIL, Antonio Carlos. *Como Elaborar Projetos de Pesquisa.* Atlas, 2018.  
- Documentação do Node.js — https://nodejs.org/  
- Documentação do jsPDF — https://github.com/parallax/jsPDF  
