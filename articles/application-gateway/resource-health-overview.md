---
title: Azure Application Gateway Kaynak Durumu genel bakış
description: Bu makale, Azure Application Gateway kaynak sistem durumu özelliğine genel bakış
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/9/2019
ms.author: victorh
ms.openlocfilehash: db29551a8150b70e797d45fe659482470c8aca2a
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67659508"
---
# <a name="azure-application-gateway-resource-health-overview"></a>Azure Application Gateway Kaynak Durumu genel bakış

[Azure Kaynak Durumu](../service-health/resource-health-overview.md), kaynaklarınızı etkileyen Azure hizmeti sorunlarını tanılayıp gerekli desteği almanıza yardımcı olur. Kaynaklarınızın geçerli ve geçmiş durumu hakkında bilgi verir. Ayrıca, sorunları azaltmanıza yardımcı olacak teknik destek sağlar.

Application Gateway için Kaynak Durumu, sağlıklı olup olmadığını değerlendirmek üzere ağ geçidi tarafından yayılan sinyallere bağımlıdır. Ağ Geçidi sağlıksız ise, Kaynak Durumu sorunun kaynağını belirlemede ek bilgileri analiz eder. Ayrıca, Microsoft 'un aldığı veya sorunu çözmek için yapabileceğiniz işlemleri de tanımlar.

Sistem durumunun değerlendirilme hakkında daha fazla bilgi için, [Azure Kaynak durumu](../service-health/resource-health-checks-resource-types.md#microsoftnetworkapplicationgateways)içindeki kaynak türlerinin ve sistem durumu denetimlerinin tam listesini gözden geçirin.


Application Gateway sistem durumu aşağıdaki durumlardan biri olarak görüntülenir:

## <a name="available"></a>Kullanılabilir

**Kullanılabilir** bir durum, hizmetin kaynağın sistem durumunu etkileyen herhangi bir olay algılamadığı anlamına gelir. Son 24 saat boyunca ağ geçidinin planlanmamış kapalı kalma süresinden kurtarıldığı durumlarda **en son çözümlenen** bildirimi görürsünüz.

![Kullanılabilir sistem durumu](media/resource-health-overview/available-full.png)

## <a name="unavailable"></a>Kullanılamaz

**Kullanılamayan** bir durum, hizmetin ağ geçidinin sistem durumunu etkileyen devam eden bir platform veya platform olmayan bir olay algıladığı anlamına gelir.

### <a name="platform-events"></a>Platform olayları

Platform olayları, Azure altyapısının birden çok bileşeni tarafından tetiklenir. Hem zamanlanmış eylemler (örneğin, planlı bakım) hem de beklenmeyen olaylar (örneğin, planlanmamış bir konak yeniden başlatması) içerirler.

Kaynak Durumu, olay ve kurtarma işlemi hakkında ek ayrıntılar sağlar. Ayrıca etkin bir Microsoft destek anlaşmanız olmasa bile destek ile iletişim kurabilmenizi sağlar.

![Kullanılamayan durum](media/resource-health-overview/unavailable.png)

## <a name="unknown"></a>Bilinmiyor

**Bilinmeyen** sistem durumu kaynak durumu, ağ geçidi hakkında 10 dakikadan uzun süredir bilgi almamış olduğunu gösterir. Bu durum, ağ geçidinin durumunun kesin bir göstergesidir. Ancak sorun giderme sürecinde önemli bir veri noktasıdır.

Ağ Geçidi beklendiği gibi çalışıyorsa, durum birkaç dakika sonra **kullanılabilir** olarak değişir.

Sorun yaşıyorsanız, **Bilinmeyen** sistem durumu, platformda bir olayın ağ geçidini etkilemesini önerebilir.

![Bilinmeyen durum](media/resource-health-overview/unknown.png)

## <a name="degraded"></a>Düzeyi düşürüldü

**Düşürülmüş** sistem durumu, ağ geçidinizin hala kullanım için kullanılabilir olsa da performans kaybı algıladı.

![Düşürülmüş durum](media/resource-health-overview/degraded.png)

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway Web uygulaması güvenlik duvarı (WAF) sorunlarını giderme hakkında bilgi edinmek için bkz. [Azure Application Gateway Için Web uygulaması güvenlik duvarı (WAF) sorunlarını giderme](web-application-firewall-troubleshoot.md).