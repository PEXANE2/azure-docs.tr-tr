---
title: Avere vFXT-Azure 'ı bağlama
description: Azure için avere vFXT ile istemcileri bağlama
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: rohogue
ms.openlocfilehash: b8486b5a33226b1faa5e3874144129dbe7a1a2f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76153420"
---
# <a name="mount-the-avere-vfxt-cluster"></a>Avere vFXT kümesini takma

İstemci makinelerini vFXT kümenize bağlamak için aşağıdaki adımları izleyin.

1. İstemci trafiğinin Küme düğümleriniz arasında nasıl yük dengelenmesi gerektiğine karar verin. Ayrıntılar için, aşağıda [istemci yükünü](#balance-client-load)okuyun.
1. Takılacak IP adresini ve birleşim yolunu belirler.
1. [Bağlama komutunu](#mount-command-arguments)uygun bağımsız değişkenlerle verin.

## <a name="balance-client-load"></a>İstemci yükünü dengeleme

İstemci isteklerinin kümedeki tüm düğümler arasında dengelenmesi için, istemcileri istemciye yönelik tüm IP adreslerinin tam aralığına bağlamanız gerekir. Bu görevi otomatikleştirmenin birkaç basit yolu vardır.

> [!TIP]
> Diğer yük dengeleme yöntemleri büyük veya karmaşık sistemler için uygun olabilir; yardım için [bir destek bileti açın](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) .)
>
> Otomatik sunucu tarafı yük dengelemesi için bir DNS sunucusu kullanmayı tercih ederseniz, Azure 'da kendi DNS sunucunuzu ayarlamanız ve yönetmeniz gerekir. Bu durumda, vFXT kümesi için hepsini bir kez deneme DNS 'yi bu belgeye göre yapılandırabilirsiniz: [avere Cluster DNS yapılandırması](avere-vfxt-configure-dns.md).

### <a name="sample-balanced-client-mounting-script"></a>Örnek dengeli istemci bağlama betiği

Bu kod örneği, istemcileri vFXT kümesinin kullanılabilir IP adreslerine dağıtmak için istemci IP adreslerini rastgele bir öğe olarak kullanır.

```bash
function mount_round_robin() {
    # to ensure the nodes are spread out somewhat evenly the default
    # mount point is based on this node's IP octet4 % vFXT node count.
    declare -a AVEREVFXT_NODES="($(echo ${NFS_IP_CSV} | sed "s/,/ /g"))"
    OCTET4=$((`hostname -i | sed -e 's/^.*\.\([0-9]*\)/\1/'`))
    DEFAULT_MOUNT_INDEX=$((${OCTET4} % ${#AVEREVFXT_NODES[@]}))
    ROUND_ROBIN_IP=${AVEREVFXT_NODES[${DEFAULT_MOUNT_INDEX}]}

    DEFAULT_MOUNT_POINT="${BASE_DIR}/default"

    # no need to write again if it is already there
    if ! grep --quiet "${DEFAULT_MOUNT_POINT}" /etc/fstab; then
        echo "${ROUND_ROBIN_IP}:${NFS_PATH}    ${DEFAULT_MOUNT_POINT}    nfs hard,proto=tcp,mountproto=tcp,retry=30 0 0" >> /etc/fstab
        mkdir -p "${DEFAULT_MOUNT_POINT}"
        chown nfsnobody:nfsnobody "${DEFAULT_MOUNT_POINT}"
    fi
    if ! grep -qs "${DEFAULT_MOUNT_POINT} " /proc/mounts; then
        retrycmd_if_failure 12 20 mount "${DEFAULT_MOUNT_POINT}" || exit 1
    fi
}
```

Yukarıdaki işlev, [avere vFXT örnekleri](https://github.com/Azure/Avere#tutorials) sitesinde bulunan Batch örneği 'nin bir parçasıdır.

## <a name="create-the-mount-command"></a>Bağlama komutunu oluşturma

> [!NOTE]
> Avere vFXT kümenizi oluştururken yeni bir blob kapsayıcısı oluşturmadıysanız, istemcileri bağlamaya çalışmadan önce depolama birimi [yapılandırma](avere-vfxt-add-storage.md) bölümünde açıklandığı gibi depolama sistemleri ekleyin.

``mount`` Komut, Istemcinizden vFXT kümesindeki sanal sunucuyu (vServer) yerel dosya sisteminde bir yola eşler. Biçim``mount <vFXT path> <local path> {options}``

Mount komutunun üç öğesi vardır:

* vFXT yolu-aşağıda açıklanan küme 9 ' da bir IP adresi ve ad alanı birleşim yolu birleşimi)
* Yerel yol-istemcideki yol
* bağlama komutu seçenekleri- [bağlama komutu bağımsız değişkenlerinde](#mount-command-arguments) listelenen

### <a name="junction-and-ip"></a>Kavşak ve IP

VServer yolu, *IP adresinin* bir birleşimidir ve bir *ad alanı birleşiminin*yoludur. Ad alanı birleşimi, depolama sistemi eklendiğinde tanımlanan bir sanal yoldur.

Kümeniz BLOB depolama ile oluşturulduysa, bu kapsayıcının ad alanı yolu`/msazure`

Örnek: ``mount 10.0.0.12:/msazure /mnt/vfxt``

Kümeyi oluşturduktan sonra depolama eklediyseniz, ad alanı birleşim yolu, birleşim oluştururken **ad alanı yolunda** ayarladığınız değerdir. Örneğin, ad alanı yolu olarak ``/avere/files`` kullandıysanız istemcileriniz *IP_address*:/avere/Files öğesini yerel bağlama noktasına bağlayabilir.

![Ad alanı yolu alanındaki/avere/Files ile "yeni birleşim Ekle" iletişim kutusu](media/avere-vfxt-create-junction-example.png) <!-- to do - change example and screenshot to vfxt/files instead of avere -->

IP adresi, vServer için tanımlanan istemciye yönelik IP adreslerinden biridir. İstemci ile karşılıklı IP aralığını avere Denetim Masası 'nda iki yerde bulabilirsiniz:

* **Vservers** tablosu (Pano sekmesi)-

  ![Grafiğin altındaki veri tablosunda seçili olan VServer sekmesi ve IP adresi bölümü daire içinde olan avere Denetim Masası Pano sekmesi](media/avere-vfxt-ip-addresses-dashboard.png)

* **Istemciye yönelik ağ** ayarları sayfası-

  ![Belirli bir vServer için tablonun adres aralığı bölümünün etrafında bir daire olan sanal sunucu > Istemciye yönelik ağ yapılandırması sayfası > ayarlar](media/avere-vfxt-ip-addresses-settings.png)

Yollara ek olarak, her bir istemciyi bağlamak için aşağıda açıklanan [bağlama komutu bağımsız değişkenlerini](#mount-command-arguments) ekleyin.

### <a name="mount-command-arguments"></a>Bağlama komutu bağımsız değişkenleri

Sorunsuz bir istemci bağlama sağlamak için, bu ayarları ve bağımsız değişkenleri bağlama komutunuz geçirin:

``mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Gerekli ayarlar | |
--- | ---
``hard`` | VFXT kümesine yönelik hafif bağlar, uygulama hatalarıyla ve olası veri kaybı ile ilişkilendirilir.
``proto=netid`` | Bu seçenek NFS ağ hatalarının uygun işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, bağlama işlemleri için ağ hatalarının uygun işlenmesini destekler.
``retry=n`` | Geçici ``retry=30`` bağlama hatalarından kaçınmak için ayarlayın. (Ön plan takmaları farklı bir değer önerilir.)

## <a name="next-steps"></a>Sonraki adımlar

İstemcileri bağlandıktan sonra, verileri kümenizdeki yeni bir BLOB depolama kapsayıcısına kopyalamak için kullanabilirsiniz. Yeni depolama alanını doldurmanız gerekmiyorsa, ek kurulum görevleri hakkında bilgi edinmek için diğer bağlantıları okuyun:

* [Verileri bir küme çekirdeği Içine taşıma](avere-vfxt-data-ingest.md) -birden çok istemci ve iş parçacığını kullanarak verilerinizi verimli bir şekilde yeni bir çekirdek filme 'ya yükleme
* [Küme ayarlamayı özelleştirme](avere-vfxt-tuning.md) -küme ayarlarını iş yükünüze uyacak şekilde uyarlayın
* [Kümeyi yönetme](avere-vfxt-manage-cluster.md) -kümeyi başlatma veya durdurma ve düğümleri yönetme
