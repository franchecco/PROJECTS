# Tweet to SVG <!-- omit in toc -->

---

## 🔠 Indice<!-- omit in toc -->

- [❓ Descrizione](#-descrizione)
- [🔀 Operazioni e spiegazione del programma](#-operazioni-e-spiegazione-del-programma)
  - [🌆 SVG](#-svg)
  - [💬 CSV](#-csv)
- [💬 Supporto](#-supporto)
- [👨‍💻 Autori](#-autori)

---

## ❓ Descrizione

**_Questo programma ha il compito di creare un file `.SVG` ([Immagine Vettoriale](https://it.wikipedia.org/wiki/Grafica_vettoriale#:~:text=L'immagine%20vettoriale%20%C3%A8%20una,attribuiti%20colori%20e%20anche%20sfumature.)) raffigurante un tweet, a partire dal ID del tweet in che si vuole trovare._**

- Un esempio di input preso da questo [tweet](https://twitter.com/elonmusk/status/1521807738317066240):

  > L'ID che si ricava dall URL del tweet `(twitter.com/elonmusk/status/`<span style="color:lightblue">1521807738317066240</span>`)` è questo:  
  > **<span style="color:lightblue">1521807738317066240</span>**
- L'output invece sarà simile a questo:

<img src="ReadmeImage.svg" height="50%" width="50%" style="  display: block;margin-left: auto;margin-right: auto;">

---

## 🔀 Operazioni e spiegazione del programma

<br>

### 🌆 SVG

Specifichiamo nel tag `SVG`, **che fa da contenitore all'intero file**, la grandezza dell'immagine, la versione del linguaggio `SVG` e il **namespace** del file che, anche se opzionale, aiuta a evitare conflitti tra nomi di file `SVG`.

```svg
<svg width="1000" height="500" version="1.1" xmlns="http://www.w3.org/2000/svg">
```

Creiamo un tag `<style>` dove saranno contenuti i `CSS` dell'immagine.

```svg
    <defs>
        <style type="text/css">
            rect{
                fill: black;
            }
            .centered_text{
                font-family: 'Inter', sans-serif;
                dominant-baseline:middle;
                text-anchor:middle;
                inline-size: 500px;
                overflow-wrap: break-word;
            }
            .like{
                fill: white;
            }
        </style>
    </defs>
```

Iniziamo a scrivere il corpo del programma che contiene tutti i testi e le immagini.  
Per ora inseriamo l' **username**, lo **screen-name**, i **likes** e i **retweet**, ovviamente applicando a ognuno dei `CSS` diversi in base al posizionamento, allo stile, etc; inoltre creiamo anche lo **sfondo nero del tweet con un tag `<rect>`**.

```svg
<!--!body-->
    <!--! BackGround -->
    <rect width="1000" height="500" x="0" y="0"/>
        <!--! Name -->
        <text x="50%" y="29%" font-size="48" font-weight="bold" fill="#E7E9EA" class="centered_text">
            Elon Musk
        </text>
        <!--! Screen_name -->
        <text x="50%" y="35%" fill="#72777C" font-size="24" class="centered_text">
                @elonmusk
        </text>
        <!--! Likes -->
        <text x="10%" y="10%" fill="white" font-size="24" font-weight="bold" class="centered_text">
            1110242
            <tspan fill="#72777C" font-weight="normal">
                Likes
            </tspan>
        </text>
        <!--! Retweets -->
        <text x="90%" y="10%" fill="white" font-size="24" font-weight="bold" class="centered_text">
            121099
            <tspan fill="#72777C" font-weight="normal">
                Retweets
            </tspan>
        </text>
```

Il testo del tweet ha bisogno di un attenzione particolare poichè **non è possibile mandare a capo automaticamente un testo in `SVG`**, quindi bisogna usare un tag `<tspan>` che crea una sezione di testo che poi spostiamo una riga sotto attraverso le coordinate.

```svg
<!--! Tweet -->
        <text x="50%" y="50%" font-size="36"  fill="#E7E9EA" class="centered_text">
            <tspan x="50%" dy="0em">
                The far left hates everyone, themselves
            </tspan>
            <tspan x="50%" dy="1.2em">
                 included!
            </tspan>
        </text>
```

**Creiamo l'immagine del cuore per il like attraverso un tag `<path>`** che crea una forma seguendo delle istruzioni di movimento; inoltre **inseriamo la forma all'interno di un contenitore col tag `<g>`**.  
A questo punto spostiamo il contenitore al centro della pagina e lo scaliamo a una dimensione adatta con l'attributo: `transform="translate(500 460) scale(0.50 0.50)"`; applichiamo anche l'attributo `transform="translate(-50 -50)"` direttamente alla forma per portarla al centro del contenitore.

```svg
<g transform="translate(500 460) scale(0.50 0.50)">
    <path transform="translate(-50 -50)" class="like" d="M92.71,7.27L92.71,7.27c-9.71-9.69-25.46-9.69-35.18,0L50,14.79l-7.54-7.52C32.75-2.42,17-2.42,7.29,7.27v0 c-9.71,9.69-9.71,25.41,0,35.1L50,85l42.71-42.63C102.43,32.68,102.43,16.96,92.71,7.27z">
    </path>
</g>
```

Aggiungiamo l'animazione del colore del cuore col tag `<animate>` e lo applichiamo al tag `<path>` . . .

```svg
<g transform="translate(500 460) scale(0.50 0.50)">
    <path transform="translate(-50 -50)" class="like" d="M92.71,7.27L92.71,7.27c-9.71-9.69-25.46-9.69-35.18,0L50,14.79l-7.54-7.52C32.75-2.42,17-2.42,7.29,7.27v0 c-9.71,9.69-9.71,25.41,0,35.1L50,85l42.71-42.63C102.43,32.68,102.43,16.96,92.71,7.27z">
        <animate
            attributeName="fill"
            values="white;red;red"
            dur="0.5s"
            repeatCount="1"
            begin="click"
            fill="freeze"
        />
    </path>
</g>
```

. . . mentre per l'animazione della dimensione utilizziamo il tag `</animateTransform>` nel tag `<g>`

```svg
<g transform="translate(500 460) scale(0.50 0.50)">
    <path transform="translate(-50 -50)" class="like" d="M92.71,7.27L92.71,7.27c-9.71-9.69-25.46-9.69-35.18,0L50,14.79l-7.54-7.52C32.75-2.42,17-2.42,7.29,7.27v0 c-9.71,9.69-9.71,25.41,0,35.1L50,85l42.71-42.63C102.43,32.68,102.43,16.96,92.71,7.27z">
        <animate
            attributeName="fill"
            values="white;red;red"
            dur="0.5s"
            repeatCount="1"
            begin="click"
            fill="freeze"
        />
    </path>
    <animateTransform
        attributeName="transform"
        type="scale"
        values="1;1.25;1;1;1"
        dur="0.5s"
        repeatCount="1"
        additive="sum"
        begin="click"
    />
</g>
```

Infine creiamo l'immagine del profilo dell'utente, **tuttavia per questo progetto ci serve un immagine circolare anche se quella fornita da twitter è spesso quadrata**, per questo usiamo il tag `<clipPath>` che crea una forma che andrà a tagliare in modo corretto l'immagine del profilo.

```svg
<!--! PFP -->
<circle cx="500" cy="60" r="50" fill="gray" />
<clipPath id="clipCircle">
    <circle r="50" cx="500" cy="60"/>
</clipPath>
<image href="http://pbs.twimg.com/profile_images/1503591435324563456/foUrqiEw_normal.jpg" height="100" width="100" x="450" y="10" clip-path="url(#clipCircle)" />
</svg>
```

<br>

### 💬 CSV

Il file `CSV` viene generato dal programma `Python`, è un formato di file estremamente chiaro e semplice, creato per raccogliere in modo ordinato dei dati:

```csv
data_creazione¬Fri Apr 29 12:28:10 +0000 2022
testo_completo¬The far left hates everyone, themselves included!
hashtags¬[]
symbols¬[]
user_mentions¬[]
client¬"<a href=""http://twitter.com/download/iphone"" rel=""nofollow"">Twitter for iPhone</a>"
nome¬Elon Musk
screen_nome¬elonmusk
verified¬True
profile_image_url¬http://pbs.twimg.com/profile_images/1503591435324563456/foUrqiEw_normal.jpg
numero_retweet¬121099
numero_like¬1110242
```

In questo esempio è usato come separatore tra i dati il carattere '¬' in modo da non incontrare problemi con la virgola _(simbolo standard per la separazione dei campi)_.

---

## 💬 Supporto

Nel caso sia necessario dell'supporto potete contattarci attraverso questi link:

| Badge                                                                                                                    | Link o Contatto                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| <img src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white" />               | [Invito](<[placeholder]() |
| <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" />                 | [Profilo]()                 |
| <img src="https://img.shields.io/badge/Stack_Overflow-FE7A16?style=for-the-badge&logo=stack-overflow&logoColor=white" /> | [Profilo]()                 |

---

## 👨‍💻 Autori

**Collaborazione al progetto:**
TODO:
| GitHub o Nome | Profilo |
| --------------- | ------------------------------ |
| Znaidi Rayan | rznaidi@fermimn.edu.it |
| Bini Matteo | mail@matteobini.me , binim@fermimn.edu.it |
| Tardiani Simone | stardiani@fermimn.edu.it |# PROJECTS
