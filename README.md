
# Aplicação de Consulta de CEP (React + Node.js)

Esta é uma aplicação simples de consulta de CEP utilizando React no frontend e Node.js no backend com a biblioteca `cep-promise`.

## Tecnologias Utilizadas

- **Frontend**: React
- **Backend**: Node.js, Express, `cep-promise`
- **Ferramentas de Desenvolvimento**: npm, Create React App

## Estrutura do Projeto

A aplicação é dividida em duas partes:

- **Backend**: Um servidor Node.js que utiliza o `cep-promise` para consultar os dados de um CEP.
- **Frontend**: Um aplicativo React que permite ao usuário digitar um CEP e visualizar os dados retornados pelo backend.

## Configuração do Backend (Node.js + `cep-promise`)

### 1. Criação do Projeto Backend

1. Crie um diretório para o backend e entre nele:

   ```bash
   mkdir api-cep
   cd api-cep
   ```

2. Inicialize o projeto Node.js:

   ```bash
   npm init -y
   ```

3. Instale as dependências necessárias:

   ```bash
   npm install express cors cep-promise
   ```

### 2. Criação do Servidor Express

Crie o arquivo `server.js` no diretório `api-cep` e adicione o seguinte código para configurar o servidor Express com a API de consulta de CEP:

```javascript
const express = require('express');
const cors = require('cors');
const cepPromise = require('cep-promise');

const app = express();
const port = 5000;

// Middleware
app.use(cors());
app.use(express.json());

// Rota para consulta de CEP
app.get('/api/cep/:cep', async (req, res) => {
  const { cep } = req.params;
  try {
    const result = await cepPromise(cep);
    res.json(result);
  } catch (error) {
    res.status(400).json({ message: 'CEP inválido ou não encontrado.' });
  }
});

// Iniciar o servidor
app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
```

### 3. Iniciar o Servidor

Após configurar o servidor, inicie-o com o seguinte comando:

```bash
node server.js
```

O servidor estará rodando na porta **5000**.

---

## Configuração do Frontend (React)

### 1. Criação do Projeto React

1. No diretório onde você deseja criar o frontend, execute o comando para criar o projeto React:

   ```bash
   npx create-react-app frontend-cep
   cd frontend-cep
   ```

2. Instale as dependências, caso necessário:

   ```bash
   npm install
   ```

### 2. Criação do Componente de Consulta de CEP

Abra o arquivo `src/App.js` e substitua o conteúdo pelo seguinte código para configurar o componente de consulta de CEP:

```javascript
import React, { useState } from 'react';
import './App.css';

function App() {
  const [cep, setCep] = useState('');
  const [resultado, setResultado] = useState(null);
  const [erro, setErro] = useState(null);

  const consultarCep = async () => {
    try {
      const response = await fetch(`http://localhost:5000/api/cep/${cep}`);
      const data = await response.json();
      if (response.ok) {
        setResultado(data);
        setErro(null);
      } else {
        setResultado(null);
        setErro(data.message);
      }
    } catch (err) {
      setErro('Erro na consulta.');
      setResultado(null);
    }
  };

  return (
    <div className="App">
      <h1>Consulta de CEP</h1>
      <input
        type="text"
        value={cep}
        onChange={(e) => setCep(e.target.value)}
        placeholder="Digite o CEP"
      />
      <button onClick={consultarCep}>Consultar</button>

      {erro && <div style={{ color: 'red' }}>{erro}</div>}

      {resultado && (
        <div>
          <h2>Resultado:</h2>
          <p>Logradouro: {resultado.logradouro}</p>
          <p>Bairro: {resultado.bairro}</p>
          <p>Cidade: {resultado.localidade}</p>
          <p>Estado: {resultado.uf}</p>
        </div>
      )}
    </div>
  );
}

export default App;
```

### 3. Iniciar o Frontend

Para iniciar o frontend React, execute o comando:

```bash
npm start
```

O aplicativo React estará rodando na porta **3000** por padrão. Você poderá usar o campo de entrada para consultar o CEP, com a resposta sendo mostrada logo abaixo.

---

## Passos Finais

1. **Inicie o Backend**: Navegue até o diretório do backend e execute `node server.js`. O servidor backend estará rodando em `http://localhost:5000`.
2. **Inicie o Frontend**: Navegue até o diretório do frontend e execute `npm start`. O aplicativo frontend estará rodando em `http://localhost:3000`.

---

## Testando

1. Abra o navegador e vá até `http://localhost:3000`.
2. Digite um CEP válido (exemplo: `01001000` para São Paulo).
3. Clique no botão "Consultar" e veja as informações de endereço sendo retornadas!

---

## Conclusão

Com isso, você tem uma aplicação funcional de consulta de CEP, utilizando **React** no frontend e **Node.js** com **cep-promise** no backend. A aplicação se comunica entre o frontend e o backend via HTTP, permitindo que o usuário insira um CEP e visualize as informações associadas ao endereço.

---
