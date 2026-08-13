const WHATSAPP_URL =
  "https://wa.me/5585965813006?text=Ol%C3%A1%2C%20Netinho!%20Preciso%20de%20ajuda%20com%20minha%20moto.%20Pode%20me%20passar%20um%20or%C3%A7amento%3F";

const services = [
  ["01", "Revisão completa", "Checagem dos pontos essenciais para você rodar com mais segurança."],
  ["02", "Manutenção preventiva", "Ajustes e cuidados para evitar que um pequeno problema vire prejuízo."],
  ["03", "Recuperação de motor", "Análise técnica e reparo criterioso para devolver força à sua moto."],
  ["04", "Diagnóstico de defeitos", "Investigação precisa para encontrar até aquela falha difícil e intermitente."],
  ["05", "Parte elétrica", "Testes e correções em partida, iluminação, carga e componentes elétricos."],
  ["06", "Atendimento a domicílio", "O mecânico vai até você, com deslocamento acertado antes da visita."],
];

const bikes = [
  ["Custom", "Torque, presença e cuidado em cada detalhe", "images/moto-harley.jpg", "custom"],
  ["Esportiva", "Precisão para máquinas de alta performance", "images/moto-kawasaki.jpg", "sport"],
  ["Yamaha", "Especialidade da casa", "images/moto-yamaha.jpg", "yamaha"],
];

const testimonials = [
  ["CS", "Carlos Silva", "Excelente profissional. Resolveu um problema que outros mecânicos não conseguiram. Recomendo!"],
  ["RL", "Rafael Lima", "Atendimento rápido, eficiente e honesto. Foi até minha casa e resolveu."],
  ["JV", "João Victor", "Especialista em Yamaha mesmo. Meu motor ficou zero. Um serviço muito bem feito."],
];

document.getElementById("services-grid").innerHTML = services
  .map(([number, title, text]) => `
    <article class="service-card" data-reveal>
      <span class="service-number">${number}</span>
      <span class="service-symbol" aria-hidden="true"><i></i><i></i></span>
      <h3>${title}</h3>
      <p>${text}</p>
      <a class="js-whatsapp" href="#" target="_blank" rel="noopener noreferrer" aria-label="Pedir orçamento para ${title}">→</a>
    </article>`)
  .join("");

document.getElementById("bikes-grid").innerHTML = bikes
  .map(([name, detail, image, className], index) => `
    <article class="bike-card ${className}" data-reveal>
      <img src="${image}" alt="Moto ${name}" loading="lazy">
      <div class="bike-shade"></div>
      <span class="bike-index">0${index + 1}</span>
      <div class="bike-copy"><p>${detail}</p><h3>${name}</h3></div>
    </article>`)
  .join("");

document.getElementById("testimonial-grid").innerHTML = testimonials
  .map(([initials, name, text]) => `
    <blockquote data-reveal>
      <div class="quote-mark">“</div>
      <p>${text}</p>
      <footer><span>${initials}</span><div><strong>${name}</strong><small>Cliente Netinho Motos</small></div></footer>
    </blockquote>`)
  .join("");

document.querySelectorAll(".js-whatsapp").forEach((link) => {
  link.href = WHATSAPP_URL;
});

const header = document.getElementById("site-header");
const menu = document.getElementById("nav-links");
const menuButton = document.getElementById("menu-toggle");

function setMenu(open) {
  menu.classList.toggle("is-open", open);
  menuButton.classList.toggle("is-open", open);
  menuButton.setAttribute("aria-expanded", String(open));
  menuButton.setAttribute("aria-label", open ? "Fechar menu" : "Abrir menu");
}

menuButton.addEventListener("click", () => setMenu(!menu.classList.contains("is-open")));
menu.querySelectorAll("a").forEach((link) => link.addEventListener("click", () => setMenu(false)));
document.addEventListener("keydown", (event) => event.key === "Escape" && setMenu(false));

function updateHeader() {
  header.classList.toggle("is-scrolled", window.scrollY > 32);
}

updateHeader();
window.addEventListener("scroll", updateHeader, { passive: true });

const revealElements = document.querySelectorAll("[data-reveal]");

if ("IntersectionObserver" in window) {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("is-visible");
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.12 },
  );
  revealElements.forEach((element) => observer.observe(element));
} else {
  revealElements.forEach((element) => element.classList.add("is-visible"));
}

document.querySelectorAll(".faq-list details").forEach((item) => {
  item.addEventListener("toggle", () => {
    if (!item.open) return;
    document.querySelectorAll(".faq-list details").forEach((other) => {
      if (other !== item) other.open = false;
    });
  });
});
