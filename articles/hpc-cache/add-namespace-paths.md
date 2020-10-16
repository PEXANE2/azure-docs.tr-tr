---
title: Azure HPC önbelleği toplanmış ad alanını yapılandırma
description: Azure HPC Cache ile arka uç depolaması için istemciye yönelik yollar oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/30/2020
ms.author: v-erkel
ms.openlocfilehash: 090e3f93d025fe87ad5b89a98193574595f3d632
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91614654"
---
# <a name="set-up-the-aggregated-namespace"></a>Toplanan ad alanını ayarlama

Depolama hedefleri oluşturduktan sonra, bunlar için de ad alanı yolları oluşturmanız gerekir. İstemci makineleri, arka uç depolamaya doğrudan bağlanmak yerine bu sanal yolları dosyalara önbellek aracılığıyla erişmek için kullanır. Bu sistem, istemci yönergelerini yeniden yazmak zorunda kalmadan önbellek yöneticilerinin arka uç depolama sistemlerini değiştirmesine izin verir.

Bu özellik hakkında daha fazla bilgi edinmek için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.

Azure portal **ad alanı** sayfasında, istemcilerin, önbellekteki verilerinize erişmek için kullandığı yollar gösterilir. Ad alanı yollarını oluşturmak, kaldırmak veya değiştirmek için bu sayfayı kullanın. Azure CLı kullanarak ad alanı yollarını da yapılandırabilirsiniz.

Tüm mevcut istemciye yönelik yollar **ad alanı** sayfasında listelenir. Bir depolama hedefinde hiçbir yol yoksa, tabloda görünmez.

Oklara tıklayıp önbelleğinizin toplanmış ad alanını daha iyi anlayabilmeniz için tablo sütunlarını sıralayabilirsiniz.

![bir tabloda iki yol içeren Portal ad alanı sayfasının ekran görüntüsü. Sütun başlıkları: ad alanı yolu, depolama hedefi, dışarı aktarma yolu ve dışarı aktarma alt dizini. İlk sütundaki öğeler tıklatılabilir bağlantılardır. Üst düğmeler: ad alanı yolu Ekle, Yenile, Sil](media/namespace-page.png)

## <a name="add-or-edit-client-facing-namespace-paths"></a>İstemciye yönelik ad alanı yollarını ekleme veya düzenleme

İstemcilerin depolama hedefine erişebilmesi için en az bir ad alanı yolu oluşturmanız gerekir. (İstemci erişimi hakkında daha fazla bilgi için [Azure HPC önbelleğini bağlama](hpc-cache-mount.md) konusunu okuyun.)

### <a name="blob-namespace-paths"></a>Blob ad alanı yolları

Azure Blob depolama hedefi yalnızca bir ad alanı yoluna sahip olabilir.

Azure portal veya Azure CLı ile yolu ayarlamak veya değiştirmek için aşağıdaki yönergeleri izleyin.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal, **ad alanı** ayarları sayfasını yükleyin. Bu sayfadan ad alanı yollarını ekleyebilir, değiştirebilir veya silebilirsiniz.

* **Yeni bir yol ekleyin:** Üstteki **+ Ekle** düğmesine tıklayın ve düzenleme panelinde bilgileri girin.

  * Açılan listeden depolama hedefini seçin. (Bu ekran görüntüsünde, zaten bir ad alanı yolu bulunduğundan BLOB depolama hedefi seçilemez.)

    ![Yeni ad alanının ekran görüntüsü depolama hedefi seçicisiyle kullanıma sunulan alanları Düzenle](media/namespace-select-storage-target.png)

  * Azure Blob depolama hedefi için, dışa aktarma ve alt dizin yolları otomatik olarak olarak ayarlanır ``/`` .

* **Mevcut bir yolu değiştirin:** Ad alanı yoluna tıklayın. Düzenleme paneli açılır ve yolu değiştirebilirsiniz.

  ![Blob ad alanı yoluna tıkladıktan sonra ad alanı sayfasının ekran görüntüsü-düzenleme alanları sağdaki bir bölmede görüntülenir](media/edit-namespace-blob.png)

* **Ad alanı yolunu silme:** Yolun solundaki onay kutusunu seçin ve **Sil** düğmesine tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Azure CLı kullanırken, depolama hedefini oluştururken bir ad alanı yolu eklemeniz gerekir. Ayrıntılar için [Yeni bir Azure Blob depolama hedefi ekleyin](hpc-cache-add-storage.md?tabs=azure-cli#add-a-new-azure-blob-storage-target) makalesini okuyun.

Hedefin ad alanı yolunu güncelleştirmek için [az HPC-Cache blob-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update) komutunu kullanın. Update komutu için bağımsız değişkenler, kapsayıcı adını veya depolama hesabını geçirmezseniz, Create komutunda bağımsız değişkenlere benzerdir.

Azure CLı ile bir BLOB depolama hedefinden bir ad alanı yolunu silemezsiniz, ancak yolun üzerine farklı bir değer yazabilirsiniz.

---

### <a name="nfs-namespace-paths"></a>NFS ad alanı yolları

Her bir yol aynı depolama sisteminde farklı bir dışarı aktarma veya alt dizini temsil ettiğinde, bir NFS depolama hedefi birden çok sanal yola sahip olabilir.

Bir NFS depolama hedefi için ad alanınızı planlarken, her bir yolun benzersiz olması gerektiğini ve başka bir ad alanı yolunun bir alt dizini olabileceğini aklınızda bulundurun. Örneğin, adlı bir ad alanı yolunuz varsa ``/parent-a`` , ve gibi ad alanı yolları da oluşturamazsınız ``/parent-a/user1`` ``/parent-a/user2`` . Bu dizin yollarına, öğesinin alt dizinleri olarak ad alanında zaten erişilebilir ``/parent-a`` .

Bir NFS depolama sistemi için tüm ad alanı yolları tek bir depolama hedefinde oluşturulur. Çoğu önbellek yapılandırması, depolama hedefi başına en fazla on ad alanı yolunu destekleyebilir, ancak daha büyük konfigürasyonlar 20 ' ye kadar destek sağlayabilir.

Bu liste, yapılandırma başına en fazla ad alanı yolu sayısını gösterir.

* 2 GB/sn aktarım hızı:

  * 3 TB önbellek-10 ad alanı yolu
  * 6 TB önbellek-10 ad alanı yolu
  * 23 TB önbellek-20 ad alanı yolu

* En fazla 5 GB/sn aktarım hızı:

  * 6 TB önbellek-10 ad alanı yolu
  * 12 TB önbellek-10 ad alanı yolu
  * 24 TB önbellek-20 ad alanı yolu

* En fazla 8 GB/sn aktarım hızı:

  * 12 TB önbellek-10 ad alanı yolu
  * 24 TB önbellek-10 ad alanı yolu
  * 48 TB önbellek-20 ad alanı yolu

Her NFS ad alanı yolu için, istemciye yönelik yolu, depolama sistemi dışarı aktarmayı ve isteğe bağlı olarak bir dışarı aktarma alt dizinini sağlayın.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal, **ad alanı** ayarları sayfasını yükleyin. Bu sayfadan ad alanı yollarını ekleyebilir, düzenleyebilir veya silebilirsiniz.

* **Yeni bir yol eklemek için:** Üstteki **+ Ekle** düğmesine tıklayın ve düzenleme panelinde bilgileri girin.
* **Varolan bir yolu değiştirmek için:** Ad alanı yoluna tıklayın. Düzenleme paneli açılır ve yolu değiştirebilirsiniz.
* **Bir ad alanı yolunu silmek için:** Yolun solundaki onay kutusunu seçin ve **Sil** düğmesine tıklayın.

Her ad alanı yolu için bu değerleri girin:

* **Ad alanı yolu** -istemciye yönelik dosya yolu.

* **Depolama hedefi** -yeni bir ad alanı yolu oluşturuyorsanız, açılan menüden bir depolama hedefi seçin.

* **Dışarı aktarma yolu** -NFS dışarı aktarmanın yolunu girin. Dışarı aktarma adını doğru yazdığınızdan emin olun; Portal bu alanın sözdizimini doğrular ancak değişikliği yapana kadar dışarı aktarmayı denetlemez.

* **Alt dizini dışarı aktar** -bu yolun, dışarı aktarmanın belirli bir alt dizinini takmasına istiyorsanız buraya girin. Aksi takdirde, bu alanı boş bırakın.

![sağ tarafta Güncelleştirme sayfası açık olan Portal ad alanı sayfasının ekran görüntüsü](media/update-namespace-nfs.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Azure CLı kullanırken, depolama hedefini oluştururken en az bir ad alanı yolu eklemeniz gerekir. Ayrıntılar için [yenı NFS depolama hedefi ekleme](hpc-cache-add-storage.md?tabs=azure-cli#add-a-new-nfs-storage-target) bölümünü okuyun.

Hedefin ad alanı yolunu güncelleştirmek veya ek yollar eklemek için [az HPC-Cache NFS-Storage-Target Update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target#ext-hpc-cache-az-hpc-cache-nfs-storage-target-update) komutunu kullanın. İstediğiniz ``--junction`` ad alanı yollarını belirtmek için seçeneğini kullanın.

Güncelleştirme komutu için kullanılan seçenekler, depolama sistemi bilgilerini (IP adresi veya ana bilgisayar adı) geçirmezseniz ve kullanım modelinin isteğe bağlı olması dışında "Oluştur" komutuna benzer. Seçeneğin sözdizimi hakkında daha fazla bilgi için [yenı NFS depolama hedefi ekleme](hpc-cache-add-storage.md?tabs=azure-cli#add-a-new-nfs-storage-target) makalesini okuyun ``--junction`` .

---

## <a name="next-steps"></a>Sonraki adımlar

Depolama hedeflerinizin toplanmış ad alanını oluşturduktan sonra, istemcileri önbellekte bağlayabilirsiniz. Daha fazla bilgi edinmek için bu makaleleri okuyun.

* [Azure HPC Önbelleği’ni bağlama](hpc-cache-mount.md)
* [Verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)
