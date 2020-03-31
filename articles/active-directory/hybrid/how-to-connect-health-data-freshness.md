---
title: Azure AD Connect Health - Sağlık hizmeti verileri güncel değil | Microsoft Dokümanlar
description: Bu belge, "Sağlık hizmeti verileri güncel değil" uyarısının nedenini ve sorun giderme nedenini açıklar.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a94bd07cf5020981cdf028ec0eccfa8fa531d240
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897173"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sağlık hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış

Azure AD Connect Health'in uyduğu şirket içi makinelerdeki aracılar, verileri Azure AD Connect Health Service'e düzenli olarak yükler. Hizmet bir aracıdan veri almazsa, portalın sunduğu bilgiler bayat olur. Sorunu vurgulamak için, hizmet Sağlık hizmeti verilerini yükseltecek güncel uyarı **güncel değildir.** Bu uyarı, hizmet son iki saat içinde tam veri almadığunda oluşturulur.  

- Sistem Durumu Hizmeti son iki saat içinde sunucudan gönderilen **yalnızca kısmi** veri türleri almışsa **Uyarı** durumu uyarısı yangınları. Uyarı durumu uyarısı, yapılandırılmış alıcılara e-posta bildirimlerini tetiklemez. 
- Sistem Durumu Hizmeti son iki saat içinde sunucudan herhangi bir veri türü almamışsa **Hata** durumu uyarısı verir. Hata durumu uyarısı, yapılandırılan alıcılara e-posta bildirimlerini tetikler.

Hizmet, servis türüne bağlı olarak şirket içi makinelerde çalışan aracılardan verileri alır. Aşağıdaki tabloda, makinede çalışan aracılar, ne yaptıkları ve hizmetin oluşturduğu veri türleri listelenir. Bazı durumlarda, sürece dahil birden fazla hizmet vardır, bu nedenle bunlardan herhangi biri suçlu olabilir. 

## <a name="understanding-the-alert"></a>Uyarıyı anlama

**Uyarı Ayrıntıları** bıçağı, uyarının ne zaman oluştuğunu ve en son algılandığını gösterir. Her iki saatte bir çalışan bir arka plan işlemi, uyarıyı oluşturur ve yeniden değerlendirir. Aşağıdaki örnekte, ilk uyarı 03/10'da 09:59'da oluştu. Uyarı, 03/12'de saat 10:00'da uyarı tekrar değerlendirildiğinde hala mevcuttu. Bıçak, Sağlık Hizmeti'nin belirli bir veri türünü en son ne zaman aldığını da ayrıntılarıyla anlatır. 
 
 ![Azure AD Connect Sistem durumu uyarı ayrıntıları](./media/how-to-connect-health-data-freshness/data-freshness-details.png)
 
Aşağıdaki tablo, hizmet türlerini ilgili gerekli veri türlerine eşler:

| Hizmet türü | Aracı (Windows Service adı) | Amaç | Oluşturulan veri türü  |
| --- | --- | --- | --- |  
| Azure AD Bağlantısı (Eşitleme) | Azure AD Connect Health Eşitleme Öngörü Hizmeti | AAD Connect'e özgü bilgileri (bağlayıcılar, senkronizasyon kuralları, vb.) toplama | - AadSyncService-SynchronizationRules <br />  - AadSyncService-Konektörler <br /> - AadSyncService-GlobalConfigurations  <br />  - AadSyncService-RunProfileResults <br /> - AadSyncService-ServiceConfigurations <br /> - AadSyncService-ServiceStatus   |
|  | Azure AD Connect Health Eşitleme İzleme Hizmeti | AAD Connect'e özel perf sayaçları, ETW izleri, dosyaları topla | Performans sayacı |
| AD DS | Azure AD Connect Health AD DS Insights Hizmeti | Sentetik testler yapın, topoloji bilgilerini toplayın, çoğaltma meta verileri |  - Ekler-TopolojiBilgi-Json <br /> - Ortak TestData-Json (test sonuçlarını oluşturur)   | 
|  | Azure AD Connect Health AD DS İzleme Hizmeti | ADDS'a özel perf sayaçları, ETW izleri, dosyaları topla | - Performans sayacı  <br /> - Ortak TestData-Json (test sonuçlarını yükler)  |
| AD FS | Azure AD Connect Health AD FS Tanılama Hizmeti | Sentetik testler yapın | Test Sonucu (test sonuçlarını oluşturur) | 
| | Azure AD Connect Health AD FS Öngörü Hizmeti  | ADFS kullanım ölçümlerini topla | Adfs-KullanımÖlçümleri |
| | Azure AD Connect Health AD FS İzleme Hizmeti | ADFS'ye özel perf sayaçları, ETW izleri, dosyaları topla | Test Sonucu (test sonuçlarını yükler) |

## <a name="troubleshooting-steps"></a>Sorun giderme adımları 

Sorunu tanılamak için gereken adımlar aşağıda verilmiştir. Birincisi, tüm Hizmet Türleri için ortak olan temel denetimler kümesidir. 

> [!IMPORTANT] 
> Bu uyarı, Sistem Durumu [veri saklama ilkesini](reference-connect-health-user-privacy.md#data-retention-policy) bağlayın

* Aracıların en son sürümlerinin yüklendiğinden emin olun. [Sürüm geçmişini görüntüleyin.](reference-connect-health-version-history.md) 
* Azure AD Connect Sistem Durumu Aracıları hizmetlerinin makinede **çalıştırdığından** emin olun. Örneğin, AD FS için Connect Health üç hizmet olmalıdır.
  ![Azure AD Connect Health'i doğrulama](./media/how-to-connect-health-agent-install/install5.png)

* Üzerinden gidip [gereksinimleri bölümüne](how-to-connect-health-agent-install.md#requirements)uygun olduğundan emin olun.
* Bağlantı sorunlarını keşfetmek için [test bağlantısı aracını](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) kullanın.
* BIR HTTP Proxy'niz varsa, aşağıdaki [yapılandırma adımlarını](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)izleyin. 


## <a name="next-steps"></a>Sonraki adımlar
Yukarıdaki adımlardan herhangi biri bir sorun tespit ettiyse, sorunu düzeltin ve uyarının çözülmesini bekleyin. Uyarı arka plan işlemi her 2 saatte bir çalışır, bu nedenle uyarıyı çözmek 2 saat kadar sürer. 

* [Azure AD Connect Sistem Durumu veri saklama ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
