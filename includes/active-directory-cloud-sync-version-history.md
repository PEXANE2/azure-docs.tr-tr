---
ms.openlocfilehash: 77866fcb72515d5167d9f06b7d9ad91c59bc7945
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97700590"
---
Bu makalede, yayımlanan Azure Active Directory Connect sağlama aracısının sürümleri ve özellikleri listelenmektedir. Azure AD ekibi, sağlama aracısını yeni özellikler ve işlevlerle düzenli olarak güncelleştirir. Yeni bir sürüm yayınlandığında sağlama Aracısı otomatik olarak güncelleştirilir. 

Microsoft, en son aracı sürümü ve bir sürümü için doğrudan destek sağlar.

## <a name="112810"></a>1.1.281.0

### <a name="release-status"></a>Yayın durumu

23 Kasım 2020: indirme için yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* [GMSA](../articles/active-directory/cloud-provisioning/how-to-prerequisites.md#group-managed-service-accounts) desteği
* Artımlı veya Delta eşitleme çevrimi sırasında 1500 ' den az üyeye kadar olan gruplar için destek. Bu, Grup kapsamı filtresi kullanılırken geçerlidir
* En fazla 15K 'ye üye boyutu olan büyük gruplar için destek
* İlk eşitleme geliştirmeleri
* Gelişmiş ayrıntılı günlük kaydı
* [Aadcloudsynctools PowerShell modülünün](../articles/active-directory/cloud-provisioning/reference-powershell.md) tanıtımı
* Ingilizce olmayan sunucuda aracının yüklenmesine izin veren bir kısıtlama düzeltildi
* Yalnızca kapsamdaki nesneler için PHS filtreleme desteği (başlangıçta tüm nesneler için parola karmaları eşitliyoruz)
* Aracıdaki bellek sızıntısı sorunu düzeltildi
* Geliştirilmiş sağlama günlükleri
* [LDAP bağlantısı zaman aşımını](../articles/active-directory/cloud-provisioning/how-to-manage-registry-options.md#configure-ldap-connection-timeout) yapılandırma desteği 
* [Başvuru](../articles/active-directory/cloud-provisioning/how-to-manage-registry-options.md#configure-referral-chasing) oluşturma yapılandırması için destek 


## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Yayın durumu

4 Aralık 2019: indirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Kullanıcıyı eşitlemeye yönelik [Azure AD Connect bulut sağlama](../articles/active-directory/cloud-provisioning/what-is-cloud-provisioning.md) , şirket içi Active Directory Ile Azure AD arasında iletişim kurma ve veri gruplama için destek içerir


## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Yayın durumu

9 Eylül 2019: otomatik güncelleştirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Hazırlama Aracısı sorunlarını gidermek için ek izleme ve günlüğe kaydetme yapılandırma olanağı
* Yalnızca eşleme performansını geliştirmek için eşlemede yapılandırılmış Azure AD özniteliklerini getirme olanağı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Azure AD bağlantı hatalarıyla ilgili sorunlar oluşursa, aracıdaki bir hata düzeltilmedi durumuna geçti
* İkili veriler Azure Active Directory okunmadığında soruna neden olan bir hata düzeltildi
* Aracıdaki bir hata düzeltildiğinde bulut hibrit kimlik hizmeti ile güveni yenileyemedi

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Yayın durumu

23 Ocak 2019: indirilmek üzere yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha iyi performans, kararlılık ve güvenilirlik için sağlama Aracısı ve bağlayıcı mimarisi kullanıma alınıyor 
* UI tabanlı yükleme sihirbazını kullanarak sağlama Aracısı yapılandırması basitleşme 
* Otomatik aracı güncelleştirmeleri için destek eklendi


