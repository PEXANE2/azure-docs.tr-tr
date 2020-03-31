---
title: Azure sanal makine ağı kullanımı | Microsoft Dokümanlar
description: Azure sanal makine ağı iş buzundan bilgi edinin.
services: virtual-network
documentationcenter: na
author: steveesp
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/26/2019
ms.author: steveesp
ms.reviewer: kumud, mareat
ms.openlocfilehash: 47f58b25b082784177910d14ab95d8d242fda71a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245440"
---
# <a name="virtual-machine-network-bandwidth"></a>Sanal makine ağı bant genişliği

Azure, her biri farklı performans özellikleri karışımına sahip çeşitli VM boyutları ve türleri sunar. Bir yetenek ağ iş kısmı (veya bant genişliği), saniyede megabit (Mbps) cinsinden ölçülür. Sanal makineler paylaşılan donanımda barındırıldığı için, ağ kapasitesi aynı donanımı paylaşan sanal makineler arasında adil bir şekilde paylaşılmalıdır. Daha büyük sanal makineler, küçük sanal makinelere göre daha fazla bant genişliği tahsis edilir.
 
Her sanal makineye ayrılan ağ bant genişliği, sanal makineden çıkış (giden) trafik üzerinde ölçülür. Sanal makineden ayrılan tüm ağ trafiği, hedeften bağımsız olarak ayrılan sınıra doğru sayılır. Örneğin, sanal bir makinenin 1.000 Mbps sınırı varsa, bu sınır giden trafiğin aynı sanal ağdaki başka bir sanal makineye mi yoksa Azure dışında mı yazıldığı konusunda geçerlidir.
 
Giriş doğrudan ölçülmez veya sınırlı değildir. Ancak, sanal bir makinenin gelen verileri işleme yeteneğini etkilenebilen CPU ve depolama sınırları gibi başka etkenler de vardır.

Hızlandırılmış ağ, gecikme, iş tükeniş ve CPU kullanımı da dahil olmak üzere ağ performansını artırmak için tasarlanmış bir özelliktir. Hızlandırılmış ağ, sanal makinenin iş bilgililiğini geliştirebilir, ancak bunu yalnızca sanal makinenin ayrılan bant genişliğine kadar yapabilir. Hızlandırılmış ağ hakkında daha fazla bilgi edinmek için [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineleri için Hızlandırılmış ağ çalışmalarına bakın.
 
Azure sanal makinelerinde bir tane olması gerekir, ancak bunlara bağlı birkaç ağ arabirimi olabilir. Sanal makineye ayrılan bant genişliği, sanal makineye bağlı tüm ağ arabirimlerindeki tüm giden trafiğin toplamıdır. Başka bir deyişle, ayrılan bant genişliği, sanal makineye kaç ağ arabirimi bağlı olursa olsun, sanal makine başına dır. Farklı Azure VM boyutlarının desteklediği ağ arabirimlerinin kaç tanesini öğrenmek için Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutlarına bakın. 

## <a name="expected-network-throughput"></a>Beklenen ağ iş tonu

Beklenen giden iş bölümü ve her VM boyutu tarafından desteklenen ağ arabirimi sayısı Azure [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutlarında ayrıntılı olarak açıklanmıştır. Genel amaçlı gibi bir tür seçin ve ardından ortaya çıkan sayfada Dv2 serisi gibi bir boyut serisi seçin. Her serinin son sütunda ağ özellikleri başlıklı bir tablo vardır, **Max NIC / Beklenen ağ performansı (Mbps)**. 

İş verme sınırı sanal makine için geçerlidir. İş mesuliyonu aşağıdaki faktörlerden etkilenmez:
- **Ağ arabirimi sayısı**: Bant genişliği sınırı sanal makineden gelen tüm giden trafiğin birikmeli olmasıdır.
- **Hızlandırılmış ağ :** Özellik yayımlanmış sınıra ulaşmada yararlı olsa da, sınırı değiştirmez.
- **Trafik hedefi**: Tüm varış noktaları giden sınıra doğru sayılır.
- **Protokol**: Tüm protokoller üzerindeki tüm giden trafik sınıra doğru sayılır.

## <a name="network-flow-limits"></a>Ağ Akış Limitleri

Bant genişliğine ek olarak, herhangi bir zamanda bir VM'de bulunan ağ bağlantısı sayısı ağ performansını etkileyebilir. Azure ağ yığını, 'akışlar' adı verilen veri yapılarında bir TCP/UDP bağlantısının her yönü için durum tutar. Tipik bir TCP/UDP bağlantısı, biri gelen ve diğeri giden yön için olmak üzere 2 akış oluşturulur. 

Uç noktalar arasında veri aktarımı, veri aktarımını gerçekleştirenlere ek olarak birkaç akış oluşturulmasını gerektirir. Bazı örnekler DNS çözünürlüğü için oluşturulan akışlar ve yük dengeleyici sistem sondaları için oluşturulan akışlardır. Ayrıca ağ geçitleri, yakınlıklar, güvenlik duvarları gibi ağ sanal cihazların (NV'ler) cihazda sonlandırılan ve cihaz tarafından oluşturulan bağlantılar için akışlar oluşturacağını da unutmayın. 

![Bir yönlendirme cihazı aracılığıyla TCP konuşması için akış sayısı](media/virtual-machine-network-throughput/flow-count-through-network-virtual-appliance.png)

## <a name="flow-limits-and-recommendations"></a>Akış Limitleri ve Öneriler

Bugün, Azure ağ yığını, 8 CPU çekirdeğinden büyük vm'ler için iyi performansla 250.000 toplam ağ akışını destekler ve 8 CPU çekirdeğinden az olan VM'ler için iyi performans la birlikte 100k toplam akışları destekler. Bu sınır ağ performansı, 500K toplam akışları, 250K gelen ve 250K giden sabit bir sınıra kadar ek akışlar için incelikle düşer ve sonra ek akışlar bırakılır.

||8 CPU Çekirdeği<vm'ler|8+ CPU Çekirdekli VM'ler|
|---|---|---|
|<b>İyi Performans</b>|100K Akar |250K Akar|
|<b>Bozulmuş Performans</b>|100k Üstü Akışlar|250K'nın üzerinde akışlar|
|<b>Akış Sınırı</b>|500K Akar|500K Akar|

Ölçümler, VM veya VMSS örneklerinizdeki ağ akışlarının sayısını ve akış oluşturma hızını izlemek için [Azure Monitor'da](../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines) kullanılabilir.

![azure-monitör-akış-metrics.png](media/virtual-machine-network-throughput/azure-monitor-flow-metrics.png)

Bağlantı kurma ve sonlandırma oranları, bağlantı kurulumu ve sonlandırma işlemleri CPU olarak ağ performansını da etkileyebilir. İş yüklerini beklenen trafik modellerine göre kıyaslamanızı ve iş yüklerini performans gereksinimlerinize uygun şekilde ölçeklendirmenizi öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine işletim sistemi için ağ aktarım hızını iyileştirme](virtual-network-optimize-network-bandwidth.md)
- Sanal bir makine için [ağ iş buzunu test](virtual-network-bandwidth-testing.md) edin.
