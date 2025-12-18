<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="width=device-width,initial-scale=1,maximum-scale=1"
    />
    <title>Votación por categorías — Responsive</title>
    <style>
      :root {
        --bg: #071129;
        --card: #0b1220;
        --accent: #06b6d4;
        --muted: #94a3b8;
        --glass: rgba(255, 255, 255, 0.03);
        --max-width: 920px;
      }
      * {
        box-sizing: border-box;
        font-family: Inter, system-ui, "Segoe UI", Roboto, Arial, sans-serif;
      }
      html,
      body {
        height: 100%;
      }
      body {
        margin: 0;
        background: linear-gradient(180deg, var(--bg) 0%, #071827 100%);
        color: #e6eef6;
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 18px;
      }

      .card {
        width: 100%;
        max-width: var(--max-width);
        background: var(--card);
        border-radius: 12px;
        padding: 20px;
        box-shadow: 0 10px 40px rgba(2, 6, 23, 0.6);
        position: relative;
        overflow: visible;
      }
      h1 {
        margin: 0 0 6px;
        font-size: 20px;
      }
      .meta {
        color: var(--muted);
        font-size: 13px;
        margin-bottom: 14px;
      }

      .vote-area {
        display: flex;
        gap: 18px;
      }
      .left {
        flex: 1;
      }
      .right {
        width: 300px;
      }

      .options {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-top: 14px;
      }
      .option {
        background: var(--glass);
        padding: 12px;
        border-radius: 10px;
        display: flex;
        align-items: center;
        gap: 12px;
        cursor: pointer;
        border: 1px solid transparent;
      }
      .option:hover {
        border-color: rgba(255, 255, 255, 0.04);
      }
      .option input {
        transform: scale(1.05);
      }

      .controls {
        display: flex;
        gap: 10px;
        margin-top: 14px;
      }
      button {
        background: var(--accent);
        border: none;
        padding: 10px 14px;
        border-radius: 10px;
        color: #042024;
        font-weight: 700;
        cursor: pointer;
      }
      button.secondary {
        background: transparent;
        border: 1px solid rgba(255, 255, 255, 0.06);
        color: var(--muted);
        font-weight: 600;
      }
      button[disabled] {
        opacity: 0.45;
        cursor: not-allowed;
      }

      .progress {
        height: 10px;
        background: rgba(255, 255, 255, 0.04);
        border-radius: 999px;
        overflow: hidden;
        margin-top: 14px;
      }
      .progress > i {
        display: block;
        height: 100%;
        background: linear-gradient(90deg, var(--accent), #8b5cf6);
        width: 0;
      }

      .summary {
        background: var(--glass);
        padding: 12px;
        border-radius: 8px;
      }
      .small {
        font-size: 13px;
        color: var(--muted);
      }

      /* slides */
      .reveal-wrap {
        padding: 6px;
      }
      .carousel {
        position: relative;
        overflow: visible;
      }
      .viewport {
        overflow: hidden;
        width: 100%;
        box-sizing: border-box;
      }
      .slides {
        display: flex;
        transition: transform 0.45s ease;
        will-change: transform;
      }
      .slide {
        flex: 0 0 100%;
        box-sizing: border-box;
        padding: 16px;
        border-radius: 10px;
        background: rgba(255, 255, 255, 0.02);
        margin-right: 20px;
        opacity: 0;
        pointer-events: none;
        transition: opacity 0.35s ease, transform 0.35s ease;
        transform: translateY(6px);
      }
      .slide.active {
        opacity: 1;
        pointer-events: auto;
        transform: translateY(0);
      }

      .bar-row {
        display: flex;
        align-items: center;
        gap: 12px;
        margin: 10px 0;
        flex-wrap: wrap;
      }
      .bar-label {
        width: 140px;
        min-width: 90px;
        flex: 0 0 140px;
      }
      .bar-track {
        flex: 1;
        min-width: 0;
        height: 16px;
        background: rgba(255, 255, 255, 0.06);
        border-radius: 999px;
        overflow: hidden;
      }
      .bar-fill {
        height: 100%;
        border-radius: 999px;
        background: linear-gradient(90deg, #10b981, #06b6d4);
        width: 0;
        transition: width 900ms ease;
      }
      .winner {
        border: 2px solid #06b6d4;
        padding: 8px;
        border-radius: 10px;
        animation: winnerPulse 1.4s ease-in-out infinite;
      }

      .nav {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        background: rgba(4, 20, 30, 0.8);
        border: none;
        color: #e6eef6;
        font-size: 22px;
        width: 44px;
        height: 48px;
        border-radius: 12px;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      .nav.left {
        left: -56px;
      }
      .nav.right {
        right: -56px;
      }

      @keyframes winnerPulse {
        0% {
          transform: scale(1);
          box-shadow: 0 0 0 rgba(6, 182, 212, 0);
        }
        50% {
          transform: scale(1.03);
          box-shadow: 0 0 18px rgba(6, 182, 212, 0.9);
        }
        100% {
          transform: scale(1);
          box-shadow: 0 0 0 rgba(6, 182, 212, 0);
        }
      }

      /* dots */
      .dots {
        display: flex;
        justify-content: center;
        gap: 8px;
        margin-top: 12px;
      }
      .dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.3);
        cursor: pointer;
        transition: transform 0.2s, background 0.2s;
      }
      .dot.active {
        background: var(--accent);
        transform: scale(1.25);
      }

      /* modal */
      .modal {
        position: fixed;
        inset: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        background: rgba(2, 6, 23, 0.6);
        z-index: 9999;
      }
      .modal-card {
        background: var(--card);
        padding: 16px;
        border-radius: 10px;
        width: 360px;
        max-width: 92%;
        color: #e6eef6;
      }
      .modal-card input {
        width: 100%;
        padding: 10px;
        border-radius: 8px;
        border: 1px solid rgba(255, 255, 255, 0.06);
        background: transparent;
        color: #e6eef6;
      }
      .modal-actions {
        display: flex;
        gap: 8px;
        justify-content: flex-end;
        margin-top: 12px;
      }

      /* ========== RESPONSIVE RULES ========== */
      @media (max-width: 900px) {
        :root {
          --max-width: 760px;
        }
        .right {
          width: 260px;
        }
        .bar-label {
          width: 120px;
          flex: 0 0 120px;
        }
        .nav.left {
          left: -46px;
        }
        .nav.right {
          right: -46px;
        }
      }

      @media (max-width: 700px) {
        body {
          padding: 12px;
        }
        .vote-area {
          flex-direction: column;
        }
        .right {
          width: 100%;
        }
        .options {
          gap: 12px;
        }
        .controls {
          flex-direction: row;
          gap: 8px;
        }
        .modal-card {
          width: 320px;
        }
        .bar-label {
          width: 110px;
          flex: 0 0 110px;
        }
        .nav {
          width: 40px;
          height: 44px;
          font-size: 20px;
        }
        .nav.left {
          left: 6px;
        }
        .nav.right {
          right: 6px;
        }
        .viewport {
          padding: 0;
        }
      }

      @media (max-width: 420px) {
        :root {
          --max-width: 420px;
        }
        h1 {
          font-size: 18px;
        }
        .option {
          padding: 14px;
        }
        .bar-label {
          width: 90px;
          flex: 0 0 90px;
        }
        .dot {
          width: 9px;
          height: 9px;
        }
        .nav {
          display: flex;
        }
      }
    </style>
  </head>
  <body>
    <main class="card" role="main">
      <h1>Votación por categorías</h1>
      <div class="meta">
        Selecciona una opción por categoría. Guardá para pasar a la siguiente.
      </div>

      <div class="vote-area">
        <section class="left">
          <div id="categoryTitle" style="font-weight: 700; font-size: 18px">
            Cargando...
          </div>
          <div id="categoryDesc" class="small" style="margin-top: 6px"></div>
          <div class="options" id="optionsContainer"></div>
          <div class="controls">
            <button id="saveBtn" disabled>Guardar y continuar</button>
            <button id="prevBtn" class="secondary" disabled>Anterior</button>
          </div>
          <div class="progress" title="Progreso">
            <i id="progressFill" style="width: 0%"></i>
          </div>
        </section>

        <aside class="right">
          <div class="summary">
            <div
              style="
                display: flex;
                justify-content: space-between;
                align-items: center;
              "
            >
              <div style="font-weight: 700">Resumen parcial</div>
              <div id="votesCount" class="small">0 / 0</div>
            </div>
            <div id="summaryList" style="margin-top: 10px"></div>
          </div>
        </aside>
      </div>
    </main>

    <!-- Modal para pedir nombre -->
    <div id="nameModal" class="modal" style="display: none">
      <div class="modal-card">
        <div style="font-weight: 700; margin-bottom: 8px">
          Ingresá tu nombre para votar
        </div>
        <input id="nameInput" placeholder="Tu nombre" autocomplete="off" />
        <div class="modal-actions">
          <button id="nameCancel" class="secondary">Cancelar</button>
          <button id="nameEnter">Entrar</button>
        </div>
        <div class="small" style="margin-top: 8px; color: var(--muted)">
          El nombre solo se usa para evitar votos duplicados en este navegador.
        </div>
      </div>
    </div>

    <script>
      // ===== CONFIG =====
      const categories = [
        {
          id: "gracioso",
          title: "Gracioso del año",
          desc: "Elegí a la persona más graciosa",
          options: ["Ozner", "Martín", "Leo"],
        },
        {
          id: "pelicula",
          title: "Película favorita",
          desc: "Elige el género que prefieras",
          options: ["Acción", "Comedia", "Drama"],
        },
        {
          id: "comida",
          title: "Comida favorita",
          desc: "¿Cuál te gusta más?",
          options: ["Italiana", "Asado", "Sushi"],
        },
      ];

      const storageKey = "votacion_votos";
      const votersKey = "votacion_votantes";
      const sessionKey = "votacion_usuario_actual";
      // for dev/testing set reveal to past date; switch to real reveal if desired
      const REVEAL_ALWAYS = true; // set true to always reveal (dev). Set false to use 31/12 logic
      const revealMonth = 11; // diciembre (0-based)
      const revealDay = 31; // 31
      // ==================

      // elementos
      const categoryTitle = document.getElementById("categoryTitle");
      const categoryDesc = document.getElementById("categoryDesc");
      const optionsContainer = document.getElementById("optionsContainer");
      const saveBtn = document.getElementById("saveBtn");
      const prevBtn = document.getElementById("prevBtn");
      const progressFill = document.getElementById("progressFill");
      const votesCount = document.getElementById("votesCount");
      const summaryList = document.getElementById("summaryList");

      const nameModal = document.getElementById("nameModal");
      const nameInput = document.getElementById("nameInput");
      const nameEnter = document.getElementById("nameEnter");
      const nameCancel = document.getElementById("nameCancel");

      let index = 0;
      let currentUser = null;
      let allVotes = JSON.parse(localStorage.getItem(storageKey) || "{}");
      let voters = JSON.parse(localStorage.getItem(votersKey) || "[]");
      let votes = {};

      // ---- FUNCIONES ----
      function openNameModal() {
        nameModal.style.display = "flex";
        nameInput.value = "";
        setTimeout(() => nameInput.focus(), 40);
      }

      function closeNameModal() {
        nameModal.style.display = "none";
      }

      function blockAlreadyVoted(name) {
        document.body.innerHTML = `
        <main class="card">
          <h1>🚫 Ya votaste</h1>
          <div class="small" style="margin-top:8px">El usuario <strong>${escapeHtml(
            name
          )}</strong> ya participó de esta votación.</div>
        </main>`;
      }

      function escapeHtml(s) {
        return String(s)
          .replace(/&/g, "&amp;")
          .replace(/</g, "&lt;")
          .replace(/>/g, "&gt;");
      }

      // startApp: robust — siempre pide nombre si no hay uno limpio en session
      function startApp() {
        const saved = sessionStorage.getItem(sessionKey);
        if (saved && typeof saved === "string" && saved.trim() !== "") {
          currentUser = saved;
          votes = allVotes[currentUser] || {};
          index = 0;
          renderCategory();
          return;
        }
        // no user -> open modal
        currentUser = null;
        votes = {};
        openNameModal();
      }

      nameEnter.addEventListener("click", () => {
        const val = nameInput.value.trim();
        if (!val) {
          alert("Por favor ingresá un nombre");
          nameInput.focus();
          return;
        }
        if (voters.includes(val.toLowerCase())) {
          closeNameModal();
          blockAlreadyVoted(val);
          return;
        }
        currentUser = val;
        sessionStorage.setItem(sessionKey, currentUser);
        votes = allVotes[currentUser] || {};
        closeNameModal();
        index = 0;
        renderCategory();
      });

      nameCancel.addEventListener("click", () => {
        closeNameModal();
        document.body.innerHTML =
          '<main class="card"><h1>Acceso cancelado</h1></main>';
      });

      // renderiza categoría actual
      function renderCategory() {
        const cat = categories[index];
        categoryTitle.textContent = cat.title;
        categoryDesc.textContent = cat.desc || "";
        optionsContainer.innerHTML = "";

        cat.options.forEach((opt, i) => {
          const lab = document.createElement("label");
          lab.className = "option";
          lab.innerHTML = `<input type="radio" name="choice" value="${escapeHtml(
            opt
          )}"> <span>${escapeHtml(opt)}</span>`;
          optionsContainer.appendChild(lab);
        });

        // si ya votó esta categoría, marcar selección
        if (votes[categorieId(cat)]) {
          const checked = optionsContainer.querySelector(
            `input[value="${escapeHtml(votes[categorieId(cat)])}"]`
          );
          if (checked) checked.checked = true;
        }

        saveBtn.disabled = true;
        prevBtn.disabled = index === 0;
        updateSummary();
      }

      function categorieId(cat) {
        return cat.id;
      }

      // habilita botón cuando el usuario selecciona
      document.body.addEventListener("change", (e) => {
        if (e.target && e.target.name === "choice") {
          saveBtn.disabled = false;
        }
      });

      // guardar voto y avanzar
      saveBtn.addEventListener("click", () => {
        const sel = document.querySelector('input[name="choice"]:checked');
        if (!sel) return alert("Seleccioná una opción antes de guardar");
        const cat = categories[index];
        votes[categorieId(cat)] = sel.value;
        allVotes[currentUser] = votes;
        localStorage.setItem(storageKey, JSON.stringify(allVotes));
        updateSummary();
        if (index < categories.length - 1) {
          index++;
          renderCategory();
        } else finishFlow();
      });

      prevBtn.addEventListener("click", () => {
        if (index > 0) {
          index--;
          renderCategory();
        }
      });

      function updateSummary() {
        summaryList.innerHTML = "";
        categories.forEach((c) => {
          const d = document.createElement("div");
          d.innerHTML = `<strong>${escapeHtml(c.title)}</strong>: ${escapeHtml(
            votes[c.id] || "—"
          )}`;
          summaryList.appendChild(d);
        });
        const done = Object.keys(votes).length;
        votesCount.textContent = `${done} / ${categories.length}`;
        progressFill.style.width =
          Math.round((done / categories.length) * 100) + "%";
      }

      // pantalla final
      function finishFlow() {
        // marcar votante
        if (currentUser && !voters.includes(currentUser.toLowerCase())) {
          voters.push(currentUser.toLowerCase());
          localStorage.setItem(votersKey, JSON.stringify(voters));
        }

        // calcular si es momento de revelar
        const today = new Date();
        let revealDate;
        if (REVEAL_ALWAYS) {
          revealDate = new Date(2000, 0, 1); // pasada -> revela siempre (dev)
        } else {
          revealDate = new Date(today.getFullYear(), revealMonth, revealDay);
        }

        if (today < revealDate) {
          // pantalla de espera
          document.querySelector("main").innerHTML = `
          <main class="card">
            <h1>⏳ Los ganadores serán revelados</h1>
            <div class="small" style="margin-top:8px">31/12</div>
            <div style="margin-top:18px;text-align:center"><button id="devReset" class="secondary">🔄 Resetear votación (DEV)</button></div>
          </main>`;
          document
            .getElementById("devReset")
            .addEventListener("click", devReset);
          return;
        }

        // === construir resultados por categoría ===
        const results = {};
        categories.forEach((c) => {
          results[c.id] = {};
          c.options.forEach((o) => (results[c.id][o] = 0));
        });
        Object.values(allVotes).forEach((u) => {
          for (const k in u) {
            if (results[k] && results[k][u[k]] !== undefined)
              results[k][u[k]]++;
          }
        });

        // generar UI: slides + dots
        const mainHtml = document.querySelector("main");
        mainHtml.innerHTML = `
        <div class="card" style="padding:14px">
          <h1 style="margin-bottom:12px">🎉 Resultados finales</h1>
          <div class="reveal-wrap">
            <div class="carousel">
              <div class="viewport"><div class="slides" id="slidesHolder"></div></div>
              <button class="nav left" id="navLeft">‹</button>
              <button class="nav right" id="navRight">›</button>
              <div class="dots" id="dotsHolder"></div>
            </div>
            <div style="text-align:center;margin-top:12px"><button id="devReset2" class="secondary">🔄 Resetear votación (DEV)</button></div>
          </div>
        </div>`;

        const slidesHolder = document.getElementById("slidesHolder");
        const dotsHolder = document.getElementById("dotsHolder");
        slidesHolder.innerHTML = "";
        dotsHolder.innerHTML = "";

        // crear slides
        categories.forEach((c, idx) => {
          const slide = document.createElement("div");
          slide.className = "slide";
          slide.innerHTML = `<h2>${escapeHtml(c.title)}</h2>`;

          // contar y ordenar
          const counts = {};
          c.options.forEach((o) => (counts[o] = 0));
          Object.values(allVotes).forEach((u) => {
            if (u[c.id]) counts[u[c.id]]++;
          });
          const total = Object.values(counts).reduce((a, b) => a + b, 0) || 1;
          const ordered = Object.entries(counts).sort((a, b) => b[1] - a[1]);

          ordered.forEach(([opt, val], i) => {
            const pct = Math.round((val / total) * 100);
            const isWinner = i === 0 && val > 0;

            // fila DOM
            const row = document.createElement("div");
            row.className = "bar-row";
            row.innerHTML = `
            <div class="bar-label small">${escapeHtml(opt)}</div>
            <div class="bar-track"><div class="bar-fill" style="width:${pct}%"></div></div>
            <div class="small" style="width:64px;text-align:right">${pct}%</div>
          `;
            if (isWinner) row.classList.add("winner");
            slide.appendChild(row);
          });

          slidesHolder.appendChild(slide);

          // dot
          const dot = document.createElement("div");
          dot.className = "dot" + (idx === 0 ? " active" : "");
          dot.addEventListener("click", () => {
            cur = idx;
            update();
          });
          dotsHolder.appendChild(dot);
        });

        // navegación
        let cur = 0;
        const totalSlides = categories.length;
        const slidesEl = slidesHolder;

        // swipe support (touch)
        let touchStartX = null;
        slidesHolder.addEventListener(
          "touchstart",
          (e) => {
            if (e.touches && e.touches.length === 1)
              touchStartX = e.touches[0].clientX;
          },
          { passive: true }
        );
        slidesHolder.addEventListener(
          "touchmove",
          (e) => {
            // prevent default only vertical? keep passive to avoid blocking; we won't prevent here
          },
          { passive: true }
        );
        slidesHolder.addEventListener("touchend", (e) => {
          if (touchStartX === null) return;
          const dx = e.changedTouches[0].clientX - touchStartX;
          const abs = Math.abs(dx);
          const threshold = 40; // px
          if (abs > threshold) {
            if (dx < 0 && cur < totalSlides - 1) cur++;
            else if (dx > 0 && cur > 0) cur--;
            update();
          }
          touchStartX = null;
        });

        const update = () => {
          slidesEl.style.transform = `translateX(-${cur * 100}%)`;
          // activar solo el slide actual
          document.querySelectorAll("#slidesHolder .slide").forEach((s, i) => {
            s.classList.toggle("active", i === cur);
          });
          document.querySelectorAll("#dotsHolder .dot").forEach((d, i) => {
            d.classList.toggle("active", i === cur);
          });
          const leftBtn = document.getElementById("navLeft");
          const rightBtn = document.getElementById("navRight");
          leftBtn.style.display = cur === 0 ? "none" : "flex";
          rightBtn.style.display = cur === totalSlides - 1 ? "none" : "flex";
        };

        document.getElementById("navLeft").addEventListener("click", () => {
          if (cur > 0) {
            cur--;
            update();
          }
        });
        document.getElementById("navRight").addEventListener("click", () => {
          if (cur < totalSlides - 1) {
            cur++;
            update();
          }
        });

        // init
        update();

        // animate fills (they already have width inline; keep this to trigger transitions)
        setTimeout(() => {
          document.querySelectorAll(".bar-fill").forEach((el) => {
            el.style.width = el.style.width;
          });
        }, 80);

        // dev reset
        document
          .getElementById("devReset2")
          .addEventListener("click", devReset);
      }

      function devReset() {
        if (!confirm("Borrar TODOS los votos y usuarios?")) return;
        localStorage.removeItem(storageKey);
        localStorage.removeItem(votersKey);
        sessionStorage.removeItem(sessionKey);
        location.reload();
      }

      // init
      document.addEventListener("DOMContentLoaded", startApp);
    </script>
  </body>
</html>
