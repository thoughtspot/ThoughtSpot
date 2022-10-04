---
nav_title: "GET : Analyse des séries de données de Canvas"
article_title: "GET : Analyse des séries de données de Canvas"
search_tag: Endpoint
page_order: 4
layout: api_page
page_type: reference
description: "Cet article présente en détail l’endpoint Analyse des séries de données de Canvas."

---
{% api %}
# Endpoint Analyse des séries de données de Canvas
{% apimethod get %}
/canvas/data_series
{% endapimethod %}

Cet endpoint vous permet d’exporter des données de série temporelles pour un Canvas.

{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#0fd61e93-7edf-4d87-a8dc-052420aefb73 {% endapiref %}

## Limite de débit

{% include rate_limits.md endpoint='default' %}

## Paramètres de demande

| Paramètre | Requis | Type de données | Description |
| --------- | -------- | --------- | ----------- |
| `canvas_id` | Requis | Chaîne de caractères | Voir [Identifiant API Canvas]({{site.baseurl}}/api/identifier_types/). |
| `ending_at` | Requis | Datetime <br>(chaîne de caractères [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601)) | Date à laquelle l’exportation de données doit se terminer. Par défaut, l’heure de la demande. |
| `starting_at` | Facultatif* | Datetime <br>(chaîne de caractères [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601)) | Date à laquelle l’exportation de données doit se terminer. <br><br>* `length` ou `starting_at` est nécessaire. |
| `length` | Facultatif* | Chaîne de caractères | Nombre maximum de jours avant `ending_at` à inclure dans la série renvoyée. Doit être compris entre 1 et 14 (inclus). <br><br>* `length` ou `starting_at` est nécessaire. |
| `include_variant_breakdown` | Facultatif | Booléen | S’il faut inclure ou non des statistiques de variante (par défaut sur Faux).  |
| `include_step_breakdown`    | Facultatif | Booléen | S’il faut inclure ou non des statistiques d’étape (par défaut sur Faux). |
| `include_deleted_step_data` | Facultatif | Booléen | S’il faut inclure ou non des statistiques d’étape pour les étapes supprimées (par défaut sur Faux). |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Exemple de demande
{% raw %}
```
curl --location -g --request GET 'https://rest.iad-01.braze.com/canvas/data_series?canvas_id={{canvas_id}}&ending_at=2018-05-30T23:59:59-5:00&starting_at=2018-05-28T23:59:59-5:00&include_variant_breakdown=true&include_step_breakdown=true&include_deleted_step_data=true' \
--header 'Authorization: Bearer YOUR-REST-API-KEY'
```
{% endraw %}

## Réponse

```json
Content-Type: application/json
Authorization: Bearer YOUR-REST-API-KEY
{
  "data": {
    "name": (string) Canvas name,
    "stats": [
      {
        "time": (string) date as ISO 8601 date,
        "total_stats": {
          "revenue": (float),
          "conversions": (int),
          "conversions_by_entry_time": (int),
          "entries": (int)
        },
        "variant_stats": (optional) {
          "00000000-0000-0000-0000-0000000000000": (API identifier for variant) {
            "name": (string) name of variant,
            "revenue": (int),
            "conversions": (int),
            "conversions_by_entry_time": (int),
            "entries": (int)
          },
          ... (more variants)
        },
        "step_stats": (optional) {
          "00000000-0000-0000-0000-0000000000000": (API identifier for step) {
            "name": (string) name of step,
            "revenue": (float),
            "conversions": (int),
            "conversions_by_entry_time": (int),
            "messages": {
              "email": [
                {
                  "sent": (int),
                  "opens": (int),
                  "unique_opens": (int),
                  "clicks": (int),
                  ... (more stats)
                }
              ],
              ... (more channels)
            }
          },
          ... (more steps)
        }
      },
      ... (more stats by time)
    ]
  },
  "message": (required, string) the status of the export, returns 'success' when completed without errors
}
```

{% alert tip %}
Pour obtenir de l’aide sur les exportations CSV et de l’API, consultez la section [Résolution des problèmes d’exportation]({{site.baseurl}}/user_guide/data_and_analytics/export_braze_data/export_troubleshooting/).
{% endalert %}

{% endapi %}