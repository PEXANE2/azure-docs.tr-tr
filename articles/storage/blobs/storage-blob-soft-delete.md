---
title: Azure Depolama lekeleri için yumuşak silme | Microsoft Dokümanlar
description: Azure Depolama artık blob nesneleri için yumuşak silme sunar, böylece bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından hatalı bir şekilde değiştirildiğinde veya silindiğinde verilerinizi daha kolay kurtarabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 4deae235ed15d02874ab5cb3470c62e934324364
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234289"
---
# <a name="soft-delete-for-azure-storage-blobs"></a>Azure Depolama blobları için geçici silme

Azure Depolama artık blob nesneleri için yumuşak silme sunar, böylece bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından hatalı bir şekilde değiştirildiğinde veya silindiğinde verilerinizi daha kolay kurtarabilirsiniz.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="how-soft-delete-works"></a>Yumuşak silme nasıl çalışır?

Etkinleştirildiğinde, yumuşak silme, blobs veya blob anlık görüntüleri silindiğinde verilerinizi kaydetmenizi ve kurtarmanızı sağlar. Bu koruma, bir üzerine yazma sonucunda silinen blob verilerine kadar uzanır.

Veriler silindiğinde, kalıcı olarak silinmek yerine yumuşak silinmiş bir duruma geçiş eder. Yumuşak silme olduğunda ve verilerin üzerine yazdığınızda, üzerine yazılan verilerin durumunu kaydetmek için yumuşak silinmiş anlık görüntü oluşturulur. Açık olarak listelenmedikçe yumuşak silinen nesneler görünmezdir. Geçici silinen verilerin kullanım süresi kalıcı olarak sonlanmadan önce verilerin kaç kere kurtarılabileceğini yapılandırabilirsiniz.

Yumuşak silme geriye dönük uyumludur, bu nedenle bu özelliğin sağladığı korumalardan yararlanmak için uygulamalarınızda herhangi bir değişiklik yapmak zorunda kalmamanız gerekir. Ancak, [veri kurtarma](#recovery) yeni bir **Undelete Blob** API tanıttı.

### <a name="configuration-settings"></a>Yapılandırma ayarları

Yeni bir hesap oluşturduğunuzda, yumuşak silme varsayılan olarak kapatılır. Yumuşak silme, varolan depolama hesapları için varsayılan olarak da kapatılır. Bir depolama hesabının ömrü boyunca özelliği istediğiniz zaman açık ve kapalı olarak geçiş yapabilirsiniz.

Özellik daha önce açıkken yumuşak silinen verilerin kaydedildiğini varsayarak, özellik kapatıldığında silinen yumuşak verilere erişmeye ve kurtarabilirsiniz. Yumuşak silmeyi açtığınızda, bekletme süresini de yapılandırmanız gerekir.

Bekletme süresi, silinen yumuşak verilerin depolanması ve kurtarma için kullanılabilir olduğu süreyi gösterir. Açıkça silinen lekeler ve blob anlık görüntüleri için, bekletme dönemi saati veriler silindiğinde başlar. Veriler üzerine yazıldığında yumuşak silme özelliği tarafından oluşturulan yumuşak silinmiş anlık görüntüler için anlık görüntü oluşturulduğunda saat başlar. Şu anda 1 ile 365 gün arasında yumuşak silinmiş verileri saklayabilirsiniz.

Yumuşak silme bekletme süresini istediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş saklama süresi yalnızca yeni silinen veriler için geçerli dir. Daha önce silinen verilerin süresi, bu veriler silindiğinde yapılandırılan bekletme süresine bağlı olarak sona erer. Silinen yumuşak bir nesneyi silmeye çalışmak son kullanma süresini etkilemez.

### <a name="saving-deleted-data"></a>Silinen verileri kaydetme

Yumuşak silme, lekelerin veya blob anlık görüntülerinin silindiği veya üzerine yazıldığı birçok durumda verilerinizi korur.

**Blob Koy**, **Blok Koy**, Blok **Listesi Koy**veya **Blob Kopyala** kullanılarak bir blob üzerine yazıldığında, yazma işlemi önce blob durumunun bir anlık görüntü otomatik olarak oluşturulur. Bu anlık görüntü yumuşak silinmiş anlık görüntüdür; yumuşak silinmiş nesneler açıkça listelenmedikçe görünmezdir. Silinen yumuşak nesneleri nasıl listeleyeceklerini öğrenmek için [Kurtarma](#recovery) bölümüne bakın.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Yumuşak silinen veriler gri, etkin veri ise mavidir. Daha yakın zamanda yazılmış veriler eski verilerin altında görünür. B0 B1 ile üzerine yazıldığında, B0'nin yumuşak silinmiş anlık görüntüsü oluşturulur. B1 B2 ile üzerine yazıldığında, B1'in yumuşak silinmiş anlık görüntüsü oluşturulur.*

> [!NOTE]  
> Yumuşak silme, yalnızca hedef blob'un hesabı için açık olduğunda kopyalama işlemleri için üzerine yazma koruması sağlar.

> [!NOTE]  
> Yumuşak silme, arşiv katmanındaki lekeler için üzerine yazma koruması sağlamaz. Arşivdeki bir blob herhangi bir katmanda yeni bir leke ile üzerine yazılırsa, üzerine yazılmış blob kalıcı olarak sona erer.

Anlık görüntüde **Delete Blob** çağrıldığında, bu anlık görüntü yumuşak silinmiş olarak işaretlenir. Yeni bir anlık görüntü oluşturulmadı.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Yumuşak silinen veriler gri, etkin veri ise mavidir. Daha yakın zamanda yazılmış veriler eski verilerin altında görünür. **Anlık Görüntü Blob** çağrıldığında, B0 anlık görüntü olur ve B1 blob etkin durumudur. B0 anlık görüntüsü silindiğinde, yumuşak silinmiş olarak işaretlenir.*

**Delete Blob** bir taban blob (kendisi bir anlık görüntü olmayan herhangi bir blob) çağrıldığında, bu blob yumuşak silinmiş olarak işaretlenir. Önceki davranışla tutarlı olarak, etkin anlık görüntüleri olan bir blob üzerinde **Blob sil'i** aramak bir hata döndürür. Silin **Blob'u** yumuşak silinmiş anlık görüntülerle bir blob üzerinde aramak bir hata döndürmez. Yumuşak silme açık olduğunda, tek bir işlemde bir lekeyi ve tüm anlık görüntülerini yine de silebilirsiniz. Bunu yapmak, temel lekeve anlık görüntüleri yumuşak silinmiş olarak işaretler.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Yumuşak silinen veriler gri, etkin veri ise mavidir. Daha yakın zamanda yazılmış veriler eski verilerin altında görünür. Burada, B2'yi ve ilişkili tüm anlık görüntüleri silmek için Bir **Silme Blob** çağrısı yapılır. Etkin blob, B2 ve ilişkili tüm anlık görüntüler yumuşak silinmiş olarak işaretlenir.*

> [!NOTE]  
> Yumuşak silinmiş bir blob üzerine yazıldığında, yazma işleminden önce blob durumunun yumuşak silinmiş anlık görüntüsü otomatik olarak oluşturulur. Yeni blob, üzerine yazılmış blob katmanını devralır.

Yumuşak silme, verilerinizi kapsayıcı veya hesap silme durumlarında veya blob meta verileri ve blob özelliklerinin üzerine yazıldığı durumlarda kaydetmez. Bir depolama hesabını hatalı silmeişleminden korumak için Azure Kaynak Yöneticisi'ni kullanarak bir kilit yapılandırabilirsiniz. Daha fazla bilgi edinmek [için Beklenmeyen Değişiklikleri Önlemek için](../../azure-resource-manager/management/lock-resources.md) lütfen Azure Kaynak Yöneticisi makalesine bakın Kaynakları Kilitleyin.

Yumuşak silme açık olduğunda aşağıdaki tablo beklenen davranışı ayrıntıları:

| REST API operasyonu | Kaynak türü | Açıklama | Davranış değişikliği |
|--------------------|---------------|-------------|--------------------|
| [Sil](/rest/api/storagerp/StorageAccounts/Delete) | Hesap | İçerdiği tüm kapsayıcılar ve lekeler de dahil olmak üzere depolama hesabını siler.                           | Değişiklik yok. Silinen hesaptaki kapsayıcılar ve lekeler kurtarılamaz. |
| [Kapsayıcıyı Silme](/rest/api/storageservices/delete-container) | Kapsayıcı | İçerdiği tüm lekeler de dahil olmak üzere kapsayıcıyı siler. | Değişiklik yok. Silinen kapsayıcıdaki lekeler kurtarılamaz. |
| [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob) | Engelleme, Ek ve Sayfa Blobs | Yeni bir leke oluşturur veya bir kapsayıcı içinde varolan bir blob değiştirir | Varolan bir blob'un yerine kullanılırsa, aramadan önce blob durumunun anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce silinmiş bir blob için de geçerlidir ve yalnızca aynı türde bir blob (Blok, Ek veya Sayfa) ile değiştirilirse. Farklı bir türde bir blob ile değiştirilirse, varolan tüm yumuşak silinmiş verilerin süresi kalıcı olarak dolacak. |
| [İkili Büyük Nesneyi Silme](/rest/api/storageservices/delete-blob) | Engelleme, Ek ve Sayfa Blobs | Silme için bir blob veya blob anlık görüntü işaretler. Blob veya anlık görüntü daha sonra çöp toplama sırasında silinir | Bir blob anlık görüntüsünü silmek için kullanılırsa, bu anlık görüntü yumuşak silinmiş olarak işaretlenir. Bir blob silmek için kullanılırsa, bu blob yumuşak silinmiş olarak işaretlenir. |
| [İkili Büyük Nesneyi Kopyalama](/rest/api/storageservices/copy-blob) | Engelleme, Ek ve Sayfa Blobs | Bir kaynak blob'u aynı depolama hesabındaki veya başka bir depolama hesabındaki bir hedef blob'una kopyalar. | Varolan bir blob'un yerine kullanılırsa, aramadan önce blob durumunun anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce silinmiş bir blob için de geçerlidir ve yalnızca aynı türde bir blob (Blok, Ek veya Sayfa) ile değiştirilirse. Farklı bir türde bir blob ile değiştirilirse, varolan tüm yumuşak silinmiş verilerin süresi kalıcı olarak dolacak. |
| [Blok Koy](/rest/api/storageservices/put-block) | Blok Blobları | Blok blobunun bir parçası olarak işlenecek yeni bir blok oluşturur. | Etkin bir blob bir blok işlemek için kullanılırsa, hiçbir değişiklik yoktur. Yumuşak silinmiş bir blob bir blok işlemek için kullanılırsa, yeni bir blob oluşturulur ve yumuşak silinen blob durumunu yakalamak için otomatik olarak bir anlık görüntü oluşturulur. |
| [Blok Listesi Koy](/rest/api/storageservices/put-block-list) | Blok Blobları | Blok blob'u oluşturan blok iD'leri kümesini belirterek bir blob işler. | Varolan bir blob'un yerine kullanılırsa, aramadan önce blob durumunun anlık görüntüsü otomatik olarak oluşturulur. Bu, daha önce yumuşak silinmiş bir blob için de geçerlidir ve yalnızca bir Blok Blob ise. Farklı bir türde bir blob ile değiştirilirse, varolan tüm yumuşak silinmiş verilerin süresi kalıcı olarak dolacak. |
| [Sayfa Koy](/rest/api/storageservices/put-page) | Sayfa Blobları | Sayfa Blob'una çeşitli sayfalar yazar. | Değişiklik yok. Bu işlem kullanılarak üzerine yazılan veya temizlenen Sayfa Blob verileri kaydedilmez ve kurtarılamaz. |
| [Ek Blok](/rest/api/storageservices/append-block) | Ekleme Blobları | Bir Ekblo Blob'un sonuna bir veri bloğu yazar | Değişiklik yok. |
| [Blob Özelliklerini Ayarla](/rest/api/storageservices/set-blob-properties) | Engelleme, Ek ve Sayfa Blobs | Bir blob için tanımlanan sistem özellikleri için değerleri ayarlar. | Değişiklik yok. Üzerine yazılmış blob özellikleri kurtarılamaz. |
| [Blob Meta verilerini ayarlama](/rest/api/storageservices/set-blob-metadata) | Engelleme, Ek ve Sayfa Blobs | Belirtilen blob için kullanıcı tanımlı meta verileri bir veya daha fazla ad değeri çifti olarak ayarlar. | Değişiklik yok. Üzerine yazılmış blob meta verileri kurtarılamaz. |

Bir Sayfa Blob'un üzerine yazmak veya açık aralıkları için "Sayfa Koy" çağrısının anlık görüntü oluşturmayacağını fark etmek önemlidir. Sanal makine diskleri Sayfa Blobs tarafından yedeklenir ve veri yazmak için **Sayfayı Koy'u** kullanın.

### <a name="recovery"></a>Kurtarma

[Undelete Blob](/rest/api/storageservices/undelete-blob) işlemini yumuşak silinmiş bir taban blob üzerinde aramak, bu işlemi geri yükler ve ilişkili tüm yumuşak silinmiş anlık görüntüleri etkin olarak geri yüklenir. İşlemi `Undelete Blob` etkin bir temel blob üzerinde çağırmak, ilişkili tüm yumuşak silinmiş anlık görüntüleri etkin olarak geri yükler. Anlık görüntüler etkin olarak geri yüklendiğinde, kullanıcı tarafından oluşturulan anlık görüntüler gibi görünür; taban blob üzerine yazmayın.

Belirli bir yumuşak silinmiş anlık görüntüiçin bir `Undelete Blob` blob geri yüklemek için, temel blob arayabilirsiniz. Ardından, anlık görüntünün şu anda etkin olan blob üzerinden kopyalanması naneden sonra. Anlık görüntünün yeni bir blob'a kopyalanması da olabilir.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Yumuşak silinen veriler gri, etkin veri ise mavidir. Daha yakın zamanda yazılmış veriler eski verilerin altında görünür. Burada, **Undelete Blob** blob b denir, bu nedenle baz blob geri, B1, ve tüm ilişkili anlık görüntüler, burada sadece B0, aktif olarak. İkinci adımda, B0 taban blob üzerinde kopyalanır. Bu kopyalama işlemi, B1'in yumuşak silinmiş anlık görüntüsünü oluşturur.*

Yumuşak silinmiş lekeleri ve blob anlık görüntülerini görüntülemek için silinen verileri **Liste Blobs'a**eklemeyi seçebilirsiniz. Yalnızca yumuşak silinmiş taban lekeleri görüntülemeyi veya yumuşak silinmiş blob anlık görüntülerini de eklemeyi seçebilirsiniz. Silinen tüm veriler için, verilerin silindiği zamanı ve verilerin kalıcı olarak süresinin dolmasından önceki gün sayısını görüntüleyebilirsiniz.

### <a name="example"></a>Örnek

Yumuşak silme açıldığında *HelloWorld* adlı bir blob yükleyen, üzerine yazan, anlık görüntüler, silen ve geri yükleyen bir .NET komut dosyasının konsol çıktısı aşağıda verilmiştir:

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

Bu çıktıyı üreten uygulamaya işaretçi için [Sonraki adımlar](#next-steps) bölümüne bakın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Silinen tüm yumuşak veriler etkin verilerle aynı oranda faturalandırılır. Yapılandırılan bekletme döneminden sonra kalıcı olarak silinen veriler için ücretlendirilmezsiniz. Anlık görüntülere daha derin bir dalış ve nasıl ücret tahakkuk ettigini anlamak için lütfen [anlık görüntülerin ücretleri nasıl tahakkuk ettirdiklerini anlama](storage-blob-snapshots.md)' ya bakın.

Anlık görüntüotomatik nesil ile ilgili işlemler için faturalandırılacak. **Undelete Blob** işlemleri için yazma işlemleri için işlem hızında faturalandırılırsınız.

Genel olarak Azure Blob Depolama fiyatları hakkında daha fazla bilgi için [Azure Blob Depolama Fiyatlandırma Sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)göz atın.

Başlangıçta yumuşak silme özelliğini açtığınızda, özelliğin faturanızı nasıl etkileyeceğini daha iyi anlamak için küçük bir bekletme süresi kullanmanızı öneririz.

## <a name="get-started"></a>Kullanmaya başlayın

Aşağıdaki adımlar, yumuşak silme ile nasıl başlanıncaya kadar başlatılacaklarını gösterir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak depolama hesabınızdaki lekeler için yumuşak silmeyi etkinleştirin:

1. Azure [portalında](https://portal.azure.com/)depolama hesabınızı seçin. 

2. **Blob Hizmeti**altında **Veri Koruma** seçeneğine gidin.

3. **Blob yumuşak silme** altında **Etkin'e** tıklayın

4. **Bekletme ilkeleri** altında *saklamak* istediğiniz gün sayısını girin

5. Veri Koruması ayarlarınızı onaylamak için **Kaydet** düğmesini seçin

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Silinen yumuşak lekeleri görüntülemek için **silinmiş blobs** onay kutusunu göster'i seçin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Belirli bir blob için silinen yumuşak anlık görüntüleri görüntülemek için, blob'u seçin ve **ardından anlık görüntüleri görüntüle'yi tıklatın.**

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

**Silinen anlık görüntüleri göster** onay kutusunun seçildiğinden emin olun.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Yumuşak silinmiş bir blob veya anlık görüntü ye tıkladığınızda, yeni blob özelliklerine dikkat edin. Nesnenin ne zaman silindiği ve blob veya blob anlık görüntüsünün kalıcı olarak süresi nin dolmasına kadar kaç gün kaldığını gösterir. Yumuşak silinen nesne anlık görüntü değilse, silme seçeneğiniz de vardır.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Bir blob undeleting de ilişkili tüm anlık görüntüleri silmek olacağını unutmayın. Etkin bir blob için silinmiş yumuşak anlık görüntüleri undelete için, blob tıklayın ve **Tüm anlık görüntüleri Sil'i**seçin.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Bir blob'un anlık görüntülerini sildikten sonra, kök lekesinin üzerinde anlık görüntü kopyalamak için **Tanıtımı'nı** tıklatarak lekeyi anlık görüntüye geri geri gönderebilirsiniz.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Yumuşak silmeyi etkinleştirmek için, blob istemcisi hizmet özelliklerini güncelleştirin. Aşağıdaki örnek, abonelikteki bir hesap alt kümesi için yumuşak silme sağlar:

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```
Aşağıdaki komutu kullanarak yumuşak silmenin açık olduğunu doğrulayabilirsiniz:

```powershell
$MatchingAccounts | $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context | Select-Object -ExpandProperty DeleteRetentionPolicy
```

Yanlışlıkla silinen lekeleri kurtarmak için bu lekelerden Undelete'i arayın. **Undelete Blob'u**hem etkin hem de yumuşak silinmiş lekelerde arayarak ilişkili tüm yumuşak silinmiş anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kaptaki tüm yumuşak silinmiş ve etkin lekelerde Undelete'i çağırır:

```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Geçerli yumuşak silme bekletme ilkesini bulmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

# <a name="cli"></a>[CLI](#tab/azure-CLI)

Yumuşak silmeyi etkinleştirmek için, blob istemcisi hizmet özelliklerini güncelleştirin:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Yumuşak silmenin açık olduğunu doğrulamak için aşağıdaki komutu kullanın: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

# <a name="python"></a>[Python](#tab/python)

Yumuşak silmeyi etkinleştirmek için, blob istemcisi hizmet özelliklerini güncelleştirin:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(
    account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(
    delete_retention_policy=DeleteRetentionPolicy(enabled=True, days=7))
```

# <a name="net"></a>[.NET](#tab/net)

Yumuşak silmeyi etkinleştirmek için, blob istemcisi hizmet özelliklerini güncelleştirin:

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Yanlışlıkla silinen lekeleri kurtarmak için bu lekelerden Undelete'i arayın. **Undelete Blob'u**hem etkin hem de yumuşak silinmiş lekelerde arayarak ilişkili tüm yumuşak silinmiş anlık görüntüleri etkin olarak geri yükleyeceğini unutmayın. Aşağıdaki örnek, bir kaptaki tüm yumuşak silinmiş ve etkin lekelerde Undelete'i çağırır:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Belirli bir blob sürümüne kurtarmak için önce blob'da Undelete'i arayın, ardından blob üzerinde istediğiniz anlık görüntükopyalayın. Aşağıdaki örnek, en son oluşturulan anlık görüntüsüne bir blok blob kurtarır:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

---

## <a name="special-considerations"></a>Özel hususlar

Verilerinizin yanlışlıkla bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla değiştirilme veya silinme olasılığı varsa, yumuşak silmeyi açmanız önerilir. Sık yazılan veriler için yumuşak silmeyi etkinleştirmek, kalıpları listelerken depolama kapasitesi ücretlerinin artmasına ve gecikme gecikmesine neden olabilir. Bu ek maliyeti ve gecikme yi hafifleterek, sık sık üzerine yazılan verileri yumuşak silmenin devre dışı bırakıldığı ayrı bir depolama hesabında saklayabilirsiniz. 

## <a name="faq"></a>SSS

### <a name="for-which-storage-services-can-i-use-soft-delete"></a>Hangi depolama hizmetleri için yumuşak silme kullanabilirim?

Şu anda, yumuşak silme yalnızca blob (nesne) depolama için kullanılabilir.

### <a name="is-soft-delete-available-for-all-storage-account-types"></a>Yumuşak silme tüm depolama hesabı türleri için kullanılabilir mi?

Evet, blob depolama hesaplarının yanı sıra genel amaçlı (Hem GPv1 hem de GPv2) depolama hesapları için de yumuşak silme kullanılabilir. Hem standart hem de premium hesap türleri desteklenir. Yumuşak silme, kapakların altındaki sayfa lekeleri olan yönetilmeyen diskler için kullanılabilir. Yönetilen diskler için yumuşak silme kullanılamaz.

### <a name="is-soft-delete-available-for-all-storage-tiers"></a>Yumuşak silme tüm depolama katmanları için kullanılabilir mi?

Evet, yumuşak silme, sıcak, serin ve arşiv dahil olmak üzere tüm depolama katmanları için kullanılabilir. Ancak, yumuşak silme arşiv katmanındaki lekeler için üzerine yazma koruması sağlamaz.

### <a name="can-i-use-the-set-blob-tier-api-to-tier-blobs-with-soft-deleted-snapshots"></a>Set Blob Tier API'yi yumuşak silinmiş anlık görüntülerle katman blobları için kullanabilir miyim?

Evet. Silinen yumuşak anlık görüntüler özgün katmanda kalır, ancak temel leke yeni katmana taşınır. 

### <a name="premium-storage-accounts-have-a-per-blob-snapshot-limit-of-100-do-soft-deleted-snapshots-count-toward-this-limit"></a>Premium depolama hesaplarının her blob anlık görüntü sınırı 100'e sahiptir. Yumuşak silinen anlık görüntüler bu sınıra doğru sayılır mı?

Hayır, silinen yumuşak anlık görüntüler bu sınıra dahil edilmez.

### <a name="can-i-turn-on-soft-delete-for-existing-storage-accounts"></a>Varolan depolama hesapları için yumuşak silmeyi açabilir miyim?

Evet, yumuşak silme hem varolan hem de yeni depolama hesapları için yapılandırılabilir.

### <a name="if-i-delete-an-entire-account-or-container-with-soft-delete-turned-on-will-all-associated-blobs-be-saved"></a>Yumuşak silme açık olan bir hesabın veya konteynerin tamamını silersem, ilişkili tüm lekeler kaydedilir mi?

Hayır, bir hesabın veya kapsayıcının tamamını silerseniz, ilişkili tüm lekeler kalıcı olarak silinir. Bir depolama hesabını yanlışlıkla silmekten koruma hakkında daha fazla bilgi için, [Beklenmeyen Değişiklikleri Önlemek için Kaynakları Kilitle'ye](../../azure-resource-manager/management/lock-resources.md)bakın.

### <a name="can-i-view-capacity-metrics-for-deleted-data"></a>Silinen verilerin kapasite ölçümlerini görüntüleyebilir miyim?

Yumuşak silinen veriler, toplam depolama hesabı kapasitenizin bir parçası olarak dahil edilir. Depolama kapasitesini izleme ve izleme hakkında daha fazla bilgi için Storage [Analytics](../common/storage-analytics.md)bölümüne bakın.

### <a name="if-i-turn-off-soft-delete-will-i-still-be-able-to-access-soft-deleted-data"></a>Yumuşak silmeyi kapatırsam, silinen yumuşak verilere erişmeye devam edebilecek miyim?

Evet, yumuşak silme kapatıldığında süresi dolmamış yumuşak silinmiş verilere erişebilir ve kurtarabilirsiniz.

### <a name="can-i-read-and-copy-out-soft-deleted-snapshots-of-my-blob"></a>Lekemin yumuşak silinmiş anlık görüntülerini okuyup kopyalayabilir miyim?  

Evet, ama önce blob'dan Undelete'i aramalısınız.

### <a name="is-soft-delete-available-for-all-blob-types"></a>Yumuşak silme tüm blob türleri için kullanılabilir mi?

Evet, blok lekeleri, ek blobs ve sayfa lekeleri için yumuşak silme kullanılabilir.

### <a name="is-soft-delete-available-for-virtual-machine-disks"></a>Sanal makine diskleri için yumuşak silme kullanılabilir mi?  

Yumuşak silme, kapakların altındaki sayfa lekeleri olan hem premium hem de standart yönetilmeyen diskler için kullanılabilir. Yumuşak silme yalnızca **Blob Sil**tarafından silinen verileri kurtarmak yardımcı olacaktır , **Blob koy**, **Blok Listesi koy**, Blok **ve** **Kopya Blob** işlemleri. **Put Page** için yapılan bir çağrı tarafından üzerine yazılan veriler kurtarılamaz.

Azure sanal makinesi **Sayfa yı Koy'a**çağrıları kullanarak yönetilmeyen bir diske yazar, bu nedenle Azure VM'den yönetilmeyen bir diske yazdığı yumuşak silmeyi kullanmak desteklenen bir senaryo değildir.

### <a name="do-i-need-to-change-my-existing-applications-to-use-soft-delete"></a>Yumuşak silme kullanmak için varolan uygulamalarımı değiştirmem gerekiyor mu?

Kullandığınız API sürümünden bağımsız olarak yumuşak silme avantajlarından yararlanmak mümkündür. Ancak, yumuşak silinmiş lekeleri ve blob anlık görüntülerini listelemek ve kurtarmak için Depolama Hizmetleri REST [API](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) veya daha büyük sürüm 2017-07-29 kullanmanız gerekir. Microsoft, Azure Depolama API'sinin en son sürümünü her zaman kullanmanızı önerir.

## <a name="next-steps"></a>Sonraki adımlar

* [.NET Örnek Kodu](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)
* [Blob Hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
* [Azure Depolama Çoğaltma](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [RA-GRS kullanarak Yüksek Kullanılabilir Uygulamaların Tasarlanması](../common/storage-designing-ha-apps-with-ragrs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure Depolama'da olağanüstü durum kurtarma ve depolama hesabı başarısız (önizleme)](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
