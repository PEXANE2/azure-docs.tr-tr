---
title: Azure çoklu oturum kapatma SAML Protokolü
description: Bu makalede Azure Active Directory içindeki çoklu oturum kapatma SAML Protokolü açıklanmaktadır
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 07/19/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: dbe21d020d5d01f24913b95587721403fa218cc8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881276"
---
# <a name="single-sign-out-saml-protocol"></a>Çoklu oturum kapatma SAML Protokolü

Azure Active Directory (Azure AD), SAML 2,0 Web tarayıcısı çoklu oturum açma profilini destekler. Çoklu oturum açma 'nın düzgün çalışması için uygulama kaydı sırasında uygulamanın **LogoutURL 'Sinin** Azure AD 'ye açık olarak kaydedilmesi gerekir. Azure AD, oturumu kapattıktan sonra kullanıcıları yeniden yönlendirmek için LogoutURL 'yi kullanır.

Aşağıdaki diyagramda Azure AD çoklu oturum açma işleminin iş akışı gösterilmektedir.

![Azure AD çoklu oturum açma Iş akışı](./media/single-sign-out-saml-protocol/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest
Bulut hizmeti, bir oturumun `LogoutRequest` sonlandırıldığını göstermek IÇIN Azure AD 'ye bir ileti gönderir. Aşağıdaki alıntıda örnek `LogoutRequest` bir öğe gösterilmektedir.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest
Azure `LogoutRequest` ad 'ye gönderilen öğe aşağıdaki öznitelikleri gerektirir:

* `ID`-Bu, oturum kapatma isteğini tanımlar. Değeri bir sayıyla `ID` başlamamalıdır. Tipik uygulama, bir GUID 'nin dize gösterimine **ID** eklemek için kullanılır.
* `Version`-Bu öğenin değerini **2,0**olarak ayarlayın. Bu değer gereklidir.
* `IssueInstant`-Bu, koordinat `DateTime` evrensel saat (UTC) değeri ve [gidiş dönüş biçimi ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx)içeren bir dizedir. Azure AD bu türden bir değer bekler, ancak bunu zorlamaz.

### <a name="issuer"></a>Veren
İçindeki `Issuer` `LogoutRequest` öğesi, Azure AD 'Deki bulut hizmetindeki **ServicePrincipalNames adlarından** biriyle tam olarak eşleşmelidir. Genellikle, bu, uygulama kaydı sırasında belirtilen **uygulama KIMLIĞI URI** 'sine ayarlanır.

### <a name="nameid"></a>NameID
`NameID` Öğesinin değeri, imzalanmakta olan kullanıcının ile tam `NameID` olarak eşleşmelidir.

## <a name="logoutresponse"></a>LogoutResponse
Azure AD, bir `LogoutResponse` `LogoutRequest` öğesine yanıt olarak bir öğesi gönderir. Aşağıdaki alıntıda bir örnek `LogoutResponse`gösterir.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse
Azure `ID`ad, `Version` `IssueInstant` `LogoutResponse` öğesi ve değerlerini ayarlar. Ayrıca, `InResponseTo` öğesini, `ID` `LogoutRequest` yanıtı kabul eden öğesinin özniteliğinin değerine ayarlar.

### <a name="issuer"></a>Veren
Azure AD bu değeri, Azure `https://login.microsoftonline.com/<TenantIdGUID>/` ad \<kiracısının kiracı kimliği olan TenantIdGUID> olarak ayarlar.

`Issuer` Öğesinin değerini değerlendirmek için uygulama kaydı sırasında BELIRTILEN **uygulama kimliği URI** 'sinin değerini kullanın.

### <a name="status"></a>Durum
Azure AD, oturum `StatusCode` kapatma başarısını veya `Status` başarısızlığını göstermek için öğesindeki öğesini kullanır. Oturum kapatma denemesi başarısız olursa, `StatusCode` öğe özel hata iletileri de içerebilir.
