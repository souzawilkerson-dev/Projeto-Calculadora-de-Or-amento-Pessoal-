Estrutura do Projeto
Primeiro, crie um novo projeto React. Se você já tem o Node.js e o npm instalados, pode usar o Vite (que é mais rápido) ou o Create React App.

Bash

# Para usar o Vite (recomendado)
npm create vite@latest meu-app-de-despesas --template react

# Navegue até a pasta do projeto
cd meu-app-de-despesas

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm run dev
2. Componentes Principais
Sua aplicação pode ser dividida em alguns componentes-chave para manter o código organizado e reutilizável.

App.jsx: O componente raiz. Ele vai gerenciar o estado principal da aplicação, como a lista de despesas.

FormularioDespesa.jsx: Um formulário para o usuário inserir a data, descrição e valor da despesa.

ListaDespesas.jsx: Onde as despesas registradas serão exibidas em uma lista.

ResumoMensal.jsx: Um componente para calcular e mostrar o total de despesas por mês.

3. Gerenciamento de Estado (useState)
Em App.jsx, você precisará de um estado para armazenar a lista de despesas. Use o hook useState para isso.

JavaScript

// App.jsx
import { useState } from 'react';

function App() {
  const [despesas, setDespesas] = useState([]);

  const adicionarDespesa = (novaDespesa) => {
    setDespesas(prevDespesas => [
      ...prevDespesas,
      {
        id: Math.random(), // Usar algo melhor em um projeto real, como um UUID
        ...novaDespesa
      }
    ]);
  };

  return (
    <div>
      <h1>Controle de Despesas</h1>
      {/* Aqui virão os outros componentes */}
    </div>
  );
}

export default App;
4. Formulário para Adicionar Despesas
No componente FormularioDespesa.jsx, crie um formulário com inputs para a descrição, valor e data. Você vai usar o useState para controlar o valor de cada input.

JavaScript

// FormularioDespesa.jsx
import { useState } from 'react';

const FormularioDespesa = ({ onAdicionarDespesa }) => {
  const [descricao, setDescricao] = useState('');
  const [valor, setValor] = useState('');
  const [data, setData] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!descricao || !valor || !data) return;

    const novaDespesa = {
      descricao,
      valor: +valor, // Converte a string do input para número
      data: new Date(data),
    };
    onAdicionarDespesa(novaDespesa);

    // Limpa os campos do formulário
    setDescricao('');
    setValor('');
    setData('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Descrição"
        value={descricao}
        onChange={(e) => setDescricao(e.target.value)}
      />
      <input
        type="number"
        placeholder="Valor"
        value={valor}
        onChange={(e) => setValor(e.target.value)}
      />
      <input
        type="date"
        value={data}
        onChange={(e) => setData(e.target.value)}
      />
      <button type="submit">Adicionar Despesa</button>
    </form>
  );
};

export default FormularioDespesa;
Agora, inclua este formulário em App.jsx e passe a função adicionarDespesa como uma prop:

JavaScript

// App.jsx (parte do return)
<FormularioDespesa onAdicionarDespesa={adicionarDespesa} />
5. Exibição da Lista de Despesas (.map)
O componente ListaDespesas.jsx receberá a lista de despesas como uma prop. Use o método JavaScript .map() para renderizar um componente ItemDespesa para cada item na lista.

JavaScript

// ListaDespesas.jsx
const ListaDespesas = ({ despesas }) => {
  return (
    <div>
      {despesas.length === 0 ? (
        <p>Nenhuma despesa registrada.</p>
      ) : (
        despesas.map((despesa) => (
          <div key={despesa.id}>
            <p>{despesa.descricao}</p>
            <p>R$ {despesa.valor.toFixed(2)}</p>
            <p>{despesa.data.toLocaleDateString('pt-BR')}</p>
          </div>
        ))
      )}
    </div>
  );
};

export default ListaDespesas;
6. Resumo Mensal
O componente ResumoMensal.jsx irá agrupar as despesas por mês e somar os valores. Você pode usar o useEffect ou simplesmente calcular os totais diretamente no componente.

JavaScript

// ResumoMensal.jsx
import { useEffect, useState } from 'react';

const ResumoMensal = ({ despesas }) => {
  const [totais, setTotais] = useState({});

  useEffect(() => {
    const totaisCalculados = despesas.reduce((acc, despesa) => {
      const mes = despesa.data.toLocaleString('pt-BR', { month: 'long', year: 'numeric' });
      const valor = despesa.valor;
      acc[mes] = (acc[mes] || 0) + valor;
      return acc;
    }, {});
    setTotais(totaisCalculados);
  }, [despesas]);

  return (
    <div>
      <h2>Resumo Mensal</h2>
      {Object.keys(totais).length === 0 ? (
        <p>Nenhum resumo disponível.</p>
      ) : (
        <ul>
          {Object.keys(totais).map(mes => (
            <li key={mes}>
              {mes}: R$ {totais[mes].toFixed(2)}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
};

export default ResumoMensal;
7. Conclusão
Ao seguir esses passos, você terá uma aplicação funcional que demonstra o uso de:

Componentes Funcionais: Criando componentes reutilizáveis.

Props: Passando dados entre componentes.

State (useState): Gerenciando o estado interno dos componentes.

Renderização de Listas (.map): Exibindo dados dinamicamente.

Hooks (useState, useEffect): Utilizando funcionalidades avançadas do React.

Esse é um projeto excelente para começar. O próximo passo pode ser adicionar estilos CSS para deixar a interface mais agradável e até mesmo persistir os dados no localStorage do navegador para que eles não se percam quando a página for recarregada.

Parabéns pela iniciativa! Se tiver alguma dúvida ou quiser explorar mais a fundo algum desses tópicos, me diga!
