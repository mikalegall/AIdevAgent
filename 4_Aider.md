Varmista, että Podman on käynnissä, koska Aider käynnistetään Podman-konttiin.
<br/>
Jos Ollama GUI on käynnissä, sammuta se, ja käynnistä Ollama komentoriviltä, niin näet lokit
<br/>
`ollama serve`
<br/><br/>
Käynnistä Visual Code projektin juuressa kirjoittamalla kansion murupolun osoiteriville `cmd` ja avautuvassa komentokehoitteessa komenna 
<br/>
`code .`
<br/>
Avautuvan Visual Coden terminaalissa (cmd) käynnistä Aider komennolla
<br/>
`npm run aider`
<br/>
Koneen uudelleen käynnistyessä ip-osoite muuttuu. Mikäli koneelle ladatun kielimallin noutaminen ei Aiderilla kontista käsin Ollamasta onnistu, tarkista nykyinen ip-osoite ja muokkaa se `package.json` tiedostossa kohdassa `--add-host=host.ollama:`
<br/><br/>
Käynnistyksen jälkeen muokkaa ainoastaan alla olevan PROMPT mallipohjan TASK osiota ja poista rivinvaihdot ennen kuin annat sen Visual Coden terminaalissa Aiderille.
<br/>
##PROMPT
<br/>
```
You are the Architect in Aider’s architect mode (editor-model exists); Aider already has and must follow README.md, .aider.conf.yml, and .aider.instructions.md as authoritative context; DO NOT write any code; DO NOT restate the plan in prose; Generate all required file paths under yourself based on the task, the user will not provide file names;  For each required file you MUST specify: exact FILE path, ACTION (CREATE or MODIFY), PURPOSE (1–2 lines), EXPORTS (names + responsibilities/types), INTERNAL_STRUCTURE (functions/types/constants + key behavior constraints), and EDITOR_IMPLEMENTATION_NOTES (step-by-step exact edits/imports/exports/wiring the Editor must perform);

TASK: Implement Redux Store + RTK Query infrastructure per project rules: Axios-based RTK Query baseQuery, baseApi via createApi with empty injectable endpoints, store configured with baseApi reducer+middleware and exported RootState/AppDispatch, typed hooks useAppDispatch/useAppSelector;

You MUST assume Strict TypeScript (NO any), arrow functions only, javadoc style documentation, descriptive naming, and all project architecture rules apply; Output ONLY this machine-parsable format with no extra text: ARCHITECT_PLAN || FILE: || ACTION:CREATE|MODIFY || PURPOSE:<...> || EXPORTS:<...> || INTERNAL_STRUCTURE:<...> || EDITOR_IMPLEMENTATION_NOTES:<...> || (repeat FILE block for every file) || END_ARCHITECT_PLAN
```
<br/>

Kun olet katselmoinut, anna samalla logiikalla seuraava TASK eli copy+paste sitä ympäröivä alku- ja loppuosuus ja sen jälkeen yhdistä ne yksiriviseksi ennen Aiderille antamista
<br/>
```
TASK: Update only src/main.tsx to connect the already-implemented Redux store to the React application by importing Provider from react-redux, importing the existing exported store from its current module (use the project’s existing store file/export, do not create new store code), and wrapping the root render with while preserving all existing render/bootstrap logic and strict TypeScript constraints;
```
<br/>

Kun frontend arkkitehtuurin perusinfrastruktuuri on pystytetty, luo Penpot leiskoista FE ja BE väliset REST-APIt ja dokumentoi ne tiedostoon `docs/api-spec.md`
<br/><br/>
Jatka kehitystä feature komponentteihin

