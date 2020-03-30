---
title: Azure AD Federasyonu Meta verileri | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizini'nin Azure Etkin Dizin belirteçlerini kabul eden hizmetler için yayımettiği federasyon meta veri belgesi açıklanmaktadır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: bcc44f61ccb7b4a19e7df39ab979669c5aa37da1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154908"
---
# <a name="federation-metadata"></a>Federasyon meta verileri

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Etkin Dizin (Azure AD), Azure AD'nin verdiği güvenlik belirteçlerini kabul etmek üzere yapılandırılan hizmetler için bir federasyon meta veri belgesi yayımlar. Federasyon meta veri belge biçimi [Web Hizmetleri Federasyonu Dili (WS-Federasyon) Sürüm 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html)açıklanmıştır , [OASIS Güvenlik İddia İşaretleme Dili (SAML) v2.0 için Meta veri](https://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)genişletir .

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Kiracıya özel ve Kiracıdan bağımsız meta veri uç noktaları
Azure AD, kiracıya özel ve kiracıdan bağımsız uç noktaları yayımlar.

Kiracıya özel uç noktalar belirli bir kiracı için tasarlanmıştır. Kiracıya özgü federasyon meta verileri, kiracıya özel veren ve bitiş noktası bilgileri de dahil olmak üzere kiracı hakkında bilgi içerir. Tek bir kiracıya erişimi kısıtlayan uygulamalar kiracıya özgü uç noktaları kullanır.

Kiracıdan bağımsız uç noktalar, tüm Azure AD kiracıları için ortak olan bilgiler sağlar. Bu *bilgiler, login.microsoftonline.com* barındırılan kiracılar için geçerlidir ve kiracılar arasında paylaşılır. Kiracıdan bağımsız uç noktalar, belirli bir kiracıyla ilişkili olmadıklarından, çok kiracılı uygulamalar için önerilir.

## <a name="federation-metadata-endpoints"></a>Federasyon meta veri uç noktaları
Azure AD, federasyon meta `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`verilerini .

**Kiracıya özgü uç**noktalar `TenantDomainName` için aşağıdaki türlerden biri olabilir:

* Bir Azure AD kiracısının kayıtlı etki `contoso.onmicrosoft.com`alanı adı: .
* Etki alanının değişmez kiracı kimliği, `72f988bf-86f1-41af-91ab-2d7cd011db45`örneğin.

**Kiracıdan bağımsız uç**noktalar `TenantDomainName` `common`için, Bu belge, yalnızca login.microsoftonline.com barındırılan tüm Azure AD kiracıları için ortak olan Federasyon Meta veri öğelerini listeler.

Örneğin, kiracıya özgü bir bitiş `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`noktası. Kiracıdan bağımsız bitiş [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml)noktası. Bu URL'yi bir tarayıcıya yazarak federasyon meta veri belgesini görüntüleyebilirsiniz.

## <a name="contents-of-federation-metadata"></a>Federasyon Meta verilerinin içeriği
Aşağıdaki bölüm, Azure AD tarafından verilen belirteçleri tüketen hizmetlerin ihtiyaç duyduğu bilgileri sağlar.

### <a name="entity-id"></a>Varlık Kimliği
Öğe `EntityDescriptor` bir `EntityID` öznitelik içerir. Özniteliğin `EntityID` değeri, belirteci veren ihracı, yani güvenlik belirteci hizmetini (STS) temsil eder. Bir belirteç aldığınızda vereni doğrulamak önemlidir.

Aşağıdaki meta veriler, bir `EntityDescriptor` `EntityID` öğeile örnek kiracı özgü öğegösterir.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Kiracıya özgü `EntityID` bir değer oluşturmak için kiracıdan bağımsız bitiş noktasındaki kiracı kimliğini kiracı kimliğinizle değiştirebilirsiniz. Elde edilen değer belirteç veren le aynı olacaktır. Strateji, çok kiracılı bir uygulamanın belirli bir kiracı için vereni doğrulamasına olanak tanır.

Aşağıdaki meta veriler, örnek kiracıdan bağımsız `EntityID` bir öğe gösterir. Lütfen unutmayın, `{tenant}` bu bir gerçek değil, bir yer tutucu olduğunu.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Belirteç imzalama sertifikaları
Bir hizmet, Bir Azure AD kiracıtarafından verilen bir belirteç aldığında, belirteç imzasının federasyon meta veri belgesinde yayınlanan bir imza anahtarıyla doğrulanması gerekir. Federasyon meta verileri, kiracıların belirteç imzalama için kullandıkları sertifikaların ortak bölümünü içerir. Sertifika ham baytlar öğede `KeyDescriptor` görünür. Belirteç imzalama sertifikası yalnızca öznitelik değeri `use` olduğunda imzalamak `signing`için geçerlidir.

Azure AD tarafından yayımlanan bir federasyon meta veri belgesinde, Azure AD'nin imzalama sertifikasını güncelleştirmeye hazırlandığı zaman gibi birden çok imzalama anahtarı olabilir. Bir federasyon meta veri sla' sı birden fazla sertifika içeriyorsa, belirteçleri doğrulayan bir hizmet belgedeki tüm sertifikaları desteklemelidir.

Aşağıdaki meta veriler, `KeyDescriptor` imzalama anahtarı olan bir örnek öğeyi gösterir.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Öğe `KeyDescriptor` federasyon meta veri belgesinde iki yerde görünür; WS-Federation'a özel bölümde ve SAML'ye özel bölümde. Her iki bölümde de yayınlanan sertifikalar aynı olacaktır.

WS-Federation'a özgü bölümde, WS-Federation meta veri okuyucusu sertifikaları `RoleDescriptor` `SecurityTokenServiceType` türüne sahip bir öğeden okur.

Aşağıdaki meta veriler bir `RoleDescriptor` örnek öğegösterir.

```
<RoleDescriptor xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:fed="https://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="https://docs.oasis-open.org/wsfed/federation/200706">
```

SAML'ye özgü bölümde, WS-Federation meta veri okuyucusu sertifikaları `IDPSSODescriptor` bir öğeden okur.

Aşağıdaki meta veriler bir `IDPSSODescriptor` örnek öğegösterir.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Kiracıya özel ve kiracıdan bağımsız sertifikaların biçiminde herhangi bir fark yoktur.

### <a name="ws-federation-endpoint-url"></a>WS-Federation bitiş noktası URL'si
Federasyon meta verileri, Azure AD'nin WS-Federation protokolünde tek oturum açma ve tek oturum açma için kullandığı URL'yi içerir. Bu bitiş noktası öğede `PassiveRequestorEndpoint` görünür.

Aşağıdaki meta veriler, `PassiveRequestorEndpoint` kiracıya özgü bir bitiş noktası için örnek bir öğegösterir.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Kiracıdan bağımsız bitiş noktası için, WS-Federation URL aşağıdaki örnekte gösterildiği gibi WS-Federation bitiş noktasında görünür.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokolü bitiş noktası URL'si
Federasyon meta verileri, Azure AD'nin SAML 2.0 protokolünde tek oturum açma ve tek oturum açma için kullandığı URL'yi içerir. Bu uç noktalar `IDPSSODescriptor` öğede görünür.

Oturum açma ve imzalama URL'leri `SingleSignOnService` ve `SingleLogoutService` öğeler görünür.

Aşağıdaki meta veriler, `PassiveResistorEndpoint` kiracıya özgü bir bitiş noktası için bir örnek gösterir.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Benzer şekilde, ortak SAML 2.0 protokol uç noktalarının uç noktaları aşağıdaki örnekte gösterildiği gibi kiracıdan bağımsız federasyon meta verilerinde de yayınlanır.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
