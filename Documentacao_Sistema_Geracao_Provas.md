# 🧾 Documentação do Projeto — Sistema de Geração de Provas

## 1. Visão Geral
**Nome do Projeto:** Sistema de Geração de Provas  
**Curso:** Análise e Desenvolvimento de Sistemas - FATEC Guarulhos  
**Autores:** [Nome dos integrantes do grupo]  
**Orientador:** [Nome do professor orientador]  
**Data:** [Mês/Ano]  

### Descrição
O sistema tem como objetivo facilitar o processo de criação, armazenamento e exportação de provas por professores.  
Após se cadastrarem com nome, e-mail, CPF e senha, os professores podem acessar a plataforma, escolher entre utilizar provas existentes ou criar novas, com até 20 questões por prova, e exportar o conteúdo final em formato PDF.

---

## 2. Objetivos e Escopo

### Objetivo Geral
Desenvolver um sistema web que permita que professores criem, gerenciem e exportem provas de forma prática e segura.

### Objetivos Específicos
- Permitir o **cadastro e autenticação de professores** com validação de CPF.  
- Implementar um **painel de controle** para criação, edição e exclusão de provas.  
- Limitar o número de questões por prova a **20 unidades**.  
- Organizar as provas por **matéria**.  
- Permitir **exportação das provas em formato PDF**.  
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

## 3. Requisitos

### 3.1 Requisitos Funcionais (RF)
| Código | Descrição |
|---------|------------|
| RF01 | O sistema deve permitir o cadastro de professores com nome, e-mail, CPF e senha. |
| RF02 | O sistema deve validar CPF e e-mail únicos. |
| RF03 | O professor deve fazer login com e-mail e senha. |
| RF04 | O sistema deve permitir selecionar uma matéria para criar ou visualizar provas. |
| RF05 | O professor pode criar até 20 questões por prova. |
| RF06 | O sistema deve permitir salvar, editar e excluir provas. |
| RF07 | O sistema deve gerar uma versão PDF da prova criada. |

### 3.2 Requisitos Não Funcionais (RNF)
| Código | Descrição |
|---------|------------|
| RNF01 | As senhas devem ser armazenadas de forma criptografada. |
| RNF02 | O sistema deve ser responsivo e acessível em dispositivos móveis. |
| RNF03 | O tempo de geração do PDF não deve ultrapassar 5 segundos. |
| RNF04 | O sistema deve seguir boas práticas de segurança e LGPD. |
| RNF05 | O sistema deve suportar, no mínimo, 100 usuários simultâneos. |

---

## 4. Casos de Uso / Fluxos do Usuário

### UC01 — Cadastro de Professor
**Atores:** Professor  
**Descrição:** Permite o registro de um novo usuário no sistema.  
**Fluxo Principal:**
1. Usuário acessa a tela de cadastro.  
2. Preenche nome, e-mail, CPF e senha.  
3. O sistema valida os dados e cria o registro.  
4. Usuário é redirecionado para a tela de login.

### UC02 — Login
**Atores:** Professor  
**Descrição:** Autenticação com e-mail e senha para acesso ao sistema.

### UC03 — Criar Prova
**Atores:** Professor  
**Descrição:** O usuário escolhe uma matéria e cria até 20 questões.  
**Fluxo Alternativo:** Se tentar adicionar mais de 20 questões, o sistema exibe uma mensagem de erro.

### UC04 — Exportar Prova em PDF
**Atores:** Professor  
**Descrição:** Após finalizar a prova, o sistema gera um arquivo PDF formatado com cabeçalho, nome do professor, matéria e data.

---

## 5. Modelagem de Dados

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

## 6. Arquitetura do Sistema

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

## 7. Regras de Negócio
- Cada professor só pode editar ou excluir provas criadas por ele.  
- O limite máximo de **20 questões por prova** é obrigatório.  
- É necessário informar **pelo menos uma alternativa** por questão.  
- CPF deve ser **único e válido**.  
- PDF deve conter **nome do professor, disciplina e data**.

---

## 8. Segurança e LGPD
- Senhas criptografadas (ex.: bcrypt).  
- Comunicação via HTTPS.  
- Proteção contra SQL Injection e XSS.  
- Consentimento explícito para uso de dados pessoais (nome, e-mail, CPF).  
- Possibilidade de exclusão de conta a pedido do usuário.  

---

## 9. Testes
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

## 10. Conclusão e Próximos Passos
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
