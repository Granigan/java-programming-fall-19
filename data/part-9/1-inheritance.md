---
path: '/part-9/1-inheritance'
title: 'Class inheritance'
hidden: false
---


<text-box variant='learningObjectives' name='Oppimistavoitteet'>
- Tiedät että Java-ohjelmointikielessä jokainen luokka perii luokan Object ja tiedät        miksi jokaisella oliolla on metodit toString, equals ja hashCode.
- Tunnet käsitteet perintä, yliluokka ja aliluokka.
- Osaat luoda luokkia, jotka perivät osan ominaisuuksistaan toisesta luokasta.
- Osaat kutsua yliluokassa määriteltyä konstruktoria ja metodia.
- Tiedät miten olion suoritettava metodi määräytyy ja tunnet käsitteen polymorfismi.
- Tiedät milloin perintää kannattaa käyttää ja osaat antaa esimerkin tilanteesta, mihin perintä ei sovi.

</text-box>


Luokkia käytetään olio-ohjelmoinnissa ongelma-alueeseen liittyvien käsitteiden selkeyttämiseen. Jokainen luomamme luokka lisää ohjelmointikieleen toiminnallisuutta. Tätä toiminnallisuutta tarvitaan kohtaamiemme ongelmien ratkomiseen. Olio-ohjelmoinnissa **ratkaisut syntyvät luokista luotujen olioiden välisen interaktion avulla**. Olio-ohjelmoinnissa olio on itsenäinen kokonaisuus, jolla on olion tarjoamien metodien avulla muutettava tila. Olioita käytetään yhteistyössä; jokaisella oliolla on oma vastuualue. Esimerkiksi käyttöliittymäluokkamme ovat tähän mennessä hyödyntäneet `Scanner`-olioita.


Jokainen Javan luokka perii luokan Object, eli jokainen luomamme luokka saa käyttöönsä kaikki Object-luokassa määritellyt metodit. Jos haluamme muuttaa Object-luokassa määriteltyjen metodien toiminnallisuutta tulee ne korvata (`Override`) määrittelemällä niille uusi toteutus luodussa luokassa. Oliomme saavat luokasta Object käyttöönsä mm. metodit `equals` ja `hashCode`.


Luokan `Object` perimisen lisäksi myös muiden luokkien periminen on mahdollista. Javan <a href="https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html" target="_blank" rel="noopener">ArrayList</a>-luokan "ohjelmointirajapintaa" eli APIa tarkasteltaessa huomaamme että `ArrayList` perii luokan `AbstractList`. Luokka `AbstractList` perii luokan `AbstractCollection`, joka perii luokan `Object`.

<br/>

<pre>
  <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html" target="_blank" rel="noopener">java.lang.Object</a>
  <img src="../img/material/perinta.gif" /><a href="https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html" target="_blank" rel="noopener">java.util.AbstractCollection</a>&lt;E&gt;
    <img src="../img/material/perinta.gif" /><a href="https://docs.oracle.com/javase/8/docs/api/java/AbstractList.html" target="_blank" rel="noopener">java.util.AbstractList</a>&lt;E&gt;
      <img src="../img/material/perinta.gif" /> java.util.ArrayList&lt;E&gt;
</pre>


Kukin luokka voi periä suoranaisesti yhden luokan. Välillisesti luokka kuitenkin perii kaikki perimänsä luokan ominaisuudet. Luokka `ArrayList` perii luokan `AbstractList`, ja välillisesti luokat `AbstractCollection` ja `Object`. Luokalla `ArrayList` on siis käytössään luokkien `AbstractList`, `AbstractCollection` ja `Object` muuttujat ja metodit.


Luokan ominaisuudet peritään avainsanalla `extends`. Luokan perivää luokkaa kutsutaan aliluokaksi (*subclass*), perittävää luokkaa yliluokaksi (*superclass*).


Tutustutaan erään autonvalmistajan järjestelmään, joka hallinnoi auton osia. Osien hallinan peruskomponentti on luokka `Osa`, joka määrittelee tunnuksen, valmistajan ja kuvauksen.


```java
public class Osa {

    private String tunnus;
    private String valmistaja;
    private String kuvaus;

    public Osa(String tunnus, String valmistaja, String kuvaus) {
        this.tunnus = tunnus;
        this.valmistaja = valmistaja;
        this.kuvaus = kuvaus;
    }

    public String getTunnus() {
        return tunnus;
    }

    public String getKuvaus() {
        return kuvaus;
    }

    public String getValmistaja() {
        return valmistaja;
    }
}
```

Yksi osa autoa on moottori. Kuten kaikilla osilla, myös moottorilla on valmistaja, tunnus ja kuvaus. Näiden lisäksi moottoriin liittyy moottorityyppi: esimerkiksi polttomoottori, sähkömoottori tai hybridi.


Perinteinen, ei perintää hyödyntävä tapa olisi toteuttaa luokka `Moottori` seuraavasti.


```java
public class Moottori {

    private String moottorityyppi;
    private String tunnus;
    private String valmistaja;
    private String kuvaus;

    public Moottori(String moottorityyppi, String tunnus, String valmistaja, String kuvaus) {
        this.moottorityyppi = moottorityyppi;
        this.tunnus = tunnus;
        this.valmistaja = valmistaja;
        this.kuvaus = kuvaus;
    }

    public String getMoottorityyppi() {
        return moottorityyppi;
    }

    public String getTunnus() {
        return tunnus;
    }

    public String getKuvaus() {
        return kuvaus;
    }

    public String getValmistaja() {
        return valmistaja;
    }
}
```


Huomaamme luokassa `Moottori` merkittävän määrän yhtäläisyyksiä luokan `Osa` kanssa. Voidaankin sanoa, että `Moottori` on luokan `Osa` erikoistapaus. **Moottori on Osa**, mutta sillä on myös ominaisuuksia, joita osalla ei ole, eli tässä moottorin tyyppi.


Tehdään sama luokka `Moottori`, ja toteutetaan luokka perintää hyödyntämällä. Luodaan luokan `Osa` perivä luokka `Moottori`: moottori on osan erikoistapaus.


```java
public class Moottori extends Osa {

    private String moottorityyppi;

    public Moottori(String moottorityyppi, String tunnus, String valmistaja, String kuvaus) {
        super(tunnus, valmistaja, kuvaus);
        this.moottorityyppi = moottorityyppi;
    }

    public String getMoottorityyppi() {
        return moottorityyppi;
    }
}
```


Luokkamäärittely `public class Moottori extends Osa` kertoo että luokka `Moottori` perii luokan `Osa` toiminnallisuuden. Luokassa `Moottori` määritellään oliomuuttuja `moottorityyppi`.


Moottori-luokan konstruktori on mielenkiintoinen. Konstruktorin ensimmäisellä rivillä on avainsana `super`, jolla kutsutaan yliluokan konstruktoria. Kutsu `super(tunnus, valmistaja, kuvaus)` kutsuu luokassa `Osa` määriteltyä konstruktoria `public Osa(String tunnus, String valmistaja, String kuvaus)`, jolloin yliluokassa määritellyt oliomuuttujat saavat arvonsa. Tämän jälkeen oliomuuttujalle `moottorityyppi` asetetaan siihen liittyvä arvo.


*Kutsu on hieman samankaltainen kuin `this`-kutsu konstruktorissa; this-kutsulla kutsutaan tämän luokan konstruktoria, super-kutsulla yliluokan konstruktoria. Mikäli konstruktorissa käytetään yliluokan konstruktoria, eli konstruktorissa on `super`-kutsu, tulee `super`-kutsun olla `this`-kutsun lailla konstruktorin ensimmäisellä rivillä.*


Kun luokka `Moottori` perii luokan `Osa`, saa se käyttöönsä kaikki luokan `Osa` tarjoamat metodit. Luokasta `Moottori` voi tehdä ilmentymän aivan kuten mistä tahansa muustakin luokasta.


```java
Moottori moottori = new Moottori("polttomoottori", "hz", "volkswagen", "VW GOLF 1L 86-91");
System.out.println(moottori.getMoottorityyppi());
System.out.println(moottori.getValmistaja());
```


<sample-output>

polttomoottori
volkswagen

</sample-output>


Kuten huomaat, luokalla `Moottori` on käytössä luokassa `Osa` määritellyt metodit.


<programming-exercise name='ABC (2 parts)' tmcname='part09-Part09_01.ABC'>

<!--Harjoitellaan tässä luokkien luomista ja perintää.-->
Let's practice creating and inheriting classes.

<!--<h2>Luokkien luominen</h2>-->
<h2>Creating classes</h2>

<!--Luo tehtäväpohjaan seuraavat kolme luokkaa:


- Luokka `A`. Luokalla ei ole oliomuuttujia eikä erikseen määriteltyä konstruktoria. Luokalla on vain metodi `public void a()`, joka tulostaa merkkijonon "A".
- Luokka `B`. Luokalla ei ole oliomuuttujia eikä erikseen määriteltyä konstruktoria. Luokalla on vain metodi `public void b()`, joka tulostaa merkkijonon "B".
- Luokka `C`. Luokalla ei ole oliomuuttujia eikä erikseen määriteltyä konstruktoria. Luokalla on vain metodi `public void c()`, joka tulostaa merkkijonon "C".
-->

Create the following three classes:

- Class `A`. Class should have no object variables nor should you specify a constructor for it. It only has the method `public void a()`, which prints a string "A".
- Class `B`. Class should have no object variables nor should you specify a constructor for it. It only has the method `public void b()`, which prints a string "B".
- Class `C`. Class should have no object variables nor should you specify a constructor for it. It only has the method `public void c()`, which prints a string "C".


```java
A a = new A();
B b = new B();
C c = new C();

a.a();
b.b();
c.c();
```

<sample-output>

A
B
C

</sample-output>



<!--<h2>Luokkien periminen</h2>-->
<h2>Class inheritance</h2>

<!--Muokkaa luokkia siten, että luokka B perii luokan A ja luokka C perii luokan B. Luokasta A tulee siis luokan B yliluokka, ja luokasta B luokan C yliluokka.-->
Modify the classes so that class B inherits class A, and class C inherits class B. In other words, class A will be a superclass for class B, and class B will be a superclass for class C.


```java
C c = new C();

c.a();
c.b();
c.c();
```

<sample-output>

A
B
C

</sample-output>


</programming-exercise>


## Näkyvyysmääreet private, protected ja public


Mikäli metodilla tai muuttujalla on näkyvyysmääre `private`, se näkyy vain luokan sisäisille metodeille. Se ei näy aliluokille eikä aliluokalla ole mitään suoraa tapaa päästä käsiksi siihen. Moottori-luokasta ei siis pääse suoraan käsiksi yliluokassa Osa määriteltyihin muuttujiin tunnus, valmistaja, kuvaus. Tällä tarkoitetaan sitä, että Moottori-luokassa ohjelmoija ei voi suoraan käsitellä niitä yliluokan muuttujia, joilla on näkyvyysmääre private.


Aliluokka näkee kaiken yliluokan julkisen eli `public`-määreellä varustetun kaluston. Jos halutaan määritellä yliluokkaan joitain muuttujia tai metodeja joiden näkeminen halutaan sallia aliluokille, mutta estää muilta, voidaan käyttää näkyvyysmäärettä `protected`.


## Yliluokan konstruktorin kutsuminen


Yliluokan konstruktoria kutsutaan avainsanalla `super`. Kutsulle annetaan parametrina yliluokan konstruktorin vaatiman tyyppiset arvot. Mikäli yliluokalla on useampi konstruktori, super-kutsulle annettavat parametrit määräävät kutsuttavan konstruktorin.


Konstruktorikutsun yhteydessä yliluokassa määritellyt muuttujat alustetaan. Konstruktorikutsussa tapahtuu käytännössä täysin samat asiat kuin normaalissa konstruktorikutsussa. Mikäli yliluokassa ei ole määritelty parametritonta konstruktoria, tulee aliluokan konstruktorikutsuissa olla aina mukana yliluokan konstruktorikutsu.


Alla olevassa esimerkissä demonstroidaan `this`-kutsua ja `super`-kutsua. Luokka `Yliluokka` sisältää oliomuuttujan ja kaksi konstruktoria. Toinen konstruktoreista kutsuu toista `this`-kutsulla. Luokka `Aliluokka` sisältää parametrillisen konstruktorin, mutta sillä ei ole yhtäkään oliomuuttujaa. Luokan `Aliluokka`-konstruktori kutsuu luokan `Yliluokka` parametrillista konstruktoria.


```java
public class Yliluokka {

    private String oliomuuttuja;

    public Yliluokka() {
        this("Esimerkki");
    }

    public Yliluokka(String arvo) {
        this.oliomuuttuja = arvo;
    }

    public String toString() {
        return this.oliomuuttuja;
    }
}
```


```java
public class Aliluokka extends Yliluokka {

    public Aliluokka() {
        super("Aliluokka");
    }
}
```


```java
Yliluokka y = new Yliluokka();
Aliluokka a = new Aliluokka();

System.out.println(y);
System.out.println(a);
```


<sample-output>

Esimerkki
Aliluokka

</sample-output>


## Yliluokan metodin kutsuminen


Yliluokassa määriteltyjä metodeja voi kutsua `super`-etuliitteen avulla, aivan kuten tässä luokassa määriteltyjä metodeja voi kutsua `this`-etuliitteellä. Esimerkiksi yliluokassa määriteltyä `toString`-metodia voi hyödyntää sen korvaavassa metodissa seuraavasti:


```java
@Override
public String toString() {
    return super.toString() + "\n  Ja oma viestini vielä!";
}
```


<programming-exercise name='Person and subclasses (5 parts)' tmcname='part09-Part09_02.PersonAndSubclasses' nocoins='true'>


<!--<h2>Person</h2>-->
<h2>Person</h2>

<!--Luo luokka `Person`. Luokan tulee toimia seuraavan esimerkin mukaisesti.-->
Create a class `Person`. The class must work as follows:

<!--
```java
Person ada = new Person("Ada Lovelace", "Korsontie 1 03100 Vantaa");
Person esko = new Person("Esko Ukkonen", "Mannerheimintie 15 00100 Helsinki");
System.out.println(ada);
System.out.println(esko);
```
-->

```java
Person ada = new Person("Ada Lovelace", "24 Maddox St. London W1S 2QN");
Person esko = new Person("Esko Ukkonen", "Mannerheimintie 15 00100 Helsinki");
System.out.println(ada);
System.out.println(esko);
```

<!--
<sample-output>
Ada Lovelace
  Korsontie 1 03100 Vantaa
Esko Ukkonen
  Mannerheimintie 15 00100 Helsinki
</sample-output>
-->

<sample-output>
Ada Lovelace
  24 Maddox St. London W1S 2QN
Esko Ukkonen
  Mannerheimintie 15 00100 Helsinki
</sample-output>

<!--<h2>Opiskelija</h2>-->
<h2>Student</h2>

<!--
Luo luokka `Opiskelija` joka perii luokan `Person`.


Opiskelijalla on aluksi 0 opintopistettä. Aina kun opiskelija opiskelee, opintopistemäärä kasvaa. Luokan tulee toimia seuraavan esimerkin mukaisesti.
-->
Create a class `Student`, which inherits the class `Person`.


At creation, student has 0 study credits. Every time a student studies, amount of study credits goes up. Class must act as follows:

<!--
```java
Opiskelija olli = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
System.out.println(olli);
System.out.println("opintopisteitä " + olli.opintopisteita());
olli.opiskele();
System.out.println("opintopisteitä "+ olli.opintopisteita());
```
-->

```java
Student ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
System.out.println(ollie);
System.out.println("Study credits " + ollie.credits());
olli.study();
System.out.println("Study credits "+ ollie.credits());
```

<!--
<sample-output>
Olli
  Ida Albergintie 1 00400 Helsinki
opintopisteitä 0
opintopisteitä 1
</sample-output>
-->

<sample-output>
Ollie
  6381 Hollywood Blvd. Los Angeles 90028
Study credits 0
Study credits 1
</sample-output>


<!--<h2>Opiskelijalle toString</h2>-->
<h2>Student's toString</h2>

<!--Edellisessä tehtävässä `Opiskelija` perii toString-metodin luokalta `Person`. Perityn metodin voi myös ylikirjoittaa, eli korvata omalla versiolla. Tee luokalle Opiskelija oma versio toString-metodista. Metodin tulee toimia seuraavan esimerkin mukaisesti.-->
In the previous task, `Student`inherits the toString method from the class `Person`. However, you can also overwrite an inherited method, replacing it with your own version. Write a version of toString method specifically for the `Student` class. Method must act as follows:

<!--
```java
Opiskelija olli = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
System.out.println(olli);
olli.opiskele();
System.out.println(olli);
```
-->

```java
Student ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
System.out.println(ollie);
olli.study();
System.out.println(ollie);
```

<!--
<sample-output>
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 0
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 1
</sample-output>
-->

<sample-output>
Ollie
  6381 Hollywood Blvd. Los Angeles 90028
  Study credits 0
Ollie
  6381 Hollywood Blvd. Los Angeles 90028
  Study credits 1
</sample-output>


<!--<h2>Opettaja</h2>-->
<h2>Teacher</h2>

<!--Luo luokan Person perivä luokka Opettaja. Opettajalla on palkka joka tulostuu opettajan merkkijonoesityksessä.


Luokan tulee toimia seuraavan esimerkin mukaisesti.-->

Create a class `Teacher`, which inherits the class `Person`.

The class must act as follows:

<!--
```java
Opettaja ada = new Opettaja("Ada Lovelace", "Korsontie 1 03100 Vantaa", 1200);
Opettaja esko = new Opettaja("Esko Ukkonen", "Mannerheimintie 15 00100 Helsinki", 5400);
System.out.println(ada);
System.out.println(esko);

Opiskelija olli = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");

int i = 0;
while (i < 25) {
  olli.opiskele();
  i = i + 1;
}
System.out.println(olli);
```
-->

```java
Teacher ada = new Teacher("Ada Lovelace", "24 Maddox St. London W1S 2QN", 1200);
Teacher esko = new Teacher("Esko Ukkonen", "Mannerheimintie 15 00100 Helsinki", 5400);
System.out.println(ada);
System.out.println(esko);

Student ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");

int i = 0;
while (i < 25) {
  ollie.study();
  i = i + 1;
}
System.out.println(ollie);
```

<!--
<sample-output>
Ada Lovelace
  Korsontie 1 03100 Vantaa
  palkka 1200 euroa/kk
Esko Ukkonen
  Mannerheimintie 15 00100 Helsinki
  palkka 5400 euroa/kk
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 25
</sample-output>
-->

<sample-output>
Ada Lovelace
  24 Maddox St. London W1S 2QN
  salary 1200 euro/month
Esko Ukkonen
  Mannerheimintie 15 00100 Helsinki
  salary 5400 euro/month
Ollie
  6381 Hollywood Blvd. Los Angeles 90028
  Study credits 25
</sample-output>

<!--<h2>Kaikki Persont listalle</h2>-->
<h2>List all Persons</h2>


<!--Toteuta pääohjelmaluokkaan `Main` luokkametodi `public static void tulostaPersont(ArrayList<Person> henkilot)`, joka tulostaa kaikki metodille parametrina annetussa listassa olevat henkilöt. Metodin tulee toimia seuraavasti `main`-metodista kutsuttaessa.-->
Write a method `public static void printPersons(ArrayList<Person> persons)` in the Main class. The method prints all the persons on the list given as the parameter. Method must act as follows when invoked from the `main` method:

<!--
```java
public static void main(String[] args) {
    ArrayList<Person> henkilot = new ArrayList<Person>();
    henkilot.add(new Opettaja("Ada Lovelace", "Korsontie 1 03100 Vantaa", 1200));
    henkilot.add(new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki"));

    tulostaPersont(henkilot);
}
```
-->

```java
public static void main(String[] args) {
    ArrayList<Person> persons = new ArrayList<Person>();
    persons.add(new Teacher("Ada Lovelace", "24 Maddox St. London W1S 2QN", 1200));
    persons.add(new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028"));

    printPersons(persons);
}
```

<!--
<sample-output>
Ada Lovelace
  Korsontie 1 03100 Vantaa
  palkka 1200 euroa/kk
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 0
</sample-output>
-->

<sample-output>
Ada Lovelace
  24 Maddox St. London W1S 2QN
  salary 1200 euro/month
Ollie
  6381 Hollywood Blvd. Los Angeles 90028
  Study credits 0
</sample-output>


</programming-exercise>



## Olion todellinen tyyppi määrää suoritettavan metodin


Olion kutsuttavissa olevat metodit määrittyvät muuttujan tyypin kautta. Esimerkiksi jos edellä toteutetun `Opiskelija`-tyyppisen olion viite on talletettu `Person`-tyyppiseen muuttujaan, on oliosta käytössä vain `Person`-luokassa määritellyt metodit (sekä Person-luokan yliluokan ja rajapintojen metodit):


```java
Person olli = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
olli.opintopisteita();        // EI TOIMI!
olli.opiskele();              // EI TOIMI!
System.out.println(olli);   // olli.toString() TOIMII
```

Oliolla on siis käytössä jokainen sen tyyppiin sekä sen yliluokkiin ja rajapintoihin liittyvä metodi. Esimerkiksi Opiskelija-tyyppisellä oliolla on käytössä Person-luokassa määritellyt metodit sekä Object-luokassa määritellyt metodit.


Edellisessä tehtävässä korvasimme Opiskelijan luokalta Henkilö perimän `toString` uudella versiolla. Myös luokka Henkilö oli jo korvannut Object-luokalta perimänsä toStringin. Jos käsittelemme olioa jonkun muun kuin sen todellisen tyypin kautta, mitä versiota olion metodista kutsutaan?


Seuraavassa esimerkissä kahta opiskelijaa käsitellään erityyppisten muuttujien kautta. Mikä versio metodista toString suoritetaan, luokassa Object, Person vai Opiskelija määritelty?


```java
Opiskelija olli = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
System.out.println(olli);
Person olliPerson = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
System.out.println(olliPerson);
Object olliObject = new Opiskelija("Olli", "Ida Albergintie 1 00400 Helsinki");
System.out.println(olliObject);

Object liisa = new Opiskelija("Liisa", "Väinö Auerin katu 20 00500 Helsinki");
System.out.println(liisa);
```

<sample-output>
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 0
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 0
Olli
  Ida Albergintie 1 00400 Helsinki
  opintopisteitä 0
Liisa
  Väinö Auerin katu 20 00500 Helsinki
  opintopisteitä 0
</sample-output>


Suoritettava metodi valitaan olion todellisen tyypin perusteella, eli sen luokan perusteella, jonka konstruktoria kutsutaan kun olio luodaan. Jos kutsuttua metodia ei ole määritelty luokassa, suoritetaan perintähierarkiassa olion todellista tyyppiä lähinnä oleva metodin toteutus.


<text-box variant='hint' name='Polymorfismi'>

Suoritettava metodi valitaan aina olion todellisen tyypin perusteella riippumatta käytetyn muuttujan tyypistä. Oliot ovat monimuotoisia, eli olioita voi käyttää usean eri muuttujatyypin kautta. Suoritettava metodi liittyy aina olion todelliseen tyyppiin. Tätä monimuotoisuutta kutsutaan polymorfismiksi.

</text-box>


Tarkastellaan Polymorfismia toisen esimerkin avulla.


Kaksiulotteisessa koordinaatiostossa sijaitsevaa pistettä voisi kuvata seuraavan luokan avulla:


```java
public class Piste {

    private int x;
    private int y;

    public Piste(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int manhattanEtaisyysOrigosta() {
        return Math.abs(x) + Math.abs(y);
    }

    protected String sijainti(){
        return x + ", " + y;
    }

    @Override
    public String toString() {
        return "(" + this.sijainti() + ") etäisyys " + this.manhattanEtaisyysOrigosta();
    }
}
```


Metodi `sijainti` ei ole tarkoitettu ulkoiseen käyttöön, joten se on näkyvyysmääreeltään protected, eli aliluokat pääsevät siihen käsiksi. Esimerkiksi reitinhakualgoritmien hyödyntämällä <a href="http://en.wiktionary.org/wiki/Manhattan_distance">Manhattan-etäisyydellä</a> tarkoitetaan pisteiden etäisyyttä, jos niiden välin voi kulkea ainoastaan koordinaattiakselien suuntaisesti.


Värillinen piste on muuten samanlainen kuin piste, mutta se sisältää merkkijonona ilmaistavan värin. Luokka voidaan siis tehdä perimällä Piste.


```java
public class VariPiste extends Piste {

    private String vari;

    public VariPiste(int x, int y, String vari) {
        super(x, y);
        this.vari = vari;
    }

    @Override
    public String toString() {
        return super.toString() + " väri: " + vari;
    }
}
```

Luokka määrittelee oliomuuttujan värin talletusta varten. Koordinaatit on valmiiksi määriteltynä yliluokassa. Merkkijonoesityksestä halutaan muuten samanlainen kuin pisteellä, mutta väri tulee myös ilmaista. Ylikirjoitettu metodi `toString` kutsuu yliluokan toString-metodia ja lisää sen tulokseen pisteen värin.


Seuraavassa on esimerkki, jossa listalle laitetaan muutama piste. Osa pisteistä on "normaaleja" ja osa väripisteitä. Lopulta tulostetaan listalla olevat pisteet. Jokaisen pisteen metodi toString suoritetaan pisteen todellisen tyypin perusteella, vaikka lista tuntee kaikki pisteet `Piste`-tyyppisinä.


```java
public class Main {
    public static void main(String[] args) {
        ArrayList<Piste> pisteet = new ArrayList<>();
        pisteet.add(new Piste(4, 8));
        pisteet.add(new VariPiste(1, 1, "vihreä"));
        pisteet.add(new VariPiste(2, 5, "sininen"));
        pisteet.add(new Piste(0, 0));

        for (Piste p: pisteet) {
            System.out.println(p);
        }
    }
}
```

<sample-output>
(4, 8) etäisyys 12
(1, 1) etäisyys 2 väri: vihreä
(2, 5) etäisyys 7 väri: sininen
(0, 0) etäisyys 0
</sample-output>


Haluamme ohjelmaamme myös kolmiulotteisen pisteen. Koska kyseessä ei ole värillinen versio, periytetään se luokasta piste.


```java
public class Piste3D extends Piste {

    private int z;

    public Piste3D(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    @Override
    protected String sijainti() {
        return super.sijainti() + ", " + z;    // tulos merkkijono muotoa "x, y, z"
    }

    @Override
    public int manhattanEtaisyysOrigosta() {
        // kysytään ensin yliluokalta x:n ja y:n perusteella laskettua etäisyyttä
        // ja lisätään tulokseen z-koordinaatin vaikutus
        return super.manhattanEtaisyysOrigosta() + Math.abs(z);
    }

    @Override
    public String toString() {
        return "(" + this.sijainti() + ") etäisyys " + this.manhattanEtaisyysOrigosta();
    }
}
```

Kolmiulotteinen piste siis määrittelee kolmatta koordinaattia vastaavan oliomuuttujan ja ylikirjoittaa metodit `sijainti`, `manhattanEtaisyysOrigosta` ja `toString` siten, että ne huomioivat kolmannen ulottuvuuden. Voimme nyt laajentaa edellistä esimerkkiä ja lisätä listalle myös kolmiulotteisia pisteitä.


```java
public class Main {

    public static void main(String[] args) {
        ArrayList<Piste> pisteet = new ArrayList<>();
        pisteet.add(new Piste(4, 8));
        pisteet.add(new VariPiste(1, 1, "vihreä"));
        pisteet.add(new VariPiste(2, 5, "sininen"));
        pisteet.add(new Piste3D(5, 2, 8));
        pisteet.add(new Piste(0, 0));


        for (Piste p: pisteet) {
            System.out.println(p);
        }
    }
}
```

<sample-output>

(4, 8) etäisyys 12
(1, 1) etäisyys 2 väri: vihreä
(2, 5) etäisyys 7 väri: sininen
(5, 2, 8) etäisyys 15
(0, 0) etäisyys 0

</sample-output>


Huomamme, että kolmiulotteisen pisteen metodi `toString` on täsmälleen sama kuin pisteen toString. Voisimmeko jättää toStringin ylikirjoittamatta? Vastaus on kyllä! Kolmiulotteinen piste pelkistyy seuraavanlaiseksi.


```java
public class Piste3D extends Piste {

    private int z;

    public Piste3D(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    @Override
    protected String sijainti() {
        return super.sijainti() + ", " + z;
    }

    @Override
    public int manhattanEtaisyysOrigosta() {
        return super.manhattanEtaisyysOrigosta() + Math.abs(z);
    }
}
```


Mitä tarkalleenottaen tapahtuu kuin kolmiulotteiselle pisteelle kutsutaan toString-metodia? Suoritus etenee seuraavasti.



1. etsitään toString:in määrittelyä luokasta Piste3D, sitä ei löydy joten mennään yliluokkaan
2. etsitään toString:in määrittelyä yliluokasta Piste, metodi löytyy, joten suoritetaan sen koodi
    * suoritettava koodi siis on `return "("+this.sijainti()+") etäisyys "+this.manhattanEtaisyysOrigosta();`
    * esimmäisenä suoritetaan metodi sijainti
    * etsitään metodin sijainti määrittelyä luokasta Piste3D, metodi löytyy ja suoritetaan sen koodi
    * metodin sijainti laskee oman tuloksensa kutsumalla yliluokassa olevaa metodia sijainti
    * seuraavaksi etsitään metodin manhattanEtaisyysOrigosta määrittelyä luokasta Piste3D, metodi löytyy ja suoritetaan sen koodi
    * jälleen metodi laskee tuloksensa kutsuen ensin yliluokassa olevaa samannimistä metodia


Metodikutsun aikaansaama toimintoketju siis on monivaiheinen. Periaate on kuitenkin selkeä: suoritettavan metodin määrittelyä etsitään ensin olion todellisen tyypin määrittelystä ja jos sitä ei löydy edetään yliluokkaan. Ja jos yliluokastakaan ei löydy metodin toteutusta siirrytään etsimään yliluokan yliluokasta jne...


<quiz id="7f53bd2a-48b3-541e-8d6a-d0cd64e16733"></quiz>

<quiz id="439cf6ac-da31-5a1e-bccf-2fd154841f7a"></quiz>


<quiz id="be260d99-07e1-5c20-843a-9f1e563ae07c"></quiz>

## Milloin perintää kannattaa käyttää?


Perintä on väline käsitehierarkioiden rakentamiseen ja erikoistamiseen; aliluokka on aina yliluokan erikoistapaus. Jos luotava luokka on olemassaolevan luokan erikoistapaus, voidaan uusi luokka luoda perimällä olemassaoleva luokka. Esimerkiksi auton osiin liittyvässä esimerkissä moottori *on* osa, mutta moottoriin liittyy lisätoiminnallisuutta mitä jokaisella osalla ei ole.


Perittäessä aliluokka saa käyttöönsä yliluokan toiminnallisuudet. Jos aliluokka ei tarvitse tai käytä perittyä toiminnallisuutta, ei perintä ole perusteltua. Perityt luokat perivät yliluokkiensa metodit ja rajapinnat, eli aliluokkia voidaan käyttää missä tahansa missä yliluokkaa on käytetty. Perintähierarkia kannattaa pitää matalana, sillä hierarkian ylläpito ja jatkokehitys vaikeutuu perintöhierarkian kasvaessa. Yleisesti ottaen, jos perintähierarkian korkeus on yli 2 tai 3, ohjelman rakenteessa on todennäköisesti parannettavaa.


Perinnän käyttöä tulee miettiä. Esimerkiksi luokan `Auto` periminen luokasta `Osa` (tai `Moottori`) olisi väärin. Auto *sisältää* moottorin ja osia, mutta auto ei ole moottori tai osa. Voimme yleisemmin ajatella että **jos olio omistaa tai koostuu toisista olioista, ei perintää tule käyttää**.


Perintää käytettäessä tulee varmistaa että <a href="https://en.wikipedia.org/wiki/Single_responsibility_principle" target="_blank">Single Responsibility Principle</a> pätee myös perittäessä. Jokaisella luokalla tulee olla vain yksi syy muuttua. Jos huomaat että perintä lisää luokan vastuita, tulee luokka pilkkoa useammaksi luokaksi.

<br/>

### Esimerkki perinnän väärinkäytöstä


Pohditaan postituspalveluun liittyviä luokkia `Asiakas`, joka sisältää asiakkaan tiedot, ja `Tilaus`, joka perii asiakkaan tiedot ja sisältää tilattavan tavaran tiedot. Luokassa `Tilaus` on myös metodi `postitusOsoite`, joka kertoo tilauksen postitusosoitteen.


```java
public class Asiakas {

    private String nimi;
    private String osoite;

    public Asiakas(String nimi, String osoite) {
        this.nimi = nimi;
        this.osoite = osoite;
    }

    public String getNimi() {
        return nimi;
    }

    public String getOsoite() {
        return osoite;
    }

    public void setOsoite(String osoite) {
        this.osoite = osoite;
    }
}
```

```java
public class Tilaus extends Asiakas {

    private String tuote;
    private String lukumaara;

    public Tilaus(String tuote, String lukumaara, String nimi, String osoite) {
        super(nimi, osoite);
        this.tuote = tuote;
        this.lukumaara = lukumaara;
    }

    public String getTuote() {
        return tuote;
    }

    public String getLukumaara() {
        return lukumaara;
    }

    public String postitusOsoite() {
        return this.getNimi() + "\n" + this.getOsoite();
    }
}
```


Yllä perintää on käytetty väärin. Luokkaa perittäessä aliluokan tulee olla yliluokan erikoistapaus; tilaus ei ole asiakkaan erikoistapaus. Väärinkäyttö ilmenee single responsibility principlen rikkomisena: luokalla `Tilaus` on vastuu sekä asiakkaan tietojen ylläpidosta, että tilauksen tietojen ylläpidosta.


Ratkaisussa piilevä ongelma tulee esiin kun mietimme mitä käy asiakkaan osoitteen muuttuessa.


Osoitteen muuttuessa joutuisimme muuttamaan *jokaista* kyseiseen asiakkaaseen liittyvää tilausoliota, mikä ei missään nimessä ole toivottua. Parempi ratkaisu olisi kapseloida `Asiakas` `Tilaus`-luokan oliomuuttujaksi. Jos ajattelemme tarkemmin tilauksen semantiikkaa, tämä on selvää. *Tilauksella on asiakas*.


Muutetaan luokkaa `Tilaus` siten, että se sisältää `Asiakas`-viitteen.


```java
public class Tilaus {

    private Asiakas asiakas;
    private String tuote;
    private String lukumaara;

    public Tilaus(Asiakas asiakas, String tuote, String lukumaara) {
        this.asiakas = asiakas;
        this.tuote = tuote;
        this.lukumaara = lukumaara;
    }

    public String getTuote() {
        return tuote;
    }

    public String getLukumaara() {
        return lukumaara;
    }

    public String postitusOsoite() {
        return this.asiakas.getNimi() + "\n" + this.asiakas.getOsoite();
    }
}
```

Yllä oleva luokka `Tilaus` on nyt parempi. Metodi `postitusosoite` käyttää *asiakas*-viitettä postitusosoitteen saamiseen sen sijaan että luokka perisi luokan `Asiakas`. Tämä helpottaa sekä ohjelman ylläpitoa, että sen konkreettista toiminnallisuutta.


Nyt asiakkaan muuttaessa tarvitsee muuttaa vain asiakkaan tietoja, tilauksiin ei tarvitse tehdä muutoksia.

<programming-exercise name='Warehousing (7 parts)' tmcname='part09-Part09_03.Warehousing'>


<!-- Tehtäväpohjassa tulee mukana luokka `Varasto`, jonka tarjoamat konstruktorit ja metodit ovat seuraavat: -->
The exercise template contains a class `Warehouse`, which has the following constructors and methods:



<!-- - **public Varasto(double tilavuus)** - Luo tyhjän varaston, jonka vetoisuus eli tilavuus annetaan parametrina; sopimaton tilavuus (<=0) luo käyttökelvottoman varaston, jonka tilavuus on 0. -->
- **public Warehouse(double capacity)** - Creates an empty warehouse, which has the capacity provided as a parameter; an invalid capacity (<=0) creates a useless warehouse, with the the capacity 0.

<!-- - **public double getSaldo()** - Palauttaa arvonaan varaston saldon, eli varastossa olevan tavaran tilavuuden. -->
- **public double getBalance()** - Returns the balance of the warehouse, i.e. the capacity which is taken up by the items in the warehouse.

<!-- - **public double getTilavuus()** - Palauttaa arvonaan varaston kokonaistilavuuden (eli sen, joka annettiin konstruktorille). -->
- **public double getCapacity()** - Returns the total capacity of the warehouse (i.e. the one that was provided in the constructor).

<!-- - **public double paljonkoMahtuu()** - Palauttaa arvonaan tiedon, paljonko varastoon vielä mahtuu. -->
- **public double howMuchSpaceLeft()** - Returns a value telling how much space is left in the warehouse.

<!-- - **public void lisaaVarastoon(double maara)** - Lisää varastoon pyydetyn määrän; jos määrä on negatiivinen, mikään ei muutu, jos kaikki pyydetty ei enää mahdu, varasto laitetaan täydeksi ja loput määrästä "heitetään menemään", "vuotaa yli". -->
- **public void addToWarehouse(double amount)** - Adds the desired amount to the warehouse; if the amount is negative, nothing changes, and if everything doesn't fit, then the warehouse is filled up and the rest is "thrown away" / "overflows".

<!-- - **public double otaVarastosta(double maara)** - Otetaan varastosta pyydetty määrä, metodi palauttaa paljonko **saadaan**. Jos pyydetty määrä on negatiivinen, mikään ei muutu ja palautetaan nolla. Jos pyydetään enemmän kuin varastossa on, annetaan mitä voidaan ja varasto tyhjenee. -->
- **public double takeFromWarehouse(double amount)** - Take the desired amount from the warehouse. The method returns much we actually **get**. If the desired amount is negative, nothing changes and we return 0. If the desired amount is greater than the amount the warehouse contains, we get all there is to take and the warehouse is emptied.

<!-- - **public String toString()** - Palauttaa olion tilan merkkijonoesityksenä tyyliin `saldo = 64.5, tilaa 123.5` -->
- **public String toString()** - Returns the state of the object represented as a string like this `balance = 64.5, space left 123.5`


<!-- Tehtävässä rakennetaan `Varasto`-luokasta useampia erilaisia varastoja. -->
In this exercise we build variations of a warehouse based on the `Warehouse` class.


<!-- <h2>Tuotevarasto, vaihe 1</h2> -->
<h2>Product warehouse, step 1</h2>

<!-- Luokka `Varasto` hallitsee tuotteen määrään liittyvät toiminnot. Nyt tuotteelle halutaan lisäksi tuotenimi ja nimen käsittelyvälineet. **Ohjelmoidaan Tuotevarasto Varaston aliluokaksi!** Toteutetaan ensin pelkkä yksityinen oliomuuttuja tuotenimelle, konstruktori ja getteri nimikentälle: -->

The class `Warehouse` handles the functions related to the amount of a product. Now we want product name for the product and a way to handle the name. **Let's write ProductWarehouse as a subclass of Warehouse!** First, we'll just create a private object variable for the product name, a constructor, and a getter for the name field:

<!-- - **public Tuotevarasto(String tuotenimi, double tilavuus)** - Luo tyhjän tuotevaraston. Tuotteen nimi ja varaston tilavuus annetaan parametrina. -->
- **public ProductWarehouse(String productName, double capacity)** - Creates an empty product warehouse. The name of the product and the capacity of the warehouse are provided as parameters.

<!-- - **public String getNimi()** - Palauttaa arvonaan tuotteen nimen. -->
- **public String getName()** - Returns the name of the product.


<!-- *Muista millä tavoin konstruktori voi ensi toimenaan suorittaa yliluokan konstruktorin!* -->
*Remind yourself of how a constructor can run the constructor of the superclass as its first action!*


<!-- Käyttöesimerkki: -->
Example usage:

<!-- ```java
Tuotevarasto mehu = new Tuotevarasto("Juice", 1000.0);
mehu.lisaaVarastoon(1000.0);
mehu.otaVarastosta(11.3);
System.out.println(mehu.getNimi()); // Juice
System.out.println(mehu);           // saldo = 988.7, tilaa 11.3
``` -->

```java
ProductWarehouse juice = new ProductWarehouse("Juice", 1000.0);
juice.addToWarehouse(1000.0);
juice.otaVarastosta(11.3);
System.out.println(juice.getName()); // Juice
System.out.println(juice);           // balance = 988.7, space left 11.3
```

<!-- <sample-output>
Juice
saldo = 988.7, vielä tilaa 11.3
</sample-output> -->


<sample-output>

Juice
balance = 988.7, space left 11.3

</sample-output>


<!-- <h2>Tuotevarasto, vaihe 2</h2> -->
<h2>Product warehouse, step 2</h2>


<!-- Kuten edellisestä esimerkistä näkee, Tuotevarasto-olion perimä `toString()` ei tiedä (tietenkään!) mitään tuotteen nimestä. *Asialle on tehtävä jotain!* Lisätään samalla myös setteri tuotenimelle: -->

As we can see from the previous example, the `toString()` inherited by the ProductWarehouse object doesn't (obviously!) know anything about the product name. *Something must be done!* Let's also add a setter for the product name while we're at it:

<!-- - **public void setNimi(String uusiNimi)** - asettaa tuotteelle uuden nimen. -->
- **public void setName(String newName)** - sets a new name for the product.

<!-- - **public String toString()** - palauttaa olion tilan merkkijonoesityksenä tyyliin `Juice: saldo = 64.5, tilaa 123.5` -->
- **public String toString()** -  Returns the state of the object represented as a string like this `Juice: balance = 64.5, space left 123.5`


<!-- Uuden `toString()`-metodin voisi toki ohjelmoida käyttäen yliluokalta perittyjä gettereitä, joilla perittyjen, mutta piilossa pidettyjen kenttien arvoja saa käyttöönsä. Koska yliluokkaan on kuitenkin jo ohjelmoitu tarvittava taito varastotilanteen merkkiesityksen tuottamiseen, miksi nähdä vaivaa sen uudelleen ohjelmointiin. Käytä siis hyväksesi perittyä `toString`iä. -->

The new `toString()` method could be written using the getters inherited from the superclass, which would give access to values of inherited, but still hidden fields. However, the superclass already has the desired functionality to provide a string representation of the warehouse state, so why bother recreating that functionality? Just take advantage of the inherited `toString()`.


<!-- *Muista miten korvattua metodia voi kutsua aliluokassa!* -->
*Remind yourself of how to call an overridden method in a subclass!*


<!-- Käyttöesimerkki: -->
Usage example:


<!-- ```java
Tuotevarasto mehu = new Tuotevarasto("Juice", 1000.0);
mehu.lisaaVarastoon(1000.0);
mehu.otaVarastosta(11.3);
System.out.println(mehu.getNimi()); // Juice
mehu.lisaaVarastoon(1.0);
System.out.println(mehu);           // Juice: saldo = 989.7, tilaa 10.299999999999955
``` -->

```java
ProductWarehouse juice = new ProductWarehouse("Juice", 1000.0);
juice.addToWarehouse(1000.0);
juice.takeFromWarehouse(11.3);
System.out.println(juice.getName()); // Juice
juice.addToWarehouse(1.0);
System.out.println(juice);           // Juice: balance = 989.7, space left 10.299999999999955
```

<!-- <sample-output>

Juice
Juice: saldo = 989.7, tilaa 10.299999999999955

</sample-output> -->


<sample-output>

Juice
Juice: balance = 989.7, space left 10.299999999999955

</sample-output>


<!-- <h2>Muutoshistoria</h2> -->
<h2>Change History, step 1</h2>


<!-- Toisinaan saattaa olla kiinnostavaa tietää, millä tavoin jonkin tuotteen varastotilanne muuttuu: onko varasto usein hyvin vajaa, ollaanko usein ylärajalla, onko vaihelu suurta vai pientä, jne. Varustetaan siksi `Tuotevarasto`-luokka taidolla muistaa tuotteen määrän muutoshistoriaa. -->

Sometimes it might be useful to know how the inventory of a product changes over time: Is the inventory often low? Are we usually at the limit? Are the changes in inventory big or small? Etc. Thus we should give the `ProducWarehouse` class the ability to remember the changes in the amount of a product.

<!-- Aloitetaan apuvälineen laadinnalla. -->
Let's begin by creating the tool aiding the functionality.


<!-- Muutoshistorian muistamisen voisi toki toteuttaa suoraankin `ArrayList<Double>`-oliona luokassa *Tuotevarasto*, mutta nyt laaditaan kuitenkin oma *erikoistettu väline* tähän tarkoitukseen. Väline tulee toteuttaa kapseloimalla `ArrayList<Double>`-olio. -->

The storing of the change history could of course have been done using an `ArrayList<Double>` object in the class *ProductWarehouse*, however, we want our own *specialized tool* for this purpose. The tool should be implemented by encapsulating the `ArrayList<Double>` object.

<!-- `Muutoshistoria`-luokan julkiset konstruktorit ja metodit: -->
Public constructors and methods of the `ChangeHistory` class:


<!-- - **public Muutoshistoria()** luo tyhjän `Muutoshistoria`-olion. -->
- **public ChangeHistory()** creates an empty `ChangeHistory` object.

<!-- - **public void lisaa(double tilanne)** lisää muutoshistorian viimeisimmäksi muistettavaksi määräksi parametrina annetun tilanteen. -->
- **public void add(double status)** adds provided status as the latest amount to remember in the change history.

<!-- - **public void nollaa()** tyhjää muistin. -->
- **public void clear()** empties the history.

<!-- - **public String toString()** palauttaa muutoshistorian merkkijonoesityksen. *ArrayList-luokan antama merkkijonoesitys kelpaa sellaisenaan.* -->
- **public String toString()** returns the string representation of the change history. *The string representation provided by the ArrayList class is sufficient.*


<!-- <h2>Muutoshistoria, vaihe 2</h2> -->
<h2>Change History, step 2</h2>

<!-- Täydennä `Muutoshistoria`-luokkaa analyysimetodein: -->
Build on the `ChangeHistory` class by adding analysis methods:


<!-- - **public double maxArvo()** palauttaa muutoshistorian suurimman arvon. Jos historia on tyhjä, metodi palauttaa nollan. -->
- **public double maxValue()** returns the largest value in the change history. If the history is empty, the method should return zero.

<!-- - **public double minArvo()** palauttaa muutoshistorian pienimmän arvon. Jos historia on tyhjä, metodi palauttaa nollan. -->
- **public double minValue()** returns the smallest value in the change history. If the history is empty, the method should return zero.

<!-- - **public double keskiarvo()** palauttaa muutoshistorian arvojen keskiarvon. Jos historia on tyhjä, metodi palauttaa nollan. -->
- **public double avarage()** returns the average of the values in the change history. If the history is empty, the method should return zero.


<!-- Metodien ei tule muokata sisäisen listan järjestystä. -->
The methods should not modify the order of the encapsulated list.


<!-- <h2>Muistava tuotevarasto, vaihe 1</h2> -->
<h2>Product warehouse with history, step 1</h2>

<!-- Toteuta luokan `Tuotevarasto` aliluokkana `MuistavaTuotevarasto`. Uusi versio tarjoaa vanhojen lisäksi varastotilanteen muutoshistoriaan liittyviä palveluita. Historiaa hallitaan `Muutoshistoria`-oliolla. -->

Implement `ProductWarehouseWithHistory` as a subclass of `ProductWarehouse`. In addition to all the previous features this new warehouse also provides services related to the change history of the warehouse inventory. The history is managed using the `ChangeHistory` object.

<!-- Julkiset konstruktorit ja metodit: -->
Public constructors and methods:

<!-- - **public MuistavaTuotevarasto(String tuotenimi, double tilavuus, double alkuSaldo)**	luo tuotevaraston. Tuotenimi, vetoisuus ja alkusaldo annetaan parametrina. -->
<!-- Aseta alkusaldo sekä varaston alkusaldoksi että muutoshistorian ensimmäiseksi arvoksi. -->

- **public ProductWarehouseWithHistory(String productName, double capacity, double initialBalance)** creates a product warehouse. The product name, capacity, and initial balance are provided as parameters.
Set the initial balance as the initial balance of the warehouse, as well as the first value of the change history.

<!-- - **public String historia()** palauttaa tuotehistorian tyyliin `[0.0, 119.2, 21.2]`.  Käytä Muutoshistoria-olion merkkiesitystä sellaisenaan. -->
- **public String history()** returns the product history like this `[0.0, 119.2, 21.2]`. Use the string representation of the ChangeHistory object as is.


<!-- **Huomaa** että tässä esiversiossa historia ei vielä toimi kunnolla; nyt vasta vain aloitussaldo muistetaan. -->
**NB** in this initial version the history is not yet working properly; currently it only remembers the initial balance.


<!-- Käyttöesimerkki: -->
Usage example:


<!-- ```java
// tuttuun tapaan:
MuistavaTuotevarasto mehu = new MuistavaTuotevarasto("Juice", 1000.0, 1000.0);
mehu.otaVarastosta(11.3);
System.out.println(mehu.getNimi()); // Juice
mehu.lisaaVarastoon(1.0);
System.out.println(mehu);           // Juice: saldo = 989.7, vielä tilaa 10.3

// jne

// mutta vielä historia() ei toimi kunnolla:
System.out.println(mehu.historia()); // [1000.0]
// saadaan siis vasta konstruktorin asettama historian alkupiste...
``` -->

```java
// the usual:
ProductWarehouseWithHistory juice = new ProductWarehouseWithHistory("Juice", 1000.0, 1000.0);
juice.takeFromWarehouse(11.3);
System.out.println(juice.getName()); // Juice
juice.addToWarehouse(1.0);
System.out.println(juice);           // Juice: balance = 989.7, space left 10.3

// etc

// however, history() still doesn't work properly:
System.out.println(juice.history()); // [1000.0]
// so we only get the initial state of the history set by the constructor...
```

<!-- <sample-output>
Juice
Juice: saldo = 989.7, vielä tilaa 10.299999999999955
[1000.0]
</sample-output> -->


<sample-output>
Juice
Juice: balance = 989.7, space left 10.299999999999955
[1000.0]
</sample-output>


<!-- <h2>Muistava tuotevarasto, vaihe 2</h2> -->
<h2>Product warehouse with history, step 2</h2>


<!-- *On aika aloittaa historia!* Ensimmäinen versio ei historiasta tiennyt kuin alkupisteen. Täydennä luokkaa metodein -->
*It's time to make history!* The first version didn't know anything but the initial state of the history. Expand the class with the following methods


<!-- - **public void lisaaVarastoon(double maara)** toimii kuin *Varasto*-luokan metodi, mutta muuttunut tilanne kirjataan historiaan.**Huom:** historiaan tulee kirjata lisäyksen jälkeinen varastosaldo, ei lisättävää määrää! -->
- **public void addToWarehouse(double amount)** works just like the method in the `Warehouse` class, but we also record the changed state to the history. **NB:** the value recorded in the history should be the warehouse's balance after adding, not the amount added!

<!-- - **public double otaVarastosta(double maara)** toimii kuin `Varasto`-luokan metodi, mutta muuttunut tilanne kirjataan historiaan. **Huom:** historiaan tulee kirjata poiston jälkeinen varastosaldo, ei poistettavaa määrää! -->
- **public double takeFromWarehouse(double amount)** works just like the method in the `Warehouse` class, but we also record the changed state to the history. **NB:** the value recorded in the history should be the warehouse's balance after removing, not the amount removed!


<!-- Käyttöesimerkki: -->
Usage example:


<!-- ```java
// tuttuun tapaan:
MuistavaTuotevarasto mehu = new MuistavaTuotevarasto("Juice", 1000.0, 1000.0);
mehu.otaVarastosta(11.3);
System.out.println(mehu.getNimi()); // Juice
mehu.lisaaVarastoon(1.0);
System.out.println(mehu);           // Juice: saldo = 989.7, vielä tilaa 10.3

// jne

// mutta nyt on historiaakin:
System.out.println(mehu.historia()); // [1000.0, 988.7, 989.7]
``` -->

```java
// the usual:
ProductWarehouseWithHistory juice = new ProductWarehouseWithHistory("Juice", 1000.0, 1000.0);
juice.takeFromWarehouse(11.3);
System.out.println(juice.getName()); // Juice
juice.takeFromWarehouse(1.0);
System.out.println(juice);           // Juice: balance = 989.7, space left 10.3

// etc

// and now we have the history:
System.out.println(juice.history()); // [1000.0, 988.7, 989.7]
```

<!-- <sample-output>
Juice
Juice: saldo = 989.7, vielä tilaa 10.299999999999955
[1000.0, 988.7, 989.7]
</sample-output> -->


<sample-output>
Juice
Juice: balance = 989.7, space left 10.299999999999955
[1000.0, 988.7, 989.7]
</sample-output>


<!-- *Muista miten korvaava metodi voi käyttää hyväkseen korvattua metodia!* -->
*Remember how an overriding method can take advantage of the overridden method!*



<!-- <h2>Muistava tuotevarasto, vaihe 3</h2> -->
<h2>Product warehouse with history, step 3</h2>


<!-- Täydennä luokkaa metodilla -->
Expand the class with the method


<!-- - **public void tulostaAnalyysi()**, joka tulostaa tuotteeseen liittyviä historiatietoja esimerkin esittämään tapaan. -->
- **public void printAnalysis()**, which prints history related information for the product in the way presented in the example.


<!-- Käyttöesimerkki: -->
Usage example:


<!-- ```java
MuistavaTuotevarasto mehu = new MuistavaTuotevarasto("Juice", 1000.0, 1000.0);
mehu.otaVarastosta(11.3);
mehu.lisaaVarastoon(1.0);
//System.out.println(mehu.historia()); // [1000.0, 988.7, 989.7]

mehu.tulostaAnalyysi();
``` -->

```java
ProductWarehouseWithHistory juice = new ProductWarehouseWithHistory("Juice", 1000.0, 1000.0);
juice.takeFromWarehouse(11.3);
juice.addToWarehouse(1.0);
//System.out.println(juice.history()); // [1000.0, 988.7, 989.7]

juice.printAnalysis();
```

<!-- <sample-output>

Tuote: Juice
Historia: [1000.0, 988.7, 989.7]
Suurin tuotemäärä: 1000.0
Pienin tuotemäärä: 988.7
Keskiarvo: 992.8

</sample-output> -->


<sample-output>

Product: Juice
History: [1000.0, 988.7, 989.7]
Largest amount of product: 1000.0
Smallest amount of product: 988.7
Average: 992.8

</sample-output>


</programming-exercise>


## Abstraktit luokat


Perintähierarkiaa pohtiessa tulee joskus esille tilanteita, missä on olemassa selkeä käsite, mutta käsite ei sellaisenaan ole hyvä kandidaatti olioksi. Hyötyisimme käsitteestä perinnän kannalta, sillä se sisältää muuttujia ja toiminnallisuuksia, jotka ovat kaikille käsitteen periville luokille samoja, mutta toisaalta käsitteestä itsestään ei pitäisi pystyä tekemään olioita.


Abstrakti luokka yhdistää rajapintoja ja perintää. Niistä ei voi tehdä ilmentymiä, vaan ilmentymät tehdään abstraktin luokan aliluokista.  Abstrakti luokka voi sisältää sekä normaaleja metodeja, joissa on metodirunko, että abstrakteja metodeja, jotka sisältävät ainoastaan metodimäärittelyn. Abstraktien metodien toteutus jätetään perivän luokan vastuulle. Yleisesti ajatellen abstrakteja luokkia käytetään esimerkiksi kun abstraktin luokan kuvaama käsite ei ole selkeä itsenäinen käsite. Tällöin siitä ei tule pystyä tekemään ilmentymiä.


Sekä abstraktin luokan että abstraktien metodien määrittelyssä käytetään avainsanaa `abstract`. Abstrakti luokka määritellään lauseella `public abstract class *LuokanNimi*`, abstrakti metodi taas lauseella `public abstract palautustyyppi metodinNimi`. Tarkastellaan seuraavaa abstraktia luokkaa `Toiminto`, joka tarjoaa rungon toiminnoille ja niiden suorittamiselle.


```java
public abstract class Toiminto {

    private String nimi;

    public Toiminto(String nimi) {
        this.nimi = nimi;
    }

    public String getNimi() {
        return this.nimi;
    }

    public abstract void suorita(Scanner lukija);
}
```

Abstrakti luokka `Toiminto` toimii runkona toimintojen toteuttamiseen. Esimerkiksi pluslaskun voi toteuttaa perimällä luokka `Toiminto` seuraavasti.


```java
public class Pluslasku extends Toiminto {

    public Pluslasku() {
        super("Pluslasku");
    }

    @Override
    public void suorita(Scanner lukija) {
        System.out.print("Anna ensimmäinen luku: ");
        int eka = Integer.valueOf(lukija.nextLine());
        System.out.print("Anna toinen luku: ");
        int toka = Integer.valueOf(lukija.nextLine());

        System.out.println("Lukujen summa on " + (eka + toka));
    }
}
```

Koska kaikki `Toiminto`-luokan perivät luokat ovat myös tyyppiä toiminto, voimme rakentaa käyttöliittymän `Toiminto`-tyyppisten muuttujien varaan. Seuraava luokka `Kayttoliittyma` sisaltaa listan toimintoja ja lukijan. Toimintoja voi lisätä käyttöliittymään dynaamisesti.


```java
public class Kayttoliittyma {

    private Scanner lukija;
    private ArrayList<Toiminto> toiminnot;

    public Kayttoliittyma(Scanner lukija) {
        this.lukija = lukija;
        this.toiminnot = new ArrayList<>();
    }

    public void lisaaToiminto(Toiminto toiminto) {
        this.toiminnot.add(toiminto);
    }

    public void kaynnista() {
        while (true) {
            tulostaToiminnot();
            System.out.println("Valinta: ");

            String valinta = this.lukija.nextLine();
            if (valinta.equals("0")) {
                break;
            }

            suoritaToiminto(valinta);
            System.out.println();
        }
    }

    private void tulostaToiminnot() {
        System.out.println("\t0: Lopeta");
        int i = 0;
        while (i < this.toiminnot.size()) {
            String toiminnonNimi = this.toiminnot.get(i).getNimi();
            System.out.println("\t" + (i + 1) + ": " + toiminnonNimi);
            i = i + 1;
        }
    }

    private void suoritaToiminto(String valinta) {
        int toiminto = Integer.valueOf(valinta);

        Toiminto valittu = this.toiminnot.get(toiminto - 1);
        valittu.suorita(lukija);
    }
}
```


Käyttöliittymä toimii seuraavasti:


```java
Kayttoliittyma kayttolittyma = new Kayttoliittyma(new Scanner(System.in));
kayttolittyma.lisaaToiminto(new Pluslasku());

kayttolittyma.kaynnista();
```

<sample-output>

Toiminnot:
        0: Lopeta
        1: Pluslasku
Valinta: **1**
Anna ensimmäinen luku: **8**
Anna toinen luku: **12**
Lukujen summa on 20

Toiminnot:
        0: Lopeta
        1: Pluslasku
Valinta: **0**

</sample-output>


Rajapintojen ja abstraktien luokkien suurin ero on siinä, että abstrakteissa luokissa voidaan määritellä metodien lisäksi myös oliomuuttujia sekä konstruktoreja. Koska abstrakteihin luokkiin voidaan määritellä toiminnallisuutta, voidaan niitä käyttää esimerkiksi oletustoiminnallisuuden määrittelyyn. Yllä käyttöliittymä käytti abstraktissa luokassa määriteltyä toiminnan nimen tallentamista.

<programming-exercise name='DifferentKindsOfBoxes (3 parts)' tmcname='part09-Part09_04.DifferentKindsOfBoxes'>

<!-- Tehtäväpohjan mukana tulee luokat `Tavara` ja `Laatikko`. Luokka `Laatikko` on abstrakti luokka, jossa useamman tavaran lisääminen on toteutettu siten, että kutsutaan aina `lisaa`-metodia. Yhden tavaran lisäämiseen tarkoitettu metodi `lisaa` on abstrakti, joten jokaisen `Laatikko`-luokan perivän laatikon tulee toteuttaa se. Tehtävänäsi on muokata luokkaa `Tavara` ja toteuttaa muutamia erilaisia laatikoita luokan `Laatikko` pohjalta. -->
In the exercise template you'll find the classes `Item` and `Box`. `Box` is an abstract class, where adding multiple items is implemented by repeatedly calling the `add`-method. The `add`-method, meant for adding a single item, is abstract, so every class that inherits it, must implement it. Your assignment is to edit the `Box`-class and to implement different kinds of boxes based on the `Box` class.


<!-- ```java
import java.util.Collection;

public abstract class Laatikko {

    public abstract void lisaa(Tavara tavara);

    public void lisaa(Collection<Tavara> tavarat) {
        for (Tavara t: tavarat) {
            lisaa(t);
        }
    }

    public abstract boolean onkoLaatikossa(Tavara tavara);
}
``` -->
```java
import java.util.ArrayList;

public abstract class Box {

    public abstract void add(Item item);

    public void add(ArrayList<Item> items) {
        for (Item item : items) {
            Box.this.add(item);
        }
    }

    public abstract boolean isInBox(Item item);
}
```


<!-- <h2>Tavaran muokkaus</h2>


Toteuta `Tavara`-luokalle metodit `equals` ja `hashCode`, joiden avulla  pääset hyödyntämään erilaisten listojen ja kokoelmien `contains`-metodia. Toteuta metodit siten, että Tavara-luokan oliomuuttujan `paino` arvolla ei ole väliä. *Kannattanee hyödyntää NetBeansin tarjoamaa toiminnallisuutta equalsin ja hashCoden toteuttamiseen.*


<h2>Maksimipainollinen laatikko</h2>


Toteuta luokka `MaksimipainollinenLaatikko`, joka perii luokan `Laatikko`. Maksimipainollisella laatikolla on konstruktori `public MaksimipainollinenLaatikko(int maksimipaino)`, joka määrittelee laatikon maksimipainon. Maksimipainolliseen laatikkoon voi lisätä tavaraa jos ja vain jos tavaran lisääminen ei ylitä laatikon maksimipainoa. -->

<h2>Editing the Item class</h2>

Implement the `equals` and `hashCode` methods for the `Item`-class. They are needed, so that you can use the `contains`-methods of different lists and collections. Implement the methods in such a way that value of the `weight` instance variable of the `Item`-class isn't considered. *It's probably a good idea to make use a certain netbeans functionality to implement the `equals` and `hashCode` methods*

<h2>Box with a max weight</h2>

Implement the class `BoxWithMaxWeight`, that inherits the `Box` class. BoxWithMaxWeight has a constructor `public BoxWithMaxWeight(int capacity)`, that defines the max weight allowed for that box. You can add an item to a BoxWithMaxWeight when and only when, adding the item won't cause the boxes max weight capacity to be exceeded.

<!-- ```java
MaksimipainollinenLaatikko kahviLaatikko = new MaksimipainollinenLaatikko(10);
kahviLaatikko.lisaa(new Tavara("Saludo", 5));
kahviLaatikko.lisaa(new Tavara("Pirkka", 5));
kahviLaatikko.lisaa(new Tavara("Kopi Luwak", 5));

System.out.println(kahviLaatikko.onkoLaatikossa(new Tavara("Saludo")));
System.out.println(kahviLaatikko.onkoLaatikossa(new Tavara("Pirkka")));
System.out.println(kahviLaatikko.onkoLaatikossa(new Tavara("Kopi Luwak")));
``` -->
```java
BoxWithMaxWeight coffeeBox = new BoxWithMaxWeight(10);
coffeeBox.add(new Item("Saludo", 5));
coffeeBox.add(new Item("Pirkka", 5));
coffeeBox.add(new Item("Kopi Luwak", 5));

System.out.println(coffeeBox.isInBox(new Item("Saludo")));
System.out.println(coffeeBox.isInBox(new Item("Pirkka")));
System.out.println(coffeeBox.isInBox(new Item("Kopi Luwak")));
```


<sample-output>

true
true
false

</sample-output>


<!-- <h2>Yhden tavaran laatikko ja Hukkaava laatikko</h2>


Toteuta seuraavaksi luokka `YhdenTavaranLaatikko`, joka perii luokan `Laatikko`. Yhden tavaran laatikolla on konstruktori `public YhdenTavaranLaatikko()`, ja siihen mahtuu tasan yksi tavara. Jos tavara on jo laatikossa sitä ei tule vaihtaa. Laatikkoon lisättävän tavaran painolla ei ole väliä. -->

<h2>One item box and the misplacing box</h2>

Next implement the class `OneItemBox`, that inherits the `Box` class. `OneItemBox` has constructor the `public OneItemBox()`, and it has the capacity of exactly one item. If there is already an item in the box, it must not be switched. The weight of the item added to the box is irrelevant.



<!-- ```java
YhdenTavaranLaatikko laatikko = new YhdenTavaranLaatikko();
laatikko.lisaa(new Tavara("Saludo", 5));
laatikko.lisaa(new Tavara("Pirkka", 5));

System.out.println(laatikko.onkoLaatikossa(new Tavara("Saludo")));
System.out.println(laatikko.onkoLaatikossa(new Tavara("Pirkka")));
``` -->
```java
OneItemBox box = new OneItemBox();
box.add(new Item("Saludo", 5));
box.add(new Item("Pirkka", 5));

System.out.println(box.isInBox(new Tavara("Saludo")));
System.out.println(box.isInBox(new Tavara("Pirkka")));
```
<sample-output>

true
false

</sample-output>


<!-- Toteuta seuraavaksi luokka `HukkaavaLaatikko`, joka perii luokan `Laatikko`. Hukkaavalla laatikolla on konstruktori `public HukkaavaLaatikko()`. Hukkaavaan laatikkoon voi lisätä kaikki tavarat, mutta tavaroita ei löydy niitä etsittäessä. Laatikkoon lisäämisen tulee siis aina onnistua, mutta metodin `onkoLaatikossa` kutsumisen tulee aina palauttaa false. -->
Next implement the class `MisplacingBox`, that inherits the `Box`-class. MisplacingBox has a constructor `public MisplacingBox()`. You can add any items to misplacing box, but items can never be found when looked for. In other words adding to the box must always succeed, but calling the method `isInbox` must always return false.


<!-- ```java
HukkaavaLaatikko laatikko = new HukkaavaLaatikko();
laatikko.lisaa(new Tavara("Saludo", 5));
laatikko.lisaa(new Tavara("Pirkka", 5));

System.out.println(laatikko.onkoLaatikossa(new Tavara("Saludo")));
System.out.println(laatikko.onkoLaatikossa(new Tavara("Pirkka")));
``` -->
```java
MisplacingBox box = new MisplacingBox();
box.add(new Item("Saludo", 5));
box.add(new Item("Pirkka", 5));

System.out.println(box.isInBox(new Item("Saludo")));
System.out.println(box.insInBox(new Item("Pirkka")));
```

<sample-output>

false
false

</sample-output>

</programming-exercise>

