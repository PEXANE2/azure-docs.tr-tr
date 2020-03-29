---
title: 'Azure AD Connect: Tek Oturum Açma için SAML 2.0 Kimlik Sağlayıcısı kullanın - Azure'
description: Bu belge, tek oturum açma için SAML 2.0 uyumlu idp kullanarak açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff4f1b81f476159c13d3725cd3cb1766aec7dc87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70305109"
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Çoklu Oturum Açma için SAML 2.0 Kimlik Sağlayıcısı (IdP) Kullanma

Bu belge, tercih edilen Güvenlik Belirteç Hizmeti (STS) / kimlik sağlayıcısı olarak SAML 2.0 uyumlu SP-Lite profil tabanlı kimlik sağlayıcısı kullanma hakkında bilgi içerir. Bu senaryo, SAML 2.0 kullanarak erişilebilen bir kullanıcı dizini ve parola deposu zaten varsa yararlıdır. Bu varolan kullanıcı dizini, Office 365 ve diğer Azure AD tarafından güvenli kaynaklarda oturum açma için kullanılabilir. SAML 2.0 SP-Lite profili, oturum açma ve öznitelik değişim çerçevesi sağlamak için yaygın olarak kullanılan Güvenlik İddiası Biçimlendirme Dili (SAML) federe kimlik standardına dayanmaktadır.

>[!NOTE]
>Azure AD ile kullanılmak üzere test edilmiş üçüncü taraf Idps listesi için [Azure AD federasyonu uyumluluk listesine](how-to-connect-fed-compatibility.md) bakın

Microsoft, bu oturum açma deneyimini Office 365 gibi bir Microsoft bulut hizmetinin düzgün yapılandırılmış SAML 2.0 profil tabanlı IdP'nizle tümleştirmesi olarak destekler. SAML 2.0 kimlik sağlayıcıları üçüncü taraf ürünlerdir ve bu nedenle Microsoft dağıtım, yapılandırma, sorun giderme en iyi uygulamaları için destek sağlamaz. Düzgün bir şekilde yapılandırıldıktan sonra, SAML 2.0 kimlik sağlayıcısıyla tümleştirme, aşağıda daha ayrıntılı olarak açıklanan Microsoft Bağlantı Çözümleyici Aracı kullanılarak doğru yapılandırma için sınanabilir. SAML 2.0 SP-Lite profil tabanlı kimlik sağlayıcınız hakkında daha fazla bilgi için, bu kuruluşu sağlayan kuruluşa danışın.

> [!IMPORTANT]
> SAML 2.0 kimlik sağlayıcıları ile bu oturum açma senaryosunda yalnızca sınırlı sayıda istemci mevcuttur ve bu şunları içerir:
> 
> - Outlook Web Erişimi ve SharePoint Online gibi Web tabanlı istemciler
> - Temel kimlik doğrulaması ve IMAP, POP, Active Sync, MAPI, vb. gibi desteklenen Exchange erişim yöntemini kullanan e-posta açısından zengin istemciler (Geliştirilmiş İstemci Protokolü bitiş noktasının dağıtılması gereklidir) aşağıdakiler de dahil olmak üzere:
>     - Microsoft Outlook 2010/Outlook 2013/Outlook 2016, Apple iPhone (çeşitli iOS sürümleri)
>     - Çeşitli Google Android Cihazlar
>     - Windows Phone 7, Windows Phone 7.8 ve Windows Phone 8.0
>     - Windows 8 Posta İstemci ve Windows 8.1 Posta İstemci
>     - Windows 10 Posta İstemci

Diğer tüm istemciler, SAML 2.0 Kimlik Sağlayıcınızla bu oturum açma senaryosunda kullanılamaz. Örneğin, Lync 2010 masaüstü istemcisi, tek oturum açma için yapılandırılan SAML 2.0 Kimlik Sağlayıcınız ile hizmette oturum açamamaktadır.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Azure AD SAML 2.0 protokol gereksinimleri
Bu belge, bir veya daha fazla Microsoft bulut hizmetinde (Office 365 gibi) oturum açmayı etkinleştirmek için SAML 2.0 kimlik sağlayıcınızın Azure AD ile federate yapmak için uygulaması gereken protokol ve ileti biçimlendirmesi ile ilgili ayrıntılı gereksinimleri içerir. Bu senaryoda kullanılan bir Microsoft bulut hizmeti için SAML 2.0 güvenen taraf (SP-STS), Azure AD'dir.

SAML 2.0 kimlik sağlayıcı çıkış iletilerinizin sağlanan örnek izlemelere mümkün olduğunca benzer olmasını sağlamanız önerilir. Ayrıca, mümkünse sağlanan Azure AD meta verilerinden belirli öznitelik değerlerini kullanın. Çıktı iletilerinizden memnun kaldıktan sonra, aşağıda açıklandığı gibi Microsoft Bağlantı Çözümleyicisi ile test edebilirsiniz.

Azure AD meta verileri bu URL'den [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml)indirilebilir: .
Office 365'in Çin'e özgü örneğini kullanan Çin'deki müşteriler [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml)için aşağıdaki federasyon bitiş noktası kullanılmalıdır: .

## <a name="saml-protocol-requirements"></a>SAML protokol gereksinimleri
Bu bölümde, iletilerinizi doğru biçimlendirmenize yardımcı olmak için istek ve yanıt iletisi çiftleri nasıl bir araya getirilir ayrıntılı olarak açıklanmaktadır.

Azure AD, saml 2.0 SP Lite profilini kullanan kimlik sağlayıcılarıyla çalışacak şekilde yapılandırılabilir ve aşağıda belirtildiği gibi bazı özel gereksinimler içerir. Örnek SAML istek ve yanıt iletilerini otomatik ve el ile sınamanın yanı sıra kullanarak Azure AD ile birlikte çalışabilirlik elde etmek için çalışabilirsiniz.

## <a name="signature-block-requirements"></a>İmza bloğu gereksinimleri
SAML Yanıt iletisi içinde, İmza düğümü iletinin kendisi için dijital imza hakkında bilgi içerir. İmza bloğu aşağıdaki gereksinimlere sahiptir:

1. İddia düğümünün kendisi imzalanmalıdır
2.  RSA-sha1 algoritması DigestMethod olarak kullanılmalıdır. Diğer dijital imza algoritmaları kabul edilmez.
   `<ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  XML belgesini de imzalayabilirsin. 
4.  Dönüştürme Algoritması aşağıdaki örnekteki değerlerle eşleşmelidir:`<ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  İmza Yöntemi Algoritması aşağıdaki örnekle eşleşmelidir:`<ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Desteklenen bağlamalar
Bağlamalar, taşımayla ilgili iletişim parametreleridir. Aşağıdaki gereksinimler bağlayıcılar için geçerlidir

1. HTTPS gerekli aktarımdır.
2.  Azure AD, oturum açma sırasında belirteç gönderimi için HTTP POST'u gerektirir.
3.  Azure AD, kimlik sağlayıcısına kimlik doğrulama isteği için HTTP POST'u ve kimlik sağlayıcısına iletiyi imzalaması için YÖNLENDIRME'yi kullanır.

## <a name="required-attributes"></a>Gerekli öznitelikler
Bu tablo, SAML 2.0 iletisinde belirli öznitelikler için gereksinimleri gösterir.
 
|Öznitelik|Açıklama|
| ----- | ----- |
|NameID|Bu iddianın değeri, Azure AD kullanıcısının Değişmez Kimliği ile aynı olmalıdır. 64 alfa sayısal karaktere kadar olabilir. Html olmayan güvenli karakterler kodlanmalıdır, örneğin "+" bir karakter ".2B" olarak gösterilir.|
|IDPEmail|Kullanıcı Adı (UPN), SAML yanıtında IDPEmail Kullanıcının Azure AD/Office 365'teki UserPrincipalName (UPN) adı ile bir öğe olarak listelenir. UPN e-posta adresi biçimindedir. Windows Office 365'te UPN değeri (Azure Etkin Dizini).|
|Veren|Kimlik sağlayıcısının URI'si olması gerekir. Örnek iletilerden Veren'i yeniden kullanmayın. Azure AD kiracılarınızda birden çok üst düzey etki alanınız varsa, İhraççı'nın etki alanı başına yapılandırılan belirtilen URI ayarıyla eşleşmesi gerekir.|

>[!IMPORTANT]
>Azure AD şu anda SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid-format:persistent için aşağıdaki NameID Format URI'yi destekler.

## <a name="sample-saml-request-and-response-messages"></a>Örnek SAML istek ve yanıt iletileri
Oturum açma iletisi değişimi için bir istek ve yanıt iletisi çifti gösterilir.
Aşağıda, Azure AD'den örnek bir SAML 2.0 kimlik sağlayıcısına gönderilen örnek bir istek iletisi vereceksiniz. Örnek SAML 2.0 kimlik sağlayıcısı Aktif Dizin Federasyon Hizmetleri (AD FS) SAML-P protokolü kullanmak üzere yapılandırılmıştır. Diğer SAML 2.0 kimlik sağlayıcılarıyla birlikte çalışabilirlik testleri de tamamlanmıştır.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Aşağıda, örnek SAML 2.0 uyumlu kimlik sağlayıcısından Azure AD / Office 365'e gönderilen örnek yanıt iletisi vereceksiniz.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>SAML 2.0 uyumlu kimlik sağlayıcınızı yapılandırın
Bu bölümde, SAML 2.0 protokolünü kullanarak bir veya daha fazla Microsoft bulut hizmetine (Office 365 gibi) tek oturum açma erişimi sağlamak için SAML 2.0 kimlik sağlayıcınızı Azure AD ile federate olarak yapılandırmanız hakkında yönergeler bulunmaktadır. Bu senaryoda kullanılan bir Microsoft bulut hizmetiiçin saml 2.0 güvenen taraf Azure AD'dir.

## <a name="add-azure-ad-metadata"></a>Azure AD meta verisi ekleme
SAML 2.0 kimlik sağlayıcınızın Azure AD'ye güvenen taraf hakkındaki bilgilere uyması gerekir. Azure AD meta verileri https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

SAML 2.0 kimlik sağlayıcınızı yapılandırırken her zaman en son Azure AD meta verilerini almanız önerilir.

>[!NOTE]
>Azure AD, kimlik sağlayıcısının meta verilerini okumaz.

## <a name="add-azure-ad-as-a-relying-party"></a>Azure AD'yi güvenilen bir taraf olarak ekleme
SAML 2.0 kimlik sağlayıcınız ile Azure AD arasındaki iletişimi etkinleştirmeniz gerekir. Bu yapılandırma, belirli kimlik sağlayıcınıza bağlıdır ve bunun için belgelere başvurmalısınız. Genellikle güvenen parti kimliğini Azure AD meta verilerindeki entityID ile aynı şekilde ayarlarsınız.

>[!NOTE]
>SAML 2.0 kimlik sağlayıcı sunucunuzdaki saatin doğru bir zaman kaynağıyla senkronize edildiğini doğrulayın. Hatalı bir saat süresi federe girişlerin başarısız lığa neden olabilir.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>SAML 2.0 kimlik sağlayıcısıyla oturum açma için Windows PowerShell'i yükleme
SAML 2.0 kimlik sağlayıcınızı Azure AD oturumunu açma ile kullanmak üzere yapılandırdıktan sonra, bir sonraki adım Windows PowerShell için Azure Active Directory Modül'ü indirmek ve yüklemektir. Yüklendikten sonra, Azure AD etki alanlarınızı federe etki alanları olarak yapılandırmak için bu cmdletleri kullanırsınız.

Windows PowerShell için Azure Active Directory Modülü, kuruluşların verilerinizin Azure AD'de yönetilmesi için bir indirme dir. Bu modül Windows PowerShell'e bir dizi cmdlet yükler; Azure AD'ye tek oturum açma erişimi ayarlamak ve abone olduğunuz tüm bulut hizmetlerine sırayla bu cmdletleri çalıştırın. Cmdlets'in nasıl indirilip yüklenirken nasıl yüklenir?[https://technet.microsoft.com/library/jj151815.aspx](https://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>SAML kimlik sağlayıcınız ile Azure AD arasında bir güven ayarlama
Bir Azure AD etki alanında federasyon yapılandırmadan önce, özel bir etki alanı yapılandırılmış olması gerekir. Microsoft tarafından sağlanan varsayılan etki alanını fesatlayamazsınız. Microsoft'un varsayılan etki alanı "onmicrosoft.com" ile sona erer.
Tek oturum açma için etki alanları eklemek veya dönüştürmek için Windows PowerShell komut satırı arabiriminde bir dizi cmdlet çalıştırın.

SAML 2.0 kimlik sağlayıcınızı kullanarak fetoye etmek istediğiniz her Azure Etkin Dizin etki alanı, tek oturum açma etki alanı olarak eklenmeli veya standart bir etki alanından tek oturum açma etki alanına dönüştürülmelidir. Etki alanı ekleme veya dönüştürme, SAML 2.0 kimlik sağlayıcınız ile Azure AD arasında bir güven ayarlar.

Aşağıdaki yordam, SAML 2.0 SP-Lite kullanarak varolan standart bir etki alanını federe bir etki alanına dönüştürme yoluyla size yol eder. 

>[!NOTE]
>Etki alanınız, bu adımı attıktan sonra kullanıcıları 2 saate kadar etkileyen bir kesintiyle karşılaşabilir.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Federasyon için Azure REKLAM Dizininizde bir etki alanı yapılandırma


1. Azure REKLAM Dizininize kiracı yönetici olarak bağlanın: Connect-MsolService .
2.  SamL 2.0 ile federasyonu kullanacak şekilde istediğiniz Office 365 etki alanını yapılandırın:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $BrandName -Authentication Federated -PassiveLogOnUri $LogOnUrl -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $MyURI -LogOffUri $LogOffUrl -PreferredAuthenticationProtocol $Protocol` 

3.  IDP meta veri dosyanızdan imza sertifikası base64 kodlanmış dizesini alabilirsiniz. Bu konumun bir örneği sağlanmıştır, ancak uygulamanıza bağlı olarak biraz farklılık gösterebilir.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

"Set-MsolDomainAuthentication" hakkında daha fazla [https://technet.microsoft.com/library/dn194112.aspx](https://technet.microsoft.com/library/dn194112.aspx)bilgi için bkz: .

>[!NOTE]
>Yalnızca kimlik `$ecpUrl = "https://WS2012R2-0.contoso.com/PAOS"` sağlayıcınız için bir ECP uzantısı ayarladıysanız kullanmanız gerekir. Outlook Web Uygulaması (OWA) hariç Exchange Online istemcileri, POST tabanlı etkin bitiş noktasına güvenir. SAML 2.0 STS'niz Shibboleth'in Etkin bir bitiş noktası nın ECP uygulamasına benzer etkin bir bitiş noktası uygularsa, bu zengin istemcilerin Exchange Online hizmetiyle etkileşimde bulunmaları mümkün olabilir.

Federasyon yapılandırıldıktan sonra "federe olmayan" (veya "yönetilen") olarak geri dönebilirsiniz, ancak bu değişikliğin tamamlanması iki saat kadar sürer ve her kullanıcıya bulut tabanlı oturum açma için yeni rasgele parolalar atanması nı gerektirir. Ayarlarınızdaki bir hatayı sıfırlamak için bazı senaryolarda "yönetilen"e geri dönmek gerekebilir. Etki Alanı dönüşümü hakkında [https://msdn.microsoft.com/library/windowsazure/dn194122.aspx](https://msdn.microsoft.com/library/windowsazure/dn194122.aspx)daha fazla bilgi için bkz: .

## <a name="provision-user-principals-to-azure-ad--office-365"></a>Azure AD / Office 365'e kullanıcı ilkelerini sağlama
Kullanıcılarınızın Office 365'e kimliğini doğrulamadan önce, Azure AD'yi SAML 2.0 iddiasındaki iddiaya karşılık gelen kullanıcı ilkeleriyle birlikte sağlamanız gerekir. Bu kullanıcı ilkeleri Azure AD tarafından önceden bilinmiyorsa, federe oturum açma için kullanılamaz. Azure AD Connect veya Windows PowerShell kullanıcı ilkelerini sağlamak için kullanılabilir.

Azure AD Connect, şirket içi Etkin Dizini'nden Azure REKLAM Dizininizde etki alanlarınıza ilkeler sağlamak için kullanılabilir. Daha ayrıntılı bilgi için bkz. Şirket [içi dizinlerinizi Azure Etkin Dizini ile tümleştirin.](whatis-hybrid-identity.md)

Windows PowerShell, Azure AD'ye yeni kullanıcılar eklemeyi otomatikleştirmek ve şirket içi dizinden değişiklikleri eşitlemek için de kullanılabilir. Windows PowerShell cmdlets kullanmak için [Azure Active Directory Modüllerini](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0)indirmeniz gerekir.

Bu yordam, Azure AD'ye tek bir kullanıcının nasıl ekleyeceğini gösterir.


1. Kiracı yönetici olarak Azure REKLAM Dizininize bağlanın: Connect-MsolService.
2.  Yeni bir kullanıcı ilkesi oluşturun:
    ```powershell
    New-MsolUser
      -UserPrincipalName elwoodf1@contoso.com
      -ImmutableId ABCDEFG1234567890
      -DisplayName "Elwood Folk"
      -FirstName Elwood 
      -LastName Folk 
      -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
      -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
      -UsageLocation "US" 
    ```

"New-MsolUser" ödeme hakkında daha fazla bilgi için,[https://technet.microsoft.com/library/dn194096.aspx](https://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>"UserPrinciplName" değeri SAML 2.0 iddianızda "IDPEmail" için göndereceğiniz değerle eşleşmeli ve "ImmutableID" değeri "NameID" iddianızda gönderilen değerle eşleşmelidir.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>SAML 2.0 IDP'nizle tek oturum açma doğrulayın
Yönetici olarak, tek oturum açma (kimlik federasyonu olarak da adlandırılır) doğrulamadan ve yönetmeden önce, saml 2.0 SP-Lite tabanlı kimlik sağlayıcınızla tek oturum açma ayarlamak için bilgileri gözden geçirin ve aşağıdaki makalelerdeki adımları gerçekleştirin:


1.  Azure AD SAML 2.0 Protokol Gereksinimlerini gözden geçirdiniz
2.  SAML 2.0 kimlik sağlayıcınızı yapılandırıldınız
3.  SAML 2.0 kimlik sağlayıcısıyla tek oturum açma için Windows PowerShell'i yükleme
4.  SAML 2.0 kimlik sağlayıcısı ile Azure AD arasında güven ayarlama
5.  Windows PowerShell veya Azure AD Connect aracılığıyla Azure Active Directory (Office 365) için bilinen bir test kullanıcı yöneticisi sağlanmaktadır.
6.  [Azure AD Connect'i](whatis-hybrid-identity.md)kullanarak dizin eşitlemesi yapılandırın.

SAML 2.0 SP-Lite tabanlı kimlik Sağlayıcınızla tek oturum açma yaptıktan sonra, doğru çalıştığını doğrulamanız gerekir.

>[!NOTE]
>Bir etki alanını eklemek yerine dönüştürdüyseniz, tek oturum açma ayarlamak 24 saat kadar sürebilir.
Tek oturum açmayı doğrulamadan önce Active Directory eşitlemeyi ayarlamayı tamamlamalı, dizinlerinizi eşitlemeli ve senkronize edilmiş kullanıcılarınızı etkinleştirmelisiniz.

### <a name="use-the-tool-to-verify-that-single-sign-on-has-been-set-up-correctly"></a>Tek oturum açmanın doğru ayarlandığını doğrulamak için aracı kullanın
Tek oturum açmaişleminin doğru ayarlandığını doğrulamak için, kurumsal kimlik bilgilerinizle bulut hizmetinde oturum açabileceğinizi doğrulamak için aşağıdaki yordamı gerçekleştirebilirsiniz.

Microsoft, SAML 2.0 tabanlı kimlik sağlayıcınızı sınamak için kullanabileceğiniz bir araç sağlamıştır. Test aracını çalıştırmadan önce, bir Azure AD kiracısını kimlik sağlayıcınızla federate yapacak şekilde yapılandırmış olmalısınız.

>[!NOTE]
>Bağlantı Çözümleyicisi, Internet Explorer 10 veya daha sonra gerektirir.



1. Bağlantı Çözümleyicisini buradan [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client)indirin.
2.  Aracı indirmeye ve yüklemeye başlamak için Şimdi Yükle'yi tıklatın.
3.  "Office 365, Azure veya Azure Etkin Dizinkullanan diğer hizmetlerle federasyon kuramıyorum" seçeneğini belirleyin.
4.  Araç indirilip çalıştırıldıktan sonra Bağlantı Tanılama penceresini görürsünüz. Araç, federasyon bağlantınızı test etmede size aşamalı olarak gelecektir.
5.  Bağlantı Çözümleyicisi, oturum açmanız için SAML 2.0 IDP'nizi açacak, test ettiğiniz ![kullanıcı ilkesinin kimlik bilgilerini girecek: SAML](./media/how-to-connect-fed-saml-idp/saml1.png)
6.  Federasyon test oturum açma penceresinde, SamL 2.0 kimlik sağlayıcınızla birlikte federated olarak yapılandırılan Azure AD kiracısı için bir hesap adı ve parola girmeniz gerekir. Araç bu kimlik bilgilerini kullanarak oturum açmagirişiminde bulunacak ve oturum açma girişimi sırasında gerçekleştirilen testlerin ayrıntılı sonuçları çıktı olarak sağlanacaktır.
![SAML](./media/how-to-connect-fed-saml-idp/saml2.png)
7. Bu pencere, sınama nın başarısız bir sonucunu gösterir. Gözden Geçir'e tıkladığınızda, gerçekleştirilen her testin sonuçları hakkında bilgi gösterin. Ayrıca, sonuçları paylaşmak için diske kaydedebilirsiniz.
 
>[!NOTE]
>Bağlantı çözümleyicisi ayrıca WS* tabanlı ve ECP/PAOS protokollerini kullanarak Active Federation'ı da test eder. Bunları kullanmıyorsanız aşağıdaki hatayı göz ardı edebilirsiniz: Kimlik sağlayıcınızın Etkin federasyon bitiş noktasını kullanarak Etkin oturum açma akışını sınamak.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Tek oturum açmanın doğru ayarlandığını el ile doğrulayın
El ile doğrulama, SAML 2.0 kimlik Sağlayıcınızın birçok senaryoda düzgün çalıştığından emin olmak için atabileceğiniz ek adımlar sağlar.
Tek oturum açmanın doğru şekilde ayarlandığını doğrulamak için aşağıdaki adımları tamamlayın:


1. Etki alanına bağlı bir bilgisayarda, kurumsal kimlik bilgileriniz için kullandığınız oturum açma adını kullanarak bulut hizmetinizde oturum açın.
2.  Parola kutusunun içini tıklatın. Tek oturum açma ayarlanırsa, parola kutusu gölgelenir ve aşağıdaki iletiyi görürsünüz: "Artık şirketinizde &lt;&gt;oturum açmanız gerekir."
3.  Şirket &lt;&gt; bağlantınızdan Oturum Aç'ı tıklayın. Oturum açabiliyorsanız, tek oturum açma ayarlandı.

## <a name="next-steps"></a>Sonraki Adımlar


- [Azure AD Connect ile Etkin Dizin Federasyonu Hizmetleri yönetimi ve özelleştirme](how-to-connect-fed-management.md)
- [Azure AD federasyonu uyumluluk listesi](how-to-connect-fed-compatibility.md)
- [Azure AD Connect Özel Kurulum](how-to-connect-install-custom.md)
