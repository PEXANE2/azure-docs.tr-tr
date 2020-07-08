---
title: Azure HPC önbelleği bağlama
description: İstemcileri Azure HPC önbellek hizmetine bağlama
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: fb3e4fd4935afc4869e50ccbc35c53333d43b1df
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515526"
---
# <a name="mount-the-azure-hpc-cache"></a>Azure HPC Önbelleği’ni bağlama

Önbellek oluşturulduktan sonra, NFS istemcileri basit bir `mount` komutla erişebilir. Komutu, Azure HPC önbelleğindeki belirli bir depolama hedef yolunu, istemci makinesindeki bir yerel dizine bağlar.

Bağlama komutu şu öğelerden oluşur:

* Önbelleğin bağlama adreslerinden biri (önbelleğe Genel Bakış sayfasında listelenir)
* Depolama hedefini oluştururken ayarladığınız sanal ad alanı yolu
* İstemcide kullanılacak yerel yol
* Bu tür NFS bağlamasının başarısını en iyileştiren komut parametreleri

Önbelleğiniz için **bağlama yönergeleri** sayfası, sizin için bilgileri ve önerilen seçenekleri toplar ve kopyalayacağınız bir prototip bağlama komutu oluşturur. Ayrıntılar için [bağlama yönergeleri yardımcı programını kullanın](#use-the-mount-instructions-utility) .

## <a name="prepare-clients"></a>İstemcileri hazırlama

Bu bölümdeki yönergeleri izleyerek istemcilerinizin Azure HPC önbelleğini bağlayabilmesini sağlayın.

### <a name="provide-network-access"></a>Ağ erişimi sağla

İstemci makinelerin, önbelleğin sanal ağına ve özel alt ağına ağ erişiminin olması gerekir.

Örneğin, aynı sanal ağ içinde istemci VM 'Leri oluşturun veya dışarıdaki bir erişim için bir uç nokta, ağ geçidi veya sanal ağda başka bir çözüm kullanın. (Önbelleğin kendisi dışında hiçbir şeyin, önbelleğin alt ağı içinde barındırılıp barındırılmadığını unutmayın.)

### <a name="install-utilities"></a>Yardımcı programları yükler

NFS bağlama komutunu desteklemek için uygun Linux yardımcı programı yazılımını yüklemek:

* Red Hat Enterprise Linux veya SuSE için:`sudo yum install -y nfs-utils`
* Ubuntu veya de, için:`sudo apt-get install nfs-common`

### <a name="create-a-local-path"></a>Yerel yol oluştur

Her istemcide önbelleğe bağlanacak bir yerel dizin yolu oluşturun. Bağlamak istediğiniz her bir ad alanı yolu için bir yol oluşturun.

Örnek: `sudo mkdir -p /mnt/hpc-cache-1/target3`

Azure portal [bağlama yönergeleri](#use-the-mount-instructions-utility) sayfasında, kopyalayacağınız bir prototip komutu bulunur.

İstemci makinesini önbelleğe bağladığınızda, bu yolu bir depolama hedefi dışarı aktarmayı temsil eden bir sanal ad alanı yoluyla ilişkilendirirsiniz. İstemcinin kullanacağı sanal ad alanı yollarının her biri için dizinler oluşturun.

## <a name="use-the-mount-instructions-utility"></a>Bağlama yönergeleri yardımcı programını kullanma

Kopyalanabilir mount komutu oluşturmak için Azure portal içindeki **bağlama yönergeleri** sayfasını kullanabilirsiniz. Portalda önbellek görünümünün **Yapılandır** bölümünden sayfayı açın.

Komutunu bir istemcide kullanmadan önce, istemcinin önkoşulları karşıladığından ve `mount` [istemcileri hazırlama](#prepare-clients)bölümünde yukarıda açıklanan şekilde NFS komutunu kullanması için gereken yazılıma sahip olduğundan emin olun.

![Portalda > bağlama yönergelerini yapılandır sayfası yüklü olan bir Azure HPC önbellek örneğinin ekran görüntüsü](media/mount-instructions.png)

Bağlama komutunu oluşturmak için bu yordamı izleyin.

1. **İstemci yolu** alanını özelleştirin. Bu alan, istemcide yerel bir yol oluşturmak için kullanabileceğiniz örnek bir komut verir. İstemci, Azure HPC önbelleğinden bu dizindeki içeriğe yerel olarak erişir.

   Alana tıklayın ve komutu istediğiniz dizin adını içerecek şekilde düzenleyin. Ad, dizenin sonunda daha sonra görüntülenir`sudo mkdir -p`

   ![imleç sonda konumlandırılmış olan istemci yolu alanının ekran görüntüsü](media/mount-edit-client.png)

   Alanı düzenledikten sonra, sayfanın altındaki mount komutu yeni istemci yoluyla güncelleştirilir.

1. Listeden **önbellek bağlama adresini** seçin. Bu menü, tüm önbelleğin [istemci bağlama noktalarını](#find-mount-command-components)listeler.

   Daha iyi önbellek performansı için tüm kullanılabilir Mount adreslerinde istemci yükünü dengeleyin.

   ![arasından seçim yapabileceğiniz üç IP adresini gösteren seçiciyle önbellek bağlama adresi alanının ekran görüntüsü](media/mount-select-ip.png)

1. İstemci için kullanılacak **sanal ad alanı yolunu** seçin. Bu yollar arka uç depolama sisteminde dışarı aktarmalar için bağlantı sağlar.

   ![Seçici açık olan ad alanı yolları alanının ekran görüntüsü](media/mount-select-target.png)

   Sanal ad alanı yollarını depolama hedefleri portalı sayfasında görüntüleyebilir ve değiştirebilirsiniz. Nasıl yapılacağını görmek için [depolama hedefleri ekleme](hpc-cache-add-storage.md) bölümünü okuyun.

   Azure HPC önbelleğinin toplanmış ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını planlayın](hpc-cache-namespace.md).

1. Üçüncü adımda bulunan **mount komutu** alanı, önceki alanlarda ayarladığınız bağlama adresini, sanal ad alanı yolunu ve istemci yolunu kullanan özelleştirilmiş bir Mount komutuyla otomatik olarak doldurulur.

   Pano 'ya otomatik olarak kopyalamak için alanın sağ tarafındaki kopyalama simgesine tıklayın.

   ![Seçici açık olan ad alanı yolları alanının ekran görüntüsü](media/mount-command-copy.png)

1. Azure HPC önbelleğine bağlamak için istemci makinedeki kopyalanmış Mount komutunu kullanın. Komutu doğrudan istemci komut satırından verebilir veya bir istemci kurulum komut dosyasına veya şablonuna Mount komutunu ekleyebilirsiniz.

## <a name="understand-mount-command-syntax"></a>Mount komut söz dizimini anlayın

Mount komutu aşağıdaki biçimdedir:

> sudo bağlama {*Options*} *cache_mount_address*:/*namespace_path* *local_path*

Örnek:

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount -o hard,proto=tcp,mountproto=tcp,retry=30 10.0.0.28:/blob-demo-0722 hpccache
root@test-client:/tmp#
```

Bu komut başarılı olduktan sonra, depolama dışarı aktarmanın içerikleri ``hpccache`` istemcideki dizinde görünür olur.

### <a name="mount-command-options"></a>Bağlama komutu seçenekleri

Sağlam bir istemci bağlama için, bu ayarları ve bağımsız değişkenleri bağlama komutunuz geçirin:

> Mount-o hard, Proto = TCP, bağlamaproto = TCP, yeniden deneme = 30 $ {CACHE_IP_ADDRESS}:/$ {NAMESPACE_PATH} $ {LOCAL_FILESYSTEM_MOUNT_POINT}

| Önerilen bağlama komutu ayarları | |
--- | ---
``hard`` | Azure HPC Cache 'e yönelik hafif bağlar, uygulama hatalarıyla ve olası veri kaybı ile ilişkilendirilir.
``proto=tcp`` | Bu seçenek NFS ağ hatalarının uygun işlenmesini destekler.
``mountproto=tcp`` | Bu seçenek, bağlama işlemleri için ağ hatalarının uygun işlenmesini destekler.
``retry=<value>`` | ``retry=30``Geçici bağlama hatalarından kaçınmak için ayarlayın. (Ön plan takmaları farklı bir değer önerilir.)

### <a name="find-mount-command-components"></a>Bağlama komutu bileşenlerini bul

Bağlama **yönergeleri** sayfasını kullanmadan bir bağlama komutu oluşturmak isterseniz, bağlama adreslerini önbelleğe **genel bakış** sayfasında ve sanal ad alanı yollarında **depolama hedefi** sayfasında bulabilirsiniz.

![Azure HPC önbellek örneğinin genel bakış sayfasının ekran görüntüsü, sağ alt taraftaki bağlama adresleri listesi etrafında bir vurgulama kutusuyla](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Önbellek bağlama adresleri, önbelleğin alt ağının içindeki ağ arabirimlerine karşılık gelir. Bir kaynak grubunda, bu NIC 'ler bitiş adlarıyla `-cluster-nic-` ve bir sayı ile listelenir. Bu arabirimleri değiştirmeyin veya silmeyin, aksi durumda önbellek kullanılamaz hale gelir.

Sanal ad alanı yolları her bir depolama hedefinin ayrıntılar sayfasında gösterilir. Ayrıntılarını görmek için, bununla ilişkili toplanmış ad alanı yolları da dahil olmak üzere, ayrı bir depolama hedefi adına tıklayın.

![depolama hedefinin ayrıntı sayfasının ekran görüntüsü ("depolama hedefini Güncelleştir" başlığı). Tablonun sanal ad alanı yol sütunundaki bir girdinin etrafında bir vurgulama kutusu bulunur](media/hpc-cache-view-namespace-paths.png)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri önbelleğin depolama hedeflerine taşımak için [yeni Azure Blob depolama alanını](hpc-cache-ingest.md)okuyun.
