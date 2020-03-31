---
title: Azure Blob depolama etkinliklerini web bitiş noktasına gönderme - Powershell | Microsoft Dokümanlar
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
author: normesta
ms.author: normesta
ms.reviewer: dastanfo
ms.date: 08/23/2018
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: f0dae5ae79234ea29e6b17627fc07abcb3b5dfcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68847161"
---
# <a name="quickstart-route-storage-events-to-web-endpoint-with-powershell"></a>Quickstart: PowerShell ile depolama olaylarını web bitiş noktasına yönlendirin

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Blob depolama olaylarına abone olmak, bir olayı tetiklemek ve sonucu görüntülemek için Azure PowerShell'i kullanıyorsunuz. 

Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

İşiniz bittiğinde, olay verilerinin web uygulamasına gönderildiğini görürsünüz.

![Sonuçları görüntüleme](./media/storage-blob-event-quickstart-powershell/view-results.png)

## <a name="setup"></a>Kurulum

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure PowerShell'in en son sürümünü çalıştırdığınızı gerektirir. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell'i Yükle ve yapılandırma](/powershell/azure/install-Az-ps)konusuna bakın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` Komutla Azure aboneliğinizde oturum açın ve kimlik doğrulaması için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Bu **örnekwestus2** kullanır ve boyunca kullanılmak üzere bir değişken seçim depolar.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun.

Aşağıdaki örnek **westus2** konumunda **gridResourceGroup** adlı bir kaynak grubu oluşturur.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Blob depolama olayları, genel amaçlı v2 depolama hesaplarında ve Blob depolama hesaplarında kullanılabilir. **Genel amaçlı v2** depolama hesapları; Bloblar, Dosyalar, Kuyruklar ve Tablolar dahil olmak üzere tüm depolama hizmetlerine yönelik tüm özellikleri destekler. **Blob depolama hesabı**, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesabıdır. Blob Storage hesapları, genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

[New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount)kullanarak LRS çoğaltma ile bir Blob depolama hesabı oluşturun, ardından kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz. Bu örnek, yerel olarak yedekdepolama (LRS) içeren **gridstorage** adlı bir depolama hesabı oluşturur. 

> [!NOTE]
> Depolama hesabı adları genel bir ad alanında olduğundan, bu komut dosyasında sağlanan ada bazı rasgele karakterler ekleniz gerekir.

```powershell
$storageName = "gridstorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind BlobStorage `
  -AccessTier Hot

$ctx = $storageAccount.Context
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

`<your-site-name>` değerini web uygulamanız için benzersiz bir adla değiştirin. Web uygulaması adı bir DNS girdisinin parçası olduğundan benzersiz olmalıdır.

```powershell
$sitename="<your-site-name>"

New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" `
  -siteName $sitename `
  -hostingPlanName viewerhost
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Şu anda iletilerin görüntülenmediği siteyi görüyor olmalısınız.

[!INCLUDE [event-grid-register-provider-powershell.md](../../../includes/event-grid-register-provider-powershell.md)]

## <a name="subscribe-to-your-storage-account"></a>Depolama hesabınıza abone olma

Olay Izgarası'na hangi olayları izlemek istediğinizi söylemek için bir konuya abone olabilirsiniz. Aşağıdaki örnek, oluşturduğunuz depolama hesabına abone dir ve olay bildiriminin bitiş noktası olarak web uygulamanızdan URL'yi geçirir. Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir.

```powershell
$storageId = (Get-AzStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
$endpoint="https://$sitename.azurewebsites.net/api/updates"

New-AzEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $endpoint `
  -ResourceId $storageId
```

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

![Abonelik olayını görüntüleme](./media/storage-blob-event-quickstart-powershell/view-subscription-event.png)

## <a name="trigger-an-event-from-blob-storage"></a>Blob depolama biriminden bir olay tetikler

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak, bir kapsayıcı ve bir nesne oluşturalım. Sonra nesneyi konteynerin içine yükleyelim.

```powershell
$containerName = "gridcontainer"
New-AzStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Olayı tetiklediniz ve Event Grid, iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Az önce gönderdiğiniz olayı görmek için web uygulamanızı görüntüleyin.

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/gridcontainer/blobs/gridTestFile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "Azure-Storage-PowerShell-d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "application/octet-stream",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/gridcontainer/gridTestFile.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu depolama hesabı ve olay aboneliğiyle çalışmaya devam etmeyi planlıyorsanız, bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi düşünmüyorsanız, bu makalede oluşturduğunuz kaynakları silmek için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Blob depolama Olayları ve Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)
- [Event Grid Hakkında](../../event-grid/overview.md)
