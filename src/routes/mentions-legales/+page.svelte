<script>
  const nom = "kreol tech";
</script>

<section class="legal">
  <h1>Mentions légales</h1>

  <h2>Éditeur du site</h2>
  <p>
    Nom : {nom}<br>
    Statut : Développeur web indépendant <br>
    Localisation : La Réunion, France<br>
    Email : code.appisland@gmail.com
  </p>

  <h2>Hébergement</h2>
  <p>
    Hébergeur : Vercel Inc.<br>
    Adresse : 340 S Lemon Ave #4133, Walnut, CA 91789, USA<br>
    Site : https://vercel.com
  </p>

  <h2>Données personnelles</h2>
  <p>
    Les informations transmises via le formulaire de contact sont utilisées
    uniquement pour répondre aux demandes et ne sont jamais revendues.
  </p>
</section>

<style>
.legal :global(a) { color: inherit; }
:global(body) {
  background: #f7f9ff;
  color: #121622;
}
.legal {
  max-width: 800px;
  margin: 120px auto;
  padding: 28px 24px;
  font-family: Inter, sans-serif;
  background: #ffffff;
  border: 1px solid rgba(18, 22, 34, 0.08);
  border-radius: 18px;
  box-shadow: 0 18px 40px rgba(18, 22, 34, 0.12);
}
.legal h1 {
  margin-bottom: 24px;
  font-size: 2.2rem;
}
.legal h2 {
  margin-top: 22px;
  color: #0ea5b7;
  font-size: 1.25rem;
}
.legal p {
  line-height: 1.7;
  color: #2c3345;
}

@media (max-width: 768px) {
  .legal {
    margin: 90px 16px;
    padding: 22px 18px;
  }
  .legal h1 { font-size: 1.9rem; }
}
</style>
