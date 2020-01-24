---
title: Azure çoklu oturum kapatma SAML Protokolü | Microsoft Docs
description: Bu makalede Azure Active Directory içindeki çoklu oturum kapatma SAML Protokolü açıklanmaktadır
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: 95d3deff73ce357f012b15a7fc1cfa3decdb4bda
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701374"
---
# <a name="single-sign-out-saml-protocol"></a>Çoklu oturum kapatma SAML Protokolü

Azure Active Directory (Azure AD), SAML 2,0 Web tarayıcısı çoklu oturum açma profilini destekler. Çoklu oturum açma 'nın düzgün çalışması için uygulama kaydı sırasında uygulamanın **LogoutURL 'Sinin** Azure AD 'ye açık olarak kaydedilmesi gerekir. Azure AD, oturumu kapattıktan sonra kullanıcıları yeniden yönlendirmek için LogoutURL 'yi kullanır.

Aşağıdaki diyagramda Azure AD çoklu oturum açma işleminin iş akışı gösterilmektedir.

![Azure AD çoklu oturum açma Iş akışı](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
Bulut hizmeti, bir oturumun sonlandırıldığını göstermek için Azure AD 'ye bir `LogoutRequest` iletisi gönderir. Aşağıdaki alıntıda bir örnek `LogoutRequest` öğesi gösterilmektedir.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
Azure AD 'ye gönderilen `LogoutRequest` öğesi aşağıdaki öznitelikleri gerektirir:

* `ID`-bu, oturum kapatma isteğini tanımlar. `ID` değeri bir sayıyla başlamamalıdır. Tipik uygulama, bir GUID 'nin dize gösterimine **ID** eklemek için kullanılır.
* `Version`-bu öğenin değerini **2,0**olarak ayarlayın. Bu değer gereklidir.
* `IssueInstant`-bu, koordinat evrensel saat (UTC) değeri ve [gidiş dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)içeren bir `DateTime` dizesidir. Azure AD bu türden bir değer bekler, ancak bunu zorlamaz.

### <a name="issuer"></a>Veren
Bir `LogoutRequest` `Issuer` öğesi, Azure AD 'deki bulut hizmetindeki **ServicePrincipalNames adlarından** birine tam olarak uymalıdır. Genellikle, bu, uygulama kaydı sırasında belirtilen **uygulama KIMLIĞI URI** 'sine ayarlanır.

### <a name="nameid"></a>NameID
`NameID` öğenin değeri, imzalanmakta olan kullanıcının `NameID` tam olarak eşleşmelidir.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD, bir `LogoutRequest` öğesine yanıt olarak bir `LogoutResponse` gönderir. Aşağıdaki alıntıda örnek bir `LogoutResponse`gösterilmektedir.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure AD, `LogoutResponse` öğesindeki `ID`, `Version` ve `IssueInstant` değerlerini ayarlar. Ayrıca, `InResponseTo` öğesini, yanıtı kabul eden `LogoutRequest` `ID` özniteliğinin değerine ayarlar.

### <a name="issuer"></a>Veren
Azure AD bu değeri, Azure AD kiracısının kiracı KIMLIĞI olan \<TenantIdGUID > `https://login.microsoftonline.com/<TenantIdGUID>/` olarak ayarlar.

`Issuer` öğesinin değerini değerlendirmek için uygulama kaydı sırasında belirtilen **uygulama KIMLIĞI URI** 'sinin değerini kullanın.

### <a name="status"></a>Durum
Azure AD, oturum kapatma başarısını veya başarısızlığını göstermek için `Status` öğesindeki `StatusCode` öğesini kullanır. Kaydolma girişimi başarısız olduğunda, `StatusCode` öğesi özel hata iletileri de içerebilir.
