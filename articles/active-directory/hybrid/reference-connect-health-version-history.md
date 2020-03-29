---
title: Azure AD Connect Health Sürüm Geçmişi
description: Bu belge, Azure AD Connect Health sürümlerini ve bu sürümlerde nelerin eklendiğini açıklar.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/20/2019
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ace8d170a5ba48c00775c3b376df8bb70a337d5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897044"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Sürüm Yayınlama Geçmişi
Azure Etkin Dizin ekibi, Azure AD Connect Health'i yeni özellikler ve işlevlerle düzenli olarak günceller. Bu makalede, serbest bırakılmış sürümler ve özellikler listelenmiştir.  

> [!NOTE]
> Connect Health aracıları yeni sürüm yayımlandığında otomatik olarak güncelleştirilir. Lütfen Azure portalından otomatik yükseltme ayarlarının etkinleştirildiğinden emin olun.
>

Azure AD Connect Health for Sync, Azure AD Connect yüklemesiyle tümleştirilir. [Azure AD Connect sürüm geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history) hakkında daha fazla bilgi Özellik geri bildirimi için, Connect Health User Voice [kanalında](https://feedback.azure.com/forums/169401-azure-active-directory/filters/new?category_id=165591) oy verin

## <a name="july-2019"></a>Temmuz 2019
**Aracı Güncelleme**
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.59.0) 
   1. TestWindowsTransport metin değişikliği
   2. AD FS RP yüklemesi için yapılan değişiklikler
   
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.56.0) 
   1. TestWindowsTransport testi ekleyin ve CheckOffice365Son puan testinde WsTrust uç noktaları denetimlerini kaldırın
   2. Giriş İşletim Sistemi ve .NET bilgileri
   3. RP yapılandırma iletisi yükleme boyutunu 1MB'a yükseltin.
   4. Hata düzeltmeleri
   
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.1.56.0) 
   1. Giriş İşletim Sistemi ve .NET bilgileri 
   2. Hata düzeltmeleri

## <a name="may-2019"></a>Mayıs 2019
**Aracı Güncelleme:** 
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.51.0) 
   1. Aynı istemci isteği-kimliğini paylaşan birden çok oturum açma arasında ayrım yapmak için hata düzeltmesi.
   2. Dil yerelleştirilmiş sunucularda kötü kullanıcı adı/parola hatalarını ayrıştama hata düzeltmesi.   

## <a name="april-2019"></a>Nisan 2019
**Aracı Güncelleme:** 
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.46.0) 
   1. ADFS için Yinelenen SPN uyarı işlemini düzeltme

## <a name="march-2019"></a>Mart 2019
**Aracı Güncelleme:** 
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.1.41.0)  
   1. .NET sürüm koleksiyonu
   2. Belirli kategorileri kaçırırken performans sayacı koleksiyonunun iyileştirilmesi
   3. Birden çok İzleme Aracısı örneklerinin yumurtlamasını önlemeye yönelik hata düzeltmesi

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.41.0) 
   1. ADFSToolBox kullanarak AD FS test komut dosyalarını tümleştirme ve yükseltme
   2. .NET sürüm koleksiyonunu uygulayın
   3. Belirli kategorileri kaçırırken performans sayacı koleksiyonunun iyileştirilmesi
   4. Birden çok İzleme Aracısı örneklerinin yumurtlamasını önlemeye yönelik hata düzeltmesi


## <a name="november-2018"></a>Kasım 2018
**Yeni GA özellikleri:** 
* Eşitleme için Azure AD Connect Health - Portaldan yinelenen öznitelik eşitleme hatalarını tanılayın ve düzeltin

**Aracı Güncelleme:** 
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.1.24.0) 
   1. Taşıma Katmanı Güvenliği (TLS) protokolü sürüm 1.2 uyumluluk ve zorlama
   2. Genel Katalog uyarı gürültüsünü azaltın
   3. Sağlık aracısı kayıt hata düzeltmeleri

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.24.0)  
   1. Taşıma Katmanı Güvenliği (TLS) protokolü sürüm 1.2 uyumluluk ve zorlama
   2. Yerelleştirilmiş işletim sistemi için Test-ADFSRequestToken desteği
   3. Çözümlü tanı aracısı EventHandler kilitleme sorunu
   4. Sağlık aracısı kayıt hata düzeltmeleri

## <a name="august-2018"></a>Ağustos 2018 
*  Azure AD Connect sürümü 1.1.880.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 3.1.7.0)    
   1. [.NET Framework KB bültenleri ile izleme aracısının yüksek CPU sorunu](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync) için Hotfix

## <a name="june-2018"></a>Haziran 2018 
**Yeni önizleme özellikleri:** 
* Eşitleme için Azure AD Connect Health - Portaldan yinelenen öznitelik eşitleme hatalarını tanılayın ve düzeltin 

**Aracı Güncelleme:** 
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.1.7.0)    
  1. [.NET Framework KB bültenleri ile izleme aracısının yüksek CPU sorunu](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync) için Hotfix
   
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.7.0)  
  1. [.NET Framework KB bültenleri ile izleme aracısının yüksek CPU sorunu](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync) için Hotfix
  2. ADFS Server 2016 ikincil sunucusunda test sonuçları düzeltmeleri
   
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.1.2.0)  
  1. 3.0.244.0 sürümü için özel olarak aracı bellek yönetimi ve ilgili uyarılar için Hotfix


## <a name="may-2018"></a>Mayıs 2018
**Aracı Güncelleme:**
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.0.244.0)
  1. Aracı gizlilik geliştirme  
  2. Hata düzeltmeleri ve genel iyileştirmeler

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.0.244.0)
  1. Ajan Teşhis Servisi ve ilgili PowerShell modül iyileştirmeleri
  2. Aracı gizlilik geliştirme  
  3. Hata düzeltmeleri ve genel iyileştirmeler

* Azure AD Connect sürümü 1.1.819.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 3.0.164.0) 
  1. Aracı gizlilik geliştirme  
  2. Hata düzeltmeleri ve genel iyileştirmeler


## <a name="march-2018"></a>Mart 2018
**Yeni önizleme özellikleri:**
* Azure AD AD Connect Health for AD FS - Riskli IP raporu ve uyarı.

**Aracı Güncelleme:**

* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.0.176.0)
  1. Aracı kullanılabilirliği iyileştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.0.176.0)
  1. Aracı kullanılabilirliği iyileştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler
* Azure AD Connect sürümü 1.1.750.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 3.0.129.0)  
  1. Aracı kullanılabilirliği iyileştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler

## <a name="december-2017"></a>Aralık 2017
**Aracı Güncelleme:**

* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.0.145.0)
  1. Aracı kullanılabilirliği iyileştirmeleri 
  2. Yeni aracı sorun giderme komutları eklendi
  3. Hata düzeltmeleri ve genel iyileştirmeler
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.0.145.0)
  1. Yeni aracı sorun giderme komutları eklendi
  2. Aracı kullanılabilirliği iyileştirmeleri 
  3. Hata düzeltmeleri ve genel iyileştirmeler
  
## <a name="october-2017"></a>Ekim 2017
**Aracı Güncelleme:**

 * Azure AD Connect sürümü 1.1.649.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 3.0.129.0)
<br></br> Azure AD Connect ile Azure AD Connect Sistem Durumu Temsilcisi arasında eşitleme için bir sürüm uyumluluğu sorunu giderildi. Bu sorun, Sürüm 1.1.647.0'a yerinde yükseltme yapan müşterileri etkiler, ancak şu anda Sağlık Aracısı sürümü 3.0.127.0'a sahiptir. Yükseltmeden sonra, Sistem Durumu Aracısı artık Azure AD Bağlantı Senkronizasyon Hizmeti hakkındaki sistem durumu verilerini Azure AD Sağlık Hizmeti'ne gönderemez. Bu düzeltmeyle, Azure AD Connect yerinde yükseltme sırasında Sağlık Aracısı sürümü 3.0.129.0 yüklenir. Health Agent sürümü 3.0.129.0, Azure AD Connect sürümü 1.1.649.0 ile uyumluluk sorunu içermez.

## <a name="july-2017"></a>Temmuz 2017
**Aracı Güncelleme:**

* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.0.68.0)
  1. Hata düzeltmeleri ve genel iyileştirmeler
  2. Egemen bulut desteği
* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.0.68.0)
  1. Hata düzeltmeleri ve genel iyileştirmeler
  2. Egemen bulut desteği
* Azure AD Connect sürümü 1.1.614.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 3.0.68.0)
  1. Microsoft Azure Kamu Bulutu ve Microsoft Cloud Almanya desteği

## <a name="april-2017"></a>Nisan 2017      
**Aracı Güncelleme:**

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 3.0.12.0)
  1. Hata düzeltmeleri ve genel iyileştirmeler
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 3.0.12.0)
  1. Performans sayaçları yükleme iyileştirmeleri
  2. Hata düzeltmeleri ve genel iyileştirmeler

## <a name="october-2016"></a>Ekim 2016
**Aracı Güncelleme:**

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 2.6.408.0)
* Kimlik doğrulama isteklerinde istemci IP adreslerini algılamada iyileştirmeler
* Uyarılarla İlgili Hata Düzeltmeleri
* AD DS için Azure AD Connect Sağlık aracısı (sürüm 2.6.408.0)
* Uyarılarla ilgili hata düzeltmeleri.
* Azure AD Connect sürümü 1.1.281.0 ile yayımlanan Eşitleme için Azure AD Connect Health aracısı (sürüm 2.6.353.0)
* Eşitleme Hata Raporları için gerekli verileri sağlama
* Uyarılarla ilgili hata düzeltmeleri

**Yeni önizleme özellikleri:**

* Azure AD Connect için Eşitleme Hata Raporları

**Yeni özellikler:**

* AD FS için Azure AD Connect Health - IP adres alanı, kötü kullanıcı adı/parolası olan en iyi 50 kullanıcı hakkında raporda mevcuttur.

## <a name="july-2016"></a>Temmuz 2016
**Yeni önizleme özellikleri:**

* [Azure AD AD DS için Sistem Durumu'na Bağlayın.](how-to-connect-health-adds.md)

## <a name="january-2016"></a>Ocak 2016
**Aracı Güncelleme:**

* AD FS için Azure AD Connect Sağlık aracısı (sürüm 2.6.91.1512)

**Yeni özellikler:**

* [Azure AD Için Test Bağlantı Aracı Sağlık Aracılarını Bağlayın](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Kasım 2015
**Yeni özellikler:**

* Role [Based Access Control](how-to-connect-health-operations.md#manage-access-with-role-based-access-control) desteği

**Yeni önizleme özellikleri:**

* [Azure AD Senkronizasyon için Sistem'e Bağlanıyor.](how-to-connect-health-sync.md)

**Düzeltilen sorunlar:**

* Aracı kayıtları sırasında görülen hatalar için hata düzeltmeleri.

## <a name="september-2015"></a>Eylül 2015
**Yeni özellikler:**

* AD FS için Yanlış Kullanıcı Adı şifre raporu
* Kimliği Doğrulanmamış HTTP Proxy'yi yapılandırma desteği
* Sunucu çekirdeğinde aracıyı yapılandırmak için destek
* AD FS için Uyarılarda Geliştirmeler
* Bağlantı ve veri yükleme için AD FS için Azure AD Connect Health Agent'daki geliştirmeler.

**Düzeltilen sorunlar:**

* AD FS Hata türleri için Kullanım İstatistikleri hata düzeltmeleri.

## <a name="june-2015"></a>Haziran 2015
**AD FS ve AD FS Proxy için Azure AD Connect Health'in ilk sürümü.**

**Yeni özellikler:**

* AD FS ve AD FS Proxy sunucularını e-posta bildirimleriyle izlemek için uyarılar.
* AD FS Performans Sayaçları'nda AD FS topolojisine ve desenlere kolay erişim.
* Uygulamalar, Kimlik Doğrulama Yöntemleri, İstek Ağı Konumu vb. tarafından gruplanan AD FS sunucularında başarılı belirteç isteklerinde eğilim
* Uygulamalar, Hata Türleri vb. tarafından gruplanan AD FS sunucularında başarısız istek eğilimleri
* Azure AD Global Yönetici kimlik bilgilerini kullanarak daha basit Aracı Dağıtımı.  

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimlik altyapınızı ve senkronizasyon hizmetlerinizi bulutta izleme](whatis-hybrid-identity-health.md)hakkında daha fazla bilgi edinin.

