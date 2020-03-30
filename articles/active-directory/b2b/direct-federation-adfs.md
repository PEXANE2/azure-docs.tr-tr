---
title: B2B için AD FS ile doğrudan federasyon ayarlama - Azure AD
description: Konukların Azure REKLAM uygulamalarınızda oturum açabilmesi için doğrudan federasyon için ad FS'yi kimlik sağlayıcısı olarak nasıl ayarlayacağınızı öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e350d6338b6ca589ab18d068ef6a314363fe205c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272839"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Örnek: Active Directory Federation Services (AD FS) ile doğrudan federasyon (önizleme)
|     |
| --- |
| Doğrudan federasyon, Azure Etkin Dizini'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.|
|     |

Bu makalede, Aktif Dizin Federasyon Hizmetleri (AD FS) kullanarak [doğrudan federasyon](direct-federation.md) nasıl kurulacağı bir SAML 2.0 veya WS-Fed kimlik sağlayıcısı olarak açıklanmaktadır. Doğrudan federasyonu desteklemek için, belirli özniteliklerin ve taleplerin kimlik sağlayıcısında yapılandırılması gerekir. Doğrudan federasyon için bir kimlik sağlayıcısının nasıl yapılandırılabildiğini göstermek için Active Directory Federation Services'ı (AD FS) örnek olarak kullanırız. AD FS'nin hem SAML kimlik sağlayıcısı hem de WS-Fed kimlik sağlayıcısı olarak nasıl kurulacağını göstereceğiz.

> [!NOTE]
> Bu makalede, illüstrasyon amacıyla hem SAML hem de WS-Fed için AD FS'nin nasıl kurulacağa yönelik tir. Kimlik sağlayıcısının AD FS olduğu doğrudan federasyon entegrasyonları için protokol olarak WS-Fed'i kullanmanızı öneririz. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>SAML 2.0 doğrudan federasyon için AD FS yapılandırma
Azure AD B2B, saml protokolünü kullanan kimlik sağlayıcılarıyla, aşağıda listelenen belirli gereksinimlerle federalete olacak şekilde yapılandırılabilir. SAML yapılandırma adımlarını göstermek için bu bölüm, SAML 2.0 için AD FS'nin nasıl ayarlanını gösterir. 

Doğrudan federasyon ayarlamak için, kimlik sağlayıcısından SAML 2.0 yanıtında aşağıdaki özniteliklerin alınması gerekir. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir. Bir test [AD FS örneği oluşturmada](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) 12 adım, AD FS uç noktalarını nasıl `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`bulacağınızı veya örneğin meta veri URL'nizi nasıl oluşturacağınızı açıklar. 

|Öznitelik  |Değer  |
|---------|---------|
|İddiaTüketiciServisi     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |Örneğin, ortak IdP'nin ihraççıURI'si`http://www.example.com/exk10l6w90DHM0yi...`         |

Aşağıdaki taleplerin kimlik sağlayıcısı tarafından verilen SAML 2.0 belirtecinde yapılandırılması gerekir:


|Öznitelik  |Değer  |
|---------|---------|
|NameID Biçimi     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|Emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


Sonraki bölümde, AD FS kullanılarak bir SAML 2.0 kimlik sağlayıcısına örnek olarak gerekli özniteliklerin ve taleplerin nasıl yapılandırılabildiğiniz gösterin.

### <a name="before-you-begin"></a>Başlamadan önce

Bu yordamı başlatmadan önce bir AD FS sunucusunun zaten ayarlanması ve çalışması gerekir. BIR AD FS sunucusu ayarlama yla ilgili yardım için [bkz.](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed)

### <a name="add-the-claim-description"></a>Talep açıklamasını ekleme

1. AD FS sunucunuzda **Araçlar** > **AD FS yönetimini**seçin.
2. Gezinti **bölmesinde, Hizmet** > **Talebi Açıklamaları'nı**seçin.
3. **Eylemler**altında, **Talep Açıklaması Ekle'yi**seçin.
4. Talep **Açıklaması Ekle** penceresinde aşağıdaki değerleri belirtin:

   - **Ekran Adı**: Kalıcı Tanımlayıcı
   - **İddia tanımlayıcısı**:`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - **Bu federasyon hizmetinin kabul edebileceği bir talep türü olarak federasyon meta verilerinde bu talep açıklamasını yayımla**için onay kutusunu seçin.
   - **Bu federasyon hizmetinin gönderebileceği bir talep türü olarak federasyon meta verilerinde bu talep açıklamasını yayımla**için onay kutusunu seçin.

5. **Tamam**'a tıklayın.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>Güvenilen taraf güven ve talep kurallarını ekleyin

1. AD FS sunucusunda, **Tools** > **AD FS yönetimine**gidin.
2. Gezinti bölmesinde,**Bağlı Parti Güvenlerine Güven** **İlişkileri'ni** > seçin.
3. **Eylemler**altında, **Bağlı Parti Güveni Ekle'yi**seçin. 
4. **Select Data Source**için güvenen taraf güven sihirbazında, çevrimiçi veya yerel bir **ağda yayınlanan güvenilen taraf la ilgili verileri alma**seçeneğini kullanın. Bu federasyon meta veri https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xmlURL' si belirtin. Diğer varsayılan seçimleri bırakın. **Kapat**'ı seçin.
5. **Talep Kurallarını Edit** sihirbazı açılır.
6. Talep **Kurallarını Edit** sihirbazında **Kural Ekle'yi**seçin. **Kural Türünü Seç'te** **LDAP Özniteliklerini Talep Olarak Gönder'i**seçin. **Sonraki'ni**seçin.
7. **Yapılandırma Talep Kuralı'nda**aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta talebi kuralı 
   - **Öznitelik deposu**: Active Directory 
   - **LDAP Özniteliği**: E-Posta Adresleri 
   - **Giden Talep Türü**: E-Posta Adresi

8. **Bitiş'i**seçin.
9. **Talep Kurallarını Edit** penceresi yeni kuralı gösterir. **Uygula**’ya tıklayın. 
10. **Tamam**'a tıklayın.  

### <a name="create-an-email-transform-rule"></a>E-posta dönüştürme kuralı oluşturma
1. Talep **Kurallarını Edit'e** gidin ve **Kural Ekle'yi**tıklatın. **Kural Türünü Seç'te,** Gelen Talebi **Dönüştür'i** seçin ve **İleri'yi**tıklatın. 
2. **Yapılandırma Talep Kuralı'nda**aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta dönüştürme kuralı 
   - **Gelen talep türü**: E-posta Adresi 
   - **Giden talep türü**: Ad Kimliği 
   - **Giden ad kimliği biçimi**: Kalıcı Tanımlayıcı 
   - **Tüm talep değerlerini geçir**’i seçin.

3. **Son**'a tıklayın. 
4. **Talep Kurallarını Edit** penceresi yeni kuralları gösterir. **Uygula**’ya tıklayın. 
5. **Tamam**'a tıklayın. AD FS sunucusu artık SAML 2.0 protokolü kullanılarak doğrudan federasyon için yapılandırılmıştır.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>WS-Fed doğrudan federasyonu için AD FS yapılandırın 
Azure AD B2B, WS-Fed protokolünü kullanan kimlik sağlayıcılarıyla, aşağıda listelenen belirli gereksinimlerle federate yapacak şekilde yapılandırılabilir. Şu anda, iki WS-Fed sağlayıcısı Azure AD ile uyumluluk açısından test edilmiştir AD FS ve Shibboleth içerir. Burada, Active Directory Federation Services (AD FS) adresini WS-Fed kimlik sağlayıcısına örnek olarak kullanacağız. Azure AD ile WS-Fed uyumlu bir sağlayıcı arasında güvenesahip bir taraf güveni oluşturma hakkında daha fazla bilgi için Azure AD Kimlik Sağlayıcısı Uyumluluk Dokümanları'nı indirin.

Doğrudan federasyon kurmak için, kimlik sağlayıcısından gelen WS-Fed iletisinde aşağıdaki özniteliklerin alınması gerekir. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir. Bir test [AD FS örneği oluşturmada](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) 12 adım, AD FS uç noktalarını nasıl `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`bulacağınızı veya örneğin meta veri URL'nizi nasıl oluşturacağınızı açıklar.
 
|Öznitelik  |Değer  |
|---------|---------|
|PasifİstekorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |Örneğin, ortak IdP'nin ihraççıURI'si`http://www.example.com/exk10l6w90DHM0yi...`         |

IdP tarafından yayınlanan WS-Fed jetonu için gerekli talepler:

|Öznitelik  |Değer  |
|---------|---------|
|Değişmez ID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|Emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

Bir sonraki bölümde, AD FS kullanılarak bir WS-Fed kimlik sağlayıcısına örnek olarak gerekli özniteliklerin ve taleplerin nasıl yapılandırılabildiğiniz gösterin.

### <a name="before-you-begin"></a>Başlamadan önce
Bu yordamı başlatmadan önce bir AD FS sunucusunun zaten ayarlanması ve çalışması gerekir. BIR AD FS sunucusu ayarlama yla ilgili yardım için [bkz.](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed)


### <a name="add-the-relying-party-trust-and-claim-rules"></a>Güvenilen taraf güven ve talep kurallarını ekleyin 
1. AD FS sunucusunda, **Tools** > **AD FS yönetimine**gidin. 
1. Gezinti bölmesinde,**Bağlı Parti Güvenlerine Güven** **İlişkileri'ni** > seçin. 
1. **Eylemler**altında, **Bağlı Parti Güveni Ekle'yi**seçin.  
1. Veri **Kaynağı Seç**için, güvenerek parti güven sihirbazı ekle, **çevrimiçi veya yerel bir ağda yayınlanan güvenilen taraf hakkında veri alma**seçeneğini kullanın. Bu federasyon meta veri `https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml`URL'si belirtin: .  Diğer varsayılan seçimleri bırakın. **Kapat**'ı seçin.
1. **Talep Kurallarını Edit** sihirbazı açılır. 
1. Talep **Kurallarını Edit** sihirbazında **Kural Ekle'yi**seçin. **Kural Türünü Seç'te,** Özel Kural Kullanarak Talep **Gönder'i**seçin. *Sonraki'ni*seçin. 
1. **Yapılandırma Talep Kuralı'nda**aşağıdaki değerleri belirtin:

   - **Talep kuralı adı**: Sorun Değişmez Kimlik  
   - **Özel kural:**`c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. **Bitiş'i**seçin. 
1. **Talep Kurallarını Edit** penceresi yeni kuralı gösterir. **Uygula**’ya tıklayın.  
1. Aynı **Edit Claim Rules** sihirbazında **Kural Ekle'yi**seçin. **Cohose Kural**Türü'nde, **LDAP Özniteliklerini Talep Olarak Gönder'i**seçin. **Sonraki'ni**seçin.
1. **Yapılandırma Talep Kuralı'nda**aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: E-posta talebi kuralı  
   - **Öznitelik deposu**: Active Directory  
   - **LDAP Özniteliği**: E-Posta Adresleri  
   - **Giden Talep Türü**: E-Posta Adresi 

1.  **Bitiş'i**seçin. 
1.  **Talep Kurallarını Edit** penceresi yeni kuralı gösterir. **Uygula**’ya tıklayın.  
1.  **Tamam**'a tıklayın. AD FS sunucusu artık WS-Fed kullanılarak doğrudan federasyon için yapılandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar
Ardından, Azure AD portalında veya PowerShell'i kullanarak [Azure AD'de doğrudan federasyonu yapılandıracaksınız.](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) 
