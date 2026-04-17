# AbraFlexi - Banka - Chybejici doklady

Automaticky identifikuje bankovni transakce v ABRA Flexi, ke kterym chybi doklady (ucet 395001 — neidentifikovane platby). Vysledky zapise do Google Sheetu a odesle email klientovi.

## Co scenar dela

```
Schedule Trigger (kazda streda 5:00)
  -> Config Variables (Flexi URL, klient, email, Google Sheet)
    -> Clear Google Sheet (vymaze stary obsah)
      -> Flexi API: zjisti pocet zaznamu
        -> Generovani stranek (paginace po 5000)
          -> Flexi API: stazeni dat (banka, ucet 395001)
            -> Transformace dat (extrakce poli)
              -> Formatovani vystupu (datum CZ, castky, dodavatel)
                -> Zapis do Google Sheetu
                  -> Odeslani emailu s odkazem
```

## Konfigurace

Po importu upravte node **Config Variables**:

| Promenna | Popis | Priklad |
|----------|-------|---------|
| `client_name` | Nazev ucetni jednotky v ABRA Flexi (cast URL) | `moje-firma` |
| `nazev_klienta` | Nazev klienta pro predmet emailu | `Moje Firma s.r.o.` |
| `google_sheet_link` | URL vaseho Google Sheetu | `https://docs.google.com/spreadsheets/d/.../edit` |
| `send_to` | Email prijemcu (vice oddelene carkou) | `klient@firma.cz, ucetni@firma.cz` |

## Potrebne credentials

V n8n musite vytvorit tyto credentials:

| Typ | Node | Poznamka |
|-----|------|----------|
| **HTTP Basic Auth** | Flexi - Get Row Count, Flexi - Fetch Page | Vase Flexi API prihlaseni (uzivatel + heslo) |
| **Google Sheets OAuth2** | Clear_google_sheet, Append row in sheet | Google Workspace ucet s pristupem k Sheetu |
| **Gmail OAuth2** | Send a message | Google Workspace ucet pro odesilani emailu |

## Jak to funguje

1. **Paginace** — scenar nejdrive zjisti celkovy pocet zaznamu a rozlozi je do stranek po 5000
2. **Filtr** — stahuje pouze transakce s `protiUcet = 395001` (neidentifikovane platby)
3. **Transformace** — extrahuje dodavatele z popisu transakce (vcetne plateb kartou)
4. **Vystup** — serazeno podle castky od nejvetsi, cesky format datumu

## Vystupni sloupce v Google Sheetu

| Sloupec | Popis |
|---------|-------|
| Datum transakce | Datum ve formatu `D.M.YYYY` |
| Castka CZK | Absolutni hodnota v CZK |
| Castka v mene | Castka v cizi mene (pokud existuje) |
| Variabilni symbol | VS transakce |
| Dodavatel sluzby | Extrahovany nazev dodavatele |
| Popis transakce | Puvodni popis z banky |
| Odpovedna osoba | Prazdne — k doplneni klientem |

## Prizpusobeni

- **Filtr uctu**: Zmente `395001` v URL na jiny ucet dle potreby
- **Frekvence**: Upravte Schedule Trigger (default: kazda streda v 5:00)
- **Email sablona**: Upravte HTML v node "Send a message"
- **Vystupni pole**: Upravte Code nody pro jine sloupce
