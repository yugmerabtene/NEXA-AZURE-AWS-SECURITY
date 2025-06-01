# LAB-01 – Création et gestion des identités dans Azure Active Directory (Azure AD)

---

## Objectifs du Lab

* Comprendre les différents types d’identités dans Azure AD : utilisateur, service principal, identité managée
* Créer ces identités avec Azure CLI via Azure Cloud Shell
* Identifier l’utilisation de chaque type d’identité pour les accès sécurisés
* Se préparer à l’étape suivante : authentification et autorisation

---

## 1. Introduction – Qu’est-ce qu’une identité dans Azure AD ?

Dans Azure Active Directory, une **identité** est un ensemble d’attributs permettant à un utilisateur ou une application de s’authentifier et d’accéder à des ressources. Chaque identité possède des éléments comme :

* Un nom d’affichage
* Un nom d’utilisateur unique (User Principal Name)
* Un mot de passe (ou un certificat dans certains cas)

---

## 2. Moyens de créer des identités

Azure offre plusieurs outils pour créer des identités :

* Le **Portail Azure** (interface graphique conviviale)
* **Azure PowerShell**
* **Azure CLI** (utilisé ici)

Azure CLI peut être utilisé localement ou depuis le service **Azure Cloud Shell**, accessible via [https://shell.azure.com](https://shell.azure.com) ou via l’icône en haut à droite du portail Azure.

---

## 3. Création d’un utilisateur Azure AD

### Commande à exécuter

```bash
az ad user create \
  --display-name "Bob Miller" \
  --password dQ76bmz7rMVs9gSP \
  --user-principal-name bob@contoso.com \
  --force-change-password-next-sign-in true
```

### Explication des options

* `--display-name` : Nom visible de l’utilisateur
* `--password` : Mot de passe initial, complexe
* `--user-principal-name` : Identifiant de connexion unique (ex. [bob@contoso.com](mailto:bob@contoso.com))
* `--force-change-password-next-sign-in` : Oblige à changer le mot de passe à la première connexion

### Résultat attendu

```json
{
  "displayName": "Bob Miller",
  "id": "6904c116-75bc-4e42-ba8b-5a0d1ac4bec4",
  "userPrincipalName": "bob@contoso.com"
}
```

---

### Exercice 1 – Créer l’utilisateur fictif Charlie

Créez un nouvel utilisateur nommé Charlie :

```bash
az ad user create \
  --display-name "Charlie Stone" \
  --password gT92bnz8rNXv3yLP \
  --user-principal-name charlie@contoso.com \
  --force-change-password-next-sign-in true
```

---

## 4. Création d’un Service Principal (application)

Un **service principal** est une identité pour une application qui doit interagir avec Azure.

### Commande

```bash
az ad sp create-for-rbac -n "MyAmazingApp"
```

### Résultat attendu

```json
{
  "appId": "728cd0c5-8ada-459f-8476-144a003d4857",
  "displayName": "MyAmazingApp",
  "password": "2568Q~R0ltg5Q2_R_G-vs5gN6X2pdta",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

Important : ne jamais exposer ces identifiants (mot de passe ou App ID).

---

### Exercice 2 – Créer un autre Service Principal

```bash
az ad sp create-for-rbac -n "MySecondApp"
```

---

## 5. Création d’une identité managée (Managed Identity)

Une **identité managée** permet à une ressource Azure (VM, App Service…) d’accéder à d’autres ressources Azure de manière sécurisée, sans gérer de mot de passe.

Deux types :

* **Assignée par le système** : liée à une seule ressource
* **Assignée par l’utilisateur** : réutilisable entre plusieurs ressources

### Commande pour créer une identité managée assignée par l’utilisateur

```bash
az identity create \
  --name MyManagedIdentity \
  --resource-group MyResourceGroup
```

### Résultat attendu

```json
{
  "clientId": "b0f14b46-df49-4172-8325-8f7f6946005a",
  "name": "MyManagedIdentity",
  "resourceGroup": "MyResourceGroup"
}
```

---

### Exercice 3 – Créer une autre identité managée

```bash
az identity create \
  --name AnotherManagedIdentity \
  --resource-group MyResourceGroup
```

---

## 6. Récapitulatif des identités créées

| Type d’identité    | Nom               | Identifiant                               | Secret utilisé                     |
| ------------------ | ----------------- | ----------------------------------------- | ---------------------------------- |
| Utilisateur (User) | Bob Miller        | [bob@contoso.com](mailto:bob@contoso.com) | dQ76bmz7rMVs9gSP                   |
| Service Principal  | MyAmazingApp      | 728cd0c5-8ada-459f-8476-144a003d4857      | 2568Q\~R0ltg5Q2\_R\_G-vs5gN6X2pdta |
| Identité managée   | MyManagedIdentity | b0f14b46-df49-4172-8325-8f7f6946005a      | Aucun                              |

---


