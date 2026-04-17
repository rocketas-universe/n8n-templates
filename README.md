# Rocketas N8N Templates

Kolekce automatizacnich scenaru pro [n8n](https://n8n.io/) od [Rocketas](https://www.rocketas.io).

Scenare jsou zamerene na automatizaci ucetnich a financnich procesu v ceskem prostredi, predevsim integraci s **ABRA Flexi (FlexiBee)**.

## Dostupne sablony

| Sablona | Popis |
|---------|-------|
| [AbraFlexi - Chybejici doklady](templates/abra-flexi-chybejici-doklady/) | Identifikace bankovnich transakci bez sparovanych dokladu |

## Jak importovat do n8n

1. Stahnete `.json` soubor sablony
2. V n8n otevrete **Settings** (ozubene kolecko) > **Import from File**
3. Nahrajte stazeny JSON soubor
4. Upravte **Config Variables** node — vyplnte sve udaje (Flexi URL, klient, emaily)
5. Pridejte sve **credentials** (Flexi API, Google Sheets, Gmail)
6. Otestujte workflow rucne pred aktivaci Schedule Triggeru

## Pozadavky

- [n8n](https://n8n.io/) (self-hosted nebo cloud)
- [ABRA Flexi](https://www.flexibee.eu/) ucetni system
- Google Workspace ucet (pro Google Sheets a Gmail)

## Podpora

Tyto sablony jsou poskytovany jako inspirace a zakladni stavebni kameny. Kazdy scenar vyzaduje konfiguraci pro vase prostredi.

Pro profesionalni implementaci a automatizaci ucetnich procesu nas kontaktujte na [www.rocketas.io](https://www.rocketas.io).

## Licence

MIT
