---
title: Kuinka lisätä WooCommerce tilauslomakkeeseen Y-tunnuskenttä
kategoriat:
  - ohjelmointi
  - php
tags:
  - wordpress
  - woocommerce
featured_media: https://datahavu.fi/wp-content/uploads/2024/04/y-tunnuskentta-kassasivulle.jpg
julkaistu: true
id: 3964
---
## Johdanto

Tässä ohjeessa kerromme, kuinka voit lisätä WooCommerce tilauslomakkeeseen Y-tunnuskentän käyttämällä PHP-ohjelmointikieltä.

Koko koodin löydät alhaalta.

---

## Esivaatimukset

- WordPress

- WPcode lisäosa (Ei pakollinen)
  

---

  
## Vaiheittaiset ohjeet

  

### Vaihe 1: Y-tunnuksen lisääminen kassalle

Ensin lisäämme Y-tunnuksen kassalle, jonka jälkeen lisäämme Y-tunnuskenttään tarkistuksen, joka tarkistaa, onko Y-tunnus oikein kirjoitettu.

#### 1.1 Y-tunnuskenttä kassalle

Ensin luomme Y-tunnuskentän tilauslomakkeelle.

![](https://datahavu.fi/wp-content/uploads/2024/04/y-tunnuskentta-kassasivulle.jpg "Kuinka lisätä WooCommerce tilauslomakkeeseen Y-tunnuskenttä")

Alla oleva koodi lisää Y-tunnuskentän tilauslomakkeelle.

```php
<?php

/*
* Lisää Y-tunnus kentän kassalle
*/
add_filter('woocommerce_checkout_fields', 'y_tunnus_kassalle');
function y_tunnus_kassalle($fields) {
    $kayttaja = wp_get_current_user();
    $tallennettu_y_tunnus = get_user_meta($kayttaja->ID, 'y_tunnus', true);
    $y_tunnus_kentta = array(
        'type'          => 'text', // Kentän tyyppi
        'label'         => __('Y-Tunnus', 'woocommerce'), // Otsikko
        'placeholder'   => _x('Y-tunnus', 'placeholder', 'woocommerce'), // Placeholder
        'required'      => false, // Jos haluat kentän olevan pakollinen, laita true ja jos et halua, laita false
        'class'         => array('form-row-wide'), // Kentän luokka
        'default'       => $tallennettu_y_tunnus, // Kentän oletusarvo
        'priority'      => 31, // Luvun pitää olla suurempi, kuin billing_company_field:in prioriteetti
    );

    $fields['billing']['y_tunnus'] = $y_tunnus_kentta; 

    return $fields;
}
```

#### 1.2 Y-tunnuksen tarkistus

Seuraavaksi tarkastamme, että Y-tunnus on oikein kirjoitettu.

Koodi tarkastaa:

- Onko Y-tunnus 9 merkkiä pitkä
- Onko Y-tunnuksessa tarpeeksi väliviivoja
- Sisältääkö Y-tunnus kirjaimia
- Onko Y-tunnuksen ensimmäisessä osassa 7 merkkiä ja toisessa osassa 1 merkki

```php
/*
* Tarkistaa, jos Y-tunnus on annettu ja se on oikeassa muodossa
*/
add_action('woocommerce_checkout_process', 'tarkista_y_tunnus');
function tarkista_y_tunnus() {

    $y_tunnus = $_POST['y_tunnus']; 

    if (!empty($y_tunnus)) 
    {       
        // Sanitize Y-tunnus
        $y_tunnus = sanitize_text_field($y_tunnus);

        if (strlen($y_tunnus) != 9) 
        {   
            // Liian vähän tai liian paljon merkkejä

            wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');
        }
        else
        {   
            // Y-tunnuksessa on oikea määrä merkkejä

            $y_tunnus_array = explode('-', $y_tunnus); // Erotetaan Y-tunnus ja tarkistusnumero


            if (count($y_tunnus_array) != 2) 
            {   
                // Y-tunnuksessa on liikaa tai liian vähän väliviivoja

                wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');

            }
            else
            {   
                // Y-tunnuksessa on oikea määrä väliviivoja

                if (!is_numeric($y_tunnus_array[0]) || !is_numeric($y_tunnus_array[1])) 
                {   
                    // Y-tunnuksessa on kirjaimia

                    wc_add_notice(__('Tarkista, että Y-tunnuksessa ei ole kirjaimia', 'woocommerce'), 'error');
                }
                else    
                {   
                    // Y-tunnuksessa ei ole kirjaimia

                    if (strlen($y_tunnus_array[0]) != 7 || strlen($y_tunnus_array[1]) != 1) 
                    {   
                        // Y-tunnuksen tunnus tai tarkistusnumero on väärän pituinen

                        wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');
                    }
                    else
                    {
                        // Y-tunnus läpäisi kaikki tarkistukset
     
                    }
                }
            }
        }
    }

}
```


Ohjelma antaa virheilmoituksen, jos Y-tunnus on väärin.  
  
Voit muokata virheilmoituksia koodissa haluamasi mukaan.

![](https://datahavu.fi/wp-content/uploads/2024/04/y-tunnus-virheilmoitus.jpg "Kuinka lisätä WooCommerce tilauslomakkeeseen Y-tunnuskenttä")

## Vaihe 2: Y-tunnuksen tallentaminen

Seuraavaksi tallennamme asiakkaan Y-tunnuksen asiakkaan ja tilauksen tietoihin.

#### 2.1 Y-tunnuksen tallentaminen tilaukseen ja käyttäjätietoihin

Seuraava koodi tallentaa asiakkaan Y-tunnuksen asiakkaan tilaukseen ja käyttäjätietoihin

```php
/*
* Tallentaa Y-tunnuksen tilaukseen ja käyttäjän tietoihin
*/
add_action( 'woocommerce_checkout_create_order', 'tallenna_y_tunnus', 10, 2 );
function tallenna_y_tunnus( $order, $data ) {

    $y_tunnus = isset( $_POST['y_tunnus'] ) ? $_POST['y_tunnus'] : '';

    if (!empty($y_tunnus)) 
    {   
        // Tallentaa Y-tunnuksen tilauksen tietoihin
        $y_tunnus = sanitize_text_field($y_tunnus);
        $order->update_meta_data('y_tunnus', $y_tunnus);

        // Tallentaa Y-tunnuksen käyttäjän tietoihin
        $kayttaja = wp_get_current_user();
        update_user_meta( $kayttaja->ID, 'y_tunnus', $y_tunnus);
    }
}
```

## Vaihe 3: Y-tunnuksen näyttäminen tilauksen tiedoissa

Seuraavaksi laitamme Y-tunnuksen näkyviin tilauksen tietoihin.

#### 3.1 Y-tunnuksen näkyminen tilauksen tiedoissa

![](https://datahavu.fi/wp-content/uploads/2024/04/Y-tunnusTilauksenTiedoissa.jpg.jpg "Kuinka lisätä WooCommerce tilauslomakkeeseen Y-tunnuskenttä")

Alla oleva koodi näyttää asiakkaan Y-tunnuksen tilauksen tiedoissa, jos tilauksessa on Y-tunnus

```php
/*
* Näyttää Y-tunnuksen tilauksen tiedoissa
*/
add_action( 'woocommerce_admin_order_data_after_billing_address', 'nayta_y_tunnus_tilauksen_muokkaus_sivulla', 10, 1 );
function nayta_y_tunnus_tilauksen_muokkaus_sivulla($order)
{
    $y_tunnus = $order->get_meta( 'y_tunnus', true );
    if (!empty($y_tunnus))  
    {   
        // Y-tunnus ei ole tyhjä
        // Näyttää Y-tunnuksen tilauksen tiedoissa

        $otsikko = "Y-tunnus";
        echo '<p><strong>'.  __($otsikko, 'woocommerce') .':</strong> ' . esc_html($y_tunnus) . '</p>';
    } 
}
```


Nyt voit testata Y-tunnusta tekemällä uuden tilauksen. Tilauksen tehtyäsi sinun pitäisi nähdä Y-tunnus tilauksen tiedoissa, jos menet Admin-paneeliin -> WooCommerce -> Tilaukset -> Valitse sinun tekemäsi tilaus. 

## 4 . Koko koodi

Tässä on vielä projektin koko koodi.

```php
<?php


/*
* Lisää Y-tunnus kentän kassalle
*/
add_filter('woocommerce_checkout_fields', 'y_tunnus_kassalle');
function y_tunnus_kassalle($fields) {
    $kayttaja = wp_get_current_user();
    $tallennettu_y_tunnus = get_user_meta($kayttaja->ID, 'y_tunnus', true);
    $y_tunnus_kentta = array(
        'type'          => 'text', // Kentän tyyppi
        'label'         => __('Y-Tunnus', 'woocommerce'), // Otsikko
        'placeholder'   => _x('Y-tunnus', 'placeholder', 'woocommerce'), // Placeholder
        'required'      => false, // Jos haluat kentän olevan pakollinen, laita true ja jos et halua, laita false
        'class'         => array('form-row-wide'), // Kentän luokka
        'default'       => $tallennettu_y_tunnus, // Kentän oletusarvo
        'priority'      => 31, // Luvun pitää olla suurempi, kuin billing_company_field:in prioriteetti
    );

    $fields['billing']['y_tunnus'] = $y_tunnus_kentta; 

    return $fields;
}

/*
* Tarkistaa, jos Y-tunnus on annettu ja se on oikeassa muodossa
*/
add_action('woocommerce_checkout_process', 'tarkista_y_tunnus');
function tarkista_y_tunnus() {

    $y_tunnus = $_POST['y_tunnus']; 

    if (!empty($y_tunnus)) 
    {       
        // Sanitize Y-tunnus
        $y_tunnus = sanitize_text_field($y_tunnus);

        if (strlen($y_tunnus) != 9) 
        {   
            // Liian vähän tai liian paljon merkkejä

            wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');
        }
        else
        {   
            // Y-tunnuksessa on oikea määrä merkkejä

            $y_tunnus_array = explode('-', $y_tunnus); // Erotetaan Y-tunnus ja tarkistusnumero


            if (count($y_tunnus_array) != 2) 
            {   
                // Y-tunnuksessa on liikaa tai liian vähän väliviivoja

                wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');

            }
            else
            {   
                // Y-tunnuksessa on oikea määrä väliviivoja

                if (!is_numeric($y_tunnus_array[0]) || !is_numeric($y_tunnus_array[1])) 
                {   
                    // Y-tunnuksessa on kirjaimia

                    wc_add_notice(__('Tarkista, että Y-tunnuksessa ei ole kirjaimia', 'woocommerce'), 'error');
                }
                else    
                {   
                    // Y-tunnuksessa ei ole kirjaimia

                    if (strlen($y_tunnus_array[0]) != 7 || strlen($y_tunnus_array[1]) != 1) 
                    {   
                        // Y-tunnuksen tunnus tai tarkistusnumero on väärän pituinen

                        wc_add_notice(__('Y-tunnuksen pitää olla muotoa XXXXXXX-X', 'woocommerce'), 'error');
                    }
                    else
                    {
                        // Y-tunnus läpäisi kaikki tarkistukset
     
                    }
                }
            }
        }
    }

}

/*
* Tallentaa Y-tunnuksen tilaukseen ja käyttäjän tietoihin
*/
add_action( 'woocommerce_checkout_create_order', 'tallenna_y_tunnus', 10, 2 );
function tallenna_y_tunnus( $order, $data ) {

    $y_tunnus = isset( $_POST['y_tunnus'] ) ? $_POST['y_tunnus'] : '';

    if (!empty($y_tunnus)) 
    {   
        // Tallentaa Y-tunnuksen tilauksen tietoihin
        $y_tunnus = sanitize_text_field($y_tunnus);
        $order->update_meta_data('y_tunnus', $y_tunnus);

        // Tallentaa Y-tunnuksen käyttäjän tietoihin
        $kayttaja = wp_get_current_user();
        update_user_meta( $kayttaja->ID, 'y_tunnus', $y_tunnus);
    }
}


/*
* Näyttää Y-tunnuksen tilauksen tiedoissa
*/
add_action( 'woocommerce_admin_order_data_after_billing_address', 'nayta_y_tunnus_tilauksen_muokkaus_sivulla', 10, 1 );
function nayta_y_tunnus_tilauksen_muokkaus_sivulla($order)
{
    $y_tunnus = $order->get_meta( 'y_tunnus', true );
    if (!empty($y_tunnus))  
    {   
        // Y-tunnus ei ole tyhjä
        // Näyttää Y-tunnuksen tilauksen tiedoissa

        $otsikko = "Y-tunnus";
        echo '<p><strong>'.  __($otsikko, 'woocommerce') .':</strong> ' . esc_html($y_tunnus) . '</p>';
    } 
}


?>
```

---
## UKK (Usein kysytyt kysymykset)

<details> <summary>Kysymys 1: [Usein kysytty kysymys]</summary> Vastaus kysymykseen. </details> <details> <summary>Minne lisään PHP-koodin</summary> Voit lisätä PHP-koodia WordPress sivuillesi lataamalla [WPcode lisäosan](https://fi.wordpress.org/plugins/insert-headers-and-footers/).

Voit myös lisätä PHP-koodin functions.php tiedostoon, jonka löydät WordPress Admin-sivultasi:  
Ulkoasu  -> Teeman tiedostoeditori.  
Valitse muokattavaksi teemaksi nykyinen teemasi. (Suosittelen tekemään teemastasi "lapsiteeman", jotta lisäämäsi PHP-koodi ei häviä teemaa päivittäessä)

Jos tarvitset lisää apua PHP-koodin lisäämiseen WordPress-sivuillasi ota meihin rohkeasti [yhteyttä](https://itinfo.fi/yhteydenotto) tai jätä kommentti tähän julkaisuun.</details>

  

---

## Yhteenveto

Olet nyt onnistuneesti lisännyt Y-tunnus kentän verkkokauppaasi.


---