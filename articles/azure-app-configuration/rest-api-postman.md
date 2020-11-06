---
title: Azure Active Directory REST API-Postman kullanarak test etme
description: Azure uygulama yapılandırmasını test etmek için Postman kullanın REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 9690678fc7b794c694e588a7993cb131d8264a72
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424485"
---
# <a name="test-the-azure-app-configuration-rest-api-using-postman"></a>Postman kullanarak Azure uygulama yapılandırma REST API test etme

[Postman](https://www.getpostman.com/)kullanarak REST API test etmek için isteklerinizin [kimlik doğrulaması](./rest-api-authentication-hmac.md) için gereken http üst bilgilerini eklemeniz gerekir. Kimlik doğrulama üstbilgilerini otomatik olarak oluşturmak için REST API test etmek üzere Postman 'ı yapılandırma hakkında bilgi edinebilirsiniz:

1. Yeni bir [istek](https://learning.getpostman.com/docs/postman/sending_api_requests/requests/) oluştur
1. `signRequest` [JavaScript kimlik doğrulama örneğinden](./rest-api-authentication-hmac.md#javascript) işlevi istek için [ön istek betiğine](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/) ekleyin
1. Ön istek betiğinin sonuna aşağıdaki kodu ekleyin. Erişim anahtarını TODO açıklamasıyla belirtilen şekilde güncelleştirin

    ```js
    // TODO: Replace the following placeholders with your access key
    var credential = "<Credential>"; // Id
    var secret = "<Secret>"; // Value

    var isBodyEmpty = pm.request.body === null || pm.request.body === undefined || pm.request.body.isEmpty();

    var headers = signRequest(
        pm.request.url.getHost(),
        pm.request.method,
        pm.request.url.getPathWithQuery(),
        isBodyEmpty ? undefined : pm.request.body.toString(),
        credential,
        secret);

    // Add headers to the request
    headers.forEach(header => {
        pm.request.headers.upsert({key: header.name, value: header.value});
    })
    ```

1. İsteği gönder
