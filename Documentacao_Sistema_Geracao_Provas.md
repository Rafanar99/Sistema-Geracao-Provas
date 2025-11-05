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
| RF09   | Cada prova pode conter até **20 questões**.                                                                           |
| RF10   | O sistema deve permitir gerar **PDF personalizado** contendo Nome/RA/Data/Logo da instituição.                        |
| RF11   | O sistema deve permitir gerar provas **aleatórias** para usuários não autenticados.                                   |


### 4.2 Requisitos Não Funcionais (RNF)
| Código | Descrição |
|---------|------------|
| RNF01 | As senhas devem ser armazenadas de forma criptografada. |
| RNF02 | O sistema deve ser responsivo e acessível em dispositivos móveis. |
| RNF03 | O tempo de geração do PDF não deve ultrapassar 5 segundos. |
| RNF04 | O sistema deve seguir boas práticas de segurança e LGPD. |
| RNF05 | O sistema deve suportar, no mínimo, 100 usuários simultâneos. |

---

## 6. Casos de Uso / Fluxos do Usuário

### UC01 — Efetuar Login
**Atores:** Professor, coordenador
**Descrição:** Permite que o usuário autenticado (professor ou coordenador) acesse o sistema.  
**Fluxo Principal:**
1. Usuário acessa a tela de login.  
2. Informa email e senha.  
3. O sistema valida as credenciais.  
4. Usuário é redirecionado para o painel correspondente ao seu perfil.

### UC02 — Criar Prova
**Atores:** Professor  
**Descrição:** Permite ao professor criar uma prova associada a uma disciplina.
**Fluxo Principal:**
1. Professor acessa a tela de criação de prova.  
2. Seleciona a disciplina.
3. Define o número de perguntas (ate 20).  
4. Adiciona o enunciado e alternativas de cada questão.
5. Salve a prova.
   Fluxo Alternativo:
   - Se o número de perguntas exceder 20, o sistema exibe uma mensagem de erro.
   - Se algum campo obrigatório não for preenchido, o sistema alerta o usuário.

### UC03 — Imprimir Prova
**Atores:** Professor  
**Descrição:** Após criar ou selecionar uma prova, o sistema gera um arquivo PDF formatado para impressão. 
**Fluxo Principal:**
1. Professor seleciona a prova desejada.
2. Clica em "Imprimir prova".
3. O sistema gera o arquivo PDF com cabeçalho (nome, disciplina, data).
4. O arquivo é disponibilizado para download ou impressão.

### UC04 — Cadastrar Questão
**Atores:** Coordenador  
**Descrição:** Permite ao coordenador cadastrar novas questões que poderão ser utilizadas pelos professores.
**Fluxo Principal:**
1. Coordenador acessa o módulo de provas.
2. Seleciona a prova existente.
3. O sistema exibe detalhes (questões e respostas).
4. Coordenador pode editar ou excluir informações, se necessário.

[Caso de Uso.pdf](https://github.com/user-attachments/files/23106324/Caso.de.Uso.pdf)

---

## 6. Modelagem de Dados

### Entidades Principais
- **Professor** — guarda informações pessoais e credenciais.  
- **Matéria** — disciplinas cadastradas no sistema.  
- **Prova** — metadados da prova (título, data, professor, matéria).  
- **Questão** — enunciado e alternativas.  
- **Alternativa** — opções de resposta.

### Exemplo de Modelo ER
```
Professor (id, nome, email, cpf, senha_hash)
Materia (id, nome)
Prova (id, titulo, materia_id, professor_id, criado_em)
Questao (id, prova_id, enunciado, ordem)
Alternativa (id, questao_id, texto, correta)
```

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
