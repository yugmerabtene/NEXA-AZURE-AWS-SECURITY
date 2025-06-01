# LAB-01 — Création et gestion des identités dans Azure Active Directory

---

## Objectifs pédagogiques

* Comprendre les différents types d’identités Azure AD : utilisateurs, service principals, identités managées
* Créer un tenant Azure AD personnel
* Attribuer les rôles nécessaires pour créer des identités
* Créer et tester les identités via Azure CLI dans Azure Cloud Shell

---

## Prérequis

* Avoir un **compte Azure for Students actif**
* Disposer d’un navigateur web
* Se connecter à [https://portal.azure.com](https://portal.azure.com)

---

## Étape 0 — Créer un tenant Azure AD personnel (important)

### Pourquoi ?

Avec un compte Azure for Students, **tu n’as pas les droits suffisants** dans le tenant par défaut pour créer des utilisateurs ou manipuler Azure AD librement.

### Étapes

1. Ouvre [https://portal.azure.com](https://portal.azure.com)
2. Recherche **Azure Active Directory**
3. Clique sur **+ Créer un tenant**
4. Sélectionne **Azure Active Directory** (pas B2C)
5. Remplis les champs :

   * Nom de l’organisation : `MonTenantLab`
   * Domaine : `monlab.onmicrosoft.com`
   * Pays : France
6. Clique sur **Créer**

---

## Étape 1 — Basculer vers le bon tenant

1. Clique en haut à droite sur ton profil > **Basculer de répertoire**
2. Choisis le tenant que tu viens de créer (`monlab.onmicrosoft.com`)
3. Tu es maintenant **administrateur global** de ce tenant

---

## Étape 2 — Ouvrir Azure Cloud Shell

1. Va sur [https://shell.azure.com](https://shell.azure.com)
2. Choisis **Bash**
3. Un groupe de ressources + stockage sera créé automatiquement (si c’est la 1re fois)

---

## Étape 3 — Créer un utilisateur Azure AD

```bash
az ad user create \
  --display-name "Bob Miller" \
  --password dQ76bmz7rMVs9gSP \
  --user-principal-name bob@monlab.onmicrosoft.com \
  --force-change-password-next-sign-in true
```

### Résultat attendu

```json
{
  "displayName": "Bob Miller",
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userPrincipalName": "bob@monlab.onmicrosoft.com"
}
```

---

## Exercice 1 — Créer l’utilisateur Charlie

```bash
az ad user create \
  --display-name "Charlie Stone" \
  --password gT92bnz8rNXv3yLP \
  --user-principal-name charlie@monlab.onmicrosoft.com \
  --force-change-password-next-sign-in true
```

---

## Étape 4 — Créer un Service Principal

```bash
az ad sp create-for-rbac -n "MyAmazingApp"
```

### Résultat attendu

```json
{
  "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "displayName": "MyAmazingApp",
  "password": "motDePasseUltraComplexe",
  "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

---

## Exercice 2 — Créer un autre Service Principal

```bash
az ad sp create-for-rbac -n "MySecondApp"
```

---

## Étape 5 — Créer une Identité Managée assignée par l’utilisateur

1. Si besoin, créer un groupe de ressources :

```bash
az group create --name MyResourceGroup --location westeurope
```

2. Créer l’identité :

```bash
az identity create \
  --name MyManagedIdentity \
  --resource-group MyResourceGroup
```

### Résultat attendu

```json
{
  "name": "MyManagedIdentity",
  "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

---

## Exercice 3 — Créer une autre identité managée

```bash
az identity create \
  --name AnotherManagedIdentity \
  --resource-group MyResourceGroup
```

---

## Résumé

| Type d’identité   | Nom               | Identifiant unique                                                      | Mot de passe / Secret           |
| ----------------- | ----------------- | ----------------------------------------------------------------------- | ------------------------------- |
| Utilisateur       | Bob Miller        | [bob@monlab.onmicrosoft.com](mailto:bob@monlab.onmicrosoft.com)         | dQ76bmz7rMVs9gSP                |
| Utilisateur       | Charlie Stone     | [charlie@monlab.onmicrosoft.com](mailto:charlie@monlab.onmicrosoft.com) | gT92bnz8rNXv3yLP                |
| Service Principal | MyAmazingApp      | appId généré automatiquement                                            | password généré automatiquement |
| Identité managée  | MyManagedIdentity | clientId généré automatiquement                                         | Aucun (géré par Azure)          |

---
