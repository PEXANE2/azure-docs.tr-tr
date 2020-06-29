---
title: Azure HPC önbellek ayarlarını yapılandırma
description: MTU ve kök sıkıştırma gibi önbellek için ek ayarların nasıl yapılandırılacağını ve Azure Blob depolama hedeflerinden hızlı anlık görüntülere nasıl erişebileceğinizi açıklar.
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 05/06/2020
ms.author: v-erkel
ms.openlocfilehash: 88aea7e58aacd9a630771948c6dbc6ed5712a674
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85505316"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Ek Azure HPC önbellek ayarlarını yapılandırma

Azure portal **yapılandırma** sayfasında, çeşitli ayarların özelleştirilmesi için seçenekler vardır. Çoğu kullanıcının bu ayarları varsayılan değerlerinden değiştirmesi gerekmez.

Bu makalede ayrıca Azure Blob depolama hedefleri için anlık görüntü özelliğinin nasıl kullanılacağı açıklanır. Anlık görüntü özelliğinin yapılandırılabilir ayarları yok.

Ayarları görmek için, önbelleğin **yapılandırma** sayfasını Azure Portal açın.

![Azure portal 'de yapılandırma sayfasının ekran görüntüsü](media/configuration.png)

> [!TIP]
> [Azure HPC Cache videosunu yönetme videosu](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) , yapılandırma sayfasını ve ayarlarını gösterir.

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

Kök sıkıştırma ayarını **Etkinleştir** ayarı, Azure HPC önbelleğinin istemci makinelerdeki kök kullanıcıdan gelen istekleri nasıl ele aldığını denetler.

Kök sıkıştırarak etkinleştirildiğinde, istemciden gelen kök kullanıcılar Azure HPC Cache aracılığıyla istek gönderdiklerinde "hiçbir zaman" kullanıcısına otomatik olarak eşlenir. Ayrıca, istemci isteklerinin Set-UID izin bitlerini kullanmasını önler.

Kök sıkıştırarak devre dışıysa, istemci kök kullanıcısı (UID 0) isteği, kök olarak bir arka uç NFS depolama sistemine geçirilir. Bu yapılandırma uygunsuz dosya erişimine izin verebilir.

Önbellek üzerinde kök sıkıştırma ayarlama, ``no_root_squash`` depolama hedefleri olarak kullanılan NAS sistemlerinde gerekli ayar için telafi sağlanmasına yardımcı olabilir. ( [NFS depolama hedefi önkoşulları](hpc-cache-prereqs.md#nfs-storage-requirements)hakkında daha fazla bilgi edinin.) Ayrıca, Azure Blob depolama hedefleri ile birlikte kullanıldığında güvenliği de iyileştirebilir.

Varsayılan ayar **Evet**' tir. (Nisan 2020 ' den önce oluşturulan önbellekler varsayılan ayar **No**olabilir.)

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

`.snapshot`BLOB depolama hedefinin ad alanındaki dizindeki anlık görüntülere erişin.
