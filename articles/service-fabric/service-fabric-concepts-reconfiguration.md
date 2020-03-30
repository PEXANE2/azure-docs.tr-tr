---
title: Azure Hizmet Kumaşında Yeniden Yapılandırma
description: Durum lu hizmet yinelemeleri için yapılandırmalar ve değişiklik sırasında tutarlılığı ve kullanılabilirliği korumak için Hizmet Kumaşı'nın kullandığı yeniden yapılandırma işlemi hakkında bilgi edinin.
author: appi101
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: bd46a7776495624affef77a44fcf68334750ba17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610004"
---
# <a name="reconfiguration-in-azure-service-fabric"></a>Azure Hizmet Kumaşında Yeniden Yapılandırma
*Yapılandırma,* durum lu bir hizmetin bölünmesi için yinelemeler ve rolleri olarak tanımlanır.

*Yeniden yapılandırma,* bir yapılandırmayı başka bir yapılandırmaya taşıma işlemidir. Bu, devlet hizmetinin bölünmesi için yineleme kümesinde bir değişiklik yapar. Eski yapılandırmaönceki *yapılandırma (PC)* olarak adlandırılır ve yeni yapılandırma *geçerli yapılandırma (CC)* olarak adlandırılır. Azure Hizmet Kumaşı'ndaki yeniden yapılandırma protokolü tutarlılığı korur ve yineleme kümesinde yapılan değişiklikler sırasında kullanılabilirliği korur.

Failover Manager, sistemdeki farklı olaylara yanıt olarak yeniden yapılandırmalar başlatır. Örneğin, birincil başarısız olursa, birincil için etkin bir ikincil teşvik etmek için bir yeniden yapılandırma başlatılır. Başka bir örnek, düğümü yükseltmek için birincil düğümü başka bir düğüme taşımak için gerekli olabilir uygulama yükseltmeleri yanıt olarak.

## <a name="reconfiguration-types"></a>Yeniden yapılandırma türleri
Yeniden yapılandırmalar iki türe sınıflandırılabilir:

- Birincil in in in değiştiği yeniden yapılandırmalar:
    - **Failover**: Failovers çalışan bir birincil başarısızlığı yanıt olarak yeniden yapılandırmaları vardır.
    - **SwapPrimary**: Takaslar, Service Fabric'in çalışan birincil bir düğümü bir düğümden diğerine taşıması gereken, genellikle yük dengeleme veya yükseltmeye yanıt olarak yeniden yapılandırmalardır.

- Birincil değişmeyen yeniden yapılandırmalar.

## <a name="reconfiguration-phases"></a>Yeniden yapılandırma aşamaları
Yeniden yapılandırma çeşitli aşamalarda ilerler:

- **Aşama0**: Bu aşama, cari birincil durumu yeni birincile aktardığı ve etkin ikincil ekime geçişyaptığı takas-birincil yeniden yapılandırmalarında gerçekleşir.

- **Aşama1**: Bu aşama, birincil inin değiştiği yeniden yapılandırmalar sırasında gerçekleşir. Bu aşamada, Hizmet Kumaşı geçerli yinelemeler arasında doğru birincil tanımlar. Yeni birincil zaten seçilmiş olduğundan, takas birincil yeniden yapılandırmaları sırasında bu aşama gerekli değildir. 

- **Aşama2**: Bu aşamada, Service Fabric tüm verilerin geçerli yapılandırmanın yinelemelerinin çoğunda kullanılabilir olmasını sağlar.

Yalnızca dahili kullanım için birkaç diğer aşamaları vardır.

## <a name="stuck-reconfigurations"></a>Sıkışmış yeniden yapılandırmaları
Yeniden yapılandırmalar çeşitli nedenlerle *sıkışıp kalabilir.* Sık karşılaşılan nedenlerden bazıları şunlardır:

- **Aşağı yinelemeler**: Bazı yeniden yapılandırma aşamaları, yapılandırmadaki yinelemelerin çoğunun yukarı olmasını gerektirir.
- **Ağ veya iletişim sorunları**: Yeniden yapılandırmalar farklı düğümler arasında ağ bağlantısı gerektirir.
- **API hataları**: Yeniden yapılandırma protokolü, hizmet uygulamalarının belirli API'leri bitirmesini gerektirir. Örneğin, güvenilir bir hizmetteki iptal belirteci onurlandırma, SwapPrimary yeniden yapılandırmalarının sıkışmasına neden olur.

Yeniden yapılandırmanın sıkıştığı yeri tanılamak için System.FM, System.RA ve System.RAP gibi sistem bileşenlerinin sistem raporlarını kullanın. [Sistem sağlık raporu sayfası](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) bu sağlık raporlarını açıklar.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)
- [Sistem sağlık raporları](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
