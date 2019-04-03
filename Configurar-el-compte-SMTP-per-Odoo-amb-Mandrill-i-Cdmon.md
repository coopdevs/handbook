L'objectiu d'aquest tutorial és de configurar tot el necessari perquè una aplicació pugui enviar correus des d'un subdomini contractat a cdmon. Al final hem fet servir el correu amb domini en comptes de subdomini, així que pot ser que falti algun pas dins de cdmon. Ara bé, aquesta documentació funciona per a configurar-lo per a nous dominis.

## Com configurar el correu

1. Inicia sessió a https://mandrillapp.com/ a través de mailchimp
2. Settings https://mandrillapp.com/settings
3. Create API Key
    * nom del host o descripció unívoca
    * si és per una sola VM, la IP de la VM (per seguretat)
4. Fer servir les credencials a l'aplicació
5. Testejar i corregir

![smtp-mandrill](https://trello-attachments.s3.amazonaws.com/5ba263b6542ddf55e313f2b3/5c910535f97b3b6e51dd6408/c15d6deea41147099c0cafad60adbdaa/imatge.png)

## En el cas d'Odoo (docs oficials)

1. Inicia sessió amb un compte amb permisos d'admin
2. Ves a Settings / General Settings i habilita External Email Servers
3. Ves a Outgoing Mail Servers i crea una nova entrada
4. Omple els camps segons la captura de pantalla. La contrasenya és l'API Key que hem creat a Mandrillapp
5. Pica Test connection

![smtp-odoo](https://trello-attachments.s3.amazonaws.com/5c910535f97b3b6e51dd6408/946x653/77ae655bd4083e862c1a6a28635e99fd/imatge.png)

## Verificar el domini per a Mandrill
[font](https://mandrill.zendesk.com/hc/en-us/articles/205582247-About-Domain-Verification))

1. A Mandrill, _[Sending Domains](https://mandrillapp.com/settings/sending-domains)_ comprova que  hi ha tres "tics" que falten
2. Clica a _View DKIM Settings_ i copia la línia que et dóna.
3. Ves a cdmon, clica a l'etiqueta _DNS_ del domini
4. Crea un registre, tipus TXT, per al (sub)domini que estiguis configurant, i la línia que t'ha donat Mandrill
5. Torna a Mandrill i clica _Test DNS Settings_. Hauria de verificar el DKIM i queixar-se per SPF.
6. Repeteix el procediment del DKIM amb les dades de SPF. També és un registre TXT i pel mateix subdomini, només canvia el contingut, la línia.

![dkim-mandrill](https://trello-attachments.s3.amazonaws.com/5c910535f97b3b6e51dd6408/940x246/c00f086111d5a659dbc4205d74a0f853/cargar_el_27_3_2019_a_las_9_05_54.png)
![cdmon-inicio](https://trello-attachments.s3.amazonaws.com/5ba263b6542ddf55e313f2b3/5c910535f97b3b6e51dd6408/0ad4acf293ce7bbb89148f4754f30bd3/imatge.png)
![dkim-cdmon](https://trello-attachments.s3.amazonaws.com/5ba263b6542ddf55e313f2b3/5c910535f97b3b6e51dd6408/cb42589ba4d2cf79f7633bca7ab95b88/imatge.png)
![spf-cdmon](https://trello-attachments.s3.amazonaws.com/5c910535f97b3b6e51dd6408/940x233/76e0e98d13899fe3bf2aa973039560c4/imatge.png)
![spf-cdmon](https://trello-attachments.s3.amazonaws.com/5ba263b6542ddf55e313f2b3/5c910535f97b3b6e51dd6408/3cada7e7dc7bcbe5545e464551328910/imatge.png)

## Registre de mail pel subdomini

7. Verifica el correu. A cdmon, [crea un registre MX](https://ticket.cdmon.com/es/support/solutions/articles/7000006119-c%c3%b3mo-configurar-el-registro-de-correo-o-registro-mx) per al subdomini desitjat. El contingut ha de ser el mateix domini que el del domini principal.
8. Crea el compte de correu amb subdomini dins de cdmon (?¿)