---
title: Azure API Yönetimi kimlik doğrulama ilkeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılmak üzere kullanılabilir kimlik doğrulama ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 828f738ff8923dc8194e2449f5fb0be74ef45ad7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473566"
---
# <a name="api-management-authentication-policies"></a>API Management kimlik doğrulaması ilkeleri
Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

##  <a name="authentication-policies"></a><a name="AuthenticationPolicies"></a>Kimlik doğrulama ilkeleri

-   Temel kimlik doğrulaması kullanarak bir arka uç hizmetiyle [Kimlik Doğrulaması](api-management-authentication-policies.md#Basic) - Temel ile kimlik doğrulaması.

-   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) - İstemci sertifikalarını kullanarak bir arka uç hizmetiyle kimlik doğrulaması.

-   [Yönetilen kimlikle kimlik doğrulaması](api-management-authentication-policies.md#ManagedIdentity) - API Yönetimi hizmeti için [yönetilen kimlikle](../active-directory/managed-identities-azure-resources/overview.md) kimlik doğrulaması.

##  <a name="authenticate-with-basic"></a><a name="Basic"></a>Temel ile kimlik doğrulaması
 Temel `authentication-basic` kimlik doğrulaması kullanarak bir arka uç hizmetiyle kimlik doğrulaması yapmak için ilkeyi kullanın. Bu ilke, HTTP Yetkilendirme üstbilgisini ilkede sağlanan kimlik bilgilerine karşılık gelen değere etkin bir şekilde ayarlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<authentication-basic username="username" password="password" />
```

### <a name="example"></a>Örnek

```xml
<authentication-basic username="testuser" password="testpassword" />
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|kimlik doğrulama-temel|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|kullanıcı adı|Temel kimlik bilgisinin kullanıcı adını belirtir.|Evet|Yok|
|password|Temel kimlik bilgisinin parolasını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="authenticate-with-client-certificate"></a><a name="ClientCertificate"></a>İstemci sertifikasıyla kimlik doğrulaması
 İstemci sertifikasını `authentication-certificate` kullanarak bir arka uç hizmetiyle kimlik doğrulaması yapmak için ilkeyi kullanın. Sertifikanın önce [API Yönetimi'ne yüklenmesi](https://go.microsoft.com/fwlink/?LinkID=511599) gerekir ve parmak izi ile tanımlanır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<authentication-certificate thumbprint="thumbprint" certificate-id="resource name"/>
```

### <a name="examples"></a>Örnekler

Bu örnekte istemci sertifikası parmak izi ile tanımlanır.
```xml
<authentication-certificate thumbprint="CA06F56B258B7A0D4F2B05470939478651151984" />
```
Bu örnekte istemci sertifikası kaynak adı ile tanımlanır.
```xml  
<authentication-certificate certificate-id="544fe9ddf3b8f30fb490d90f" />  
```  

### <a name="elements"></a>Öğeler  
  
|Adı|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama sertifikası|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Adı|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|Parmak izi|İstemci sertifikasıiçin parmak izi.|Ya `thumbprint` `certificate-id` da orada olmalı.|Yok|  
|sertifika-id|Sertifika kaynak adı.|Ya `thumbprint` `certificate-id` da orada olmalı.|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** tüm kapsamlar  

##  <a name="authenticate-with-managed-identity"></a><a name="ManagedIdentity"></a>Yönetilen kimlikle kimlik doğrulaması  
 API `authentication-managed-identity` Yönetimi hizmetinin yönetilen kimliğini kullanarak bir arka uç hizmetiyle kimlik doğrulaması yapmak için ilkeyi kullanın. Bu ilke, belirtilen kaynağa erişmek için Azure Active Directory'den bir erişim jetonu almak için yönetilen kimliği kullanır. Belirteci başarıyla aldıktan sonra, ilke `Authorization` `Bearer` düzeni kullanarak üstbilgideki belirteç değerini ayarlar.
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-managed-identity resource="resource" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>Örnek  
#### <a name="use-managed-identity-to-authenticate-with-a-backend-service"></a>Arka uç hizmetiyle kimlik doğrulama yapmak için yönetilen kimliği kullanma
```xml  
<authentication-managed-identity resource="https://graph.microsoft.com"/> 
```
```xml  
<authentication-managed-identity resource="https://management.azure.com/"/> <!--Azure Resource Manager-->
```
```xml  
<authentication-managed-identity resource="https://vault.azure.net"/> <!--Azure Key Vault-->
```
```xml  
<authentication-managed-identity resource="https://servicebus.azure.net/"/> <!--Azure Service Busr-->
```
```xml  
<authentication-managed-identity resource="https://storage.azure.com/"/> <!--Azure Blob Storage-->
```
```xml  
<authentication-managed-identity resource="https://database.windows.net/"/> <!--Azure SQL-->
```
  
#### <a name="use-managed-identity-in-send-request-policy"></a>Gönderme isteği ilkesinde yönetilen kimliği kullanma
```xml  
<send-request mode="new" timeout="20" ignore-error="false">
    <set-url>https://example.com/</set-url>
    <set-method>GET</set-method>
    <authentication-managed-identity resource="ResourceID"/>
</send-request>
```

### <a name="elements"></a>Öğeler  
  
|Adı|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama-yönetilen kimlik |Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Adı|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|kaynak|Dize. Azure Active Directory'deki hedef web API'sinin (güvenli kaynak) Uygulama Kimliği.|Evet|Yok|  
|çıkış-belirteç-değişken-adı|Dize. Nesne türü olarak belirteç değeri alacak bağlam `string`değişkeninin adı. |Hayır|Yok|  
|yoksay-hata|Boolean. `true`Ayarlanırsa, bir erişim belirteci elde edilmese bile ilke ardışık uygulama yürütmeye devam edecektir.|Hayır|yanlış|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar
İlkelerle çalışan daha fazla bilgi için bkz:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
