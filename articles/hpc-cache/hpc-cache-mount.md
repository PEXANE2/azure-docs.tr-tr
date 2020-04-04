---
title: Azure HPC Önbelleğini Takma
description: İstemcileri Azure HPC Önbellek hizmetine bağlama
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: rohogue
ms.openlocfilehash: f176e30cfaf9a52e4f58091b7fc76098a4c88a48
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657375"
---
# <a name="mount-the-azure-hpc-cache"></a>Azure HPC Önbelleğini Dağın

Önbellek oluşturulduktan sonra, NFS istemcileri basit `mount` bir komutla erişebilir. Komut, Azure HPC Önbelleğindeki belirli bir depolama hedef yolunu istemci makinesindeki yerel bir dizine bağlar.

Montaj komutu şu öğelerden oluşur:

* Önbelleğin montaj adreslerinden biri (önbelleğe genel bakış sayfasında listelenir)
* Depolama hedefini oluşturduğunuzda ayarladığınız sanal ad alanı yolu
* İstemcide kullanılacak yerel yol
* Bu tür NFS montajının başarısını optimize eden komut parametreleri

Önbelleğiniz için **Mount yönergeleri** sayfası sizin için bilgileri ve önerilen seçenekleri toplar ve kopyalayabileceğiniz bir prototip montaj komutu oluşturur. Ayrıntılar için [montaj talimatları yardımcı programını kullanın.](#use-the-mount-instructions-utility)

## <a name="prepare-clients"></a>İstemcileri hazırlama

Müşterilerinizin bu bölümdeki yönergeleri izleyerek Azure HPC Önbelleğini monte edebilmelerini unutmayın.

### <a name="provide-network-access"></a>Ağ erişimi sağlama

İstemci makineleri önbelleğin sanal ağına ve özel alt ağına ağ erişimine sahip olmalıdır.

Örneğin, aynı sanal ağ içinde istemci VM'leri oluşturun veya dışarıdan erişmek için sanal ağda bir bitiş noktası, ağ geçidi veya başka bir çözüm kullanın. (Önbelleğin kendisinden başka hiçbir şeyin önbelleğin alt ağı içinde barındırılması gerektiğini unutmayın.)

### <a name="install-utilities"></a>Yardımcı programları yükleme

NFS montaj komutunu desteklemek için uygun Linux yardımcı programı yazılımını yükleyin:

* Red Hat Enterprise Linux veya SuSE için:`sudo yum install -y nfs-utils`
* Ubuntu veya Debian için:`sudo apt-get install nfs-common`

### <a name="create-a-local-path"></a>Yerel bir yol oluşturma

Önbelleğe bağlanmak için her istemcide yerel bir dizin yolu oluşturun. Monte etmek istediğiniz her depolama hedefi için bir yol oluşturun.

Örnek: `sudo mkdir -p /mnt/hpc-cache-1/target3`

## <a name="use-the-mount-instructions-utility"></a>Montaj talimatları yardımcı programını kullanma

Azure portalındaki önbellek görünümünün **Yapılandırma** bölümünden **Yönergeleri Dağı'nı** açın.

![Portaldaki bir Azure HPC Önbelleği örneğinin ekran görüntüsü, Yapılandırma > Montaj yönergeleri sayfası yüklü](media/mount-instructions.png)

Montaj komutu sayfası, istemci montaj işlemi ve ön koşullar hakkında bilgi ve kopyalanabilir bir montaj komutu oluşturmak için kullanabileceğiniz alanları içerir.

Bu sayfayı kullanmak için aşağıdaki yordamı izleyin:

<!--1.  In step one of **Mounting your file system**, enter the path that the client will use to access the Azure HPC Cache storage target.

   * This path is local to the client.
   * After you provide the directory name, the field populates with a command you can copy. Use this command on the client directly or in a setup script to create the directory path on the client VM. -->

1. İstemci önkoşulları gözden geçirin ve yukarıda `mount` [hazırla istemcilerde](#prepare-clients)açıklandığı gibi NFS komutunu kullanmak için gereken yardımcı programları yükleyin.

1. **Dosya sisteminizi monte** etmek için birinci adım<!-- label will change --> istemcide yerel yol oluşturmak için örnek bir komut verir. Bu, istemcinin Azure HPC Önbelleğinden içeriğe erişmek için kullanacağı yoldur.

   Gerekirse komutta değiştirebilmeniz için yol adını not edin.

1. İkinci adımda, kullanılabilir IP adreslerinden birini seçin. Önbelleğin istemci [montaj noktalarının](#find-mount-command-components) tümü burada listelenmiştir. Tüm IP adresleri arasında yükü dengeleyen bir sisteminiz olduğundan emin olun.

1. Üçüncü adımdaki alan otomatik olarak bir prototip montaj komutuyla doldurulur. Otomatik olarak panonuza kopyalamak için alanın sağ tarafındaki kopya simgesini tıklatın.

   > [!NOTE]
   > Kullanmadan önce kopyalama komutunu kontrol edin. İstemci montaj yolunu ve depolama hedefi sanal ad alanı yolunu özelleştirmeniz gerekebilir, ki bu arabirimde henüz seçilemez. Ayrıca, aşağıda [önerilen seçenekleri](#mount-command-options) yansıtacak şekilde montaj komutseçeneklerini güncelleştirmeniz gerekir. Yardım için [mount komut sözdizimini anlayın](#understand-mount-command-syntax) okuyun.

1. Azure HPC Önbelleğindeki depolama hedefine bağlamak için istemci makinesindeki kopyalanan montaj komutunu (gerekirse düzenlemeyle) kullanın. Komutu doğrudan istemci komut satırından verebilir veya montaj komutunu istemci kurulum komut dosyasına veya şablonuna ekleyebilirsiniz.

## <a name="understand-mount-command-syntax"></a>Montaj komutu sözdizimini anlama

Montaj komutu aşağıdaki forma sahiptir:

> sudo mount *cache_mount_address*:/*namespace_path* *local_path* {*seçenekler*}

Örnek:

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -o hard,proto=tcp,mountproto=tcp,retry=30
root@test-client:/tmp#
```

Bu komut başarılı olduktan sonra, depolama dışa aktarAn ların içeriği istemcideki dizinde ``hpccache`` görünür olacaktır.

### <a name="mount-command-options"></a>Montaj komut seçenekleri

Sağlam bir istemci yuvası için, montaj komutunuzda bu ayarları ve bağımsız değişkenleri geçirin:

> montaj -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}

| Önerilen montaj komutu ayarları | |
--- | ---
``hard`` | Azure HPC Önbelleğine yumuşak bağlar uygulama hataları ve olası veri kaybıyla ilişkilidir.
``proto=tcp`` | Bu seçenek, NFS ağ hatalarının uygun şekilde işlenmesini destekler.
``mountproto=tcp`` | Bu seçenek, montaj işlemleri için ağ hatalarının uygun şekilde işlenmesini destekler.
``retry=<value>`` | Geçici ``retry=30`` montaj hatalarını önlemek için ayarlayın. (Ön plandaki bağlarda farklı bir değer önerilir.)

### <a name="find-mount-command-components"></a>Montaj komut bileşenlerini bulma

**Bağlı yönergeleri** sayfasını kullanmadan bir montaj komutu oluşturmak istiyorsanız, önbelleğe **Genel Bakış** sayfasında montaj adreslerini ve Depolama **hedefleri** sayfasında sanal ad alanı yollarını bulabilirsiniz.

![Sağ alttaki montaj adresleri listesinin etrafında bir vurgu kutusu olan Azure HPC Önbellek örneğinin Genel Bakış sayfasının ekran görüntüsü](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Önbellek montaj adresleri, önbelleğin alt ağındaki ağ arabirimlerine karşılık gelir. Bir kaynak grubunda, bu NIC'ler biten `-cluster-nic-` adlar ve bir sayı ile listelenir. Bu arabirimleri değiştirmeyin veya silmeyin, yoksa önbellek kullanılamaz hale gelir.

Sanal ad alanı yolları **Depolama hedefleri** sayfasında gösterilir. Ayrıntılarıyla ilişkili toplu ad alanı yolları da dahil olmak üzere ayrıntılarını görmek için tek bir depolama hedef adını tıklatın.

![önbelleğin Depolama hedef panelinin ekran görüntüsü, tablonun Yol sütunundaki bir girişin etrafında bir vurgu kutusu yla](media/hpc-cache-view-namespace-paths.png)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri önbelleğin depolama hedeflerine taşımak [için, yeni Azure Blob depolama alanını doldur'u](hpc-cache-ingest.md)okuyun.
