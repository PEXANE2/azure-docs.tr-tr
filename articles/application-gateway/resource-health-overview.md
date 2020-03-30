---
title: Azure Uygulama Ağ Geçidi Kaynak Durumu genel bakış
description: Bu makale, Azure Uygulama Ağ Geçidi için kaynak durumu özelliğine genel bir bakıştır
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/9/2019
ms.author: victorh
ms.openlocfilehash: db29551a8150b70e797d45fe659482470c8aca2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67659508"
---
# <a name="azure-application-gateway-resource-health-overview"></a>Azure Uygulama Ağ Geçidi Kaynak Durumu genel bakış

[Azure Kaynak Durumu](../service-health/resource-health-overview.md), kaynaklarınızı etkileyen Azure hizmeti sorunlarını tanılayıp gerekli desteği almanıza yardımcı olur. Kaynaklarınızın mevcut ve geçmiş sağlık durumu hakkında sizi bilgilendirir. Ve sorunları azaltmanıza yardımcı olmak için teknik destek sağlar.

Uygulama Ağ Geçidi için Kaynak Durumu, ağ geçiditarafından yayılan sinyallere güvenerek sağlıklı olup olmadığını değerlendirir. Ağ geçidi sağlıksızsa, Kaynak Durumu sorunun kaynağını belirlemek için ek bilgileri çözümler. Ayrıca, Microsoft'un gerçekleştirdiği eylemleri veya sorunu gidermek için neler yapabileceğinizi de tanımlar.

Sağlık durumunun nasıl değerlendirildiği hakkında daha fazla bilgi için [Azure Kaynak Durumu'ndaki](../service-health/resource-health-checks-resource-types.md#microsoftnetworkapplicationgateways)kaynak türlerinin ve sistem durumu denetimlerinin tam listesini gözden geçirin.


Uygulama Ağ Geçidi için sistem durumu aşağıdaki durumlardan biri olarak görüntülenir:

## <a name="available"></a>Kullanılabilir

**Kullanılabilir** durum, hizmetin kaynağın sistem durumunu etkileyen herhangi bir olay algılamadığı anlamına gelir. Ağ geçidinin son 24 saat içinde planlanmamış kapalı kalma sürelerinden kurtarıldığı **durumlarda, son olarak çözümlenen** bildirimi görürsünüz.

![Kullanılabilir sağlık durumu](media/resource-health-overview/available-full.png)

## <a name="unavailable"></a>Kullanılamaz

**Kullanılamayan** durum, hizmetin ağ geçidinin sistem durumunu etkileyen devam eden bir platform veya platform dışı olay algıladığını anlamına gelir.

### <a name="platform-events"></a>Platform etkinlikleri

Platform olayları, Azure altyapısının birden çok bileşeni tarafından tetiklenir. Bunlar hem zamanlanmış eylemleri (örneğin, planlı bakım) hem de beklenmeyen olayları (örneğin, planlanmamış bir ana bilgisayar yeniden başlatma) içerir.

Kaynak Durumu olay ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Etkin bir Microsoft destek sözleşmeniz olmasa bile desteğe başvurmanızı da sağlar.

![Kullanılamayan durum](media/resource-health-overview/unavailable.png)

## <a name="unknown"></a>Bilinmiyor

**Bilinmeyen** sistem durumu, Kaynak Durumu'nun ağ geçidi hakkında 10 dakikadan uzun süredir bilgi almadığını gösterir. Bu durum, ağ geçidinin durumunun kesin bir göstergesi değildir. Ancak sorun giderme işleminde önemli bir veri noktasıdır.

Ağ geçidi beklendiği gibi çalışıyorsa, durum birkaç dakika sonra **Kullanılabilir** olarak değişir.

Sorunlarla karşınızda, **Bilinmeyen** sağlık durumu platformdaki bir olayın ağ geçidini etkilediğini düşündürebilir.

![Bilinmeyen durum](media/resource-health-overview/unknown.png)

## <a name="degraded"></a>Düzeyi düşürüldü

**Bozulmuş** sistem durumu, ağ geçidinizin hala kullanılabilir olmasına rağmen performans kaybı algıladığınızı gösterir.

![Degrated durumu](media/resource-health-overview/degraded.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme Uygulaması Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF) hakkında bilgi edinmek için Azure [Uygulama Ağ Geçidi için Sorun Giderme Web Uygulaması Güvenlik Duvarı (WAF)](web-application-firewall-troubleshoot.md)bakın.