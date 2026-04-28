# Rockt-Agendamento-Pet-Shop-
Neste desafio, você vai desenvolver um site web responsivo para agendamento em pet shop
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Agenda Petshop</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<header>
  <h1>Agenda Petshop</h1>
  <input type="date" id="dataSelecionada">
  <button id="novoAgendamento">Novo agendamento</button>
</header>

<main id="agenda">
  <section id="manha">
    <h2>🌅 Manhã</h2>
    <div class="lista"></div>
  </section>

  <section id="tarde">
    <h2>🌤️ Tarde</h2>
    <div class="lista"></div>
  </section>

  <section id="noite">
    <h2>🌙 Noite</h2>
    <div class="lista"></div>
  </section>
</main>

<!-- Modal -->
<div id="modal" class="hidden">
  <div class="modal-content">
    <h2>Novo Agendamento</h2>

    <input type="text" id="tutor" placeholder="Nome do tutor">
    <input type="text" id="pet" placeholder="Nome do pet">
    <input type="tel" id="telefone" placeholder="Telefone">
    <input type="text" id="servico" placeholder="Serviço">
    <input type="date" id="data">
    <input type="time" id="hora">

    <button id="salvar">Agendar</button>
    <button id="fechar">Cancelar</button>
  </div>
</div>

<script src="script.js"></script>
</body>
</html>

body {
  font-family: Arial;
  margin: 0;
  padding: 0;
}

header {
  display: flex;
  gap: 10px;
  padding: 10px;
  background: #222;
  color: white;
}

main {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  padding: 10px;
}

section {
  flex: 1;
  min-width: 250px;
  background: #f4f4f4;
  padding: 10px;
  border-radius: 10px;
}

.card {
  background: white;
  padding: 8px;
  margin: 5px 0;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
}

.hidden {
  display: none;
}

#modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.6);
}

.modal-content {
  background: white;
  padding: 20px;
  margin: 50px auto;
  width: 300px;
  border-radius: 10px;
}

const modal = document.getElementById("modal");
const btnNovo = document.getElementById("novoAgendamento");
const btnFechar = document.getElementById("fechar");
const btnSalvar = document.getElementById("salvar");
const dataSelecionada = document.getElementById("dataSelecionada");

let agendamentos = [];

// abrir modal
btnNovo.onclick = () => {
  modal.classList.remove("hidden");
};

// fechar modal
btnFechar.onclick = () => {
  modal.classList.add("hidden");
};

// salvar agendamento
btnSalvar.onclick = () => {
  const tutor = document.getElementById("tutor").value;
  const pet = document.getElementById("pet").value;
  const telefone = document.getElementById("telefone").value;
  const servico = document.getElementById("servico").value;
  const data = document.getElementById("data").value;
  const hora = document.getElementById("hora").value;

  if (!tutor || !pet || !telefone || !servico || !data || !hora) {
    alert("Preenche tudo direito aí, campeão 😑");
    return;
  }

  // evitar conflito
  const conflito = agendamentos.find(a => a.data === data && a.hora === hora);
  if (conflito) {
    alert("Já existe agendamento nesse horário!");
    return;
  }

  const novo = { tutor, pet, telefone, servico, data, hora };
  agendamentos.push(novo);

  modal.classList.add("hidden");
  render();
};

// renderizar agenda
function render() {
  const data = dataSelecionada.value;

  document.querySelectorAll(".lista").forEach(l => l.innerHTML = "");

  agendamentos
    .filter(a => a.data === data)
    .sort((a, b) => a.hora.localeCompare(b.hora))
    .forEach(a => {
      let periodo;

      if (a.hora < "12:00") periodo = "manha";
      else if (a.hora < "18:00") periodo = "tarde";
      else periodo = "noite";

      const card = document.createElement("div");
      card.className = "card";

      card.innerHTML = `
        <div>
          <strong>${a.hora}</strong> - ${a.pet} <br>
          Tutor: ${a.tutor} <br>
          ${a.servico}
        </div>
        <button onclick="remover('${a.data}','${a.hora}')">X</button>
      `;

      document.querySelector(`#${periodo} .lista`).appendChild(card);
    });
}

// remover
function remover(data, hora) {
  agendamentos = agendamentos.filter(a => !(a.data === data && a.hora === hora));
  render();
}

// trocar data
dataSelecionada.onchange = render;
