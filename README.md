# Groepsbestelmodule Woongroep 3

Statische export voor GitHub Pages. Alle assets (fonts, logo's, design system, runtime) zitten in de HTML-bestanden zelf — geen build, geen externe requests.

## Publiceren
1. Commit de map `docs/` naar je repo (branch `main`).
2. Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/docs`.
3. De module staat daarna op `https://<gebruiker>.github.io/<repo>/`.

Wil je liever de root van de repo publiceren: kopieer de inhoud van `docs/` naar de root en kies folder `/ (root)`.

## Pagina's
| Bestand | Scherm |
|---|---|
| `index.html` | 1 · Keuzes per persoon / per dag |
| `bestellijst.html` | 2 · Bestellijst ingrediënten |
| `bevestiging.html` | 3 · Bestelling geplaatst |

`.nojekyll` staat erin zodat GitHub Pages de bestanden ongewijzigd serveert.
