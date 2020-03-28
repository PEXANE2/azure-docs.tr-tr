---
title: Microsoft Azure FXT Edge Filer kümesinde istemcileri monte etme
description: NFS istemci makineleri Azure FXT Edge Filer hibrit depolama önbelleğini nasıl monte edebilir?
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: rohogue
ms.openlocfilehash: 43223db298e4ad170ea6d0687a342b3aee35500e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80130761"
---
# <a name="tutorial-mount-the-cluster"></a>Öğretici: Kümeyi monte edin

Bu öğretici, NFS istemcilerini Azure FXT Edge Filer kümesine nasıl monte edebilirsiniz. İstemciler, arka uç depolama alanı eklediğinizde atadığınız sanal ad alanı yollarını bağlar.

Bu öğretici öğretir:

> [!div class="checklist"]
> * İstemciye dönük IP adresleri aralığında yük dengeleme istemcileri için stratejiler
> * İstemciye bakan IP adresi ve ad alanı bağlantısından montaj yolu oluşturma
> * Montaj komutunda kullanılacak bağımsız değişkenler

Bu eğitimin tamamlanması yaklaşık 45 dakika sürer.

## <a name="steps-to-mount-the-cluster"></a>Kümeye monte etme adımları

İstemci makinelerini Azure FXT Edge Filer kümenize bağlamak için aşağıdaki adımları izleyin.

1. İstemci trafiğini küme düğümleriniz arasında nasıl dengeleeceğine karar verin. Ayrıntılar için aşağıdaki [Bakiye istemci yükünü](#balance-client-load)okuyun.
1. Küme IP adresini ve monte etmek için kavşak yolunu tanımlayın.
1. Montaj için istemciye bakan yolu belirleyin.
1. Montaj [komutunu](#use-recommended-mount-command-options)uygun bağımsız değişkenlerle sorun.

## <a name="balance-client-load"></a>Denge istemci yükü

İstemci isteklerini kümedeki tüm düğümler arasında dengelemeye yardımcı olmak için, istemcileri istemciye bakan tüm IP adresleri aralığına monte etmelisiniz. Bu görevi otomatikleştirmenin birkaç yolu vardır.

Küme için round-robin DNS yük dengeleme hakkında bilgi edinmek için [Azure FXT Edge Filer kümesi için DNS'yi Yapılandır'ı](fxt-configure-network.md#configure-dns-for-load-balancing)okuyun. Bu yöntemi kullanmak için, bu makalelerde açıklanmayan bir DNS sunucusu tutmanız gerekir.

Küçük yüklemeler için daha basit bir yöntem, istemci montaj zamanında aralık boyunca IP adresleri atamak için bir komut dosyası kullanmaktır.

Diğer yük dengeleme yöntemleri büyük veya karmaşık sistemler için uygun olabilir. Microsoft temsilcinize danışın veya yardım için bir [destek isteği](fxt-support-ticket.md) açın. (Azure Yük Dengeleyicişu anda Azure FXT Edge Filer ile *desteklenmez.)*

## <a name="create-the-mount-command"></a>Montaj komutunu oluşturma

Komut, ``mount`` istemcinizden Azure FXT Edge Filer kümesindeki sanal sunucuyu (vserver) yerel dosya sistemindeki bir yola eşler.

Biçim,``mount <FXT cluster path> <local path> {options}``

Montaj komutu için üç öğe vardır:

* küme yolu - IP adresi ve namespace kavşak yolunun bir leşimi aşağıda açıklanan
* yerel yol - istemci üzerinde yol
* montaj komut seçenekleri - [(Kullanım önerilen montaj komut seçenekleri](#use-recommended-mount-command-options)listelenen)

### <a name="create-the-cluster-path"></a>Küme yolunu oluşturma

Küme yolu vserver *IP adresi* artı bir ad *alanı kavşak*yolu bir arada. Ad alanı bağlantısı, [depolama sistemini eklediğinizde](fxt-add-storage.md#create-a-junction)tanımladığınız sanal bir yoldur.

Örneğin, ad alanı ``/fxt/files`` yolunuz olarak kullandıysanız, müşterileriniz yerel montaj noktasına *IP_address*:/fxt/files monte eder.

![Ad alanı yol alanında /avere/files içeren "yeni bağlantı ekle" iletişim kutusu](media/fxt-mount/fxt-junction-example.png)

IP adresi, vserver için tanımlanan istemciye bakan IP adreslerinden biridir. İstemcilere bakan IP aralığını küme Denetim Masası'nda iki yerde bulabilirsiniz:

* **VServers** tablosu (Pano sekmesi) -

  ![Grafiğin altındaki veri tablosunda vserver sekmesi seçilen Kontrol Paneli'nin pano sekmesi ve IP adresi bölümü daire içine alınmış](media/fxt-mount/fxt-ip-addresses-dashboard.png)

* **İstemci Karşı Karşıya Ağ** ayarları sayfası -

  ![Belirli bir vserver için tablonun Adres Aralığı bölümü etrafında bir daire ile VServer > İstemci Bakan Ağ yapılandırma sayfası > Ayarlar](media/fxt-mount/fxt-ip-addresses-settings.png)

IP adresini ve ad alanı yolunu birleştirerek montaj komutu için küme yolunu oluşturabilirsiniz.

Örnek istemci montaj komutu:``mount 10.0.0.12:/sd-access /mnt/fxt {options}``

### <a name="create-the-local-path"></a>Yerel yolu oluşturma

Montaj komutu için yerel yol size kalmış. Sanal ad alanının bir parçası olarak istediğiniz yol yapısını ayarlayabilirsiniz. İstemcinizin iş akışı için uygun olan bir ad alanı ve yerel yol tasarla.

İstemcilere bakan ad alanı hakkında daha fazla bilgi için Küme Yapılandırma Kılavuzu'nun [ad alanına genel bakışı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html)okuyun.

Yollara ek olarak, her istemciyi monte ederken aşağıda açıklanan [montaj komut seçeneklerini](#use-recommended-mount-command-options) de ekleyin.

### <a name="use-recommended-mount-command-options"></a>Önerilen montaj komutu seçeneklerini kullanma

Sorunsuz bir istemci montajı sağlamak için, montaj komutunuzda bu ayarları ve bağımsız değişkenleri geçirin:

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${VSERVER_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Gerekli ayarlar | |
--- | ---
``hard`` | Azure FXT Edge Filer kümesine yumuşak bağlar uygulama hataları ve olası veri kaybıyla ilişkilidir.
``proto=netid`` | Bu seçenek, NFS ağ hatalarının uygun şekilde işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, montaj işlemleri için ağ hatalarının uygun şekilde işlenmesini destekler.
``retry=n`` | Geçici ``retry=30`` montaj hatalarını önlemek için ayarlayın. (Ön plandaki bağlarda farklı bir değer önerilir.)

| Tercih edilen ayarlar  | |
--- | ---
``nointr``            | Müşterileriniz bu seçeneği destekleyen eski işletim sistemi çekirdekleri kullanıyorsa (Nisan 2008'den önce), bunu kullanın. "Intr" seçeneği varsayılandır.

## <a name="next-steps"></a>Sonraki adımlar

İstemcilere monte ettikten sonra, iş akışınızı sınayabilir ve kümenize devam edebilirsiniz.

Verileri yeni bir bulut çekirdeği filer'ine taşımanız gerekiyorsa, paralel veri yutma kullanarak önbellek yapısından yararlanın. Bazı stratejiler [vFXT kümesine veri taşıma](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-data-ingest)açıklanır. (Azure için Avere vFXT, Azure FXT Edge Filer'a çok benzeyen önbelleğe alma teknolojisini kullanan bulut tabanlı bir üründür.)

Donanım sorunlarını gidermeniz gerekiyorsa [Azure FXT Edge Filer donanım durumunu okuyun.](fxt-monitor.md)
