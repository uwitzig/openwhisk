---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Gestione degli spazi dei nomi
{: #namespaces}

Con {{site.data.keyword.openwhisk}}, puoi creare spazi dei nomi gestiti IAM (Identity and Access Management) per raggruppare tra loro le entità, quali azioni o trigger. Successivamente, puoi creare delle politiche di accesso IAM per lo spazio dei nomi.
{: shortdesc}


**Cos'è uno spazio dei nomi?**

Gli spazi dei nomi contengono entità {{site.data.keyword.openwhisk_short}}, quali azioni e trigger, e appartengono a un gruppo di risorse. Puoi consentire agli utenti di accedere alle tue entità concedendo loro l'accesso allo spazio dei nomi.

Il nome completo di un'entità è
`/namespaceName/[packageName]/entityName`.


**Cosa succede quando creo uno spazio dei nomi?**

Gli spazi dei nomi creati in {{site.data.keyword.openwhisk_short}} vengono identificati come istanza di servizio IAM.
Durante la creazione di uno spazio dei nomi, puoi specificare il [gruppo di risorse](/docs/resources?topic=resources-rgs) in cui aggiungere l'istanza del servizio.

Quando crei il tuo spazio dei nomi, vengono create contemporaneamente le seguenti risorse:

* Un ID servizio utilizzabile come ID funzionale durante le chiamate in uscita. Tutte le azioni create in questo spazio dei nomi possono utilizzare questo ID servizio per accedere ad altre risorse. Per visualizzare tutti gli ID servizio, esegui `ibmcloud iam service - ids`.

* Una chiave API per l'ID servizio che può essere utilizzata per generare token IAM. Puoi quindi utilizzare i token per autenticare lo spazio dei nomi con altri servizi {{site.data.keyword.Bluemix_notm}}. La chiave API viene fornita alle azioni come una variabile di ambiente.

    Non eliminare chiavi API.
    {: tip}

**Ci sono limiti per gli spazi dei nomi?**

[La creazione delle API con il gateway API](/docs/openwhisk?topic=cloud-functions-apigateway) e l'utilizzo dell'[SDK mobile](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) non sono supportati per gli spazi dei nomi gestiti da IAM.

{{site.data.keyword.openwhisk_short}} ha delle restrizioni sui nomi dello spazio dei nomi. Per ulteriori informazioni, fai riferimento alla documentazione [Dettagli e limiti del sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov).
{: tip}



**Cosa faccio se ho uno spazio dei nomi basato su Cloud Foundry?**

I tuoi spazi dei nomi basati su Cloud Foundry continueranno a funzionare. Tuttavia, per poter usufruire delle nuove funzioni, devi [migrare i tuoi spazi dei nomi a IAM](/docs/resources?topic=resources-migrate).

</br>


## Creazione di uno spazio dei nomi con la CLI
{: #namespaces_create}

Puoi creare uno spazio dei nomi gestito da IAM come parte di un gruppo di risorse e gestire le politiche di accesso per le tue risorse selezionando il gruppo di risorse quando viene creato uno spazio dei nomi. Se hai altri utenti che richiedono l'accesso al tuo spazio dei nomi o se vuoi accedere ad altre risorse dalle azioni del tuo spazio dei nomi, assicurati di configurare le politiche IAM dopo la creazione del tuo spazio dei nomi.
{: shortdesc}

1. Seleziona il gruppo di risorse in cui vuoi creare lo spazio dei nomi. Se non hai ancora creato un [gruppo di risorse](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puoi selezionare il gruppo predefinito (`default`).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crea uno spazio dei nomi abilitato IAM.

  ```
  ibmcloud fn namespace create <namespace_name> [-n <description>]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>Il nome di visualizzazione per lo spazio dei nomi basato su IAM.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facoltativo: aggiungi una descrizione allo spazio dei nomi, ad esempio quale tipo di azioni o pacchetti conterrà.</td>
      </tr>
    </tbody>
  </table>

  Output di esempio:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verifica che il tuo nuovo spazio dei nomi sia stato creato.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Output di esempio:

  ```
  Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  Puoi anche elencare tutti gli spazi dei nomi, inclusi quelli basati su IAM e Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Prima di creare entità nello spazio dei nomi, imposta il tuo contesto della CLI sullo spazio dei nomi specificandolo.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

## Creazione di uno spazio dei nomi con l'API
{: #namespaces_create_api}

Puoi creare uno spazio dei nomi gestito da IAM come parte di un gruppo di risorse e gestire le politiche di accesso per le tue risorse selezionando il gruppo di risorse quando viene creato uno spazio dei nomi. Se hai altri utenti che richiedono l'accesso al tuo spazio dei nomi o se vuoi accedere ad altre risorse dalle azioni del tuo spazio dei nomi, assicurati di configurare le politiche IAM dopo la creazione del tuo spazio dei nomi.
{: shortdesc}


1. Seleziona il gruppo di risorse in cui vuoi creare lo spazio dei nomi. Se non hai ancora creato un [gruppo di risorse](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puoi selezionare il gruppo predefinito (`default`).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crea uno spazio dei nomi abilitato IAM.

  ```
  curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>Il tuo token Identity and Access Management (IAM) {{site.data.keyword.Bluemix_notm}}. Per richiamare il tuo token IAM, esegui <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>Il nome dello spazio dei nomi.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>L'ID del gruppo di risorse in cui vuoi creare lo spazio dei nomi. Per visualizzare gli ID del gruppo di risorse, esegui <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>L'ID del piano di risorse, ad esempio functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facoltativo: aggiungi una descrizione allo spazio dei nomi, ad esempio quale tipo di azioni o pacchetti conterrà.</td>
      </tr>
    </tbody>
  </table>

  Output di esempio:

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. Verifica che il tuo nuovo spazio dei nomi sia stato creato.

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Puoi anche elencare tutti gli spazi dei nomi, inclusi quelli basati su IAM e Cloud Foundry:
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Output di esempio:
  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}


Per ulteriori informazioni sull'utilizzo di REST HTTP, consulta la [documentazione API {{site.data.keyword.openwhisk_short}}](/apidocs/functions).
{: tip}



## Passi successivi
{: #namespaces_next}

Ora che hai creato uno spazio dei nomi, puoi creare le politiche di accesso IAM per proteggerlo. Per iniziare, consulta [Gestione dell'accesso](/docs/openwhisk?topic=cloud-functions-iam). Per ulteriori informazioni su come puoi gestire gli spazi dei nomi basati su IAM, vedi la [Guida di riferimento API REST {{site.data.keyword.openwhisk_short}}](/apidocs/functions).

