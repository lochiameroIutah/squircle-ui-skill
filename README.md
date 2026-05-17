# squircle-ui

Una Claude Code skill per usare [`@squircle-js/react`](https://squircle.js.org) come default al posto di `border-radius` / `rounded-*` di Tailwind nelle UI React.

## Perché esiste

Credo che gli squircle potrebbero armonizzare di più la vita digitale con il mondo reale.

Gli angoli del software sono quasi tutti archi di cerchio — `border-radius` del CSS, che salta di colpo dal lato dritto alla curva (continuità G1). Nel mondo fisico, gli oggetti che maneggiamo ogni giorno — un sasso levigato dal fiume, una saponetta consumata, un iPhone — hanno curve che si raccordano in modo continuo (G2). Non spigoli, non transizioni nette: passaggi morbidi.

Apple lo sa dal 2013, quando ha sostituito gli arrotondamenti delle app icon iOS con una **superellipse**. Da allora ogni interfaccia premium che ti sembra "fatta bene" probabilmente sta usando squircle senza che tu te ne accorga.

Questa skill rende quel default automatico per chi costruisce UI con Claude Code.

## Cosa fa

Quando attiva, Claude usa `<Squircle>` / `<StaticSquircle>` con `cornerSmoothing: 0.6` (preset Apple/iOS) al posto di `rounded-xl`, `rounded-2xl` ecc. su bottoni, card, avatar, hero image, app icon.

Sono inclusi:
- API reale di `@squircle-js/react` (props, `asChild`)
- Scala consigliata di `cornerRadius` per ogni tipo di elemento
- 7 pattern copia-incollabili (bottone, card, avatar, hero, app icon iOS, Framer Motion, bordo)
- Regola anti-blob per elementi piccoli
- Tabella errori comuni
- Eccezioni dove restare su CSS standard (`rounded-full` per cerchi veri, input piccoli)

## Install

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/lochiameroIutah/squircle-ui-skill.git squircle-ui
```

Da quel momento Claude Code la scopre automaticamente.

## Forzare l'uso come default

La skill viene attivata se il tuo prompt contiene trigger come "squircle", "stile iOS", "Apple". Per renderla **default** anche quando dici solo "fammi una card", aggiungi un file di memoria personale in `~/.claude/projects/<tuo-progetto>/memory/` che istruisca Claude a invocarla al posto di `rounded-*`. (Esempio nel file `feedback_squircle_default.md` di esempio incluso.)

## License

MIT.
