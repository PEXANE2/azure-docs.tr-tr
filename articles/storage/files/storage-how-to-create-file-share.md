---
title: Azure dosya paylaşımı oluşturma
titleSuffix: Azure Files
description: Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak Azure dosya paylaşımı oluşturma.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 2/22/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ed6abbac7c5953eaec4fa4584248d0d98b49ba63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596941"
---
# <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Azure dosya paylaşımı oluşturmak için, nasıl kullanacağınızla ilgili üç soruyu yanıtlamanız gerekir:

- **Azure dosya paylaşımınızın performans gereksinimleri nelerdir?**  
    Azure Files, sabit disk tabanlı (HDD tabanlı) donanımda barındırılan standart dosya paylaşımları ve katı hal disk tabanlı (SSD tabanlı) donanımda barındırılan premium dosya paylaşımları sunar.

- **Hangi boyutta dosya paylaşımına ihtiyacınız var?**  
    Standart dosya paylaşımları 100 TiB'ye kadar yayılabilir, ancak bu özellik varsayılan olarak etkinleştirilemez; 5 TiB'den büyük bir dosya paylaşımına ihtiyacınız varsa, depolama hesabınız için büyük dosya paylaşımı özelliğini etkinleştirmeniz gerekir. Premium dosya paylaşımları herhangi bir özel ayar olmadan 100 TiB'ye kadar yayılabilir, ancak standart dosya paylaşımları gibi giderken ödeme yapmak yerine premium dosya paylaşımları sağlanabilir. Bu, gereksinim duyduğunuzdan çok daha büyük bir dosya paylaşımı sağlamanın toplam depolama maliyetini artıracağı anlamına gelir.

- **Azure dosya paylaşımınız için artıklık gereksinimleriniz nelerdir?**  
    Standart dosya paylaşımları yerel olarak yedekli (LRS), bölge yedekli (ZRS), coğrafi yedekli (GRS) veya coğrafi bölge yedekli (GZRS) depolama sunar, ancak büyük dosya paylaşımı özelliği yalnızca yerel olarak yedekli ve bölge gereksiz dosya paylaşımlarında desteklenir. Premium dosya paylaşımları herhangi bir coğrafi artıklığı desteklemez.

    Premium dosya paylaşımları, depolama hesapları sunan çoğu bölgede yerel olarak artıklıkla ve daha küçük bir bölge alt kümesinde bölge artıklığıyla kullanılabilir. Premium dosya paylaşımlarının bölgenizde şu anda kullanılabilir olup olmadığını öğrenmek için Azure için bölge sayfasına [göre kullanılabilen ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=storage) bakın. ZRS'yi destekleyen bölgeler hakkında bilgi için [Azure Depolama artıklığı'na](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

Bu üç seçenek hakkında daha fazla bilgi için Azure [Dosyaları dağıtımı için Planlama'ya](storage-files-planning.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
- Bu makalede, zaten bir Azure aboneliği oluşturduğunuz varsayar. Zaten bir aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Azure PowerShell'i kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)
- Azure CLI'yi kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Azure dosya paylaşımları, paylaşılan bir depolama havuzunun temsil edildiği üst düzey nesneler olan *depolama hesaplarına*dağıtılır. Bu depolama havuzu birden çok dosya paylaşımıdağıtmak için kullanılabilir. 

Azure, müşterilerin sahip olabileceği farklı depolama senaryoları için birden çok depolama hesabı türünü destekler, ancak Azure Dosyaları için iki ana depolama hesabı türü vardır. Oluşturmanız gereken depolama hesabı türü, standart bir dosya paylaşımı veya premium dosya paylaşımı oluşturmak isteyip istemediğiniz bağlıdır: 

- **Genel amaçlı sürüm 2 (GPv2) depolama hesapları : GPv2**depolama hesapları, Azure dosya paylaşımlarını standart/sabit disk tabanlı (HDD tabanlı) donanıma dağıtmanıza olanak tanır. GPv2 depolama hesapları, Azure dosya paylaşımlarını depolamanın yanı sıra blob kapsayıcıları, kuyruklar veya tablolar gibi diğer depolama kaynaklarını da depolayabilir. 

- **FileStorage depolama hesapları**: FileStorage depolama hesapları, Azure dosya paylaşımlarını premium/solid-state disk tabanlı (SSD tabanlı) donanıma dağıtmanıza olanak tanır. FileStorage hesapları yalnızca Azure dosya paylaşımlarını depolamak için kullanılabilir; FileStorage hesabında başka depolama kaynakları (blob kapsayıcıları, kuyruklar, tablolar, vb.) dağıtılamaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalı üzerinden bir depolama hesabı oluşturmak için panodan **+ kaynak oluşturun'u** seçin. Ortaya çıkan Azure Marketi arama penceresinde, **depolama hesabı** nı arayın ve ortaya çıkan arama sonucunu seçin. Bu, depolama hesapları için bir genel bakış sayfasına yol açar; depolama hesabı oluşturma sihirbazına devam etmek için **Oluştur'u** seçin.

![Bir tarayıcıda depolama hesabının hızlı oluşturma seçeneğinin ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-0.png)

#### <a name="the-basics-section"></a>Temel Bilgiler bölümü
Bir depolama hesabı oluşturmak için tamamlanması gereken ilk bölüm **Temelleri**olarak etiketlenir. Bu, bir depolama hesabı oluşturmak için gereken tüm alanları içerir. Bir GPv2 depolama hesabı oluşturmak **için, Performans** radyo düğmesinin *Standart* olarak ayarlandığından ve **Hesap türü** açılır listesinin *StorageV2 (genel amaçlı v2)* olarak seçildiğinden emin olun.

![Standart seçili performans radyo düğmesinin ekran görüntüsü ve StorageV2 seçili Hesap türü](media/storage-how-to-create-file-share/create-storage-account-1.png)

FileStorage depolama hesabı oluşturmak **için, Performans** radyo düğmesinin *Premium* olarak ayarlandığından ve *FileStorage*için **Hesap türü** açılır listesinin seçildiğinden emin olun.

![Premium seçili Performans radyo düğmesinin ekran görüntüsü ve FileStorage seçili Hesap türü](media/storage-how-to-create-file-share/create-storage-account-2.png)

Diğer temel alanlar depolama hesabı seçiminden bağımsızdır:
- **Abonelik**: Dağıtılacak depolama hesabının aboneliği. 
- **Kaynak grubu**: Dağıtılacak depolama hesabı için kaynak grubu. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu kullanabilirsiniz. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır.
- **Depolama hesabı adı**: Oluşturulacak depolama hesabı kaynağının adı. Bu ad genel olarak benzersiz olmalıdır, ancak aksi takdirde istediğiniz herhangi bir ad olabilir. SMB üzerinden Azure dosya paylaşımı yaptığınızda depolama hesabı adı sunucu adı olarak kullanılır.
- **Konum**: Depolama hesabının dağıtılacak olduğu bölge. Bu, kaynak grubuyla ilişkili bölge veya kullanılabilir başka bir bölge olabilir.
- **Çoğaltma**: Bu çoğaltma etiketli olmasına rağmen, bu alan aslında **artıklık**anlamına gelir; bu istenilen artıklık düzeyidir: yerel artıklık (LRS), bölge artıklığı (ZRS), coğrafi artıklık (GRS) ve coğrafi bölge artıklığı. Bu açılır liste ayrıca Azure dosya paylaşımları için geçerli olmayan okuma erişimi coğrafi artıklığını (RA-GRS) ve okuma erişimi coğrafi bölge artıklığını (RA-GZRS) içerir; bu seçili bir depolama hesabında oluşturulan herhangi bir dosya paylaşımı aslında sırasıyla, jeo-yedekli veya coğrafi bölge gereksiz olacaktır. Bölgenize veya seçili depolama hesabı türüne bağlı olarak, bazı artıklık seçeneklerine izin verilmeyebilir.
- **Erişim katmanı**: Bu alan Azure Dosyaları için geçerli değildir, bu nedenle radyo düğmelerinden birini seçebilirsiniz.

#### <a name="the-networking-blade"></a>Ağ bıçağı
Ağ bölümü, ağ seçeneklerini yapılandırmanızı sağlar. Bu ayarlar depolama hesabının oluşturulması için isteğe bağlıdır ve istenirse daha sonra yapılandırılabilir. Bu seçenekler hakkında daha fazla bilgi için [Azure Dosyaları ağ la ilgili hususlara](storage-files-networking-overview.md)bakın.

#### <a name="the-advanced-blade"></a>Gelişmiş bıçak
Gelişmiş bölüm, Azure dosya paylaşımları için birkaç önemli ayar içerir:

- **Güvenli aktarım gereklidir**: Bu alan, depolama hesabının depolama hesabına iletişim için aktarım sırasında şifreleme gerekip gerekmediğini gösterir. Bunun etkin olmasını öneririz, ancak SMB 2.1 desteğine ihtiyacınız varsa, bunu devre dışı bırakmanız gerekir. Depolama hesabı erişiminizi hizmet bitiş noktaları ve/veya özel uç noktalarıyla sanal ağa kısıtlamak için şifrelemeyi devre dışı bağınıza girmenizi öneririz.
- **Büyük dosya paylaşımları**: Bu alan, 100 TiB'e kadar olan dosya paylaşımlarının depolama hesabını sağlar. Bu özelliği etkinleştirmek, depolama hesabınızı yalnızca yerel olarak yedekli ve bölge gereksiz depolama seçenekleriyle sınırlandıracaktır. Büyük dosya paylaşımları için bir GPv2 depolama hesabı etkinleştirildikten sonra, büyük dosya paylaşımı özelliğini devre dışı kalamazsınız. FileStorage depolama hesapları (premium dosya paylaşımları için depolama hesapları) bu seçeneğe sahip değildir, çünkü tüm premium dosya paylaşımları 100 TiB'ye kadar ölçeklendirilebilir. 

![Azure Dosyaları için geçerli olan önemli gelişmiş ayarların ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-3.png)

Gelişmiş sekmesinde kullanılabilen diğer ayarlar (blob soft-delete, Azure Veri Gölü depolama gen 2 için hiyerarşik ad alanı 2 ve blob depolama için NFSv3) Azure Dosyaları için geçerli değildir.

#### <a name="tags"></a>Etiketler
Etiketler, aynı etiketi birden çok kaynak ve kaynak grubuna uygulayarak kaynakları kategorilere ayırmanızı ve birleştirilmiş faturalandırmayı görüntülemenizi sağlayan ad/değer çiftleridir. Bunlar isteğe bağlıdır ve depolama hesabı oluşturulduktan sonra uygulanabilir.

#### <a name="review--create"></a>İnceleme + oluşturma
Depolama hesabı oluşturmak için son adım Gözden Geçir **+ oluştur** sekmesinde **Oluştur** düğmesini seçmektir. Depolama hesabı için gerekli tüm alanlar doldurulmazsa, bu düğme kullanılamaz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
PowerShell kullanarak bir depolama hesabı oluşturmak `New-AzStorageAccount` için cmdlet kullanacağız. Bu cmdlet birçok seçenek vardır; yalnızca gerekli seçenekler gösterilir. Gelişmiş seçenekler hakkında daha fazla bilgi edinmek için [ `New-AzStorageAccount` cmdlet belgelerine](/powershell/module/az.storage/new-azstorageaccount)bakın.

Depolama hesabının ve sonraki dosya paylaşımının oluşturulmasını kolaylaştırmak için, çeşitli parametreleri değişkenlerde depolarız. Değişken içeriği istediğiniz değerlerle değiştirebilirsiniz, ancak depolama hesabı adının genel olarak benzersiz olması gerektiğini unutmayın.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$storageAccountName = "mystorageacct$(Get-Random)"
$region = "westus2"
```

Standart Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanırız. `-SkuName` Parametre, istenen fazlalık türüyle ilgilidir; Coğrafi-yedekli veya coğrafi bölge gereksiz depolama hesabı istiyorsanız, parametreyi `-EnableLargeFileShare` de kaldırmanız gerekir.

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Standard_LRS `
    -Location $region `
    -Kind StorageV2 `
    -EnableLargeFileShare
```

Premium Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanırız. Parametrenin `-SkuName` hem de `Premium` istenilen artıklık düzeyini içerecek şekilde değiştiğini unutmayın.`LRS` Parametre, `-Kind` `FileStorage` premium `StorageV2` dosya paylaşımlarının GPv2 depolama hesabı yerine FileStorage depolama hesabında oluşturulması gerektiği için bunun yerine.

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $region `
    -Kind FileStorage 
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLI'yi kullanarak bir depolama hesabı oluşturmak için az depolama hesabı oluşturma komutunu kullanırız. Bu komutun birçok seçeneği vardır; yalnızca gerekli seçenekler gösterilir. Gelişmiş seçenekler hakkında daha fazla bilgi edinmek için [ `az storage account create` komut belgelerine](/cli/azure/storage/account)bakın.

Depolama hesabının ve sonraki dosya paylaşımının oluşturulmasını kolaylaştırmak için, çeşitli parametreleri değişkenlerde depolarız. Değişken içeriği istediğiniz değerlerle değiştirebilirsiniz, ancak depolama hesabı adının genel olarak benzersiz olması gerektiğini unutmayın.

```azurecli-interactive
resourceGroupName="myResourceGroup"
storageAccountName="mystorageacct$RANDOM"
region="westus2"
```

Standart Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanırız. `--sku` Parametre, istenen fazlalık türüyle ilgilidir; Coğrafi-yedekli veya coğrafi bölge gereksiz depolama hesabı istiyorsanız, parametreyi `--enable-large-file-share` de kaldırmanız gerekir.

```azurecli-interactive
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind StorageV2 \
    --sku Standard_ZRS \
    --enable-large-file-share \
    --output none
```

Premium Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanırız. Parametrenin `--sku` hem de `Premium` istenilen artıklık düzeyini içerecek şekilde değiştiğini unutmayın.`LRS` Parametre, `--kind` `FileStorage` premium `StorageV2` dosya paylaşımlarının GPv2 depolama hesabı yerine FileStorage depolama hesabında oluşturulması gerektiği için bunun yerine.

```azurecli-interactive
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind FileStorage \
    --sku Premium_LRS \
    --output none
```

---

## <a name="create-file-share"></a>Dosya paylaşımı oluşturma
Depolama hesabınızı oluşturduktan sonra, geriye kalan tek şey dosya paylaşımınızı oluşturmaktır. Bu işlem, premium dosya paylaşımı veya standart bir dosya paylaşımı kullanıp kullanmadığınıza bakılmaksızın çoğunlukla aynıdır. Birincil fark **kota** ve ne temsil eder.

Standart dosya paylaşımları için, son kullanıcıların gidemediği Azure dosya paylaşımının bir üst sınırıdır. Standart bir dosya paylaşımı için kotanın temel amacı bütçedir: "Bu dosya paylaşımının bu noktanın ötesine büyümesini istemiyorum". Kota belirtilmemişse, standart dosya paylaşımı 100 TiB'ye (veya büyük dosya paylaşım özelliği bir depolama hesabı için ayarlanmamışsa 5 TiB'ye) kadar yayılabilir.

Premium dosya paylaşımları için kota, **sağlanan boyut**ortalamasına göre aşırı yüklenir. Sağlanan boyut, fiili kullanımdan bağımsız olarak faturalandırılacak tutardır. Bir premium dosya paylaşımı sağlarken, iki faktörü göz önünde bulundurmak istersiniz: 1) alan kullanım perspektifinden payın gelecekteki büyümesi ve 2) iş yükünüz için gerekli Olan IOPS. Her verilen GiB ek ayrılmış ve patlama IOPS hakkı verir. Premium dosya paylaşımının nasıl planlanması hakkında daha fazla bilgi için, [bkz.](storage-files-planning.md#understanding-provisioning-for-premium-file-shares)

# <a name="portal"></a>[Portal](#tab/azure-portal)
Depolama hesabınızı yeni oluşturduysanız, **kaynağa git**seçeneğini seçerek dağıtım ekranından bu hesaba gidebilirsiniz. Depolama hesabını daha önce oluşturduysanız, depoyu içeren kaynak grubu aracılığıyla bu hesaba yönlendirebilirsiniz. Depolama hesabına girdiğimizde, dosya **paylaşımları** etiketli döşemeyi seçin (depolama hesabının içindekiler tablosu üzerinden **Dosya paylaşımlarına** da gidebilirsiniz).

![Dosya nın ekran görüntüsü, döşemeyi paylaşıyor](media/storage-how-to-create-file-share/create-file-share-1.png)

Dosya paylaşımı listesinde, bu depolama hesabında daha önce oluşturduğunuz dosya paylaşımlarını görmeniz gerekir; henüz dosya paylaşımı oluşturulmadıysa boş bir tablo. Yeni bir dosya paylaşımı oluşturmak için **+ Dosya paylaşımını** seçin.

Yeni dosya paylaşım bıçağı ekranda görünmelidir. Dosya paylaşımı oluşturmak için yeni dosya paylaşım bıçaklarındaki alanları tamamlayın:

- **Adı**: oluşturulacak dosya paylaşımının adı.
- **Kota**: Standart dosya paylaşımları için dosya payı kotası; premium dosya paylaşımları için dosya paylaşımının sağlanan boyutu.

Yeni paylaşımı oluşturmak için **Oluştur'u** seçin. Depolama hesabınız sanal bir ağdaysa, müşteriniz sanal ağda olmadığı sürece Azure dosya paylaşımını başarıyla oluşturamadığınızı unutmayın. Azure PowerShell `New-AzRmStorageShare` cmdlet'i kullanarak bu zaman içinde zaman sınırlamasını da çözebilirsiniz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Cmdlet ile Azure dosya [`New-AzRmStorageShare`](/powershell/module/az.storage/New-AzRmStorageShare) paylaşımını oluşturabilirsiniz. Aşağıdaki PowerShell komutları, azure PowerShell `$resourceGroupName` `$storageAccountName` bölümünde bir depolama hesabı oluştururken değişkenleri ayarladığınızı ve yukarıda tanımlandığı gibi varsaydığınızı varsayar. 

> [!Important]  
> Premium dosya paylaşımları `-QuotaGiB` için parametre, dosya paylaşımının sağlanan boyutunu ifade eder. Dosya payının sağlanan boyutu, kullanımdan bağımsız olarak faturalandırılacak tutardır. Standart dosya paylaşımları, sağlanan boyut yerine kullanıma göre faturalandırılır.

```azurepowershell-interactive
$shareName = "myshare"

New-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -QuotaGiB 1024 | Out-Null
```

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarını ve dosyalarını adlandırma hakkında tam ayrıntılar için, [paylaşımları, dizinleri, dosyaları ve meta verileri adlandırma ve başvurma bölümüne](https://msdn.microsoft.com/library/azure/dn167011.aspx)bakın.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLI ile bir Azure dosya paylaşımı oluşturmadan önce, dosya paylaşımı oluşturma işlemini yetkilendirmek için bir depolama hesabı anahtarı almanız gerekir. Bu [`az storage account keys list`](/cli/azure/storage/account/keys) komutu ile yapılabilir:

```azurecli-interactive
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')
```

Depolama hesabı anahtarına sahip olduktan sonra, komutla [`az storage share create`](/cli/azure/storage/share) Azure dosya paylaşımını oluşturabilirsiniz. 

> [!Important]  
> Premium dosya paylaşımları `--quota` için parametre, dosya paylaşımının sağlanan boyutunu ifade eder. Dosya payının sağlanan boyutu, kullanımdan bağımsız olarak faturalandırılacak tutardır. Standart dosya paylaşımları, sağlanan boyut yerine kullanıma göre faturalandırılır.

```azurecli-interactive
shareName="myshare"

az storage share create \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --name $shareName \
    --quota 1024 \
    --output none
```

Depolama hesabı sanal bir ağ içinde bulunursa ve bu komutu çağırdığınız bilgisayar sanal ağın bir parçası değilse bu komut başarısız olur. Azure PowerShell `New-AzRmStorageShare` cmdlet'ini yukarıda açıklandığı gibi kullanarak veya Sanal ağın bir parçası olan bir bilgisayardan Azure CLI'yi çalıştırarak(VPN bağlantısı da dahil olmak üzere) bu zaman dilimi sınırlamasını çözebilirsiniz.

---

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarını ve dosyalarını adlandırma hakkında tam ayrıntılar için, [paylaşımları, dizinleri, dosyaları ve meta verileri adlandırma ve başvurma bölümüne](https://msdn.microsoft.com/library/azure/dn167011.aspx)bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosyalarının dağıtımını planlayın](storage-files-planning.md) veya [Azure Dosya Eşitlemeyi'nin dağıtımını planlayın.](storage-sync-files-planning.md) 
- [Ağ genel bakış](storage-files-networking-overview.md).
- [Windows,](storage-how-to-use-files-windows.md) [macOS](storage-how-to-use-files-mac.md)ve [Linux'ta](storage-how-to-use-files-linux.md)dosya paylaşımını bağlayın ve bağlayın.