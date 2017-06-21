---

copyright:
  years: 2016, 2017
lastupdated: 2017-04-26

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Comparaison de FaaS (Function as a Service)
{: #openwhisk_faas_compared}

L'architecture sans serveur n'est pas la panacée à tous les problèmes de traitement, mais en résout quelques-uns. Sous [plusieurs scénarios d'utilisation](./openwhisk_use_cases.html), une architecture sans serveur peut être un choix idoine. Nous allons comparer ici les architectures suivantes :

1. **FaaS (Function as a Service)** - gérée par OpenWhisk. Actuellement, IBM est le seul à proposer une architecture gérée [OpenWhisk on Bluemix](https://console.ng.bluemix.net/openwhisk).

2. **IaaS (Infrastructure as a Service)** avec OpenWhisk RYO (Roll Your Own). L'utilisateur final peut télécharger OpenWhisk depuis le site Apache Incubation Project et l'installer et l'exécuter sur [Bluemix IaaS](https://console.ng.bluemix.net/catalog/?category=devices), ou un autre [cloud IaaS](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29).

3. **PaaS (Platform as a Service)** - environnement d'exécution géré. L'environnement d'exécution [Liberty for Java](https://console.ng.bluemix.net/catalog/starters/liberty-for-java) géré par l'implémentation IBM Bluemix CloudFoundry en est un bon exemple.

4. **CaaS (Container as a Service)** - environnement de conteneur géré. La solution IBM [Containers on Bluemix](https://console.ng.bluemix.net/catalog/?category=containerImages) en est un bon exemple.

5. **IaaS (Infrastructure as a Service)** avec environnement d'exécution Java EE. La solution IBM [WebSphere Application Server VM on Bluemix](https://console.ng.bluemix.net/catalog/services/websphere-application-server) en est un bon exemple.

Ci-après figure un récapitulatif des avantages et des inconvénients de chaque choix d'architecture depuis la **perspective d'un utilisateur final** qui développe et exploite des applications sur ces différents environnements d'exécution :


| Sujet | (1) OpenWhisk FaaS | (2) OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS+Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unité d'application	|	Fonction unique (généralement un petit bloc de code dans JavaScript, Swift ou un conteneur Docker) - peut peser moins d'1 Ko, mais aussi être plus volumineux. Ne dépasse généralement pas quelques Ko.	|	Identique à la colonne (1)	|	Selon l'environnement d'exécution, il peut s'agir d'un fichier EAR ou WAR ou d'un autre bundle d'application spécifique au langage, généralement assez volumineux - Ko ou même Mo avec de nombreux services dans un bundle, mais peut aussi être aussi petit qu'un service unique	|	Le conteneur Docker est l'unité de déploiement.	|	Machine virtuelle avec serveur d'application, fichier EAR ou WAR et autres dépendances - taille exprimée généralement en Go.	|
|	Empreinte sur les ressources	|	L'utilisateur final ne paye pas et ne soucie pas de l'utilisation de la mémoire, de l'UC ou d'autres ressources. Bien que l'action ait une certaine empreinte, l'utilisateur n'a pas à s'en soucier	|	Elevée. L'utilisateur final doit d'abord rendre disponible l'environnement IaaS et seulement alors installer et configurer OpenWhisk par-dessus.	|	Faible. L'utilisateur final paye pour l'utilisation de la mémoire et de l'UC pour les applications en exécution, mais n'est pas facturé pour celles qui ne sont pas en exécution	|	Faible à moyenne	|	Elevé. L'utilisateur est facturé pour le stockage sur disque, la mémoire, les UC, et éventuellement pour d'autres composants quand l'application est en exécution. Lorsqu'elle est arrêtée, il n'encourt que les coûts du stockage.	|
|	Installation et configuration	|	Aucune	|	Difficile - toute à la charge de l'utilisateur final	|	Aucune	|	Modérée - matériel, mise en réseau, système d'exploitation, outils de gestion de conteneurs fournis par le fournisseur CaaS, images, connectivité et instances par l'utilisateur final.	|	Difficile - matériel, mise en réseau, système d'exploitation, installation initiale du logiciel Java EE fourni par le vendeur, configuration additionnelle, mise en cluster, mise à l'échelle par l'utilisateur final	|
|	Délai jusqu'à la mise à disposition	|	Millisecondes	|	Voir les colonnes (4) et (5)	|	Minutes	|	Minutes	|	Heures	|
|	Administration continue	|	Aucune	|	Difficile	|	Aucune	|	Modérée	|	Difficile	|
|	Mise à l'échelle élastique	|	Chaque action est instantanément et systématiquement mise à l'échelle en fonction de la charge. Des machines virtuelles ou d'autres ressources n'ont pas besoin d'être rendues disponibles d'avance	|	Non fournie - L'utilisateur final doit fournir la capacité de traitement sur IaaS et gérer la mise à l'échelle des machines virtuelles. Une fois les machines virtuelles mises à l'échelle, OpenWhisk met automatiquement à l'échelle l'action, mais les ressources doivent être rendues disponibles d'avance.	|	Automatique, mais lente. Au début d'une période de pointe, l'utilisateur peut avoir à patienter plusieurs minutes jusqu'à l'achèvement de l'action de mise à l'échelle. La mise à l'échelle automatique requiert un réglage consciencieux.	|	Automatique, mais lente. Au début d'une période de pointe, l'utilisateur peut avoir à patienter plusieurs minutes jusqu'à l'achèvement de l'action de mise à l'échelle. La mise à l'échelle automatique requiert un réglage consciencieux.	|	Non fournie	|
|	Planification de la capacité	|	Non nécessaire. FaaS alloue automatiquement la capacité requise	|	Allocation d'avance d'une capacité suffisante ou utilisation d'un script à cet effet.	|	Un certain degré de planification de capacité est requis, mais un certain niveau d'augmentation automatique de la capacité est fourni.	|	Un certain degré de planification de capacité est requis, mais un certain niveau d'augmentation automatique de la capacité est fourni.	|	Nécessité d'allouer une capacité statique suffisante pour gérer les charges de travail en période de pointe	|
|	Connexions persistantes et état	|	Très limitée - Ne peut pas maintenir une connexion persistante, sauf en cas de mise en cache du conteneur. Généralement, l'état doit être mémorisé dans une ressource externe.	|	Identique à la colonne (1)	|	Pris en charge - Peut maintenir ouvert un socket ou une connexion pendant longtemps, peut conserver en mémoire l'état entre les appels.	|	Pris en charge - Peut maintenir ouvert un socket ou une connexion pendant longtemps, peut conserver en mémoire l'état entre les appels.	|	Pris en charge - Peut maintenir ouvert un socket ou une connexion pendant longtemps, peut conserver en mémoire l'état entre les appels.	|
|	Maintenance	|	Non requise - La pile complète est gérée par IBM	|	Significative - Selon l'environnement cible, l'utilisateur doit rendre disponible le matériel, l'opération en réseau, le système d'exploitation, la base de données, installer et assurer la maintenance d'OpenWhisk, etc.	|	Non requise - La pile complète est gérée par le vendeur	|	Significative - L'utilisateur doit créer des images personnalisées et en assurer la maintenance, déployer et gérer des conteneurs, des connexions entre les conteneurs, etc.	|	Significative - L'utilisateur doit allouer des machines virtuelles, gérer et mettre à l'échelle individuellement les serveurs Java EE.	|
|	Haute disponibilité (HA) et reprise après incident (DR)	|	Inhérent / aucun coût supplémentaire.	|	RYO (Roll your own) 	|	Disponible pour un coût supplémentaire	|	Les conteneurs en état d'échec peuvent être redémarrés automatiquement.	|	Disponibles pour un coût supplémentaire, semi-automatique. Basculement automatique possible des machines virtuelles.	|
|	Sécurité	|	Fournie par le vendeur	|	RYO (Roll your own)	|	Combinaison de RYO et de sécurité fournie par le vendeur	|	Combinaison de RYO et de sécurité fournie par le vendeur	|	RYO (Roll your own)	|
|	Vélocité du développement	|	Maximale	|	Maximale	|	Maximale	|	Moyenne	|	Faible	|
|	Utilisation des ressources (ressources inactives mais néanmoins facturées)	|	Les ressources ne sont jamais inactives vu qu'elles ne sont appelées que suite à une demande. En l'absence de charge de travail, aucune allocation de ressources n'intervient et aucun coût n'est encouru.	|	Vu que cette option utilise IaaS ou CaaS - des considérations similaires s'appliquent comme dans les colonnes (4) et (5)	|	Certaines ressources peuvent être inactives. La mise à l'échelle automatique (augmentation ou diminution) aide à éliminer les ressources inactives, mais un certain nombre d'instances en exécution doit toujours être présent et il est probable qu'elles seront utilisées à moins de 50 % de leur capacité. Aucun coût n'est encouru pour les instances arrêtées.	|	Similaire à la colonne (3)	|	Certaines ressources peuvent être inactives. La mise à l'échelle automatique n'est pas prise en charge. Un certain nombre d'instances en exécution doit toujours être présent et il est probable qu'elles seront utilisées à moins de 50 % de leur capacité. Les instances arrêtées peuvent induire un coût de stockage.	|
|	Maturité	|	Maturité rapide	|	Maturité rapide	|	Maturité rapide	|	Maturité modérée	|	Très mature	|
|	Limites de ressources	|	[Certaines limites s'appliquent](./openwhisk_reference.html#openwhisk_syslimits)	|	Dépend des ressources allouées.	|	Non	|	Non	|	Non	|
|	Temps d'attente pour les services rarement utilisés	|	Les demandes inhabituelles peuvent encourir un temps de réponse de plusieurs secondes, mais de quelques microsecondes seulement par la suite.	|	Varie	|	Très long	|	Très long	|	Très long - en supposant que le système dispose de ressources suffisantes.	|
|	Type d'application Sweet spot	|	Traitement d'événements, IoT, back end mobile, microservices. Ne convient absolument pas aux applications monolithiques. Voir [Scénarios d'utilisation](./openwhisk_use_cases.html)	|	Identique à la colonne (1), mais lorsque l'utilisateur veut opérer sur un cloud non IBM ou sur site.	|	Applications Web avec charge de travail 24 heures sur 24, 7 jours sur 7, services avec état ayant besoin de garder ouverte la connexion sur de longues périodes. Peut être utilisé pour exécuter des microservices ou des applications monolithiques.	|	Convient particulièrement bien aux applications de microservices.	|	Applications d'entreprise traditionnelles migrées depuis le site vers le cloud. Convient mieux aux application monolithiques.	|
|	Facturation de la granularité et facturation	|	[Par blocs de 100 millisecondes](https://console.ng.bluemix.net/openwhisk/learn/pricing)	|	Dépend de l'implémentation - Si elle utilise IaaS ou CaaS, des considérations similaires s'appliquent - Voir les colonnes (4) et (5)	|	Généralement facturée par heure (rarement par minute) pour le bundle de ressources (UC + mémoire + certain espace disque)	|	Similaire à la colonne (3)	|	Similaire à la colonne (3)	|
|	Coût total de possession (TCO)	|	Pour ses applications sweet spot, ce coût sera probablement substantiellement moindre que les alternatives. Comme les ressources sont mises à l'échelle automatiquement, un problème d'allocation excédentaire ne survient jamais.	|	Pour les déploiements en cloud, probablement plus onéreux que OpenWhisk FaaS, mais pour un déploiement sur site, peut être moins coûteux que les architectures traditionnnelles.	|	Relativement faible - L'utilisateur n'a pas besoin d'allouer ou de gérer des ressources et peut se concentrer sur son application, mais un certain degré d'allocation excédentaire intervient comparé à l'architecture sans serveur.	|	Modéré - L'utilisateur doit allouer et gérer des conteneurs et l'application, mais un certain degré d'allocation excédentaire intervient comparé à l'architecture sans serveur et à PaaS	|	Relativement élevé, mais du fait que la migration d'applications existantes vers le modèle de cloud natif peut avoir un coût prohibitif, cette solution peut constituer un choix viable et économique pour ces applications.	|