<script lang="ts">
  import { onMount } from 'svelte';
  import logoKreol from '../lib/assets/logo-kreol-tech.png';
  import emailjs from '@emailjs/browser';

  let nom = "kreol tech";
  let sending = false;
  let successMessage = '';
  let errorMessage = '';

  // Clés depuis .env
  const SERVICE_ID = import.meta.env.VITE_EMAILJS_SERVICE_ID;
  const TEMPLATE_ID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID;
  const PUBLIC_KEY = import.meta.env.VITE_EMAILJS_PUBLIC_KEY;

  onMount(() => {
    window.scrollTo(0, 0);
  });

  function sendEmail(e: SubmitEvent) {
    e.preventDefault();
    sending = true;
    successMessage = '';
    errorMessage = '';

    const form = e.currentTarget as HTMLFormElement;
    const formData = new FormData(form);

    // Honeypot anti-bot
    if (formData.get('website')) {
      sending = false;
      return; // bot détecté -> on stop
    }

    emailjs.sendForm(SERVICE_ID, TEMPLATE_ID, form, PUBLIC_KEY)
      .then(
        (result) => {
          sending = false;
          successMessage = 'Email envoyé avec succès ! ✅';
          form.reset();
          console.log('EmailJS success:', result.text);
        },
        (error) => {
          sending = false;
          errorMessage = 'Erreur lors de l\'envoi ❌. Réessaie.';
          console.error('EmailJS error:', error);
        }
      );
      console.log(Object.fromEntries(new FormData(form)));
  }
</script>

<!-- MENU -->
<nav class="menu">
  <a href="#home">Accueil</a>
  <a href="#about">À propos</a>
  <a href="#tarifs">Offre</a>
  <a href="#contact">Contact</a>
</nav>

<main>
  <!-- HOME -->
  <section id="home" class="container">
    <div class="text-block">
      <h1 class="animated-text">{nom}</h1>
      <h2 class="animated-text2">Développeur web</h2>
</div>
    <div class="logo-frame">
      <img class="animated-img" src={logoKreol} alt={nom}>
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about" class="about-section">
    <div class="about-container">
      <h2>À propos de moi</h2>
      <div class="about-cards">
        <!-- Carte 1 : Passion & Expertise -->
        <div class="about-card">
          <h3>Passion & Expertise</h3>
          <p>
            Je suis développeur web passionné, spécialisé dans la création de
            <strong>sites et applications modernes, interactifs et évolutifs</strong>.
          </p>
        </div>

        <!-- Carte 2 : Technologies -->
        <div class="about-card">
          <h3>Technologies</h3>
          <p>
            J'utilise des technologies modernes comme <strong>Svelte, React ou Vue</strong>
            pour construire des projets performants et maintenables.
          </p>
        </div>

        <!-- Carte 3 : Philosophie -->
        <div class="about-card">
          <h3>Philosophie</h3>
          <p>
            Mes créations sont pensées pour être <strong>adaptables et évolutives</strong>,
            afin que ton site puisse grandir avec ton activité.
          </p>
          <p>
            Mon objectif : transformer tes idées en <strong>projets concrets, beaux et efficaces</strong>.
          </p>
        </div>
      </div>
    </div>
  </section>

 <!-- OFFRE -->
<section id="tarifs" class="tarifs-section">
  <h2>Offre de lancement</h2>
  <div class="tarif-card unique">
    <h3>Site vitrine / Landing page professionnelle</h3>
    <p class="subtitle">
      Idéal pour entrepreneurs, freelances et petites entreprises
    </p>
    <ul>
      <li>✔ Design moderne, rapide et responsive (mobile / tablette / PC)</li>
      <li>✔ Jusqu'à 4 sections personnalisées (Accueil, À propos, Services, Contact)</li>
      <li>✔ Animations légères et interactions modernes</li>
      <li>✔ Formulaire de contact fonctionnel</li>
      <li>✔ Optimisation SEO de base (Google)</li>
      <li>✔ Mise en ligne + accompagnement</li>
    </ul>
    <p class="prix">400€</p>
    <span class="badge">Offre de lancement – Île de La Réunion</span>
  </div>
</section>


  <!-- CONTACT -->
  <section id="contact" class="contact-section">
    <h2>Contactez-moi</h2>
    <form class="contact-form" on:submit={sendEmail}>
      <input type="text" name="user_name" placeholder="Votre nom" required />
      <input type="email" name="user_email" placeholder="Votre email" required />
      <textarea name="message" placeholder="Votre message" rows="5" required></textarea>

      <!-- Honeypot invisible -->
      <input type="text" name="website" style="display:none" tabindex="-1" autocomplete="off" />

      <button type="submit" disabled={sending}>
        {sending ? 'Envoi...' : 'Envoyer'}
      </button>
    </form>

    {#if successMessage}
      <p class="success">{successMessage}</p>
    {/if}

    {#if errorMessage}
      <p class="error">{errorMessage}</p>
    {/if}
  </section>
  <footer class="footer">
  <p>
    © {new Date().getFullYear()} kreol tech — Développeur web freelance à La Réunion
    · <a href="/mentions-legales">Mentions légales</a>
  </p>
</footer>

</main>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Poppins:wght@400;500;600;700&display=swap');

:global(:root) {
  --accent: #27f3ff;
  --accent-2: #7bffb5;
  --accent-dark: #0ea5b7;
  --blue-glow: #64caf1;
  --surface: #9ddcff;
  --surface-alt: #c6edff;
  --ink: #121622;
  --ink-soft: #2c3345;
  --muted: #6b7285;
  --white: #f7f9ff;
}

:global(body) { margin: 0; font-family: 'Inter', sans-serif; color: var(--white); }

/* MENU */
.menu {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: transparent;
  padding: 6px 0;
  border-radius: 0;
  display: flex;
  gap: 34px;
  z-index: 100;
  font-family: 'Inter', sans-serif;
  justify-content: center;
}
.menu a {
  color: rgba(255, 255, 255, 0.88);
  text-decoration: none;
  font-weight: 300;
  letter-spacing: 0.9px;
  text-shadow: 0 2px 10px rgba(0,0,0,0.35);
  transition: color 0.25s ease;
}
.menu a:hover { color: var(--accent); }

/* HOME */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  gap: 50px;
  background:
    url('/bg-tech.png') center/cover no-repeat,
    linear-gradient(135deg, #6a11cb, #ff6a6a);
}
.text-block { display: flex; flex-direction: column; align-items: flex-start; text-align: left; gap: 10px; }
.animated-text {
  --typing-width: 10ch;
  font-size: 4.6rem;
  color: var(--white);
  white-space: nowrap;
  border-right: 3px solid rgba(255,255,255,0.9);
  overflow: hidden;
  width: 0;
  letter-spacing: 0.6px;
  font-weight: 600;
  text-shadow: 0 8px 30px rgba(0,0,0,0.35);
  animation: typing 2s steps(10) forwards, blink 0.7s infinite;
}
.animated-text2 {
  --typing2-width: 15ch;
  font-size: 1.6rem;
  color: rgba(255,255,255,0.82);
  white-space: nowrap;
  border-right: 3px solid var(--accent);
  overflow: hidden;
  width: 0;
  letter-spacing: 0.8px;
  font-weight: 300;
  animation: typing2 2.2s steps(10) 2s forwards, blink 0.7s infinite;
}
@keyframes typing { to { width: var(--typing-width); } }
@keyframes typing2 { to { width: var(--typing2-width); } }
@keyframes blink { 50% { border-color: transparent; } }
.logo-frame {
  padding: 12px;
  border-radius: 16px;
  border: 1px solid rgba(255,255,255,0.16);
  background: transparent;
  box-shadow: 0 12px 28px rgba(0,0,0,0.35);
  animation: float 3s ease-in-out infinite alternate;
}
.animated-img {
  display: block;
  max-width: 260px;
  border-radius: 10px;
  background: transparent;
  filter: drop-shadow(0 8px 18px rgba(0,0,0,0.35));
}
@keyframes float { to { transform: translateY(-20px) rotate(2deg); } }

@media (min-width: 769px) {
  .animated-text { --typing-width: 8.0ch; }
  .animated-text2 { --typing2-width: 14.8ch; }
}

/* ABOUT */
.about-section {
  padding: 120px 20px;
  background:
    radial-gradient(45% 65% at 15% 20%, rgba(79,182,232,0.55), transparent 65%),
    linear-gradient(135deg, var(--surface), var(--surface-alt));
  display: flex;
  justify-content: center;
}
.about-container {
  max-width: 1000px;
  width: 100%;
  text-align: center;
}
.about-container h2 {
  font-size: 2.5rem;
  margin-bottom: 60px;
  color: var(--ink);
  font-family: 'Poppins', sans-serif;
}
.about-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 40px;
}
.about-card {
  background: white;
  padding: 30px 25px;
  border-radius: 18px;
  box-shadow: 0 15px 40px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.about-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0,0,0,0.15);
}
.about-card h3 {
  margin-bottom: 15px;
  color: var(--accent-dark);
  font-size: 1.4rem;
}
.about-card p {
  font-size: 1rem;
  line-height: 1.6;
  color: var(--ink-soft);
}

/* OFFRE */
.tarifs-section {
  padding: 120px 20px;
  background:
    url('/bg-tech.png') center/cover no-repeat,
    linear-gradient(135deg, #1dded0, #4facfe);
  text-align: center;
}
.tarif-card.unique {
  background: rgba(255,255,255,0.92);
  max-width: 420px;
  margin: auto;
  padding: 42px;
  border-radius: 28px;
  box-shadow: 0 20px 50px rgba(0,0,0,0.28);
  color: var(--ink);
  backdrop-filter: blur(4px);
}
.tarif-card ul { list-style: none; padding: 0; color: var(--ink-soft); line-height: 1.65; }
.prix { font-size: 2rem; font-weight: 600; color: var(--accent-dark); letter-spacing: 0.4px; }
.badge {
  display: inline-block;
  margin-top: 15px;
  padding: 6px 16px;
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  color: #0b0f1a;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
}
.subtitle {
  margin-bottom: 20px;
  font-size: 1rem;
  color: var(--muted);
  font-style: normal;
}

/* CONTACT */
.contact-section {
  padding: 120px 20px;
  background:
    radial-gradient(45% 65% at 85% 15%, rgba(79,182,232,0.55), transparent 65%),
    linear-gradient(135deg, var(--surface), var(--surface-alt));
  text-align: center;
  color: var(--ink);
}
.contact-form { max-width: 500px; margin: auto; display: flex; flex-direction: column; gap: 20px; }
.contact-form input, .contact-form textarea {
  padding: 15px;
  border-radius: 14px;
  border: 1px solid #dfe6ee;
  font-weight: 300;
}
.contact-form input:focus, .contact-form textarea:focus {
  outline: none;
  border-color: var(--accent-dark);
  box-shadow: 0 0 0 3px rgba(39,243,255,0.15);
}
.contact-form button {
  padding: 15px;
  border: none;
  border-radius: 18px;
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  color: #0b0f1a;
  font-weight: 600;
  letter-spacing: 0.3px;
  cursor: pointer;
}
.success { margin-top: 15px; color: #39d98a; font-weight: 600; }
.error { margin-top: 15px; color: #ff6a6a; font-weight: 600; }

/* RESPONSIVE */
@media (max-width: 768px) {
  .menu { width: 90%; padding: 6px 0; gap: 14px; font-size: 0.9rem; }
  .container { flex-direction: column; text-align: center; gap: 30px; padding: 20px; }
  .text-block { align-items: center; text-align: center; }
  .animated-text { font-size: 2.5rem; }
  .animated-text2 { font-size: 1.2rem; }
  .logo-frame { padding: 10px; border-radius: 14px; }
  .animated-img { max-width: 170px; }
  .about-container h2 { font-size: 2rem; margin-bottom: 40px; }
  .about-card { padding: 25px 20px; border-radius: 16px; }
  .about-card p { font-size: 1rem; line-height: 1.6; }
  .tarif-card.unique { padding: 30px 20px; }
  .tarif-card h3 { font-size: 1.4rem; }
  .prix { font-size: 1.6rem; }
  .contact-form { padding: 0 10px; }
  .contact-form input, .contact-form textarea { font-size: 0.95rem; }
  .contact-form button { font-size: 1rem; }
}
/* FOOTER */
.footer {
  background: #0b0f1a;
  color: #aab3c3;
  text-align: center;
  padding: 25px 15px;
  font-size: 0.9rem;
}

.footer a {
  color: var(--accent);
  text-decoration: none;
  margin-left: 5px;
}

.footer a:hover {
  text-decoration: underline;
}

</style>
