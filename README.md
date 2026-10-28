# Pyryn lääkekalenteri

Yhteinen viikkokalenteri Pyry-koiran lääkkeille. Sivu toimii GitHub Pagesissa, ja merkinnät näkyvät kaikille reaaliajassa Firebase-tietokannan kautta.

| Lääke | Aamu | Päivä | Ilta |
|---|---|---|---|
| Omega3 | ✓ | | ✓ |
| Rimadyl | ✓ | | |
| Arthryl | ✓ | | |
| Neurontin (puolikas) | ✓ | ✓ | ✓ |

## Käyttö

- Napauta haaleaa lääkettä, kun olet antanut sen. Merkintä näkyy heti myös toisella puhelimella.
- Lääkkeen voi myös raahata alareunan hyllystä päivän kohdalle.
- Napauta merkittyä lääkettä, jos haluat perua merkinnän. Alareunan Kumoa-painike palauttaa sen.
- Kun kaikki viikon 49 annosta on merkitty, Pyry kiittää ja kalenteri tyhjenee uutta viikkoa varten.

## Tietokanta

Kalenteri käyttää Liekkiboardin Firebase-projektia polussa `pyry/annokset`. Säännöt hyväksyvät vain oikeat päivä-, aika- ja lääkeyhdistelmät, joten tietokantaan ei voi kirjoittaa muuta.

```json
"pyry": {
  ".read": true,
  "annokset": {
    ".write": true,
    "$id": {
      ".validate": "$id.matches(/^[0-6]-(aamu-(omega|rimadyl|arthryl|neurontin)|paiva-neurontin|ilta-(omega|neurontin))$/) && newData.hasChildren(['p', 'a', 'l', 't']) && newData.child('p').val() + '-' + newData.child('a').val() + '-' + newData.child('l').val() === $id",
      "p": { ".validate": "newData.isString()" },
      "a": { ".validate": "newData.isString()" },
      "l": { ".validate": "newData.isString()" },
      "t": { ".validate": "newData.isNumber()" },
      "$muu": { ".validate": false }
    }
  }
}
```

Lääkkeitä tai antoaikoja voi muuttaa `index.html`:n `AJAT`-kohdassa. Silloin myös sääntöjen tunnistekuvio pitää päivittää vastaamaan uusia yhdistelmiä.
