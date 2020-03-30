---
title: Azure Depolama hesabını başka bir bölgeye taşıma | Microsoft Dokümanlar
description: Azure Depolama hesabını başka bir bölgeye nasıl taşıyabileceğinizi gösterir.
services: storage
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: article
ms.date: 09/27/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 8ce949ac997ba7ee38cb057752d89f4b4d22388f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838715"
---
# <a name="move-an-azure-storage-account-to-another-region"></a>Azure Depolama hesabını başka bir bölgeye taşıma

Depolama hesabını taşımak için, başka bir bölgedeki depolama hesabınızın bir kopyasını oluşturun. Ardından, AzCopy'yi veya seçtiğiniz başka bir aracı kullanarak verilerinizi bu hesaba taşıyın.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> 
> * Şablon uy.
> * Hedef bölge ve depolama hesabı adını ekleyerek şablonu değiştirin.
> * Yeni depolama hesabı oluşturmak için şablonu dağıtın.
> * Yeni depolama hesabını yapılandırın.
> * Verileri yeni depolama hesabına taşıyın.
> * Kaynak bölgedeki kaynakları silin.

## <a name="prerequisites"></a>Ön koşullar

- Hesabınızın kullandığı hizmetlerin ve özelliklerin hedef bölgede desteklendirilmesini sağlayın.

- Önizleme özellikleri için, aboneliğinizin hedef bölge için beyaz listeye alındığından emin olun.

<a id="prepare" />

## <a name="prepare"></a>Hazırlama

Başlamak için kaynak yöneticisi şablonu dışa aktarın ve sonra değiştirin. 

### <a name="export-a-template"></a>Şablonu dışarı aktar

Bu şablon, depolama hesabınızı açıklayan ayarlar içerir. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak şablon uyguluyor:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. **Tüm kaynakları** seçin ve ardından depolama hesabınızı seçin.

3. **Ayarlar** > **> Verme şablonunu**seçin.

4. **Dışa Aktarma şablonu** bıçaklarında **İndir'i** seçin.

5. Portaldan indirdiğiniz .zip dosyasını bulun ve dosyayı istediğiniz klasöre çekin.

   Bu zip dosyası şablonu dağıtmak için şablon ve komut dosyalarını oluşturan .json dosyalarını içerir.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell kullanarak bir şablon dışa aktarmak için:

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:

   ```azurepowershell-interactive
   Connect-AzAccount
   ```
2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi taşımak istediğiniz depolama hesabıaboneliğine ayarlayın.

   ```azurepowershell-interactive
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

3. Kaynak depolama hesabınızın şablonunu dışa aktarın. Bu komutlar geçerli dizininize bir json şablonu kaydeder.

   ```azurepowershell-interactive
   $resource = Get-AzResource `
     -ResourceGroupName <resource-group-name> `
     -ResourceName <storage-account-name> `
     -ResourceType Microsoft.Storage/storageAccounts
   Export-AzResourceGroup `
     -ResourceGroupName <resource-group-name> `
     -Resource $resource.ResourceId
   ```

---

### <a name="modify-the-template"></a>Şablonu değiştirme 

Depolama hesabı adını ve bölgesini değiştirerek şablonu değiştirin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak şablonu dağıtmak için:

1. Azure portalda **Kaynak oluştur**’u seçin.

2. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.

3. **Şablon dağıtımı**'nı seçin.

    ![Azure Resource Manager şablon kitaplığı](./media/storage-account-move/azure-resource-manager-template-library.png)

4. **Oluştur'u**seçin.

5. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.

6. **Dosyayı Yükle'yi**seçin ve ardından son bölümde indirdiğiniz **template.json** dosyasını yüklemek için yönergeleri izleyin.

7. **template.json** dosyasında, depolama hesabı adının varsayılan değerini ayarlayarak hedef depolama hesabını adlandırın. Bu örnek, depolama hesabı adının varsayılan `mytargetaccount`değerini .
    
    ```json
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccounts_mysourceaccount_name": {
            "defaultValue": "mytargetaccount",
            "type": "String"
        }
    },
 
8. Edit the **location** property in the **template.json** file to the target region. This example sets the target region to `centralus`.

    ```json
    "resources": [{
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2019-04-01",
         "name": "[parameters('storageAccounts_mysourceaccount_name')]",
         "location": "centralus"
         }]          
    ```
    Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.  Bir bölgenin kodu boşluksuz bölge adıdır, **Orta ABD** = **centralus.**

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak şablonu dağıtmak için:

1. **template.json** dosyasında, depolama hesabı adının varsayılan değerini ayarlayarak hedef depolama hesabını adlandırın. Bu örnek, depolama hesabı adının varsayılan `mytargetaccount`değerini .
    
    ```json
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccounts_mysourceaccount_name": {
            "defaultValue": "mytargetaccount",
            "type": "String"
        }
    },
    ``` 

2. **template.json** dosyasındaki **konum** özelliğini hedef bölgeye edin. Bu örnek, hedef `eastus`bölgeyi .

    ```json
    "resources": [{
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2019-04-01",
         "name": "[parameters('storageAccounts_mysourceaccount_name')]",
         "location": "eastus"
         }]          
    ```

    [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) komutunu çalıştırarak bölge kodları alabilirsiniz.

    ```azurepowershell-interactive
    Get-AzLocation | format-table 
    ```
---

<a id="move" />

## <a name="move"></a>Taşı

Hedef bölgede yeni bir depolama hesabı oluşturmak için şablonu dağıtın. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. **template.json** dosyasını kaydedin.

2. Özellik değerlerini girin veya seçin:

- **Abonelik**: Azure aboneliği seçin.

- **Kaynak grubu**: **Yeni Oluştur**'u seçin ve kaynak grubuna bir ad verin.

- **Konum**: Azure konumu seçin.

3. Onay **kutusunun üzerinde belirtilen hüküm ve koşulları kabul** ediyorum ve ardından Satın Al düğmesini **seç** düğmesini tıklatın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0)ile hedef genel IP dağıtmak istediğiniz abonelik kimliği edinin:

   ```azurepowershell-interactive
   Get-AzSubscription
   ```

2. Şablonunuzu dağıtmak için bu komutları kullanın:

   ```azurepowershell-interactive
   $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

   New-AzResourceGroup -Name $resourceGroupName -Location "$location"
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "<name of your local template file>"  
   ```
---

### <a name="configure-the-new-storage-account"></a>Yeni depolama hesabını yapılandırma

Bazı özellikler şablona dışa aktarmaz, bu nedenle bunları yeni depolama hesabına eklemeniz gerekir. 

Aşağıdaki tabloda bu özelliklerin yanı sıra yeni depolama hesabınıza ekleme kılavuzu listeleneb.red.

| Özellik    | Rehber    |
|--------|-----------|
| **Yaşam döngüsü yönetimi politikaları** | [Azure Blob depolama yaşam döngüsünü yönetme](../blobs/storage-lifecycle-management-concepts.md) |
| **Statik web siteleri** | [Azure Depolama'da statik bir web sitesi barındırma](../blobs/storage-blob-static-website-how-to.md) |
| **Etkinlik abonelikleri** | [Blob depolama olaylarına yanıt verme](../blobs/storage-blob-event-overview.md) |
| **Uyarılar** | [Azure Monitör'ü kullanarak etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-activity-log.md) |
| **İçerik Dağıtım Ağı (CDN)** | [HTTPS üzerinden özel etki alanları olan blob'lara erişmek için Azure CDN'yi kullanın](../blobs/storage-https-custom-domain-cdn.md) |

> [!NOTE] 
> Kaynak depolama hesabı için bir CDN ayarlarsanız, mevcut CDN'nizin kaynağını yeni hesabınızın birincil blob hizmeti bitiş noktası (veya birincil statik web sitesi bitiş noktası) olarak değiştirmeniz gerekir. 

### <a name="move-data-to-the-new-storage-account"></a>Verileri yeni depolama hesabına taşıma

Verilerinizi taşımanın bazı yolları aşağıda verebebilirsiniz.

:heavy_check_mark: **Azure Depolama Gezgini**

  Kullanımı kolay ve küçük veri kümeleri için uygundur. Kapsayıcıları kopyalayabilir ve paylaşımları dosyalayabilir ve ardından hedef hesaba yapıştırabilirsiniz.

  Bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/);

:heavy_check_mark: **AzCopy**

  Bu tercih edilen bir yaklaşımdır. Performans için optimize edilmiş.  Daha hızlı olmasının bir yolu, verilerin doğrudan depolama sunucuları arasında kopyalanmasıdır, böylece AzCopy bilgisayarınızın ağ bant genişliğini kullanmaz. Komut satırında veya özel bir komut dosyasının parçası olarak AzCopy'yi kullanın.

  Bkz. [AzCopy ile başlayın](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

:heavy_check_mark: **Azure Veri Fabrikası** 

  Bu aracı yalnızca AzCopy'nin geçerli sürümünde desteklenmeyen işlevselliğe ihtiyacınız varsa kullanın. Örneğin, AzCopy'nin geçerli sürümünde, hiyerarşik ad alanına sahip hesaplar arasındaki lekeleri kopyalayamazsınız. Ayrıca AzCopy dosya erişim denetim listelerini veya dosya zaman damgalarını korumaz (Örneğin: zaman damgaları oluşturma ve değiştirin). 

  Şu bağlantılara bakın:
  - [Azure Veri Fabrikası'nı kullanarak verileri Azure Blob depolamasına veya Azure Blob depolamasından kopyalama](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
  - [Azure Veri Fabrikası'nı kullanarak Azure Veri Depolama Gen2'ye veya Azure Veri Gölü Depolama Gen2'den veri kopyalama](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
  - [Azure Veri Fabrikası'nı kullanarak verileri Azure Dosya Depolama'dan veya Azure Dosya Depolamasına kopyalama](https://docs.microsoft.com/azure/data-factory/connector-azure-file-storage)
  - [Azure Veri Fabrikası'nı kullanarak Verileri Azure Table depolamasına ve azure tablo depolamasına kopyalama](https://docs.microsoft.com/azure/data-factory/connector-azure-table-storage)

---

## <a name="discard-or-clean-up"></a>Atın veya temizleme

Dağıtımdan sonra, yeniden başlamak isterseniz, hedef depolama hesabını silebilir ve bu makalenin [Hazırla](#prepare) ve [Taşı](#move) bölümlerinde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri işlemek ve bir depolama hesabının hareketini tamamlamak için kaynak depolama hesabını silin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak bir depolama hesabını kaldırmak için:

1. Azure portalında, hizmetler menüsünü açmak için sol taraftaki menüyü genişletin ve depolama hesaplarınızın listesini görüntülemek için **Depolama hesaplarını** seçin.

2. Silmek için hedef depolama hesabını bulun ve girişin sağ tarafındaki **Daha Fazla** düğmesine **(...**) sağ tıklayın.

3. **Sil'i**seçin ve onaylayın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Kaynak grubunu ve yeni depolama hesabı da dahil olmak üzere ilişkili kaynaklarını kaldırmak için [Kaldır-AzStorageAccount](/powershell/module/az.resources/remove-azstorageaccount) komutunu kullanın:

```powershell
Remove-AzStorageAccount -ResourceGroupName  $resourceGroup -AccountName $storageAccount
```
---

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure depolama hesabını bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
