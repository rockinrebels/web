Title: Kontakt
Lang: cs

<form action="https://formspree.io/f/xeaapzwr" method="POST" class="contact-form" id="contact-form">
  <p>
    <label for="name">Jméno</label><br>
    <input type="text" id="name" name="name" required>
  </p>
  <p>
    <label for="email">E-mail</label><br>
    <input type="email" id="email" name="email" required>
  </p>
  <p>
    <label for="message">Zpráva</label><br>
    <textarea id="message" name="message" rows="6" required></textarea>
  </p>
  <input type="text" name="_gotcha" style="display:none" tabindex="-1" autocomplete="off">
  <p>
    <button type="submit">Odeslat</button>
  </p>
  <p class="form-status" role="status" aria-live="polite"></p>
</form>

<script>
(function () {
  var form = document.getElementById("contact-form");
  var status = form.querySelector(".form-status");
  var button = form.querySelector("button[type=submit]");
  function show(text, color) {
    status.textContent = text;
    status.style.color = color;
  }
  form.addEventListener("submit", function (event) {
    event.preventDefault();
    button.disabled = true;
    show("Odesílám…", "");
    fetch(form.action, {
      method: "POST",
      body: new FormData(form),
      headers: { "Accept": "application/json" }
    }).then(function (response) {
      if (response.ok) {
        form.reset();
        show("Děkujeme, zpráva byla odeslána.", "green");
        return;
      }
      return response.json().then(function (data) {
        var detail = data && data.errors ? data.errors.map(function (e) { return e.message; }).join(", ") : "";
        show("Zprávu se nepodařilo odeslat" + (detail ? ": " + detail : ".") + " Zkuste to prosím znovu.", "red");
      });
    }).catch(function () {
      show("Zprávu se nepodařilo odeslat. Zkontrolujte připojení k internetu a zkuste to znovu.", "red");
    }).then(function () {
      button.disabled = false;
    });
  });
})();
</script>
