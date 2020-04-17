---
title: Azure HPC Önbelleğini Takma
description: İstemcileri Azure HPC Önbellek hizmetine bağlama
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: a44232f06b455e20530271723e816c2117b339a0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458421"
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

Önbelleğe bağlanmak için her istemcide yerel bir dizin yolu oluşturun. Monte etmek istediğiniz her ad alanı yolu için bir yol oluşturun.

Örnek: `sudo mkdir -p /mnt/hpc-cache-1/target3`

Azure portalındaki [Montaj yönergeleri](#use-the-mount-instructions-utility) sayfası, kopyalayabileceğiniz bir prototip komutu içerir.

İstemci makinesini önbelleğe bağladığınızda, bu yolu depolama hedef dışa aktarmayı temsil eden sanal bir ad alanı yolu ile ilişkilendireceksiniz. İstemcinin kullanacağı sanal ad alanı yollarının her biri için dizinler oluşturun.

## <a name="use-the-mount-instructions-utility"></a>Montaj talimatları yardımcı programını kullanma

Kopyalanabilir bir montaj komutu oluşturmak için Azure portalındaki **Montaj yönergeleri** sayfasını kullanabilirsiniz. Sayfayı portaldaki önbellek görünümünün **Yapıla** bölümünden açın.

Bir istemci üzerinde komutu kullanmadan önce, istemci ön koşulları karşıladığından emin olun ve `mount` yazılım yukarıda açıklandığı gibi NFS komutunu kullanmak için gerekli [olan hazır istemciler](#prepare-clients).

![Portaldaki bir Azure HPC Önbelleği örneğinin ekran görüntüsü, Yapılandırma > Montaj yönergeleri sayfası yüklü](media/mount-instructions.png)

Montaj komutunu oluşturmak için bu yordamı izleyin.

1. **İstemci yol** alanını özelleştirin. Bu alan, istemcide yerel bir yol oluşturmak için kullanabileceğiniz bir örnek komut verir. İstemci bu dizindeki Azure HPC Önbelleğinden içeriğe yerel olarak eriştir.

   Alanı tıklatın ve istediğiniz dizin adını içerecek şekilde komutu edin. Ad, dize sonunda görünür sonra`sudo mkdir -p`

   ![sonunda konumlandırılmış imleç ile istemci yol alanının ekran görüntüsü](media/mount-edit-client.png)

   Alanı düzenlemeyi bitirdikten sonra, sayfanın altındaki montaj komutu yeni istemci yolu ile güncellenir.

1. Listeden **Önbellek montaj adresini** seçin. Bu menü, önbelleğin [istemci montaj noktalarının](#find-mount-command-components)tümünün listesini listeler.

   Daha iyi önbellek performansı için kullanılabilir montaj adreslerinin tümüne istemci yükünü dengeleyin.

   ![seçim için üç IP adresi gösteren seçici ile önbellek montaj adresi alanının ekran görüntüsü](media/mount-select-ip.png)

1. İstemci için kullanılacak **Sanal ad alanı yolunu** seçin. Bu yollar arka uç depolama sistemindedışada dışa bağlanır.

   ![seçici açık olan ad alanı yolları alanının ekran görüntüsü](media/mount-select-target.png)

   Depolama hedefleri portalı sayfasında sanal ad alanı yollarını görüntüleyebilir ve değiştirebilirsiniz. Nasıl yapılacağını görmek için [depolama hedefleri ekle'yi](hpc-cache-add-storage.md) okuyun.

   Azure HPC Önbelleği'nin toplu ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplu ad alanını planla'yı](hpc-cache-namespace.md)okuyun.

1. Üçüncü adımdaki **Mount komut** alanı, önceki alanlarda ayarladığınız montaj adresini, sanal ad alanı yolunu ve istemci yolunu kullanan özelleştirilmiş bir montaj komutuyla otomatik olarak doldurulur.

   Otomatik olarak panonuza kopyalamak için alanın sağ tarafındaki kopya simgesini tıklatın.

   ![seçici açık olan ad alanı yolları alanının ekran görüntüsü](media/mount-command-copy.png)

1. Azure HPC Önbelleğine bağlamak için istemci makinesindeki kopyalanan montaj komutunu kullanın. Komutu doğrudan istemci komut satırından verebilir veya montaj komutunu istemci kurulum komut dosyasına veya şablonuna ekleyebilirsiniz.

## <a name="understand-mount-command-syntax"></a>Montaj komutu sözdizimini anlama

Montaj komutu aşağıdaki forma sahiptir:

> sudo mount {*options*} *cache_mount_address*:/*namespace_path* *local_path*

Örnek:

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount -o hard,proto=tcp,mountproto=tcp,retry=30 10.0.0.28:/blob-demo-0722 hpccache
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

**Bağlı yönergeleri** sayfasını kullanmadan bir montaj komutu oluşturmak istiyorsanız, önbelleğe **Genel Bakış** sayfasında montaj adreslerini ve Depolama **hedef** sayfasında sanal ad alanı yollarını bulabilirsiniz.

![Sağ alttaki montaj adresleri listesinin etrafında bir vurgu kutusu olan Azure HPC Önbellek örneğinin Genel Bakış sayfasının ekran görüntüsü](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Önbellek montaj adresleri, önbelleğin alt ağındaki ağ arabirimlerine karşılık gelir. Bir kaynak grubunda, bu NIC'ler biten `-cluster-nic-` adlar ve bir sayı ile listelenir. Bu arabirimleri değiştirmeyin veya silmeyin, yoksa önbellek kullanılamaz hale gelir.

Sanal ad alanı yolları her depolama hedefinin ayrıntılar sayfasında gösterilir. Ayrıntılarıyla ilişkili toplu ad alanı yolları da dahil olmak üzere ayrıntılarını görmek için tek bir depolama hedef adını tıklatın.

![depolama hedefinin ayrıntı sayfasının ekran görüntüsü (üstbilgi "Depolama hedefini güncelleştir"). Tablonun Sanal ad alanı yolu sütununda bir girişin etrafında bir vurgu kutusu vardır](media/hpc-cache-view-namespace-paths.png)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri önbelleğin depolama hedeflerine taşımak [için, yeni Azure Blob depolama alanını doldur'u](hpc-cache-ingest.md)okuyun.
