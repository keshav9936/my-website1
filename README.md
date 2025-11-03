<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Keshav — Python Developer Portfolio</title>
  <meta name="description" content="Keshav's code portfolio — shows projects, skills, contact, and downloadable code." />
  <style>
    :root{--accent:#2563eb;--bg:#f8fafc;--card:#fff;--muted:#6b7280;--radius:12px;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Helvetica Neue",Arial}
    *{box-sizing:border-box}
    body{margin:0;background:var(--bg);color:#0f172a}
    .container{max-width:1000px;margin:0 auto;padding:24px}
    header{background:rgba(255,255,255,0.6);backdrop-filter:blur(4px);position:sticky;top:0;z-index:40}
    .nav{display:flex;align-items:center;justify-content:space-between;padding:10px 0}
    .brand{display:flex;align-items:center;gap:12px}
    .brand img{width:44px;height:44px;border-radius:8px}
    h1{font-size:18px;margin:0}
    nav ul{display:flex;gap:12px;list-style:none;padding:0;margin:0}
    nav a{color:var(--muted);text-decoration:none;padding:8px 10px;border-radius:8px}
    .hero{display:grid;grid-template-columns:1fr;gap:20px;align-items:center;padding:36px 0}
    .hero h2{font-size:28px;margin:0}
    .muted{color:var(--muted)}
    .card{background:var(--card);padding:16px;border-radius:var(--radius);box-shadow:0 6px 18px rgba(15,23,42,0.06)}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:16px}
    pre{background:#0b1220;color:#e6f0ff;padding:12px;border-radius:8px;overflow:auto}
    code{font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", monospace}
    .btn{display:inline-block;padding:10px 14px;border-radius:10px;background:var(--accent);color:white;text-decoration:none;font-weight:600;border:none}
    .secondary{border:1px solid rgba(15,23,42,0.06);background:transparent;color:var(--muted)}
    footer{padding:24px 0;color:var(--muted)}
    @media(min-width:768px){.hero{grid-template-columns:1fr 320px}.hero h2{font-size:36px}}
  </style>
</head>
<body>
  <header>
    <div class="container nav">
      <div class="brand">
        <img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'><rect rx='18' width='100' height='100' fill='%232563eb'/><text x='50' y='57' font-size='40' text-anchor='middle' fill='white' font-family='Arial'>K</text></svg>" alt="logo">
        <div>
          <h1>Keshav</h1>
          <div class="muted">Python Developer</div>
        </div>
      </div>
      <nav>
        <ul>
          <li><a href="#projects">Projects</a></li>
          <li><a href="#skills">Skills</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <main class="container">
    <section class="hero">
      <div>
        <h2>Hi — I build Python tools and assistants.</h2>
        <p class="muted">Welcome to my portfolio. Below you can view project details, see demo output, and download the source code.</p>
      </div>
      <aside class="card">
        <h3>Quick info</h3>
        <p class="muted">Theme: Light • Users can view demo output and download project code.</p>
      </aside>
    </section>

    <section id="projects" style="margin-top:18px">
      <h2>Projects</h2>
      <div class="grid" style="margin-top:12px">
        <!-- Project: AI Assistant Jarvis -->
        <div class="card">
          <h3>AI Assistant — Jarvis</h3>
          <p class="muted">A smart voice assistant built with Python that listens, speaks, and automates simple tasks. Includes speech recognition, text-to-speech, and basic command handlers.</p>

          <h4 style="margin-top:12px">Demo output</h4>
          <pre id="demoOutput">(click "Show demo output" to simulate Jarvis response)</pre>
          <div style="margin-top:10px;display:flex;gap:8px">
            <button class="btn" onclick="showOutput()">Show demo output</button>
            <button class="btn secondary" onclick="downloadCode()">Download code (.py)</button>
            <a class="btn secondary" href="#contact" style="text-decoration:none;display:inline-flex;align-items:center;justify-content:center">Contact</a>
          </div>

          <h4 style="margin-top:14px">Code (snippet)</h4>
          <pre><code># jarvis.py — simplified example
            import speech_recognition as sr
import threading
import pyttsx3
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.scrollview import ScrollView
from kivy.uix.label import Label
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.image import Image
from kivy.uix.gridlayout import GridLayout
from kivy.core.window import Window
from kivy.clock import mainthread
from kivy.properties import StringProperty, ColorProperty
from kivy.graphics import Color, Rectangle
from google import genai
import random
import time

# ==========================
# CONFIGURATION
# ==========================
pro = random.randint(1, 2)
API_KEY = (
    "AIzaSyA-MjhDp8cZOyV1zM4Js-6dCc0hvznPsl4"
    if pro == 1
    else "AIzaSyBHBYalp3xzvkeNrx_MINuGlK1tjOymMxo"
)
FILE = "history.txt"
client = genai.Client(api_key=API_KEY)
engine = pyttsx3.init()
Window.clearcolor = (0.07, 0.07, 0.07, 1)  # DARK BACKGROUND
speak_enabled = True


# ==========================
# FUNCTIONS
# ==========================
def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    try:
        return recognizer.recognize_google(audio)
    except Exception:
        return None


def speak(text, update_status):
    if speak_enabled:
        update_status("🟢 Speaking...")
        engine.say(text)
        engine.runAndWait()
        time.sleep(0.3)
        update_status("")


def ai_process(user_input):
    try:
        with open(FILE, "r") as f:
            history = f.read()
    except FileNotFoundError:
        history = ""

    try:
        response = client.models.generate_content(
            model="gemini-2.0-flash",
            contents=[
                f"You are Jarvis, a helpful AI assistant. "
                f"Use this conversation history: {history}\nUser: {user_input}"
                f"YOU are made by keshav from bihar"
            ],
        )
        reply = response.text.strip()
    except Exception as e:
        reply = f"(Error: {str(e)})"

    with open(FILE, "a") as f:
        f.write(f"\nUser: {user_input}\nJarvis: {reply}\n")
    return reply


# ==========================
# MAIN APP
# ==========================
class ChatScreen(BoxLayout):
    status_text = StringProperty("")
    status_color = ColorProperty((1, 1, 1, 1))

    def __init__(self, **kwargs):
        super().__init__(orientation="vertical", **kwargs)
        Window.size = (750, 700)

        # ===== TOP HEADER =====
        header = BoxLayout(size_hint=(1, 0.12), padding=[10, 5], spacing=10)
        with header.canvas.before:
            Color(0.1, 0.1, 0.1, 1)  # Dark gray background for header
            self.header_rect = Rectangle(size=header.size, pos=header.pos)
        header.bind(size=self._update_header_rect, pos=self._update_header_rect)

        try:
            logo_top = Image(source="logo.png", size_hint_x=0.15)
        except Exception:
            logo_top = Label(text="🤖", font_size=36, size_hint_x=0.15)

        title = Label(
            text="[b]Jarvis AI Assistant[/b]",
            markup=True,
            font_size=22,
            color=(1, 1, 1, 1),
        )

        header.add_widget(logo_top)
        header.add_widget(title)
        self.add_widget(header)

        # ===== CHAT SCREEN =====
        self.scroll = ScrollView(size_hint=(1, 0.7))
        self.chat_layout = GridLayout(cols=1, spacing=10, size_hint_y=None, padding=10)
        self.chat_layout.bind(minimum_height=self.chat_layout.setter("height"))
        self.scroll.add_widget(self.chat_layout)
        self.add_widget(self.scroll)

        # ===== STATUS BAR =====
        self.status_label = Label(
            text=self.status_text,
            color=self.status_color,
            font_size=15,
            size_hint=(1, 0.05),
        )
        self.add_widget(self.status_label)

        # ===== BOTTOM BAR (input + buttons) =====
        bottom_bar = BoxLayout(size_hint=(1, 0.15), spacing=10, padding=[10, 5])
        with bottom_bar.canvas.before:
            Color(0.1, 0.1, 0.1, 1)  # Match header dark color
            self.bottom_rect = Rectangle(size=bottom_bar.size, pos=bottom_bar.pos)
        bottom_bar.bind(size=self._update_bottom_rect, pos=self._update_bottom_rect)

        self.entry = TextInput(
            hint_text="Type your message...",
            multiline=False,
            font_size=16,
            size_hint_x=0.55,
            background_color=(0.15, 0.15, 0.15, 1),
            foreground_color=(1, 1, 1, 1),
            cursor_color=(1, 1, 1, 1),
        )
        self.entry.bind(on_text_validate=self.send_message)

        send_btn = Button(
            text="Send",
            size_hint_x=0.1,
            background_color=(0.2, 0.4, 1, 1),
            color=(1, 1, 1, 1),
            on_release=self.send_message,
        )

        clear_btn = Button(
            text="🧹 Clear",
            size_hint_x=0.1,
            background_color=(1, 0, 0, 1),
            color=(1, 1, 1, 1),
            on_release=self.clear_chat,
        )

        speak_btn = Button(
            text="🎙️ Speak",
            size_hint_x=0.1,
            background_color=(0.8, 0.2, 0.2, 1),
            color=(1, 1, 1, 1),
            on_release=self.toggle_speak,
        )

        listen_btn = Button(
            text="🎧 Listen",
            size_hint_x=0.1,
            background_color=(0.2, 0.6, 0.2, 1),
            color=(1, 1, 1, 1),
            on_release=self.voice_input,
        )

        bottom_bar.add_widget(self.entry)
        bottom_bar.add_widget(send_btn)
        bottom_bar.add_widget(clear_btn)
        bottom_bar.add_widget(speak_btn)
        bottom_bar.add_widget(listen_btn)
        self.add_widget(bottom_bar)

        # Initial message
        self.add_message("🤖 Hello! I'm Jarvis. Ask me anything.", "bot")

    def _update_header_rect(self, instance, value):
        self.header_rect.pos = instance.pos
        self.header_rect.size = instance.size

    def _update_bottom_rect(self, instance, value):
        self.bottom_rect.pos = instance.pos
        self.bottom_rect.size = instance.size

    @mainthread
    def update_status(self, text, color=(1, 1, 1, 1)):
        self.status_label.text = text
        self.status_label.color = color

    @mainthread
    def add_message(self, text, sender):
        label = Label(
            text=f"[b]{'You' if sender=='user' else 'Jarvis'}:[/b] {text}",
            markup=True,
            size_hint_y=None,
            text_size=(Window.width * 0.9, None),
            color=(1, 1, 1, 1),
        )
        label.bind(texture_size=label.setter("size"))
        self.chat_layout.add_widget(label)
        self.scroll.scroll_y = 0

    def send_message(self, instance=None):
        user_input = self.entry.text.strip()
        if not user_input:
            return
        self.add_message(user_input, "user")
        self.entry.text = ""

        def run_ai():
            reply = ai_process(user_input)
            self.add_message(reply, "bot")
            speak(reply, lambda t: self.update_status(t, (0, 1, 0, 1)))

        threading.Thread(target=run_ai, daemon=True).start()

    def voice_input(self, instance=None):
        def run_voice():
            self.update_status("🎧 Listening...", (0, 0.7, 1, 1))
            text = listen()
            self.update_status("")
            if text:
                self.add_message(text, "user")
                reply = ai_process(text)
                self.add_message(reply, "bot")
                speak(reply, lambda t: self.update_status(t, (0, 1, 0, 1)))
            else:
                self.add_message("⚠️ Sorry, I didn’t catch that.", "bot")

        threading.Thread(target=run_voice, daemon=True).start()

    def clear_chat(self, instance):
        with open(FILE, "w") as f:
            f.write("")
        self.chat_layout.clear_widgets()
        self.add_message("🧠 Chat cleared.", "bot")

    def toggle_speak(self, instance):
        global speak_enabled
        speak_enabled = not speak_enabled
        instance.text = "🎙️ Speak: ON" if speak_enabled else "🎙️ Speak: OFF"
        instance.background_color = (0.1, 0.7, 0.1, 1) if speak_enabled else (0.8, 0.2, 0.2, 1)


class JarvisApp(App):
    def build(self):
        self.title = "Jarvis AI Assistant"
        return ChatScreen()


if __name__ == "__main__":
    JarvisApp().run()

          </code></pre>
        </div>
      </div>
    </section>

    <section id="skills" style="margin-top:18px">
      <h2>Skills</h2>
      <div style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap">
        <div class="card" style="padding:10px">Learning Python</div>
      </div>
    </section>

    <section id="contact" style="margin-top:18px">
      <h2>Contact</h2>
      <div class="card" style="margin-top:10px">
        <p class="muted">Want changes, more projects, or help deploying? Send your message below (this demo form does not actually send).</p>
        <form onsubmit="event.preventDefault();alert('Thanks! (demo contact)')">
          <input type="text" placeholder="Your name" style="width:100%;padding:8px;margin-bottom:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)" required>
          <input type="email" placeholder="Email" style="width:100%;padding:8px;margin-bottom:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)" required>
          <textarea placeholder="Message" style="width:100%;padding:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)" required></textarea>
          <div style="margin-top:8px"><button class="btn">Send (demo)</button></div>
        </form>
      </div>
    </section>

  </main>

  <footer>
    <div class="container" style="display:flex;justify-content:space-between;align-items:center">
      <div>&copy; <span id="year"></span> Keshav</div>
      <div class="muted">Built with ❤️ — edit freely.</div>
    </div>
  </footer>

  <script>
    document.getElementById('year').textContent = new Date().getFullYear();

    function showOutput(){
      const demo = `User: Hey Jarvis, what's the time?
Jarvis: The time is 10:42 AM.
User: Open calculator.
Jarvis: Opening calculator...`;
      document.getElementById('demoOutput').textContent = demo;
    }

    function downloadCode(){
      const code = `# jarvis.py - example (simplified)
import speech_recognition as sr
import pyttsx3

engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()

if __name__ == \"__main__\":
    print(\"Jarvis demo starting...\")
    speak(\"Hello, I am Jarvis. How can I help?\")
`;
      const blob = new Blob([code], {type: 'text/x-python'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'jarvis.py';
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    }
  </script>
</body>
</html>
