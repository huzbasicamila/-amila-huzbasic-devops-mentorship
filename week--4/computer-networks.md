# Computer Networks

## OSI MODEL

OSI MODEL opisuje sedam layera koji se koriste za komuniciranje kompjuterskih sistema koji su umreženi.

 Imamo:

 * Aplikativni sloj 
 * Prezentacijski Sloj
 * Sesijski sloj
 * Transportni sloj
 * Mrežni sloj
 * Sloj veze
 * Fizički sloj

 ### Aplikativni sloj

 Na ovom sloju se nalaze mrežne aplikacije i njihovi protokoli aplikativnog sloja. Tu pripadaju protokoli kao što su HTTP, SMTP, FTP. DNS translacija se odvija na ovom sloju. 

 ### Prezentacijski sloj

 Predstavlja podatke korisnicima u lako razmljivom obliku. Ovaj sloj omogućuje da su podaci čitljivi, brine se formatu i strukturi- 

 ### Sesijski sloj

 Sesijski sloj je odgovoran za uspostavljanje, održavanje i okončavanje veza između različitih aplikacija. 

 ### Transportni sloj

 Zadužen je za prijenos podataka između uređaja. Otkriva i ispravlja greške u prijenosu. Protokoli ovog sloja TCP i UDP. TCP nudi uslugu uspostavljanja veze i garantuje isporuku paketa. UDP pruza brzi i jednostavniji prijenos podataka bez garantovanja o isporuci.
 Transportni sloj dijeli podatke na manje segmente kako bi ih uspjesno prenijeo preko mreže. 

 ### Mrežni sloj

 Mrežni sloj je odgovoran za prenošenje paketa (datagrama) od jednog računara do drugog. Datagrami su obicno sastavljeni od zaglavlja (header) i korisnih podataka (payload). Zaglavlje sadrzi informacije o adresama odredista i izvora, kao i druge kontrole informacija koje su potrebne za uspjesan prijenos podataka. Payload sadrzi stvarne podatke koji se prenose preko mreze. IP Protokol pripada mrežnom sloju.

 ### Sloj veze

 Ovaj sloj se bavi uspostavkom i održavanjem komunikacije između susjednih čvorova u mreži. Glavna funkcija ovog sloja je da osigura pouzdan prijenos podataka preko fizičkog sloja. Ovaj sloj koristi fizicke adrese (MAC ADRESE). MAC adresa je 48-bitna adresa koja se nalazi na mrežnom adapteru svakog uređaja. Ethernet je najpoznatiji protokol sloja veze i često se koristi u lokalnim mrežama (LAN). Podaci se prenose putem Ethernet okvira koji sadrže MAC adrese izvora i odredišta.

 ### Fizički sloj

 Fizicki sloj se bavi fizičkim prijenosom podataka preko fizičkog medija. Fizički sloj koristi različite medije za prijenos podataka, kao što su bakreni kablovi, optički kablovi ili bežične veze (poput WiFi-a). Svaki medij ima svoje karakteristike i metode prijenosa podataka. Fizički sloj se bavi različitim aspektima signalizacije, uključujući načine predstavljanja i prepoznavanja podataka putem električnih, svjetlosnih ili radijskih signala. Signalizacija može biti analogna (npr. električni naponi) ili digitalna (npr. binarni nizovi). Modulacija se koristi za prenošenje digitalnih podataka preko analognih medija. To uključuje pretvaranje digitalnih podataka u odgovarajući oblik analognog signala koji se može prenijeti preko fizičkog medija. Fizički sloj definira brzinu prijenosa podataka, koja se mjeri u bitovima po sekundi (bps).  Fizički sloj definira različite topologije mreže, poput magistralne, zvjezdaste, prstena ili mreže s više točaka pristupa. Ove topologije određuju način na koji su mrežni uređaji povezani preko fizičkog medija.