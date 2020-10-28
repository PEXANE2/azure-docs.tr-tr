---
title: Kaynak sistem durumunu anlayın
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS örneğinizin sistem durumunu denetlemek için Azure Kaynak Durumu kullanma konusuna bakın.
author: baanders
ms.author: baanders
ms.date: 10/6/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 9c31345a4ddaf9ac2b75204172dbc47606cb07db
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681730"
---
# <a name="troubleshooting-azure-digital-twins-resource-health"></a>Azure dijital TWINS sorunlarını giderme: kaynak durumu

[Azure Kaynak durumu](../service-health/resource-health-overview.md) , Azure kaynaklarınızı etkileyen hizmet sorunlarını tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın geçerli ve geçmiş durumunu raporlar.

Bu makalede, Azure dijital TWINS örneklerinizin **kaynak durum** bilgilerini nasıl alacağınız gösterilmektedir.

## <a name="use-azure-resource-health"></a>Azure Kaynak Durumu kullan

Azure Kaynak Durumu, Azure dijital TWINS örneğinizin çalışır duruma ve çalışıyor olup olmadığını izlemenize yardımcı olabilir. Ayrıca, bir bölgesel kesintide örneğinizin sistem durumunu etkileyip etkilemediğini öğrenmek için de kullanabilirsiniz.

Örneğinizin durumunu denetlemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Örnek menüsünde, *destek + sorun giderme* altında _**kaynak durumu**_ ' nu seçin. Bu işlem sizi kaynak sistem durumu geçmişini görüntüleme sayfasına götürür. 

    :::image type="content" source="media/troubleshoot-resource-health/resource-health.png" alt-text="' Kaynak sistem durumu ' sayfasını gösteren ekran görüntüsü. Son dokuz günden itibaren günlük bir raporu gösteren bir ' sistem durumu geçmişi ' bölümü var. Her gün ' kullanılabilir ' durumunu gösterir.":::

Yukarıdaki görüntüde, bu örnek *kullanılabilir* olarak gösteriliyor ve son dokuz güne kadar oldu. *Kullanılabilir* durum ve görünebilen diğer durum türleri hakkında daha fazla bilgi edinmek için bkz. [*Azure Kaynak durumu genel bakış*](../service-health/resource-health-overview.md).

[*Azure Kaynak durumu 'Nda kaynak türleri ve durum denetimleri*](../service-health/resource-health-checks-resource-types.md)' nde farklı türlerdeki Azure kaynakları için kaynak sistem durumu ' na gidecek farklı denetimler hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure dijital TWINS örneğinizi izlemenin diğer yolları hakkında bilgi edinin:
* [*Sorun giderme: Azure Izleyici ile ölçümleri görüntüleme*](troubleshoot-metrics.md)
* [*Sorun giderme: tanılamayı ayarlayın*](troubleshoot-diagnostics.md).
* [*Sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md)
