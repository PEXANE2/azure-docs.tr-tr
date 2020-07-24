---
title: Azure HPC önbelleğine depolama ekleme
description: Azure HPC önbelleğinizin, uzun süreli dosya depolaması için şirket içi NFS sisteminizi veya Azure Blob kapsayıcılarını kullanabilmesi için depolama hedeflerini tanımlama
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 6923bb31e53493dd01f41cb0b0449f2093bc7e91
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092482"
---
# <a name="add-storage-targets"></a>Depolama hedefleri ekleme

*Depolama hedefleri* , BIR Azure HPC önbelleğinden erişilen dosyalar için arka uç depolardır. NFS depolama (Şirket içi donanım sistemi gibi) ekleyebilir veya Azure Blob 'da verileri saklayabilirsiniz.

Tek bir önbellek için en fazla on farklı depolama hedefi tanımlayabilirsiniz. Önbellek, tüm depolama hedeflerini toplanmış bir ad alanında gösterir.

Depolama dışarı aktarımlarının, önbelleğinizin sanal ağından erişilebilir olması gerektiğini unutmayın. Şirket içi donanım depolaması için NFS depolama erişimi için konak adlarını çözebilen bir DNS sunucusu ayarlamanız gerekebilir. [DNS erişiminde](hpc-cache-prerequisites.md#dns-access)daha fazla bilgi edinin.

Önbelleğinizi oluşturduktan sonra depolama hedefleri ekleyin. Yordam, Azure Blob depolama veya NFS dışarı aktarma eklediğinize bağlı olarak biraz farklılık gösterebilir. Her biri için Ayrıntılar aşağıda verilmiştir.

Bir önbellek oluşturma ve Azure portal bir depolama hedefi ekleme hakkında [video tanıtımı](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/) izlemek için aşağıdaki görüntüye tıklayın.

[![video küçük resmi: Azure HPC Cache: Setup (video sayfasını ziyaret etmek için tıklayın)](media/video-4-setup.png)](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/)

## <a name="view-storage-targets"></a>Depolama hedeflerini görüntüleme

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal, önbellek örneğinizi açın ve sol kenar çubuğundaki **depolama hedefleri** ' ne tıklayın. Depolama hedefleri sayfası, tüm mevcut hedefleri listeler ve yeni bir bağlantı eklemek için bir bağlantı sağlar.

![Kategori başlıkları ayarları ve Izleme arasındaki başlık yapılandırması altında bulunan, alt çubukta depolama hedefleri bağlantısının ekran görüntüsü](media/hpc-cache-storage-targets-sidebar.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Bir önbelleğin mevcut depolama hedeflerini göstermek için [az HPC-Cache Storage-Target List](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-list) seçeneğini kullanın. Önbellek adını ve kaynak grubunu sağlayın (Bu ayarı küresel olarak ayarlamadığınız müddetçe).

```azurecli
az hpc-cache storage-target list --resource-group "scgroup" --cache-name "sc1"
```

Belirli bir depolama hedefi hakkındaki ayrıntıları görmek için [az HPC-Cache Storage-Target Show](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-list) ' i kullanın. (Depolama hedefini ada göre belirtin.)

Örnek:

```azurecli
$ az hpc-cache storage-target show --cache-name doc-cache0629 --name nfsd1

{
  "clfs": null,
  "id": "/subscriptions/<subscription_ID>/resourceGroups/scgroup/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "scgroup",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}
$
```

---

## <a name="add-a-new-azure-blob-storage-target"></a>Yeni bir Azure Blob depolama hedefi ekleme

Yeni bir BLOB depolama hedefi boş bir blob kapsayıcısına veya Azure HPC önbellek bulut dosya sistemi biçimindeki verilerle doldurulmuş bir kapsayıcıya ihtiyaç duyuyor. [Verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)bölümünde bir blob kapsayıcısını önceden yükleme hakkında daha fazla bilgi edinin.

Azure portal **depolama hedefi ekleme** sayfası, eklemeden hemen önce yeni bir blob kapsayıcısı oluşturma seçeneğini içerir.

### <a name="portal"></a>[Portal](#tab/azure-portal)

![Yeni bir Azure Blob depolama hedefi için bilgilerle doldurulmuş depolama hedefi ekleme sayfasının ekran görüntüsü](media/hpc-cache-add-blob.png)

Bir Azure Blob kapsayıcısı tanımlamak için bu bilgileri girin.

* **Depolama hedefi adı** -Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.
* **Hedef türü** - **BLOB**seçin.
* **Depolama hesabı** -kullanmak istediğiniz hesabı seçin.

  [Erişim rolleri ekleme](#add-the-access-control-roles-to-your-account)bölümünde açıklandığı gibi, depolama hesabına erişmek için önbellek örneğini yetkilendirmeniz gerekecektir.

  Kullanabileceğiniz depolama hesabı türü hakkında daha fazla bilgi için, [BLOB depolama gereksinimlerini](hpc-cache-prerequisites.md#blob-storage-requirements)okuyun.

* **Depolama kapsayıcısı** -bu hedefin blob kapsayıcısını seçin veya **Yeni oluştur**' a tıklayın.

  ![Yeni kapsayıcı için ad ve erişim düzeyi (özel) belirtme iletişim kutusunun ekran görüntüsü](media/add-blob-new-container.png)

* **Sanal ad alanı yolu** -bu depolama hedefi için istemciye yönelik dosya yolunu ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını Yapılandır](hpc-cache-namespace.md) makalesini okuyun.

İşiniz bittiğinde, depolama hedefini eklemek için **Tamam** ' ı tıklatın.

> [!NOTE]
> Depolama hesabı güvenlik duvarınız yalnızca "seçili ağlara erişimi kısıtla" olarak ayarlandıysa, [BLOB depolama hesabı güvenlik duvarı ayarlarında çözüm](hpc-cache-blob-firewall-fix.md)olarak belgelenen geçici geçici çözümü kullanın.

### <a name="add-the-access-control-roles-to-your-account"></a>Erişim denetimi rollerini hesabınıza ekleyin

Azure HPC Cache, önbellek hizmetinin Azure Blob depolama hedeflerine yönelik depolama hesabınıza erişmesini yetkilendirmek için [rol tabanlı erişim denetimi 'ni (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index) kullanır.

Depolama hesabı sahibi, "HPC Cache kaynak sağlayıcısı" kullanıcısı için rol [depolama hesabı katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) ve [Depolama Blobu veri katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) ' nı açıkça eklemesi gerekir.

Bunu zaman içinde yapabilir veya bir BLOB depolama hedefi eklediğiniz sayfada bir bağlantıya tıklayarak yapabilirsiniz. Rol ayarlarının Azure ortamından yayılması beş dakika sürebileceğini unutmayın. bu nedenle, bir depolama hedefi oluşturmadan önce roller eklendikten sonra birkaç dakika beklemeniz gerekir.

RBAC rolleri ekleme adımları:

1. Depolama hesabı için **erişim denetimi (IAM)** sayfasını açın. ( **Depolama hedefi Ekle** sayfasındaki bağlantı, seçili hesap için otomatik olarak bu sayfayı açar.)

1. Sayfanın üst kısmındaki öğesine tıklayın **+** ve **rol ataması Ekle**' yi seçin.

1. Listeden "depolama hesabı katılımcısı" rolünü seçin.

1. **Erişim ata** alanına, varsayılan değeri seçili bırakın ("Azure AD kullanıcısı, Grup veya hizmet sorumlusu").  

1. **Seç** alanında "HPC" ifadesini arayın.  Bu dize, "HPC Cache Resource Provider" adlı bir hizmet sorumlusu ile eşleşmelidir. Bu sorumluyu seçmek için tıklayın.

   > [!NOTE]
   > "HPC" araması işe yaramazsa bunun yerine "storagecache" dizesini kullanmayı deneyin. Önizlemelerde (GA 'den önce) katılan kullanıcıların hizmet sorumlusu için eski adı kullanması gerekebilir.

1. Alttaki **Kaydet** düğmesine tıklayın.

1. "Depolama Blobu verileri katılımcısı" rolünü atamak için bu işlemi tekrarlayın.  

![rol ataması Ekle GUI ekran görüntüsü](media/hpc-cache-add-role.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

### <a name="prerequisite-storage-account-access"></a>Önkoşul: depolama hesabı erişimi

BLOB depolama hedefi eklemeden önce, önbelleğin depolama hesabına erişmek için doğru rollere sahip olduğunu ve güvenlik duvarı ayarlarının depolama hedefi oluşturulmasına izin ver, olduğunu kontrol edin.

Azure HPC Cache, önbellek hizmetinin Azure Blob depolama hedeflerine yönelik depolama hesabınıza erişmesini yetkilendirmek için [rol tabanlı erişim denetimi 'ni (RBAC)](../role-based-access-control/index.yml) kullanır.

Depolama hesabı sahibi, "HPC Cache kaynak sağlayıcısı" kullanıcısı için rol [depolama hesabı katılımcısı](../role-based-access-control/built-in-roles.md#storage-account-contributor) ve [Depolama Blobu veri katılımcısı](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) ' nı açıkça eklemesi gerekir.

Önbellekte bu roller yoksa depolama hedefi oluşturma başarısız olur.

Rol ayarlarının Azure ortamından yayılması beş dakika kadar sürebilir. bu nedenle, bir depolama hedefi oluşturmadan önce roller eklendikten sonra birkaç dakika beklemeniz gerekir.

Ayrıntılı yönergeler için Azure [CLI kullanarak Azure rol atamaları ekleme veya kaldırma](../role-based-access-control/role-assignments-cli.md) konusunu okuyun.

Ayrıca depolama hesabınızın güvenlik duvarı ayarlarını kontrol edin. Güvenlik Duvarı yalnızca "seçili ağlara erişimi kısıtla" olarak ayarlandıysa, depolama hedefi oluşturma işlemi başarısız olabilir. [BLOB depolama hesabı güvenlik duvarı ayarları etrafında çalışırken](hpc-cache-blob-firewall-fix.md)belgelenen geçici çözümü kullanın.

### <a name="add-a-blob-storage-target-with-azure-cli"></a>Azure CLı ile BLOB depolama hedefi ekleme

Bir Azure Blob depolama hedefi tanımlamak için [az HPC-Cache blob-Storage-Target Add](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-add) arabirimini kullanın.

Standart kaynak grubu ve önbellek adı parametrelerine ek olarak, depolama hedefi için bu seçenekleri sağlamanız gerekir:

* ``--name``-Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.

* ``--storage-account``-Hesap tanımlayıcısı şu biçimdedir:/Subscriptions/*<subscription_id>*/ResourceGroups/*<storage_resource_group>*/Providers/Microsoft.Storage/storageAccounts/*<account_name>*

  Kullanabileceğiniz depolama hesabı türü hakkında daha fazla bilgi için, [BLOB depolama gereksinimlerini](hpc-cache-prerequisites.md#blob-storage-requirements)okuyun.

* ``--container-name``-Bu depolama hedefi için kullanılacak kapsayıcının adını belirtin.

* ``--virtual-namespace-path``-Bu depolama hedefi için istemciye yönelik dosya yolunu ayarlayın. Yolları tırnak işaretleri içine alın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanı planını](hpc-cache-namespace.md) okuyun.

Örnek komut:

```azurecli
az hpc-cache blob-storage-target add --resource-group "hpc-cache-group" \
    --cache-name "cache1" --name "blob-target1" \
    --storage-account "/subscriptions/<subscriptionID>/resourceGroups/myrgname/providers/Microsoft.Storage/storageAccounts/myaccountname" \
    --container-name "container1" --virtual-namespace-path "/blob1"
```

---

## <a name="add-a-new-nfs-storage-target"></a>Yeni bir NFS depolama hedefi ekleme

NFS depolama hedefi, BLOB depolama hedefinden daha fazla alan içeriyor. Bu alanlar, depolama dışa aktarmaya nasıl ulaşılmayı ve verilerin nasıl etkili bir şekilde önbelleğe alınacağını belirtir. Ayrıca, NFS ana bilgisayarında birden fazla dışa aktarma işlemi varsa, NFS depolama hedefi birden çok ad alanı yolu oluşturmanıza olanak sağlar.

![NFS hedefi tanımlı depolama hedefi ekleme sayfasının ekran görüntüsü](media/add-nfs-target.png)

> [!NOTE]
> Bir NFS depolama hedefi oluşturmadan önce, depolama sisteminizin Azure HPC önbelleğinden erişilebilir olduğundan ve izin gereksinimlerini karşıladığından emin olun. Önbellek depolama sistemine erişemeyecektir depolama hedefi oluşturma işlemi başarısız olur. Ayrıntılar için [NFS depolama gereksinimlerini](hpc-cache-prerequisites.md#nfs-storage-requirements) okuyun ve [NAS yapılandırma ve NFS depolama hedefi sorunlarını giderin](troubleshoot-nas.md) .

### <a name="choose-a-usage-model"></a>Kullanım modeli seçin
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Bir NFS depolama sistemine işaret eden bir depolama hedefi oluşturduğunuzda, bu hedefin kullanım modelini seçmeniz gerekir. Bu model, verilerinizin nasıl önbelleğe alınacağını belirler.

Üç seçenek vardır:

* **Ağır, seyrek erişimli yazmaları okuma** -statik veya nadiren değiştirilen dosyalara okuma erişimini hızlandırmak istiyorsanız bu seçeneği kullanın.

  Bu seçenek, istemcilerin okuyan dosyaları önbelleğe alır, ancak yazma işlemlerini hemen arka uç depolamaya geçirir. Önbellekte depolanan dosyalar hiçbir şekilde NFS depolama birimindeki dosyalarla karşılaştırılmaz.

  Bir dosyanın doğrudan depolama sisteminde önbellekte yazılmadan değiştirilebilmesi için bir risk varsa bu seçeneği kullanmayın. Bu durumda, dosyanın önbelleğe alınmış sürümü arka uçtaki değişikliklerle hiçbir şekilde güncellenmez ve veri kümesi tutarsız hale gelebilir.

* **%15 ' ten fazla yazma** -Bu seçenek hem okuma hem de yazma performansını hızlandırır. Bu seçeneği kullanırken, tüm istemcilerin arka uç depolamayı doğrudan bağlamak yerine Azure HPC Cache aracılığıyla dosyalara erişmesi gerekir. Önbelleğe alınan dosyalar arka uçta depolanmayan son değişikliklere sahip olur.

  Bu kullanım modelinde, önbellekteki dosyalar arka uç depolamadaki dosyalara karşı denetlenmez. Dosyanın önbelleğe alınan sürümünün daha güncel olduğu varsayılır. Önbellekte değiştirilen bir dosya, önbellekte olduktan sonra, ek değişiklik yapmadan bir saat için arka uç depolama sistemine yazılır.

* **İstemciler, önbelleği ATLAYARAK NFS hedefine yazar** -iş akışdaki herhangi bir istemci, önce önbelleğe yazmadan verileri doğrudan depolama sistemine yazardıysanız bu seçeneği belirleyin. İstemcilerin istediği dosyalar önbelleğe alınır, ancak istemciden bu dosyalardaki tüm değişiklikler arka uç depolama sistemine hemen geçirilir.

  Bu kullanım modeliyle, önbellekteki dosyalar güncelleştirmeler için arka uç sürümlerine göre sıklıkla denetlenir. Bu doğrulama, verilerin tutarlılığın korunmasında önbelleğin dışında değiştirilmesine izin verir.

Bu tablo, kullanım modeli farklarını özetler:

| Kullanım modeli                   | Önbelleğe alma modu | Arka uç doğrulaması | En fazla geri yazma gecikmesi |
|-------------------------------|--------------|-----------------------|--------------------------|
| Yoğun, seyrek okunan yazma işlemleri | Okuma         | Asla                 | Yok                     |
| %15 yazma boyutundan büyük       | Okuma/yazma   | Asla                 | 1 saat                   |
| İstemcileri önbelleği atlar      | Okuma         | 30 saniye            | Yok                     |

### <a name="create-an-nfs-storage-target"></a>NFS depolama hedefi oluşturma

### <a name="portal"></a>[Portal](#tab/azure-portal)

![NFS hedefi tanımlı depolama hedefi ekleme sayfasının ekran görüntüsü](media/add-nfs-target.png)

NFS ile desteklenen bir depolama hedefi için şu bilgileri sağlayın:

* **Depolama hedefi adı** -Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.

* **Hedef türü** - **NFS**'yi seçin.

* **Ana bilgisayar** adı-NFS depolama sisteminizin IP adresini veya tam etki alanı adını girin. (Yalnızca önbelleğinizin adı çözebilecek bir DNS sunucusuna erişimi varsa, bir etki alanı adı kullanın.)

* **Kullanım modeli** -yukarıda [bir kullanım modeli seçin](#choose-a-usage-model) bölümünde açıklanan iş akışınızı temel alan veri önbelleğe alma profillerinden birini seçin.

### <a name="nfs-namespace-paths"></a>NFS ad alanı yolları

Her bir yol aynı depolama sisteminde farklı bir dışarı aktarma veya alt dizini temsil ettiğinde, bir NFS depolama hedefi birden çok sanal yola sahip olabilir.

Bir depolama hedefinden tüm yolları oluşturun.

Dilediğiniz zaman, bir depolama hedefinde [ad alanı yolları ekleyebilir ve düzenleyebilirsiniz](hpc-cache-edit-storage.md) .

Her ad alanı yolu için bu değerleri girin:

* **Sanal ad alanı yolu** -bu depolama hedefi için istemciye yönelik dosya yolunu ayarlayın. Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını Yapılandır](hpc-cache-namespace.md) makalesini okuyun.

* **NFS dışarı aktarma yolu** -NFS dışarı aktarmanın yolunu girin.

* **Alt dizin yolu** -dışarı aktarmanın belirli bir alt dizinini bağlamak istiyorsanız buraya girin. Aksi takdirde, bu alanı boş bırakın.

İşiniz bittiğinde, depolama hedefini eklemek için **Tamam** ' ı tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [cli-reminder.md](includes/cli-reminder.md)]

Depolama hedefini oluşturmak için [az HPC-Cache NFS-Storage-Target Add](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target#ext-hpc-cache-az-hpc-cache-nfs-storage-target-add) Azure CLI komutunu kullanın. Bu değerleri, önbellek adı ve önbellek kaynak grubuna ek olarak sağlayın:

* ``--name``-Azure HPC önbelleğinde bu depolama hedefini tanımlayan bir ad ayarlayın.
* ``--nfs3-target``-NFS depolama sisteminizin IP adresi. (Önbelleğiniz adı çözebilecek bir DNS sunucusuna erişimi varsa, tam etki alanı adını burada kullanabilirsiniz.)
* ``--nfs3-usage-model``-Yukarıda [bir kullanım modeli seçin](#choose-a-usage-model)bölümünde açıklanan veri önbelleğe alma profillerinden biridir.

  [Az HPC-Cache Usage-model List](/cli/azure/ext/hpc-cache/hpc-cache/usage-model#ext-hpc-cache-az-hpc-cache-usage-model-list)komutuyla kullanım modellerinin adlarını doğrulayın.

* ``--junction``-Kavşak parametresi, istemciye yönelik sanal dosya yolunu depolama sistemindeki bir dışarı aktarma yoluyla bağlar.

  Her bir yol aynı depolama sisteminde farklı bir dışarı aktarma veya alt dizini temsil ettiğinde, bir NFS depolama hedefi birden çok sanal yola sahip olabilir. Tek bir depolama hedefinde bir depolama sistemi için tüm yolları oluşturun.

  Dilediğiniz zaman, bir depolama hedefinde [ad alanı yolları ekleyebilir ve düzenleyebilirsiniz](hpc-cache-edit-storage.md) .

  ``--junction``Parametresi şu değerleri kullanır:

  * ``namespace-path``-İstemciye yönelik sanal dosya yolu
  * ``nfs-export``-İstemciye yönelik yol ile ilişkilendirilecek depolama sistemi dışa aktarma
  * ``target-path``(isteğe bağlı)-gerekirse, dışarı aktarmanın alt dizini

  Örnek: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

  Sanal ad alanı özelliği hakkında daha fazla bilgi edinmek için [toplanan ad alanını Yapılandır](hpc-cache-namespace.md) makalesini okuyun.

Örnek komut:

```azurecli

az hpc-cache nfs-storage-target add --resource-group "hpc-cache-group" --cache-name "doc-cache0629" \
    --name nfsd1 --nfs3-target 10.0.0.4 --nfs3-usage-model WRITE_WORKLOAD_15 \
    --junction namespace-path="/nfs1/data1" nfs-export="/datadisk1" target-path=""
```

Çıktı:
```azurecli

{- Finished ..
  "clfs": null,
  "id": "/subscriptions/<subscriptionID>/resourceGroups/hpc-cache-group/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "hpc-cache-group",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}

```

---

## <a name="next-steps"></a>Sonraki adımlar

Depolama hedefleri oluşturduktan sonra şu görevlerden birini göz önünde bulundurun:

* [Azure HPC Önbelleği’ni bağlama](hpc-cache-mount.md)
* [Verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)

Herhangi bir ayarı güncelleştirmeniz gerekiyorsa, [bir depolama hedefini düzenleyebilirsiniz](hpc-cache-edit-storage.md).
