# Kotitehtävä 1
### Tehtävänanto

"Lue aikataulussa mainittu lukukläksy. Tee Puppet-moduuli, joka tekee jotain lukuläksyn aiheesta. Raportoi."

Läksyssä tehdään Puppet-moduuli, joka varmistaa, että nmap-paketti on asennettuna koneella.


### Testiympäristö

	- Haaga-Helian laboratorioluokan 5004 työasema
	- Xubuntu 15.04 LTS 64 bit
	- Virtualbox-kone
		- 2048 MB RAM
		- 12 GB tallennustilaa

### Puppetin asentaminen

Ensiksi asennetaan Puppet-ohjelmisto Ubuntun omasta paketinhallinnasta

	$ sudo apt-get update && sudo apt-get install puppet -y

Luodaan käyttäjän kotihakemistoon uusi hakemisto "puppet" ja sen alle alihakemisto "modules".

	$ mkdir -p ~/puppet/modules/

Nyt meillä on Puppet asennettuna ja sille on luotu valmiiksi hakemisto moduulejamme varten.


### Moduulin luominen ja testaaminen

Nmap-moduulia varten luodaan "puppet/modules/"-hakemistoon uusi alihakemisto moduuliamme varten. Nimetään moduuli kuvaavalla nimellä.

	$ mkdir nmap/

Luomamme hakemiston alle luodaan jälleen yksi alihakemisto "manifests". Hakemistossa ovat kaikki Puppet-tiedostot joissa on pääte ".pp".

	$ mkdir manifests/

Hakemistoon luomme luokan "init.pp", joka sisältää Puppet moduulin luokkamäärittelyt. Luokan on oltava samanniminen kuin Puppet-moduulimme!

	
	
	class nmap {					# Määritellään luokan nimi
		package { 'nmap:'			# Määritellään mitä moduuli tekee (asentaa tässä tapauksessa paketin)
			ensure => 'installed',	# Määritellään, että paketin on oltava asennettuna
		}
	}

Tallennetaan tiedosto ja ajetaan moduuli koneellamme. Olemme hakemistossa ~/puppet/modules/nmap/manifests. Koska moduulimme asentaa paketin apt-paketinhallintaa käyttäen, tarvitsee toimenpide pääkäyttäjäoikeudet.

	$ sudo -i

Nyt ajamme moduulin kutsumalla luokkaa "nmap".

	$ puppet apply --modulepath ~/puppet/modules/ -e 'class {"nmap":}'

Saamme seuraavan ilmoituksen:

	Notice: Compiled catalog for xubutuu-virtualbox in environment production in 0.35 seconds
	Notice: /Stage[main]/Nmap/Package[nmap]/ensure: ensure changed 'purged' to 'present'
	Notice: Finished catalog run in 4.13 seconds

Tarkistetaan tämän jälkeen, että nmap asentui koneelle.

	$ dpkg -s nmap|grep installed

Komento antaa tulosteen:

	Status: install ok installed


### Pohdinnat

Harjoitus oli yksinkertainen, mutta kuitenkin hyvin opettava. Ymmärrän jo pelkästään tämän harjoituksen jälkeen enemmän keskitetyn hallinnan hyödyllisyyden. Puppet vaikuttaa alkuun erittäin hyvältä valinnalta kurssia varten, koska erityisesti sen dokumentaatio on erittäin kattava.


## Lähteet

http://terokarvinen.com/2013/hello-puppet-revisited-%E2%80%93-on-ubuntu-12-04-lts
http://www.puppetcookbook.com/posts/install-package.html
