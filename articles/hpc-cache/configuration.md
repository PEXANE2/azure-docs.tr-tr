---
title: Azure HPC önbellek ayarlarını yapılandırma
description: MTU ve kök sıkıştırma gibi önbellek için ek ayarların nasıl yapılandırılacağını ve Azure Blob depolama hedeflerinden hızlı anlık görüntülere nasıl erişebileceğinizi açıklar.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/27/2020
ms.author: v-erkel
ms.openlocfilehash: 7938fcc0819fc3e5e0762cc8c3c2931594ed1c68
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195069"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Ek Azure HPC önbellek ayarlarını yapılandırma

Azure portal **yapılandırma** sayfasında, çeşitli ayarların özelleştirilmesi için seçenekler vardır. Çoğu kullanıcının bu ayarları varsayılan değerlerinden değiştirmesi gerekmez.

Bu makalede ayrıca Azure Blob depolama hedefleri için anlık görüntü özelliğinin nasıl kullanılacağı açıklanır. Anlık görüntü özelliğinin yapılandırılabilir ayarları yok.

Ayarları görmek için, önbelleğin **yapılandırma** sayfasını Azure Portal açın.

![Azure portal 'de yapılandırma sayfasının ekran görüntüsü](media/configuration.png)

## <a name="adjust-mtu-value"></a>MTU değerini ayarla
<!-- linked from troubleshoot-nas article -->

**MTU boyutu**etiketli açılan menüyü kullanarak önbelleğin en büyük iletim birimi boyutunu seçebilirsiniz.

Varsayılan değer 1500 bayttır, ancak bunu 1400 olarak değiştirebilirsiniz.

> [!NOTE]
> Önbelleğin MTU boyutunu düşürürseniz, önbellekle iletişim kuran istemcilerin ve depolama sistemlerinin aynı MTU ayarına veya daha düşük bir değere sahip olduğundan emin olun.

Önbellek MTU değerini düşürmek, önbelleğin ağının geri kalanında paket boyutu kısıtlamalarına geçici çözüm sağlamanıza yardımcı olabilir. Örneğin, bazı VPN 'Ler tam boyutlu 1500 baytlık paketleri başarıyla iletebilir. VPN üzerinden gönderilen paketlerin boyutunu azaltmak bu sorunu ortadan kaldırabilir. Ancak, daha düşük bir önbellek MTU ayarı, istemci ve depolama sistemleri dahil olmak üzere önbelleğiyle iletişim kuran diğer tüm bileşenlerin Ayrıca iletişim sorunlarından kaçınmak için daha düşük bir MTU ayarı olması gerektiğini gösterir.

Diğer sistem bileşenlerinde MTU ayarlarını değiştirmek istemiyorsanız, önbelleğin MTU ayarını düşürmemelisiniz. VPN paket boyutu kısıtlamalarına geçici bir çözüm bulabilirsiniz. Bu sorunu tanılama ve ele alma hakkında daha fazla bilgi edinmek için, NAS sorun giderme makalesindeki [VPN paket boyutu kısıtlamalarını ayarla](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) makalesini okuyun.

Azure [VM 'leri Için TCP/IP performans ayarlamayı](../virtual-network/virtual-network-tcpip-performance-tuning.md)okuyarak Azure sanal ağlarında MTU ayarları hakkında daha fazla bilgi edinin.

## <a name="configure-root-squash"></a>Kök sıkıştırarak 'i yapılandırma
<!-- linked from troubleshoot -->

**Kök sıkıştırarak ayarını etkinleştir** ayarı, Azure HPC önbelleğinin kök erişimine nasıl izin verdiğini denetler. Kök sıkıştırarak, yetkisiz istemcilerden kök düzeyinde erişimi önlemeye yardımcı olur.

Bu ayar, kullanıcıların, depolama hedefleri olarak kullanılan NAS sistemlerine yönelik gerekli ``no_root_squash`` ayar için telafi sağlayan önbellek düzeyinde kök erişimi denetlemesine olanak tanır. ( [NFS depolama hedefi önkoşulları](hpc-cache-prereqs.md#nfs-storage-requirements)hakkında daha fazla bilgi edinin.) Ayrıca, Azure Blob depolama hedefleri ile birlikte kullanıldığında güvenliği de iyileştirebilir.

Varsayılan ayar **Evet**' tir. (Nisan 2020 ' den önce oluşturulan önbellekler varsayılan ayar **No**olabilir.)

Bu özellik etkinleştirildiğinde, istemci isteklerindeki Set-UID izin bitlerinin önbelleğe yönelik kullanımını da engeller.

## <a name="view-snapshots-for-blob-storage-targets"></a>BLOB depolama hedeflerinin anlık görüntülerini görüntüleme

Azure HPC Cache, Azure Blob depolama hedeflerine yönelik depolama anlık görüntülerini otomatik olarak kaydeder. Anlık görüntüler, arka uç depolama kapsayıcısının içeriği için hızlı bir başvuru noktası sağlar.

Anlık görüntüler veri yedeklemelerinin yerini almaz ve önbelleğe alınmış verilerin durumu hakkında bilgi içermez.

> [!NOTE]
> Bu anlık görüntü özelliği, NetApp veya ıconnectionstorage yazılımında bulunan Snapshot özelliğinden farklıdır. Bu anlık görüntü uygulamaları, anlık görüntüyü almadan önce önbellekteki değişiklikleri arka uç depolama sistemine temizler.
>
> Verimlilik için, Azure HPC önbellek anlık görüntüsü önce değişiklikleri temizlemez ve yalnızca blob kapsayıcısına yazılmış verileri kaydeder. Bu anlık görüntü, önbelleğe alınmış verilerin durumunu temsil etmez, bu nedenle son değişiklikleri içermeyebilir.

Bu özellik yalnızca Azure Blob depolama hedefleri için kullanılabilir ve yapılandırması değiştirilemez.

Anlık görüntüler, UTC 0:00, 08:00 ve 16:00 ' de sekiz saatte bir alınır.

Azure HPC Cache, günlük, haftalık ve aylık anlık görüntüleri yeni olanlar tarafından değiştirilene kadar depolar. Sınırlar şunlardır:

* en fazla 20 günlük anlık görüntü
* 8 haftalık anlık görüntüye kadar
* en fazla 3 aylık anlık görüntü

BLOB depolama hedefinin ad alanındaki `.snapshot` dizindeki anlık görüntülere erişin.
