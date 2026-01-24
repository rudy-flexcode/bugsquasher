<script lang="ts">
  import { onMount } from 'svelte';
  import logoBS from '../lib/assets/logoBS.png';
  import emailjs from '@emailjs/browser';

  let nom = "BugSquasher";
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

    // 🛡 Honeypot anti-bot
    if (formData.get('website')) {
      sending = false;
      return; // bot détecté → on stop
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
      <h2 class="animated-text2">Créateur de sites web</h2>
</div>
    <img class="animated-img" src={logoBS} alt={nom}>
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
            J’utilise des technologies modernes comme <strong>Svelte, React ou Vue</strong>
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
      <li>✔ Jusqu’à 4 sections personnalisées (Accueil, À propos, Services, Contact)</li>
      <li>✔ Animations légères et interactions modernes</li>
      <li>✔ Formulaire de contact fonctionnel</li>
      <li>✔ Optimisation SEO de base (Google)</li>
      <li>✔ Mise en ligne + accompagnement</li>
    </ul>
    <p class="prix">400€</p>
    <span class="badge">Offre de lancement – Île de La Réunion 🇷🇪</span>
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
    © {new Date().getFullYear()} BugSquasher — Développeur web freelance à La Réunion  
    · <a href="/mentions-legales">Mentions légales</a>
  </p>
</footer>

</main>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Poppins:wght@600;700&display=swap');

:global(body) { margin: 0; font-family: 'Inter', sans-serif; }

/* MENU */
.menu { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: rgba(0,0,0,0.6); padding: 10px 25px; border-radius: 50px; display: flex; gap: 25px; z-index: 100; font-family: 'Poppins', sans-serif; justify-content: center; }
.menu a { color: #fff; text-decoration: none; }
.menu a:hover { color: #ff6a6a; }

/* HOME */
.container { display: flex; justify-content: center; align-items: center; height: 100vh; gap: 50px; background: linear-gradient(135deg, #6a11cb, #ff6a6a); }
.text-block { display: flex; flex-direction: column; align-items: center; }
.animated-text { font-size: 4rem; color: #fff; white-space: nowrap; border-right: 4px solid #fff; overflow: hidden; width: 0; animation: typing 2s steps(12) forwards, blink 0.7s infinite; }
.animated-text2 { font-size: 1.7rem; color: #fff; white-space: nowrap; border-right: 4px solid gold; overflow: hidden; width: 0; animation: typing2 2s steps(26) 2s forwards, blink 0.7s infinite; }
@keyframes typing { to { width: 11ch; } }
@keyframes typing2 { to { width: 17ch; } }
@keyframes blink { 50% { border-color: transparent; } }
.animated-img { max-width: 300px; border-radius: 25px; box-shadow: 0 15px 40px rgba(0,0,0,0.3); animation: float 3s ease-in-out infinite alternate; }
@keyframes float { to { transform: translateY(-20px) rotate(2deg); } }

/* ABOUT */
.about-section {
  padding: 120px 20px;
  background: linear-gradient(135deg, #f0f4f8, #e8eef4);
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
  color: #333;
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
  border-radius: 25px;
  box-shadow: 0 15px 40px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.about-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0,0,0,0.15);
}
.about-card h3 {
  margin-bottom: 15px;
  color: #6a11cb;
  font-size: 1.4rem;
}
.about-card p {
  font-size: 1rem;
  line-height: 1.6;
  color: #555;
}

/* OFFRE */
.tarifs-section { padding: 120px 20px; background: linear-gradient(135deg, #1dded0, #4facfe); text-align: center; }
.tarif-card.unique { background: white; max-width: 400px; margin: auto; padding: 40px; border-radius: 30px; box-shadow: 0 20px 50px rgba(0,0,0,0.2); }
.tarif-card ul { list-style: none; padding: 0; }
.prix { font-size: 2rem; font-weight: bold; color: #6a11cb; }
.badge { display: inline-block; margin-top: 15px; padding: 5px 15px; background: #ff6a6a; color: white; border-radius: 20px; font-size: 0.9rem; }
.subtitle {
  margin-bottom: 20px;
  font-size: 1rem;
  color: #666;
  font-style: italic;
}

/* CONTACT */
.contact-section { padding: 120px 20px; background: #fdf7f2; text-align: center; }
.contact-form { max-width: 500px; margin: auto; display: flex; flex-direction: column; gap: 20px; }
.contact-form input, .contact-form textarea { padding: 15px; border-radius: 15px; border: 1px solid #ccc; }
.contact-form button { padding: 15px; border: none; border-radius: 20px; background: linear-gradient(135deg, #6a11cb, #ff6a6a); color: white; font-weight: bold; cursor: pointer; }
.success { margin-top: 15px; color: green; font-weight: 600; }
.error { margin-top: 15px; color: red; font-weight: 600; }

/* RESPONSIVE */
@media (max-width: 768px) {
  .menu { width: 90%; padding: 8px 15px; gap: 15px; font-size: 0.9rem; }
  .container { flex-direction: column; text-align: center; gap: 30px; padding: 20px; }
  .animated-text { font-size: 2.5rem; }
  .animated-text2 { font-size: 1.2rem; }
  .animated-img { max-width: 180px; }
  .about-container h2 { font-size: 2rem; margin-bottom: 40px; }
  .about-card { padding: 25px 20px; }
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
  background: #111;
  color: #aaa;
  text-align: center;
  padding: 25px 15px;
  font-size: 0.9rem;
}

.footer a {
  color: #ff6a6a;
  text-decoration: none;
  margin-left: 5px;
}

.footer a:hover {
  text-decoration: underline;
}

</style>
