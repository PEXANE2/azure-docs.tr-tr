---
title: Azure Service Fabric yeniden yapılandırma
description: Durum bilgisi olan hizmet çoğaltmalarının yapılandırmalarını ve değişiklik sırasında tutarlılığı ve kullanılabilirliği korumak Service Fabric yeniden yapılandırma işlemini öğrenin.
author: appi101
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: bd46a7776495624affef77a44fcf68334750ba17
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75610004"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Azure Service Fabric yeniden yapılandırma
Bir *yapılandırma* , durum bilgisi olan bir hizmetin bir bölümü için çoğaltmalar ve rolleri olarak tanımlanır.

Yeniden *yapılandırma, bir* yapılandırmayı başka bir yapılandırmaya taşıma işlemidir. Durum bilgisi olan bir hizmetin bir bölümü için çoğaltma kümesinde değişiklik yapar. Eski yapılandırmaya *önceki yapılandırma (PC)* adı verilir ve yeni yapılandırma *GEÇERLI yapılandırma (CC)* olarak adlandırılır. Azure Service Fabric yeniden yapılandırma Protokolü tutarlılığı korur ve çoğaltma kümesinde yapılan herhangi bir değişiklik sırasında kullanılabilirliği korur.

Yük Devretme Yöneticisi, sistemdeki farklı olaylara yanıt olarak yeniden yapılandırma başlatır. Örneğin, birincil başarısız olursa, etkin bir ikincili birincil olarak yükseltmek için yeniden yapılandırma işlemi başlatılır. Başka bir örnek ise uygulama yükseltmelerine yanıt olarak, düğümü yükseltmek için birincili başka bir düğüme taşınması gerekebilir.

## <a name="reconfiguration-types"></a>Yeniden yapılandırma türleri
Yeniden yapılandırma iki tür olarak sınıflandırılabilirler:

- Birincil 'ın değişmekte olduğu yeniden yapılandırma:
    - **Yük devretme: yük devretme**işlemleri, çalışan bir birincil hataya yanıt olarak yeniden yapılandırmalara sahiptir.
    - **SwapPrimary**: takas, genellikle yük dengelemeye veya yükseltmeye yanıt olarak, Service Fabric çalışan bir birincili bir düğümden diğerine taşınması gereken yeniden yapılandırlardır.

- Birincil değişiklik olmayan yeniden yapılandırma.

## <a name="reconfiguration-phases"></a>Yeniden yapılandırma aşamaları
Yeniden yapılandırma birkaç aşamada devam eder:

- **Phase0**: Bu aşama, geçerli birincil sunucunun durumunu yeni birincil konuma aktardığı ve etkin ikinciye geçişleri olan takas birincili yeniden yapılandırmalarda gerçekleşir.

- **Aşama 1**: Bu aşama, birincil olarak değişen yeniden yapılandırma sırasında gerçekleşir. Bu aşamada, Service Fabric geçerli çoğaltmalar arasında doğru birincili tanımlar. Yeni birincil zaten seçildiğinden, takas birincili yeniden yapılandırma sırasında bu aşama gerekli değildir. 

- **Aşama 2**: bu aşamada Service Fabric, tüm verilerin geçerli yapılandırmanın çoğu çoğaltmalarda kullanılabilir olmasını sağlar.

Yalnızca iç kullanım için birkaç başka aşama vardır.

## <a name="stuck-reconfigurations"></a>Takılı yeniden yapılandırma
Yeniden yapılandırma çeşitli nedenlerle *takılmasına* neden olabilir. Bazı yaygın nedenler şunlardır:

- **Aşağı çoğaltmalar**: bazı yeniden yapılandırma aşamaları, yapılandırmadaki çoğaltmaların büyük çoğunluğunu gerektirir.
- **Ağ veya iletişim sorunları**: yeniden yapılandırma, farklı düğümler arasında ağ bağlantısı gerektirir.
- **API sorunları**: yeniden yapılandırma Protokolü, hizmet uygulamalarının belirli API 'leri bitirmesini gerektirir. Örneğin, güvenilir bir hizmette iptal belirtecinin olmaması SwapPrimary yeniden yapılandırmaların takılmasına neden olur.

Yeniden yapılandırma nerede takılmış olduğunu tanılamak için System.FM, System. RA ve System. RAP gibi sistem bileşenlerinden sistem durumu raporlarını kullanın. [Sistem durumu raporu sayfasında](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bu durum raporları açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
- [Sistem durumu raporları](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
