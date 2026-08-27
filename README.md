<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vendas Online em 1 Hora — Guia Interativo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700;800&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #0A1216;
    --bg-2: #0E1A20;
    --surface: #101C22;
    --surface-2: #142530;
    --border: rgba(146, 197, 191, 0.14);
    --green: #34D1A3;
    --green-dim: #1F7A63;
    --blue: #4FB4E8;
    --blue-dim: #2C6C8C;
    --text: #E7F1EF;
    --text-muted: #8FA6A3;
    --text-faint: #5E7472;
  }

  *{ box-sizing: border-box; }
  html{ scroll-behavior: smooth; }

  body{
    margin:0;
    background: radial-gradient(1200px 800px at 15% -10%, #103227 0%, transparent 55%),
                radial-gradient(1000px 700px at 100% 10%, #0d2b3a 0%, transparent 50%),
                var(--bg);
    color: var(--text);
    font-family: 'Inter', sans-serif;
    line-height: 1.65;
    overflow-x: hidden;
  }

  h1,h2,h3{ font-family:'Sora', sans-serif; margin:0; }
  .eyebrow{
    font-family:'JetBrains Mono', monospace;
    letter-spacing:.14em;
    text-transform:uppercase;
    font-size:.72rem;
    color: var(--green);
    display:flex; align-items:center; gap:10px;
  }
  .eyebrow::before{
    content:"";
    width:22px; height:1px;
    background: linear-gradient(90deg,var(--green),var(--blue));
  }

  a{ color:inherit; }

  /* ---------- progress bar ---------- */
  #progress-wrap{
    position:fixed; top:0; left:0; right:0; height:3px; z-index:100;
    background: rgba(255,255,255,0.04);
  }
  #progress-bar{
    height:100%; width:0%;
    background: linear-gradient(90deg, var(--green), var(--blue));
    box-shadow: 0 0 12px rgba(79,180,232,.6);
    transition: width .08s linear;
  }

  /* ---------- topbar ---------- */
  header.topbar{
    position:fixed; top:3px; left:0; right:0; z-index:90;
    display:flex; align-items:center; justify-content:space-between;
    padding: 14px 5vw;
    backdrop-filter: blur(14px);
    background: rgba(10,18,22,0.55);
    border-bottom: 1px solid var(--border);
  }
  .brand{ display:flex; align-items:center; gap:10px; font-family:'Sora'; font-weight:700; font-size:.95rem; }
  .brand-mark{
    width:26px; height:26px; border-radius:50%;
    background: conic-gradient(var(--green), var(--blue), var(--green));
    display:flex; align-items:center; justify-content:center;
    box-shadow: 0 0 14px rgba(52,209,163,.35);
  }
  .brand-mark::after{
    content:""; width:11px; height:11px; border-radius:50%; background: var(--bg);
  }
  nav.links{ display:flex; gap:26px; font-size:.82rem; color: var(--text-muted); }
  nav.links a{ text-decoration:none; position:relative; padding-bottom:4px; transition:color .25s; }
  nav.links a::after{
    content:""; position:absolute; left:0; bottom:0; height:1px; width:0%;
    background: linear-gradient(90deg,var(--green),var(--blue));
    transition: width .3s;
  }
  nav.links a:hover{ color: var(--text); }
  nav.links a:hover::after{ width:100%; }
  @media (max-width: 820px){ nav.links{ display:none; } }

  /* ---------- hero ---------- */
  .hero{
    min-height: 100vh;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; position:relative;
    padding: 140px 6vw 80px;
  }
  .blob{
    position:absolute; border-radius:50%; filter: blur(90px); opacity:.35; z-index:0;
    animation: float 14s ease-in-out infinite;
  }
  .blob.b1{ width:420px; height:420px; background:var(--green-dim); top:-100px; left:-120px; }
  .blob.b2{ width:380px; height:380px; background:var(--blue-dim); bottom:-140px; right:-100px; animation-delay:-6s; }
  @keyframes float{
    0%,100%{ transform: translate(0,0) scale(1); }
    50%{ transform: translate(30px,-20px) scale(1.08); }
  }

  .hero-content{ position:relative; z-index:2; max-width:820px; }
  .hero h1{
    font-size: clamp(2.4rem, 5.4vw, 4.4rem);
    font-weight:800; letter-spacing:-.02em; line-height:1.05;
    margin: 22px 0 18px;
    background: linear-gradient(120deg, #F2FBF9 30%, var(--green) 70%, var(--blue) 100%);
    -webkit-background-clip:text; background-clip:text; color:transparent;
  }
  .hero p.sub{ font-size:1.15rem; color: var(--text-muted); max-width:600px; margin:0 auto; }

  .ring-wrap{ position:relative; width:260px; height:260px; margin: 46px auto 8px; z-index:2; }
  .ring-wrap svg{ width:100%; height:100%; transform: rotate(-90deg); }
  .ring-bg{ fill:none; stroke: rgba(255,255,255,0.06); stroke-width: 6; }
  .ring-fg{
    fill:none; stroke-width:6; stroke-linecap:round;
    stroke: url(#ringGrad);
    stroke-dasharray: 691;
    stroke-dashoffset: 691;
    transition: stroke-dashoffset 1.4s cubic-bezier(.2,.8,.2,1);
  }
  .ring-center{
    position:absolute; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center;
  }
  .ring-center .time{ font-family:'JetBrains Mono'; font-size:2.1rem; font-weight:600; letter-spacing:-.02em; }
  .ring-center .label{ font-size:.7rem; text-transform:uppercase; letter-spacing:.12em; color:var(--text-muted); margin-top:4px; }

  .scroll-cue{
    margin-top:34px; font-size:.75rem; color: var(--text-faint);
    display:flex; flex-direction:column; align-items:center; gap:8px;
    letter-spacing:.1em; text-transform:uppercase;
  }
  .scroll-cue span{
    width:1px; height:34px;
    background: linear-gradient(var(--green), transparent);
    animation: pulse 1.8s ease-in-out infinite;
  }
  @keyframes pulse{ 0%,100%{ opacity:.2 } 50%{ opacity:1 } }

  /* ---------- sections ---------- */
  main{ position:relative; z-index:1; max-width: 880px; margin: 0 auto; padding: 40px 6vw 140px; }

  section.chapter{ padding: 90px 0; border-top: 1px solid var(--border); }
  section.chapter:first-of-type{ border-top:none; }

  .reveal{ opacity:0; transform: translateY(28px); transition: opacity .7s ease, transform .7s ease; }
  .reveal.in{ opacity:1; transform:none; }

  .chapter-head{ display:flex; gap:22px; align-items:flex-start; margin-bottom: 26px; }
  .chapter-num{
    font-family:'JetBrains Mono'; font-size:.85rem; color: var(--bg);
    background: linear-gradient(135deg, var(--green), var(--blue));
    width:44px; height:44px; border-radius:12px;
    display:flex; align-items:center; justify-content:center;
    flex-shrink:0; font-weight:700;
    box-shadow: 0 6px 20px rgba(52,209,163,.18);
  }
  .chapter-title h2{ font-size: clamp(1.5rem, 3vw, 2rem); font-weight:700; }
  .chapter-title .meta{ margin-top:6px; font-size:.8rem; color: var(--text-faint); font-family:'JetBrains Mono'; }

  p{ color: var(--text); }
  p.lead{ font-size:1.05rem; color: var(--text); }
  .muted{ color: var(--text-muted); }

  h3.sub-h{ font-size:1.05rem; margin: 30px 0 12px; color: var(--text); font-weight:600; }

  ul.list, ol.list{ padding-left:0; margin: 14px 0; list-style:none; }
  ul.list li, ol.list li{
    position:relative; padding-left:30px; margin-bottom:12px; color: var(--text);
  }
  ul.list li::before{
    content:"›"; position:absolute; left:0; top:0; color: var(--green); font-weight:700; font-size:1.1rem;
  }
  ol.list{ counter-reset: step; }
  ol.list li::before{
    counter-increment: step; content: counter(step);
    position:absolute; left:0; top:0; width:20px; height:20px; border-radius:6px;
    background: rgba(52,209,163,.12); color: var(--green);
    font-size:.7rem; display:flex; align-items:center; justify-content:center;
    font-family:'JetBrains Mono';
  }

  /* tip box */
  .tip{
    margin: 26px 0; padding: 18px 20px; border-radius: 14px;
    background: linear-gradient(135deg, rgba(52,209,163,.08), rgba(79,180,232,.06));
    border: 1px solid var(--border);
    display:flex; gap:14px; align-items:flex-start;
    position:relative; overflow:hidden;
  }
  .tip::before{
    content:""; position:absolute; left:0; top:0; bottom:0; width:3px;
    background: linear-gradient(var(--green), var(--blue));
  }
  .tip .icon{ font-size:1.2rem; flex-shrink:0; }
  .tip strong{ color: var(--green); }

  .quote-msg{
    margin: 20px 0; padding:16px 20px; border-radius:12px;
    background: var(--surface-2); border:1px dashed var(--border);
    font-style:italic; color: var(--text-muted); font-size:.95rem;
  }

  /* checklist */
  .checklist-box{
    margin-top: 20px; border-radius:18px; border:1px solid var(--border);
    background: var(--surface); padding: 6px; overflow:hidden;
  }
  .check-item{
    display:flex; align-items:center; gap:16px;
    padding: 16px 18px; border-radius:12px; cursor:pointer;
    transition: background .2s;
  }
  .check-item:hover{ background: rgba(255,255,255,.03); }
  .box{
    width:22px; height:22px; border-radius:7px; flex-shrink:0;
    border: 1.5px solid var(--text-faint);
    display:flex; align-items:center; justify-content:center;
    transition: all .25s;
  }
  .check-item.done .box{
    background: linear-gradient(135deg,var(--green),var(--blue));
    border-color: transparent;
  }
  .box svg{ width:13px; height:13px; opacity:0; transform: scale(.5); transition: all .2s; }
  .check-item.done .box svg{ opacity:1; transform:scale(1); }
  .check-text{ flex:1; }
  .check-text .title{ font-weight:500; transition: color .2s, opacity .2s; }
  .check-item.done .check-text .title{ color: var(--text-faint); text-decoration: line-through; opacity:.7; }
  .check-time{ font-family:'JetBrains Mono'; font-size:.72rem; color: var(--text-faint); flex-shrink:0; }

  .timer-summary{
    display:flex; align-items:center; justify-content:space-between;
    margin-top:22px; padding:18px 22px; border-radius:14px;
    background: var(--surface-2); border:1px solid var(--border);
  }
  .timer-summary .num{ font-family:'JetBrains Mono'; font-size:1.6rem; font-weight:700;
    background: linear-gradient(90deg,var(--green),var(--blue)); -webkit-background-clip:text; background-clip:text; color:transparent; }

  /* CTA end */
  .cta{
    text-align:center; padding: 90px 20px 40px;
  }
  .cta h2{ font-size: clamp(1.6rem,3.6vw,2.4rem); margin-bottom:16px; }
  .cta p{ color:var(--text-muted); max-width:520px; margin: 0 auto 30px; }
  .btn{
    display:inline-flex; align-items:center; gap:10px;
    padding: 15px 30px; border-radius: 100px; border:none; cursor:pointer;
    background: linear-gradient(135deg, var(--green), var(--blue));
    color:#062019; font-weight:700; font-size:.95rem; font-family:'Sora';
    box-shadow: 0 10px 30px rgba(52,209,163,.22);
    transition: transform .25s, box-shadow .25s;
  }
  .btn:hover{ transform: translateY(-2px); box-shadow: 0 14px 36px rgba(52,209,163,.32); }

  footer{
    text-align:center; padding: 30px; color: var(--text-faint); font-size:.78rem;
    border-top:1px solid var(--border);
  }

  @media (prefers-reduced-motion: reduce){
    *{ animation: none !important; transition: none !important; }
  }
</style>
</head>
<body>

<div id="progress-wrap"><div id="progress-bar"></div></div>

<header class="topbar">
  <div class="brand"><div class="brand-mark"></div>Vendas em 1 Hora</div>
  <nav class="links">
    <a href="#intro">Início</a>
    <a href="#cap1">Produto</a>
    <a href="#cap3">Página</a>
    <a href="#cap5">Divulgação</a>
    <a href="#checklist">Checklist</a>
  </nav>
</header>

<section class="hero">
  <div class="blob b1"></div>
  <div class="blob b2"></div>
  <div class="hero-content">
    <div class="eyebrow" style="justify-content:center">GUIA PRÁTICO · INFOPRODUTOS</div>
    <h1>Vendas online<br>em 1 hora.</h1>
    <p class="sub">Um método enxuto para lançar seu curso, mentoria ou e-book e fazer a primeira venda — hoje, com o que você já tem.</p>

    <div class="ring-wrap">
      <svg viewBox="0 0 240 240">
        <defs>
          <linearGradient id="ringGrad" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" stop-color="#34D1A3"/>
            <stop offset="100%" stop-color="#4FB4E8"/>
          </linearGradient>
        </defs>
        <circle class="ring-bg" cx="120" cy="120" r="110"/>
        <circle class="ring-fg" id="hero-ring" cx="120" cy="120" r="110"/>
      </svg>
      <div class="ring-center">
        <div class="time" id="ring-time">60:00</div>
        <div class="label">minutos para vender</div>
      </div>
    </div>

    <div class="scroll-cue"><span></span>role para começar</div>
  </div>
</section>

<main>

  <section class="chapter reveal" id="intro">
    <div class="chapter-head">
      <div class="chapter-num">00</div>
      <div class="chapter-title">
        <h2>Introdução</h2>
        <div class="meta">por que 1 hora é suficiente</div>
      </div>
    </div>
    <p class="lead">Se você acha que precisa de meses para começar a vender um infoproduto online, tenho uma notícia: precisa é de <strong>1 hora bem usada</strong>. Não da hora perfeita — da hora possível, com o que você já tem em mãos.</p>
    <p>Este guia não é sobre construir um império digital hoje. É sobre colocar uma oferta simples no ar, validar se alguém está disposto a pagar por ela, e aprender fazendo. Depois, você escala.</p>
    <p>O exemplo central é um infoproduto — curso online, mentoria gravada ou e-book. Mas o método (produto, oferta, página, pagamento, divulgação) funciona para qualquer coisa que você venda digitalmente.</p>
    <div class="tip"><div class="icon">⏱️</div><div><strong>Dica rápida:</strong> separe um cronômetro antes de começar. A pressão do tempo tira você do modo planejamento infinito e coloca no modo ação.</div></div>
  </section>

  <section class="chapter reveal" id="cap1">
    <div class="chapter-head">
      <div class="chapter-num">01</div>
      <div class="chapter-title">
        <h2>Escolhendo o que vender</h2>
        <div class="meta">5 minutos · definição do produto</div>
      </div>
    </div>
    <p class="lead">Você não precisa criar um curso do zero em 1 hora. Precisa escolher <strong>um formato</strong> que já esteja na sua cabeça, mesmo que incompleto.</p>
    <h3 class="sub-h">Opções rápidas de infoproduto</h3>
    <ul class="list">
      <li>E-book curto (10–20 páginas) sobre algo que você já sabe fazer</li>
      <li>Aula única gravada (20–40 min) resolvendo um problema específico</li>
      <li>Mentoria individual ao vivo — você vende o horário, não o conteúdo pronto</li>
      <li>Checklist ou planilha paga em PDF</li>
    </ul>
    <h3 class="sub-h">Como escolher o tema em 5 minutos</h3>
    <ol class="list">
      <li>Liste 3 perguntas que mais te fazem sobre algo que você domina</li>
      <li>Escolha a pergunta com resposta mais prática, não a mais teórica</li>
      <li>Transforme essa resposta em título: "Como [resultado] em [tempo/forma]"</li>
    </ol>
    <div class="tip"><div class="icon">🎯</div><div><strong>Dica rápida:</strong> um infoproduto pequeno e específico vende mais rápido que um curso completo e genérico.</div></div>
  </section>

  <section class="chapter reveal" id="cap2">
    <div class="chapter-head">
      <div class="chapter-num">02</div>
      <div class="chapter-title">
        <h2>Público e oferta</h2>
        <div class="meta">10 minutos · clareza antes de tudo</div>
      </div>
    </div>
    <h3 class="sub-h">Público</h3>
    <p>Não precisa de persona detalhada. Responda em uma frase: quem tem o problema que seu infoproduto resolve, e onde essa pessoa está — Instagram, WhatsApp, grupos, LinkedIn?</p>
    <h3 class="sub-h">Os 4 elementos da oferta</h3>
    <ul class="list">
      <li>O que a pessoa recebe — formato + conteúdo em uma frase</li>
      <li>Em quanto tempo ela vê resultado ou usa o material</li>
      <li>Preço — comece entre R$ 19 e R$ 97 para validar</li>
      <li>Um bônus simples e rápido de produzir</li>
    </ul>
    <div class="tip"><div class="icon">💬</div><div><strong>Dica rápida:</strong> preço baixo no início é sobre validação, não sobre lucro. Você quer saber se compram, não maximizar receita ainda.</div></div>
  </section>

  <section class="chapter reveal" id="cap3">
    <div class="chapter-head">
      <div class="chapter-num">03</div>
      <div class="chapter-title">
        <h2>A página de vendas</h2>
        <div class="meta">20 minutos · o essencial, nada além disso</div>
      </div>
    </div>
    <p>Você não precisa de um site profissional para a primeira venda. Precisa de uma página que explique o problema, a solução e o preço.</p>
    <h3 class="sub-h">Ferramentas rápidas (escolha uma)</h3>
    <ul class="list">
      <li>Uma página de link (Linktree, Bio.link) apontando para o checkout</li>
      <li>Um PDF com a oferta + link de pagamento</li>
      <li>Plataforma de infoproduto — Hotmart, Eduzz, Kiwify</li>
    </ul>
    <h3 class="sub-h">Estrutura mínima</h3>
    <ol class="list">
      <li>Título com a promessa principal</li>
      <li>3 bullets com o que a pessoa recebe</li>
      <li>Preço e botão de compra</li>
      <li>Uma frase de urgência — vagas limitadas ou preço promocional</li>
    </ol>
    <div class="tip"><div class="icon">🧩</div><div><strong>Dica rápida:</strong> se você já usa uma plataforma de infoproduto, use o template pronto dela. Não invente o layout do zero.</div></div>
  </section>

  <section class="chapter reveal" id="cap4">
    <div class="chapter-head">
      <div class="chapter-num">04</div>
      <div class="chapter-title">
        <h2>Pagamento e entrega</h2>
        <div class="meta">10 minutos · deixar simples de propósito</div>
      </div>
    </div>
    <h3 class="sub-h">Pagamento</h3>
    <p>Use uma solução que já aceite Pix e cartão sem burocracia: plataformas de infoproduto ou um link de pagamento — Mercado Pago, PagSeguro, Stripe.</p>
    <h3 class="sub-h">Entrega</h3>
    <p>Pode ser automática (link de acesso por e-mail) ou manual (você envia por WhatsApp após confirmar o pagamento). No início, entrega manual é aceitável — e até gera mais conexão com o primeiro cliente.</p>
    <div class="tip"><div class="icon">📦</div><div><strong>Dica rápida:</strong> não deixe a busca pela automação perfeita atrasar sua primeira venda. Automatize depois de validar.</div></div>
  </section>

  <section class="chapter reveal" id="cap5">
    <div class="chapter-head">
      <div class="chapter-num">05</div>
      <div class="chapter-title">
        <h2>Divulgação rápida</h2>
        <div class="meta">10 minutos · avisar quem já te conhece</div>
      </div>
    </div>
    <p>Você não precisa de tráfego pago para a primeira venda. Precisa avisar quem já te conhece.</p>
    <h3 class="sub-h">Canais para hoje</h3>
    <ul class="list">
      <li>Status do WhatsApp + mensagem direta para 10–20 contatos relevantes</li>
      <li>Stories no Instagram contando por que você criou o infoproduto</li>
      <li>Grupos de WhatsApp/Telegram onde seu público está</li>
      <li>Um post simples explicando o problema que o produto resolve</li>
    </ul>
    <div class="quote-msg">"Acabei de lançar [nome do infoproduto], que ensina [resultado] em [tempo]. Fiz baseado em [sua experiência]. Hoje está saindo por R$ [preço]. Quem quiser, me chama."</div>
    <div class="tip"><div class="icon">📣</div><div><strong>Dica rápida:</strong> divulgar para poucas pessoas certas vale mais que divulgar para muitas pessoas aleatórias no primeiro momento.</div></div>
  </section>

  <section class="chapter reveal" id="checklist">
    <div class="chapter-head">
      <div class="chapter-num">06</div>
      <div class="chapter-title">
        <h2>Checklist final</h2>
        <div class="meta">marque conforme avança</div>
      </div>
    </div>
    <p>Use este checklist cronometrado — clique em cada item para marcar como feito.</p>

    <div class="checklist-box" id="checklist-box">
      <div class="check-item" data-min="5">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Escolher o formato e o tema do infoproduto</div></div>
        <div class="check-time">5 min</div>
      </div>
      <div class="check-item" data-min="10">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Definir público e os 4 elementos da oferta</div></div>
        <div class="check-time">10 min</div>
      </div>
      <div class="check-item" data-min="20">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Criar a página de vendas — título, bullets e preço</div></div>
        <div class="check-time">20 min</div>
      </div>
      <div class="check-item" data-min="10">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Configurar pagamento (Pix/cartão) e forma de entrega</div></div>
        <div class="check-time">10 min</div>
      </div>
      <div class="check-item" data-min="5">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Escrever a mensagem de divulgação</div></div>
        <div class="check-time">5 min</div>
      </div>
      <div class="check-item" data-min="10">
        <div class="box"><svg viewBox="0 0 24 24" fill="none"><path d="M4 12l5 5L20 6" stroke="#062019" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <div class="check-text"><div class="title">Enviar para contatos e postar nos stories/grupos</div></div>
        <div class="check-time">10 min</div>
      </div>
    </div>

    <div class="timer-summary">
      <span class="muted">Tempo restante</span>
      <span class="num" id="time-left">60 min</span>
    </div>
  </section>

  <section class="chapter reveal" id="conclusao">
    <div class="chapter-head">
      <div class="chapter-num">07</div>
      <div class="chapter-title">
        <h2>Depois da primeira venda</h2>
        <div class="meta">o que vem a seguir</div>
      </div>
    </div>
    <p class="lead">A primeira venda não precisa ser perfeita — precisa acontecer. É ela que te dá dado real: preço certo, mensagem que funciona, público que responde.</p>
    <ul class="list">
      <li>Ajustar preço e oferta com base no feedback</li>
      <li>Investir um pouco em tráfego pago nos canais que já converteram</li>
      <li>Criar uma página de vendas mais robusta</li>
      <li>Automatizar entrega e cobrança</li>
    </ul>
  </section>

  <div class="cta reveal">
    <h2>Sua próxima hora livre<br>vale uma venda.</h2>
    <p>Volte ao topo, siga o checklist e coloque sua oferta no ar ainda hoje.</p>
    <a href="#intro" class="btn">Começar agora ↑</a>
  </div>

</main>

<footer>Guia prático — Vendas Online em 1 Hora</footer>

<script>
  // progress bar
  const bar = document.getElementById('progress-bar');
  function updateProgress(){
    const h = document.documentElement;
    const scrolled = (h.scrollTop) / (h.scrollHeight - h.clientHeight) * 100;
    bar.style.width = scrolled + '%';

    // hero ring + timer tied to scroll progress
    const ring = document.getElementById('hero-ring');
    const pct = Math.min(Math.max(scrolled/100, 0), 1);
    const circumference = 691;
    ring.style.strokeDashoffset = circumference - (circumference * pct);
    const minsLeft = Math.max(0, Math.round(60 * (1 - pct)));
    document.getElementById('ring-time').textContent =
      String(minsLeft).padStart(2,'0') + ':00';
  }
  document.addEventListener('scroll', updateProgress);
  updateProgress();

  // scroll reveal
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(e=>{
      if(e.isIntersecting){ e.target.classList.add('in'); }
    });
  }, { threshold: 0.15 });
  document.querySelectorAll('.reveal').forEach(el=>io.observe(el));

  // checklist interactivity
  const items = document.querySelectorAll('.check-item');
  const totalMin = Array.from(items).reduce((a,el)=>a+Number(el.dataset.min),0);
  const timeLeftEl = document.getElementById('time-left');

  function recalc(){
    let done = 0;
    items.forEach(el=>{ if(el.classList.contains('done')) done += Number(el.dataset.min); });
    timeLeftEl.textContent = (totalMin - done) + ' min';
  }
  items.forEach(el=>{
    el.addEventListener('click', ()=>{
      el.classList.toggle('done');
      recalc();
    });
  });
  recalc();
</script>

</body>
</html>
