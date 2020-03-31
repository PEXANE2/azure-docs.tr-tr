---
title: Avere vFXT Dağı - Azure
description: Azure için Avere vFXT ile istemcilere nasıl monte edilir?
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: rohogue
ms.openlocfilehash: b8486b5a33226b1faa5e3874144129dbe7a1a2f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153420"
---
# <a name="mount-the-avere-vfxt-cluster"></a>Avere vFXT kümesini takma

İstemci makinelerini vFXT kümenize bağlamak için aşağıdaki adımları izleyin.

1. İstemci trafiğini küme düğümleriniz arasında nasıl dengeleeceğine karar verin. Ayrıntılar için aşağıdaki [Bakiye istemci yükünü](#balance-client-load)okuyun.
1. Ip adresini ve monte etmek için kavşak yolunu tanımlayın.
1. Montaj [komutunu](#mount-command-arguments)uygun bağımsız değişkenlerle sorun.

## <a name="balance-client-load"></a>Denge istemci yükü

İstemci isteklerini kümedeki tüm düğümler arasında dengelemeye yardımcı olmak için, istemcileri istemciye bakan tüm IP adresleri aralığına monte etmelisiniz. Bu görevi otomatikleştirmenin birkaç basit yolu vardır.

> [!TIP]
> Diğer yük dengeleme yöntemleri büyük veya karmaşık sistemler için uygun olabilir; yardım için [bir destek bileti açın.)](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt)
>
> Otomatik sunucu tarafı yük dengelemesi için bir DNS sunucusu kullanmayı tercih ediyorsanız, Azure içinde kendi DNS sunucunuzu kurmanız ve yönetmeniz gerekir. Bu durumda, vFXT kümesi için yuvarlanma DNS'yi bu belgeye göre yapılandırabilirsiniz: [Avere cluster DNS yapılandırması.](avere-vfxt-configure-dns.md)

### <a name="sample-balanced-client-mounting-script"></a>Örnek dengeli istemci montaj komut dosyası

Bu kod örneği, istemcileri vFXT kümesinin kullanılabilir IP adreslerinin tümüne dağıtmak için istemci IP adreslerini rasgele leştirme öğesi olarak kullanır.

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

Yukarıdaki işlev, [Avere vFXT örnekleri](https://github.com/Azure/Avere#tutorials) sitesinde bulunan Toplu İşlem örneğinin bir parçasıdır.

## <a name="create-the-mount-command"></a>Montaj komutunu oluşturma

> [!NOTE]
> Avere vFXT kümenizi oluştururken yeni bir Blob kapsayıcısı oluşturmadıysanız, istemcileri monte etmeye çalışmadan önce [Depolamayı Yapıla'da](avere-vfxt-add-storage.md) açıklandığı gibi depolama sistemleri ekleyin.

Komut, ``mount`` istemcinizden vFXT kümesindeki sanal sunucuyu (vserver) yerel dosya sistemindeki bir yola eşler. Biçim,``mount <vFXT path> <local path> {options}``

Montaj komutu üç öğeden oluşur:

* vFXT yolu - aşağıda açıklanan küme üzerinde bir IP adresi ve namespace kavşak yolu bir arada)
* yerel yol - istemci üzerinde yol
* montaj komut seçenekleri - [Mount komut bağımsız değişkenlerinde](#mount-command-arguments) listelenmiştir

### <a name="junction-and-ip"></a>Kavşak ve IP

Vserver yolu *IP adresi* artı bir ad *alanı kavşak*yolu bir arada. Ad alanı bağlantısı, depolama sistemi eklendiğinde tanımlanan sanal bir yoldur.

Kümeniz Blob depolama alanı yla oluşturulduysa, bu kapsayıcının ad alanı yolu`/msazure`

Örnek: ``mount 10.0.0.12:/msazure /mnt/vfxt``

Kümeyi oluşturduktan sonra depolama alanı eklediyseniz, ad alanı bağlantı yolu, bağlantı noktası oluştururken **Ad Alanı yolunda** ayarladığınız değerdir. Örneğin, ad alanı ``/avere/files`` yolunuz olarak kullandıysanız, istemcileriniz yerel montaj noktasına *IP_address*:/avere/files monte eder.

![Ad alanı yol alanında /avere/files içeren "yeni bağlantı ekle" iletişim kutusu](media/avere-vfxt-create-junction-example.png) <!-- to do - change example and screenshot to vfxt/files instead of avere -->

IP adresi, vserver için tanımlanan istemciye bakan IP adreslerinden biridir. Müşteriye yönelik IP'lerin yelpazesini Avere Kontrol Paneli'nde iki yerde bulabilirsiniz:

* **VServers** tablosu (Pano sekmesi) -

  ![Grafiğin altındaki veri tablosunda seçilen VServer sekmesi ile Avere Kontrol Paneli'nin pano sekmesi ve IP adresi bölümü daire içine alınmış](media/avere-vfxt-ip-addresses-dashboard.png)

* **İstemci Karşı Karşıya Ağ** ayarları sayfası -

  ![Belirli bir vserver için tablonun Adres Aralığı bölümü etrafında bir daire ile VServer > İstemci Bakan Ağ yapılandırma sayfası > Ayarlar](media/avere-vfxt-ip-addresses-settings.png)

Yollara ek olarak, her istemciyi monte ederken aşağıda açıklanan [Mount komut argümanlarını](#mount-command-arguments) ekleyin.

### <a name="mount-command-arguments"></a>Montaj komutu bağımsız değişkenleri

Sorunsuz bir istemci montajı sağlamak için, montaj komutunuzda bu ayarları ve bağımsız değişkenleri geçirin:

``mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Gerekli ayarlar | |
--- | ---
``hard`` | vFXT kümesine yumuşak bağlar uygulama hataları ve olası veri kaybı ile ilişkilidir.
``proto=netid`` | Bu seçenek, NFS ağ hatalarının uygun şekilde işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, montaj işlemleri için ağ hatalarının uygun şekilde işlenmesini destekler.
``retry=n`` | Geçici ``retry=30`` montaj hatalarını önlemek için ayarlayın. (Ön plandaki bağlarda farklı bir değer önerilir.)

## <a name="next-steps"></a>Sonraki adımlar

İstemlerinizi monte ettikten sonra, verileri kümenizdeki yeni bir Blob depolama kapsayıcısına kopyalamak için bunları kullanabilirsiniz. Yeni depolama alanı doldurmanız gerekmiyorsa, ek kurulum görevleri hakkında bilgi edinmek için diğer bağlantıları okuyun:

* [Verileri küme çekirdeği filer'ine taşıma](avere-vfxt-data-ingest.md) - Verilerinizi yeni bir çekirdek filer'e verimli bir şekilde yüklemek için birden çok istemci ve iş parçacığı nasıl kullanılır?
* [Küme ayarını özelleştir](avere-vfxt-tuning.md) - Küme ayarlarını iş yükünüze uyacak şekilde özelleştirin
* [Kümeyi yönetme](avere-vfxt-manage-cluster.md) - Kümeyi başlatma veya durdurma ve düğümleri yönetme
