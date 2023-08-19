# Cryptographie & blockchain
## Cryptographie : fonctions et applications
La CNIL (Commission Nationale de l’Informatique et des Libertés) définit la cryptologie comme étant la science du secret [CNIL, 16]. Elle réunit la cryptographie, qui fait référence à un ensemble de mécanismes pour réaliser des écritures secrètes, et la cryptanalyse qui correspond à l’étude des attaques contre les mécanismes de cryptographie. Dans cet article, nous nous intéressons à la cryptographie qui, aujourd’hui, assure non-seulement la confidentialité des données secrètes mais s’est élargie à prouver mathématiquement d’autres notions telles que l’authenticité d’un message (prouver qui est l’auteur d’un message) ou encore son intégrité (prouver s’il a été modifié ou non). Pour assurer ces usages, la cryptographie regroupe les trois principales fonctions suivantes: 
- le hachage 
- le chiffrement et le déchiffrement
- la signature numérique

Dans cette section nous allons présenter brièvement ces trois fonctions ainsi que deux concepts fondamentaux, à savoir le certificat électronique et la blockchain. Il est nécessaire de connaitre ces notions de base pour la bonne compréhension du reste de ce chapitre.
### Hachage
Le hachage permet de garantir qu’un message est intègre et de détecter s’il a été involontairement modifié. Comme le montre la Figure 1, une "fonction de hachage" permet d’associer à un message M (un simple texte, un fichier texte ou multimédia, un répertoire, etc.), quelle que soit sa taille, une empreinte unique H(M), qui est une suite de caractères hexadécimaux de taille fixe. La taille de l’empreinte dépend de la fonction de hachage utilisée. Connaissant l’algorithme de hachage :
- toute personne peut calculer l’empreinte H(M) d’un message M. Cette valeur ne change pas tant que la fonction de hachage n’a pas changé.
- personne ne peut retrouver le message M à partir de son empreinte H(M) ; il s’agit d’une fonction à sens unique (irréversible).
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/5a5cd215-94a6-42ad-bbe0-cf0ad996c20d" /><br/>
  <i>Figure 1. Mécanisme de hachage</i>
</p>

L’empreinte H(M) doit être une caractéristique du message M et il doit y avoir une très faible probabilité de collision, c’est-à-dire que deux messages différents aient la même empreinte. Différents algorithmes existent pour calculer l’empreinte d’un message :
- **MD5** (Message Digest 5) : inventée par Ronald Rivest [Rivest, 92], cette fonction de hachage produit une empreinte de 128 bits (16 octets ou 32 caractères en notation hexadécimale). Wang et al. ont démontré qu’il était possible d’obtenir la même empreinte (des collisions) sur des fichiers différents [Wang, 04], ce qui fait donc de MD5 un algorithme faillible et obsolète (même s’il est encore un standard très utilisé dans les entreprises pour le stockage des mots de passe).
- **SHA-1** (Secure Hash Algorithm 1) : conçue par la NSA (National Security Agency), cette fonction de hachage produit une empreinte de 160 bits (20 octets ou 40 caractères en notation hexadécimale). En 2005, des cryptanalystes ont découvert des attaques sur SHA-1 déduisant ainsi que l’algorithme pourrait ne plus être suffisamment sûr [Schneier, 05].
- **SHA-2** (Secure Hash Algorithm 2) : est une famille de fonctions de hachage qui ont été conçues par la NSA, sur le modèle de la fonction SHA-1. Cette famille comporte de nombreuses fonctions dont les plus utilisées aujourd’hui sont SHA-256 et SHA-512 qui ont des algorithmes similaires mais opèrent sur des tailles de mot différentes (256 bits pour SHA-256 et 512 bits pour SHA-512). On y trouve également d’autres fonctions comme SHA-384 qui est considérée plus sécurisée que SHA-512 et apporte certains changements à l’initialisation et à la sortie (tronquée à 384 bits). 
- **SHA-3** (Secure Hash Algorithm 3) : en octobre 2012, le NIST (National Institute of Standards and Technology) a annoncé la sélection de l’algorithme Keccak de Bertoni et al. [Bertoni, 08] comme lauréat du concours d’algorithmes de hachage cryptographique SHA-3. Cette fonction n’est pas destinée à remplacer SHA-2 mais à fournir une autre solution à la suite des possibilités d’attaque contre les standards MD5 et SHA-1.

Outre l’intégrité numérique, le hachage a de nombreuses applications telles que la signature numérique, le certificat électronique, le stockage des mots de passe, la gestion des blocs dans une blockchain, etc.
### Chiffrement et Déchiffrement
Le chiffrement et le déchiffrement (ou, cryptage et décryptage) sont un procédé de cryptographie qui permet de rendre la compréhension d’un message impossible à toute personne qui n’a pas la clé de déchiffrement. Le chiffrement consiste, grâce à une clé et un algorithme de chiffrement, à transformer un texte en clair en un texte chiffré, appelé cryptogramme, qui n’est pas compréhensible. Le déchiffrement est l’opération inverse ; grâce à une clé et un algorithme de déchiffrement, il permet de transformer un texte chiffré en texte en clair. Trois types de chiffrement existent : le chiffrement symétrique, le chiffrement asymétrique et le chiffrement hybride.

Le chiffrement symétrique, comme illustré dans la Figure 2, permet de chiffrer et de déchiffrer un message moyennant une clé secrète dite "clé privée". C’est la même clé qui est utilisée pour chiffrer et déchiffrer le message. 
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/17d4f1b7-de2c-456c-b140-9ca112053009" /><br/>
  <i>Figure 2. Mécanisme du chiffrement symétrique</i>
</p>

Le chiffrement symétrique est particulièrement rapide, puisqu’on utilise la même clé, mais nécessite que l’émetteur et le destinataire se mettent d’accord sur une clé secrète commune ou se la transmettent par un autre canal [CNIL, 16]. Celui-ci doit être choisi avec précaution pour éviter que la clé soit récupérée par des pirates.

Le chiffrement asymétrique, quant à lui, est basé sur deux clés : une pour le chiffrement et une autre pour le déchiffrement. Si la clé de chiffrement est publique, celle de déchiffrement doit être secrète et vice versa. Comme le chiffrement symétrique utilise la même clé, il est plus rapide que le chiffrement asymétrique qui nécessite des calculs plus complexes pour chiffrer et déchiffrer des données. Dans le cas général, comme le montre la Figure 3, le chiffrement asymétrique consiste à ce que l’émetteur utilise la clé publique du destinataire pour chiffrer le message et que le destinataire utilise sa clé privée pour le déchiffrer. 
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/f188882f-0b80-4fb7-9937-c0c25213ce3a" /><br/>
  <i>Figure 3. Chiffrement asymétrique (chiffrement par clé publique et déchiffrement par clé privée)</i>
</p>

Dans certaines situations, comme le montre la Figure 4, il est possible que l’émetteur chiffre un message avec sa clé privée et que le destinataire utilise la clé publique de l’émetteur pour déchiffrer le message. Ce processus est utilisé par exemple pour prouver l’authenticité de l’émetteur d’une signature électronique. 
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/c42d82e2-46d0-4741-ac88-18273525bcf2" /><br/>
  <i>Figure 4. Chiffrement asymétrique (chiffrement par clé privée et déchiffrement par clé publique)</i>
</p>

L’avantage du chiffrement asymétrique c’est qu’il n’y a pas de clé secrète à se partager entre l’émetteur et le destinataire : connaissant la clé publique du destinataire, l’émetteur chiffre le message, et ensuite, le destinataire, étant le seul à connaitre sa clé privée, l’utilise pour déchiffrer les messages qu’il reçoit. Le risque dans ce mode de fonctionnement c’est que l’émetteur doit s’assurer que la clé publique est bien celle du destinataire et qu’elle n’a pas été remplacée par celle d’un pirate. Grâce au certificat électronique, présenté un peu plus loin, ce problème d’authenticité de la clé publique est résolu.

Le chiffrement hybride est une technique qui combine le chiffrement symétrique et asymétrique dans le but de bénéficier de leurs avantages. Ce type de chiffrement consiste à se partager la clé secrète chiffrée par un chiffrement asymétrique, et ensuite, une fois connue par l’émetteur et le destinataire, l’utiliser pour chiffrer et déchiffrer les messages confidentiels par un chiffrement symétrique qui présente l’avantage d’être rapide.

Concernant les algorithmes de chiffrement, l’AES (Advanced Encryption Standard) [FIPS PUBS, 01], aussi connu par l’algorithme Rijndael, est aujourd’hui le standard le plus utilisé dans le chiffrement symétrique. L’AES est destiné à remplacer le DES (Data Encryption Standard) [FIPS PUBS, 99] qui est devenu trop faible au regard des attaques d’aujourd’hui. Pour le chiffrement asymétrique, RSA [RSA, 78], nommé par les initiales de ses trois inventeurs Rivest, Shamir et Adleman, est l’algorithme le plus utilisé aujourd’hui pour échanger des données confidentielles sur Internet.
### Signature Numérique
Par analogie avec la signature manuscrite d’un document papier, la signature numérique permet d’authentifier l’auteur d’un message (ou de tout document électronique) et de garantir son intégrité (en s’assurant qu’il n’a pas été modifié). Une signature numérique doit répondre à trois caractéristiques : l’authenticité (elle ne doit pas pouvoir être imitée), la non-répudiation (le signataire ne doit pas pouvoir se rétracter ou prétendre qu’il ne l’a pas signée) et l’intégrité (en comparant la signature et le message, on doit pouvoir être sûr que le message n’a pas été modifié lors de l’envoi). La signature numérique est basée sur le chiffrement asymétrique et les fonctions de hachage selon le protocole illustré dans la Figure 5.
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/eca69781-e4f8-4658-82dd-9c01168ba7bd" /><br/>
  <i>Figure 5. Mécanisme de la signature numérique</i>
</p>

Le protocole de la signature numérique consiste à :
- Pour signer un message, le signataire doit, tout d’abord, générer l’empreinte du message au moyen d’une fonction de hachage et, ensuite, chiffrer cette empreinte avec sa clé privée. La signature ainsi obtenue, est transmise avec le message au destinataire. 
- Pour vérifier la validité du message, le vérificateur doit tout d’abord générer l’empreinte du message en utilisant la même fonction de hachage appliquée par le signataire et, ensuite, déchiffrer la signature en utilisant la clé publique du signataire. Il suffit enfin de comparer les deux empreintes pour juger de la validité de la signature numérique.
### Certificat Electronique
Le chiffrement asymétrique est basé sur le partage d’une clé publique entre différents utilisateurs. Le partage de cette clé publique se fait souvent au travers d’un annuaire ou d’un serveur web. Le problème c’est que dans ce mode de partage, rien ne garantit que la clé publique est bien celle de l’utilisateur qui l’a partagée. Un pirate qui réussit à remplacer une clé publique par la sienne pourra déchiffrer tous les messages ayant été chiffrés par cette nouvelle clé corrompue. Les certificats électroniques permettent de résoudre ce problème. Un certificat électronique permet d’associer une clé publique à une entité (une personne, une organisation, une machine, ...) afin d’en assurer la validité. Ainsi, lorsqu’on récupère une clé publique, grâce au certificat électronique qui lui est associé, on saura si elle est valide ou pas. 

Le certificat électronique peut être vu comme la pièce d’identité de l’entité désirant partager une clé publique. Il est généralement délivré par un organisme appelé "autorité de certification" ou CA (pour Certification Authority). Une autorité de certification est chargée de délivrer les certificats et de leur assigner une date de validité. Elle peut éventuellement révoquer à tout moment un certificat électronique. Selon la norme X509[^1], le certificat électronique doit contenir un ensemble d’informations telles que le nom de l’autorité de certification l’ayant délivré, sa date de validité, la clé publique associée, certaines informations sur l’entité ayant partagé la clé publique (nom, prénom, adresse électronique, etc.), …, et la signature de l’autorité de certification. Cette signature électronique est une empreinte générée à partir des informations contenues dans le certificat (nom, adresse, clé publique, …). La signature est chiffrée par la clé privée de l’autorité de certification ayant délivré le certificat.

Selon le niveau de signature, on distingue deux types de certificats :
- **Les certificats auto-signés** : ce sont des certificats signés par un serveur local et destinés à un usage interne. Ce type de certificats permet de garantir la confidentialité des échanges au sein d’une organisation.
- **Les certificats signés par un organisme de certification** : pour assurer la confidentialité des échanges avec des utilisateurs externes (par exemple les sites de e-commerces, le paiement en ligne, les échanges confidentiels avec des organismes publics ou privés, etc.), il est nécessaire que les certificats soient délivrés par des autorités de certifications.

Les certificats électroniques peuvent être utilisés dans différents contextes ; voici quelques exemples :
- Le certificat installé sur un serveur web permet d’assurer le lien entre un service et son propriétaire. Par exemple, dans le cas d’un site web, le certificat permet de garantir que le nom de domaine dans l’url d’une page web appartient bien au propriétaire du site web. Il permet également de sécuriser les échanges avec les utilisateurs grâce aux protocoles HTTPS (Hypertext Transfer Protocol Secure), SSL (Secure Socket Layer) et TLS (Transport Layer Security).
- Un certificat installé sur le poste de travail d’un utilisateur ou embarqué dans une carte à puce permet d’identifier l’utilisateur et de lui associer ainsi des droits.
- Un certificat installé sur les équipements d’un réseau virtuel privé VPN (Virtual Private Network) permet de chiffrer les flux de communication de bout en bout entre deux points (par exemple, l’accès au réseau interne d’une organisation depuis l’extérieur).
## Blockchain
Avec son émergence dans les crypto-monnaies, la blockchain a été conçue pour servir de base de données distribuée sécurisée pour le stockage des transactions d’actifs numériques[^2]. Il s’agit d’un registre dans lequel les transactions sont enregistrées dans des blocs chainés dans un ordre chronologique d’où sa nomination de "blockchain". Ce registre est public, décentralisé, tolérant aux fautes, immuable et ne dépend d’aucune autorité centrale. La blockchain est désignée comme étant une technologie qui a surmonté avec succès deux problèmes complexes :

- Sans l’intervention d’une partie centralisée, atteindre un consensus dans un groupe de participants anonymes dont certains peuvent se comporter avec une intention malveillante.
- Eviter les attaques de sécurité, comme le problème de la double-dépense[^3], et assurer l’intégrité de toutes les informations sur la blockchain.

La blockchain a eu son progrès dans le Bitcoin [Nakamoto, 08] pour enregistrer de façon sécurisée les transactions financières. Aujourd’hui, elle est utilisée pour stocker tout type d’informations. A titre d’exemples, Ouaddah et al. [Ouaddah, 17] l’utilisent pour stocker des contrats intelligents appliqués pour échanger des exécutions de politiques de contrôle d’accès contre des jetons d’accès ; Pinno et al. [Pinno, 17] l’utilisent pour stocker les règles de contrôle d’accès, les relations, les contextes et les informations de responsabilité ; Azaria et al. [Azaria, 16] l’utilisent pour contrôler les autorisations d’accès aux données des dossiers médicaux des patients à travers des contrats intelligents entre les patients, les prestataires et les tiers pour accorder des autorisations d’accès, etc. Certains modèles de contrôle d’accès, que nous étudions un peu plus loin dans ce chapitre, sont basés sur la blockchain ; c’est pour cette raison que nous dédions cette section à la présentation des notions fondamentales de la blockchain.

La blockchain est une technologie qui permet de stocker de façon sécurisée des données numériques dans une architecture distribuée. Elle est basée principalement sur le hachage. Comme son nom l’indique, une blockchain est une chaine de blocs ordonnés, chacun contient des données numériques en plus du hash du bloc précédent. Le tout premier bloc s’appelle le bloc de genèse (genesis block, en anglais) et sert de base sur laquelle des blocs supplémentaires sont ajoutés pour former la chaîne de blocs. Dans une blockchain, un nouveau bloc ne peut être créé que si le hash du bloc précédent ait été généré (sachant que le hash d’un bloc ne peut être calculé qu’à la fin d’écriture de toutes les données qu’il doit contenir). Comme illustré dans la Figure 6, le hash du bloc 64 n’a été calculé qu’après avoir terminé toutes les écritures de ce bloc et le bloc 65 n’a été généré qu’après avoir calculé le hash du bloc 64. C’est donc la fonction de hachage qui permet d’ordonner les blocs dans une blockchain.
<p align="center">
  <img src="https://github.com/Cloud-Elit/cryptpgraphie_blockchain/assets/142179779/ee0a6a1a-b09f-4b55-9ae5-8ca33e6c5e39" /><br/>
  <i>Figure 6. Principe de la Blockchain</i>
</p>

Un bloc peut contenir toute sorte de données (des transactions dans le cadre de la crypto-monnaie, des données que l’on doit protéger ou archiver, des données que l’on souhaite partager dans un réseau distribué, etc.). L’écriture dans un bloc suit généralement les étapes suivantes :

- Tout d’abord, il doit contenir le hash du bloc précédent.
- Ensuite, on y inscrit les données que l’on souhaite écrire. On peut éventuellement y ajouter les signatures des auteurs.
- Et enfin, on calcule le hash du bloc pour pouvoir créer le bloc suivant.

Pour mieux expliquer la robustesse de la blockchain, prenons l’exemple de la crypto-monnaie Bitcoin [Nakamoto, 08]. Il s’agit d’un réseau créé en 2009 par Satoshi Nakamoto (c’est le pseudonyme adopté par la ou les personnes ayant développé le Bitcoin). Concrètement, cette blockchain regroupe un ensemble de personnes sur un réseau. Chaque personne est représentée par un nœud, possède un portefeuille de bitcoins, peut télécharger une copie à jour de tous les blocs de la blockchain et peut valider les transactions. Toutes les transactions réalisées entre les différents nœuds du réseau sont inscrites dans des blocs. Les blocs sont générés (ou minés) par des "mineurs" selon un consensus bien défini. Ce processus s’appelle le "minage" et consiste à :

1. récupérer toutes les transactions non encore inscrites sur la blockchain : à chaque fois qu’un nœud du réseau effectue une transaction (transfert de sommes vers d’autres nœuds), celle-ci est propagée dans tout le réseau de la blockchain. La transaction n’est effectivement valable qu’une fois validée par un mineur.
1. vérifier les transactions : cela consiste à vérifier la signature électronique associée à chaque transaction et que l’émetteur est bien en possession des fonds qu’il désire transmettre. 
1. inscrire les transactions dans un bloc : la taille d’un bloc ne dépasse pas 1 Mo (généralement, un bloc est généré toutes les 10 minutes en moyenne).
1. calculer "une preuve de travail" et l’ajouter dans le bloc : c’est l’étape la plus complexe dans le processus de minage ; elle consiste à résoudre un problème mathématique très difficile pour trouver une valeur, appelée "nonce" (Number Only Used Once), répondant à certaines exigences.
1. partager le bloc, comprenant la preuve de travail représentée par le nonce, avec les autres mineurs pour validation et inscription sur la blockchain contre une rémunération prédéterminée en Bitcoin.

Comme il y a plusieurs mineurs sur la blockchain, ils se mettent tous en compétition pour calculer la preuve de travail en résolvant des énigmes (puzzles) mathématiques difficiles. Le mineur qui trouve la preuve de travail en premier en informe les autres. Si plusieurs mineurs trouvent la réponse en même temps, ce qui est théoriquement possible, c’est celui qui a le plus de blocs attachés postérieurement à son bloc qui gagne ; c’est le consensus de la blockchain Bitcoin !

Concrètement, le calcul de la preuve de travail consiste à calculer l’empreinte du bloc en utilisant les informations qui y sont inscrites (transactions, signatures, …) et le nonce. Une règle de la blockchain est qu’on ne peut ajouter un bloc à la blockchain que si son empreinte commence par un certain nombre prédéterminé de zéros. Comme il n’est pas évident que le résultat du hachage commence par le nombre de zéros nécessaires et comme on ne peut pas modifier les informations inscrites sur le bloc, les mineurs ajustent le nonce pour trouver une empreinte commençant par le nombre de zéros requis. Cet ajustement consiste à modifier la valeur du nonce, recalculer l’empreinte et répéter ce processus indéfiniment jusqu’à trouver une bonne valeur du nonce générant l’empreinte contenant le nombre minimal de zéros nécessaires à l’inscription du bloc dans la blockchain. 

Le calcul de la preuve de travail nécessite un très grand nombre d’itérations : plus le nombre d’itérations est grand, plus le temps pour trouver la réponse est lent d’où l’intérêt d’avoir des calculateurs très performants et très énergivores pour pouvoir trouver rapidement la réponse. L’idée de la "preuve de travail" a été développée pour la première fois en 1993 par Cynthia Dwork et Moni Naor [Dwork, 93] et a été formalisée en 1999 par Markus Jakobsson et Ari Juels [Jakobsson, 99]. Le but derrière une preuve de travail est de démontrer que beaucoup de temps et beaucoup d’énergie ont été dépensés dissuadant ainsi les personnes malveillantes de tenter des attaques de sécurité. Pour calculer la preuve de travail, le Bitcoin adopte l’algorithme Hashcash, conçu par Adam Back en 1997 [Back, 97], [Back, 02]. Cet algorithme, tel qu’il est utilisé dans le Bitcoin, consiste à trouver une collision partielle de la fonction de hachage dSHA-256[^4]. Cela revient à trouver deux messages ayant une empreinte commençant par les mêmes caractères. Comme la fonction de hachage est à sens unique, il faudra tester plusieurs millions (ou milliards) de combinaisons pour trouver les messages. Dans Bitcoin, la preuve de travail consiste à trouver un message dont l’empreinte calculée par la fonction dSHA-256 commence par un certain nombre de zéros prédéterminés ; ce message doit également comporter des indications sur le contexte dans lequel la preuve de travail a été calculée (identifiant du mineur, timestamp de calcul, etc.) pour démontrer qu’elle n’a pas été utilisée autre part.

Pour la gestion des transactions, les blockchains peuvent être globalement classées en deux catégories :

- **les blockchains basées sur la sortie de transaction non dépensée** - **UTXO** (Unspent Transaction Output) : dans ce type de blockchains, chaque transaction correspond à un transfert de pièces entre plusieurs nœuds (relations plusieurs-à-plusieurs). Un transfert consomme (i.e. dépense des pièces à partir de) certaines entrées et crée (i.e. dirige les pièces vers) de nouvelles sorties. Il s’agit donc d’un graphe dirigé dans le temps reliant les opérations de transferts entre les différents nœuds représentés par leurs adresses publiques dans le réseau de la blockchain. 
- **les blockchains basées sur le compte** : dans ce type de blockchains, un nœud, représenté par son adresse publique, peut dépenser une fraction de ses pièces et conserver le solde restant. Dans ces blockchains, une transaction a exactement une entrée et une adresse de sortie (relations un-à-un contrairement à UTXO). Bien que la création d’adresse soit gratuite, une seule adresse est généralement utilisée pour recevoir et envoyer des pièces plusieurs fois.
## Bibliographie
- [Azaria, 16] Azaria, Asaph & Ekblaw, Ariel & Vieira, Thiago & Lippman, Andrew. (2016). MedRec: Using Blockchain for Medical Data Access and Permission Management. 25-30. DOI: 10.1109/OBD.2016.11. 
- [Back, 02] Back, Adam. (2002). Hashcash - A Denial of Service Counter-Measure. 
- [Back, 97] Back, Adam. (1997). Hashcash. Available online: http://www.hashcash.org/ (last accessed December 25, 2021)
- [Bertoni, 08] Bertoni, Guido & Daemen, Joan & Peeters, Michaël & Van Assche, Gilles. (2008). Keccak speciecations. Available online: https://keccak.team/index.html (last accessed December 25, 2021)
- [CNIL, 16] Commission Nationale de l'Informatique et des Libertés (CNIL). (2016). Comprendre les grands principes de la cryptologie et du chiffrement. Available online: https://www.cnil.fr/fr/comprendre-les-grands-principes-de-la-cryptologie-et-du-chiffrement (last accessed December 25, 2021)
- [Dwork, 93] Dwork, Cynthia & Naor, Moni. (1993). Pricing via Processing or Combatting Junk Mail. Advances in Cryptology - CRYPT0 '92, LNCS 740, pp. 139-14, 1993. DOI: 10.1007/3-540-48071-4_10. 
- [FIPS PUBS, 01] Federal Information Processing Standards Publications 197. (2001). ADVANCED ENCRYPTION STANDARD (AES). 
- [FIPS PUBS, 99] Federal Information Processing Standards Publications 46-3. (1999). DATA ENCRYPTION STANDARD (DES).
- [Jakobsson, 99] Jakobsson, Markus & Juels, Ari. (1999). Proofs of Work and Bread Pudding Protocols.. Communications and Multimedia Security. 258-272. DOI: 10.1007/978-0-387-35568-9_18. 
- [Nakamoto, 08] Nakamoto, Satoshi. (2008). Bitcoin: A Peer-to-Peer Electronic Cash System. Available online: https://bitcoin.org/bitcoin.pdf (last accessed December 25, 2021)
- [Ouaddah, 17] Ouaddah, Aafaf & Elkalam, Anas & Ouahman, Abdellah. (2017). Towards a Novel Privacy-Preserving Access Control Model Based on Blockchain Technology in IoT. DOI: 10.1007/978-3-319-46568-5_53. 
- [Pinno, 17] Pinno, Otto Julio & Grégio, André & Bona, Luis Carlos. (2017). ControlChain: Blockchain as a Central Enabler for Access Control Authorizations in the IoT. University of Paris 2 Panth´eon-Assas, CRED Paris Center for Law and Economics, Chaire Finance Digitale.. DOI: 10.1109/GLOCOM.2017.8254521. 
- [Rivest, 92] Rivest, R.. (1992). The MD5 Message-digest Algorithm. Network Working Group. MIT Laboratory for Computer Science and RSA Data Security. 
- [RSA, 78] Rivest, Ron & Shamir, Adi & Adleman, Len. (1978). A Method for Obtaining Digital Signatures and PublicKey Cryptosystems. Communications of the ACM. Volume 21. Issue 2. pp 120–126. DOI: 10.1145/359340.359342.
- [Schneier, 05] Schneier. (2005). Schneier on Security. Cryptanalysis of SHA-1. Available online: https://www.schneier.com/blog/archives/2005/02/cryptanalysis_o.html (last accessed December 25, 2021)
- [Wang, 04] Wang, Xiaoyun & Feng, Dengguo & Lai, Xuejia & Yu, Hongbo. (2004). Collisions for Hash Functions MD4, MD5, HAVAL-128 and RIPEMD.. IACR Cryptology ePrint Archive. 2004. 199. 

[^1]: https://www.itu.int/rec/T-REC-X.509/fr
[^2]: Un actif numérique est un bien immatériel, constitué de données numériques, qui possède une valeur financière déterminée par l’offre et la demande, comme tout autre actif financier.
[^3]: https://en.bitcoin.it/wiki/Irreversible\_Transactions
[^4]: dSHA-256, ou double SHA-256, ressemble à la fonction SHA-256 mais appliquée 2 fois de suite.
