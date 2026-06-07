# 🧙‍♂️ Harry Potter Quiz — Descubra sua Casa de Hogwarts

Aplicação web full-stack temática do universo de Harry Potter, onde o usuário se cadastra, responde um quiz de personalidade e descobre a qual Casa de Hogwarts pertence. Os resultados são salvos no banco de dados e exibidos em um dashboard com gráficos interativos.

---

## 📋 Sumário

- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Modelagem do Banco de Dados](#modelagem-do-banco-de-dados)
- [Rotas da API](#rotas-da-api)
- [Configuração e Instalação](#configuração-e-instalação)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Como Usar](#como-usar)
- [Documentação](#documentação)

---

## Sobre o Projeto

O **Harry Potter Quiz** é um projeto individual desenvolvido como parte do programa **São Paulo Tech School / Bandtec Digital School**. A aplicação permite que usuários criem uma conta, realizem um quiz de personalidade baseado no universo de Harry Potter e descubram a qual das quatro casas de Hogwarts pertencem — Grifinória, Sonserina, Corvinal ou Lufa-Lufa. Os resultados são armazenados em banco de dados relacional e visualizados em um dashboard com gráfico de barras interativo.

---

## Funcionalidades

- **Página Inicial** — apresentação temática com informações sobre os livros, o trio de ouro, as casas de Hogwarts e seção sobre a desenvolvedora.
- **Cadastro de Usuário** — formulário com validações de nome, e-mail, senha (mínimo 8 caracteres, letra maiúscula e número obrigatórios) e confirmação de senha.
- **Login** — autenticação via e-mail e senha, com sessão armazenada via `sessionStorage`.
- **Quiz das Casas** — série de perguntas de personalidade com pontuação por casa; ao final, a casa com mais pontos é registrada no banco de dados.
- **Dashboard** — exibe a casa do usuário logado e um gráfico de barras com a distribuição de todos os participantes pelas quatro casas de Hogwarts.
- **Logout** — encerra a sessão e redireciona para a página inicial.

---

## Tecnologias Utilizadas

### Backend
| Tecnologia | Descrição |
|---|---|
| [Node.js](https://nodejs.org/) | Ambiente de execução JavaScript no servidor |
| [Express.js](https://expressjs.com/) | Framework web para criação das rotas e API REST |
| [MySQL2](https://www.npmjs.com/package/mysql2) | Driver de conexão com o banco de dados MySQL |
| [dotenv](https://www.npmjs.com/package/dotenv) | Gerenciamento de variáveis de ambiente |
| [cors](https://www.npmjs.com/package/cors) | Habilitação de CORS para requisições cross-origin |
| [nodemon](https://www.npmjs.com/package/nodemon) | Reinicialização automática do servidor em desenvolvimento |

### Frontend
| Tecnologia | Descrição |
|---|---|
| HTML5 | Estrutura das páginas |
| CSS3 | Estilização e layout |
| JavaScript (Vanilla) | Lógica do quiz, chamadas à API e manipulação do DOM |
| [Chart.js](https://www.chartjs.org/) | Renderização do gráfico de barras no dashboard |

### Banco de Dados
| Tecnologia | Descrição |
|---|---|
| MySQL | Banco de dados relacional |

---

## Estrutura do Projeto

```
Projeto-Individual-main/
├── Documentação/
│   ├── Documentação_ProjetoIndividual.docx
│   └── Slides.pdf
├── Modelagem/
│   └── Modelagem_individual.mwb        # Modelo do banco de dados (MySQL Workbench)
├── web-data-viz/
│   ├── app.js                          # Ponto de entrada do servidor
│   ├── package.json
│   ├── .env                            # Variáveis de ambiente (produção)
│   ├── .env.dev                        # Variáveis de ambiente (desenvolvimento)
│   ├── .gitignore
│   ├── Public/
│   │   ├── index.html                  # Página inicial
│   │   ├── login.html                  # Página de login
│   │   ├── cadastro.html               # Página de cadastro
│   │   ├── quiz.html                   # Página do quiz
│   │   ├── dashboard.html              # Página do dashboard
│   │   ├── css/
│   │   │   ├── style.css               # Estilo da página inicial
│   │   │   ├── cadastro.css            # Estilo de login e cadastro
│   │   │   ├── quiz.css                # Estilo do quiz
│   │   │   └── dashborard.css          # Estilo do dashboard
│   │   ├── Js/
│   │   │   ├── questions.js            # Banco de perguntas do quiz
│   │   │   ├── quizScript.js           # Lógica e pontuação do quiz
│   │   │   └── sessao.js               # Gerenciamento de sessão
│   │   └── assets/                     # Imagens, ícones e vídeo
│   └── src/
│       ├── controllers/
│       │   └── usuarioController.js    # Lógica de negócio dos usuários
│       ├── database/
│       │   ├── config.js               # Configuração e conexão com MySQL
│       │   └── script-tabelas.sql      # Script de criação do banco de dados
│       ├── models/
│       │   └── usuarioModel.js         # Queries SQL
│       └── routes/
│           ├── index.js                # Rota raiz
│           └── usuarios.js             # Rotas de usuários
└── README.md
```

---

## Modelagem do Banco de Dados

O banco de dados chama-se `hp` e é composto por três tabelas:

```sql
CREATE DATABASE hp;
USE hp;

-- Tabela de usuários
CREATE TABLE usuario (
    idusuario    INT PRIMARY KEY AUTO_INCREMENT,
    nomeCompleto VARCHAR(45),
    Email        VARCHAR(80),
    senha        VARCHAR(20)
);

-- Tabela de casas de Hogwarts
CREATE TABLE Casa (
    idCasa    INT PRIMARY KEY AUTO_INCREMENT,
    nomeCasa  VARCHAR(45),
    descricao VARCHAR(45)
);

-- Tabela de resultados do quiz (relacionamento N:N entre usuário e casa)
CREATE TABLE CasaIdeal (
    idResposta INT PRIMARY KEY AUTO_INCREMENT,
    fkusuario  INT,
    fkCasa     INT,
    dtResposta DATE,
    FOREIGN KEY (fkusuario) REFERENCES usuario(idusuario),
    FOREIGN KEY (fkCasa)    REFERENCES Casa(idCasa)
);
```

> O arquivo `Modelagem/Modelagem_individual.mwb` contém o diagrama visual do banco de dados, editável no **MySQL Workbench**.

**Populate inicial das casas (execute após criar as tabelas):**
```sql
INSERT INTO Casa (nomeCasa, descricao) VALUES
  ('Grifinória', 'Coragem e lealdade'),
  ('Sonserina',  'Ambição e astúcia'),
  ('Corvinal',   'Inteligência e sabedoria'),
  ('Lufa-Lufa',  'Justiça e dedicação');
```

---

## Rotas da API

Base URL: `http://localhost:<APP_PORT>`

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/usuarios/cadastrar` | Cadastra um novo usuário |
| `POST` | `/usuarios/autenticar` | Autentica um usuário (login) |
| `POST` | `/usuarios/registrar-casa` | Registra o resultado do quiz para o usuário |
| `GET` | `/usuarios/estatisticas` | Retorna a contagem de participantes por casa |
| `GET` | `/usuarios/casa/:idUsuario` | Retorna a casa do usuário pelo ID |

### Exemplos de Payload

**POST `/usuarios/cadastrar`**
```json
{
  "nomeServer": "Hermione Granger",
  "emailServer": "hermione@hogwarts.com",
  "senhaServer": "Alohomora1"
}
```

**POST `/usuarios/autenticar`**
```json
{
  "emailServer": "hermione@hogwarts.com",
  "senhaServer": "Alohomora1"
}
```

**POST `/usuarios/registrar-casa`**
```json
{
  "idUsuario": 1,
  "idCasa": 3
}
```

---

## Configuração e Instalação

### Pré-requisitos

- [Node.js](https://nodejs.org/) (v14 ou superior)
- [MySQL](https://www.mysql.com/) (v8 ou superior)
- [MySQL Workbench](https://www.mysql.com/products/workbench/) *(opcional, para visualizar a modelagem)*

### Passo a passo

**1. Clone o repositório**
```bash
git clone https://github.com/seu-usuario/Projeto-Individual.git
cd Projeto-Individual/web-data-viz
```

**2. Instale as dependências**
```bash
npm install
```

**3. Configure o banco de dados**

Acesse o MySQL e execute o script de criação:
```bash
mysql -u root -p < src/database/script-tabelas.sql
```

Em seguida, populate a tabela de casas:
```sql
USE hp;
INSERT INTO Casa (nomeCasa, descricao) VALUES
  ('Grifinória', 'Coragem e lealdade'),
  ('Sonserina',  'Ambição e astúcia'),
  ('Corvinal',   'Inteligência e sabedoria'),
  ('Lufa-Lufa',  'Justiça e dedicação');
```

**4. Configure as variáveis de ambiente**

Edite o arquivo `.env.dev` com suas credenciais locais (veja a seção abaixo).

**5. Inicie o servidor**

Em modo de desenvolvimento:
```bash
npm run dev
```

Em modo de produção:
```bash
npm start
```

> Para alternar entre os ambientes, edite as linhas 1 e 2 do arquivo `app.js`, comentando ou descomentando a variável `ambiente_processo`.

**6. Acesse a aplicação**

Abra o navegador em: [http://localhost:3333](http://localhost:3333)

---

## Como Usar

1. **Acesse a página inicial** e explore o conteúdo sobre o universo Harry Potter.
2. **Clique em "Entrar"** para ir à tela de login ou acesse o cadastro caso ainda não tenha conta.
3. **Cadastre-se** preenchendo nome, e-mail e uma senha que contenha pelo menos 8 caracteres, uma letra maiúscula e um número.
4. **Faça login** com seu e-mail e senha.
5. **Responda o quiz** — selecione uma opção por pergunta e clique em "Próxima" para avançar.
6. **Veja seu resultado** — ao final, sua casa será exibida e salva no banco de dados.
7. **Acesse o Dashboard** para visualizar sua casa e o gráfico com a distribuição de todos os participantes.
8. **Logout** — clique em "Logout" na barra lateral para encerrar a sessão.

---

## Documentação

A pasta `Documentação/` contém materiais de apoio do projeto:

- `Documentação_ProjetoIndividual.docx` — documentação técnica completa do projeto.
- `Slides.pdf` — apresentação utilizada na defesa do projeto.

---

## Licença

Este projeto está licenciado sob a licença **MIT**. Consulte o arquivo `LICENSE` para mais detalhes.

---

*Desenvolvido como Projeto Individual — São Paulo Tech School / Bandtec Digital School* ⚡
