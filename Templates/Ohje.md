---
title: <% tp.file.title %>
kategoriat: <%* 
  const path = tp.file.path(true);
  const folders = path.split('/');
  folders.shift(); // Remove the first folder
  folders.pop(); // Remove the file name
  tR += "\n"
  folders.forEach((folder) => { 
    tR += `  - ${folder}\n`; 
  });
  %>
tags: 
featured_media: 
julkaistu: false
---
## Johdanto

Tässä osiossa annetaan lyhyt yleiskuvaus siitä, mitä tämä ohje käsittelee ja sen tarkoituksesta.

---

## Esivaatimukset

Listaa tarvittavat työkalut, riippuvuudet tai taidot, jotka tarvitaan ennen aloittamista.

  

- Vaatimus 1

- Vaatimus 2

- Vaatimus 3

  

---

  
## Vaiheittaiset ohjeet

  

### Vaihe 1: [Vaiheen otsikko]

Kuvaus vaiheesta.

  

```bash

# Esimerkkikomento

komento --vaihtoehto arvo

```

  

**Odottava tulos:**

```bash

Odottava järjestelmän vastaus tai tulos

```

#### **Alavaihe 1.1: [Vaiheen otsikko]**

  Kuvaus vaiheesta.

  

```python

# Esimerkki Python-koodista

print("Hei, maailma!")

```

### **Vaihe 2: [Vaiheen otsikko]**

Kuvaus vaiheesta.

  

```python

# Esimerkki Python-koodista

print("Hei, maailma!")

```

  

**Odottava tulos:**

```bash

Hei, maailma!

```

  

---

  

## Vianmääritys

<details>
  <summary>Ongelma 1: [Virheilmoitus tai ongelma]</summary>
 Syy Selitys, miksi tämä tapahtuu.
 
 Ratkaisu: Vaiheet ongelman korjaamiseksi.
</details>
<details>
  <summary>Ongelma 2: [Virheilmoitus tai ongelma]</summary>
 Syy Selitys, miksi tämä tapahtuu.
 
 Ratkaisu: Vaiheet ongelman korjaamiseksi.
 </details>
  

---

  

## UKK (Usein kysytyt kysymykset)

<details> <summary>Kysymys 1: [Usein kysytty kysymys]</summary> Vastaus kysymykseen. </details> <details> <summary>Kysymys 2: [Usein kysytty kysymys]</summary> Vastaus kysymykseen. </details>

  

---

  

## Yhteenveto

Tiivistä tärkeimmät kohdat ja mitä käyttäjän pitäisi olla saavuttanut.

  

---

  

## Lisäresurssit

- [Linkki 1 - Virallinen dokumentaatio](#)

- [Linkki 2 - Liittyvä ohje](#)
