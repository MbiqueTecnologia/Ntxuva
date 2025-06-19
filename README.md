<!DOCTYPE html>
<html lang="pt">
<head>
  <meta charset="UTF-8">
  <title>Txuva Digital</title>
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <!doctype html>
  <style>
    :root {
      --bg-main: #eaf6ff;
      --accent: #1976d2;
      --accent2: #1de9b6;
      --border: #aac2ee;
      --dark: #263238;
      --highlight: #e53935;
      --final: #1de9b6;
      --shadow: 0 4px 24px #aac2ee66;
      --radius: 3vw;
      --peca: #fff;
      --peca-captura: #e53935;
      --peca-borda: #888;
      --player1: #28386b;
      --player2: #256049;
      --player1-light: #3c4e9b;
      --player2-light: #319e72;
      --turnbar-h: 10px;
      --tooltip-bg: #fff;
      --tooltip-color: #e53935;
      --tooltip-shadow: 0 2px 10px #e5393533;
    }
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: linear-gradient(135deg, var(--bg-main) 0%, #d2f1e2 100%);
      margin: 0;
      padding: 0;
      min-height: 100vh;
      min-width: 100vw;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center; /* Centraliza verticalmente */
      user-select: none;
      -webkit-user-select: none;
      -ms-user-select: none;
      -moz-user-select: none;
    }
    #main-content {
      width: 100%;
      max-width: 700px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center; /* Centraliza verticalmente dentro do main-content */
      transition: filter 0.2s;
    }
    #main-content.desfocado {
      filter: blur(3px) brightness(0.92) grayscale(0.3);
      pointer-events: none;
      user-select: none;
    }
        #settings-btn {
      position: fixed;
      right: 24px;
      top: 20px;
      width: 40px;
      height: 40px;
      background: #fff;
      border-radius: 50%;
      box-shadow: 0 2px 12px #aac2ee77;
      border: 2px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      z-index: 6000;
      transition: box-shadow 0.14s, background 0.13s;
    }
    #settings-btn:hover {
      background: #e3f2fd;
      box-shadow: 0 4px 18px #aac2eeaa;
    }
    #settings-icon {
      width: 26px;
      height: 26px;
      fill: var(--player1-light);
      transition: transform 0.23s;
    }
    #settings-panel-bg {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(40,56,107,0.09);
      z-index: 5999;
      display: none;
    }
    #settings-panel-bg.open {
      display: block;
    }
    #settings-panel {
      position: fixed;
      top: 38px;
      right: 16px;
      width: var(--settings-panel-w);
      max-width: 97vw;
      background: #fff;
      border-radius: 18px;
      box-shadow: 0 4px 38px #3c4e9bdc;
      border: 2.5px solid var(--border);
      z-index: 6001;
      display: none;
      flex-direction: column;
      align-items: center;
      padding: 34px 18px 24px 18px;
      min-width: 220px;
      animation: settingsIn 0.35s cubic-bezier(.32,1.5,.6,1.01);
    }
    @keyframes settingsIn {
      0% { opacity: 0; transform: translateY(-25px) scale(0.96);}
      100% { opacity: 1; transform: translateY(0) scale(1);}
    }
    #settings-panel.open {
      display: flex;
    }
    #settings-panel .settings-title {
      font-size: 1.22em;
      font-weight: bold;
      color: var(--player1-light);
      margin-bottom: 22px;
      letter-spacing: 2.5px;
      text-align: center;
    }
    #settings-panel .settings-group {
      width: 100%;
      margin-bottom: 18px;
      text-align: center;
    }
    #audio-controls {
      display: flex;
      align-items: center;
      gap: 8px;
    }
    #settings-panel .settings-label {
      font-size: 1.05em;
      color: var(--dark);
      font-weight: 600;
      margin-bottom: 11px;
      display: block;
    }
    #settings-panel .settings-btn {
      width: 100%;
      padding: 14px 0;
      font-size: 1.17em;
      font-weight: 800;
      color: #fff;
      background: var(--player2-light);
      border: none;
      border-radius: 10px;
      margin-bottom: 9px;
      cursor: pointer;
      box-shadow: 0 2px 11px #319e7277;
      transition: background .15s, box-shadow .16s, filter .12s;
      letter-spacing: 1.7px;
    }
    #settings-panel .settings-btn.donate {
      background: linear-gradient(90deg, #f9a826, #ffb300 70%);
      color: #28386b;
      box-shadow: 0 4px 14px #f9a82666;
      font-size: 1.2em;
      margin-top: 2px;
    }
    #settings-panel .settings-btn.donate:hover {
      filter: brightness(1.08) contrast(1.07);
      background: linear-gradient(90deg, #ffd54f, #ffb300 100%);
    }
    #settings-panel .settings-btn:hover {
      filter: brightness(1.11) contrast(1.11);
    }
    #settings-panel .close-btn {
      background: #fff;
      color: var(--player1-light);
      border: 2px solid var(--player1-light);
      font-weight: bold;
      border-radius: 8px;
      margin-top: 7px;
      font-size: 1.1em;
      padding: 7px 0;
      cursor: pointer;
      box-shadow: 0 2px 7px #aac2ee66;
      width: 100%;
      transition: background .12s, color .12s;
    }
    #settings-panel .close-btn:hover {
      background: #e3f2fd;
      color: var(--player2-light);
    }
    @media (max-width: 600px) {
      #settings-panel {
        width: var(--settings-panel-w-mobile);
        right: 1vw;
        left: 1vw;
        padding: 22px 4vw 22px 4vw;
        min-width: 0;
      }
    }
#escolha-modo {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #fff;
  border-radius: 22px;
  box-shadow: 0 4px 32px #aac2ee44;
  border: 2.5px solid var(--border);
  padding: 32px 38px 28px 38px;
  max-width: 420px;
  width: 96vw;
  z-index: 6000;
  transition: box-shadow .18s, background .18s;
}

#escolha-modo h2 {
  color: var(--player1-light);
  font-size: 1.6em;
  font-weight: bold;
  margin-bottom: 28px;
  letter-spacing: 1.5px;
  text-align: center;
}

.botao-modo {
  width: 260px;
  max-width: 80vw;
  padding: 16px 0;
  margin-bottom: 18px;
  font-size: 1.18em;
  font-weight: 800;
  color: #fff;
  background: linear-gradient(90deg, var(--player1-light) 0, var(--player2-light) 100%);
  border: none;
  border-radius: 12px;
  cursor: pointer;
  box-shadow: 0 2px 11px #319e7277;
  letter-spacing: 1.7px;
  transition: background .15s, box-shadow .16s, filter .12s;
  outline: none;
  border: 2px solid var(--border);
}
.botao-modo:hover, .botao-modo:focus {
  filter: brightness(1.09) contrast(1.12);
  box-shadow: 0 4px 18px #aac2eeaa;
  outline: none;
}

@media (max-width: 600px) {
  #escolha-modo {
    padding: 18px 4vw 18px 4vw;
    max-width: 98vw;
    margin-top: 18px;
  }
  .botao-modo {
    width: 90vw;
    font-size: 1em;
    padding: 13px 0;
  }
  #escolha-modo h2 {
    font-size: 1.1em;
    margin-bottom: 18px;
  }
}
    #placar, #placar-invertido {
      font-size: 2.4rem;
      font-weight: bold;
      background: #fff;
      border-radius: 18px;
      box-shadow: var(--shadow);
      padding: 10px 44px 10px 44px;
      letter-spacing: 2px;
      border: 2px solid var(--border);
      min-width: 260px;
      text-align: center;
      text-shadow: 1px 1px 0 #fff, 0 2px 8px #aac2ee55;
      color: #fff;
      position: relative;
      background: linear-gradient(90deg, var(--player1-light) 0 50%, var(--player2-light) 50% 100%);
      margin-bottom: 18px;
      margin-top: 0;
      transition: box-shadow .2s;
      user-select: none;
    }
    #placar {
      margin-top: 36px;
    }
    #placar-invertido {
      margin-top: 30px;
      margin-bottom: 20px;
      transform: rotate(180deg);
    }
    #turnbar {
      width: 100%;
      height: var(--turnbar-h);
      border-radius: 7px;
      margin: 0 auto 18px auto;
      background: linear-gradient(90deg, var(--player1-light) 0 50%, var(--player2-light) 50% 100%);
      position: relative;
      box-shadow: 0 0 12px #aac2ee33;
      max-width: 400px;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    #turnindicator {
      height: 100%;
      border-radius: 7px;
      position: absolute;
      top: 0; left: 0;
      transition: left 0.3s;
      width: 50%;
      background: var(--accent2);
      opacity: 0.7;
      z-index: 1;
      box-shadow: 0 0 10px 4px #1de9b644;
    }
    .tabuleiro {
      display: grid;
      grid-template-columns: repeat(6, minmax(48px, 10vw));
      grid-template-rows: repeat(4, minmax(48px, 10vw));
      gap: 2vw;
      justify-content: center;
      background: #f6fcff;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 4vw 2vw 4vw 2vw;
      margin: 0 auto 0 auto;
      max-width: 100vw;
    }
    .casa {
      width: 10vw;
      height: 10vw;
      max-width: 112px;
      max-height: 112px;
      min-width: 50px;
      min-height: 50px;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3.6vw;
      font-weight: 700;
      border-radius: 1.5vw;
      box-shadow: 0 2px 8px #2223;
      cursor: pointer;
      transition: background .16s, color .13s, box-shadow .23s, border .18s;
      box-sizing: border-box;
      position: relative;
      z-index: 1;
      letter-spacing: 0.8vw;
      user-select: none;
      outline: none;
      text-shadow: 1px 1px 5px #0008;
      will-change: background, box-shadow, border;
      overflow: hidden;
      flex-direction: row;
      flex-wrap: wrap;
      align-content: center;
      border: 3.5px solid var(--border);
      background: var(--dark);
    }
    .casa.j1 {
      background: linear-gradient(145deg, var(--player1) 85%, #263238 100%);
      border-color: var(--player1-light);
    }
    .casa.j2 {
      background: linear-gradient(35deg, var(--player2) 85%, #263238 100%);
      border-color: var(--player2-light);
    }
    .tabuleiro.j1vez .casa.j1 {
      box-shadow: 0 0 24px 6px #3c4e9b66, 0 2px 8px #2223;
      filter: brightness(1.1) contrast(1.13);
    }
    .tabuleiro.j2vez .casa.j2 {
      box-shadow: 0 0 24px 6px #319e7266, 0 2px 8px #2223;
      filter: brightness(1.1) contrast(1.13);
    }
    .casa .peca {
      display: inline-block;
      width: 19%;
      height: 19%;
      min-width: 13px;
      min-height: 13px;
      max-width: 22px;
      max-height: 22px;
      margin: 2px 2px;
      background: var(--peca);
      border-radius: 50%;
      border: 2px solid var(--peca-borda);
      box-shadow: 0 2px 4px #0005;
      position: relative;
      z-index: 2;
      opacity: 1;
      transition: background 0.2s, border 0.2s, opacity 0.2s;
    }
    .casa .peca.moving {
      background: var(--accent2);
      border: 2px solid var(--accent2);
      box-shadow: 0 0 12px 2px #1de9b699;
      z-index: 5;
      animation: pecaMove 0.3s cubic-bezier(.24,1.4,.68,1.01);
    }
    @keyframes pecaMove {
      from { transform: scale(1.2) rotate(-10deg); }
      to   { transform: scale(1) rotate(0); }
    }
    .casa .peca.capturando {
      background: var(--highlight);
      border: 2px solid var(--highlight);
      box-shadow: 0 0 8px 2px #e5393599;
      animation: pecaCaptura 0.3s cubic-bezier(.23,1.47,.62,1.01);
    }
    @keyframes pecaCaptura {
      0% { transform: scale(1.13); }
      100% { transform: scale(0); opacity: 0; }
    }
    .movimento {
      background: var(--accent) !important;
      animation: movimentoAnim 0.33s cubic-bezier(.23,1.47,.62,1.01);
      z-index: 2;
      box-shadow: 0 0 12px 5px #8ab4f8cc;
    }
    .captura {
      background: var(--highlight) !important;
      color: #fff;
      animation: capturaAnim 0.34s cubic-bezier(.05,1.3,.65,1.01);
      z-index: 2;
      box-shadow: 0 0 24px 8px #e5393580;
      border: 3.5px solid #e53935;
    }
    .final {
      background: var(--final) !important;
      color: #00251a;
      box-shadow: 0 0 18px 8px #1de9b6a0;
      animation: finalAnim 0.43s cubic-bezier(.12,.93,.29,1.01);
      z-index: 3;
      border: 3.5px solid #00251a;
    }
    @keyframes movimentoAnim {
      0% { background: #263238; }
      100% { background: var(--accent); }
    }
    @keyframes capturaAnim {
      0% { background: #c62828; color: #fff; }
      100% { background: var(--highlight); color: #fff; }
    }
    @keyframes finalAnim {
      0% { background: var(--accent); color: #fff; box-shadow: 0 0 0 0 #1de9b6; }
      100% { background: var(--final); color: #00251a; box-shadow: 0 0 18px 8px #1de9b6a0; }
    }
    .captura-tooltip {
      position: absolute;
      left: 50%;
      top: 5px;
      transform: translateX(-50%);
      background: var(--tooltip-bg);
      color: var(--tooltip-color);
      border-radius: 10px;
      padding: 3px 10px 3px 10px;
      font-size: 1.13em;
      font-weight: 900;
      box-shadow: var(--tooltip-shadow);
      border: 2px solid var(--highlight);
      pointer-events: none;
      z-index: 10;
      white-space: nowrap;
      opacity: 0.96;
      transition: opacity .13s;
      user-select: none;
    }
    .casa.j2 .captura-tooltip {
      transform: translateX(-50%) rotate(180deg);
    }
    .casa.inicio-jogada {
      box-shadow: 0 0 0 4px #f9a826cc, 0 2px 8px #2223;
      border: 3.5px solid #f9a826 !important;
      position: relative;
      z-index: 4;
      animation: inicioJogadaPulse 0.7s cubic-bezier(.7,0,.3,1.6) 0s 1 normal both;
    }
    @keyframes inicioJogadaPulse {
      0% { box-shadow: 0 0 0 0 #f9a82600; border-color: #f9a82600; }
      60% { box-shadow: 0 0 0 8px #f9a82688; border-color: #f9a826; }
      100% { box-shadow: 0 0 0 4px #f9a826cc; border-color: #f9a826; }
}
    #controls-bar {
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      gap: 18px;
      margin-bottom: 32px;
      margin-top: 0;
      width: 100%;
    }
    .botao-pequeno {
      padding: 7px 17px;
      font-size: 1.09rem;
      border-radius: 12px;
      border: none;
      background: var(--player1-light);
      color: #fff;
      cursor: pointer;
      font-weight: 700;
      box-shadow: 0 2px 7px #aac2ee55;
      letter-spacing: 1px;
      transition: background .13s, box-shadow .14s, filter .12s;
      outline: none;
      border: 2px solid var(--border);
    }
    .botao-pequeno#btn-voltar-modo {
      background: var(--player2-light);
    }
    .botao-pequeno:hover, .botao-pequeno:focus {
      filter: brightness(1.09) contrast(1.12);
      box-shadow: 0 4px 12px #aac2ee77;
      outline: none;
    }
    #vinheta-vencedor {
      position: fixed;
      left: 0; right: 0;
      top: 0; bottom: 0;
      z-index: 5000;
      pointer-events: none;
      display: none;
      align-items: center;
      justify-content: center;
      background: rgba(40,56,107,0.18);
      animation: vinhetaFadeIn 0.5s cubic-bezier(.5,1.7,.5,1.1) both;
      /* NOVO: efeito de brilho ao fundo */
      box-shadow: 0 0 80px 30px #fff8, 0 0 0 0 transparent;
    }
    @keyframes vinhetaFadeIn {
      0% { opacity: 0; }
      100% { opacity: 1; }
    }
    #vinheta-vencedor .vinheta-msg {
      margin: 0 auto;
      min-width: 220px;
      max-width: 88vw;
      min-height: 60px;
      text-align: center;
      font-size: 2.2rem;
      font-weight: bold;
      border-radius: 28px;
      box-shadow: 0 4px 32px #0008, 0 2px 16px #aac2ee33;
      letter-spacing: 2px;
      padding: 32px 44px 28px 44px;
      border: none;
      opacity: 1;
      background: linear-gradient(90deg, var(--player1-light) 0, var(--player2-light) 100%);
      color: #fff;
      text-shadow: 0 2px 10px #2228, 0 1px 0 #fff;
      animation: vinhetaPulse 1.6s cubic-bezier(.7,0,.3,1.6) 0s 1 normal both, vinhetaGlow 2.5s infinite alternate;
      transition: background 0.7s;
      position: relative;
      overflow: visible;
    }
    #vinheta-vencedor .vinheta-trofeu {
      display: block;
      margin: 0 auto 12px auto;
      width: 54px;
      height: 54px;
      filter: drop-shadow(0 0 12px #fff8);
      animation: trofeuBounce 1.2s cubic-bezier(.6,1.7,.5,1.1) 0s 1 normal both;
    }
    @keyframes vinhetaPulse {
      0% { transform: scale(0.7); opacity: 0; }
      60% { transform: scale(1.08); opacity: 1;}
      80% { transform: scale(0.96);}
      100% { transform: scale(1);}
    }
    @keyframes vinhetaGlow {
      0% { box-shadow: 0 0 32px 8px #fff5, 0 2px 16px #aac2ee33; }
      100% { box-shadow: 0 0 64px 24px #fff9, 0 2px 16px #aac2ee33; }
    }
    @keyframes trofeuBounce {
      0% { transform: scale(0.5) translateY(-40px); opacity: 0; }
      60% { transform: scale(1.2) translateY(10px); opacity: 1;}
      80% { transform: scale(0.95) translateY(-4px);}
      100% { transform: scale(1) translateY(0);}
    }
    @media (max-width: 700px) {
      .tabuleiro {
        grid-template-columns: repeat(6, minmax(30px, 15vw));
        grid-template-rows: repeat(4, minmax(30px, 15vw));
        gap: 2.5vw;
        padding: 6vw 1vw;
      }
      .casa {
        width: 15vw;
        height: 15vw;
        font-size: 7vw;
        letter-spacing: 2vw;
        border-radius: 3vw;
        min-width: 28px; min-height: 28px;
      }
      #placar, #placar-invertido {
        font-size: 1.2rem;
        min-width: 140px;
        padding: 8px 6vw;
      }
      #placar-invertido {
        margin-bottom: 18px;
        margin-top: 20px;
      }
      #escolha-modo {
        width: 98vw;
        padding: 21px 0 24px 0;
      }
      #controls-bar {
        margin-bottom: 20px;
      }
      #mensagem-vencedor, #vinheta-vencedor .vinheta-msg {
        font-size: 1.07rem;
        padding: 8px 8vw 7px 8vw;
      }
      .botao-modo {
        width: 60vw;
      }
    }
    @media (max-width: 400px) {
      .tabuleiro {
        grid-template-columns: repeat(6, 15vw);
        grid-template-rows: repeat(4, 15vw);
        gap: 2vw;
        padding: 3vw 0.5vw;
      }
      .casa {
        width: 15vw;
        height: 15vw;
        font-size: 9vw;
        letter-spacing: 1.5vw;
        border-radius: 4vw;
        min-width: 16px; min-height: 16px;
      }
      #placar, #placar-invertido {
        font-size: 1rem;
        padding: 4px 7px;
        border-radius: 8px;
        min-width: 0;
      }
      #placar-invertido {
        margin-bottom: 12px;
        margin-top: 13px;
      }
      #mensagem-vencedor, #vinheta-vencedor .vinheta-msg {
        font-size: .97rem;
        padding: 6px 4vw 6px 4vw;
      }
      #controls-bar {
        gap: 8px;
      }
      .botao-modo {
        width: 90vw;
      }
    }

    @media (max-width: 350px) {
  .tabuleiro {
    grid-template-columns: repeat(6, 16vw);
    grid-template-rows: repeat(4, 16vw);
    gap: 1vw;
    padding: 2vw 0.5vw;
  }
  .casa {
    width: 16vw;
    height: 16vw;
    font-size: 10vw;
    letter-spacing: 1vw;
    border-radius: 5vw;
    min-width: 12px;
    min-height: 12px;
  }
}

/* Garante rolagem horizontal se o tabuleiro ficar maior que a tela */
.tabuleiro {
  overflow-x: auto;
  max-width: 100vw;
}

/* Centraliza o conteúdo e permite ajuste de zoom em telas muito pequenas */
@media (max-width: 320px) {
  #main-content {
    zoom: 0.85;
  }
}

  /* Responsividade para o modal de tela cheia */
  #modal-full {
  display: none;
  position: fixed;
  z-index: 9999;
  top: 0; left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(40,56,107,0.93);
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 0;
  /* Permite rolar o conteúdo interno em telas pequenas */
  overflow-y: auto;
}
#modal-full-content {
  max-width: 700px;
  width: 96vw;
  min-height: 0;
  background: #fff;
  color: #263238;
  border-radius: 22px;
  box-shadow: 0 4px 38px #3c4e9bdc;
  padding: 38px 24px 30px 24px;
  position: relative;
  margin: 0;
  box-sizing: border-box;
  word-break: break-word;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  /* Permite rolar o texto se for muito longo */
  max-height: 92vh;
  overflow-y: auto;
}
#modal-full-text {
  margin-top: 18px;
  font-size: 1.18em;
  line-height: 1.7;
  word-break: break-word;
  width: 100%;
  /* Garante quebra de linha e rolagem */
  overflow-wrap: break-word;
  white-space: normal;
}
@media (max-width: 800px) {
  #modal-full-content {
    max-width: 98vw;
    width: 98vw;
    padding: 24px 4vw 22px 4vw;
    border-radius: 16px;
    max-height: 94vh;
  }
}
@media (max-width: 500px) {
  #modal-full-content {
    max-width: 100vw;
    width: 100vw;
    padding: 12px 2vw 14px 2vw;
    border-radius: 10px;
    max-height: 98vh;
  }
  #modal-full-text {
    font-size: 1em;
  }
  #modal-full button {
    font-size: 1em !important;
    padding: 6px 12px !important;
    top: 8px !important;
    right: 8px !important;
  }
}
  /* Ajuste para o botão fechar no modal */
  #modal-full-content > button {
    position: absolute;
    top: 18px;
    right: 18px;
    background: #e53935;
    color: #fff;
    border: none;
    border-radius: 8px;
    padding: 7px 18px;
    font-size: 1.1em;
    cursor: pointer;
    font-weight: bold;
    box-shadow: 0 2px 7px #aac2ee66;
    transition: background .13s;
  }
  #modal-full-content > button:hover {
    background: #b71c1c;
  }
  </style>

<style>
#top-icons {
  position: fixed;
  top: 80px; /* logo abaixo do parafuso */
  right: 25px;
  display: flex;
  flex-direction: column;
  gap: 14px;
  z-index: 5000;
}
#top-icons > div {
  width: 40px;
  height: 40px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 2px 12px #aac2ee77;
  border: 2px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: box-shadow 0.14s, background 0.13s;
}
#top-icons > div:hover {
  background: #e3f2fd;
  box-shadow: 0 4px 18px #aac2eeaa;
}
#reiniciar-btn svg path {
  /* Verde player2 */
  fill: #319e72;
}
#modo-btn svg path {
  /* Azul player1 */
  fill: #3c4e9b;
}
@media (max-width: 600px) {
  #top-icons {
    top: 73px;
    right: 28px;
    gap: 10px;
  }
  #top-icons > div {
    width: 34px;
    height: 34px;
  }
  #top-icons > div svg {
    width: 22px;
    height: 22px;
  }
}
#top-icons.invisivel {
  opacity: 0 !important;
  pointer-events: none !important;
  transition: opacity 0.18s;
}

@keyframes reiniciarFade {
  0%   { opacity: 0; transform: scale(0.97);}
  100% { opacity: 1; transform: scale(1);}
}
#main-content.reiniciando {
  animation: reiniciarFade 0.35s cubic-bezier(.4,1.2,.5,1) both;
}
#main-content.reiniciando {
  animation: reiniciarFade 0.7s cubic-bezier(.4,1.6,.5,1) both;
}

    .img-container1 img {
      max-width: 60vw;
      max-height: 70vh;
      border-radius: 16px;
      object-fit: contain;
      display: block;
      opacity: 0.20;
    }
</style>

  <script>
    // Impede o menu do botão direito em toda a página
    document.addEventListener('contextmenu', function(e) {
      e.preventDefault();
      return false;
    });
    
  </script>

<!-- Adicione logo após a tag <body> -->
<div id="splash-screen">
  <img class="logo-principal" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhw2UPEqumpy3zbbntN2duz9dc5HNOForW0VFQ8xcen8WoxGIRMZrtx8bMTjm7C6Hiy5jBBBYG4nPQVYc1gQm_X5-YhEBEk5WQ6R7phMPc9UAmielxI-pu_AHJypj4OWcJyrNyA4uMTlqHdcVoh5uoQH3r6EavUkrd5fC3Pk6UKT8D8OQcsGqrhvvCJ4jsV/s1600/Ntxuva%20logo.png" alt="Ntxuva Logo">
  <img class="logo-spoiler" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_W3fn6uFAOExhWtYY0ikcGlUSARrUHv_GVB3ztOKxPol7h4dwqIVN47T73SXOPLWEZ-UsMiumgMMAM-6wj_iypQwlp324W2kSGq1dbsaAu6Ol5fp4XRt3cfKR7HUz5cjDzlojvzMKbqKk-sUAl4Ql1Fk3cYFC98Cms_nKBMbVO5WF8tqmrTTdGexrCiqu/s1600/SPOILER%20tec%20logo.png" alt="Spoiler Tec Logo">
</div>
<style>
#splash-screen {
  position: fixed;
  z-index: 99999;
  inset: 0;
  background: #eaf6ff;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  transition: opacity .5s;
}

#splash-screen .logo-principal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60vw;
  max-width: 220px;
  max-height: 22vh;
  object-fit: contain;
  display: block;
  margin: 0;
}

#splash-screen .logo-spoiler {
  position: absolute;
  left: 50%;
  bottom: 1.5vh;
  transform: translateX(-50%);
  max-width: 22vw;
  width: 60px;
  max-height: 7vh;
  object-fit: contain;
  opacity: 0.92;
}

@media (max-width: 600px) {
  #splash-screen .logo-principal { width: 70vw; max-width: 200px; }
  #splash-screen .logo-spoiler { width: 22vw; max-width: 38vw; }
}

#form-doacao-area {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
<script>
window.addEventListener('load', function() {
  setTimeout(function() {
    var splash = document.getElementById('splash-screen');
    splash.style.opacity = '0';
    setTimeout(function() {
      splash.style.display = 'none';
    }, 500);
  }, 5000); // 5 segundos
});

</script>


  <div id="settings-btn" title="Configurações" onclick="openSettingsPanel(event)">
    <svg id="settings-icon" viewbox="0 0 24 24">
      <g>
        <circle cx="12" cy="12" r="3.3"/>
        <path d="M19.43 12.98c.04-.32.07-.65.07-.98s-.03-.66-.07-.98l2.11-1.65c.19-.15.24-.41.12-.62l-2-3.46a.495.495 0 0 0-.6-.22l-2.49 1a7.03 7.03 0 0 0-1.7-.98l-.38-2.65a.488.488 0 0 0-.48-.41h-4a.488.488 0 0 0-.48.41l-.38 2.65c-.63.24-1.21.56-1.7.98l-2.49-1a.495.495 0 0 0-.6.22l-2 3.46a.5.5 0 0 0 .12.62l2.11 1.65c-.04.32-.07.65-.07.98s.03.66.07.98l-2.11 1.65a.495.495 0 0 0-.12.62l2 3.46c.13.22.39.28.6.22l2.49-1c.53.41 1.1.74 1.7.98l.38 2.65c.06.28.3.41.48.41h4c.19 0 .42-.13.48-.41l.38-2.65a7.03 7.03 0 0 0 1.7-.98l2.49 1c.22.09.47-.01.6-.22l2-3.46a.5.5 0 0 0-.12-.62l-2.11-1.65z"/>
      </path></circle></g>
    </svg>
  </div>
  <div id="settings-panel-bg" onclick="closeSettingsPanel()"></div>
  <div id="settings-panel">
    <div class="settings-title">Configurações</div>
    <div class="settings-group">
      <button class="settings-btn" onclick="abrirTutorial()">📖 Tutorial</button>
      <button class="settings-btn" onclick="abrirSuporte()">💬 Suporte</button>
      <button class="settings-btn loja-btn" onclick="abrirLoja()" style="background:#3c4e9b;color:#fff;font-weight:900;">🛒 Loja</button>
      <button class="settings-btn donate" onclick="abrirDoacao(event)">💛 Doar</button>
      <button class="settings-btn" onclick="abrirCreditos()">👨‍💻 Créditos</button>
    </div>
<div class="settings-group" id="audio-controls">
  <button id="btn-audio-on" title="Ativar som" onclick="setAudio(true)" style="display:none;">
    <svg width="28" height="28" viewbox="0 0 24 24" fill="none"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v2.06c.58.35 1 .98 1 1.72s-.42 1.37-1 1.72v2.06c1.48-.74 2.5-2.26 2.5-4.03z" fill="#319e72"/></path></svg>
  </button>
  <button id="btn-audio-off" title="Desativar som" onclick="setAudio(false)">
    <svg width="28" height="28" viewbox="0 0 24 24" fill="none"><path d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.06c.58.35 1 .98 1 1.72s-.42 1.37-1 1.72v2.06c1.48-.74 2.5-2.26 2.5-4.03zM19 3L4.27 17.73 5.73 19.19 21 4.92 19 3zM3 9v6h4l5 5V4L7 9H3z" fill="#e53935"/></path></svg>
  </button>
  <input id="audio-volume" type="range" min="0" max="1" step="0.01" value="1" style="vertical-align:middle; width:90px; margin-left:12px;">
</div>
    <button class="close-btn" onclick="closeSettingsPanel()">Fechar</button>
  </div>

</div>

<div id="top-icons">
  <div id="reiniciar-btn" title="Reiniciar Jogo" onclick="reiniciarJogo()">
    <!-- Ícone de refresh -->
    <svg viewBox="0 0 24 24" width="26" height="26">
      <path fill="#319e72" d="M12 6V3l-5 5 5 5V8c2.76 0 5 2.24 5 5s-2.24 5-5 5-5-2.24-5-5H5c0 3.87 3.13 7 7 7s7-3.13 7-7-3.13-7-7-7z"/>
    </svg>
  </div>
  <div id="modo-btn" title="Escolher Modo" onclick="voltarEscolherModo()">
    <!-- Ícone de home -->
    <svg viewBox="0 0 24 24" width="26" height="26">
      <path fill="#3c4e9b" d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/>
    </svg>
  </div>
</div>

  <!-- Modal tela cheia -->
<div id="modal-full" style="display:none;position:fixed;z-index:9999;top:0;left:0;width:100vw;height:100vh;background:rgba(40,56,107,0.93);color:#fff;align-items:center;justify-content:center;flex-direction:column;padding:0;">
  <div id="modal-full-content" style="max-width:700px;width:96vw;min-height:0;background:#fff;color:#263238;border-radius:22px;box-shadow:0 4px 38px #3c4e9bdc;padding:38px 24px 30px 24px;position:relative;margin:0;box-sizing:border-box;word-break:break-word;display:flex;flex-direction:column;align-items:center;justify-content:flex-start;">
    <button onclick="fecharModalFull()" style="position:absolute;top:18px;right:18px;background:#e53935;color:#fff;border:none;border-radius:8px;padding:7px 18px;font-size:1.1em;cursor:pointer;font-weight:bold;box-shadow:0 2px 7px #aac2ee66;">Fechar</button>
    <div id="modal-full-text" style="margin-top:18px;font-size:1.18em;line-height:1.7;"></div>
  </div>
</div>

  <div id="escolha-modo">
    <h2>Escolha o modo de jogo:</h2>
    <button class="botao-modo" onclick="iniciarComAmigo()">Contra Amigo</button>
    <button class="botao-modo" onclick="mostrarEscolhaNivel()">Contra Computador (IA)</button>
  </div>

<div id="escolha-nivel" style="display:none; position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); background:#fff; border-radius:18px; box-shadow:0 4px 32px #aac2ee44; border:2.5px solid #aac2ee; padding:32px 38px 28px 38px; z-index:5991; max-width:340px; width:90vw; text-align:center;">
  <h2 style="color:#1976d2; margin-bottom:22px;">Escolha o adversário IA:</h2>
  <button class="botao-modo" style="margin-bottom:12px;" onclick="escolherNivelComputador('puto')">Gerson</button><br>
  <button class="botao-modo" style="margin-bottom:12px;" onclick="escolherNivelComputador('leo')">Leonardo</button><br>
  <button class="botao-modo" style="margin-bottom:18px;" onclick="escolherNivelComputador('tembe')">Tembe</button><br>
  <button class="botao-pequeno" onclick="fecharEscolhaNivel()">Cancelar</button>
</div>

  <div id="main-content">
      <div class="img-container1">
    <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhMBQ_AK8kGNm6c_xLufmHvEDnRjnBDcN6J4r50stJwV5TUbR1_oQpZ6IkqkAllf9pP8KnqRjSWKp_Xv5AvVKeb2_LK8OKmENAp4tMYJxQddJYgKkLP__bynFC9exm2ITeNum_lykRzmtEMinTZJPNdyAHuhuqtwXa4Sn_IHNes4oGmadTKgew9mxgz6qku/s1600/back1.png" alt="Imagem 2">
  </div>
    <div id="placar"></div>
    <div id="turnbar">
      <div id="turnindicator"></div>
    </div>
    <div class="tabuleiro" id="tabuleiro"></div>
    <div id="mensagem-vencedor"></div>
    <div id="vinheta-vencedor"><div class="vinheta-msg"></div></div>
    <div id="placar-invertido"></div>
      <div class="img-container1">
    <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgciKCBicd1aZbXoBR6zAtzv5pbtCCsfXFZSo8VdBCNETrADYQJqH0SFQeCDxd4B_S94oncRpB4_kUHbmTBT0JW2NL2u7lSI9QoxX4ZHCdlAtiCUJmnFjWkzSR-6jG-mbZurHRmbCybmlhdZe3DdoaZScVAyQQjXvGTG9ZMOx-BJ4atQMN0n0I4fG3fzuwq/s1600/back2.png" alt="Imagem 1">
  </div>
    <div id="controls-bar">
    </div>
  </div>
  <audio id="som-movimento" src="https://cdn.pixabay.com/download/audio/2022/03/24/audio_92f13fce0d.mp3?filename=bow-release-bow-and-arrow-4-101936.mp3" preload="auto"></audio>
  <audio id="som-captura"  src="https://cdn.pixabay.com/download/audio/2024/08/03/audio_169049f4a1.mp3?filename=arcade-ui-6-229503.mp3" preload="auto"></audio>
  <audio id="som-vitoria"   src="https://cdn.pixabay.com/download/audio/2024/08/03/audio_ddc41966d6.mp3?filename=arcade-ui-26-229495.mp3" preload="auto"></audio>
  <audio id="som-erro"      src="https://cdn.pixabay.com/audio/2022/03/15/audio_115b9c6b51.mp3" preload="auto"></audio>
  <script>

function abrirLoja() {
  document.getElementById('modal-full-text').innerHTML = `
    <h2 style="text-align:center;color:#f9a826;">Loja Oficial</h2>
    <ul style="list-style:none;padding:0;margin:0;">
BREVEMENTE
    <div style="color:#888;font-size:0.95em;margin-top:12px;">* Volte aqui sempre, pois a qualquer momento a loja estara pronta.</div>
  `;
  abrirModalFull();
}

function comprarItem(item) {
  alert('Você selecionou "' + item + '". Em breve disponível para compra!');
}

// Configurações (parafuso)
    function openSettingsPanel(e) {
      e.stopPropagation && e.stopPropagation();
      document.getElementById('settings-panel').classList.add('open');
      document.getElementById('settings-panel-bg').classList.add('open');
      playSound('som-movimento'); // efeito sonoro ao abrir
    }
    function closeSettingsPanel() {
      document.getElementById('settings-panel').classList.remove('open');
      document.getElementById('settings-panel-bg').classList.remove('open');
      playSound('som-movimento'); // efeito sonoro ao fechar
    }
    // Ação do botão de doação
    function doar(e) {
      e && e.stopPropagation && e.stopPropagation();
      window.open("https://www.buymeacoffee.com/MbiqueTecnologia", "_blank");
    }

    let casas, jogadorAtual, ultimosMovimentos, ultimasCapturas, ultimaCasaFinal, capturadas1, capturadas2, jogoFinalizado;
    let animPecas = [];
    let animStep = 0, animMovimentos = [], animCapturasIdx = [], animCasaFinal = null, animating = false, animDoCaptura = false;
    let animCapturaTimeout = null;
    let nextCapturasHighlight = [];
    let captureTooltipMap = {};

    let modoJogo = null; // 1 = amigo, 2 = computador
    let jogandoContraComputador = false;
    let animVencedorTimeout = null;

let nivelComputador = "tembe"; // padrão

function mostrarEscolhaNivel() {
  document.getElementById("escolha-nivel").style.display = "";
  document.getElementById("escolha-modo").style.display = "none";
}
function fecharEscolhaNivel() {
  document.getElementById("escolha-nivel").style.display = "none";
  document.getElementById("escolha-modo").style.display = "";
}
function escolherNivelComputador(nivel) {
  nivelComputador = nivel;
  document.getElementById("escolha-nivel").style.display = "none";
  iniciarComComputador();
}

    const ordem1 = [6, 7, 8, 9, 10, 11, 5, 4, 3, 2, 1, 0];
    const ordem2 = [18, 19, 20, 21, 22, 23, 17, 16, 15, 14, 13, 12];

    const ataque1 = [6, 7, 8, 9, 10, 11];
    const ataque2 = [12, 13, 14, 15, 16, 17];

    const mapaAtaque = {
      6: { adv_ataque: 12, adv_defesa: 18 },
      7: { adv_ataque: 13, adv_defesa: 19 },
      8: { adv_ataque: 14, adv_defesa: 20 },
      9: { adv_ataque: 15, adv_defesa: 21 },
      10: { adv_ataque: 16, adv_defesa: 22 },
      11: { adv_ataque: 17, adv_defesa: 23 },
      12: { adv_ataque: 6, adv_defesa: 0 },
      13: { adv_ataque: 7, adv_defesa: 1 },
      14: { adv_ataque: 8, adv_defesa: 2 },
      15: { adv_ataque: 9, adv_defesa: 3 },
      16: { adv_ataque: 10, adv_defesa: 4 },
      17: { adv_ataque: 11, adv_defesa: 5 },
    };

let historicoEstadosIA = [];

// Função para gerar uma assinatura única do estado do tabuleiro
function estadoTabuleiroAssinatura(casas, jogadorAtual) {
  return casas.join(',') + '|' + jogadorAtual;
}

function mostrarEscolhaModo() {
      document.getElementById("escolha-modo").style.display = "";
      document.getElementById("top-icons").classList.add("invisivel");
      document.getElementById("mensagem-vencedor").style.display = "none";
      document.getElementById("vinheta-vencedor").style.display = "none";
      document.getElementById("main-content").classList.add("desfocado");
      clearTimeout(animVencedorTimeout);
    }
function esconderEscolhaModo() {
      document.getElementById("escolha-modo").style.display = "none";
      document.getElementById("top-icons").classList.remove("invisivel");
      document.getElementById("main-content").classList.remove("desfocado");
    }

    function iniciarComAmigo() {
      modoJogo = 1;
      jogandoContraComputador = false;
      esconderEscolhaModo();
      playSound('som-movimento'); // efeito ao escolher modo
      inicializarJogo();
    }
    function iniciarComComputador() {
      modoJogo = 2;
      jogandoContraComputador = true;
      esconderEscolhaModo();
      playSound('som-movimento'); // efeito ao escolher modo
      inicializarJogo();
      if (modoJogo === 2 && jogadorAtual === 1) setTimeout(jogadaComputador, 700);
    }

    function inicializarJogo() {
      casas = Array(24).fill(2);
      if (modoJogo === 2) {
      jogadorAtual = Math.random() < 0.5 ? 1 : 2; // sorteia quem começa também contra computador
      } else {
      jogadorAtual = Math.random() < 0.5 ? 1 : 2;
      }
      ultimosMovimentos = [];
      ultimasCapturas = [];
      ultimaCasaFinal = null;
      capturadas1 = 0;
      capturadas2 = 0;
      jogoFinalizado = false;
      animating = false;
      animDoCaptura = false;
      nextCapturasHighlight = [];
      captureTooltipMap = {};
      atualizarTurnoBar();
      renderTabuleiro();
      atualizarPlacar();
      document.getElementById("mensagem-vencedor").style.display = "none";
      document.getElementById("vinheta-vencedor").style.display = "none";
      clearTimeout(animVencedorTimeout);
    }

    function renderTabuleiro() {
      const tabuleiro = document.getElementById("tabuleiro");
      tabuleiro.classList.remove("j1vez", "j2vez");
      tabuleiro.classList.add(jogadorAtual === 1 ? "j1vez" : "j2vez");
      tabuleiro.innerHTML = "";
      for (let i = 0; i < 24; i++) {
        const div = document.createElement("div");
        if (i >= 0 && i <= 11) div.className = "casa j1";
        else div.className = "casa j2";

        if (
          (animating && nextCapturasHighlight && nextCapturasHighlight.includes(i)) ||
          ultimasCapturas.includes(i)
        ) {
          div.classList.add("captura");
        }
        if (ultimosMovimentos.includes(i)) div.classList.add("movimento");
        if (i === ultimaCasaFinal) div.classList.add("final");
        if (!(jogandoContraComputador && jogadorAtual === 1) && !jogoFinalizado) {
          div.onclick = () => jogar(i);
        } else {
          div.style.cursor = "not-allowed";
        }

        div.innerHTML = "";
        let pecasNormais = casas[i];
        let pecasAnim = 0, pecasCapt = 0;

        if (animating) {
          pecasAnim = animPecas.filter(p => p.to === i && p.step === animStep && p.visible).length;
          pecasNormais -= pecasAnim;
          if (animDoCaptura && animCapturasIdx.includes(i)) {
            pecasCapt = animPecas.filter(p => p.to === i && p.captura).length;
            pecasNormais -= pecasCapt;
          }
        }

        for(let p=0; p<Math.max(0,pecasNormais); p++) {
          let pecad = document.createElement("span");
          pecad.className = "peca";
          div.appendChild(pecad);
        }
        if (pecasAnim > 0) {
          for(let p=0; p<pecasAnim; p++) {
            let pecad = document.createElement("span");
            pecad.className = "peca moving";
            div.appendChild(pecad);
          }
        }
        if (pecasCapt > 0) {
          for(let p=0; p<pecasCapt; p++) {
            let pecad = document.createElement("span");
            pecad.className = "peca capturando";
            div.appendChild(pecad);
          }
        }
        if (
          ((animating && nextCapturasHighlight && nextCapturasHighlight.includes(i)) ||
           ultimasCapturas.includes(i)
          ) &&
          captureTooltipMap[i] > 0
        ) {
          let tooltip = document.createElement("span");
          tooltip.className = "captura-tooltip";
          tooltip.innerText = "+" + captureTooltipMap[i];
          div.appendChild(tooltip);
        }
        tabuleiro.appendChild(div);
      }
    }

    function emFase2(jogador) {
      const ordem = jogador === 1 ? ordem1 : ordem2;
      return ordem.every(i => casas[i] <= 1);
    }

    let audioEnabled = true;

function setAudio(enable) {
  audioEnabled = enable;
  document.getElementById('btn-audio-on').style.display = enable ? 'none' : '';
  document.getElementById('btn-audio-off').style.display = enable ? '' : 'none';
  // Feedback sonoro ao ativar/desativar
  if (enable) playSound('som-movimento');
}
setAudio(true); // Garante que o estado inicial é "ligado"

// Garante que o áudio seja liberado no primeiro toque/click do usuário (requisito de navegadores móveis)
window.addEventListener('touchstart', liberarAudio, { once: true });
window.addEventListener('click', liberarAudio, { once: true });
function liberarAudio() {
  // Toca e pausa rapidamente todos os áudios para liberar o autoplay
  ['som-movimento', 'som-captura', 'som-vitoria', 'som-erro'].forEach(function(id) {
    var el = document.getElementById(id);
    if (el) {
      try {
        el.muted = true;
        el.play().catch(()=>{});
        setTimeout(function() {
          el.pause();
          el.currentTime = 0;
          el.muted = false;
        }, 50);
      } catch(e) {}
    }
  });
}

// Função de tocar áudio, sempre respeitando o estado do botão
function playSound(id) {
  if (!audioEnabled) return;
  const el = document.getElementById(id);
  if (!el) return;
  try {
    el.pause();
    el.currentTime = 0;
    el.volume = audioVolume;
    el.play().catch(()=>{});
  } catch(e) {}
}

let audioVolume = 1;

document.getElementById('audio-volume').addEventListener('input', function(e) {
  audioVolume = parseFloat(e.target.value);
  // Atualiza o volume de todos os áudios
  ['som-movimento', 'som-captura', 'som-vitoria', 'som-erro'].forEach(function(id) {
    const el = document.getElementById(id);
    if (el) el.volume = audioVolume;
  });
});

    function animarMovimento(seq, capturas, casaFinal, callback, tipoCaptura, capturasPorCasa) {
      animating = true;
      animDoCaptura = false;
      animStep = 0;
      animMovimentos = seq;
      animCapturasIdx = capturas;
      animCasaFinal = casaFinal;
      animPecas = [];
      if (animCapturaTimeout) clearTimeout(animCapturaTimeout);

      nextCapturasHighlight = [...capturas];
      captureTooltipMap = {};
      if (capturasPorCasa) {
        for (const k in capturasPorCasa) {
          if (capturasPorCasa[k] > 0) captureTooltipMap[k] = capturasPorCasa[k];
        }
      }

      let interval = 110;
      let totalSteps = seq.length - 1;

      for(let i=1; i<=totalSteps; i++) {
        animPecas.push({ from: seq[i-1], to: seq[i], step: i, visible: true, captura: false });
      }

function doStep() {
  renderTabuleiro();
  // Só toca som de movimento se NÃO houver capturas
  if (capturas.length === 0) playSound('som-movimento');
  if (animStep < totalSteps) {
    animStep++;
    setTimeout(doStep, interval);
  } else {
    setTimeout(() => {
      if (capturas.length > 0) playSound('som-captura');
      animDoCaptura = true;
      animPecas.forEach(p => {
        if (capturas.includes(p.to) && p.step === animStep) p.captura = true;
      });
      renderTabuleiro();
      animCapturaTimeout = setTimeout(() => {
        animating = false;
        animDoCaptura = false;
        animPecas = [];
        nextCapturasHighlight = [];
        captureTooltipMap = {};
        ultimosMovimentos = seq;
        ultimasCapturas = capturas;
        ultimaCasaFinal = casaFinal;
        renderTabuleiro();
        setTimeout(callback, 180);
      }, 380);
    }, 160);
  }
}
      doStep();
    }

    function jogar(index) {
      if (animating || jogoFinalizado) return;
      if (jogandoContraComputador && jogadorAtual === 1) return;

      ultimosMovimentos = [];
      ultimasCapturas = [];
      ultimaCasaFinal = null;

      const ordem = jogadorAtual === 1 ? ordem1 : ordem2;
      const ataque = jogadorAtual === 1 ? ataque1 : ataque2;
      const fase2 = emFase2(jogadorAtual);

      let pos = ordem.indexOf(index);
      if (pos === -1) {
        playSound('som-erro');
        return;
      }

      if (!fase2) {
        if (casas[index] <= 1 && ordem.some(i => casas[i] > 1)) {
          playSound('som-erro');
          return;
        }
        let sementes = casas[index];
        casas[index] = 0;
        let movimentos = [index];
        let capturas = [];
        let capturasPorCasa = {};
        let ultimaCasa = null;
        let capturadasEssaJogada = 0;
        let tipoCaptura = "";

        while (sementes > 0) {
          pos = (pos + 1) % 12;
          casas[ordem[pos]]++;
          movimentos.push(ordem[pos]);
          sementes--;
        }
        ultimaCasa = ordem[pos];

        while (casas[ultimaCasa] > 1) {
          sementes = casas[ultimaCasa];
          casas[ultimaCasa] = 0;
          movimentos.push(ultimaCasa);
          while (sementes > 0) {
            pos = (pos + 1) % 12;
            casas[ordem[pos]]++;
            movimentos.push(ordem[pos]);
            sementes--;
          }
          ultimaCasa = ordem[pos];
        }

        if (ataque.includes(ultimaCasa)) {
          const dados = mapaAtaque[ultimaCasa];
          const a = dados.adv_ataque;
          const d = dados.adv_defesa;
          if (casas[a] > 0) {
            capturadasEssaJogada += casas[a];
            capturas.push(a);
            capturasPorCasa[a] = casas[a];
            casas[a] = 0;
            if (casas[d] > 0) {
              capturadasEssaJogada += casas[d];
              capturas.push(d);
              capturasPorCasa[d] = casas[d];
              casas[d] = 0;
              tipoCaptura = "dupla";
            } else {
              tipoCaptura = "simples";
            }
          }
        }

        if (jogadorAtual === 1) {
          capturadas1 += capturadasEssaJogada;
        } else {
          capturadas2 += capturadasEssaJogada;
        }

        animarMovimento(movimentos, capturas, ultimaCasa, () => {
          atualizarTurnoBar();
          atualizarPlacar();
          if (checarFimDeJogo()) return;
          trocarJogador();
          renderTabuleiro();
          if (jogandoContraComputador && jogadorAtual === 1) setTimeout(jogadaComputador, 700);
        }, tipoCaptura, capturasPorCasa);
      } else {
        if (casas[index] !== 1) {
          playSound('som-erro');
          return;
        }

        casas[index] = 0;
        let movimentos = [index];
        pos = (pos + 1) % 12;
        const destino = ordem[pos];

        if (casas[destino] === 1) {
          playSound('som-erro');
          casas[index] = 1;
          return;
        }

        casas[destino] = 1;
        movimentos.push(destino);

        let capturas = [];
        let capturasPorCasa = {};
        let capturadasEssaJogada = 0;
        let tipoCaptura = "";

        if (ataque.includes(destino)) {
          const dados = mapaAtaque[destino];
          const a = dados.adv_ataque;
          const d = dados.adv_defesa;
          if (casas[a] > 0) {
            capturadasEssaJogada += casas[a];
            capturas.push(a);
            capturasPorCasa[a] = casas[a];
            casas[a] = 0;
            if (casas[d] > 0) {
              capturadasEssaJogada += casas[d];
              capturas.push(d);
              capturasPorCasa[d] = casas[d];
              casas[d] = 0;
              tipoCaptura = "dupla";
            } else {
              tipoCaptura = "simples";
            }
          }
        }

        if (jogadorAtual === 1) {
          capturadas1 += capturadasEssaJogada;
        } else {
          capturadas2 += capturadasEssaJogada;
        }

        animarMovimento(movimentos, capturas, destino, () => {
          atualizarTurnoBar();
          atualizarPlacar();
          if (checarFimDeJogo()) return;
          trocarJogador();
          renderTabuleiro();
          if (jogandoContraComputador && jogadorAtual === 1) setTimeout(jogadaComputador, 700);
        }, tipoCaptura, capturasPorCasa);
      }
    }

    function checarFimDeJogo() {
      const total1 = ordem1.reduce((s, idx) => s + casas[idx], 0);
      const total2 = ordem2.reduce((s, idx) => s + casas[idx], 0);

      let vencedor = null;
      if (total1 === 0 && total2 > 0) vencedor = 2;
      if (total2 === 0 && total1 > 0) vencedor = 1;

      if (vencedor) {
        jogoFinalizado = true;
        renderTabuleiro();
        atualizarTurnoBar();
        atualizarPlacar();
        playSound('som-vitoria');
        exibirVencedor(vencedor);
        return true;
      }
      return false;
    }

    function exibirVencedor(vencedor) {
      let msg = "";
      let el = document.getElementById("vinheta-vencedor");
      let msgEl = el.querySelector('.vinheta-msg');
      el.classList.remove("v1", "v2");
      let trofeuSVG = `<svg class="vinheta-trofeu" viewBox="0 0 64 64" fill="none"><ellipse cx="32" cy="60" rx="18" ry="4" fill="#fff" opacity="0.18"/><path d="M16 8h32v8a16 16 0 0 1-32 0V8z" fill="#FFD700" stroke="#fff" stroke-width="2"/><path d="M12 8v8c0 11 8 20 20 20s20-9 20-20V8" stroke="#FFD700" stroke-width="3" fill="none"/><circle cx="32" cy="8" r="6" fill="#FFD700" stroke="#fff" stroke-width="2"/></svg>`;
      if (vencedor === 1) {
        msg = jogandoContraComputador
          ? "O Computador venceu!"
          : "Jogador 1 venceu!";
        el.classList.add("v1");
      } else if (vencedor === 2) {
        msg = jogandoContraComputador
          ? "Você venceu!"
          : "Jogador 2 venceu!";
        el.classList.add("v2");
      }
      msgEl.innerHTML = trofeuSVG + `<div>${msg}</div>`;
      el.style.display = "flex";
      clearTimeout(animVencedorTimeout);
      animVencedorTimeout = setTimeout(() => {
        el.style.display = "none";
        reiniciarJogo(); 
      }, 5000);
    }

    function atualizarPlacar() {
      document.getElementById("placar").textContent = `${capturadas1} | ${capturadas2}`;
      document.getElementById("placar-invertido").textContent = `${capturadas1} | ${capturadas2}`;
    }

    function atualizarTurnoBar() {
      const ind = document.getElementById("turnindicator");
      if (!ind) return;
      ind.style.left = jogadorAtual === 1 ? "0%" : "50%";
      ind.style.width = "50%";
      ind.style.background = jogadorAtual === 1 ? "var(--player1-light)" : "var(--player2-light)";
      ind.style.opacity = "0.7";
    }

    function trocarJogador() {
      jogadorAtual = jogadorAtual === 1 ? 2 : 1;
    }

    function reiniciarJogo() {
      playSound('som-movimento');
      const main = document.getElementById('main-content');
      main.classList.add('reiniciando');
      setTimeout(() => {
      inicializarJogo();
      main.classList.remove('reiniciando');
    // Se for modo computador e for a vez dele, faz ele jogar
      if (modoJogo === 2 && jogadorAtual === 1) {
      setTimeout(jogadaComputador, 700);
      }
      }, 700); // tempo igual ao da animação
    }
    function voltarEscolherModo() {
      playSound('som-movimento'); // efeito ao voltar para escolha de modo
      mostrarEscolhaModo();
    }

    // Estratégia MUITO difícil para o computador na fase 2: evita completamente dar peças, se possível

function jogadaComputador() {
  if (jogoFinalizado || jogadorAtual !== 1 || animating) return;

  // Multiplicadores por nível
  let mult = {
    puto:   { capt: 40, ataque: 2, risco: 1, fase2: 400, fase2Capt: 40 },
    leo:    { capt: 120, ataque: 8, risco: 2, fase2: 1200, fase2Capt: 120 },
    tembe:  { capt: 500, ataque: 40, risco: 3, fase2: 8000, fase2Capt: 500 }
  }[nivelComputador];


  // Limite o histórico para os últimos 20 estados
  if (historicoEstadosIA.length > 20) historicoEstadosIA.shift();

  const estadoAtual = estadoTabuleiroAssinatura(casas, jogadorAtual);

  const ordem = ordem1;
  const ataque = ataque1;
  const fase2 = emFase2(1);

  let melhores = [];
  let maxScore = -Infinity;
  let jogadasPontuadas = [];

  if (!fase2) {
    for (let idx of ordem) {
      if (casas[idx] <= 1 && ordem.some(i => casas[i] > 1)) continue;
      let snapshot = casas.slice();
      let movimentos = [idx];
      let pos = ordem.indexOf(idx);
      let sementes = snapshot[idx];
      if (sementes < 1) continue;
      snapshot[idx] = 0;
      while (sementes > 0) {
        pos = (pos + 1) % 12;
        snapshot[ordem[pos]]++;
        movimentos.push(ordem[pos]);
        sementes--;
      }
      let ultimaCasa = ordem[pos];
      while (snapshot[ultimaCasa] > 1) {
        sementes = snapshot[ultimaCasa];
        snapshot[ultimaCasa] = 0;
        movimentos.push(ultimaCasa);
        while (sementes > 0) {
          pos = (pos + 1) % 12;
          snapshot[ordem[pos]]++;
          movimentos.push(ordem[pos]);
          sementes--;
        }
        ultimaCasa = ordem[pos];
      }
      let capt = 0;
      let ataqueBonus = 0;
      if (ataque.includes(ultimaCasa)) {
        const dados = mapaAtaque[ultimaCasa];
        const a = dados.adv_ataque, d = dados.adv_defesa;
        if (snapshot[a] > 0) {
          capt += snapshot[a];
          if (snapshot[d] > 0) capt += snapshot[d];
          ataqueBonus += 2;
        }
      }
      let risco = 0;
      const ordemHumano = ordem2;
      for (let j = 0; j < ordemHumano.length; j++) {
        let casaHumano = ordemHumano[j];
        if (snapshot[casaHumano] > 1)
          risco -= snapshot[casaHumano];
      }
      if (ataque.includes(ultimaCasa)) ataqueBonus += 1;

      // Checagem de repetição
      let assinatura = estadoTabuleiroAssinatura(snapshot, 2); // próximo jogador será o 2
      let repetido = historicoEstadosIA.includes(assinatura);

      // AUMENTE O PESO DAS CAPTURAS E JOGADAS BOAS
      let score = capt * mult.capt + ataqueBonus * mult.ataque + risco * mult.risco;
      if (repetido) score -= 1000; // penalidade forte para repetição

      jogadasPontuadas.push({ idx, score, repetido });

      if (score > maxScore) {
        melhores = [idx];
        maxScore = score;
      } else if (score === maxScore) {
        melhores.push(idx);
      }
    }
  } else {
    // Fase 2: simula TODAS as jogadas possíveis do humano após cada jogada do PC
    let jogadasPossiveis = [];
    for (let idx of ordem) {
      if (casas[idx] !== 1) continue;
      let snapshot = casas.slice();
      let pos = ordem.indexOf(idx);
      let destino = ordem[(pos + 1) % 12];
      if (snapshot[destino] === 1) continue;
      snapshot[idx] = 0;
      snapshot[destino] = 1;

      // Calcula quantas peças serão capturadas PELO HUMANO na resposta, após esta jogada
      let maxCapturaHumano = 0;
      let minCapturaHumano = Infinity;
      let jogadaSegura = true;
      let ordemHumano = ordem2;
      let ataqueHumano = ataque2;
      // Para cada jogada possível do humano após esta jogada
      for (let jdx of ordemHumano) {
        if (snapshot[jdx] !== 1) continue;
        let sh = snapshot.slice();
        let pos2 = ordemHumano.indexOf(jdx);
        let destino2 = ordemHumano[(pos2 + 1) % 12];
        if (sh[destino2] === 1) continue;
        sh[jdx] = 0;
        sh[destino2] = 1;
        let captHumano = 0;
        if (ataqueHumano.includes(destino2)) {
          const dados = mapaAtaque[destino2];
          const a = dados.adv_ataque, d = dados.adv_defesa;
          if (sh[a] > 0) {
            captHumano += sh[a];
            if (sh[d] > 0) captHumano += sh[d];
          }
        }
        maxCapturaHumano = Math.max(maxCapturaHumano, captHumano);
        minCapturaHumano = Math.min(minCapturaHumano, captHumano);
        if (captHumano > 0) jogadaSegura = false;
      }
      // Score: menor capturado pelo humano é melhor, se possível impede captura
      let score = (jogadaSegura ? mult.fase2 : -maxCapturaHumano * mult.fase2 - minCapturaHumano * (mult.fase2/4)); score += captPC * mult.fase2Capt;
      // Bonus: se capturar peças do humano, melhor ainda
      let captPC = 0;
      if (ataque.includes(destino)) {
        const dados = mapaAtaque[destino];
        const a = dados.adv_ataque, d = dados.adv_defesa;
        if (snapshot[a] > 0) {
          captPC += snapshot[a];
          if (snapshot[d] > 0) captPC += snapshot[d];
        }
      }
      score += captPC * 300;
      jogadasPossiveis.push({idx, score, jogadaSegura, captPC});
    }
    // Prioriza jogadas seguras, depois maior captura do PC, depois menor captura do humano
    jogadasPossiveis.sort((a, b) =>
      b.score - a.score ||
      b.captPC - a.captPC ||
      (a.jogadaSegura ? -1 : 1)
    );
    if (jogadasPossiveis.length > 0) {
      maxScore = jogadasPossiveis[0].score;
      melhores = jogadasPossiveis.filter(j => j.score === maxScore).map(j => j.idx);
    }
  }

  // Evita repetir a última jogada
  let ultimaJogada = historicoEstadosIA.length > 0 ? historicoEstadosIA[historicoEstadosIA.length - 1] : null;
  let jogadasNaoRepetidas = melhores.filter(idx => {
    // Não repetir a última jogada feita
    return idx !== window.__ultimaJogadaComputador;
  });

  let opcoes = jogadasNaoRepetidas.length > 0 ? jogadasNaoRepetidas : melhores;

  // Sorteia entre as melhores opções
  const escolhaIdx = opcoes[Math.floor(Math.random() * opcoes.length)];

  window.__ultimaJogadaComputador = escolhaIdx; // Salva para evitar repetição

  // Salva o estado atual no histórico
  historicoEstadosIA.push(estadoAtual);

  setTimeout(() => {
    jogarComputador(escolhaIdx);
  }, 300);
}

    function jogarComputador(index) {
      if (animating || jogoFinalizado || jogadorAtual !== 1) return;
      ultimosMovimentos = [];
      ultimasCapturas = [];
      ultimaCasaFinal = null;

      const ordem = ordem1;
      const ataque = ataque1;
      const fase2 = emFase2(1);

      let pos = ordem.indexOf(index);
      if (pos === -1) return;

      if (!fase2) {
        if (casas[index] <= 1 && ordem.some(i => casas[i] > 1)) return;
        let sementes = casas[index];
        casas[index] = 0;
        let movimentos = [index];
        let capturas = [];
        let capturasPorCasa = {};
        let ultimaCasa = null;
        let capturadasEssaJogada = 0;
        let tipoCaptura = "";

        while (sementes > 0) {
          pos = (pos + 1) % 12;
          casas[ordem[pos]]++;
          movimentos.push(ordem[pos]);
          sementes--;
        }
        ultimaCasa = ordem[pos];

        while (casas[ultimaCasa] > 1) {
          sementes = casas[ultimaCasa];
          casas[ultimaCasa] = 0;
          movimentos.push(ultimaCasa);
          while (sementes > 0) {
            pos = (pos + 1) % 12;
            casas[ordem[pos]]++;
            movimentos.push(ordem[pos]);
            sementes--;
          }
          ultimaCasa = ordem[pos];
        }

        if (ataque.includes(ultimaCasa)) {
          const dados = mapaAtaque[ultimaCasa];
          const a = dados.adv_ataque;
          const d = dados.adv_defesa;
          if (casas[a] > 0) {
            capturadasEssaJogada += casas[a];
            capturas.push(a);
            capturasPorCasa[a] = casas[a];
            casas[a] = 0;
            if (casas[d] > 0) {
              capturadasEssaJogada += casas[d];
              capturas.push(d);
              capturasPorCasa[d] = casas[d];
              casas[d] = 0;
              tipoCaptura = "dupla";
            } else {
              tipoCaptura = "simples";
            }
          }
        }

        capturadas1 += capturadasEssaJogada;

        animarMovimento(movimentos, capturas, ultimaCasa, () => {
          atualizarTurnoBar();
          atualizarPlacar();
          if (checarFimDeJogo()) return;
          trocarJogador();
          renderTabuleiro();
        }, tipoCaptura, capturasPorCasa);
      } else {
        if (casas[index] !== 1) return;

        casas[index] = 0;
        let movimentos = [index];
        pos = (pos + 1) % 12;
        const destino = ordem[pos];

        if (casas[destino] === 1) {
          casas[index] = 1;
          return;
        }

        casas[destino] = 1;
        movimentos.push(destino);

        let capturas = [];
        let capturasPorCasa = {};
        let capturadasEssaJogada = 0;
        let tipoCaptura = "";

        if (ataque.includes(destino)) {
          const dados = mapaAtaque[destino];
          const a = dados.adv_ataque;
          const d = dados.adv_defesa;
          if (casas[a] > 0) {
            capturadasEssaJogada += casas[a];
            capturas.push(a);
            capturasPorCasa[a] = casas[a];
            casas[a] = 0;
            if (casas[d] > 0) {
              capturadasEssaJogada += casas[d];
              capturas.push(d);
              capturasPorCasa[d] = casas[d];
              casas[d] = 0;
              tipoCaptura = "dupla";
            } else {
              tipoCaptura = "simples";
            }
          }
        }

        capturadas1 += capturadasEssaJogada;

        animarMovimento(movimentos, capturas, destino, () => {
          atualizarTurnoBar();
          atualizarPlacar();
          if (checarFimDeJogo()) return;
          trocarJogador();
          renderTabuleiro();
        }, tipoCaptura, capturasPorCasa);
      }
    }

    function checarFimDeJogo() {
      const total1 = ordem1.reduce((s, idx) => s + casas[idx], 0);
      const total2 = ordem2.reduce((s, idx) => s + casas[idx], 0);

      let vencedor = null;
      if (total1 === 0 && total2 > 0) vencedor = 2;
      if (total2 === 0 && total1 > 0) vencedor = 1;

      if (vencedor) {
        jogoFinalizado = true;
        renderTabuleiro();
        atualizarTurnoBar();
        atualizarPlacar();
        playSound('som-vitoria');
        exibirVencedor(vencedor);
        return true;
      }
      return false;
    }

    function exibirVencedor(vencedor) {
      let msg = "";
      let el = document.getElementById("vinheta-vencedor");
      let msgEl = el.querySelector('.vinheta-msg');
      el.classList.remove("v1", "v2");
      let trofeuSVG = `<svg class="vinheta-trofeu" viewBox="0 0 64 64" fill="none"><ellipse cx="32" cy="60" rx="18" ry="4" fill="#fff" opacity="0.18"/><path d="M16 8h32v8a16 16 0 0 1-32 0V8z" fill="#FFD700" stroke="#fff" stroke-width="2"/><path d="M12 8v8c0 11 8 20 20 20s20-9 20-20V8" stroke="#FFD700" stroke-width="3" fill="none"/><circle cx="32" cy="8" r="6" fill="#FFD700" stroke="#fff" stroke-width="2"/></svg>`;
      if (vencedor === 1) {
        msg = jogandoContraComputador
          ? "O Computador venceu!"
          : "Jogador 1 venceu!";
        el.classList.add("v1");
      } else if (vencedor === 2) {
        msg = jogandoContraComputador
          ? "Você venceu!"
          : "Jogador 2 venceu!";
        el.classList.add("v2");
      }
      msgEl.innerHTML = trofeuSVG + `<div>${msg}</div>`;
      el.style.display = "flex";
      clearTimeout(animVencedorTimeout);
      animVencedorTimeout = setTimeout(() => {
        el.style.display = "none";
        reiniciarJogo(); 
      }, 5000);
    }

    function atualizarPlacar() {
      document.getElementById("placar").textContent = `${capturadas1} | ${capturadas2}`;
      document.getElementById("placar-invertido").textContent = `${capturadas1} | ${capturadas2}`;
    }

    function atualizarTurnoBar() {
      const ind = document.getElementById("turnindicator");
      if (!ind) return;
      ind.style.left = jogadorAtual === 1 ? "0%" : "50%";
      ind.style.width = "50%";
      ind.style.background = jogadorAtual === 1 ? "var(--player1-light)" : "var(--player2-light)";
      ind.style.opacity = "0.7";
    }

    function trocarJogador() {
      jogadorAtual = jogadorAtual === 1 ? 2 : 1;
    }

    function reiniciarJogo() {
      playSound('som-movimento');
      const main = document.getElementById('main-content');
      main.classList.add('reiniciando');
      setTimeout(() => {
      inicializarJogo();
      main.classList.remove('reiniciando');
    // Se for modo computador e for a vez dele, faz ele jogar
      if (modoJogo === 2 && jogadorAtual === 1) {
      setTimeout(jogadaComputador, 700);
      }
      }, 700); // tempo igual ao da animação
    }
    function voltarEscolherModo() {
      playSound('som-movimento'); // efeito ao voltar para escolha de modo
      mostrarEscolhaModo();
    }

    // Estratégia MUITO difícil para o computador na fase 2: evita completamente dar peças, se possível
    function jogadaComputador() {
      if (jogoFinalizado || jogadorAtual !== 1 || animating) return;

      // Limite o histórico para os últimos 10 estados
      if (historicoEstadosIA.length > 10) historicoEstadosIA.shift();

      const estadoAtual = estadoTabuleiroAssinatura(casas, jogadorAtual);

      const ordem = ordem1;
      const ataque = ataque1;
      const fase2 = emFase2(1);

      let melhores = [];
      let maxScore = -Infinity;

      if (!fase2) {
        for (let idx of ordem) {
          if (casas[idx] <= 1 && ordem.some(i => casas[i] > 1)) continue;
          let snapshot = casas.slice();
          let movimentos = [idx];
          let pos = ordem.indexOf(idx);
          let sementes = snapshot[idx];
          if (sementes < 1) continue;
          snapshot[idx] = 0;
          while (sementes > 0) {
            pos = (pos + 1) % 12;
            snapshot[ordem[pos]]++;
            movimentos.push(ordem[pos]);
            sementes--;
          }
          let ultimaCasa = ordem[pos];
          while (snapshot[ultimaCasa] > 1) {
            sementes = snapshot[ultimaCasa];
            snapshot[ultimaCasa] = 0;
            movimentos.push(ultimaCasa);
            while (sementes > 0) {
              pos = (pos + 1) % 12;
              snapshot[ordem[pos]]++;
              movimentos.push(ordem[pos]);
              sementes--;
            }
            ultimaCasa = ordem[pos];
          }
          let capt = 0;
          let ataqueBonus = 0;
          if (ataque.includes(ultimaCasa)) {
            const dados = mapaAtaque[ultimaCasa];
            const a = dados.adv_ataque, d = dados.adv_defesa;
            if (snapshot[a] > 0) {
              capt += snapshot[a];
              if (snapshot[d] > 0) capt += snapshot[d];
              ataqueBonus += 2;
            }
          }
          let risco = 0;
          const ordemHumano = ordem2;
          for (let j = 0; j < ordemHumano.length; j++) {
            let casaHumano = ordemHumano[j];
            if (snapshot[casaHumano] > 1)
              risco -= snapshot[casaHumano];
          }
          if (ataque.includes(ultimaCasa)) ataqueBonus += 1;

          // Checagem de repetição
          let assinatura = estadoTabuleiroAssinatura(snapshot, 2); // próximo jogador será o 2
          let repetido = historicoEstadosIA.includes(assinatura);
          let score = capt * 100 + ataqueBonus * 7 + risco;
          if (repetido) score -= 1000; // penalidade forte para repetição

          if (score > maxScore) {
            melhores = [idx];
            maxScore = score;
          } else if (score === maxScore) {
            melhores.push(idx);
          }
        }
      } else {
        // Fase 2: simula TODAS as jogadas possíveis do humano após cada jogada do PC
        let jogadasPossiveis = [];
        for (let idx of ordem) {
          if (casas[idx] !== 1) continue;
          let snapshot = casas.slice();
          let pos = ordem.indexOf(idx);
          let destino = ordem[(pos + 1) % 12];
          if (snapshot[destino] === 1) continue;
          snapshot[idx] = 0;
          snapshot[destino] = 1;

          // Calcula quantas peças serão capturadas PELO HUMANO na resposta, após esta jogada
          let maxCapturaHumano = 0;
          let minCapturaHumano = Infinity;
          let jogadaSegura = true;
          let ordemHumano = ordem2;
          let ataqueHumano = ataque2;
          // Para cada jogada possível do humano após esta jogada
          for (let jdx of ordemHumano) {
            if (snapshot[jdx] !== 1) continue;
            let sh = snapshot.slice();
            let pos2 = ordemHumano.indexOf(jdx);
            let destino2 = ordemHumano[(pos2 + 1) % 12];
            if (sh[destino2] === 1) continue;
            sh[jdx] = 0;
            sh[destino2] = 1;
            let captHumano = 0;
            if (ataqueHumano.includes(destino2)) {
              const dados = mapaAtaque[destino2];
              const a = dados.adv_ataque, d = dados.adv_defesa;
              if (sh[a] > 0) {
                captHumano += sh[a];
                if (sh[d] > 0) captHumano += sh[d];
              }
            }
            maxCapturaHumano = Math.max(maxCapturaHumano, captHumano);
            minCapturaHumano = Math.min(minCapturaHumano, captHumano);
            if (captHumano > 0) jogadaSegura = false;
          }
          // Score: menor capturado pelo humano é melhor, se possível impede captura
          let score = (jogadaSegura ? 2000 : -maxCapturaHumano * 200 - minCapturaHumano * 70);
          // Bonus: se capturar peças do humano, melhor ainda
          let captPC = 0;
          if (ataque.includes(destino)) {
            const dados = mapaAtaque[destino];
            const a = dados.adv_ataque, d = dados.adv_defesa;
            if (snapshot[a] > 0) {
              captPC += snapshot[a];
              if (snapshot[d] > 0) captPC += snapshot[d];
            }
          }
          score += captPC * 100;
          jogadasPossiveis.push({idx, score, jogadaSegura, captPC});
        }
        // Prioriza jogadas seguras, depois maior captura do PC, depois menor captura do humano
        jogadasPossiveis.sort((a, b) =>
          b.score - a.score ||
          b.captPC - a.captPC ||
          (a.jogadaSegura ? -1 : 1)
        );
        if (jogadasPossiveis.length > 0) {
          melhores = [jogadasPossiveis[0].idx];
        }
      }

      // Salva o estado atual no histórico
      historicoEstadosIA.push(estadoAtual);

      // Evita repetir a última jogada do computador
      let ultimaJogada = window.__ultimaJogadaComputador;
      let jogadasNaoRepetidas = melhores.filter(idx => idx !== ultimaJogada);
      let opcoes = jogadasNaoRepetidas.length > 0 ? jogadasNaoRepetidas : melhores;

      // Sorteia entre as melhores opções
      const escolhaIdx = opcoes[Math.floor(Math.random() * opcoes.length)];
      window.__ultimaJogadaComputador = escolhaIdx; // Salva para evitar repetição

      setTimeout(() => {
        jogarComputador(escolhaIdx);
      }, 300);
    }

    mostrarEscolhaModo();

function abrirTutorial() {
  document.getElementById('modal-full-text').innerHTML = `
    <h2 style="text-align:center;color:#1976d2;">Como Jogar Ntxuva</h2>
    <p>
      <b>Objetivo:</b><br>
      Capturar mais sementes que o adversário até que um dos lados fique sem sementes.<br><br>
      
      <b>Visão Geral do Tabuleiro:</b><br>
      • O tabuleiro tem 24 casas, 12 para cada jogador.<br>
      • As casas do Jogador 1 (azul) ficam na parte inferior, as do Jogador 2 (verde) na superior.<br>
      • As 6 casas centrais de cada lado são chamadas de <b>casas de ataque</b>.<br>
      • Cada casa começa com 2 sementes.<br><br>
      
      <b>Como Jogar:</b><br>
      1. Em sua vez, escolha uma casa do seu lado que tenha mais de 1 semente.<br>
      2. Pegue todas as sementes dessa casa.<br>
      3. Distribua-as, uma por uma, nas casas seguintes do seu lado, no sentido anti-horário.<br>
      4. Se a última semente cair em uma casa que já tinha sementes, pegue todas e continue distribuindo.<br>
      5. Se a última semente cair em uma casa de ataque, você pode capturar sementes do adversário:<br>
      &nbsp;&nbsp;• Olhe a casa de ataque correspondente do adversário (em frente à sua).<br>
      &nbsp;&nbsp;• Se ela tiver sementes, capture todas.<br>
      &nbsp;&nbsp;• Se a casa de defesa atrás dela também tiver sementes, capture essas também.<br>
      6. Após capturar, sua vez termina.<br>
      7. Se não houver captura, o turno passa para o outro jogador.<br><br>
      
      <b>Fase 2 (Final do Jogo):</b><br>
      • Quando todas as casas do seu lado têm no máximo 1 semente, entra na Fase 2.<br>
      • Agora, só pode mover casas com exatamente 1 semente.<br>
      • Não pode mover para uma casa que já tenha 1 semente.<br>
      • As regras de captura continuam valendo.<br><br>
      
      <b>Regras Especiais:</b><br>
      • Não pode jogar casas do adversário.<br>
      • Se não houver jogada possível, o turno passa automaticamente.<br>
      • O jogo termina quando um dos lados fica sem sementes.<br>
      • Vence quem capturar mais sementes.<br><br>
      
      <b>Dicas Estratégicas:</b><br>
      • Planeje para capturar o máximo possível e evitar dar capturas fáceis ao adversário.<br>
      • Fique atento às casas de ataque e defesa.<br>
      • Tente prever os próximos movimentos do oponente.<br>
      • Na Fase 2, evite deixar casas vulneráveis à captura.<br><br>
      
      <b>Controles:</b><br>
      • Clique em uma casa do seu lado para jogar.<br>
      • Use o botão de configurações (<span style="color:#1976d2;">⚙️</span>) para acessar tutorial, créditos e suporte.<br>
      • Reinicie o jogo ou troque de modo pelos ícones no topo direito.<br><br>
      
      <b>Sobre o Jogo:</b><br>
      • Inspirado no tradicional jogo africano Ntxuva/Mancala.<br>
      • Modo contra amigo ou computador (IA).<br>
      • IA com estratégia avançada na Fase 2.<br>
      <br>
      Bom jogo!<br>
      <span style="font-size:0.95em;color:#888;">Dúvidas? Acesse o suporte nas configurações.</span>
    </p>
  `;
  abrirModalFull();
}

function abrirCreditos() {
  document.getElementById('modal-full-text').innerHTML = `
    <h2 style="text-align:center;color:#1976d2;">Créditos</h2>
    <p>
      <b>Desenvolvimento e Programação:</b><br>
      Spoiler Tec<br>
      <span style="font-size:0.95em;color:#555;">
        <i>Equipe responsável pela implementação do código, lógica do jogo, interface digital e integração de IA.</i>
      </span>
      <br><br>
      <b>Design Visual e UX:</b><br>
      Equipe Ntxuva Digital<br>
      <span style="font-size:0.95em;color:#555;">
        <i>Criação dos elementos gráficos, animações, responsividade e experiência do usuário.</i>
      </span>
      <br><br>
      <b>Pesquisa e Consultoria Cultural:</b><br>
      Mestres de Ntxuva de Moçambique<br>
      <span style="font-size:0.95em;color:#555;">
        <i>Colaboração para garantir fidelidade às regras tradicionais e respeito à cultura do jogo.</i>
      </span>
      <br><br>
      <b>Testes e Feedback:</b><br>
      Comunidade Spoiler Tec, jogadores voluntários e educadores<br>
      <span style="font-size:0.95em;color:#555;">
        <i>Testes de usabilidade, sugestões de melhorias e validação das regras.</i>
      </span>
      <br><br>
      <b>Recursos Sonoros:</b><br>
      <span style="font-size:0.95em;color:#555;">
        Efeitos de áudio de <a href="#" target="_blank" style="color:#1976d2;">Pixabay</a> e trilhas livres de direitos autorais.
      </span>
      <br><br>
      <b>Agradecimentos Especiais:</b><br>
      A todos que apoiaram, divulgaram e incentivaram o projeto.<br>
      <span style="font-size:0.95em;color:#888;">
        Se você contribuiu e não foi citado, entre em contato para adicionarmos seu nome!
      </span>
      <br><br>
      <b>Versão:</b> 2.3 &nbsp; | &nbsp; <b>Data:</b> Junho/2025
    </p>
  `;
  abrirModalFull();
}


// Substitua a função abrirFormDoacao(tipo) por esta versão melhorada:

function abrirDoacao(e) {
  e && e.stopPropagation && e.stopPropagation();
  document.getElementById('modal-full-text').innerHTML = `
    <h2 style="text-align:center;color:#f9a826;">Doação</h2>
    <div style="text-align:center;margin-bottom:18px;">
      <button onclick="abrirFormDoacao('mpesa')" style="background:linear-gradient(90deg,#3c4e9b 0%,#1976d2 100%);color:#fff;font-weight:800;padding:10px 0;width:140px;border:none;border-radius:8px;cursor:pointer;font-size:1.13em;box-shadow:0 2px 8px #aac2ee44;margin:0 8px 10px 0;">M-Pesa</button>
      <button onclick="abrirFormDoacao('emola')" style="background:linear-gradient(90deg,#319e72 0%,#1de9b6 100%);color:#fff;font-weight:800;padding:10px 0;width:140px;border:none;border-radius:8px;cursor:pointer;font-size:1.13em;box-shadow:0 2px 8px #aac2ee44;">E-Mola</button>
    </div>
    <div id="form-doacao-area"></div>
    <div style="color:#888;font-size:0.95em;margin-top:12px;">Sua doação apoia o projeto Ntxuva Digital!</div>
  `;
  abrirModalFull();
}

function abrirFormDoacao(tipo) {
  let nome = tipo === 'mpesa' ? 'M-Pesa' : 'E-Mola';
  let cor = tipo === 'mpesa' ? '#3c4e9b' : '#319e72';
  let grad = tipo === 'mpesa'
    ? 'linear-gradient(90deg, #3c4e9b 0%, #1976d2 100%)'
    : 'linear-gradient(90deg, #319e72 0%, #1de9b6 100%)';
  document.getElementById('form-doacao-area').innerHTML = `
    <form id="form-doacao-${tipo}" style="
      margin-top:18px;
      text-align:center;
      background:#fff;
      border-radius:16px;
      box-shadow:0 2px 18px #aac2ee33;
      border:2px solid #aac2ee;
      padding:22px 18px 18px 18px;
      max-width:320px;
      margin-left:auto;
      margin-right:auto;
      display:inline-block;
    ">
      <label style="font-weight:700;color:${cor};font-size:1.08em;">
        Seu número (${nome}):<br>
        <input type="tel" name="numero" required pattern="\\d{8,12}" placeholder="8xxxxxxxx"
          style="padding:9px 14px;border-radius:8px;border:2px solid #aac2ee;margin-top:7px;width:170px;font-size:1.08em;outline:none;transition:border .15s;"
          onfocus="this.style.borderColor='${cor}'" onblur="this.style.borderColor='#aac2ee'">
      </label><br>
      <label style="font-weight:700;color:${cor};font-size:1.08em;margin-top:14px;display:inline-block;">
        Valor (MT):<br>
        <input type="number" name="valor" required min="10" placeholder="Ex: 50"
          style="padding:9px 14px;border-radius:8px;border:2px solid #aac2ee;margin-top:7px;width:110px;font-size:1.08em;outline:none;transition:border .15s;"
          onfocus="this.style.borderColor='${cor}'" onblur="this.style.borderColor='#aac2ee'">
      </label><br>
      <button type="submit"
        style="margin-top:18px;background:${grad};color:#fff;font-weight:800;padding:10px 0;width:100%;border:none;border-radius:8px;cursor:pointer;font-size:1.13em;box-shadow:0 2px 8px #aac2ee44;letter-spacing:1px;transition:filter .13s;">
        Doar via ${nome}
      </button>
    </form>
    <div id="doacao-status" style="margin-top:14px;font-size:1.08em;font-weight:600;"></div>
  `;
  document.getElementById(`form-doacao-${tipo}`).onsubmit = function(ev) {
    ev.preventDefault();
    const numero = this.numero.value;
    const valor = this.valor.value;
    document.getElementById('doacao-status').innerHTML = "<span style='color:#1976d2;'>Processando...</span>";
    fetch('https://api.mozpayment.co.mz/v1/payment', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        provider: tipo,
        phone: numero,
        amount: valor,
        description: "Doação Ntxuva Digital"
      })
    })
    .then(r => r.json())
    .then(resp => {
      if (resp.success) {
        document.getElementById('doacao-status').innerHTML = '<span style="color:#319e72;">Doação enviada! Obrigado 💛</span>';
      } else {
        document.getElementById('doacao-status').innerHTML = '<span style="color:#e53935;">Erro: ' + (resp.message || 'Não foi possível processar.') + '</span>';
      }
    })
    .catch(() => {
      document.getElementById('doacao-status').innerHTML = '<span style="color:#e53935;">Erro ao conectar. Tente novamente.</span>';
    });
  };
}


function abrirSuporte() {
  document.getElementById('modal-full-text').innerHTML = `
    <h2 style="text-align:center;color:#1976d2;">Suporte</h2>
    <p>
      Precisa de ajuda ou quer enviar sugestões?<br><br>
      <b>Email:</b><br> <span style="font-size:1.15em;color:#1976d2;">geral@jogontxuva.com</span><br>
      <br>
      Responderemos o mais rápido possível!
    </p>
  `;
  abrirModalFull();
}

function abrirModalFull() {
  document.getElementById('modal-full').style.display = 'flex';
  document.body.style.overflow = 'hidden';
}
function fecharModalFull() {
  document.getElementById('modal-full').style.display = 'none';
  document.body.style.overflow = '';
}
  </script>

</!doctype>
