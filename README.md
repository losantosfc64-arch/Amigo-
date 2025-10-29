<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LingAI — IA Real con OpenRouter</title>
<style>
body {
  margin: 0;
  background: radial-gradient(circle at 10% 20%, #0a0a0a, #000);
  font-family: "Segoe UI", system-ui, sans-serif;
  color: #eee;
  display: flex;
  flex-direction: column;
  height: 100vh;
}
header {
  background: linear-gradient(90deg, #101010, #1a1a1a);
  border-bottom: 1px solid #222;
  text-align: center;
  padding: 10px;
  color: #00ffff;
  text-shadow: 0 0 6px #0ff;
  font-weight: bold;
  letter-spacing: 1px;
}
#chatbox {
  flex: 1;
  overflow-y: auto;
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.message {
  max-width: 80%;
  padding: 10px 15px;
  border-radius: 10px;
  font-size: 15px;
  line-height: 1.4em;
  animation: fadeIn 0.4s ease;
}
.user {
  align-self: flex-end;
  background: linear-gradient(90deg, #0077ff, #00ccff);
  color: #fff;
}
.ai {
  align-self: flex-start;
  background: linear-gradient(90deg, #202020, #1a1a1a);
  border: 1px solid #333;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(5px); }
  to { opacity: 1; transform: translateY(0); }
}
#inputArea {
  display: flex;
  border-top: 1px solid #222;
  padding: 10px;
  background: #0a0a0a;
}
input {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #333;
  background: #111;
  color: #fff;
  font-size: 14px;
  outline: none;
}
button {
  margin-left: 8px;
  background: linear-gradient(90deg, #00ccff, #0077ff);
  border: none;
  color: #fff;
  font-weight: bold;
  border-radius: 8px;
  padding: 10px 15px;
  cursor: pointer;
}
button:active {
  opacity: 0.8;
}
footer {
  text-align: center;
  font-size: 12px;
  color: #666;
  padding: 6px;
  background: #0a0a0a;
}
</style>
</head>
<body>

<header>LingAI — IA Real (OpenRouter)</header>

<div id="chatbox">
  <div class="ai message">¡Hola! Soy LingAI con <b>IA real de OpenRouter</b>. ¡Pregúntame lo que quieras! <span style="font-size:0.8em">Usa el modelo gratis</span></div>
</div>

<div id="inputArea">
  <input type="text" id="userInput" placeholder="Escribe algo..." autofocus>
  <button onclick="enviar()">Enviar</button>
</div>

<footer>IA gratis • OpenRouter • Hecho por una tortuga</footer>

<script>
const chatbox = document.getElementById('chatbox');
const userInput = document.getElementById('userInput');

async function generarRespuesta(input) {
  // Respuestas locales rápidas
  if (input.includes('hola')) return '¡Hola! ¡Ahora tengo IA real y potente! ¿Qué quieres probar?';
  if (input.includes('tortuga')) return '¡TORTUGA DETECTADA! Lenta pero con superpoderes de IA. ¿Quieres un chiste?';
  if (input.includes('reset')) {
    chatbox.innerHTML = '<div class="ai message">Reiniciando LingAI...</div>';
    setTimeout(() => {
      agregarMensaje('ai', '¡Listo! ¿En qué te ayudo ahora?');
    }, 800);
    return;
  }

  // Mostrar "escribiendo..."
  const typingMsg = agregarMensaje('ai', 'Escribiendo con IA real...');

  try {
    // ¡¡¡ AQUÍ VA TU CLAVE DE OPENROUTER !!!
    const response = await fetch('https://openrouter.ai/api/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Authorization': 'Bearer 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',  // ← ¡EDITA ESTA LÍNEA!
        'Content-Type': 'application/json',
        'HTTP-Referer': window.location.href,
        'X-Title': 'LingAI Tortuga',
      },
      body: JSON.stringify({
        model: 'meta-llama/llama-3.2-1b-instruct:free',  // GRATIS, rápido y en español
        messages: [
          { role: 'system', content: 'Eres LingAI, un asistente conversacional ingenioso, divertido y en español. Usa emojis y responde con personalidad. ¡Eres una tortuga sabia y moderna!' },
          { role: 'user', content: input }
        ],
        temperature: 0.9,
        max_tokens: 150
      })
    });

    if (!response.ok) {
      const err = await response.text();
      throw new Error('Error: ' + err);
    }

    const data = await response.json();
    const aiReply = data.choices[0]?.message?.content?.trim() || '¡Estoy pensando...';

    typingMsg.innerHTML = aiReply.replace(/\n/g, '<br>');

  } catch (error) {
    console.error(error);
    typingMsg.innerHTML = 'Sin conexión. Modo local activado<br><small>(Revisa tu clave o internet)</small>';
  }
}

function enviar() {
  const text = userInput.value.trim();
  if (!text) return;
  
  agregarMensaje('user', text);
  userInput.value = '';

  generarRespuesta(text.toLowerCase());
}

function agregarMensaje(tipo, texto) {
  const msg = document.createElement('div');
  msg.classList.add('message', tipo);
  msg.innerHTML = texto;
  chatbox.appendChild(msg);
  chatbox.scrollTop = chatbox.scrollHeight;
  return msg;
}

// Enviar con Enter
userInput.addEventListener('keypress', e => {
  if (e.key === 'Enter') enviar();
});
</script>

</body>
</html>
