# EPFL Tools Custom Auth (ETCA)

Certains services créés par les étudiants (donc non officiels) ont besoin de vérifier qu'un utilisateur est bien un étudiant de l'EPFL.

Or, Tequila (le service d'authentification fédéral utilisé par l'EPFL) ne supporte que les services officiels.

## Fonctionnement de l'ETCA

* L'utilisateur demande à être authentifié sur le site.
* L'utilisateur entre son mail `@epfl.ch` sur le site.
* Le serveur envoie un code de confirmation au mail.
* L'utilisateur rentre le code sur le site.
* Le serveur récupère les informations depuis `people.epfl.ch`.
* En fonction des informations, le serveur autorise ou non la connexion. 

## Documentation

URL: https://etca.epfl.tools

### Commencer une authentification

* `POST /auth?mail=john.doe@epfl.ch`

Response:
```json
{
    "authId": AuthId,
    "status": AuthStatus,
    "email": "john.doe@epfl.ch"
}
```

AuthStatus est 'success' | 'failed'.
AuthId est l'identifiant de l'authentification en cours.

### Valider une authentification

* `POST /confirm?authId=AuthId&code=4288842`

Response:
```json
{
    "authId": AuthId,
    "status": AuthConfirmStatus,
    "token": AuthToken,
    "email": "john.doe@epfl.ch"
}
```

AuthConfirmStatus est 'confirmed' | 'failed'.
AuthToken est une chaîne de caractère servant à prouver au serveur que le compte a bien été validé par l'utilisateur.

### Vérifier une authentification

Maintenant côté serveur, avant d'ajouter une entrée dans la base de données par exemple, pour vérifier que l'utilisateur est bien authentifié.

* `GET /data?token=AuthToken`

Response:
```json
{
    "firstName": "John",
    "lastName": "Doe",
    "type": "student",
    "course": "GM-BA5",
    "email": "john.doe@epfl.ch"
}
```
