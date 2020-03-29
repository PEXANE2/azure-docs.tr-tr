---
title: Azure Depolama ile Azure PowerShell'i Kullanma | Microsoft Dokümanlar
description: Azure Depolama için Azure PowerShell cmdlets'i nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/16/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: d2404ee58f5f44fbe5625f267e6d1c504d0bd237
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465100"
---
# <a name="using-azure-powershell-with-azure-storage"></a>Azure Storage ile Azure PowerShell’i kullanma

Azure PowerShell, PowerShell komut satırından veya komut dosyasında Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Azure Depolama için bu cmdletler iki kategoriye ayrılır: kontrol düzlemi ve veri düzlemi. Kontrol düzlemi cmdlets depolama hesabı yönetmek için kullanılır - depolama hesapları oluşturmak için, özellikleri ayarlamak, depolama hesaplarını silmek, erişim anahtarlarını döndürmek, ve benzeri. Veri düzlemi cmdlets depolama hesabında *depolanan* verileri yönetmek için kullanılır. Örneğin, blob'lar yüklemek, dosya paylaşımları oluşturmak ve kuyruğa ileti eklemek.

Bu nasıl yapılan makale, depolama hesaplarını yönetmek için yönetim düzlemi cmdlets kullanarak yaygın işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesaplarını listele
> * Varolan bir depolama hesabına başvuru alma
> * Depolama hesabı oluşturma
> * Depolama hesabı özelliklerini ayarlama
> * Erişim anahtarlarını alın ve yeniden oluşturun
> * Depolama hesabınıza erişimi koruma
> * Depolama Analizini Etkinleştir

Bu makalede, Depolama Analitiği etkinleştirme ve bunlara nasıl erişilen, veri düzlemi cmdlets'in nasıl kullanılacağı ve China Cloud, German Cloud ve Government gibi Azure bağımsız bulutlarına nasıl erişilen gibi depolama alanı için diğer birçok PowerShell makalesine bağlantılar sağlanmaktadır Bulut.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu alıştırma, Azure PowerShell modülü Az sürüm 0.7 veya daha sonra gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

Bu alıştırma için komutları normal bir PowerShell penceresine yazabilir veya [Windows PowerShell Tümleşik Komut Dosyası Ortamı'nı (ISE)](/powershell/scripting/components/ise/exploring-the-windows-powershell-ise) kullanabilir ve komutları bir düzenleyiciye yazabilir, ardından örnekler arasında gezinirken bir veya daha fazla komutu test edebilirsiniz. Yürütmek istediğiniz satırları vurgulayabilir ve yalnızca bu komutları çalıştırmak için Seçili Çalıştır'ı tıklatabilirsiniz.

Depolama hesapları hakkında daha fazla bilgi [About Azure storage accounts](storage-create-storage-account.md)için [bkz.](storage-introduction.md)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Abonelikteki depolama hesaplarını listele

Geçerli abonelikteki depolama hesaplarının listesini almak için [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) cmdlet'i çalıştırın.

```powershell
Get-AzStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Depolama hesabına başvuru alma

Ardından, bir depolama hesabına başvurmanız gerekir. Yeni bir depolama hesabı oluşturabilir veya varolan bir depolama hesabına başvuruda bulunabilirsiniz. Aşağıdaki bölümde her iki yöntem de gösterilmektedir.

### <a name="use-an-existing-storage-account"></a>Varolan bir depolama hesabı kullanma

Varolan bir depolama hesabını almak için kaynak grubunun adı ve depolama hesabının adı gerekir. Bu iki alan için değişkenleri ayarlayın ve ardından [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) cmdlet'ini kullanın.

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
```

Şimdi varolan bir depolama hesabını gösteren $storageAccount var.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Aşağıdaki komut dosyası, [Yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount)kullanarak genel amaçlı bir depolama hesabı oluşturmak için nasıl gösterir. Hesabı oluşturduktan sonra, her aramada kimlik doğrulamasını belirtmek yerine sonraki komutlarda kullanılabilecek bağlamını alın.

```powershell
# Get list of locations and select one.
Get-AzLocation | select Location
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzResourceGroup -Name $resourceGroup -Location $location

# Set the name of the storage account and the SKU name.
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context.
$ctx = $storageAccount.Context
```

Komut dosyası aşağıdaki PowerShell cmdlets kullanır:

*   [Get-AzLocation](/powershell/module/az.resources/get-azlocation) -- geçerli konumların listesini alır. Örnek, `eastus` konum için kullanır.

*   [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) -- yeni bir kaynak grubu oluşturur. Kaynak grubu, Azure kaynaklarınızın dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bizimkinin `teststoragerg`adı.

*   [Yeni-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) -- depolama hesabı oluşturur. Örnek kullanır. `testpshstorage`

SKU adı, LRS (Yerel Yedekli Depolama) gibi depolama hesabının çoğaltma türünü gösterir. Çoğaltma hakkında daha fazla bilgi için Azure [Depolama Çoğaltma'ya](storage-redundancy.md)bakın.

> [!IMPORTANT]
> Depolama hesabınızın adı Azure içinde benzersiz olmalı ve küçük olmalıdır. Kuralları ve kısıtlamaları adlandırmak için, [Kapsayıcıları, Blob'ları ve Meta verilerini adlandırma ve başvurma](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)bölümüne bakın.
>

Şimdi yeni bir depolama hesabınız ve ona bir referans var.

## <a name="manage-the-storage-account"></a>Depolama hesabını yönetme

Artık yeni bir depolama hesabına veya varolan bir depolama hesabına başvurunuz olduğuna göre, aşağıdaki bölümde depolama hesabınızı yönetmek için kullanabileceğiniz komutlardan bazıları gösterilmektedir.

### <a name="storage-account-properties"></a>Depolama hesabı özellikleri

Depolama hesabının ayarlarını değiştirmek için [Set-AzStorageAccount'u](/powershell/module/az.storage/set-azstorageaccount)kullanın. Bir depolama hesabının veya içinde bulunduğu kaynak grubunun konumunu değiştiremeseniz de, diğer özelliklerin çoğunu değiştirebilirsiniz. Aşağıda PowerShell kullanarak değiştirebileceğiniz bazı özellikler listelenizdir.

* Depolama hesabına atanan **özel etki alanı.**

* Depolama hesabına atanan **etiketler.** Etiketler genellikle kaynakları faturalandırma amacıyla kategorilere ayırmak için kullanılır.

* **SKU,** Yerel Yedekli Depolama için LRS gibi depolama hesabının çoğaltma ayarıdır. Örneğin, Standart LRS'den Standart\_\_GRS'ye\_veya Standart RAGRS'a geçiş olabilirsiniz. Standart\_ZRS, Standart\_GZRS, Standart\_RAGZRS veya Premium\_LRS'yi diğer SUS'larla değiştiremezsiniz veya diğer SUS'ları bunlarla değiştiremezsiniz.

* Blob depolama hesapları için **erişim katmanı.** Erişim katmanı nın değeri **sıcak** veya **serin**olarak ayarlanır ve depolama hesabını kullanma şeklinize uygun erişim katmanını seçerek maliyetinizi en aza indirmenize olanak tanır. Daha fazla bilgi için [Sıcak, serin ve arşiv depolama katmanlarına](../blobs/storage-blob-storage-tiers.md)bakın.

* Yalnızca HTTPS trafiğine izin verin.

### <a name="manage-the-access-keys"></a>Erişim anahtarlarını yönetme

Azure Depolama hesabı iki hesap anahtarıyla birlikte gelir. Anahtarları almak için [Get-AzStorageAccountKey'i](/powershell/module/az.Storage/Get-azStorageAccountKey)kullanın. Bu örnek, ilk anahtarı alır. Diğerini almak için, `Value[1]` `Value[0]`".

```powershell
$storageAccountKey = `
    (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Anahtarı yeniden oluşturmak için [New-AzStorageAccountKey'i](/powershell/module/az.Storage/New-azStorageAccountKey)kullanın.

```powershell
New-AzStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1
```

Diğer anahtarı yeniden oluşturmak `key2` için, `key1`anahtar adı yerine kullanın.

Anahtarlarınızı yeniden oluşturun ve yeni değeri görmek için yeniden alın.

> [!NOTE]
> Üretim depolama hesabının anahtarını yenilemeden önce dikkatli planlama yapmalısınız. Bir veya her iki anahtarı yeniden oluşturmak, yeniden oluşturulan anahtarı kullanarak herhangi bir uygulamanın erişimini geçersiz klar. Daha fazla bilgi için [bkz.](storage-account-keys-manage.md)


### <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Bir depolama hesabını silmek için [Remove-AzStorageAccount'u](/powershell/module/az.storage/Remove-azStorageAccount)kullanın.

```powershell
Remove-AzStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> Bir depolama hesabını sildiğinizde, hesapta depolanan tüm varlıklar da silinir. Bir hesabı yanlışlıkla silerseniz, hemen Destek'i arayın ve depolama hesabını geri yüklemek için bir bilet açın. Verilerinizin kurtarılması garanti edilmez, ancak bazen işe yarar. Destek bileti çözülene kadar eskisiyle aynı ada sahip yeni bir depolama hesabı oluşturmayın.
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>VNet'ler ve güvenlik duvarlarını kullanarak depolama hesabınızı koruyun

Varsayılan olarak, tüm depolama hesaplarına Internet erişimi olan herhangi bir ağ erişebilir. Ancak, ağ kurallarını yalnızca belirli sanal ağlardaki uygulamaların bir depolama hesabına erişmesine izin verecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için [bkz.](storage-network-security.md)

Makale, aşağıdaki PowerShell cmdlets kullanarak bu ayarları yönetmek için nasıl gösterir:
* [Ekle-AzStorageAccountNetworkRule](/powershell/module/az.Storage/Add-azStorageAccountNetworkRule)
* [Update-AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset)
* [Remove-AzStorageAccountNetworkRule](https://docs.microsoft.com/powershell/module/az.storage/remove-azstorageaccountnetworkrule)

## <a name="use-storage-analytics"></a>Depolama analitiğini kullanma

[Azure Depolama Analitiği,](storage-analytics.md) [Depolama Analizi Ölçümleri](/rest/api/storageservices/about-storage-analytics-metrics) ve [Depolama Analizi Günlüğü'nden](/rest/api/storageservices/about-storage-analytics-logging)oluşur.

**Depolama Analizi Ölçümleri,** Azure depolama hesaplarınız için bir depolama hesabının durumunu izlemek için kullanabileceğiniz ölçümlertoplamak için kullanılır. Ölçümler blobs, dosyalar, tablolar ve kuyruklar için etkinleştirilebilir.

**Depolama Analizi Günlüğe Kaydetme** sunucu tarafında gerçekleşir ve depolama hesabınıza hem başarılı hem de başarısız istekler için ayrıntıları kaydetmenizi sağlar. Bu günlükler, tablolarınıza, kuyruklarınıza ve bloblarınıza karşı okuma, yazma ve silme işlemlerinin ayrıntılarını ve başarısız isteklerin nedenlerini görmenizi sağlar. Azure Dosyaları için günlüğe kaydetme kullanılamıyor.

İzlemeyi [Azure portalı](https://portal.azure.com)PowerShell'i kullanarak veya depolama istemcikitaplığını kullanarak programlı bir şekilde yapılandırabilirsiniz.

> [!NOTE]
> PowerShell'i kullanarak dakika analitiğini etkinleştirebilirsiniz. Bu özellik portalda kullanılamaz.
>

* PowerShell kullanarak Depolama Ölçümleri verilerini nasıl etkinleştirecek ve görüntüleyebilirsiniz öğrenmek için [Depolama analizi ölçümlerine](storage-analytics-metrics.md)bakın.

* PowerShell'i kullanarak Depolama Günlüğü verilerini nasıl etkinleştirip nasıl edineceklerini öğrenmek için [PowerShell'i kullanarak Depolama Günlüğü'ne nasıl etkinleştirilir](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data) ve [Depolama Günlüğü günlük verilerinizi bulma](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)'ya bakın.

* Depolama sorunlarını gidermek için Depolama Ölçümleri ve Depolama Günlüğü'ni kullanma hakkında ayrıntılı bilgi için [bkz.](storage-monitoring-diagnosing-troubleshooting.md)

## <a name="manage-the-data-in-the-storage-account"></a>Depolama hesabındaki verileri yönetme

PowerShell ile depolama hesabınızı nasıl yönetebileceğinizi anladığınıza göre, depolama hesabındaki veri nesnelerine nasıl erişebileceğinizi öğrenmek için aşağıdaki makaleleri kullanabilirsiniz.

* [PowerShell ile lekeler nasıl yönetilir?](../blobs/storage-how-to-use-blobs-powershell.md)
* [PowerShell ile dosyaları yönetme](../files/storage-how-to-use-files-powershell.md)
* [PowerShell ile kuyrukları yönetme](../queues/storage-powershell-how-to-use-queues.md)
* [PowerShell ile Azure Tablo depolama işlemleri gerçekleştirin](../../storage/tables/table-storage-how-to-use-powershell.md)

Azure Cosmos DB Tablo API anahtar teslimi küresel dağıtım, düşük gecikme sonu okuma ve yazma, otomatik ikincil dizin oluşturma ve özel iş ortalığı gibi tablo depolama için premium özellikler sağlar.

* Daha fazla bilgi için Azure [Cosmos DB Tablo API'ye](../../cosmos-db/table-introduction.md)bakın.

## <a name="independent-cloud-deployments-of-azure"></a>Azure'un bağımsız bulut dağıtımları

Çoğu kişi, küresel Azure dağıtımı için Azure Genel Bulutu'nunu kullanır. Ayrıca, egemenlik ve benzeri nedenlerle Microsoft Azure'un bazı bağımsız dağıtımları da vardır. Bu bağımsız dağıtımlara "ortamlar" adı verilir. Kullanılabilir ortamlar şunlardır:

* [Azure Devlet Bulutu](https://azure.microsoft.com/features/gov/)
* [Azure China 21Vianet Cloud Çin'de 21Vianet tarafından işletilen](http://www.windowsazure.cn/)
* [Azure Alman Bulutu](../../germany/germany-welcome.md)

Bu bulutlara ve PowerShell ile depolamalarına nasıl erişici olunacakları hakkında bilgi için lütfen [PowerShell'i kullanarak Azure bağımsız bulutlarında Depolamayı Yönetme'ye](storage-powershell-independent-clouds.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu alıştırma için yeni bir kaynak grubu ve bir depolama hesabı oluşturduysanız, kaynak grubunu kaldırarak oluşturduğunuz tüm varlıkları kaldırabilirsiniz. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturulan depolama hesabı ve kaynak grubunun kendisini kaldırır.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılan makale, depolama hesaplarını yönetmek için yönetim düzlemi cmdlets kullanarak yaygın işlemleri kapsar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Depolama hesaplarını listele
> * Varolan bir depolama hesabına başvuru alma
> * Depolama hesabı oluşturma
> * Depolama hesabı özelliklerini ayarlama
> * Erişim anahtarlarını alın ve yeniden oluşturun
> * Depolama hesabınıza erişimi koruma
> * Depolama Analizini Etkinleştir

Bu makalede ayrıca, veri nesnelerinin nasıl yönetilenyönetilen, Depolama Analitiği'nin nasıl etkinleştirilen ve China Cloud, German Cloud ve Government Cloud gibi Azure bağımsız bulutlarına nasıl erişilen gibi diğer birçok makaleye de atıfta bulunulması sağlanmıştır. Burada referans için bazı daha ilgili makaleler ve kaynaklar şunlardır:

* [Azure Depolama kontrol uçağı PowerShell cmdlets](/powershell/module/az.storage/)
* [Azure Depolama veri düzlemi PowerShell cmdlets](/powershell/module/azure.storage/)
* [Windows PowerShell Başvurusu](/powershell/scripting/overview)
