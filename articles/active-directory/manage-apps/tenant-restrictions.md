---
title: SaaS uygulamalarına erişimi yönetmek için kiracı kısıtlamalarını kullanın - Azure AD
description: Azure AD kiracılarına göre hangi kullanıcıların uygulamalara erişebileceğini yönetmek için kiracı kısıtlamaları nasıl kullanılır?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: mimart
ms.reviewer: richagi
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecd49b340810f92727f0fc98f84031c8cbf68179
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481186"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>SaaS bulut uygulamalarına erişimi yönetmek için kiracı kısıtlamalarını kullanma

Güvenliği vurgulayan büyük kuruluşlar Office 365 gibi bulut hizmetlerine geçmek ister, ancak kullanıcılarının yalnızca onaylı kaynaklara erişebileceğini bilmek gerekir. Şirketler geleneksel olarak, erişimi yönetmek istediklerinde alan adlarını veya IP adreslerini kısıtlar. Bu yaklaşım, hizmet olarak yazılım (veya SaaS) uygulamalarının genel bir bulutta barındırıldığı ve [outlook.office.com](https://outlook.office.com/) ve [login.microsoftonline.com](https://login.microsoftonline.com/)gibi paylaşılan alan adlarında çalıştığı bir dünyada başarısız olur. Bu adresleri engellemek, kullanıcıların yalnızca onaylanmış kimlikler ve kaynaklarla sınırlamak yerine Outlook'a tamamen erişmelerini engeller.

Azure Etkin Dizin (Azure AD) çözümü, kiracı kısıtlamaları adı verilen bir özelliktir. Kiracı kısıtlamalarıyla kuruluşlar, uygulamaların tek oturum açma için kullandığı Azure AD kiracısını temel alarak SaaS bulut uygulamalarına erişimi denetleyebilir. Örneğin, kuruluşunuzun Office 365 uygulamalarına erişime izin verirken, diğer kuruluşların bu aynı uygulamalara yönelik örneklerine erişimi engellemek isteyebilirsiniz.  

Kiracı kısıtlamalarıyla, kuruluşlar kullanıcılarının erişmelerine izin verilen kiracılar listesini belirtebilir. Azure AD, yalnızca bu izin verilen kiracılara erişim izni verir.

Bu makalede, Office 365 için kiracı kısıtlamaları üzerinde duruluyor, ancak özellik tek oturum açma için Azure AD ile modern kimlik doğrulama protokolleri kullanan herhangi bir SaaS bulut uygulaması ile çalışması gerekir. Office 365 tarafından kullanılan kiracıdan farklı bir Azure AD kiracısı olan SaaS uygulamalarını kullanıyorsanız, gerekli tüm kiracılara izin verildiğinden emin olun. SaaS bulut uygulamaları hakkında daha fazla bilgi için [Active Directory Marketplace'e](https://azure.microsoft.com/marketplace/active-directory/)bakın.

## <a name="how-it-works"></a>Nasıl çalışır?

Genel çözüm aşağıdaki bileşenleri içerir:

1. **Azure AD**: `Restrict-Access-To-Tenants: <permitted tenant list>` Azure AD yalnızca izin verilen kiracılar için güvenlik belirteçleri yayınlar.

2. **Şirket içi proxy sunucu altyapısı**: Bu altyapı, Taşıma Katmanı Güvenliği (TLS) denetimi yapabilen bir proxy cihazıdır. Azure AD için ayrılan trafiğe izin verilen kiracıların listesini içeren üstbilgi eklemek için proxy'yi yapılandırmanız gerekir.

3. **İstemci yazılımı**: Kiracı kısıtlamalarını desteklemek için, proxy altyapısının trafiği engelleyebilmeleri için istemci yazılımının doğrudan Azure AD'den belirteçler istemesi gerekir. Tarayıcı tabanlı Office 365 uygulamaları, modern kimlik doğrulaması kullanan Office istemcileri (OAuth 2.0 gibi) gibi şu anda kiracı kısıtlamalarını desteklemede bulunur.

4. **Modern Kimlik Doğrulama**: Bulut hizmetleri, kiracı kısıtlamalarını kullanmak ve izin verilmeyen tüm kiracılara erişimi engellemek için modern kimlik doğrulamayı kullanmalıdır. Varsayılan olarak modern kimlik doğrulama protokollerini kullanmak için Office 365 bulut hizmetlerini yapılandırmanız gerekir. Modern kimlik doğrulama için Office 365 desteği hakkında en son bilgiler için [Güncelleştirilmiş Office 365 modern kimlik doğrulaması'nı](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)okuyun.

Aşağıdaki diyagram, üst düzey trafik akışını göstermektedir. Kiracı kısıtlamaları, Office 365 bulut hizmetlerine değil, yalnızca Azure AD trafiğinde TLS denetimi gerektirir. Azure AD kimlik doğrulaması için trafik hacmi genellikle Exchange Online ve SharePoint Online gibi SaaS uygulamalarına yönelik trafik hacminden çok daha düşük olduğundan, bu ayrım önemlidir.

![Kiracı kısıtlamaları trafik akışı - diyagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Kiracı kısıtlamalarını ayarlama

Kiracı kısıtlamalarıile başlamak için iki adım vardır. İlk olarak, müşterilerinizin doğru adreslere bağlanabilmesini unutmayın. İkinci olarak, proxy altyapınızı yapılandırın.

### <a name="urls-and-ip-addresses"></a>URL'ler ve IP adresleri

Kiracı kısıtlamalarını kullanmak için, müşterilerinizin kimlik doğrulaması için aşağıdaki Azure REKLAM URL'lerine bağlanabilmesi gerekir: [login.microsoftonline.com,](https://login.microsoftonline.com/) [login.microsoft.com](https://login.microsoft.com/)ve [login.windows.net.](https://login.windows.net/) Ayrıca, Office 365'e erişmek için müşterilerinizin [Office 365 URL'leri ve IP adres aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)tanımlanan tam nitelikli alan adları (FQDN'ler), URL'ler ve IP adreslerine de bağlanabilmesi gerekir. 

### <a name="proxy-configuration-and-requirements"></a>Proxy yapılandırması ve gereksinimleri

Proxy altyapınız aracılığıyla kiracı kısıtlamalarını etkinleştirmek için aşağıdaki yapılandırma gereklidir. Bu kılavuz geneldir, bu nedenle belirli uygulama adımları için proxy satıcınızın belgelerine başvurmalısınız.

#### <a name="prerequisites"></a>Ön koşullar

- Proxy, FQDNs/URL'leri kullanarak TLS engelleme, HTTP üstbilgi ekleme ve filtre hedefleri gerçekleştirebilmeli.

- İstemciler TLS iletişimi için proxy tarafından sunulan sertifika zincirine güvenmelidir. Örneğin, bir iç ortak [anahtar altyapısından (PKI)](/windows/desktop/seccertenroll/public-key-infrastructure) sertifikalar kullanılıyorsa, iç kuruluş sertifika yetkilisi sertifikasına güvenilmelidir.

- Bu özellik Office 365 aboneliklerine dahildir, ancak diğer SaaS uygulamalarına erişimi denetlemek için kiracı kısıtlamalarını kullanmak istiyorsanız, Azure AD Premium 1 lisansları gereklidir.

#### <a name="configuration"></a>Yapılandırma

login.microsoftonline.com, login.microsoft.com ve login.windows.net gelen her istek için iki HTTP üstbilgi ekleyin: *Kiracıya Erişimi Kısıtla* ve *Kısıtla-Erişim-Bağlam.*

Üstbilgi aşağıdaki öğeleri içermelidir:

- *Kiracılara Erişimi Kısıtla,* kullanıcıların erişmesine \<izin vermek\>istediğiniz virgülle ayrılmış kiracı listesi olan izin verilen kiracı listesinin değerini kullanın. Kiracıya kayıtlı herhangi bir etki alanı, bu listedeki kiracıyı tanımlamak için kullanılabilir. Örneğin, hem Contoso hem de Fabrikam kiracılarına erişime izin vermek için ad/değer çifti aşağıdaki gibi görünür: `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com`

- *Erişimi Kısıtla-Bağlam*için, kiracının kısıtlamalarını ayarlayan kiracıyı bildiren tek bir dizin kimliği değerini kullanın. Örneğin, Contoso'yu kiracı kısıtlamaları ilkesini ayarlayan kiracı olarak bildirmek için ad/değer çifti aşağıdaki gibi görünür: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Dizin kimliğinizi [Azure Etkin Dizin portalında](https://aad.portal.azure.com/)bulabilirsiniz. Yönetici olarak oturum açın, **Azure Etkin Dizin'i**seçin ve ardından **Özellikler'i**seçin.

Kullanıcıların kendi HTTP üstbilgilerini onaylanmamış kiracılarla eklemelerini önlemek için, proxy'nin gelen istekte zaten mevcutsa *Kiracıya Erişimi Kısıtla* üstbilgisini değiştirmesi gerekir.

İstemciler, login.microsoftonline.com, login.microsoft.com ve login.windows.net tüm istekleri için proxy kullanmak zorunda olmalıdır. Örneğin, PAC dosyaları proxy'yi kullanmak için istemcileri yönlendirmek için kullanılıyorsa, son kullanıcılar PAC dosyalarını değiştirememeli veya devre dışı bırakmamalıdır.

## <a name="the-user-experience"></a>Kullanıcı deneyimi

Bu bölümde hem son kullanıcılar hem de yöneticiler için deneyim açıklanmaktadır.

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

Örnek bir kullanıcı Contoso ağındadır, ancak Outlook çevrimiçi gibi paylaşılan bir SaaS uygulamasının Fabrikam örneğine erişmeye çalışır. Fabrikam, Contoso örneğinin izin verilmeyen kiracısıysa, kullanıcı BT departmanınız tarafından onaylanmayan bir kuruluşa ait bir kaynağa erişmeye çalıştığınızı belirten bir erişim reddi iletisi görür.

### <a name="admin-experience"></a>Yönetici deneyimi

Kurumsal proxy altyapısında kiracı kısıtlamalarının yapılandırması yapılırken, yöneticiler Azure portalındaki kiracı kısıtlama raporlarına doğrudan erişebilir. Raporları görüntülemek için:

1. Azure Active [Directory portalında](https://aad.portal.azure.com/)oturum açın. **Azure Etkin Dizin yönetici merkezi** panosu görüntülenir.

2. Sol bölmede **Azure Active Directory**’yi seçin. Azure Etkin Dizin genel bakış sayfası görüntülenir.

3. Diğer **özellikler** başlığında **Kiracı kısıtlamalarını**seçin.

Kısıtlı Erişim-Bağlam kiracısı olarak belirtilen kiracının yöneticisi, kullanılan kimlik ve hedef dizin kimliği de dahil olmak üzere kiracı kısıtlamaları ilkesi nedeniyle oturum açmaların engellendiğini görmek için bu raporu kullanabilir. Kısıtlamayı ayarlayıp oturum açma nın kullanıcı kiracısı veya oturum açma için kaynak kiracı olması durumunda oturum açma işlemleri dahildir.

> [!NOTE]
> Rapor, Kısıtlı Erişim-Bağlam kiracısı dışında kiracı olan bir kullanıcı nın giriş yaptığında hedef dizin kimliği gibi sınırlı bilgiler içerebilir. Bu durumda, ad ve kullanıcı anaadı gibi kullanıcı tanımlanabilir bilgiler, diğer kiracılardaki kullanıcı verilerini korumak için maskelenir.

Azure portalındaki diğer raporlar gibi, raporunuzun kapsamını belirtmek için filtreleri kullanabilirsiniz. Belirli bir zaman aralığına, kullanıcıya, uygulamaya, istemciye veya duruma filtre uygulayabilirsiniz. **Sütunlar** düğmesini seçerseniz, verileri aşağıdaki alanların herhangi bir bileşimiyle görüntülemeyi seçebilirsiniz:

- **Kullanıcı**
- **Uygulama**
- **Durum**
- **Tarih**
- **Tarih (UTC)** (UTC'nin Evrensel Saatin Eşgüdümlü olduğu yer)
- **MFA Auth Yöntemi** (çok faktörlü kimlik doğrulama yöntemi)
- **MFA Auth Detay** (çok faktörlü kimlik doğrulama ayrıntısı)
- **MFA Sonucu**
- **IP Adresi**
- **Istemci**
- **Username**
- **Konum**
- **Hedef kiracı kimliği**

## <a name="office-365-support"></a>Office 365 desteği

Office 365 uygulamaları, kiracı kısıtlamalarını tam olarak desteklemek için iki ölçüte uygun olmalıdır:

1. Kullanılan istemci modern kimlik doğrulamasını destekler.
2. Bulut hizmeti için varsayılan kimlik doğrulama protokolü olarak modern kimlik doğrulaması etkinleştirilir.

Office istemcilerinin şu anda modern kimlik doğrulamasını desteklediği en son bilgiler için [Güncelleştirilmiş Office 365 modern kimlik doğrulamasına](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) bakın. Bu sayfa ayrıca belirli Exchange Online ve Skype for Business Online kiracılarında modern kimlik doğrulamasını etkinleştirme talimatlarına bağlantılar da içerir. SharePoint Online zaten varsayılan olarak Modern kimlik doğrulaması sağlar.

Office 365 tarayıcı tabanlı uygulamalar (Office Portal, Yammer, SharePoint siteleri, Web'de Outlook ve daha fazlası) şu anda kiracı kısıtlamalarını desteklemede bulunmaktadır. Kalın istemciler (Outlook, Skype for Business, Word, Excel, PowerPoint ve daha fazlası) yalnızca modern kimlik doğrulamayı kullanırken kiracı kısıtlamalarını uygulayabilir.  

Modern kimlik doğrulamasını destekleyen Outlook ve Skype for Business istemcileri, modern kimlik doğrulamanın etkin olmadığı kiracılara karşı eski protokolleri kullanmaya devam edebilir ve kiracı kısıtlamalarını etkin bir şekilde atlayabilir. Kiracı kısıtlamaları, kimlik doğrulama sırasında login.microsoftonline.com, login.microsoft.com veya login.windows.net iletişim kurmaları halinde eski protokolleri kullanan uygulamaları engelleyebilir.

Windows'da Outlook için müşteriler, son kullanıcıların profillerine onaylanmamış posta hesapları eklemesini engelleyen kısıtlamalar uygulamayı seçebilir. Örneğin, varsayılan olmayan Exchange hesapları grup ilkesi ayarını [eklemeyi önleyin.](https://gpsearch.azurewebsites.net/default.aspx?ref=1)

## <a name="testing"></a>Sınama

Tüm kuruluşunuz için uygulamadan önce kiracı kısıtlamalarını denemek istiyorsanız, iki seçeneğiniz vardır: Fiddler gibi bir aracı kullanan ana bilgisayar tabanlı bir yaklaşım veya proxy ayarlarının aşamalı olarak kullanıma sunulması.

### <a name="fiddler-for-a-host-based-approach"></a>Ev sahibi tabanlı bir yaklaşım için Kemancı

Fiddler yakalamak ve HTTP/ HTTPS trafiği değiştirmek için kullanılabilir ücretsiz bir web hata ayıklama proxy, HTTP üstbilgileri ekleme de dahil olmak üzere. Fiddler'ı kiracı kısıtlamalarını sınamak için yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. [Fiddler'ı indirin ve kurun.](https://www.telerik.com/fiddler)

2. [Fiddler'ın yardım belgelerine](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)göre, HTTPS trafiğinin şifresini çözmek için Fiddler'ı yapılandırın.

3. Özel kuralları kullanarak *Bağ-Kur'luları Kiracılara Erişimi Kısıtla* ve *Bağlamı Kısıtla* üstbilgilerini eklemek için Fiddler'ı yapılandırın:

   1. Fiddler Web Hata Ayıklayıcı aracında **Kurallar** menüsünü seçin ve **Kuralları Özelleştir'i seçin...** CustomRules dosyasını açmak için.

   2. `OnBeforeRequest` İşlevin başında aşağıdaki satırları ekleyin. Kiracı \<etki\> alanını kiracınıza kayıtlı bir etki `contoso.onmicrosoft.com`alanıyla değiştirin (örneğin, ). Dizin \<kimliğini\> kiracınızın Azure REKLAM GUID tanımlayıcısıyla değiştirin.

      ```JScript.NET
      if (
          oSession.HostnameIs("login.microsoftonline.com") ||
          oSession.HostnameIs("login.microsoft.com") ||
          oSession.HostnameIs("login.windows.net")
      )
      {
          oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";
          oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";
      }
      ```

      Birden çok kiracıya izin vermeniz gerekiyorsa, kiracı adlarını ayırmak için virgül kullanın. Örnek:

      `oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";`

4. CustomRules dosyasını kaydedin ve kapatın.

Fiddler'ı yapılandırıldıktan sonra **Dosya** menüsüne giderek ve **Trafiği Yaka'yı**seçerek trafiği yakalayabilirsiniz.

### <a name="staged-rollout-of-proxy-settings"></a>Proxy ayarlarının aşamalı kullanıma sunulması

Proxy altyapınızın özelliklerine bağlı olarak, ayarların kullanıcılarınız için kullanıma sunulmasını gerçekleştirebilirsiniz. Burada dikkate alınması gereken bir çift üst düzey seçenek vardır:

1. Normal kullanıcılar üretim proxy altyapısını kullanmaya devam ederken, kullanıcıları test proxy altyapısına işaret etmek için PAC dosyalarını kullanın.
2. Bazı proxy sunucuları grupları kullanarak farklı yapılandırmaları destekleyebilir.

Belirli ayrıntılar için proxy sunucu belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) hakkında bilgi edinin
- Office [365 URL'lerini ve IP adres aralıklarını](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) gözden geçirin
