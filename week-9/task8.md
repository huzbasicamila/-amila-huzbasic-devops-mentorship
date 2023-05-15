# TASK DESCRIPTION

Za kompletiranje Task-8 potrebno je napraviti sljedece:
IAM User 1 ce svoje resurse da kreira unutar eu-central-1 .


 Od AMI image ec2-ime-prezime-web-server napravite novu EC2 instancu ec2-ime-prezime-task-8

 Kreirati DNS record <ime>-<prezime>.awsbosnia.com za Hosted Zone awsbosnia.com (Hosted zone ID: Z3LHP8UIUC8CDK) koji ce da pokazuje na EC2 instancu koju ste krairali. Za kreiranje DNS zapisa korisite AWS CLI AWS kredencijale koji se nalaze unutar sljedeceg excel file-a. AWS CLI konfigurisite tako da koristite named profile aws-bosnia. Za ovaj dio taska mozete da iskorisite change-resource-record-sets AWS CLI komandu. Kada ste dodali novi DNS record njegov Name i Value ispiste uz pomoc komande aws route53 list-hosted-zones i alata jq gdje cete prikazati samo Name i Value za DNS record koji ste vi kreirali odnosno za vase domensko ime.

 Na EC2 instanci ec2-ime-prezime-task-8 kreirati Let's Encrypt SSL certifikat za vasu domenu. Neophodno je omoguciti da se nodejs aplikaciji moze pristupiti preko linka https://<ime>-<prezime>.awsbosnia.com, to verifikujte skrinsotom gdje se vidi validan certifikat u browseru.

 Omoguciti ACM SSL certifikata

 Koristeci openssl komande prikazati koji SSL certitikat koristite i datum njegovog isteka. Probajte korisitit razlicite openssl komande (HINT: Biljeskama za office hours imate knjigu u kojoj mozete pronaci recepte za razlicite openssl komande)

 Importujte Lets Encrypt SSL certifikat unutar AWS Certified Managera.

 Kreirajte Load Balancer gdje cete na nivou Load Balancera da koristite SSL cert koji ste ranije importovali. (Hint: NGINX config je nophodno auzrirati). Load Balancer u pozadini koristi EC2 instancu ec2-ime-prezime-task-8, nije potrebno kreirati ASG.

 Koristeci openssl komande prikazati koji SSL certitikat koristite za vasu domenu i datum njegovog isteka.

 Kreirajte novi SSL certifikat unutar AWS Certified Managera, azurirajte ALB da koristi novi SSL cert koji ste kreirali.

 Koristeci openssl komande prikazati koji SSL certitikat koristite za vasu domenu i datum njegovog isteka.

 Kada zavrsite sa taskom kreirajte AMI image pod nazivom ami-ec2-ime-prezime-task-8 i terminirajte resurse koje ste koristili za izradu taska.

 1. Od AMI image ec2-ime-prezime-web-server napravite novu EC2 instancu ec2-ime-prezime-task-8

 ![ec2-created](/week-9/images/ec2-created_from_ami.PNG "ec2-created")

 2.  Kreirati DNS record <ime>-<prezime>.awsbosnia.com za Hosted Zone awsbosnia.com (Hosted zone ID: Z3LHP8UIUC8CDK) koji ce da pokazuje na EC2 instancu koju ste krairali. Za kreiranje DNS zapisa korisite AWS CLI AWS kredencijale koji se nalaze unutar sljedeceg excel file-a. AWS CLI konfigurisite tako da koristite named profile aws-bosnia. Za ovaj dio taska mozete da iskorisite change-resource-record-sets AWS CLI komandu. Kada ste dodali novi DNS record njegov Name i Value ispiste uz pomoc komande aws route53 list-hosted-zones i alata jq gdje cete prikazati samo Name i Value za DNS record koji ste vi kreirali odnosno za vase domensko ime.

 Za ovaj dio taska koristili smo naredbu aws configure u aws cli. 
 Za AWS Access Key ID i Secret access key koritstimo kredencijale koji se nalaze u excel file-u. Default region name biramo eu-central-1 a default output format biramo json. 

Ova naredba se koristi za programsko upravljanje DNS zapisima u AWS Route 53 hostanoj zoni.

 > $ aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"CREATE","ResourceRecordSet":{"Name":"amila-huzbasic.awsbosnia.com.","Type":"A","TTL":60,"ResourceRecords":[{"Value":"3.65.35.162"}]}}]}

Koristi se za kreiranje novog resursnog zapisa u hostanom zonu sa specificiranim ID-om Z3LHP8UIUC8CDK. 

Parametar --change-batch specificira promjene koje se trebaju napraviti u hostanom zonu. U ovom slučaju, kreira se novi A zapis za domenu amila-huzbasic.awsbosnia.com sa vremenom života (TTL) od 60 sekundi i IP adresom 3.65.35.162.

Radnja CREATE označava da se novi zapis dodaje u hostanu zonu. Postoje i druge moguće radnje, kao što su DELETE i UPSERT, koje odgovarajuće uklanjaju ili ažuriraju postojeći zapis.

3. Na EC2 instanci ec2-ime-prezime-task-8 kreirati Let's Encrypt SSL certifikat za vasu domenu. Neophodno je omoguciti da se nodejs aplikaciji moze pristupiti preko linka https://<ime>-<prezime>.awsbosnia.com, to verifikujte skrinsotom gdje se vidi validan certifikat u browseru.

Kreirali smo Route 53 DNS zapis http://amila-huzbasic.awsbosnia.com/ i usmerili ga na nasu Public IP adresu nase EC2 instance na kojoj se nalazi nasa aplikacija. 
Za kreiranje Lets Encrypt certifikata iskorisiti cemo certbot alat. Certbot je alat koji nam omogucava da automatski generisemo i instaliramo SSL certifikat. Na Amazon Linux Ami 3 cert bot cemo instalirati koristeci pip alat.

```$ sudo dnf install python3 augeas-libs
$ sudo python3 -m venv /opt/certbot/
$ sudo /opt/certbot/bin/pip install --upgrade pip
$ sudo /opt/certbot/bin/pip install certbot certbot-nginx
$ sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
$ sudo certbot --nginx 
 ```

 Outout komande sudo certbot --nginx

 ![ssl](/week-9/images/ssl.PNG "ssl")

 Certifikati se nalaze na sljedećoj lokaciji: 

 ```
    Key is saved at:         /etc/letsencrypt/live/amila-huzbasic.awsbosnia.com/privkey.pem 
    ACM is saved at: /etc/letsencrypt/live/amila-huzbasic.awsbosnia.com/fullchain.pem
 ```
 Dalji korak je da ažuriramo NGNIX konfiguraciju. 
 Ono šta treba da uradimo jeste da napravimo kopiju originalnog fajla kako bi imali pristup originalnoj konfiguraciji. 

 > cp node-app.conf node-app.conf.bak14052023 

 Nakon što smo napravili kopiju možemo da ažuriramo .conf file. 

  ```
server {
  listen 80;
  server_name <amila-huzbasic.awsbosnia.com>;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl;
  server_name <amila-huzbasic.awsbosnia.com>;

  ssl_ACM /etc/letsencrypt/live/<amila-huzbasic.awsbosnia.com>/fullchain.pem;
  ssl_ACM_key /etc/letsencrypt/live/<amila-huzbasic.awsbosnia.com>/privkey.pem;

  location / {
    proxy_pass http://127.0.0.1:8008;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
} 
```
Potrebno je da restartujemo server. To ćemo uraditi komandom 

> $ sudo systemctl restart nginx 

Na slici ispod možemo vidjeti pokrenutu aplikaciju te certifikat za nju:

![certificate](/week-9/images/certificate-https.PNG "certificate")

4.  Omoguciti ACM SSL certifikata

Certbot posjeduje automatizme koji omogucavaju renewal certifikata uz pomoc cronjob-a. 

O postavljanju automatskog renewala se može pogledati [ovdje](https://eff-certbot.readthedocs.io/en/stable/using.html#setting-up-automated-renewal).

> Komanda koja ce dodati cronjob u /etc/crontab 

> SLEEPTIME=$(awk 'BEGIN{srand(); print int(rand()*(3600+1))}'); echo "0 0,12 * * * root sleep $SLEEPTIME && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null 

Output :

![autorenewal](/week-9/images/autorenewal.PNG "autorenewal")

5. Koristeci openssl komande prikazati koji SSL certitikat koristite i datum njegovog isteka. Probajte korisitit razlicite openssl komande 

Za ovaj dio taska možemo koristiti komandu: 

> openssl s_client -showcerts -connect <ime-prezime>.awsbosnia.com:443 

Output komande prikazan je na slici ispod: 

![ACM](/week-9/images/ACM.PNG "ACM")

6.  Importujte Lets Encrypt SSL certifikat unutar AWS Certified Managera.

Više o importovanju ključeva u AWS Certified Menager se može vidjeti [ovdje](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html).

Poziciramo se unutar foldera 

> $ cd /etc/letsencrypt/live/ime-prezime.awsbosnia.com/  

![kljucevi](/week-9/images/kljucevi.PNG "kljucevi")

Unutar AWS Certificate Menagera (ACM) u dashboardu izaberemo Import Certificate 

> Certificate body - cert.pem

> Certificate private key - privkey.pem

> Certificate chain - fullchain.pem

![ACM](/week-9/images/ACM.PNG "ACM")

![imported-key](/week-9/images/imported-key.PNG "imported-key")

7.  Kreirajte Load Balancer gdje cete na nivou Load Balancera da koristite SSL cert koji ste ranije importovali. (Hint: NGINX config je nophodno auzrirati). Load Balancer u pozadini koristi EC2 instancu ec2-ime-prezime-task-8, nije potrebno kreirati ASG.

Kako se radi o LB, potrebno je da imamo najmanje 2 instance kreiranje koje dodajemo u target group, kako bismo obezbjedili visoku dostupnost aplikacije. 

* Prvi korak je da napravimo jos jednu instancu od AMI Image-a.

* Drugi korak kreiramo sec group za LB; Omogućimo HTTP 80 anywhere i HTTPS 443 anywhere

* Treci korak je da kreiramo target group za LB

8.  Koristeci openssl komande prikazati koji SSL certitikat koristite za vasu domenu i datum njegovog isteka.

Koristimo komandu 
> openssl s_client -showcerts -connect ime-prezime.awsbosnia.com:443 2>/dev/null | openssl x509 -noout -text


![ssl-cert-zadomenu](/week-9/images/ssl-cert-zadomenu.PNG "sslcertzadomenu")

9. Kreirajte novi SSL certifikat unutar AWS Certified Managera, azurirajte ALB da koristi novi SSL cert koji ste kreirali.

### Komanda za izlistavanje svih domena i poddomena u json formatu

* aws route53 list-resource-record-sets -
komanda za izlistavanje svih rekorda asociranih sa hosted zonom
* --hosted-zone-id Z3LHP8UIUC8CDK - specificiramo id hosted zone

* --output json - bit će prikazano u json formatu

* jq -r '.ResourceRecordSets[].Name': Ovo je komandna linija za procesiranje JSON-a zvana jq, koja se koristi za izdvajanje imena zapisa resursa iz JSON izlaza. Opcija -r se koristi za izlazak rezultata u obliku običnog teksta, a filter .ResourceRecordSets[].Name se koristi za izdvajanje vrijednosti polja Name iz svakog objekta u nizu ResourceRecordSets.


>aws route53 list-resource-record-sets --hosted-zone-id Z3LHP8U                                                                         IUC8CDK --output json | jq -r '.ResourceRecordSets[].Name'

* Korak jedan je da unutar ACM-a requestamo certifikat. 
* Korak dva je da unesemo komandu: 
>aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"CREATE","ResourceRecordSet":{"Name":"_de041442b7ff7bf10801006b71f6a92f.amila-huzbasic.awsbosnia.com.","Type":"CNAME","TTL":60,"ResourceRecords":[{"Value":"_ec262f184e2f36cb4eb86176475c0a0f.fcgjwsnkyp.acm-validations.aws."}]}}]}' 

U ovoj naredbi, aws route53 change-resource-record-sets koristimo kako bismo promijenili DNS zapisne setove u Route 53. --hosted-zone-id Z3LHP8UIUC8CDK odnosi se na identifikator zone kojoj želimo promijeniti DNS zapis.

U --change-batch parametru, koristimo JSON format za definiranje promjena koje želimo napraviti. U ovom slučaju, imamo jednu promjenu koju želimo izvršiti. Action je postavljen na "CREATE", što znači da želimo stvoriti novi DNS zapis. ResourceRecordSet sadrži informacije o novom DNS zapisu koji želimo stvoriti. Name određuje ime domena za koji želimo dodati DNS zapis. Type je tip DNS zapisa, u ovom slučaju "CNAME" (Canonical Name). TTL (Time to Live) je vremenski interval u sekundama tijekom kojeg će DNS zapis biti keširan. ResourceRecords sadrži vrijednost CNAME zapisa.

* Korak 3 

U dijelu LB idemo na opciju add listener. Dodajemo port 443 te izaberemo ACM registrovani certifikat. 

* Korak 4 

Ova naredba se koristi za brisanje postojećeg DNS zapisa tipa "A" (adresa) s odgovarajućim imenom i vrijednošću iz Zone u Route 53 usluzi. U ovom slučaju, DNS zapis s imenom "amila-huzbasic.awsbosnia.com." i IP adresom "3.65.35.162" će biti izbrisan.

> aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"amila-huzbasic.awsbosnia.com.","Type":"A","TTL":60,"ResourceRecords":[{"Value":"3.65.35.162"}]}}]}'

Nakon toga kreiramo novi dns zapis.

 Ova naredba se koristi za stvaranje novog CNAME DNS zapisa s odgovarajućim imenom i vrijednosti u Route 53 usluzi. U ovom slučaju, stvaramo CNAME DNS zapis s imenom "amila-huzbasic.awsbosnia.com." koji će se mapirati na "alb-asg-task8-1-856414661.eu-central-1.elb.amazonaws.com".

>aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"CREATE","ResourceRecordSet":{"Name":"amila-huzbasic.awsbosnia.com.","Type":"CNAME","TTL":60,"ResourceRecords":[{"Value":"alb-asg-task8-1-856414661.eu-central-1.elb.amazonaws.com"}]}}]}'

* Korak 5

Ažuriramo node-app.conf file na sljedeći način 

```server {
  listen 80;
  server_name aleksandra-ljuboje.awsbosnia.com;
  location / {
    proxy_pass http://127.0.0.1:8008;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
} 
```

* Korak 6

Restartovati nginx 

>systemctl restart nginx

10.  Koristeci openssl komande prikazati koji SSL 
certitikat koristite za vasu domenu i datum njegovog isteka.a
Ova naredba koristi OpenSSL alat za dohvaćanje i prikazivanje informacija o certifikatu HTTPS veze.

>echo | openssl s_client -showcerts -servername amila-huzbasic.awsbosnia.com -connect >.awsbosnia.com:443 2>/dev/null | openssl x509 -inform pem -noout -text

![cli-certificate](/week-9/images/cli-certificate.PNG "cli-certificate")

![amazon-https](/week-9/images/amazon-https.jpg "amazon https")

11.  Kada zavrsite sa taskom kreirajte AMI image pod nazivom ami-ec2-ime-prezime-task-8 i terminirajte resurse koje ste koristili za izradu taska. 

![terminated](/week-9/images/terminated.PNG "terminated")
