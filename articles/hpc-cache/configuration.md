---
title: Azure HPC Önbellek ayarlarını yapılandırma
description: MTU ve kök yok-squash gibi önbellek için ek ayarların nasıl yapılandırılabildiğini ve Azure Blob depolama hedeflerinden açık anlık görüntülere nasıl erişeceğiniaçıklar.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: 137e41a3f7d346bb612605660d7798ac2fc297d1
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538823"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Ek Azure HPC Önbelleği ayarlarını yapılandırma

Azure portalındaki **Yapılandırma** sayfasında çeşitli ayarları özelleştirme seçenekleri vardır. Çoğu kullanıcının bunları varsayılan değerlerden değiştirmesi gerekmez.

Bu makalede, Azure Blob depolama hedefleri için anlık görüntü özelliğinin nasıl kullanılacağı da açıklanmaktadır. Anlık görüntü özelliğinin yapılandırılabilir ayarları yoktur.

Ayarları görmek için Azure portalında önbelleğin **Yapılandırma** sayfasını açın.

![Azure portalında yapılandırma sayfasının ekran görüntüsü](media/configuration.png)

## <a name="adjust-mtu-value"></a>MTU değerini ayarlama
<!-- linked from troubleshoot-nas article -->

**MTU boyutu**etiketli açılır menüyü kullanarak önbellek için maksimum iletim birimi boyutunu seçebilirsiniz.

Varsayılan değer 1500 bayttır, ancak bunu 1400 olarak değiştirebilirsiniz.

> [!NOTE]
> Önbelleğin MTU boyutunu düşürürseniz, önbellekle iletişim kuran istemcilerin ve depolama sistemlerinin aynı MTU ayarına veya daha düşük bir değere sahip olduğundan emin olun.

Önbellek MTU değerini düşürmek, önbelleğin ağının geri kalanında paket boyutu kısıtlamaları üzerinde çalışmanıza yardımcı olabilir. Örneğin, bazı VPN'ler tam boyutlu 1500 bayt paketlerini başarıyla iletemez. VPN üzerinden gönderilen paketlerin boyutunu azaltmak bu sorunu ortadan kaldırabilir. Ancak, daha düşük bir önbellek MTU ayarı önbellek ile iletişim kuran diğer herhangi bir bileşenin (istemciler ve depolama sistemleri de dahil olmak üzere) önbellekle iletişim sorunlarını önlemek için daha düşük bir ayarına sahip olması gerektiği anlamına geldiğini unutmayın.

Diğer sistem bileşenlerindeki MTU ayarlarını değiştirmek istemiyorsanız, önbelleğin MTU ayarını düşürmemelisiniz. VPN paket boyutu kısıtlamaları etrafında çalışmak için başka çözümler vardır. Bu sorunu tanılama ve giderme hakkında daha fazla bilgi edinmek için NAS sorun giderme makalesindeki [VPN paket boyutu kısıtlamalarını ayarla'yı](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) okuyun.

Azure SANAL MM'ler [için TCP/IP performans ayarını](../virtual-network/virtual-network-tcpip-performance-tuning.md)okuyarak Azure sanal ağlardaki MTU ayarları hakkında daha fazla bilgi edinin.

## <a name="configure-root-squash"></a>Kök kabak yapılandırma
<!-- linked from troubleshoot -->

**Kök kabamasını etkinleştir** mesuliyonu, Azure HPC Önbelleğinin kök erişimine nasıl izin verdiğini denetler. Kök kabak yetkisiz istemcilerden kök düzeyinde erişimi önlemeye yardımcı olur.

Bu ayar, kullanıcıların önbellek düzeyinde kök erişimini denetlemelerine olanak ``no_root_squash`` tanır ve bu da depolama hedefi olarak kullanılan NAS sistemleri için gerekli ayarı telafi etmesine yardımcı olabilir. [(NFS depolama hedef önkoşulları](hpc-cache-prereqs.md#nfs-storage-requirements)hakkında daha fazla bilgi edinin .) Azure Blob depolama hedefleri ile kullanıldığında güvenliği de artırabilir.

Varsayılan ayar **Evet'tir.** (Nisan 2020'den önce oluşturulan önbellekler varsayılan ayarı **No**olabilir.) Etkinleştirildiğinde, bu özellik istemci isteklerinde ayar-UID izin bitlerinin önbelleğe kullanılmasını da engeller.

## <a name="view-snapshots-for-blob-storage-targets"></a>Blob depolama hedefleri için anlık görüntüleri görüntüleme

Azure HPC Önbelleği, Azure Blob depolama hedefleri için depolama anlık görüntülerini otomatik olarak kaydeder. Anlık görüntüler, arka uç depolama kapsayıcısının içeriği için hızlı bir başvuru noktası sağlar. Anlık görüntüler veri yedeklemelerinin yerini almamaktadır ve önbelleğe alınan verilerin durumu hakkında herhangi bir bilgi içermez.

> [!NOTE]
> Bu anlık görüntü özelliği, NetApp, Isilon veya ZFS depolama yazılımında bulunan anlık görüntü özelliğinden farklıdır. Bu anlık görüntü uygulamaları, anlık görüntü almadan önce önbellekten arka uç depolama sistemine değişiklikleri temizler.
>
> Verimlilik için Azure HPC Önbellek anlık görüntüsü önce değişiklikleri temizlemez ve yalnızca Blob kapsayıcısına yazılmış verileri kaydeder. Bu anlık görüntü önbelleğe alınmış verilerin durumunu göstermez, bu nedenle son değişiklikler dışlanabilir.

Bu özellik yalnızca Azure Blob depolama hedefleri için kullanılabilir ve yapılandırması değiştirilemez.

Enstantaneler her sekiz saatte bir UTC 0:00, 08:00 ve 16:00'da alınır.

Azure HPC Önbelleği, yenileri ile değiştirilene kadar günlük, haftalık ve aylık anlık görüntüleri depolar. Sınırlar şunlardır:

* günde en fazla 20 anlık görüntü
* en fazla 8 haftalık anlık görüntü
* en fazla 3 aylık anlık görüntü

Blob depolama hedefinizin ad alanında dizinden `.snapshot` anlık görüntülere erişin.
