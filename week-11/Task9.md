# Task Description

### Date: 15.05.2023

U tasku 9 je zadatak napraviti .html file koji ce prikazivati Vaše ime i prezime, kratki Vaš opis, te DevOps image koji koristimo od početka programa. HTML file uredite kako god želite (text, colors, fonts, etc.), nije bitno, ali da je preglednost u najmanju ruku okey.

Potrebno je kreirati S3 bucket u formatu: ime-prezime-devops-mentorship-program-week-11, te omogućiti static website:

* Dodati .html i error.html file,
* Podesiti bucket na public access, te dodati bucket policy koji će omogućiti samo minimalne access permissions nad bucketom.

Drugi dio zadatka jeste objaviti tu statičku web stranicu kroz CloudFront distribuciju.

Prilikom kreiranja CloudFront distribucije potrebno je samo sljedeće opcije modifikovati:

* Origin domain,
* Name,
* Viewer protocol policy (Redirect HTTP to HTTPS),
* Custom SSL certificate,
* SSL certifikat koristite od AWS Certificate Manager-a.
 
Sve ostale opcije po default ostaviti. Kada se završi kreacija distribucije napraviti record unutar Route 53 ( www.ime-prezime.awsbosnia.com) koji će pokazivati na tu distribuciju.

Konfiguraciju Route 53 odraditi na sličan način kroz CLI kao u prethodnom zadatku (TASK-8) gdje će Vaš record pokazivati na distrubuciju.

Ovo su primjeri navedenih endpointa koje je potrebno dostaviti:

* S3 website endpoint primjer: S3 static web
* Primjer distribution endpointa: d1ax7sali2r51c.cloudfront.net
* Primjer R53 recorda: www.boris-bradic.awsbosnia.com

Da biste kompletirali ovaj task potrebno je dostaviti sljedeće kroz PR:

U komentar postaviti:

* S3 website endpoint - non-encrypted
* vaš-distribution.net endpoint - encrypted
* R53 record - encrypted

Bucket može ostati na private, ali je potrebno u tome slučaju prilagoditi bucket policy.

Kreirati novi direktorijum za navedeni task i u njega smjestiti:

S3 bucket policy screenshot ( voditi računa da su minimalne permisije postavljene),
.html i error.html file, DevOps image,
Screenshot - S3 website endpointa,
Screenshot - distribution endpointa,
Screenshot - R53 recorda koji je uspješno izvršio load distribucije,
Screenshot konfigurisanoga R53 recorda prema distribuciji.
Koristite AWS dokumentaciju ukoliko naiđete na bilo kakve probleme konfigurisanja CloudFronta sa R53.

1. U tasku 9 je zadatak napraviti .html file koji ce prikazivati Vaše ime i prezime, kratki Vaš opis, te DevOps image koji koristimo od početka programa. HTML file uredite kako god želite (text, colors, fonts, etc.), nije bitno, ali da je preglednost u najmanju ruku okey.

Naoravljeni .html fajlovi se nalaze unutar direktorija "html"

* [index.html](./html/index.html) 
* [error.html](./html/error.html)

2. Potrebno je kreirati S3 bucket u formatu: ime-prezime-devops-mentorship-program-week-11, te omogućiti static website:

* Dodati .html i error.html file,


![uploaded files](/week-11/images/uploaded_files.PNG "uploaded files")

* Podesiti bucket na public access, te dodati bucket policy koji će omogućiti samo minimalne access permissions nad bucketom.

Više o bucket policy može se vijdeti [ovdje]("https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html") i [ovdje]("https://docs.informatica.com/integration-cloud/cloud-data-integration-connectors/h2l/1199-configuring-iam-authentication-for-amazon-s3-and-amazon-s3-/configuring-iam-authentication-for-amazon-s3-and-amazon-s3-v2-co/create-a-minimal-amazon-s3-bucket-policy.html#:~:text=The%20minimal%20Amazon%20S3%20bucket,policy%20through%20the%20AWS%20console.")


![s3_bucket_policy](/week-11/images/s3_bucket_policy.PNG "s3_bucket_policy")

Na slici ispod mozemo vijdeti .html file na s3 endpointu
![s3_endpoint](/week-11/images/s3_endpoint.PNG "s3_endpoint")

3. Drugi dio zadatka jeste objaviti tu statičku web stranicu kroz CloudFront distribuciju.

Prilikom kreiranja CloudFront distribucije potrebno je samo sljedeće opcije modifikovati:

* Origin domain,
* Name,
* Viewer protocol policy (Redirect HTTP to HTTPS),
* Custom SSL certificate,
* SSL certifikat koristite od AWS Certificate Manager-a.

* Prvi korak 

Napraviti certifikat u servisu ACM te odraditi njegovu aktivaciju. 
Za domain tame biramo: 
> www.amila-huzbasic.awsbosnia.com

Kada se kreira certifikat potrebno je uraditi njegovu aktivaciju. Prva stvar koju ćemo uraditi jeste komanda
> aws configure

Nakon toga 
>aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"CREATE","ResourceRecordSet":{"Name":"_0e07e0ca783e9c76d499f8b562fcba50.www.amila-huzbasic.awsbosnia.com.","Typ
e":"CNAME","TTL":60,"ResourceRecords":[{"Value":"_61559b82083e9271186cbb29784f14b3.fcgjwsnkyp.acm-validations.aws."}]}}]}'

Ukoliko je sve prošlo bez problema certifikat će nakon nekoliko minuta biti issued. 

![issued_certificate](/week-11/images/issued_certificate.PNG "issued_certificate")

* Drugi korak

Kreiramo Cloud Front distribuciju

> Create Cloud Front distribution

> Origin domain - izaberemo s3 bucket

>Origin path preskocimo

>Name ostavimo defaultno

>Origin access - public

>Default root object - index.html

>Viewer protocol policy- (Redirect HTTP to HTTPS)

>Settings - Custom SSL certificate - izaberemo kreirani Amazon certifikat

![cloud_front_distribution](/week-11/images/cloud_front_distribution.PNG "cloud_front_distribution")

![distribution_endpoint](/week-11/images/distribution_endpoint.PNG "distribution_endpoint")

Route 53 configuration:

>aws route53 change-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK --change-batch '{"Changes":[{"Action":"UPSERT","ResourceRecordSet":{"Name":"www.amila-huzbasic.awsbosnia.com.","Type":"CNAME","TTL":60,"ResourceRecords":[{"Value":"dslplsrsp5m63.cloudfront.net"}]}}]}'

To check DNS propagation:

>`aws route53 list-resource-record-sets --hosted-zone-id Z3LHP8UIUC8CDK | jq '.ResourceRecordSets[] | select(.Name == "www.amila-huzbasic.awsbosnia.com.") | {Name, Value}'`

![r53_record](/week-11/images/r53_record.PNG "r53_record")


# 
* S3 website endpoint - http://amila-huzbasic-devops-mentorship-program-week-11.s3-website.eu-central-1.amazonaws.com

* vaš-distribution.net endpoint - https://dslplsrsp5m63.cloudfront.net/
* R53 record - www.amila-huzbasic.awsbosnia.com
