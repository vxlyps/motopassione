# Motopassione

Sito dell'officina Motopassione, via del Pigneto 5G, Roma.

HTML e CSS statici. Nessun framework, nessun build step, nessuna libreria
esterna, zero JavaScript. Il carattere è Inter e sta dentro la cartella
`fonts/`, non viene da Google Fonts: quindi niente richieste a terzi,
niente cookie e niente banner. Si carica su GitHub Pages così com'è e
funziona subito.

## Cosa c'è dentro

    index.html                          la pagina principale
    restauri/ducati-250-match1.html     il racconto di un restauro
    style.css                           unico foglio di stile
    fonts/InterVariable.woff2           il carattere, caricato da qui
    img/                                le foto in webp
    CNAME.txt                           il dominio, da attivare a DNS pronto

Le icone non sono una libreria: sono disegnate a mano in uno sprite SVG
in cima a ogni pagina e richiamate con `<use>`.

## Da sistemare prima di mandarlo online

1. **Partita IVA.** Nel piede di pagina di tutte e due le pagine c'è
   scritto `DA INSERIRE`.
2. **Coordinate della mappa.** Nel JSON-LD di `index.html` le coordinate
   sono approssimate. Le esatte si prendono da Google Maps: tasto destro
   sul punto dell'officina, il primo valore è la latitudine.
3. **Indirizzo Facebook.** In `index.html` è messo
   `facebook.com/motopassione`, va controllato che sia quello giusto
   (compare due volte: nei contatti e nel JSON-LD).
4. **Foto della Ducati.** La pagina del restauro racconta il lavoro ma le
   foto della moto non ci sono. Nel file i posti sono già pronti,
   commentati, con dentro il nome del file che si aspettano: si salvano
   le foto in `img/` con quel nome e si tolgono i commenti.
5. **Foto più grandi.** Le foto di adesso arrivano dal vecchio sito e sono
   480x360 pixel, piccole per gli schermi di oggi. Quando ci sono gli
   originali conviene rifarle a 1600 pixel di lato lungo, riconvertirle in
   webp tenendo gli stessi nomi, e aggiornare i `width` e `height` scritti
   dentro ai tag `img`.

Gli orari sono già dentro, presi da Google Maps: da lunedì a venerdì
9:00-13:00 e 15:30-19:30, sabato e domenica chiuso. Se cambiano vanno
corretti in due posti, nella sezione "Dove siamo" e nel JSON-LD in fondo
alla pagina.

## Come si aggiunge un altro restauro

Si duplica `restauri/ducati-250-match1.html`, gli si dà un nome tipo
`restauri/yamaha-rd-350.html` e si cambiano solo i contenuti. Le
istruzioni riga per riga stanno in cima al file stesso, dentro un
commento. Poi si aggiunge il link alla pagina nuova in `index.html`,
nella sezione "Restauro moto d'epoca".

Struttura e grafica non si toccano: sta tutto in `style.css`, che è
diviso in sezioni numerate con i colori e le spaziature raccolti in cima.

## DNS su Aruba per puntare motopassione.eu a GitHub Pages

Si entra nel pannello Aruba, si apre il dominio `motopassione.eu` e si va
su **Gestione DNS e Redirect**.

**Record A** per il dominio senza www. Host vuoto (o `@`), quattro record,
uno per riga:

    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153

Se Aruba ha già un record A che punta al suo hosting, va sostituito con
questi quattro.

**Record CNAME** per il www:

    host: www        valore: NOMEUTENTE.github.io

(al posto di `NOMEUTENTE` va il nome dell'account GitHub che ospita il
repository, con il punto finale se il pannello lo richiede)

### Attenzione ai record MX

**Non toccare i record MX.** Sono quelli che fanno arrivare la posta di
`info@motopassione.eu`. Se si cancellano o si modificano, la casella
smette di ricevere. Vale anche per il record TXT dell'SPF, quello che
comincia con `v=spf1`: si lascia dov'è.

Si cambiano solo i record A e il CNAME del www. Tutto il resto resta
com'è.

### Dopo il DNS

Su GitHub, nelle impostazioni del repository, si va su **Pages**, si
mette `motopassione.eu` come dominio personalizzato e si spunta
**Enforce HTTPS** quando il certificato è pronto (ci vuole da qualche
minuto a qualche ora). In questa cartella c'è il file `CNAME.txt`, che contiene già
`motopassione.eu`. **Va rinominato in `CNAME`, senza estensione, solo
quando i record DNS sopra sono a posto.** Finché il DNS non punta a
GitHub, lasciarlo con l'estensione `.txt`: se lo si attiva prima, GitHub
manda tutti su motopassione.eu e il sito non si apre più nemmeno
dall'indirizzo github.io. Se si decide di usare `www.motopassione.eu`,
va scritto quello dentro al file.

La propagazione del DNS di Aruba di solito ci mette da un'ora a un giorno.
