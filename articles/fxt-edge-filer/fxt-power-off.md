---
title: Microsoft Azure FXT Edge Filer birimi nasıl kapatılır?
description: Azure FXT Edge Filer düğümünün başlatılması ve güvenli bir şekilde kapatılması için yordamlar
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: rohogue
ms.openlocfilehash: 91445c4219abaa67385105f52515f52c87e2cf06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72255991"
---
# <a name="how-to-safely-power-off-azure-fxt-edge-filer-hardware"></a>Azure FXT Edge Filer donanımı güvenli bir şekilde nasıl kapatılabilen

Fiziksel güç düğmesini tek bir düğümü açmak için kullanabiliyor olsanız da, normal koşullar altında üniteyi kapatmak için kullanmamalısınız.

Bir Azure FXT Kenar Filer düğümü kümenin bir parçası olarak kullanımda kaldıktan sonra, donanımı kapatmak için küme denetim paneli yazılımını kullanmanız gerekir. 

> [!NOTE] 
> Olası veri kaybını veya bozulmasını önlemek için, bir Azure FXT Edge Filer'ı kapatmak için her zaman Denetim Masası yazılımını kullanın. Microsoft Müşteri Hizmetleri ve Desteği tarafından size talimat verilmedikçe, fiziksel güç düğmesini kapatma için kullanmayın.
> 
> Elektrik acil durumlarda, güç kablolarını kesin veya veri merkezinizin elektrik kesme mekanizmasını kullanın.

## <a name="shut-down-a-node-from-the-control-panel"></a>Denetim Paneli'nden bir düğümü kapatma

Azure FXT Edge Filer düğümlerini güvenli bir şekilde kapatmak için aşağıdaki yönergeleri izleyin:

1. Küme Denetim Paneli'nde oturum açın. (Ayarlar [sayfalarını Açma](fxt-cluster-create.md#open-the-settings-pages)yol tarifi )
1. **Ayarlar** sekmesini tıklatın ve ardından **Cluster** > **FXT Düğümleri** sayfasını yükleyin.
1. Küme düğümleri listesinde kapatmak istediğinizi bulun. **Eylemler** sütunundaki **Güç aşağı** düğmesini tıklatın. 
1. Birkaç dakika bekle. Düğüm kapanacak ve kendi kendine güçlenecek.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure FXT Edge Filer donanım durumunu izleyin'deki](fxt-monitor.md)durum LED'leri ve diğer göstergeler hakkında bilgi edinin.
* [Connect güç kablolarında](fxt-network-power.md#connect-power-cables)Azure FXT Edge Filer güç kaynakları hakkında daha fazla bilgi edinin.
