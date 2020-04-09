---
title: 'Azure AD Uygulama Proxy: Sürüm sürüm geçmişi | Microsoft Dokümanlar'
description: Bu makalede, Azure AD Uygulama Proxy'sinin tüm sürümleri listelenir ve yeni özellikler ve düzeltilen sorunlar açıklanır
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2020
ms.subservice: app-mgmt
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: f027fbce66a73306165a0ad35d1ba3faa7a5c0bc
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983900"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure AD Uygulama Proxy: Sürüm sürüm geçmişi
Bu makalede, Azure Etkin Dizin (Azure AD) Uygulama Proxy'sinin yayımlanmış sürümleri ve özellikleri listelenmiştir. Azure AD ekibi, Uygulama Proxy'sini düzenli olarak yeni özellikler ve işlevlerle güncelleştirir. Uygulama Proxy bağlayıcıları yeni bir sürüm yayımlandığında otomatik olarak güncelleştirilir. 

En son özelliklere ve hata düzeltmelerine sahip olduğundan emin olmak için bağlayıcılarınızın otomatik güncelleştirmelerinin etkin olduğundan emin olmamızı öneririz. Microsoft, en son bağlayıcı sürümü ve daha önce bir sürüm için doğrudan destek sağlar.

İlgili kaynakların listesi aşağıda veda edileb:

Kaynak |  Ayrıntılar
--------- | --------- |
Uygulama Proxy nasıl etkinleştirilir | Uygulama Proxy etkinleştirmek ve yükleme ve bir bağlayıcı kayıt için ön koşullar bu [öğreticide](application-proxy-add-on-premises-application.md)açıklanmıştır.
Azure AD Uygulama Proxy bağlayıcılarını anlama | [Konektör yönetimi](application-proxy-connectors.md) ve bağlayıcıların [otomatik yükseltme](application-proxy-connectors.md#automatic-updates)si hakkında daha fazla bilgi edinin.
Azure AD Uygulama Proxy Bağlayıcı İndir |  [En son konektörü indirin.](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download)

## <a name="1515260"></a>1.5.1526.0

### <a name="release-status"></a>Yayın durumu

Nisan 07, 2020: İndirme için yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
-   Bağlayıcılar tüm bağlantılar için yalnızca TLS 1.2 kullanır. Daha fazla ayrıntı için [Bağlayıcı ön koşulları](application-proxy-add-on-premises-application.md#before-you-begin) görün.
- Bağlayıcı ve Azure hizmetleri arasında gelişmiş sinyal. Buna, Bağlayıcı ve Azure hizmetleri arasındaki WCF iletişimi için güvenilir oturumların desteklenmesi ve WebSocket iletişimleri için DNS önbelleğe alma geliştirmeleri dahildir.
- Bağlayıcı ve arka uç uygulaması arasında proxy yapılandırma desteği. Daha fazla bilgi için bkz: [Mevcut şirket içi proxy sunucularıyla çalışma.](application-proxy-configure-connectors-with-proxy-servers.md)

### <a name="fixed-issues"></a>Düzeltilen sorunlar
- Bağlayıcıdan Azure hizmetlerine iletişim için bağlantı noktası 8080'e geri dalma kaldırıldı.
- WebSocket iletişimi için hata ayıklama izlemeleri eklendi. 
- Arka uç uygulama tanımlama bilgilerinde ayarlandığında SameSite özniteliğinin korunması çözüldü.

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Yayın durumu

20 Eylül 2018: İndirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- QlikSense uygulaması için WebSocket desteği eklendi. QlikSense'i Uygulama Proxy'si ile nasıl tümleştirdiğiniz hakkında daha fazla bilgi edinmek için bu [izbise](application-proxy-qlik.md)bakın. 
- Giden proxy'yi yapılandırmayı kolaylaştırmak için yükleme sihirbazı geliştirildi. 
- TLS 1.2'yi bağlayıcılar için varsayılan protokol olarak ayarlayın. 
- Yeni bir Son Kullanıcı Lisans Sözleşmesi (EULA) eklendi.  

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- Konektörde bazı bellek sızıntıları neden bir hata düzeltildi.
- Bağlayıcı zaman aşım sorunları için hata düzeltmesi içeren Azure Hizmet Veri Servisi sürümünü güncelleştirin.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Yayın durumu

19 Ocak 2018: İndirme için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- Çerezde etki alanı çevirisi gerektiren özel etki alanları için destek eklendi.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Yayın durumu 

Mayıs 25, 2017: İndiriçin yayımlandı 

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 

Konektörlerin giden bağlantı sınırları üzerinde geliştirilmiş denetim. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Yayın durumu

15 Nisan 2017: İndirme için yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- Daha az gerekli bağlantı noktası yla basitleştirilmiş onboarding ve yönetimi. Uygulama Proxy şimdi sadece iki standart giden bağlantı noktası: 443 ve 80 açılması gerektirir. Uygulama Proxy yalnızca giden bağlantıları kullanmaya devam eder, bu nedenle hala bir DMZ herhangi bir bileşen gerekmez. Ayrıntılar için [yapılandırma belgelerimize](application-proxy-add-on-premises-application.md)bakın.  
- Harici proxy veya güvenlik duvarınız tarafından desteklenirse, artık ağınızı IP aralığı yerine DNS ile açabilirsiniz. Uygulama Proxy hizmetleri yalnızca *.msappproxy.net ve *.servicebus.windows.net bağlantıları gerektirir.


## <a name="earlier-versions"></a>Önceki sürümler

Uygulama Proxy bağlayıcı sürümünü 1.5.36.0'dan önce kullanıyorsanız, tam olarak desteklenen en son özelliklere sahip olduğunuzdan emin olmak için en son sürüme güncelleyin.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Application Proxy aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)hakkında daha fazla bilgi edinin.
- Uygulama Proxy'sini kullanmaya başlamak [için Bkz. Öğretici: Uygulama Proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](application-proxy-add-on-premises-application.md)
