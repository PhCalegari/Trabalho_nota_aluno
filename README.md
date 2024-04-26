<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Planilha de Alunos</title>
  <style>
    body {
      font-family: Arial, sans-serif;
    }

    input, button {
      margin-bottom: 10px;
    }

    #notas {
      margin-top: 20px;
    }

    #lista-alunos {
      list-style: none;
      padding: 0;
    }

    .aluno {
      margin-bottom: 10px;
    }

    .aluno input {
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>Planilha de Alunos</h1>
  <div id="alunos">
    <input type="text" id="nome" placeholder="Nome do aluno">
    <input type="text" id="ra" placeholder="RA do aluno">
    <input type="email" id="email" placeholder="E-mail do aluno">
    <button onclick="adicionarAluno()">Adicionar Aluno</button>
  </div>
  <div id="notas">
    <h2>Notas</h2>
    <ul id="lista-alunos">
    </ul>
    <button onclick="calcularNotas()">Calcular Notas</button>
  </div>
  <script>
    function adicionarAluno() {
      let nome = document.getElementById("nome").value;
      let ra = document.getElementById("ra").value;
      let email = document.getElementById("email").value;
      let aluno = { nome: nome, ra: ra, email: email, notas: [] };

      let listaAlunos = document.getElementById("lista-alunos");
      let li = document.createElement("li");
      li.classList.add("aluno");

      let inputs = "";
      inputs += '<input type="text" value="' + nome + '" placeholder="Nome do aluno">';
      inputs += '<input type="text" value="' + ra + '" placeholder="RA do aluno">';
      inputs += '<input type="email" value="' + email + '" placeholder="E-mail do aluno">';
      inputs += '<button onclick="editarAluno(this)">Editar</button>';
      inputs += '<button onclick="adicionarNota(this)">Adicionar Nota</button>';
      inputs += '<button onclick="excluirAluno(this)">Excluir</button>';
      li.innerHTML = inputs;
      listaAlunos.appendChild(li);

      let alunosSalvos = JSON.parse(localStorage.getItem('alunos')) || [];
      alunosSalvos.push(aluno);
      localStorage.setItem('alunos', JSON.stringify(alunosSalvos));
    }

    function adicionarNota(btn) {
      let li = btn.parentNode;
      let nomeNota = prompt("Digite o nome da nota:");
      let nota = parseFloat(prompt("Digite a nota:"));
      let alunoIndex = Array.from(li.parentNode.children).indexOf(li);
      let alunosSalvos = JSON.parse(localStorage.getItem('alunos')) || [];
      alunosSalvos[alunoIndex].notas.push({ nome: nomeNota, valor: nota });
      localStorage.setItem('alunos', JSON.stringify(alunosSalvos));
    }

    function editarAluno(btn) {
      let li = btn.parentNode;
      let nome = li.querySelector("input[type='text']:nth-child(1)").value;
      let ra = li.querySelector("input[type='text']:nth-child(2)").value;
      let email = li.querySelector("input[type='email']").value;
      let alunoIndex = Array.from(li.parentNode.children).indexOf(li);
      let alunosSalvos = JSON.parse(localStorage.getItem('alunos')) || [];
      alunosSalvos[alunoIndex].nome = nome;
      alunosSalvos[alunoIndex].ra = ra;
      alunosSalvos[alunoIndex].email = email;
      localStorage.setItem('alunos', JSON.stringify(alunosSalvos));
    }

    function excluirAluno(btn) {
      let confirma = confirm("Tem certeza que deseja excluir este aluno?");
      if (confirma) {
        let li = btn.parentNode;
        let alunoIndex = Array.from(li.parentNode.children).indexOf(li);
        let alunosSalvos = JSON.parse(localStorage.getItem('alunos')) || [];
        alunosSalvos.splice(alunoIndex, 1);
        localStorage.setItem('alunos', JSON.stringify(alunosSalvos));
        li.remove();
      }
    }

    function calcularNotas() {
      let listaAlunos = document.querySelectorAll("#lista-alunos .aluno");
      listaAlunos.forEach(function(aluno) {
        let notas = aluno.querySelectorAll("input[type='number']");
        let total = 0;
        notas.forEach(function(nota) {
          total += parseFloat(nota.value);
        });
        let media = total / notas.length;
        alert("A média do aluno é: " + media.toFixed(2));
      });
    }
    window.onload = function() {
      let alunosSalvos = JSON.parse(localStorage.getItem('alunos')) || [];
      let listaAlunos = document.getElementById("lista-alunos");
      alunosSalvos.forEach(function(aluno) {
        let li = document.createElement("li");
        li.classList.add("aluno");
        let inputs = "";
        inputs += '<input type="text" value="' + aluno.nome + '" placeholder="Nome do aluno">';
        inputs += '<input type="text" value="' + aluno.ra + '" placeholder="RA do aluno">';
        inputs += '<input type="email" value="' + aluno.email + '" placeholder="E-mail do aluno">';
        inputs += '<button onclick="editarAluno(this)">Editar</button>';
        inputs += '<button onclick="adicionarNota(this)">Adicionar Nota</button>';
        inputs += '<button onclick="excluirAluno(this)">Excluir</button>';
        aluno.notas.forEach(function(nota) {
          inputs += '<input type="text" value="' + nota.nome + '" placeholder="Nome da nota">';
          inputs += '<input type="number" value="' + nota.valor + '" placeholder="Nota">';
        });
        li.innerHTML = inputs;
        listaAlunos.appendChild(li);
      });
    };
  </script>
</body>
</html>
