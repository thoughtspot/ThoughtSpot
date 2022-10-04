---
nav_title: "POST : Mettre à jour les messages de campagnes planifiés déclenchés par API"
article_title: "POST : Mettre à jour les messages de campagnes planifiés déclenchés par API"
search_tag: Endpoint
page_order: 4
layout: api_page
description: "Cet article présente en détail l’endpoint Braze Mettre à jour des campagnes planifiées déclenchées par API."

---
{% api %}
# Mettre à jour les campagnes planifiées déclenchées par API
{% apimethod post core_endpoint|https://www.braze.com/docs/core_endpoints %} 
/campaigns/trigger/schedule/update
{% endapimethod %}

Utilisez cet endpoint pour mettre à jour les campagnes planifiées déclenchées par API, qui sont créées sur le tableau de bord et initiées via l’API. Vous pouvez indiquer les `trigger_properties` qui seront modélisés dans le message lui-même.

Cet endpoint vous permet d’envoyer des messages de campagne via une livraison déclenchée par API, ce qui vous permet de décider quelle action doit déclencher le message à envoyer. Notez que pour envoyer des messages avec cet endpoint, vous devez avoir un ID de campagne créé lorsque vous élaborez une [campagne déclenchée par API]({{site.baseurl}}/api/api_campaigns/).

Toute planification écrasera complètement celle que vous avez établie dans la demande de création de planification ou dans les demandes de mise à jour de planification précédentes. Par exemple, si vous indiquez initialement `"schedule" : {"time" : "2015-02-20T13:14:47", "in_local_time" : true}` et, que dans votre mise à jour, vous renseignez `"schedule" : {"time" : "2015-02-20T14:14:47"}`, votre message sera envoyé à l’heure indiquée (UTC), et non à l’heure locale de l’utilisateur. Les déclencheurs planifiés qui sont mis à jour peu de temps avant ou pendant la période où ils sont censés être envoyés seront mis à jour dans les meilleurs délais, de sorte que les changements de dernière minute pourraient être appliqués à tous, certains ou aucun de vos utilisateurs ciblés.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#6d2a6e66-9d6f-4ae1-965a-79fa52b86b1d {% endapiref %}

## Limite de débit

{% include rate_limits.md endpoint='default' %}

## Corps de la demande

```
Content-Type: application/json
Authorization: Bearer YOUR-REST-API-KEY
```

```json
{
  "campaign_id": (required, string) see campaign identifier,
  "schedule_id": (required, string) the `schedule_id` to update (obtained from the response to create schedule),
  "schedule": {
    // required, see create schedule documentation
  }
}
```

## Paramètres de demande

| Paramètre | Requis | Type de données | Description |
| --------- | ---------| --------- | ----------- |
|`campaign_id`|Requis|Chaîne de caractères| Voir [Identifiant de campagne]({{site.baseurl}}/api/identifier_types/)|
| `schedule_id` | Facultatif | Chaîne de caractères | Le `schedule_id` à mettre à jour (obtenu à partir de la réponse pour créer une planification). |
|`schedule` | Requis | Objet | Voir [Objet de planification]({{site.baseurl}}/api/objects_filters/schedule_object/). |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Exemple de demande
```
curl --location --request POST 'https://rest.iad-01.braze.com/campaigns/trigger/schedule/update' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY' \
--data-raw '{
  "campaign_id": "campaign_identifier",
  "schedule_id": "schedule_identifier",
  "schedule": {
    "time": "2017-05-24T21:30:00Z",
    "in_local_time": true
  }
}'
```

{% endapi %}