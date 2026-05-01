# Vilhon konfirmaatiovieraskirja

Staattinen GitHub Pages -sivu, jossa vieraat voivat jättää tervehdyksen Vilholle. Sivu toimii ensin paikallisella esikatselutallennuksella selaimen `localStorage`-muistissa. Kun lisäät Firebase-asetukset, tervehdykset tallentuvat Firestore-tietokantaan.

## Firebase-käyttöönotto

1. Luo projekti Firebase Consolessa.
2. Luo Web App ja kopioi sen `firebaseConfig`.
3. Ota Firestore Database käyttöön.
4. Korvaa tiedoston `index.html` `firebaseConfig`-kohdan tyhjät arvot omilla arvoillasi.
5. Julkaise repo GitHub Pagesiin.

## Firestore-säännöt

Alla oleva sääntö sallii tervehdyksen lisäämisen ja lukemisen, mutta ei muokkaamista tai poistamista selaimesta. Vaihda päivämäärä juhlan jälkeen tiukemmaksi tai poista kirjoitusoikeus kokonaan.

```txt
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /greetings/{greetingId} {
      allow read: if true;
      allow create: if
        request.time < timestamp.date(2026, 12, 31) &&
        request.resource.data.keys().hasOnly(['name', 'message', 'createdAt']) &&
        request.resource.data.name is string &&
        request.resource.data.name.size() > 0 &&
        request.resource.data.name.size() <= 80 &&
        request.resource.data.message is string &&
        request.resource.data.message.size() > 0 &&
        request.resource.data.message.size() <= 900 &&
        request.resource.data.createdAt == request.time;
      allow update, delete: if false;
    }
  }
}
```

## GitHub Pages

GitHubissa: `Settings` -> `Pages` -> `Deploy from a branch` -> valitse branch ja `/root`. Et tarvitse build-komentoa.
