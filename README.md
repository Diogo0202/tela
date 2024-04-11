import React, { useState } from 'react';
import axios from 'axios';

function App() {
  const [nome, setNome] = useState('');
  const [usuario, setUsuario] = useState('');
  const [senha, setSenha] = useState('');
  const [mensagem, setMensagem] = useState('');

  const handleSubmit = async (event) => {
    event.preventDefault();

    try {
      const salt = await bcrypt.genSalt(10);
      const hashedSenha = await bcrypt.hash(senha, salt);

     
      await axios.post('/cadastro', { nome, usuario, senha: hashedSenha });

      setMensagem('Usuário cadastrado com sucesso!');
    } catch (error) {
      setMensagem('Erro ao cadastrar usuário!');
    }
  };

  const handleLogin = async (event) => {
    event.preventDefault();

    try {
      const response = await axios.post('/login', { usuario, senha });

      if (response.data) {
        setMensagem('Login realizado com sucesso!');
      } else {
        setMensagem('Senha incorreta!');
      }
    } catch (error) {
      setMensagem('Erro ao realizar login!');
    }
  };

  return (
    <div className="App">
      <h1>Cadastro de Usuário</h1>
      <form onSubmit={handleSubmit}>
        <label>
          Nome:
          <input
            type="text"
            value={nome}
            onChange={(event) => setNome(event.target.value)}
          />
        </label>
        <br />
        <label>
          Usuário:
          <input
            type="text"
            value={usuario}
            onChange={(event) => setUsuario(event.target.value)}
          />
        </label>
        <br />
        <label>
          Senha:
          <input
            type="password"
            value={senha}
            onChange={(event) => setSenha(event.target.value)}
          />
        </label>
        <br />
        <button type="submit">Cadastrar</button>
      </form>
      <h1>Login de Usuário</h1>
      <form onSubmit={handleLogin}>
        <label>
          Usuário:
          <input
            type="text"
            value={usuario}
            onChange={(event) => setUsuario(event.target.value)}
          />
        </label>
        <br />
        <label>
          Senha:
          <input
            type="password"
            value={senha}
            onChange={(event) => setSenha(event.target.value)}
          />
        </label>
        <br />
        <button type="submit">Login</button>
      </form>
      <p>{mensagem}</p>
    </div>
  );
}

export default App;
