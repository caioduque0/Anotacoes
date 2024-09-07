# Full Stack - Revisão

# 1. Introdução ao HTTP

# Mensagens HTTP

- **Mensagens HTTP** são a chave ao usar o protocolo HTTP. Elas possuem uma estrutura simples e são altamente extensíveis. As mensagens são divididas em:
  - **Requisição HTTP**: O cliente envia ao servidor.
  - **Resposta HTTP**: O servidor responde ao cliente.

# Status HTTP

Os códigos de status HTTP indicam se uma solicitação foi concluída com sucesso. As respostas são agrupadas em cinco classes:

- **1xx (Informativas)**: A solicitação foi recebida e está sendo processada.
- **2xx (Sucesso)**: A solicitação foi recebida, compreendida e aceita.
- **3xx (Redirecionamento)**: A solicitação requer mais ações para ser completada.
- **4xx (Erro do cliente)**: A solicitação contém erros que o cliente precisa corrigir.
- **5xx (Erro do servidor)**: O servidor falhou ao completar a solicitação válida.

# Exemplos de Status HTTP

- **100 Continue**: O cliente deve continuar com a solicitação.
- **200 OK**: A solicitação foi bem-sucedida.
- **301 Moved Permanently**: O recurso foi movido permanentemente para outra URL.
- **404 Not Found**: O recurso solicitado não foi encontrado.
- **500 Internal Server Error**: Ocorreu um erro no servidor.

---

# 2. Express.js - Configuração do Servidor

# Exemplo Básico

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    res.send('Hello world');
});

app.listen(port, () => {
    console.log(`Servidor rodando em http://localhost:${port}`);
});
```

# Requisição HTTP com Axios

Você pode realizar requisições a APIs de terceiros usando a biblioteca Axios.

```javascript
const express = require('express');
const axios = require('axios');

const app = express();
const port = 3000;

app.get('/consulta-cep/:cep', async (req, res) => {
    const cep = req.params.cep;

    try {
        const response = await axios.get(`https://viacep.com.br/ws/${cep}/json/`);
        res.json(response.data);
    } catch (error) {
        res.status(500).send('Erro ao consultar o CEP');
    }
});

app.listen(port, () => {
    console.log(`Servidor rodando em http://localhost:${port}`);
});
```

---

# 3. Sequelize - ORM

# Introdução ao Sequelize

O Sequelize é um ORM (Object-Relational Mapper) para Node.js que suporta diversos bancos de dados, como Postgres, MySQL, SQLite e outros. Ele facilita a interação com bancos de dados relacionais utilizando JavaScript.

### Configuração do Banco de Dados

Criar o arquivo `database.js` para configurar a conexão com o banco de dados.

```javascript
module.exports = {
    dialect: 'postgres',
    host: 'localhost',
    username: 'usuario',
    password: 'senha',
    database: 'api-node',
    define: {
        timestamps: true,
        underscored: true,
    },
};
```

# Models

Models no Sequelize representam tabelas no banco de dados. Um model é uma classe que estende o Sequelize e define a estrutura da tabela e seus tipos de dados.

Exemplo de um model no Sequelize:

```javascript
const { Model, DataTypes } = require('sequelize');
const sequelize = require('../config/database');

class User extends Model {}

User.init({
    name: DataTypes.STRING,
    email: DataTypes.STRING,
}, {
    sequelize,
    modelName: 'User',
    tableName: 'users',
    timestamps: true,
});

module.exports = User;
```

---

# 4. Migrations

Migrations no Sequelize permitem versionar o esquema do banco de dados, facilitando mudanças e a sincronização entre diferentes ambientes.

# Comandos para criar Migrations

- Inicialize o Sequelize CLI:

```bash
npx sequelize-cli init
```

- Crie uma nova migration:

```bash
npx sequelize-cli migration:generate --name create-endereço
```

Esse comando gera um arquivo de migration onde você define a estrutura da tabela.

# Exemplo de Migration

```javascript
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('enderecos', {
      id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        allowNull: false
      },
      rua: {
        type: Sequelize.STRING,
        allowNull: false
      },
      numero: {
        type: Sequelize.STRING,
        allowNull: false
      },
      createdAt: {
        type: Sequelize.DATE,
        allowNull: false
      },
      updatedAt: {
        type: Sequelize.DATE,
        allowNull: false
      }
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('enderecos');
  }
};
```

---

# 5. Revisão Geral

1. **Mensagens HTTP**: Importância de entender requisições e respostas.
2. **Express.js**: Estrutura básica de um servidor e requisições HTTP.
3. **Sequelize**: ORM para trabalhar com bancos de dados, simplificando queries e operações.
4. **Migrations**: Controle de versão para banco de dados usando Sequelize CLI.

---


