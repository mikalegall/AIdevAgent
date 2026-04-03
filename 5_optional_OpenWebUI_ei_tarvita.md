Edellisten kohtien 1-4 "air gap Sandboxin" höllennettyä: <b>Open WebUi paikallisesti asennettuna</b>
<br/><br/>
Lataa puhe-tekstiksi toiminnallisuus, jotta voit ohjata paikallista Ollama (paikallinen moottori joka pyörittää kielimallia) + Open WebUI (visuaalinen käyttöliittymä moottorille) kokoonpanoasi puheella ilman näppäimistön koskettelua
<br/>
https://www.ffmpeg.org/download.html
<br/>
--> Windows builds from gyan.dev
<br/>
--> ffmpeg-release-essentials.zip
<br/>
Purkaa paketti esim.
<br/>
`C:\Path_programs\`
<br/>
ja lisää ohjelma ympäristömuuttujiin
<br/>
https://youtu.be/r1AtmY-RMyQ?si=RMsMx-TkzBDkvixP&t=293
<br/>
Minulla se on
<br/>
`C:\Path_programs\ffmpeg-8.0.1-essentials_build\bin\`
<br/><br/>
To support symlinks on Windows, you either need to activate Developer Mode or to run Python as an administrator. In order to activate developer mode, see this article
<br/>
https://learn.microsoft.com/en-us/windows/advanced-settings/developer-mode
<br/><br/>
Lataa Python ja asennuksessa rastita ruutu: "Add python.exe to PATH"
<br/>
https://www.python.org/downloads/windows/
<br/><br/>
Luo virtuaaliympäristö
<br/>
`python -m venv venv`
<br/>
Aktivoi Pythonin virtuaaliympäristö Windowsilla
<br/>
`.\venv\Scripts\activate`
<br/><br/>
Varmista, että Ollama on käynnissä
<br/><br/>
Asenna
<br/>
`pip install open-webui`
<br/>
Käynnistä
<br/>
```
set USER_AGENT=MyOpenWebUI
open-webui serve
```

<br/>

http://localhost:8080
<br/>
Admin tunnuksen luomisen jälkeen vasemmasta alakulmasta löytyy käyttäjäkuvake, jonka takaa
<br/>
`Settings`
<br/>
ja avautuvasta ikkunasta vasemmasta alakulmasta Admin Settings
<br/>
-> Connections
<br/>
Ohjaa Ollama API
<br/>
http://localhost:11434
<br/>
ja passivoi OpenAI API
<br/><br/>
Käytä Open WebUIta
<br/>
ideointiin, kysymyksiin ja tekstin tuottamiseen esim. Mistral kielimallilla


_******************************************_
```
podman run -d --name open-webui \
  -p 3000:8080 \
  --add-host=host.ollama:127.0.0.1 \
  -e OLLAMA_BASE_URL=http://host.ollama:11434 \
  -v open-webui-data:/app/backend/data:Z \
  --cpus="4" \
  --memory="4g" \
  --memory-reservation="2g" \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```


