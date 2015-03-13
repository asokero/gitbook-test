## Primuksen ja Wilman autentikointi Puavoon

Primus ja Wilma saadaan käyttämään käyttäjien autentikointiin Puavon LDAP-kantaa luomalla primusta varten tunnus Puavon ulkoisiin palveluihin ja konfiguroimalla Primus-palvelin käyttämään ulkoista LDAP-kantaa.

Autentikoinnin toiminnan testaamiseen on saatavilla Starsoftilta ohjelma LdapTest.exe, josta tarvitaan vähintään versio 1.5.

```
Server	extldap1.opinsys.fi
Port	389
CACert	opinsyscert.pem (Sertifikaatin sisältö täältä)
DN	uid=wilma,ou=System Accounts,dc=edu,dc=kunta,dc=fi
Passwd	salasana
LdapV3	x
TLS	x
Base DN	ou=People,dc=edu,dc=kunta,dc=fi
Search	(uid=wilma.testi)
Scope	2
```

Primuksen asetukset tehdään Primus-palvelimelle tiedostoon prserver.ini, johon lisätään seuraavat tiedot. Puavon LDAP-kannassa opettajien ja oppilaiden käyttäjätunnukset ovat samassa haarassa. opinsyscert.pem -tiedosto luodaan sivun https://opinsys.zendesk.com/entries/20160652-web-palveluiden-ldap-autentikointi-puavoon mukaan ja CertCAFile -asetus laitetaan osoittamaan luotuun tiedostoon. Configuraation Host1 ja Host2 määreet vaikuttavat wilman autentikointiin. Host1 on opiskelijoiden haara ja Host2 opettajien.

```
[Ldap]
Library=synapse
Host=extldap1.opinsys.fi
Port=389
MainDn=uid=primus,ou=System Accounts,dc=edu,dc=kunta,dc=fi
MainPasswd=salasana
Search=(uid=$USERNAME)
SearchBase=ou=People,dc=edu,dc=kunta,dc=fi
SearchScope=2
Tls=1
CertCAFile=opinsyscert.pem

Host1=extldap1.opinsys.fi
Port1=389
MainDn1=uid=primus,ou=System Accounts,dc=edu,dc=kunta,dc=fi
MainPasswd1=salasana
Search1=(uid=$USERNAME)
SearchBase1=ou=People,dc=edu,dc=kunta,dc=fi
SearchScope1=2
Tls1=1
CertCAFile1=opinsyscert.pem

Host2=extldap1.opinsys.fi
Port2=389
MainDn2=uid=primus,ou=System Accounts,dc=edu,dc=kunta,dc=fi
MainPasswd2=salasana
Search2=(uid=$USERNAME)
SearchBase2=ou=People,dc=edu,dc=kunta,dc=fi
SearchScope2=2
Tls2=1
CertCAFile2=opinsyscert.pem
```
Huom! Jos ldaptest.exe ei toimi oikein, vaikka sertifikaattitiedosto on paikallaan, kannattaa varmistaa, että Windows-koneella on asennettuna DLL:t ssleay32.dll ja libeay32.dll. Mikäli ne puuttuvat, helpoiten ne saa käyttöön kopioimalla ne samaan hakemistoon, jossa primuksen .exe -tiedostot ovat.