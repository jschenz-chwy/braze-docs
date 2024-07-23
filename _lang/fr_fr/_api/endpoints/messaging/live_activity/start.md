---
nav_title: "POST : Démarrer l’activité en direct"
article_title: "POST : Démarrer l’activité en direct"
search_tag: Endpoint
page_order: 1

layout: api_page
page_type: reference
description: "Cet article décrit en détail le point de terminaison Démarrer l’activité en direct."

---
{% api %}
# Démarrer l’activité en direct
{% apimethod post %}
/messages/live_activity/start
{% endapimethod %}

> Utilisez ce point de terminaison pour démarrer à distance les [activités en direct]({{site.baseurl}}/developer_guide/platform_integration_guides/swift/live_activities/live_activities/) affichées dans votre application iOS. Cet endpoint nécessite une configuration supplémentaire.

Après avoir créé une activité en direct, vous pouvez effectuer une requête POST pour démarrer à distance votre activité pour un segment donné. Pour en savoir plus sur les activités en direct d’Apple, consultez [la rubrique Démarrage et mise à jour des activités en direct avec les notifications push ActivityKit](https://developer.apple.com/documentation/activitykit/starting-and-updating-live-activities-with-activitykit-push-notifications).

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#2300226e-f26a-4154-9bcc-5883f1f294cd {% endapiref %}

## Conditions préalables

Pour utiliser cet endpoint, vous devrez effectuer les opérations suivantes :

- Générez une clé API avec l’autorisation `messages.live_activity.start` .
- [Créez une activité en direct]({{site.baseurl}}/developer_guide/platform_integration_guides/swift/live_activities/live_activities/#step-1-create-a-live-activity) à l’aide du SDK Braze Swift.

## Limite de débit

{% multi_lang_include rate_limits.md endpoint='default' %}

## Corps de la demande

```json
{
  "app_id": "(required, string) App API identifier retrieved from the Developer Console.",
  "activity_id": "(required, string) Define a custom string as your `activity_id`. You will use this ID when you wish to send update or end events to your Live Activity.",
  "activity_attributes_type": "(required, string) The activity attributes type you define within `liveActivities.registerPushToStart` in your app",
  "activity_attributes": "(required, object) The static attribute values for the activity type (i.e. the sports team names which don't change)",
  "content_state": "(required, object) You define the ContentState parameters when you create your Live Activity. Pass the updated values for your ContentState using this object. The format of this request must match the shape you initially defined.",
  "dismissal_date": "(optional, datetime in ISO-8601 format) The time to remove the Live Activity from the user’s UI. If this time is in the past, the Live Activity will be removed immediately.",
  "stale_date": "(optional, datetime in ISO-8601 format) The time the Live Activity content is marked as outdated in the user’s UI.",
  "notification": "(required, object) Include an `apple_push` object to define a push notification that creates an alert for the user, displayed on paired watchOS devices. Should include `notification.alert.title` and `notification.alert.body`",
  // One of the following:
  "external_user_ids": "(optional, array of strings) see external user identifier",
  "custom_audience": "(optional, connected audience object) see connected audience",
  "segment_id": "(optional, string) see segment identifier"
}
```

## Paramètres de demande

| Paramètre | Obligatoire | Type de données| Descriptif |
|-----------|----------|----------|--------------|
| `app_id` | Obligatoire | Chaîne | [Identificateur d’API]({{site.baseurl}}/api/identifier_types/#the-app-identifier) d’application récupéré à partir de la page [Clés d’API]({{site.baseurl}}/user_guide/administrative/app_settings/api_settings_tab/) .  |
| `activity_id` | Obligatoire | Chaîne de caractères | Définissez une chaîne de caractères personnalisée comme votre `activity_id`. Vous utiliserez cet ID lorsque vous souhaiterez envoyer des événements de mise à jour ou de fin à votre activité en direct.  |
| `activity_attributes_type`  | Obligatoire | Chaîne de caractères | Type d’attributs d’activité que vous définissez au sein de `liveActivities.registerPushToStart` dans votre application.  |
| `activity_attributes` | Obligatoire | Objet | Les valeurs d’attribut statiques pour le type d’activité (c’est-à-dire les noms des équipes sportives qui ne changent pas). |
| `content_state` | Obligatoire | Objet | Vous définissez les `ContentState` paramètres lorsque vous créez votre activité en direct. Transmettez les valeurs mises à jour pour votre `ContentState` en utilisant cet objet.<br><br>Le format de cette requête doit correspondre à la forme que vous avez initialement définie.
| `dismissal_date` | Facultatif | Date/heure <br>(chaîne [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) ) | Ce paramètre définit la durée de suppression de l’activité en direct de l’interface utilisateur de l’utilisateur. |
| `stale_date` | Facultatif | Date/heure <br>(chaîne [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) ) | Ce paramètre indique au système quand le contenu de l’activité en direct est marqué comme obsolète dans l’interface utilisateur de l’utilisateur. |
| `notification` | Obligatoire | Objet | Incluez un [`apple_push`]({{site.baseurl}}/api/objects_filters/messaging/apple_object/) objet pour définir une notification push. Le comportement de cette notification push dépend du fait que l’utilisateur soit actif ou utilise un appareil proxy. {::nomarkdown}<ul><li>Si une <code>notification</code> est incluse et que l’utilisateur est actif sur son iPhone lorsque la mise à jour est livrée, l’interface utilisateur Live Activity mise à jour glissera vers le bas et s’affichera comme une notification push.</li><li>Si une <code>notification</code> est incluse et que l’utilisateur n’est pas actif sur son iPhone, son écran s’allume pour afficher l’interface utilisateur Live Activity mise à jour sur son écran de verrouillage.</li><li><code>L’alerte de notification</code> ne s’affichera pas comme une notification push standard. De plus, si un utilisateur dispose d’un périphérique proxy, comme une Apple Watch, <code>l’alerte</code> y sera affichée.</li></ul>{:/} |
| `external_user_ids` | Facultatif si `segment_id` ou `audience` est fourni | Tableau de chaînes | Voir [ID utilisateur externe]({{site.baseurl}}/api/objects_filters/user_attributes_object/#braze-user-profile-fields).  |
| `segment_id `  | Facultatif si `external_user_ids` ou `audience` est fourni | Chaîne | Voir [identificateur de segment]({{site.baseurl}}/api/identifier_types/). |
| `custom_audience` | Facultatif si `external_user_ids` ou `segment_id` est fourni | Objet d’audience connectée | Voir [l’audience connectée]({{site.baseurl}}/api/objects_filters/connected_audience/). |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## Exemple de demande

```json
curl --location --request POST 'https://rest.iad-01.braze.com/messages/live_activity/start' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {YOUR-REST-API-KEY}' \
--data-raw '{
    "app_id": "{YOUR-APP-API-IDENTIFIER}",
    "activity_id": "football-chiefs-bills-2024-01-21",
    "content_state": {
        "teamOneScore": 0,
        "teamTwoScore": 0
    },
    "activity_attributes_type": "FootballActivity",
    "activity_attributes": {
        "team1Name": "Chiefs",
        "team2Name": "Bills"
    },
    "dismissal_date": "2024-01-22T00:00:00+0000",
    "stale_date": "2024-01-22T16:55:49+0000",
    "notification": {
        "alert": {
            "body": "The game is starting! Tune in soon!",
            "title": "Chiefs v. Bills"
        }
    },
    // One of the following required:
    "segment_id": "{YOUR-SEGMENT-API-IDENTIFIER}", // Optional
    "custom_audience": {...}, // Optional
    "external_user_ids": ["user-id1", "user-id2"], // Optional
}'
```

## Réponse

Deux réponses de code de statut existent pour cet endpoint : `201` et `4XX`.

### Exemple de réponse réussie

Un code de statut `201` est renvoyé si la requête a été formatée correctement et que nous l’avons reçue. Le code de statut `201` pourrait renvoyer le corps de réponse suivant.

```json
{
  "message": "success"
}
```

### Exemple de réponse échouée

La classe du code de statut `4XX` indique une erreur client. Reportez-vous à [l’article Erreurs et réponses de l’API]({{site.baseurl}}/api/errors/) pour plus d’informations sur les erreurs que vous pouvez rencontrer.

Le code de statut `400` pourrait renvoyer le corps de réponse suivant. 

```json
{
    "error": "\nProblem:\n  message body does not match declared format\nResolution:\n  when specifying application/json as content-type, you must pass valid application/json in the request's 'body' "
}
```

{% endapi %}