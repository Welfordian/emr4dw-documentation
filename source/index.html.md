---
title: SDK Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - php

includes:
  - errors

search: true
---

# Introduction

Welcome to the EMR4DW SDK documentation. Throughout this documentation you'll 
find all the necessary information to get started with using the EMR4DW SDK. By general rule of thumb
you'll be using the PHP SDK which will already be provided with the installation of EMR4DW.

# Authentication

Authentication with the EMR4DW SDK is **not** required. The only thing you'll need to do
is make sure you send a request signature along with your requests.

The signature consists of the following:-

* The Clinic ID
* The Clinic UID
* The Clinic API Key
* The Current Time

Request signature generation is already provided within the EMR4DW SDK.

```php
<?php

public function generateClientActionSignature($action = "show")
{
    return sha1($this->clinicID . $this->clinicUID . $action . (time() - 10) . $this->clinicApiKey);
}
```

The EMR4DW API uses API keys to allow access to the API. Each clinic is given a randomly generated API key which can be found in the `.env` file in `Laravel`.

All requests to the EMR4DW API **must** be signed. Signatures are only valid for 30 minutes and are only valid for the action the signature was generated for.

This means that signatures generated for the endpoint `/clinics/1` will **not** be valid for the endpoint `/clinics/1/plugins` and must be regenerated for each resource requested.

# Clinics

## Get Clinic

```php
<?php

Route::get('test', function (SDK\Clinic $clinic) {
  return $clinic->get();
})
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "name": "Hyderabad",
  "created_at": "2019-03-28 17:30:24",
  "updated_at": "2019-03-28 17:30:24",
  "subdomain": "hyderabad",
  "national_id": null,
  "dhis2_id": null,
  "national_pid_system": 0,
  "address": null,
  "postcode": null,
  "country": null,
  "timezone": null,
  "website": null,
  "level": null,
  "notes": null,
  "defaults": {
    "id": 1,
    "clinic_id": 1,
    "language": "English",
    "currency": null,
    "flag": null,
    "logo": null,
    "pid_prefix": null,
    "created_at": "2019-03-29 17:01:32",
    "updated_at": "2019-03-29 17:01:32"
  },
  "contacts": [],
  "plugins": []
}
```

This endpoint retrieves information about the given clinic.

**_NOTE:_** There is no way of retrieving information about a clinic other than the one making the request **through the EMR4DW SDK**. Even though this is bad practice, it cannot be stopped, so please refrain from using the API in this way. Clinics should not directly be sharing information.

### HTTP Request

`GET https://console.emr4dw.org/api/clinics/1`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
signature | true | The request signature must be present to get a valid API response.

<aside class="success">
<strong>Remember</strong> — All requests to the EMR4DW API <strong>must</strong> include a request signature!
</aside>

## Get plugins available for a clinic

```php
<?php

Route::get('test', function (SDK\Clinic $clinic) {
  return $clinic->plugins();
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "clinic_id": 1,
    "plugin_id": 1,
    "created_at": "2019-03-28 17:30:56",
    "updated_at": "2019-03-28 17:30:56",
    "plugin": {
      "id": 1,
      "name": "Pharmacy Stock",
      "description": "Adds pharmacy functionality to EMR4DW. Pharmacy item management, stock management & patient prescriptions.",
      "namespace": "PharmacyStock",
      "version": "0.2",
      "created_at": "2019-03-28 20:12:11",
      "updated_at": "2019-03-28 20:12:11",
      "downloadUri": "/api/plugins/PharmacyStock"
    }
  }
]
```

This endpoint retrieves plugins available for the given clinic.

### HTTP Request

`GET https://console.emr4dw.org/api/clinic/1/plugins`

### URL Parameters

Parameter | Required | Description
--------- | ----------- | -----------
Signature | true | The request signature must be present to get a valid API response.

## Get contacts for a clinic

```php
<?php

Route::get('test', function (SDK\Clinic $clinic) {
  return $clinic->contacts();
})
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 2,
    "clinic_id": 1,
    "name": "Bass Stewart",
    "phone": null,
    "email": "bass.stewart@onyxweb.co.uk",
    "created_at": "2019-03-29 17:02:12",
    "updated_at": "2019-03-29 17:02:12"
  }
]
```

This endpoint retrieves contacts for the specified clinic.

### HTTP Request

`GET https://console.emr4dw.org/api/clinics/1/contacts`

### URL Parameters

Parameter | Required | Description
--------- | ----------- | -----------
Signature | true | The request signature must be present to get a valid API response.

