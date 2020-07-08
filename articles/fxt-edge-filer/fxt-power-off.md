---
title: Microsoft Azure FXT Edge Filer birimini kapatma
description: Azure FXT Edge Filer düğümünü başlatma ve güvenle kapanmaya yönelik yordamlar
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: how-to
ms.date: 07/01/2019
ms.author: rohogue
ms.openlocfilehash: 92364de82bc3de8229eced4ee02997a27afbde45
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85506421"
---
# <a name="how-to-safely-power-off-azure-fxt-edge-filer-hardware"></a>Azure FXT Edge Filer donanımını güvenle kapatma

Tek bir düğüme geçiş yapmak için fiziksel güç düğmesini kullanabilseniz de, birimi normal koşullarda kapatmak için kullanmamalısınız.

Bir Azure FXT Edge Filer düğümü bir kümenin parçası olarak kullanıldıktan sonra, donanımı kapatmak için küme Denetim Masası yazılımını kullanmanız gerekir. 

> [!NOTE] 
> Olası veri kaybını veya bozulmasını önlemek için, her zaman Denetim Masası yazılımını kullanarak bir Azure FXT Edge filu 'ni kapatın. Microsoft Müşteri Hizmetleri ve destek ile ilgili olarak belirtilmedikçe, kapatma için fiziksel güç düğmesini kullanmayın.
> 
> Elektrik acil bir durumda, güç bağlantılarının bağlantısını kesin veya veri merkezinizi elektrik bağlantısı kesme mekanizmasını kullanın.

## <a name="shut-down-a-node-from-the-control-panel"></a>Denetim masasından bir düğümü kapatma

Azure FXT Edge Filer düğümünü güvenle kapatmak için bu yönergeleri izleyin:

1. Küme denetim masasında oturum açın. ( [Ayarlar sayfalarında açık](fxt-cluster-create.md#open-the-settings-pages)olan yönergeler)
1. **Ayarlar** sekmesine tıklayın ve ardından **cluster**  >  **FXT düğümleri** sayfasını yükleyin.
1. Küme düğümleri listesinde, kapatmak istediğiniz bir tane bulun. **Eylemler** sütununda, **Güç aşağı** düğmesine tıklayın. 
1. Birkaç dakika bekleyin. Düğüm kapanır ve devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure FXT Edge Filer donanım durumunu izlemek](fxt-monitor.md)Için durum LED 'leri ve diğer göstergeler hakkında bilgi edinin.
* [Power kabloları](fxt-network-power.md#connect-power-cables)ile Azure FXT Edge Filer güç kaynakları hakkında daha fazla bilgi edinin.
