// Dados simulados por token
const cebolasConfig = {
  states: ["Normal", "Ferido", "Morto"],
  currentState: 0,
  bars: [
    { label: "Vida", value: 10, max: 20, color: "#ff0000" },
    { label: "Mana", value: 5, max: 10, color: "#0000ff" }
  ],
  effects: [
    {
      name: "Envenenado",
      color: "#4caf50",
      type: "Fumaça",
      active: false
    }
  ]
};

// Alternância de estados
function applyState(index) {
  cebolasConfig.currentState = index;
  const stateName = cebolasConfig.states[index];
  console.log(`Estado aplicado: ${stateName}`);
  // Aqui você aplicaria a imagem ou estilo no token via API
}

// Botão Flip
document.querySelector(".flip-btn").addEventListener("click", () => {
  cebolasConfig.currentState = (cebolasConfig.currentState + 1) % cebolasConfig.states.length;
  applyState(cebolasConfig.currentState);
});

// Atalhos de teclado
document.addEventListener("keydown", (event) => {
  if (event.key.toLowerCase() === "c") {
    if (event.shiftKey) {
      cebolasConfig.currentState =
        (cebolasConfig.currentState - 1 + cebolasConfig.states.length) % cebolasConfig.states.length;
    } else {
      cebolasConfig.currentState =
        (cebolasConfig.currentState + 1) % cebolasConfig.states.length;
    }
    applyState(cebolasConfig.currentState);
  }
});

// Gatilhos automáticos
function checkTriggers() {
  if (cebolasConfig.bars[0].value <= 0) {
    applyState(2); // Morto
  }
}

// Efeito: Aplicar/Cessar
const effectBtn = document.querySelector(".effect-toggle");
effectBtn.addEventListener("click", () => {
  const effect = cebolasConfig.effects[0];
  effect.active = !effect.active;
  effectBtn.textContent = effect.active ? "Cessar" : "Aplicar";
  console.log(`${effect.active ? "Aplicando" : "Removendo"} efeito: ${effect.name}`);
  // Aqui você aplicaria ou removeria o efeito visual no token
});

// Atualização de barras
document.querySelectorAll(".bar-slider").forEach((slider, index) => {
  slider.addEventListener("input", (e) => {
    cebolasConfig.bars[index].value = parseInt(e.target.value);
    checkTriggers();
  });
});
