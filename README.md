<!DOCTYPE html>
<html>
<head>
<style>
  body { font-family: 'Segoe UI', Arial, sans-serif; margin: 0; padding: 10px; background: transparent; }
  .form-card { background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); border: 1px solid #eee; max-width: 450px; margin: auto; }
  h2 { color: #333; font-size: 20px; margin-bottom: 15px; text-align: center; }
  .form-group { margin-bottom: 12px; }
  label { display: block; margin-bottom: 4px; font-weight: 600; color: #555; font-size: 13px; }
  input, select, textarea { width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; font-size: 14px; }
  .row { display: flex; gap: 10px; }
  .row > div { flex: 1; }
  .submit-btn { background-color: #25D366; color: white; padding: 12px; border: none; border-radius: 6px; cursor: pointer; font-size: 16px; font-weight: bold; width: 100%; margin-top: 10px; transition: 0.3s; }
  .submit-btn:hover { background-color: #128C7E; }
  #result { text-align: center; margin-top: 10px; font-weight: bold; display: none; }
</style>
</head>
<body>

<div class="form-card">
  <h2>Wycena indywidualna</h2>
  <form id="my-form">
    <!-- ВСТАВТЕ ВАШ КЛЮЧ ТУТ -->
    <input type="hidden" name="access_key" value="23490443-79ce-47ad-ba63-ac5b4219b20f">

    <div class="form-group">
      <label>Imię i Nazwisko</label>
      <input type="text" name="name" required>
    </div>
    <div class="form-group">
      <label>Adres E-mail</label>
      <input type="email" name="email" required>
    </div>
    <div class="row">
      <div class="form-group">
        <label>Szerokość (mm)</label>
        <input type="number" name="width">
      </div>
      <div class="form-group">
        <label>Wysokość (mm)</label>
        <input type="number" name="height">
      </div>
    </div>
    <div class="form-group">
      <label>Dodatkowe uwagi</label>
      <textarea name="message" rows="2"></textarea>
    </div>
    <button type="submit" id="btn" class="submit-btn">Wyślij zapytanie</button>
  </form>
  <div id="result"></div>
</div>

<script>
const form = document.getElementById('my-form');
const result = document.getElementById('result');
const btn = document.getElementById('btn');

form.addEventListener('submit', function(e) {
    e.preventDefault();
    const formData = new FormData(form);
    const object = Object.fromEntries(formData);
    const json = JSON.stringify(object);
    
    result.style.display = "block";
    result.innerHTML = "Wysyłanie...";
    btn.disabled = true;

    fetch('https://api.web3forms.com', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: json
        })
        .then(async (response) => {
            let json = await response.json();
            if (response.status == 200) {
                result.innerHTML = "Dziękujemy! Wiadomość została wysłana.";
                result.style.color = "green";
                form.reset();
            } else {
                console.log(response);
                result.innerHTML = "Błąd: " + json.message;
                result.style.color = "red";
            }
        })
        .catch(error => {
            console.log(error);
            result.innerHTML = "Coś poszło nie tak!";
        })
        .then(function() {
            btn.disabled = false;
            setTimeout(() => { result.style.display = "none"; }, 5000);
        });
});
</script>

</body>
</html>
