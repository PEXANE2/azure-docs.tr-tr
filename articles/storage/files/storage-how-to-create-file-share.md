---
title: Azure dosya paylaşımı oluşturma
titleSuffix: Azure Files
description: Azure portal, PowerShell veya Azure CLı kullanarak Azure dosya paylaşma oluşturma.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 2/22/2020
ms.author: rogarana
ms.subservice: files
ms.custom: devx-track-azurecli, references_regions
ms.openlocfilehash: aaba608ba80a751c40cd300dee80f673897c22a8
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88525658"
---
# <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Bir Azure dosya paylaşımının oluşturulması için, nasıl kullanacağınız hakkında üç soruyu yanıtlamanız gerekir:

- **Azure dosya paylaşımınıza ilişkin performans gereksinimleri nelerdir?**  
    Azure dosyaları, sabit disk tabanlı (HDD tabanlı) donanımda barındırılan standart dosya paylaşımları ve katı hal disk tabanlı (SSD tabanlı) donanım üzerinde barındırılan Premium dosya paylaşımları sunar.

- **Dosya paylaşımıyla ne kadar boyut gerekir?**  
    Standart dosya paylaşımları 100 TiB 'ye yayılabilir, ancak bu özellik varsayılan olarak etkinleştirilmemiştir; 5 TiB 'den büyük bir dosya paylaşımının olması gerekiyorsa, depolama hesabınız için büyük dosya paylaşma özelliğini etkinleştirmeniz gerekir. Premium dosya paylaşımları, herhangi bir özel ayar olmadan 100 TiB 'ye kadar yayılabilir, ancak standart dosya paylaşımları gibi Kullandıkça Öde yerine Premium dosya paylaşımları temin edilir. Bu, bir dosya paylaşımının sağlamaktan çok daha büyük olması için toplam depolama maliyetini artıracak anlamına gelir.

- **Azure dosya paylaşımınıza yönelik yedeklilik gereksinimleriniz nelerdir?**  
    Standart dosya paylaşımları yerel olarak yedekli (LRS), bölgesel olarak yedekli (ZRS), coğrafi olarak yedekli (GRS) veya coğrafi bölge-yedekli (GZRS) depolama alanı sunar, ancak büyük dosya paylaşımı özelliği yalnızca yerel olarak yedekli ve bölge yedekli dosya paylaşımlarında desteklenir. Premium dosya paylaşımları hiçbir coğrafi artıklık biçimini desteklemez.

    Premium dosya paylaşımları, çoğu bölgede depolama hesapları sunan ve bölge artıklığı sağlayan, bölgelerin daha küçük bir alt kümesinde yerel olarak artıklık ile kullanılabilir. Premium dosya paylaşımlarının bölgede şu anda kullanılabilir olup olmadığını öğrenmek için bkz. Azure için [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=storage) sayfası. ZRS 'yi destekleyen bölgeler hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

Bu üç seçenek hakkında daha fazla bilgi için bkz. [Azure dosyaları dağıtımı Için planlama](storage-files-planning.md).

## <a name="prerequisites"></a>Ön koşullar
- Bu makalede, zaten bir Azure aboneliği oluşturmuş olduğunuz varsayılmaktadır. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Azure PowerShell kullanmayı düşünüyorsanız, [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Azure dosya paylaşımları, paylaşılan bir depolama havuzunu temsil eden üst düzey nesneler olan *depolama hesaplarına*dağıtılır. Bu depolama havuzu, birden çok dosya paylaşımını dağıtmak için kullanılabilir. 

Azure, müşterilerin sahip olabileceği farklı depolama senaryoları için birden çok depolama hesabı türünü destekler, ancak Azure dosyaları için iki ana depolama hesabı türü vardır. Oluşturmanız gereken depolama hesabı türü, standart bir dosya paylaşma veya Premium dosya paylaşımının oluşturulmasını isteyip istemediğinize bağlıdır: 

- **Genel amaçlı sürüm 2 (GPv2) depolama hesapları**: GPv2 depolama hesapları, Azure dosya paylaşımlarını standart/sabit disk tabanlı (HDD tabanlı) donanımda dağıtmanıza olanak tanır. GPv2 depolama hesapları, Azure dosya paylaşımlarını depolamanın yanı sıra blob kapsayıcıları, kuyrukları veya tabloları gibi diğer depolama kaynaklarını da depolayabilirler. 

- **FileStorage depolama hesapları**: FileStorage depolama hesapları, Azure dosya paylaşımlarını Premium/katı hal disk tabanlı (SSD tabanlı) donanımda dağıtmanıza olanak tanır. FileStorage hesapları yalnızca Azure dosya paylaşımlarını depolamak için kullanılabilir; başka depolama kaynakları (blob kapsayıcıları, kuyruklar, tablolar vb.) bir FileStorage hesabında dağıtılabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal aracılığıyla bir depolama hesabı oluşturmak için panodan **+ kaynak oluştur** ' u seçin. Elde edilen Azure Marketi araması penceresinde, **depolama hesabı** ' nı arayın ve elde edilen arama sonucunu seçin. Bu, depolama hesapları için bir genel bakış sayfasına yol açacaktır; depolama hesabı oluşturma Sihirbazı ile devam etmek için **Oluştur** ' u seçin.

![Tarayıcıda depolama hesabı hızlı oluşturma seçeneğinin ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-0.png)

#### <a name="the-basics-section"></a>Temel bilgiler bölümü
Bir depolama hesabı oluşturmak için tamamlanacak ilk bölüm **temel bilgiler**olarak etiketlenir. Bu, depolama hesabı oluşturmak için gerekli tüm alanları içerir. GPv2 depolama hesabı oluşturmak için, **performans** radyo düğmesinin *Standart* olarak ayarlandığından ve **Hesap türü** açılan listesinin *StorageV2 (genel amaçlı v2)* olarak seçildiğinden emin olun.

![Standart seçili ve hesap türü StorageV2 seçiliyken performans radyo düğmesinin ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-1.png)

Bir dosya depolama depolama hesabı oluşturmak için, **performans** radyo düğmesinin *Premium* olarak ayarlandığından ve *dosya depolama*için **Hesap türü** açılır listesinin seçildiğinden emin olun.

![Premium seçiliyken ve dosya depolama seçiliyken hesap türüyle performans radyo düğmesinin ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-2.png)

Diğer temelleri alanları, depolama hesabı seçiminizden bağımsızdır:
- **Abonelik**: dağıtım yapılacak depolama hesabı için abonelik. 
- **Kaynak grubu**: dağıtılacak depolama hesabının kaynak grubu. Yeni bir kaynak grubu oluşturabilir veya var olan bir kaynak grubunu kullanabilirsiniz. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır.
- **Depolama hesabı adı**: oluşturulacak depolama hesabı kaynağının adı. Bu ad genel olarak benzersiz olmalıdır, aksi takdirde istediğiniz ad olabilir. SMB aracılığıyla bir Azure dosya paylaşımının bağladığınızda, depolama hesabı adı sunucu adı olarak kullanılır.
- **Konum**: dağıtım yapılacak depolama hesabının bölgesi. Bu, kaynak grubuyla ilişkili bölge veya kullanılabilir başka bir bölge olabilir.
- **Çoğaltma**: Bu, çoğaltma olarak etiketlendiği halde bu alan aslında **yedeklilik**anlamına gelir; Bu, istenen artıklık düzeyindedir: yerel olarak yedekli (LRS), bölge artıklığı (ZRS), coğrafi yedeklilik (GRS) ve coğrafi bölge-artıklık. Bu açılan liste ayrıca, Azure dosya paylaşımları için uygulanan okuma erişimli coğrafi yedeklilik (RA-GRS) ve Okuma Erişimli Coğrafi bölge yedekliliği (RA-GZRS) içerir; Bu seçilenlerle bir depolama hesabında oluşturulan herhangi bir dosya paylaşımının sırasıyla coğrafi olarak yedekli veya coğrafi bölge-yedekli olması gerekir. Bölgenize veya seçili depolama hesabı türüne bağlı olarak, bazı artıklık seçeneklerine izin verilmiyor olabilir.
- **Erişim katmanı**: Bu alan Azure dosyaları için uygulanmadığından radyo düğmelerinden birini seçebilirsiniz.

#### <a name="the-networking-blade"></a>Ağ dikey penceresi
Ağ bölümü, ağ seçeneklerini yapılandırmanızı sağlar. Bu ayarlar, depolama hesabının oluşturulması için isteğe bağlıdır ve isterseniz daha sonra yapılandırılabilir. Bu seçenekler hakkında daha fazla bilgi için bkz. [Azure dosyaları ağ iletişimi konuları](storage-files-networking-overview.md).

#### <a name="the-advanced-blade"></a>Gelişmiş dikey pencere
Gelişmiş bölüm, Azure dosya paylaşımları için çeşitli önemli ayarlar içerir:

- **Güvenli aktarım gerekli**: Bu alan, depolama hesabının depolama hesabıyla iletişim için şifreleme gerektirip gerektirmediğini belirtir. Bunun etkin olmasını öneririz, ancak SMB 2,1 desteğinin olması gerekiyorsa, bunu devre dışı bırakmanız gerekir. Depolama hesabınızı, hizmet uç noktaları ve/veya özel uç noktalarla bir sanal ağa erişimi sınırlandırdıysanız şifrelemeyi devre dışı bırakmanızı öneririz.
- **Büyük dosya paylaşımları**: Bu alan, 100 TİB 'ye yayılan dosya paylaşımları için depolama hesabı sağlar. Bu özelliği etkinleştirmek, depolama hesabınızı yalnızca yerel olarak yedekli ve bölgesel olarak yedekli depolama seçenekleriyle sınırlandırır. Büyük dosya paylaşımları için GPv2 depolama hesabı etkinleştirildikten sonra, büyük dosya paylaşımı özelliğini devre dışı bırakayükleyemezsiniz. Tüm Premium dosya paylaşımları 100 TiB 'ye kadar ölçeklenebileceğinden, FileStorage depolama hesapları (Premium dosya paylaşımları için depolama hesapları) bu seçeneğe sahip değildir. 

![Azure dosyaları için uygulanan önemli gelişmiş ayarların ekran görüntüsü](media/storage-how-to-create-file-share/create-storage-account-3.png)

Gelişmiş sekmesinde bulunan diğer ayarlar (blob geçici silme, Azure Data Lake Storage Gen 2 için hiyerarşik ad alanı ve BLOB depolama için NFSv3), Azure dosyaları için uygulanmaz.

#### <a name="tags"></a>Etiketler
Etiketler, birden fazla kaynağa ve kaynak grubuna aynı etiketi uygulayarak kaynakları kategorilere ayırmanıza ve birleştirilmiş faturalandırmayı görüntülemenize olanak tanıyan ad/değer çiftleridir. Bunlar isteğe bağlıdır ve depolama hesabı oluşturulduktan sonra uygulanabilir.

#### <a name="review--create"></a>Gözden geçirme ve oluşturma
Depolama hesabı oluşturmaya yönelik son adım, **gözden geçir + oluştur** sekmesinde **Oluştur** düğmesini seçmesidir. Bir depolama hesabı için gerekli tüm alanlar doldurulmamışsa bu düğme kullanılamaz.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell kullanarak bir depolama hesabı oluşturmak için `New-AzStorageAccount` cmdlet 'ini kullanacağız. Bu cmdlet 'in birçok seçeneği vardır; yalnızca gerekli seçenekler gösterilir. Gelişmiş seçenekler hakkında daha fazla bilgi edinmek için [ `New-AzStorageAccount` cmdlet belgelerine](/powershell/module/az.storage/new-azstorageaccount)bakın.

Depolama hesabının ve sonraki dosya paylaşımının oluşturulmasını basitleştirmek için, çeşitli parametreleri değişkenlerde depolayacağız. Değişken içeriğini istediğiniz değerle değiştirebilirsiniz, ancak depolama hesabı adının genel olarak benzersiz olması gerektiğini unutmayın.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$storageAccountName = "mystorageacct$(Get-Random)"
$region = "westus2"
```

Standart Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanacağız. `-SkuName`Parametresi, istenen artıklık türü ile ilgilidir; coğrafi olarak yedekli veya coğrafi bölge yedekli depolama hesabı istiyorsanız parametreyi de kaldırmanız gerekir `-EnableLargeFileShare` .

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Standard_LRS `
    -Location $region `
    -Kind StorageV2 `
    -EnableLargeFileShare
```

Premium Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanacağız. `-SkuName`Parametresinin hem hem de `Premium` istenen artıklık düzeyini yerel olarak yedekli () içerecek şekilde değiştirildiğini unutmayın `LRS` . `-Kind` `FileStorage` Bunun yerine, `StorageV2` bir GPv2 depolama hesabı yerine Premium dosya paylaşımlarının bir FileStorage depolama hesabında oluşturulması gerekir.

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $region `
    -Kind FileStorage 
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLı kullanarak bir depolama hesabı oluşturmak için az Storage Account Create komutunu kullanacağız. Bu komutun birçok seçeneği vardır; yalnızca gerekli seçenekler gösterilir. Gelişmiş seçenekler hakkında daha fazla bilgi için, bkz. [ `az storage account create` komut belgeleri](/cli/azure/storage/account).

Depolama hesabının ve sonraki dosya paylaşımının oluşturulmasını basitleştirmek için, çeşitli parametreleri değişkenlerde depolayacağız. Değişken içeriğini istediğiniz değerle değiştirebilirsiniz, ancak depolama hesabı adının genel olarak benzersiz olması gerektiğini unutmayın.

```azurecli-interactive
resourceGroupName="myResourceGroup"
storageAccountName="mystorageacct$RANDOM"
region="westus2"
```

Standart Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanacağız. `--sku`Parametresi, istenen artıklık türü ile ilgilidir; coğrafi olarak yedekli veya coğrafi bölge yedekli depolama hesabı istiyorsanız parametreyi de kaldırmanız gerekir `--enable-large-file-share` .

```azurecli-interactive
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind StorageV2 \
    --sku Standard_ZRS \
    --enable-large-file-share \
    --output none
```

Premium Azure dosya paylaşımlarını depolayabilen bir depolama hesabı oluşturmak için aşağıdaki komutu kullanacağız. `--sku`Parametresinin hem hem de `Premium` istenen artıklık düzeyini yerel olarak yedekli () içerecek şekilde değiştirildiğini unutmayın `LRS` . `--kind` `FileStorage` Bunun yerine, `StorageV2` bir GPv2 depolama hesabı yerine Premium dosya paylaşımlarının bir FileStorage depolama hesabında oluşturulması gerekir.

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
Depolama hesabınızı oluşturduktan sonra, dosya paylaşımınızın oluşturulması gerekir. Bu işlem, genellikle bir Premium dosya paylaşımının veya standart dosya paylaşımının kullanılıp kullanılmadığından bağımsız olarak aynıdır. Birincil fark, **kotanın** ve gösterdiği şeydir.

Standart dosya paylaşımları için bu, son kullanıcıların gidebileceği Azure dosya paylaşımının üst sınırıdır. Standart dosya paylaşımının kotasının birincil amacı budgetary: "Bu dosya paylaşımının bu noktanın ötesine büyümesini istemiyorum". Bir kota belirtilmemişse standart dosya paylaşımı 100 TiB 'ye yayılabilir (veya büyük dosya paylaşımları özelliği bir depolama hesabı için ayarlanmamışsa, 5 TiB).

Premium dosya paylaşımları için kotanın **sağlanan boyut**ile aşırı yüklenmiş olması gerekir. Sağlanan boyut, fiili kullanımdan bağımsız olarak faturalandırılacaksınız. Premium bir dosya paylaşma sağladığınızda iki faktörü göz önünde bulundurmanız gerekir: 1) paylaşımın bir alan kullanım perspektifinden ve 2) iş yükünüz için gereken ıOPS. Her sağlanan GiB size ek ayrılmış ve patlama ıOPS sahibine. Premium dosya paylaşımının nasıl planlanacağı hakkında daha fazla bilgi için bkz. [Premium dosya paylaşımlarını sağlama](storage-files-planning.md#understanding-provisioning-for-premium-file-shares).

# <a name="portal"></a>[Portal](#tab/azure-portal)
Depolama hesabınızı yeni oluşturduysanız, **Kaynağa Git**' i seçerek dağıtım ekranından bu sayfaya gidebilirsiniz. Depolama hesabını daha önce oluşturduysanız, onu içeren kaynak grubu aracılığıyla buna gidebilirsiniz. Depolama hesabında bir kez, **dosya paylaşımları** etiketli kutucuğu seçin (depolama hesabı için içindekiler tablosu aracılığıyla **dosya paylaşımlarına** da gidebilirsiniz).

![Dosya paylaşımları kutucuğunun ekran görüntüsü](media/storage-how-to-create-file-share/create-file-share-1.png)

Dosya paylaşımı listesinde, bu depolama hesabında daha önce oluşturduğunuz herhangi bir dosya paylaşımını görmeniz gerekir; henüz bir dosya paylaşımı oluşturulmadıysa boş bir tablo. Yeni bir dosya paylaşma oluşturmak için **+ dosya paylaşma** ' yı seçin.

Yeni dosya paylaşma dikey penceresi ekranda görünmelidir. Yeni dosya paylaşma dikey penceresindeki alanları tamamlayarak bir dosya paylaşma oluşturun:

- **Ad**: oluşturulacak dosya paylaşımının adı.
- **Kota**: Standart dosya paylaşımları için dosya paylaşımının kotası; Premium dosya paylaşımları için dosya paylaşımının sağlanan boyutu.

Yeni paylaşımın oluşturulmasını bitirilirken **Oluştur** ' u seçin. Depolama hesabınız bir sanal ağda ise, istemciniz sanal ağda de olmadığı takdirde, başarıyla bir Azure dosya paylaşımının oluşturabileceksiniz. Ayrıca, Azure PowerShell cmdlet 'ini kullanarak bu zaman noktası sınırlamasını de çözebilirsiniz `New-AzRmStorageShare` .

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Cmdlet ile Azure dosya paylaşımının oluşturabilirsiniz [`New-AzRmStorageShare`](/powershell/module/az.storage/New-AzRmStorageShare) . Aşağıdaki PowerShell komutları, `$resourceGroupName` `$storageAccountName` Azure PowerShell ile depolama hesabı oluşturma bölümünde değişkenleri ve yukarıda tanımlanan şekilde ayarlamış olduğunuz varsayılır. 

> [!Important]  
> Premium dosya paylaşımları için parametresi, `-QuotaGiB` dosya paylaşımının sağlanan boyutunu ifade eder. Dosya paylaşımının sağlanan boyutu, kullanımdan bağımsız olarak, faturalandırılacak tutardır. Standart dosya paylaşımları, sağlanan boyut yerine kullanıma göre faturalandırılır.

```azurepowershell-interactive
$shareName = "myshare"

New-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -QuotaGiB 1024 | Out-Null
```

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarını ve dosyaları adlandırma hakkında tüm ayrıntılar için bkz. [adlandırma ve başvuru paylaşımları, dizinler, dosyalar ve meta veriler](https://msdn.microsoft.com/library/azure/dn167011.aspx).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Azure CLı ile bir Azure dosya paylaşma oluşturabilmeniz için, ile dosya paylaşma oluşturma işlemini yetkilendirmek üzere bir depolama hesabı anahtarı almalısınız. Bu [`az storage account keys list`](/cli/azure/storage/account/keys) komut şu komutla yapılabilir:

```azurecli-interactive
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')
```

Depolama hesabı anahtarına sahip olduktan sonra, komutuyla Azure dosya paylaşımından bir kayıt oluşturabilirsiniz [`az storage share create`](/cli/azure/storage/share) . 

> [!Important]  
> Premium dosya paylaşımları için parametresi, `--quota` dosya paylaşımının sağlanan boyutunu ifade eder. Dosya paylaşımının sağlanan boyutu, kullanımdan bağımsız olarak, faturalandırılacak tutardır. Standart dosya paylaşımları, sağlanan boyut yerine kullanıma göre faturalandırılır.

```azurecli-interactive
shareName="myshare"

az storage share create \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --name $shareName \
    --quota 1024 \
    --output none
```

Depolama hesabı bir sanal ağ içinde yer alıyorsa ve bu komutu çağırdığınız bilgisayar sanal ağın bir parçası değilse, bu komut başarısız olur. `New-AzRmStorageShare`Yukarıda açıklandığı gibi Azure PowerShell cmdlet 'ini kullanarak veya BIR VPN bağlantısı aracılığıyla sanal ağın bir parçası olan bir bilgisayardan Azure CLI 'yi yürüterek bu zaman noktası sınırlamasını geçici olarak çözebilirsiniz.

---

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarını ve dosyaları adlandırma hakkında tüm ayrıntılar için bkz. [adlandırma ve başvuru paylaşımları, dizinler, dosyalar ve meta veriler](https://msdn.microsoft.com/library/azure/dn167011.aspx).

### <a name="create-a-hot-or-cool-file-share"></a>Sık erişimli veya seyrek erişimli dosya paylaşma oluşturma
**Genel amaçlı v2 (GPv2) depolama hesabındaki** bir dosya paylaşımı, işlem için iyileştirilmiş, sık erişimli veya seyrek erişimli dosya paylaşımları (veya bir karışımı) içerebilir. İşlem için iyileştirilmiş paylaşımlar tüm Azure bölgelerinde kullanılabilir, ancak sık ve seyrek erişimli dosya paylaşımları yalnızca [bölgelerin bir alt kümesinde](storage-files-planning.md#storage-tiers)kullanılabilir. Azure PowerShell Preview modülünü veya Azure CLı kullanarak sık erişimli veya seyrek erişimli bir dosya paylaşma oluşturabilirsiniz. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal, sık ve seyrek dosya paylaşımları oluşturmayı veya var olan işlem için iyileştirilmiş dosya paylaşımlarını sık erişimli veya seyrek erişimli olarak taşımayı desteklemez. Lütfen PowerShell veya Azure CLı ile bir dosya paylaşma oluşturma yönergelerini görüntüleyin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell
# Update the Azure storage module to use the preview version. You may need to close and 
# reopen PowerShell before running this command. If you are running PowerShell 5.1, ensure 
# the following:
# - Run the below cmdlets as an administrator.
# - Have PowerShellGet 2.2.3 or later. Uncomment the following line to check.
# Get-Module -ListAvailable -Name PowerShellGet
Remove-Module -Name Az.Storage -ErrorAction SilentlyContinue
Uninstall-Module -Name Az.Storage
Install-Module -Name Az.Storage -RequiredVersion "2.1.1-preview" -AllowClobber -AllowPrerelease 

# Assuming $resourceGroupName and $storageAccountName from earlier in this document have already
# been populated. The access tier parameter may be TransactionOptimized, Hot, or Cool for GPv2 
# storage accounts. Standard tiers are only available in standard storage accounts. 
$shareName = "myhotshare"

New-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -AccessTier Hot

# You can also change an existing share's tier.
Update-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -AccessTier Cool
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Belirli bir katmana bir dosya paylaşımının oluşturulması veya taşınması işlevselliği, en son Azure CLı güncelleştirmesinde bulunabilir. Azure CLı 'nın güncelleştirilmesi, kullanmakta olduğunuz işletim sistemi/Linux dağıtımına özeldir. Azure CLı 'yi sisteminizde güncelleştirme hakkında yönergeler için bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

```bash
# Assuming $resourceGroupName and $storageAccountName from earlier in this document have already
# been populated. The access tier parameter may be TransactionOptimized, Hot, or Cool for GPv2
# storage accounts. Standard tiers are only available in standard storage accounts.
shareName="myhotshare"

az storage share-rm create \
    --resource-group $resourceGroupName \
    --storage-account $storageAccountName \
    --name $shareName \
    --access-tier "Hot"
```
---

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosyaları dağıtımını planlayın](storage-files-planning.md) veya [Azure dosya eşitleme dağıtımını planlayın](storage-sync-files-planning.md). 
- [Ağa genel bakış](storage-files-networking-overview.md).
- [Windows](storage-how-to-use-files-windows.md), [MacOS](storage-how-to-use-files-mac.md)ve [Linux](storage-how-to-use-files-linux.md)'ta bir dosya paylaşımının bağlantısını yapın ve bağlayın.