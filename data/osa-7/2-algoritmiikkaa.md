---
path: '/osa-7/2-algoritmiikkaa'
title: 'Algorithms'
hidden: true
---

<!-- - Tiedät mitä käsite algoritmi tarkoittaa ja tunnet muutamia algoritmeja.
- Osaat kertoa miten valintajärjestämisalgoritmi toimii.
- Osaat kertoa miten peräkkäishaku- ja binäärihakualgoritmi toimii. -->

<text-box variant='learningObjectives' name='Learning Objectives'>

- You understand the concept of algorithms and you are familiar with a few algorithms
- You can explain how selection sort works
- You can explain how the linear search and binary search algorithms work

</text-box>

<!-- Tietojenkäsittelytieteen ja tietotekniikan ytimessä ovat algoritmit, eli selkeät ohjeet siitä, miten tietty tehtävä suoritetaan. Ohjelmoinnin yhteydessä algoritmit määritellään tyypillisesti ohjelmakoodin avulla.

Algoritmeihin kytketään usein käsite *tehokkuus*. Ohjelman tehokas toiminta, esimerkiksi halutun tiedon tarpeeksi nopea laskenta, on oleellinen osa ohjelmistojen käytettävyyttä. Mikäli seuraavan päivän sään ennustamiseen käytettävän algormitmin suorittaminen kestäisi kaksi päivää, ei ennustuksesta olisi juurikaan hyötyä. Vastaavasti televisio-ohjelmistoja selaava käyttäjä ei hyödy televisio-ohjelman tiedoista mitään jos tiedot latautuvat vasta ohjelman katsomisen jälkeen.

Laajemmin voidaan ajatella, että nopeasti tapahtuva tiedon hakeminen ja näyttäminen on oleellista oikeastaan lähes missä tahansa sovelluksessa. Tutustutaan seuraavaksi tiedon hakemiseen ja järjestämiseen liittyviin algoritmeihin. Vaikka esimerkit käyttävät taulukoita, algoritmit toimivat myös muilla tiedon tallentamiseen tarkoitetuilla tietorakenteilla kuten listoilla. -->

Algorithms, precise instructions on how to to accomplish a specific task, are at the core of computer science. In the context of programming, algorithms are typically defined using source code.

The concept of *efficiency* is often associated with algorithms. A programs efficiency, i.e, the computation of required information fast enough, is an integral part of a programs usability. If it took two days for an algorithm designed for forecasting tomorrows weather run, the results wouldn't be very useful! Similarly, a user viewing a TVs program guide won't get any use out of it, if the tv-shows info only loads after the show already ended.

In a more general sense, retrieving and displaying information quickly is an integral part of any applications function. Next let's explore algorithms associated with retrieving and sorting information. While the following examples utilize arrays, the algorithms shown will also work with other data-structures meant for storing information, such as lists.

## Sorting information

<!-- Jos tieto ei noudata minkäänlaista järjestystä tai sääntöä, on tiedon hakeminen tietokoneelle työlästä. Tarvitsemme järjestystä! -->
If the information (data) giving to a computer, doesn't follow any rules and isn't in order, retrieving that information is taxing for the computer. We need order!


### Selection sort

<!-- Ohjelmoijan yleissivistykseen kuuluu ainakin yhden järjestämisalgoritmin (eli tavan järjestää taulukko) tuntemus. Tutustutaan erääseen "klassiseen" järjestämisalgoritmiin, valintajärjestämiseen. Tutustuminen tapahtuu harjoitustehtävien avulla. -->
Every programmer should be familiar with at least one sorting algorithm (i.e a way to sort an array). Let's familiarize ourselves with one "classic" sorting algorithm, the selection sort. We'll do so with a programing exercise.

<programming-exercise name='Järjestäminen (5 osaa)' tmcname='osa07-Osa07_03.Jarjestaminen'>

<h2>Pienimmän arvon etsiminen</h2>

Tee luokkaan `Paaohjelma` luokkametodi `pienin`, joka palauttaa metodille parametrina annetun kokonaislukutaulukon pienimmän luvun.

Metodin runko on seuraava:

```java
public static int pienin(int[] taulukko) {
    // kirjoita koodia tähän
}
```

Seuraava esimerkki esittelee metodin toimintaa:

```java
int[] luvut = {6, 5, 8, 7, 11};
System.out.println("Pienin: " + Paaohjelma.pienin(luvut));
```

<sample-output>

Pienin: 5

</sample-output>


<h2>Pienimmän arvon indeksi</h2>

Tee luokkaan Paaohjelma luokkametodi `pienimmanIndeksi`, joka palauttaa sille parametrina annetun taulukon pienimmän luvun indeksin.

Metodin runko on seuraava:

```java
public static int pienimmanIndeksi(int[] taulukko) {
    // kirjoita koodia tähän
}
```

Seuraava koodi esittelee metodin toimintaa:

```java
// indeksit:   0  1  2  3  4
int[] luvut = {6, 5, 8, 7, 11};
System.out.println("Pienimmän indeksi: " + Paaohjelma.pienimmanIndeksi(luvut));
```

<sample-output>

Pienimmän indeksi: 1

</sample-output>

Taulukon pienin luku on 5, ja sen indeksi eli sijaintipaikka taulukossa on 1. Muistathan, että taulukon numerointi alkaa 0:sta.


<h2>Pienimmän arvon indeksi taulukon loppuosassa</h2>

Tee luokkaan Paaohjelma luokkametodi `pienimmanIndeksiAlkaen`, joka toimii samalla tavalla kuin edellisen tehtävän metodi, mutta ottaa huomioon vain taulukon loppuosan jostain indeksistä alkaen. Metodille annetaan parametrina taulukon lisäksi aloitusindeksi, josta lähtien pienintä lukua etsitään.

Metodin runko on seuraava:

```java
public static int pienimmanIndeksiAlkaen(int[] taulukko, int aloitusIndeksi) {
    // kirjoita koodia tähän
}
```

Seuraava koodi esittelee metodin toimintaa:

```java
// indeksit:    0  1  2  3   4
int[] luvut = {-1, 6, 9, 8, 12};
System.out.println(Paaohjelma.pienimmanIndeksiAlkaen(luvut, 0));
System.out.println(Paaohjelma.pienimmanIndeksiAlkaen(luvut, 1));
System.out.println(Paaohjelma.pienimmanIndeksiAlkaen(luvut, 2));
```

<sample-output>

0
1
3

</sample-output>

Esimerkissä ensimmäinen metodikutsu etsii pienimmän luvun indeksin aloittaen indeksistä 0. Indeksistä 0 alkaen pienin luku on -1, ja sen indeksi on 0. Toinen metodikutsu etsii pienimmän luvun indeksiä indeksistä 1 aloittaen. Tällöin pienin luku on 6, ja sen indeksi on 1. Kolmas kutsu etsii pienimmän luvun indeksiä aloittaen indeksistä 2. Indeksistä 2 alkaen pienin luku on 8, ja sen indeksi on 3.


<h2>Lukujen vaihtaminen</h2>

Tee luokkaan Paaohjelma luokkametodi `vaihda`, jolle annetaan taulukko ja kaksi sen indeksiä. Metodi vaihtaa indekseissä olevat luvut keskenään.

Metodin runko on seuraava:

```java
public static void vaihda(int[] taulukko, int indeksi1, int indeksi2) {
    // kirjoita koodia tähän
}
```

Seuraavassa estellään metodin toimintaa. Taulukon tulostamisessa käytetään apuna taulukon merkkijonoksi muotoilevaa `Arrays`-luokan `toString`-luokkametodia:


```java
int[] luvut = {3, 2, 5, 4, 8};

System.out.println(Arrays.toString(luvut));

Paaohjelma.vaihda(luvut, 1, 0);
System.out.println(Arrays.toString(luvut));

Paaohjelma.vaihda(luvut, 0, 3);
System.out.println(Arrays.toString(luvut));
```

<sample-output>
[3, 2, 5, 4, 8]
[2, 3, 5, 4, 8]
[4, 3, 5, 2, 8]
</sample-output>


<h2>Järjestäminen</h2>


Nyt koossa on joukko hyödyllisiä metodeja, joiden avulla voimme toteuttaa järjestämisalgoritmin nimeltä valintajärjestäminen.

Valintajärjestämisen idea on seuraava:

- Siirretään taulukon pienin luku indeksiin 0.
- Siirretään taulukon toiseksi pienin luku indeksiin 1.
- Siirretään taulukon kolmanneksi pienin luku indeksiin 2.
- Jne.

Toisin sanoen:

- Tarkastellaan taulukkoa indeksistä 0 alkaen. Vaihdetaan keskenään indeksissä 0 oleva luku sekä taulukon pienin luku indeksistä 0 alkaen.
- Tarkastellaan taulukkoa indeksistä 1 alkaen. Vaihdetaan keskenään indeksissä 1 oleva luku sekä taulukon pienin luku indeksistä 1 alkaen.
- Tarkastellaan taulukkoa indeksistä 2 alkaen. Vaihdetaan keskenään indeksissä 2 oleva luku sekä taulukon pienin luku indeksistä 2 alkaen.
- Jne.


Toteuta luokkaan Paaohjelma luokkametodi `jarjesta`, joka perustuu yllä olevaan ideaan. Metodissa on syytä olla silmukka, joka käy läpi taulukon indeksejä. Metodeista `pieninIndeksiAlkaen` ja `vaihda` on varmasti hyötyä. Tulosta myös taulukon sisältö ennen järjestämistä ja jokaisen kierroksen jälkeen, jotta voit varmistaa algoritmin toimivan oikein.

Metodin runko on seuraava:

```java
public static void jarjesta(int[] taulukko) {

}
```

Testaa metodin toimintaa ainakin seuraavalla esimerkillä:

```java
int[] luvut = {8, 3, 7, 9, 1, 2, 4};
Paaohjelma.jarjesta(luvut);
```

Ohjelman tulosteen tulisi olla seuraavanlainen. Huomaa että sinun tulee tulostaa taulukon sisältö jokaisen vaihtamisen jälkeen!

<sample-output>
[8, 3, 7, 9, 1, 2, 4]
[1, 3, 7, 9, 8, 2, 4]
[1, 2, 7, 9, 8, 3, 4]
[1, 2, 3, 9, 8, 7, 4]
[1, 2, 3, 4, 8, 7, 9]
[1, 2, 3, 4, 7, 8, 9]
[1, 2, 3, 4, 7, 8, 9]
</sample-output>

Huomaat, miten taulukko tulee pikkuhiljaa järjestykseen alkaen alusta ja edeten loppua kohti.

</programming-exercise>



<text-box variant='hint' name='Static or not'>

<!-- Kurssin alussa kaikissa metodeissa esiintyi määre `static`, mutta aloittaessamme olioiden käytön, tuon määreen käyttö jopa kiellettiin.

Javan metodit voidaan jakaa kahteen ryhmään `static`-määreen olemassaolon perusteella. Metodit joissa ei ole `static`-määrettä ovat **oliometodeja** (tunnetaan myös instanssimetodeina). Metodit joissa on `static`-määre ovat **luokkametodeja**.

Oliometodit ovat metodeja, jotka liittyvät olioihin, ja joiden koodissa voi käsitellä oliomuuttujia ja kutsua olion muita oliometodeja. Oliometodeissa on erityisesti pääsy `this`-määreeseen, joka on viite juuri metodia kutsuvaan olioon muuttujiin. Luokkametodeilla ei ole pääsyä `this`-määreeseen, jolloin niiden käytössä on vain ne muuttujat, jotka niille annetaan parametrina ja ne, jotka ne luovat.

*Todellisuudessa luokkametodit pääsevät myös mm. luokkamuuttujiin -- näitä ei kuitenkaan käsitellä tällä kurssilla.* -->
At the start of the course, all of our methods included the `static` modifier, but when we started using objects, use of the `static` modifier was banned.

Methods in Java can be divided into two groups, based on whether they have the `static` modifier or not. Methods without the `static` modifier are **instance methods**. Methods with the `static` modifier are **class methods**

Instance methods are methods that are associated with an object, can process the objects variables and can call the object's other methods. Instance methods specifically CAN use the `this` modifier, which refers to the variables associated with the specific object, that is calling the instance method. Class methods can't use the `this` modifier, meaning that they can only access the variables they are given as parameters or that they create themselves.

*In reality class methods can also access class variable, among other things. However, these things are  outside the scope of this course.*

</text-box>


###  Build-in sorting algorithms in Java

<!-- Java tarjoaa merkittävän määrän valmiita järjestysalgoritmeja. Taulukot voi järjestää (luonnolliseen järjestykseen) luokan `Arrays` tarjoamalla luokkametodilla `sort`, ja listat voi järjestää (luonnolliseen järjestykseen) luokan `Collections` tarjoamalla luokkametodilla `sort`. -->

Java offers a significant amount of ready to use sorting algorithms. Arrays can be sorted (into their natural order) using the class method `sort` of the `Arrays`-class. Lists can be sorted (into their natural order) using the class method sort of the `Collections` class.

<!-- ```java
int[] luvut = {8, 3, 7, 9, 1, 2, 4};
System.out.println(Arrays.toString(luvut));
Arrays.sort(luvut);
System.out.println(Arrays.toString(luvut));
``` -->
```java
int[] numbers = {8, 3, 7, 9, 1, 2, 4};
System.out.println(Arrays.toString(numbers));
Arrays.sort(numbers);
System.out.println(Arrays.toString(numbers));
```

<sample-output>
[8, 3, 7, 9, 1, 2, 4]
[1, 2, 3, 4, 7, 8, 9]
</sample-output>


<!-- ```java
ArrayList<Integer> luvut = new ArrayList<>();
luvut.add(8);
luvut.add(3);
luvut.add(7);
System.out.println(luvut);
Collections.sort(luvut);
System.out.println(luvut);
``` -->
```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(8);
numbers.add(3);
numbers.add(7);
System.out.println(numbers);
Collections.sort(numbers);
System.out.println(numbers);
```

<sample-output>
[8, 3, 7]
[3, 7, 8]
</sample-output>


<!-- Valmiit järjestämisalgoritmit toimivat sekä alkeistyyppisille muuttujille, että joillekin Javan valmiille viittaustyyppisille muuttujille kuten String. Omien luokkiemme järjestämistä varten joudumme antamaan Javalle hieman lisävinkkejä, sillä luokat eivät sisällä tietoa siitä, miten niistä luodut oliot pitäisi järjestää. Palaamme omista luokista tehtyjen olioiden järjestämiseen ohjelmoinnin jatkokurssilla. -->

Java's build-in sorting algorithms work with value type variables and some of Java's build-in reference type variables, like String. In order for our own classes to be sorted, we need to provide Java with some tips on how to do that, because the classes themselves don't contain information on how objects created from them should be ordered. We'll get back to ordering objects created from classes we made ourselves in the advanced course in programming.

<programming-exercise name='Ready-made Sorting Algorithms' tmcname='part07-Part07_04.ReadymadeSortingAlgorithms'>

<!--Lisää luokkaan `Paaohjelma` seuraavat luokkametodit:

- `public static void jarjesta(int[] taulukko)` järjestää kokonaislukutaulukon.

- `public static void jarjesta(String[] taulukko)` järjestää merkkijonotaulukon.

- `public static void jarjestaLuvut(ArrayList<Integer> luvut)` järjestää lukuja sisältävän listan.

- `public static void jarjestaMerkkijonot(ArrayList<String> merkkijonot)` järjestää merkkijonoja sisältävän listan.

Hyödynnä metodien toteutuksessa Javan valmiita kirjastoja.-->

Add the following methods to the Main class:

- `public static void sort(int[] array)` that sorts an array of integers.

- `public static void sort(String[] array)` that sorts an array of strings.

- `public static void sortIntegers(ArrayList<Integer> integers)` that sorts a list of integers.

- `public static void sortStrings(ArrayList<String> strings)` that sorts a list of strings.

Be sure to use the ready-made Java libraries in your implementation.

</programming-exercise>


## Information retrieval

<!-- Tarkastellaan seuraavaksi tiedon hakemiseen tarkoitettuja algoritmeja. -->
Next let's take a look at algorithms meant for information retrieval.

### Linear search


<!-- Peräkkäishaku on hakualgoritmi, joka etsii tietoa taulukosta käymällä taulukkoa läpi alkio alkiolta. Heti kun haettu alkio löytyy, sen indeksi palautetaan. Jos alkiota ei löydy, palautetaan tieto siitä ettei haettavaa alkiota löytynyt -- tämä kerrotaan tyypillisesti palauttamalla indeksin sijaan arvo `-1`. -->

Linear search is a search algorithm, that searches for information in array by going through every value in the array one by one. When the value that was searched for is found, it's index is immediately returned. If the requested value cannot be found, linear sort return the information that the value was not found -- typically this means returning `-1` instead of a valid index.

<!-- ```java
public class Algoritmit {

    public static int perakkaishaku(int[] taulukko, int haettava) {
        for (int i = 0; i < taulukko.length; i++) {
            if (taulukko[i] == haettava) {
                return i;
            }
        }

        return -1;
    }
}
``` -->
```java
public class Algorithms {

    public static int linearSearch(int[] array, int searched) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] == searched) {
                return i;
            }
        }

        return -1;
    }
}
```

<!-- Pahimmassa tapauksessa, eli tilanteessa missä alkiota ei löydy, algoritmi tekee taulukon koon verran vertailuja. Vaikkapa 10 miljoonaa arvoa sisältävässä taulukossa tämä tarkoittaa kymmentä miljoonaa vertailua. Jos tietoa haetaan useampia kertoja, kannattaa tehokkuutta yrittää parantaa.
 -->
 In the worst case scenario, i.e, when the value searched for isn't found, the algorithm has to do as many comparisons as there are values in the array. In an array containing, say, 10 million values, this means 10 comparisons. If we are doing more than one search, it makes sense to try and improve efficiency.

### Binary search (aka half-interval search or logarithmic search )

<!-- Kun tieto on järjestyksessä, hakeminen voidaan toteuttaa paljon peräkkäishakua tehokkaammin. Binäärihakualgoritmin idea aloittaa tiedon etsiminen taulukon (tai listan) keskimmäisestä indeksistä, verrata indeksissä olevaa arvoa haettuun arvoon, ja rajata tarvittaessa (eli mikäli haettavaa arvoa ei ole indeksissä) puolet tarkasteltavasta alueesta pois. Algoritmi esitellään seuraavassa slideshowssa. -->
When the data searched is in order, searching can be implemented a lot more efficiently than in linear search.
THe idea behind Binary Search is to start looking for the searched value in the middle index of the array (or list), compare the value found there to the searched value, and if needed (i.e, when the value isn't found there) eliminate half of the search area. The algorithm is more thoroughly introduced in the following slideshow.

<pdf-slideshow>

[a](../slideshows/binaarihaku.pdf)

</pdf-slideshow>

<br/>


<programming-exercise name='Hakemista' tmcname='osa07-Osa07_05.Hakemista'>

Tehtäväpohjassa on valmiina luokka `Kirja`, joka kuvaa numeerisen tunnuksen `id` ja nimen `nimi` sisältäviä olioita.

Tässä tehtävässä toteutetaan peräkkäishaku- ja binäärihakualgoritmi kirjojen hakemiseen niiden numeerisen tunnuksen perusteella. Tehtäväpohjassa on valmiina toteutettavien metodien nimet -- tällä hetkellä kumpikin metodeista palauttaa arvon `-1` -- sekä pääohjelma, jota voi käyttää metodien testaamiseen.

<h2>Peräkkäishaku</h2>

Peräkkäishakualgoritmi toimii siten, että se tarkastelee kutakin listassa tai taulukossa olevaa arvoa yksi kerrallaan, nollannesta indeksistä lähtien.

Toteuta luokkaan `Paaohjelma` metodi `public static int perakkaishaku(ArrayList<Kirja> kirjat, int haettavaId)`, joka hakee parametrina annetusta listasta kirjaa, jonka `id`-muuttujan arvo on sama kuin metodille parametrina annetun `haettavaId`-muuttujan arvo. Mikäli kirja löytyy, tulee metodin palauttaa kyseisen kirjan indeksi parametrina annetussa listassa. Mikäli kirjaa ei löydy, tulee metodin palauttaa arvo `-1`.


<h2>Binäärihaku</h2>

Toteuta luokkaan `Paaohjelma` metodi `public static int binaarihaku(ArrayList<Kirja> kirjat, int haettavaId)`, joka hakee parametrina annetusta listasta kirjaa, jonka `id`-muuttujan arvo on sama kuin metodille parametrina annetun `haettavaId`-muuttujan arvo. Mikäli kirja löytyy, tulee metodin palauttaa kyseisen kirjan indeksi parametrina annetussa listassa. Mikäli kirjaa ei löydy, tulee metodin palauttaa arvo `-1`.

Metodi tulee toteuttaa binäärihakuna, jolloin alkuoletuksena on se, että lista on järjestyksessä. Oleta lisäksi, että listan alkupäässä olevat `id`t ovat aina pienempiä kuin listan loppupäässä olevat `id`t.

Saat apuusi myös edellisessä esityksessä käytetyn binäärihaun idean sekä *pseudokoodin*, eli ohjelmointikielen kaltaisella kielellä kuvatun ohjelman kuvauksen.

Edellisessä esityksessä binäärihaun idea kuvattiin seuraavasti:

- Tietoa etsitään järjestyksessä olevasta taulukosta tai listasta.
- Hakeminen aloitetaan keskikohdasta.
- Mikäli tarkasteltavan keskikohdan arvo ei ole haettu arvo, rajataan haettavasta alueesta puolet pois ja siirrytään tarkastelemaan jäljelle jäävän alueen keskikohtaa.
- Mikäli tarkasteltavan keskikohdan arvo on haettu arvo, palautetaan tarkasteltavan keskikohdan indeksi.
- Mikäli tarkasteltavaa aluetta ei ole enää jäljellä (koko alue rajattu pois), palautetaan arvo -1, joka kuvaa ettei haettavaa arvoa löydy

Binäärihaun pseudokoodi on seuraavanlainen.

```code
// oletetaan, että käytössä on muuttuja haettava
// oletetaan, että käytössä on muuttuja lista
alku = 0 // listan nollas indeksi
loppu = koko(lista) - 1 // listan viimeinen indeksi

toista kunnes alku on suurempi kuin loppu:
    keski = (loppu + alku) / 2

    jos arvo kohdassa lista[keski] on haettava
        palauta muuttujan keski arvo

    jos arvo kohdassa lista[keski] on pienempi kuin haettava
        alku = keski + 1

    jos arvo kohdassa lista[keski] on suurempi kuin haettava
        loppu = keski - 1

palauta arvo -1
```

Huomaa, että kirjojen tapauksessa tarkastelet kirjojen `id`-muuttujien arvoja. Tämä tarkoittaa sitä, että sen sijaan, että käsittelet vain listan tietyssä indeksissä olevaa arvoa, tulee tehtävässä käsitellä tietyssä indeksissä olevan arvon `id`-muuttujaa.

</programming-exercise>


