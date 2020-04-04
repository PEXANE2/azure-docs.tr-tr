---
title: Azure AD Connect ve Connect Health nedir? | Microsoft Belgeleri
description: Şirket içi ortamınızı Azure AD ile senkronize etmek ve izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 01/08/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e60c35a32152d4adec72fb507becc0db535036f
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631718"
---
# <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?

Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır.  Aşağıdaki özellikleri sağlar:
     
- [Parola karma eşitleme](whatis-phs.md) - Azure AD ile şirket içinde reklam parolası kullanan bir kullanıcının karmasını senkronize eden oturum açma yöntemi.
- [Geçiş kimlik doğrulaması](how-to-connect-pta.md) - Kullanıcıların aynı parolayı şirket içinde ve bulutta kullanmasına olanak tanıyan, ancak federe ortamın ek altyapısını gerektirmeyen oturum açma yöntemi.
- [Federasyon tümleştirmesi](how-to-connect-fed-whatis.md) - Federasyon Azure AD Connect'in isteğe bağlı bir parçasıdır ve şirket içi AD FS altyapısını kullanarak karma ortamı yapılandırmak için kullanılabilir. Ayrıca, sertifika yenileme ve ek AD FS sunucu dağıtımları gibi AD FS yönetim özellikleri de sağlar.
- [Eşitleme](how-to-connect-sync-whatis.md) - Kullanıcıları, grupları ve diğer nesneleri oluşturmaktan sorumludur.  Ayrıca, şirket içi kullanıcılarınız ve gruplarınız için kimlik bilgilerinin bulutla eşleştirdiğinden emin olun.  Bu eşitleme, parola işlemesi de içerir.
- [Sistem Durumu İzleme](whatis-hybrid-identity-health.md) - Azure AD Connect Health sağlam izleme sağlayabilir ve bu etkinliği görüntülemek için Azure portalında merkezi bir konum sağlayabilir. 


![Azure AD Connect nedir?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Azure AD Connect Health nedir?

Azure Active Directory (Azure AD) Connect Health, şirket içi kimlik altyapınızın güçlü bir şekilde izlenmesini sağlar. Office 365 ve Microsoft Çevrimiçi Hizmetler ile güvenilir bir bağlantı sağlamanızı sağlar.  Bu güvenilirlik, temel kimlik bileşenleriniz için izleme özellikleri sağlayarak elde edilir. Ayrıca, bu bileşenlerle ilgili önemli veri noktalarına kolayca erişilebilir hale getirir.

Bilgiler, [Azure AD Connect Health Portalı](https://aka.ms/aadconnecthealth)'nda sunulur. Uyarıları, performans izlemeyi, kullanım analitiğini ve diğer bilgileri görüntülemek için Azure AD Connect Health portalını kullanın. Azure AD Connect Health, anahtar kimlik bileşenleriniz için aynı yerde tek sistem durumu odağı sağlar.

![Azure AD Connect Health nedir?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)

## <a name="why-use-azure-ad-connect"></a>Azure AD Connect neden kullanılır?
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Kullanıcılar ve kuruluşlar şu avantajlardan yararlanabilir:

* Kullanıcılar, Office 365 gibi bulut hizmetlerine ve şirket içi uygulamalara erişmek için tek bir kimlik kullanabilir.
* Eşitleme ve oturum açmaya yönelik kolay bir dağıtım deneyimi sağlamak için tek araç.
* Senaryolarınız için en yeni işlevleri sağlar. Azure AD Connect, DirSync ve Azure AD Eşitleme gibi kimlik tümleştirme araçlarının eski sürümlerinin yerini alır. Daha fazla bilgi için, [Karma Kimlik dizin tümleştirme araçları karşılaştırmasına](plan-hybrid-identity-design-considerations-tools-comparison.md)bakın.

## <a name="why-use-azure-ad-connect-health"></a>Azure AD Connect Health neden kullanılır?
Azure AD ile yken, hem bulut hem de şirket içi kaynaklara erişmek için ortak bir kimlik olduğundan kullanıcılarınız daha üretken olur. Kullanıcıların bu kaynaklara erişebilmeleri için ortamın güvenilir olmasını sağlamak zor bir hal alır.  Azure AD Connect Health, şirket içi kimlik altyapınızı izlemenize ve öngörüler elde ederek bu ortamın güvenilirliğini sağlamaya yardımcı olur. Her bir şirket içi kimlik sunucunuza bir aracı yüklemek kadar basittir.

AD FS'ye ilişkin Azure AD Connect Health Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 üzerinde AD FS 2.0'ı destekler. Ayrıca, extranet erişimi için kimlik doğrulaması desteği sağlayan AD FS ve web uygulaması ara sunucularının izlenmesini de destekler. AD FS için Azure AD Connect Health, çabucak ve kolayca kurulan Sistem Durumu Aracısı ile bir dizi başlıca özellik sunar.

Temel avantajlar ve en iyi uygulamalar:

|Önemli Avantajlar|En İyi Uygulamalar|
|-----|-----|
|Geliştirilmiş güvenlik|[Extranet kilitlemede trendler](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)</br>[Başarısız oturum açma raporu](how-to-connect-health-adfs-risky-ip.md)</br>[Gizlilik uyumlu](reference-connect-health-user-privacy.md)|
|Tüm kritik [ADFS sistem sorunları](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services) hakkında uyarı alın|Sunucu yapılandırması ve kullanılabilirlik</br>[Performans ve bağlantı](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs)</br>Düzenli bakım|
|Kurulumu ve yönetimi kolay|[Hızlı aracı yükleme](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)</br>Aracıyı en sün sürüme otomatik yükseltme</br>Dakikalar içinde portalda veri|
Zengin [kullanım ölçümleri](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)|En çok kullanılan uygulamalar</br>Ağ konumları ve TCP bağlantısı</br>Sunucu başına belirteç istekleri|
|Harika kullanıcı deneyimi|Azure portaldan bir pano tarzında</br>[E-postayla uyarılar](how-to-connect-health-adfs.md#alerts-for-ad-fs)|


## <a name="license-requirements-for-using-azure-ad-connect"></a>Azure AD Connect'i kullanmak için lisans gereksinimleri

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="license-requirements-for-using-azure-ad-connect-health"></a>Azure AD Connect Health'i kullanmak için lisans gereksinimleri
[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Ekspres ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Azure AD Connect Health aracılarını yükleme](how-to-connect-health-agent-install.md) 
