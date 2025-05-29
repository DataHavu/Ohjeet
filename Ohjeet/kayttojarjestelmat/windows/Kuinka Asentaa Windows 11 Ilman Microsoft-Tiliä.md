---
featured_media: https://datahavu.fi/wp-content/uploads/2025/04/windows-11-asennus-microsoft-tili.jpg
id: 4743
julkaistu: true
kategoriat:
- kayttojarjestelmat
- windows
tags:
- asennus
title: Kuinka Asentaa Windows 11 Ilman Microsoft-Tiliä
---

## Johdanto

Tässä ohjeessa kerromme, kuinka voit asentaa Windows 11 -käyttöjärjestelmän ilman Microsoft-tiliä.

---

## Esivaatimukset

-  **Windows 11 -asennusmedia**: Tarvitset USB-muistitikun tai DVD-levyn, johon on luotu Windows 11:n asennusmedia.

- **Yhteensopiva tietokone**: Varmista, että tietokoneesi täyttää Windows 11:n [minimivaatimukset](https://www.microsoft.com/fi-fi/windows/windows-11-specifications?r=1)

- **Perustiedot Windowsin asennuksesta**: Ohjeessa käsitellään vain Microsoft-tilin ohittamista, ei koko asennusprosessia.


---

  
## Vaiheittaiset ohjeet


### Vaihe 1: Seuraa Windows 11 asennusprosessia

Seuraa Windows 11 asennusprosessia, kunnes pääset Microsoft-tilin kirjautumis kohtaan.

![](../../../Kuvat/Pasted%20image%2020250529152837.png)

### Vaihe 2: Paikallisen tilin luonti

Paina `Shift + F10`, jotta saat auki komentokehotteen.

Kirjoita komentokehotteeseen `start ms-cxh:localonly`

![](../../../Kuvat/Pasted%20image%2020250529153409.png)

Kirjoita käyttäjänimi ja halutessasi salasana. Näytän myöhemmin, kuinka voit lisätä salasanan ilman, että sinun tarvitsee määrittää suojauskysymyksiä. Paina lopuksi "Seuraava".

![](../../../Kuvat/Pasted%20image%2020250529153607.png)

Seuraa seuraavaksi Windowsin asennusohjetta, kunnes pääset työpöydälle. Olet nyt onnistuneesti luonut paikallisen tilin. Näytän vielä kuinka voit asettaa tilille salasanan.
![](../../../Kuvat/Pasted%20image%2020250529154037.png)

### Vaihe 3: Paikalliselle tilille salasana (Valinnainen)

Paina `CTRL ALT DELETE` ja valitse "Vaihda Salasanaa".

![](../../../Kuvat/Pasted%20image%2020250529154344.png)

Jätä vanha salasana tyhjäksi, jos sitä ei ole ja lisää uusi salasanasi, jonka jälkeen paina `Enter`.

![](../../../Kuvat/Pasted%20image%2020250529154717.png)

---

## VANHA ASENNUSOHJE, MIKÄ EI ENÄÄ TOIMI UUDEMMISSA VERSIOISSA
### Vaihe 1: Seuraa Windows 11 asennusprosessia

Seuraa Windows 11 asennusprosessia, kunnes pääset kohtaan **"Onko tämä oikea maa tai alue"**.

  ![](https://datahavu.fi/wp-content/uploads/2025/04/Pasted-image-20250413115433.png)

### Vaihe 2: Internet-vaatimuksen poistaminen

Seuraavaksi poistamme Windowsin asennuksen aikana vaadittavan internet-yhteyden, jotta asennus voi jatkua ilman verkoyhteyttä.

#### 2.1 Internet-yhteyden katkaiseminen

  Ensin katkaisemme internet-yhteyden, jotta voimme suorittaa seuraavassa vaiheessa olevan `oobe\bybassnro` komennon. **Jos käytät verkkokaapelia, voit myös irrottaa johdon koneesta ja ohittaa tämän kohdan.**
  

Paina `Shift + F10`, jotta saat auki komentokehotteen.

![](https://datahavu.fi/wp-content/uploads/2025/04/Pasted-image-20250413121104.png)


Kirjoita `ipconfig /release` ja paina enter, katkaistaksesi internet-yhteyden.

![](https://datahavu.fi/wp-content/uploads/2025/04/Pasted-image-20250413125912.png)

#### 2.1 Internet vaatimuksen poistaminen

Seuraavaksi ohitamme Windowsin asennuksen yhteydessä vaadittavan internet-yhteyden, jolloin asennus voi jatkua ilman verkoyhteyttä.

Sulje ja avaa komentokehote uudestaan kirjoita `oobe\bybassnro` ja paina enter.

![](https://datahavu.fi/wp-content/uploads/2025/04/Pasted-image-20250413130009.png)

Tämän jälkeen tietokoneesi käynnistyy uudelleen.

#### 2.2 Internet-yhteyden katkaiseminen taas
Jos irrotit verkkokaapelin, voit ohittaa tämän kohdan.

Suorita aikaisemmin 2.1 kohdassa näytetty `ipconfig /release` komento uudestaan ja sulje komentokehote.

### Vaihe 3: Windowsin asennus

Seuraa Windowsin asennusohjetta, kunnes pääset kohtaan **"Muodostetaan yhteys verkkoon"**.

Valitse **"Minulla ei ole Internet-yhteyttä"**

![](https://datahavu.fi/wp-content/uploads/2025/04/win11-muodostetaan-verkkoyhteys.png)

Tämän jälkeen voit jatkaa Windowsin asennusta normaalisti, kunnes saat Windowsin asennettua.

![](https://datahavu.fi/wp-content/uploads/2025/04/Pasted-image-20250413131530.png)

---

## UKK (Usein kysytyt kysymykset)

<details> <summary>Miksi haluaisin käyttää paikallista tiliä Windows 11:ssä?</summary> Paikallisen tilin käyttö Windows 11:ssä voi tarjota enemmän yksityisyyttä ja hallintaa, koska se ei vaadi yhteyksiä Microsoftin pilvipalveluihin. Se voi myös olla hyödyllinen, jos et halua synkronoida tietojasi Microsoft-tilin kautta tai jos käytät laitetta ilman internetyhteyttä.</details> <details> <summary>Voinko vaihtaa paikallisen tilin Microsoft-tiliksi myöhemmin?</summary> Kyllä, voit myöhemmin lisätä Microsoft-tilin ja siirtyä käyttämään sitä paikallisen tilin sijaan. </details>
<details>
  <summary>Mikä ero on paikallisella tilillä ja Microsoft-tilillä?</summary>
  Paikallinen tili on sidottu vain yhteen laitteeseen ja sen tiedot tallennetaan vain kyseiselle laitteelle, kun taas Microsoft-tili on pilvipohjainen ja mahdollistaa asetusten, tiedostojen ja sovellusten synkronoinnin eri laitteiden välillä sekä pääsyn Microsoftin palveluihin, kuten OneDriveen ja Officeen.
</details>
<details> <summary>Voinko käyttää kaikkia Windows 11:n ominaisuuksia ilman Microsoft-tiliä?</summary>Useimmat perusominaisuudet toimivat ilman Microsoft-tiliä, mutta tietyt toiminnot, kuten OneDrive ja Microsoft Store, vaativat Microsoft-tilin. </details>

  
---

## Yhteenveto

Tässä ohjeessa opit asentamaan Windows 11:n käyttämällä paikallista tiliä, ilman tarvetta Microsoft-tilille.

---