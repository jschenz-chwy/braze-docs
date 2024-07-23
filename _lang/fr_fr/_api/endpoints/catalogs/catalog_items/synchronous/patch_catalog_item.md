---
nav_title: "PATCH : Éditer un produit du catalogue"
article_title: "PATCH : Éditer un produit du catalogue"
search_tag: Endpoint
page_order: 4

layout: api_page
page_type: reference
description: "Cet article présente en détail l’endpoint Braze Éditer un produit du catalogue."

---
{% api %}
# Éditer un produit du catalogue
{% apimethod patch %}
/catalogs/{catalog_name}/items/{item_id}
{% endapimethod %}

> Utilisez ce point de terminaison pour modifier un élément existant dans votre catalogue.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#e35976ae-ff77-42b7-b691-a883c980d8c0 {% endapiref %}

## Conditions préalables

Pour utiliser cet endpoint, vous aurez besoin d'une [clé API]({{site.baseurl}}/api/basics#rest-api-key/) avec l’autorisation `catalogs.update_item`.

## Limite de débit

{% multi_lang_include rate_limits.md endpoint='synchronous catalog item' %}

## Paramètres de chemin

| Paramètre | Obligatoire | Type de données | Descriptif |
|---|---|---|---|
| `catalog_name` | Obligatoire | Chaîne | Nom du catalogue. |
| `item_id` | Obligatoire | Chaîne | L’ID de l’élément de catalogue. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Paramètres de demande

| Paramètre | Obligatoire | Type de données | Descriptif |
|---|---|---|---|
| `items` | Obligatoire | Tableau | Un tableau qui contient des objets élément. Les objets Produits devraient contenir les champs qui existent dans le catalogue à l’exception du champ `id`. Un seul objet de produit est autorisé par requête.
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Exemple de demande

```
curl --location --request PATCH 'https://rest.iad-03.braze.com/catalogs/restaurants/items/restaurant1' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY' \
--data-raw '{
  "items": [
    {
      "Name": "Restaurant",
      "Loyalty_Program": false,
      "Location": {
        "Latitude": 33.6112,
        "Longitude": -117.8711
      },
      "Open_Time": "2021-09-03T09:03:19.967+00:00"
    }
  ]
}'
```

## Réponse

Trois réponses de code de statut existent pour cet endpoint : `200`, `400` et `404`.

### Exemple de réponse réussie

Le code de statut `200` pourrait renvoyer le corps de réponse suivant.

```json
{
  "message": "success"
}
```

### Exemple de réponse échouée

Le code de statut `400` pourrait renvoyer le corps de réponse suivant. Consultez la `400`résolution des problèmes[](#troubleshooting) pour plus d’informations concernant les erreurs que vous pourriez rencontrer.

```json
{
  "errors": [
    {
      "id": "invalid-fields",
      "message": "Some of the fields given do not exist in the catalog",
      "parameters": [
        "id"
      ],
      "parameter_values": [
        "restaurant1"
      ]
    }
  ],
  "message": "Invalid Request"
}
```

## Résolution des problèmes

Le tableau suivant répertorie les erreurs renvoyées possibles et les étapes de résolution des problèmes associées.

| Erreur | Dépannage |
| --- | --- |
| `arbitrary-error` | Une erreur arbitraire s'est produite. Veuillez réessayer ou contacter l’Assistance[]({{site.baseurl}}/support_contact/).
| `catalog-not-found` | Vérifiez que le nom du catalogue est valide. |
| `filtered-set-field-too-long` | La valeur du champ est utilisée dans un ensemble filtré qui dépasse la limite de caractères pour un élément. |
| `id-in-body` | Un ID de produit existe déjà dans le catalogue. |
| `ids-too-large` | La limite de caractères pour chaque ID de produit est de 250 caractères. |
| `invalid-ids` | Les caractères pris en charge pour les noms d'ID d'élément sont les lettres, les chiffres, les traits d'union et les traits de soulignement. |
| `invalid-fields` | Confirmez que les champs de la demande existent dans le catalogue. |
| `invalid-keys-in-value-object` | Les clés d'objet d'élément ne peuvent pas inclure `.` ou `$`. |
| `item-not-found` | Vérifiez que l'article est dans le catalogue. |
| `item-array-invalid` | `items` doit être un tableau d’objets. |
| `items-too-large` | La limite de caractères pour chaque élément est de 5 000 caractères. |
| `request-includes-too-many-items` | Vous ne pouvez modifier qu'un seul produit de catalogue par requête. |
| `too-deep-nesting-in-value-object` | Les objets élément ne peuvent pas avoir plus de 50 niveaux d’imbrication. |
| `unable-to-coerce-value` | Les types d'éléments ne peuvent pas être convertis. |
{: .reset-td-br-1 .reset-td-br-2}

{% endapi %}