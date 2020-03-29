---
title: Azure Tek Oturum Açma SAML Protokolü | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizini'nde Tek Oturum Açma SAML Protokolü açıklanmaktadır
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76701374"
---
# <a name="single-sign-out-saml-protocol"></a>Tek Oturum Açma SAML Protokolü

Azure Active Directory (Azure AD), SAML 2.0 web tarayıcısı tek oturum açma profilini destekler. Tek oturum son hakkının doğru çalışabilmesi için, uygulamanın **LogoutURL'sinin** başvuru kaydı sırasında Azure AD'ye açıkça kaydedilmesi gerekir. Azure AD, oturumu aştıktan sonra kullanıcıları yönlendirmek için LogoutURL'yi kullanır.

Aşağıdaki diyagram, Azure AD tek oturum açma işleminin iş akışını gösterir.

![Azure AD Tek Oturum Çıkışı İş Akışı](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>Giriş İsteği
Bulut hizmeti, `LogoutRequest` bir oturumun sonlandırıldığını belirtmek için Azure AD'ye bir ileti gönderir. Aşağıdaki alıntı örnek `LogoutRequest` bir öğeyi gösterir.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>Giriş İsteği
Azure `LogoutRequest` AD'ye gönderilen öğe aşağıdaki öznitelikleri gerektirir:

* `ID`- Bu, oturum açma isteğini tanımlar. Değeri bir `ID` sayı ile başlamamalıdır. Tipik uygulama, **id'yi** GUID'in dize temsiline eklemektir.
* `Version`- Bu öğenin değerini **2,0**olarak ayarlayın. Bu değer gereklidir.
* `IssueInstant`- Bu `DateTime` bir Koordinat Evrensel Zaman (UTC) değeri ve [gidiş-dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)ile bir dize. Azure AD bu tür bir değer bekler, ancak zorlamaz.

### <a name="issuer"></a>Veren
A'daki `Issuer` `LogoutRequest` öğe, Azure AD'deki bulut hizmetindeki **ServicePrincipalNames'ten** biriyle tam olarak eşleşmelidir. Genellikle, bu uygulama kaydı sırasında belirtilen **App ID URI** olarak ayarlanır.

### <a name="nameid"></a>NameID
`NameID` Öğenin değeri, oturumdan `NameID` çıkan kullanıcıyla tam olarak eşleşmelidir.

## <a name="logoutresponse"></a>LogoutYanıt
Azure AD `LogoutResponse` bir `LogoutRequest` öğeye yanıt olarak bir yanıt gönderir. Aşağıdaki alıntı bir `LogoutResponse`örnek gösterir.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutYanıt
Azure AD `ID` `LogoutResponse` öğedeki `IssueInstant` ve `Version` değerleri ayarlar. Ayrıca, `InResponseTo` yanıtı ortaya `ID` `LogoutRequest` çıkaran özniteliğin değerine öğeyi ayarlar.

### <a name="issuer"></a>Veren
Azure AD bu `https://login.microsoftonline.com/<TenantIdGUID>/` değeri \<TenantIdGUID>'nin Azure AD kiracısının kiracı kimliği olduğu yere ayarlar.

Öğenin `Issuer` değerini değerlendirmek için, uygulama kaydı sırasında sağlanan **App ID URI** değerini kullanın.

### <a name="status"></a>Durum
Azure AD, `StatusCode` oturum `Status` açmanın başarısını veya başarısızlığını belirtmek için öğedeki öğeyi kullanır. Oturum açma denemesi başarısız olduğunda, `StatusCode` öğe özel hata iletileri de içerebilir.
