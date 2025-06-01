La liste suivante met en évidence un sous-ensemble de menaces pertinentes en matière de sécurité dans le cloud, mais elle n’est en aucun cas exhaustive, car il en existe bien d'autres :

* Ransomware (rançongiciel)
* Attaques par force brute
* Attaques de la chaîne d’approvisionnement
* Vulnérabilités de type zero-day
* Vulnérabilités et expositions communes (CVE)
* Erreurs de configuration
* Accès non autorisé
* Attaques par déni de service (DoS)
* Menaces internes
* Vol de données

La question est donc de savoir comment sécuriser vos environnements Azure contre les incidents de sécurité courants dans le cloud et les menaces pesant sur vos ressources cloud.

Pour progresser dans cette démarche, vous pouvez utiliser un ensemble de services de sécurité natifs disponibles dans Azure, généralement désignés sous le nom de **sécurité Azure**.

Ce livre vous enseigne les services de sécurité Azure suivants :

* **Microsoft Entra ID**
* **Azure DDoS Protection Standard**
* **Azure Firewall Standard**
* **Azure Firewall Premium**
* **Azure Web Application Firewall**
* **Microsoft Sentinel**
* **Microsoft Defender for Cloud**
* **Microsoft Defender for DevOps**
* **Azure Monitor**
* **Azure Key Vault**
* **Azure Policy**


### Modèle de responsabilité partagée

Dans un environnement cloud public comme Azure, certaines responsabilités en matière de sécurité sont prises en charge par le fournisseur de cloud (Microsoft), tandis que d'autres restent à la charge du client. Il est essentiel de bien comprendre cette répartition, car elle garantit une sécurité cohérente et efficace.

**Responsabilité de Microsoft (le fournisseur) :**
Microsoft est responsable de la **sécurité de l'infrastructure cloud**. Cela inclut :

* La **sécurité physique** des centres de données (accès restreint, surveillance, etc.)
* La **gestion des pannes matérielles**
* L’**alimentation de secours**
* La **résilience face à certains sinistres**
* La maintenance de l’**infrastructure réseau, de calcul et de stockage**

En bref, Microsoft sécurise **le cloud lui-même**, afin que vous n’ayez pas à gérer les problèmes liés au matériel ou aux infrastructures physiques.

**Responsabilité du client (vous) :**
En tant qu'utilisateur d'Azure, vous êtes responsable de la **sécurité dans le cloud**. Cela comprend :

* La **gestion et protection de vos données**
* La **configuration sécurisée de vos services** (VMs, bases de données, etc.)
* Le **contrôle des accès utilisateurs**
* L’**application des politiques de sécurité**

En environnement **on-premise**, vous auriez à gérer l’ensemble de ces aspects, mais dans le cloud, une partie est déléguée au fournisseur.

**Cette répartition varie selon le modèle de service utilisé :**

* **SaaS (Software as a Service)** : Le fournisseur gère presque tout. Vous ne gérez que les données et les identités des utilisateurs.
* **PaaS (Platform as a Service)** : Vous gérez les applications que vous déployez et les données. Le fournisseur gère l'infrastructure et la plateforme.
* **IaaS (Infrastructure as a Service)** : Vous gérez tout sauf le matériel sous-jacent (machines virtuelles, OS, stockage, réseau, sécurité logicielle).

---

### Le paysage des menaces

De nouvelles attaques apparaissent toutes les quelques minutes, visant aussi bien les grandes que les petites entreprises. Ces attaques sont menées par des **acteurs malveillants** (ou *threat actors*), qui exploitent les vulnérabilités des systèmes pour atteindre leurs objectifs. Ces individus peuvent venir de l’extérieur (cybercriminels) ou de l’intérieur (employés malveillants).

La plupart du temps, leurs motivations sont financières, politiques ou idéologiques. Ils exploitent des failles de sécurité dans les contrôles en place. Ainsi, comprendre les **vulnérabilités**, les **malwares** et les **profils d’attaquants** permet de mieux cerner ce que l’on appelle le **paysage des menaces** (*threat landscape*).

Une bonne analogie est celle de la salle de sport : comme on apprend en observant les autres, en cybersécurité, observer les comportements des attaquants aide à anticiper leurs actions. Dans ce domaine, on parle souvent de **TTP** : **Tactiques, Techniques et Procédures**.

* **Objectif** : ce que l’attaquant cherche à atteindre
* **Tactique** : la méthode globale choisie pour y parvenir
* **Technique** : la manière précise d’exécuter cette tactique
* **Procédure** : les détails techniques spécifiques à l’attaque

Par exemple :
Un attaquant vise un gain financier (objectif).
Il essaie d’obtenir un accès initial à Azure (tactique).
Il lance une attaque par force brute via RDP (technique).
Il utilise une infrastructure spécifique pour cela, identifie des cibles, etc. (procédure).

Ces attaques réussissent souvent à cause de **mauvaises pratiques de sécurité**, comme l’usage de **mots de passe faibles ou réutilisés**, ou l’absence de **MFA** (authentification multifacteur).

Les attaquants cherchent toujours à **échapper à la détection** tout en atteignant leur but. Ils adaptent donc leurs TTP au fil du temps. C’est pourquoi votre **posture de sécurité** doit évoluer en permanence, comme un entraînement physique.

---

### Défis de la sécurité dans le cloud

Comme dans une salle de sport où de nouvelles machines apparaissent, les environnements cloud introduisent de nouveaux outils de sécurité. Mais il faut **apprendre à les utiliser**, car ils sont différents de ceux des environnements classiques (on-premise).

Avec l’explosion des technologies cloud, **les attaquants aussi se modernisent**. Il faut donc **passer d’une posture réactive à une posture proactive**, pour rester **en avance sur les menaces**.

Dans le cloud, les ressources sont souvent **accessibles à distance**, ce qui ouvre la porte à des compromissions. Une fois dans votre environnement, un attaquant peut :

* Prendre le contrôle d’un compte
* Se déplacer latéralement vers d'autres ressources (VM, bases de données, stockage, objets IoT, etc.)
* Déployer une attaque finale, comme un ransomware

Ce **modèle d’attaque** est souvent appelé **kill chain**, composé de cinq étapes :

1. **Exposition** : la faille est visible
2. **Accès** : l’attaquant entre dans le système
3. **Mouvement latéral** : il explore d’autres ressources
4. **Actions sur l’objectif** : il agit selon son but
5. **Objectif atteint** : données volées, rançon, sabotage, etc.

----
---

### Défense en profondeur

La **défense en profondeur** est une stratégie de sécurité qui vise à **réduire les risques de compromission** en combinant **plusieurs couches de sécurité**.
L’idée est simple : plus il y a d’obstacles à franchir, plus il est difficile pour un attaquant d’atteindre son objectif.

**Exemple** : imaginez un match de football. Si un attaquant (acteur malveillant) veut marquer un but, il doit passer le gardien. S’il y a un autre joueur entre lui et le gardien, cela complique déjà sa tâche. Et plus on ajoute de défenseurs, plus il devient difficile de marquer.
De la même manière, dans Azure, **chaque couche de sécurité ajoutée** complique la tâche des attaquants. Même si une couche échoue, les autres peuvent continuer à protéger les ressources.

La **posture de sécurité** d’une organisation repose sur trois phases :

1. **Sécuriser les charges de travail**
2. **Surveiller et détecter les menaces**
3. **Enquêter et répondre aux incidents**

Pour appliquer efficacement une **défense en profondeur**, il faut **multiplier les contrôles de sécurité** à chaque étape, en combinant **plusieurs services de sécurité Azure**. Ces protections doivent correspondre aux différentes **étapes du modèle de kill chain** (exposition, accès, mouvement latéral, action, objectif) car une attaque ne commence pas toujours par une tentative d’accès – parfois, l’attaquant est déjà dans l’environnement.

Il est donc crucial de **sécuriser l’environnement Azure dans sa globalité**, en prenant en compte cinq grandes catégories de ressources :

* **Identités**
* **Données**
* **Applications**
* **Infrastructure**
* **Réseau**

Pour chaque catégorie, il faut :

* Appliquer plusieurs services de sécurité Azure
* Croiser ces services avec les **étapes du kill chain**
* Les faire correspondre aux **trois phases de sécurité** (sécurisation, détection, réponse)

---
### Sécurisation des identités

La **sécurité des identités** est un point critique dans la protection d’un environnement Azure. En partant du **modèle de kill chain**, il faut anticiper les étapes qu’un acteur malveillant pourrait suivre pour **obtenir un accès non autorisé** via des identifiants utilisateurs compromis.

#### Exemple d'attaque :

Un attaquant peut, par exemple, utiliser une **attaque par force brute** pour pirater un compte utilisateur.
Pour cela, dès la **phase de sécurisation des charges de travail**, vous devez :

* **Détecter les connexions suspectes**
* **Déclencher une authentification multifactorielle (MFA)** si nécessaire

#### Solutions proposées :

* **Microsoft Entra ID** (anciennement Azure AD) permet :

  * De détecter les connexions anormales
  * D’appliquer des politiques de MFA
  * De protéger les identités à privilèges (comme les administrateurs)
  * De forcer des actions (ex. : changement de mot de passe) si une compromission est suspectée

Mais, comme il n'existe **aucune solution miracle**, l’attaque peut aussi venir **d’une identité déjà compromise**, utilisée ensuite pour faire du **mouvement latéral** ou lancer des campagnes de **phishing** ciblées.

#### Complément de protection :

* **Microsoft Sentinel** permet de :

  * **Détecter les comportements suspects**
  * **Réagir automatiquement** (containment, alertes, automatisation de la réponse)

Ainsi, la **sécurisation des identités** ne se limite pas à empêcher l'accès initial. Il faut aussi :

* Anticiper les mouvements post-compromission
* Combiner **plusieurs services de sécurité Azure**
* Couvrir **l’ensemble des étapes du kill chain** (accès initial, mouvement latéral, action sur objectif, etc.)

---

### Protéger votre environnement contre les vulnérabilités

Pour **empêcher les attaquants** d’exploiter des failles dans votre environnement Azure, vous pouvez suivre les **bonnes pratiques de sécurité** proposées par **Microsoft Defender for Cloud**. Ce service vous aide à :

* **Renforcer la sécurité** des ressources Azure (comme les machines virtuelles)
* **Surveiller les activités** suspectes (ex. : attaques RDP par force brute)
* Déclencher des **alertes et réponses automatiques** en cas d’incident

En cas d’alerte, vous pouvez approfondir l’analyse via **Microsoft Sentinel** et automatiser une réponse (par exemple : blocage, suppression d'accès, etc.).

### Prévenir les mouvements latéraux

Une fois dans votre environnement, un attaquant peut tenter de se **déplacer latéralement**. Exemple : dans un déploiement Kubernetes, un service sensible comme **le dashboard Kubeflow** est laissé exposé. Un attaquant peut alors :

* Accéder à Kubeflow
* Déployer une **image malveillante** (ex. : crypto-minage)

**Microsoft Defender for Cloud** permet :

* De **détecter l’exposition** de ces interfaces sensibles
* De **repérer les comportements anormaux** (ex. : minage)
* De **déclencher des réponses automatiques** via des flux de travail
* D’envoyer des alertes à **Microsoft Sentinel**, qui peut ensuite **orchestrer la réponse**

### Sécurisation complète

Puisque les **applications tournent sur les infrastructures**, et que les **données sont stockées** aussi bien dans les applications que dans les ressources elles-mêmes, il est essentiel de :

* Sécuriser **l'infrastructure**
* Sécuriser **les applications**
* Sécuriser **les données**

---


### Sécurisation des applications et des données

Sécuriser les **données** et les **applications** est **aussi crucial** que la protection des autres types de ressources dans un environnement Azure.

#### Outils et services recommandés :

* **Azure Policy** permet :

  * D’imposer des **règles de conformité** (guardrails)
  * De s’assurer que les ressources déployées respectent les **bonnes pratiques de sécurité** et les **règles métier** de l’organisation

* **Microsoft Defender for Cloud** :

  * Détecte les **non-conformités** aux bonnes pratiques
  * Peut **corriger automatiquement certaines erreurs de configuration**
  * Améliore ainsi la **posture de sécurité globale** de l’entreprise

Si un attaquant parvient à effectuer un **mouvement latéral** et à compromettre des ressources critiques (ex. : un compte de stockage contenant des données sensibles), vous pouvez :

* **Détecter l’activité suspecte** avec Defender for Cloud et Microsoft Sentinel
* **Réagir automatiquement** pour contenir l’attaque

### Approche par la défense en profondeur

L'utilisation de **plusieurs services de sécurité Azure** crée **plusieurs couches de protection**, ce qui :

* Réduit considérablement la probabilité de succès d'une attaque
* Protège simultanément :

  * Les **identités**
  * Les **données**
  * Les **applications**
  * L’**infrastructure**
  * Les **ressources réseau**

---


