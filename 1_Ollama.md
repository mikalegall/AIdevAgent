Asenna Ollama paikallisesti ajattavien kielimallien moottoriksi
https://ollama.com/download
<br/><br/>
Aseta ympäristömuuttuja komentokehoitteessa (command prompt)
<br/>
`setx OLLAMA_API_BASE "http://localhost:11434"`
<br/><br/>
Bindaa Ollama vain localhostiin: Podman-kontit voivat käyttää APIa hostin kautta, mutta palvelu ei ole saavutettavissa ulkoisista verkkorajapinnoista (air-gap safe)
<br/>
`setx OLLAMA_HOST "127.0.0.1:11434" /m`
<br/><br/>
Käynnistä Ollama graafisena käyttöliittymänä ja muuta sen asetuksissa
* Cloud, Auto-download updates ja Expose Ollama to the Network passiiviseksi (disable)
* Context lenght esim. 32k tai riippuu läppärisi RAM suuruudesta
<br/>

Lataa koodaukseen tarvittavat DeepSeek kielimallit
* R1 senior Arkkitehdiksi pohdiskelemaan
    * Kotiläppärillä pienikokoinen malli
    `ollama pull hf.co/bartowski/deepseek-r1-distill-qwen-14b-GGUF:IQ4_XS`
* Qwen2.5 Editor-modeen eli koodari rooliin
    * Kotiläppärillä pienikokoinen malli
    `ollama pull hf.co/bartowski/Qwen2.5-Coder-14B-Instruct-GGUF:IQ4_XS`

<br/><br/>
<b>*** OPTIONAL (ei tarkastettu eikä testattu lähdetietoja) ***</b>
<br/>

Lataa tarvittaessa muitakin kielimalleja:
<br/>
Esim. ideointiin, kysymyksiin ja tekstin tuottamiseen
`ollama pull hf.co/bartowski/Mistral-7B-Instruct-v0.3-GGUF:IQ4_XS`
<br/>
tai sensuroimaton versio `ollama run dolphin-mistral`
<br/>
tai Backend tunkeutumistestaukseen logiikkavirheiden etsimiseen (vaatii 46.6GB RAM 😂)
`ollama pull hf.co/bartowski/DeepSeek-Coder-V2-Lite-Instruct-GGUF:IQ4_XS`
<br/>
ja BE hyökkäykseen
`ollama pull hf.co/bartowski/WhiteRabbitNeo-2.5-Qwen-2.5-Coder-7B-GGUF:IQ4_XS`
<br/>
tai Frontend tunkeutumistestaukseen DOM & Framework (mukaan lukien CORS, CSP, Redux ja Context manipulointi)
`ollama pull hf.co/bartowski/Qwen2.5-Coder-7B-Instruct-abliterated-GGUF:IQ4_XS`
<br/>
ja FE pentesting hyökkäykseen Yleislogiikka & Arkkitehtuuri
`ollama pull hf.co/bartowski/Meta-Llama-3.1-8B-Instruct-abliterated-GGUF:IQ4_XS`
<br/>
<b>*** OPTIONAL ***</b>
<br/><br/>

Jos tulee mahdolliseksi käyttää kielimalleja pilvestä, niin reititä ne kaikki kerralla yhden Gatewayn takaa eli [OpenRouter](https://openrouter.ai/docs/guides/routing/provider-selection).
<br/><br/>
SANASTOA
<br/><br/>
Instruct = Malli on ohjeistettu vastaamaan eli malli on jalostettu keskustelemaan. Ilman Instruct (tai chat) lisämäärettä kyseessä on Base-malli joka päättelee antaamastasi syötteestä miten tekstisi voisi jatkua. Jos kirjoitat sille "Mikä on Suomen pääkaupunki?", se saattaa jatkaa "ja mikä on sen asukasluku?". Tai jos sanot Perusmallille "Tee Python-funktio, joka laskee summan", se saattaa jatkaa "Tee JavaScript-funktio, joka laskee erotuksen. Tee C++-funktio, joka...". osa Ollaman suosituimmista malleista saattaa olla oletuksena Instruct-versioita, vaikka nimeen ei olisi sitä erikseen kirjoitettu, koska ne on tehty nimenomaan avustajiksi.
<br/><br/>
Abliterated = Riisuttu suojauksista eli malli ei kieltäydy vastaamasta vaikka pyytäisit jotain pikkutuhmaa (kuten esim. päästä tunkeutumaan sisään), jolloin se sopii hyvin crakkerointi-työkaluksi omaa koodia vastaan
<br/><br/>
GGUF = Ollaman tarvitsema tiedostoformaatti kielimallille, jotta sitä voidaan ajaa eri tavalla kuin pilvessä (VRAM), eli se osaa hyödyntää läppärin prosessoria (CPU) ja näytönohjaimen muistia (VRAM). Jos malli ei mahdu kokonaan näytönohjaimelle, GGUF sallii mallin "paloittelun" niin, että osa on GPU:lla ja osa keskusmuistissa (RAM).
<br/><br/>
70b = 70 miljardia parametria
<br/><br/>
Tiheä malli (Dense) = Jokainen sisääntuleva sana (token) kulkee jokaisen tehtaan työntekijän kautta. Kun kirjoitat kehotteeseen sanan "kissa", malli tekee laskutoimituksen kaikkien (esim. 70 miljardin) parametrinsa läpi ymmärtääkseen ja prosessoidakseen tuon yhden sanan. Ja se tekee saman uudestaan, kun se tuottaa seuraavan sanan. Tämä on uskomattoman laskentateho- ja muistisyöppöä, koska prosessorin pitää liikutella kaikkia (esim. 70 miljardia) numeroa jatkuvasti edestakaisin.
<br/><br/>
MoE-malli (Mixture of Experts) = Tehdas on jaettu osastoihin. Kun sana "kissa" tulee sisään, "reititin" ohjaa sen vain esimerkiksi 10 prosentin työntekijöistä luokse, jotka ovat erikoistuneet eläimiin ja kielioppiin. Yli 90 % parametreista lepää sen tokenin kohdalla. Siksi iso MoE-malli voi pyöriä paljon kevyemmällä laitteistolla nopeasti, vaikka sen kokonaiskoko gigatavuissa olisikin massiivinen.
<br/><br/>
Distilled = Tislaaminen tarkoittaa prosessia, jossa suuren mallin (opettaja) osaaminen siirretään pienempään malliin (oppilas). Jos näet mallin, jonka nimi on esimerkiksi Llama-3.1-8B-Distilled-from-70B, se tarkoittaa, että 8 miljardin parametrin mallia on opetettu 70-miljardisen mallin vastausten perusteella. Se on siis "pikkumalli, jolla on ison mallin käytöstavat".
<br/><br/>
Kvantisointi = Käytännössä tekoälymallin pakkaamista. Normaalisti kielimallin parametrit (ne miljardit kertoimet) on tallennettu 16-bittisinä liukulukuina (FP16). Yksi 16-bittinen luku vie 2 tavua tilaa. Jos haluat ajaa esimerkiksi 8 miljardin (8B) parametrin mallia, se vaatii puhtaasti matematiikalla:
8 000 000 000 parametria × 2 tavua = 16 Gigatavua (GB) RAM-muistia (pelkästään mallin lataamiseen, plus lisätilaa itse kontekstin prosessointiin).
Kvantisoinnissa mallin tarkkuutta pyöristetään karkeammaksi. Ne 16-bittiset numerot pakataan esimerkiksi 4-bittisiksi (INT4) numeroiksi. Se on vähän kuin pakkaisit häviöttömän FLAC-äänitiedoston 128 kbps MP3-tiedostoksi. Jotain hienovaraista nyanssia katoaa, mutta lopputulos on yhä erittäin käyttökelpoinen. Vaikutus RAM-käyttöön: Se sama 8B malli, joka pakataan 4-bittiseksi, käyttääkin enää noin 4–5 Gigatavua RAM-muistia.
<br/><br/>
LLM_nimi:671b-q2_K_M = Kielimallin nimen jälkeen tulee parametri tieto (montako miljardia) ja sen jälkeen
qN = N-bittinen pakkaus eli
q8: Kahdekasan bittinen pakkaus on lähes häviötön pakkaus, jokavie paljon muistia
q4–q6: Kultainen keskitie. Malli toimii lähes yhtä hyvin kuin alkuperäinen, mutta vie murto-osan tilaa. q4_K_M on usein suosituin valinta.
q2–q3: Erittäin rankka pakkaus. Malli mahtuu pieneen tilaan, mutta se voi alkaa "hallusinoida" tai menettää kielellistä tarkkuuttaan.
Lopussa oleva tunniste S, M, L tai K on menetelmä miten pakkaus on tehty eli
_K viittaa yleensä niin sanottuun älykkääseen "K-quants" -menetelmään, joka tarkoittaa, että pakkaus ei ole tasapaksua, vaan tärkeimmät osat mallin aivoista säilytetään tarkempina ja vähemmän tärkeitä osia puristetaan kovempaa.
_XS (Extra Small)
_S (Small): Pienempi ja nopeampi, hieman tyhmempi.
_M (Medium): Tasapainoinen (suositus).
_L (Large): Tarkempi, mutta raskaampi.
<br/><br/>
LLM_nimi:IQ2_M = "I-Quants", jossa kirjain I viittaa sanoihin Importance Matrix (tärkeysmatriisi). Tärkeät osat säilytetään tarkempina, ja vähemmän tärkeitä osia puristetaan kovempaa. Muutoin siis sama kuin "K-quants", mutta päätös siitä mikä on tärkeää tehdään eri tavalla. K-quantit perustuvat yleisiin oletuksiin kielimallien rakenteesta. Kehittäjät tietävät kokemuksesta, että tietyt kerrokset (kuten attention-kerrokset) ovat yleensä tärkeämpiä kuin toiset. I-quantit eivät oleta mitään. Tärkeysmatriisi luodaan ajamalla mallin läpi oikeaa tekstiä (calibration dataset) ennen pakkaamista. K-Quants on "valistunut arvaus". I-Quants on "mitattu fakta".
