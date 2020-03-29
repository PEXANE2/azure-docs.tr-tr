---
title: Azure DevTest Labs'dan kişisel veriler silme ve dışa aktarma
description: Genel Veri Koruma Yönetmeliği (GDPR) kapsamındaki yükümlülüklerinizi desteklemek için Azure DevLast Labs hizmetinden kişisel verilerinizi nasıl silip dışa aktarılacanız gerektiğini öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: c87e2fb534480bbf9bbe625d67782e5a11eda18c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169694"
---
# <a name="export-or-delete-personal-data-from-azure-devtest-labs"></a>Azure DevTest Labs'dan kişisel verileri dışa aktarma veya silme
Bu makalede, Azure DevTest Labs hizmetinden kişisel verileri silme ve dışa aktarma adımları sağlanmaktadır. 

## <a name="what-personal-data-does-devtest-labs-collect"></a>DevTest Labs hangi kişisel verileri toplar?
DevTest Labs kullanıcıdan iki ana kişisel veri toplar. Bunlar: kullanıcı e-posta adresi ve kullanıcı nesnekimliği. Bu bilgiler, hizmetin laboratuvar yöneticileri ve laboratuvar kullanıcılarına hizmet içi özellikler sağlaması açısından önemlidir.

### <a name="user-email-address"></a>Kullanıcı e-posta adresi
DevTest Labs, laboratuvar kullanıcılarına otomatik kapatma e-posta bildirimleri göndermek için kullanıcı e-posta adresini kullanır. E-posta, kullanıcılara makinelerinin kapatıldığını haber verir. Kullanıcılar, isterlerse kapatmayı erteleyebilir veya atlayabilir. E-posta adresini laboratuvar düzeyinde veya VM düzeyinde yapılandırırsınız.

**Laboratuvarda e-posta ayarlama:**

![E-postayı laboratuvar düzeyinde ayarlama](./media/personal-data-delete-export/lab-user-email.png)

**VM'de e-posta ayarlama:**

![VM düzeyinde e-posta ayarlama](./media/personal-data-delete-export/vm-user-email.png)

### <a name="user-object-id"></a>Kullanıcı nesne kimliği
DevTest Labs, kullanıcı nesnesi kimliğini, kaynak bilgileriyle ilgili olarak laboratuvar yöneticilerine aylık maliyet eğilimlerini ve maliyetini göstermek için kullanır. Bu, onların laboratuvarları için maliyetleri izlemelerine ve eşikleri yönetmelerine olanak tanır. 

**Geçerli takvim ayı için tahmini maliyet eğilimi:**
![Geçerli takvim ayı için tahmini maliyet eğilimi](./media/personal-data-delete-export/estimated-cost-trend-per-month.png)

**Kaynağa göre tahmini ay-tarih maliyeti:**
![Kaynağa göre tahmini ay-to-date maliyet](./media/personal-data-delete-export/estimated-month-to-date-cost-by-resource.png)


## <a name="why-do-we-need-this-personal-data"></a>Bu kişisel verilere neden ihtiyacımız var?
DevTest Labs hizmeti kişisel verileri operasyonel amaçlar için kullanır. Bu veriler, hizmetin temel özellikleri sunması için çok önemlidir. Kullanıcı e-posta adresine bir saklama ilkesi ayarlarsanız, laboratuvar kullanıcıları e-posta adresleri sistemimizden silindikten sonra otomatik kapatma e-posta bildirimlerini zamanında almazlar. Benzer şekilde, kullanıcı nesnesi kimliklerinin bekletme ilkesine göre silinmesi durumunda, laboratuvar yöneticisi de ay boyunca maliyet eğilimlerini ve laboratuvarlarındaki makineler için kaynak tarafından maliyet görüntülenemez. Bu nedenle, kullanıcının kaynağı Laboratuarda etkin olduğu sürece bu verilerin saklanması gerekir.

## <a name="how-can-i-have-the-system-to-forget-my-personal-data"></a>Kişisel verilerimi unutacak sisteme nasıl sahip olabilirim?
Bir laboratuvar kullanıcısı olarak, bu kişisel verilerin silinmiş olmasını istiyorsanız, bunu Laboratuvar'daki ilgili kaynağı silerek yapabilirsiniz. DevTest Labs hizmeti, silinen kişisel verileri kullanıcı tarafından silindikten 30 gün sonra anonimleştirir.

Örneğin, VM'nizi silerseniz veya e-posta adresinizi kaldırdıysanız, DevTest Labs hizmeti kaynak silindikten 30 gün sonra bu verileri anonimleştirir. Silme işleminden sonraki 30 günlük saklama ilkesi, laboratuvar yöneticisine doğru bir aylık maliyet tahmini sağladığımızı sağlamaktır.

## <a name="how-can-i-request-an-export-on-my-personal-data"></a>Kişisel verilerimiçin nasıl ihracat talep edebilirim?
Azure portalını veya PowerShell'i kullanarak kişisel ve laboratuvar kullanım verilerini dışa aktarabilirsiniz. Veriler iki farklı CSV dosyası olarak dışa aktarılır:

- **disks.csv** - farklı VM'ler tarafından kullanılan diskler hakkında bilgi içerir
- **virtualmachines.csv** - laboratuarda VM hakkında bilgi içerir.

### <a name="azure-portal"></a>Azure portalında
Bir laboratuvar kullanıcısı olarak, DevTest Labs hizmetinin depolayacağı kişisel veriler den bir dışa aktarma talep edebilirsiniz. İhracat talebinde bulunmak için, laboratuvarınızın Genel **Bakış** sayfasındaki **Kişisel veriler** seçeneğine gidin. **İstinye dışa aktarma** düğmesini seçin, Lab yöneticinizin depolama hesabında indirilebilir bir excel dosyası oluşturulabilir bir dosya oluşturmaya başladı. Daha sonra bu verileri görüntülemek için laboratuvar yöneticinize başvurabilirsiniz.

1. Sol menüde **Kişisel verileri** seçin. 

    ![Kişisel veriler sayfası](./media/personal-data-delete-export/personal-data-page.png)
2. Laboratuarı içeren **kaynak grubunu** seçin.

    ![Kaynak grubu seçin](./media/personal-data-delete-export/select-resource-group.png)
3. Kaynak grubundaki **depolama hesabını** seçin.
4. Depolama **hesabı** sayfasında **Blobs'u**seçin.

    ![Blobs döşemesi seçin](./media/personal-data-delete-export/select-blobs-tile.png)
5. Kapsayıcılar listesinde **labresourceusage** adlı kapsayıcıyı seçin.

    ![Blob kabını seçin](./media/personal-data-delete-export/select-blob-container.png)
6. Laboratuvarınızın adını taşıyan **klasörü** seçin. Bu klasörde laboratuarınızda **diskler** ve **sanal makineler** için **csv** dosyaları bulabilirsiniz. Bu csv dosyalarını indirebilir, erişim isteyen laboratuvar kullanıcısının içeriğini filtreleyebilir ve onlarla paylaşabilirsiniz.

    ![CSV dosyasını indirin](./media/personal-data-delete-export/download-csv-file.png)

### <a name="azure-powershell"></a>Azure PowerShell

```powershell
Param (
    [Parameter (Mandatory=$true, HelpMessage="The storage account name where to store usage data")]
    [string] $storageAccountName,

    [Parameter (Mandatory=$true, HelpMessage="The storage account key")]
    [string] $storageKey,

    [Parameter (Mandatory=$true, HelpMessage="The DevTest Lab name to get usage data from")]
    [string] $labName,

    [Parameter (Mandatory=$true, HelpMessage="The DevTest Lab subscription")]
    [string] $labSubscription
    )

#Login
Login-AzureRmAccount

# Set the subscription for the lab
Get-AzureRmSubscription -SubscriptionId $labSubscription  | Select-AzureRmSubscription

# DTL will create this container in the storage when invoking the action, cannot be changed currently
$containerName = "labresourceusage"

# Get the storage context
$Ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageKey 
$SasToken = New-AzureStorageAccountSASToken -Service Blob, File -ResourceType Container, Service, Object -Permission rwdlacup -Protocol HttpsOnly -Context $Ctx

# Generate the storage blob uri
$blobUri = $Ctx.BlobEndPoint + $SasToken

# blobStorageAbsoluteSasUri and usageStartDate are required
$actionParameters = @{
    'blobStorageAbsoluteSasUri' = $blobUri    
}

$startdate = (Get-Date).AddDays(-7)

$actionParameters.Add('usageStartDate', $startdate.Date.ToString())

# Get the lab resource group
$resourceGroupName = (Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' | Where-Object { $_.Name -eq $labName}).ResourceGroupName
    
# Create the lab resource id
$resourceId = "/subscriptions/" + $labSubscription + "/resourceGroups/" + $resourceGroupName + "/providers/Microsoft.DevTestLab/labs/" + $labName + "/"

# !!!!!!! this is the new resource action to get the usage data.
$result = Invoke-AzureRmResourceAction -Action 'exportLabResourceUsage' -ResourceId $resourceId -Parameters $actionParameters -Force
 
# Finish up cleanly
if ($result.Status -eq "Succeeded") {
    Write-Output "Telemetry successfully downloaded for " $labName
    return 0
}
else
{
    Write-Output "Failed to download lab: " + $labName
    Write-Error $result.toString()
    return -1
}
```

Yukarıdaki örnekteki temel bileşenler şunlardır:

- Invoke-AzureRmResourceAction komutu.
   
    ```
    Invoke-AzureRmResourceAction -Action 'exportLabResourceUsage' -ResourceId $resourceId -Parameters $actionParameters -Force
    ```
- İki eylem parametresi
    - **blobStorageAbsoluteSasUri** - Paylaşılan Erişim İmzası (SAS) belirteci ile depolama hesabı URI. PowerShell komut dosyasında, bu değer depolama anahtarı yerine geçirilebilir.
    - **usageStartDate** - Son tarih eylemin yürütüldolduğu geçerli tarih olan veri çekmek için başlangıç tarihi. Parçalı lık gün düzeyindedir, bu nedenle zaman bilgisi ekleseniz bile, yoksayılır.

### <a name="exported-data---a-closer-look"></a>Dışa aktarılan veriler - daha yakından bakmak
Şimdi dışa aktarılan verilere daha yakından bakalım. Daha önce de belirtildiği gibi, veriler başarıyla dışa aktarıldıktan sonra iki CSV dosyası olacaktır. 

**virtualmachines.csv** aşağıdaki veri sütunlarını içerir:

| Sütun adı | Açıklama |
| ----------- | ----------- | 
| SubscriptionId | Laboratuvarın bulunduğu abonelik tanımlayıcısı. |
| Labuid | Laboratuvar için benzersiz GUID tanımlayıcısı. |
| Laboratuvar Adı | Laboratuvarın adı. |
| LabResourceId | Tam nitelikli laboratuvar kaynak kimliği. |
| ResourceGroupName | VM içeren kaynak grubunun adı | 
| ResourceId | VM için tam nitelikli kaynak kimliği. |
| ResourceUid | VM için GUID |
| Adı | Sanal makine adı. |
| CreatedTime | VM'nin oluşturulduğu tarih-saati. |
| Silinen Tarih | VM'nin silindiği tarih-saati. Eğer boşsa, silme henüz gerçekleşmedi. |
| Kaynak Sahibi | VM'nin sahibi. Değer boşsa, talep edilebilir bir VM'dir veya bir hizmet sorumlusu tarafından oluşturulur. |
| FiyatlandırmaTier | VM'nin fiyatlandırma katmanı |
| Kaynak Durumu | VM'nin kullanılabilirlik durumu. Etkin, hala varsa veya Etkin değilse, VM silinmişse. |
| ComputeResourceId | Tam nitelikli sanal makine bilgi işlem kaynak tanımlayıcısı. |
| Talep edilebilir | VM talep edilebilir bir VM ise doğru ayarlayın | 
| ÇevreKimliği | Sanal makinenin oluşturulduğu ortam kaynak tanımlayıcısı. VM bir çevre kaynağının parçası olarak oluşturulmadığında boştur. |
| Son Kullanma Tarihi | VM için son kullanma tarihi. Son kullanma tarihi belirlenmediyse boşolarak ayarlanır.
| GaleriImageReferenceVersion |  VM temel görüntüsünün sürümü. |
| GaleriImageReferenceOffer | VM taban görüntüsü teklifi. |
| GaleriImageReferencePublisher | VM temel görüntüyayıncısı. |
| GaleriImageReferenceSku | VM taban görüntüsünün Sku'su |
| GaleriImageReferenceOsType | VM taban görüntüsünün işletim sistemi tipi |
| CustomImageId | VM tabanı özel görüntütam nitelikli kimliği. |

**disks.csv'de** yer alan veri sütunları aşağıda listelenmiştir:

| Sütun adı | Açıklama | 
| ----------- | ----------- | 
| SubscriptionId | Laboratuarı içeren aboneliğin kimliği |
| Labuid | Laboratuvar için GUID |
| Laboratuvar Adı | Laboratuvarın adı | 
| LabResourceId | Laboratuvar için tam nitelikli kaynak kimliği | 
| ResourceGroupName | Laboratuarı içeren kaynak grubunun adı | 
| ResourceId | VM için tam nitelikli kaynak kimliği. |
| ResourceUid | VM için GUID |
 |Adı | Ekteki diskin adı |
| CreatedTime |Veri diskinin oluşturulduğu tarih ve saat. |
| Silinen Tarih | Veri diskinin silindiği tarih ve saat. |
| Kaynak Durumu | Kaynağın durumu. Kaynak varsa etkin. Etkin değil, silindiğinde. |
| DiskBlobName | Veri diski için blob adı. |
| DiskSizeGB | Veri diskinin boyutu. |
| DiskType | Veri diskinin türü. Standart için 0, Premium için 1. |
| KiralıkByVmId | Veri diskinin eklendiği VM'nin kaynak kimliği. |


> [!NOTE]
> Birden çok laboratuvarla uğraşıyorsanız ve genel bilgi almak istiyorsanız, iki temel sütun, abonelikler arasında benzersiz kimlikler olan **LabUID** ve **ResourceUId'dir.**

İhraç edilen veriler SQL Server, Power BI, vb. gibi araçlar kullanılarak manipüle edilebilir ve görselleştirilebilir. Bu özellik, özellikle laboratuvarınızın kullanımını sizinle aynı Azure aboneliğini kullanmayan yönetim ekibinize bildirmek istediğinizde kullanışlıdır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Laboratuvar için ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
