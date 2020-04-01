---
title: Azure Etkinlik Günlüğünü Dışa Aktarma
description: Azure Etkinliği günlüğünü arşivleme için depolamaya veya Azure Dışında dışa aktarmak için Azure Etkinlik günlüğe aktarın.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 12c750f96b8852cdd6a6039ebfa750c2ee792a6b
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396712"
---
# <a name="export-azure-activity-log-to-storage-or-azure-event-hubs"></a>Azure Etkinliği günlüğünü depolama veya Azure Etkinlik Hub'larına dışa aktarma

> [!IMPORTANT]
> Azure Etkinliği günlüğünü Azure Depolama ve Azure Etkinlik Hub'larına gönderme yöntemi [tanı ayarlarına](diagnostic-settings.md)dönüştürülmüştür. Bu makalede, amortismana alınma sürecinde olan eski yöntem açıklanmaktadır. Karşılaştırma için Azure Etkinliği oturum açmak için [Güncelleştirme'ye](activity-log-collect.md) bakın.


[Azure Etkinlik Günlüğü,](platform-logs-overview.md) Azure aboneliğinizde gerçekleşen abonelik düzeyindeki olaylara ilişkin öngörüler sağlar. Azure portalındaki Etkinlik günlüğünü görüntülemenin veya Azure Monitor tarafından toplanan diğer verilerle analiz edilebilen bir Log Analytics çalışma alanına kopyalamanın yanı sıra, Etkinlik günlüğünü bir Azure depolama hesabına arşivlemek veya Etkinlik Hub'ına aktarmak için bir günlük profili oluşturabilirsiniz.

## <a name="archive-activity-log"></a>Arşiv Faaliyet Günlüğü
Denetim, statik çözümleme veya yedekleme için günlük verilerinizi 90 günden daha uzun süre (bekletme ilkesi üzerinde tam denetimle) tutmak istiyorsanız, Etkinlik Günlüğü'nün bir depolama hesabına arşivlemesi yararlıdır. Etkinlik Günlüğü etkinlikleri Azure platformunda 90 gün boyunca tutulduğundan, etkinliklerinizi yalnızca 90 gün veya daha kısa bir süre saklamanız gerekiyorsa, bir depolama hesabına arşiv ayarlamanız gerekmez.

## <a name="stream-activity-log-to-event-hub"></a>Etkinlik Günlüğü'ne Akış Etkinliği Günlüğü
[Azure Etkinlik Hub'ları,](/azure/event-hubs/) saniyede milyonlarca olay alabilen ve işleyebilen bir veri akış platformu ve etkinlik hizmetidir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Etkinlik Günlüğü için akış yeteneğini kullanmanın iki yolu şunlardır:
* **Üçüncü taraf günlük ve telemetri sistemlerine akış**: Zaman içinde Azure Etkinlik Hub'ları akışı, Etkinlik Oturumunuzu üçüncü taraf SIEM'lere ve günlük analizi çözümlerine aktaran mekanizma haline gelecektir.
* **Özel bir telemetri ve günlük platformu oluşturun**: Zaten özel olarak oluşturulmuş bir telemetri platformunuz varsa veya bir platform oluşturmayı düşünüyorsanız, Event Hub'larının yüksek ölçeklenebilir yayımlama-abone yapısı, etkinlik günlüğünü esnek bir şekilde yutmanızı sağlar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="storage-account"></a>Depolama hesabı
Etkinlik Günlüğününüzü arşivliyorsanız, zaten bir depolama hesabınız yoksa [bir depolama hesabı oluşturmanız](../../storage/common/storage-account-create.md) gerekir. İzleme verilerine erişimi daha iyi denetlemek için içinde başka, izlemeyen veriler depolanan varolan bir depolama hesabı kullanmamalısınız. Günlükleri ve ölçümleri bir depolama hesabına da arşivliyorsanız, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı seçebilirsiniz.

Depolama hesabı, ayarı yapılandıran kullanıcının her iki aboneye de uygun RBAC erişimine sahip olması sürece, abonelik yayan günlüklerle aynı abonelikte olması gerekmez. 

> [!TIP]
> Güvenli bir sanal ağın arkasındaki depolama hesabına erişim sağlamak için [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırın.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)

### <a name="event-hubs"></a>Event Hubs
Etkinlik Günlüğününüzü bir etkinlik merkezine gönderiyorsanız, zaten bir etkinlik merkeziniz yoksa [bir etkinlik merkezi oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir. Etkinlik Günlüğü olaylarını daha önce bu Olay Hub'ları ad alanına akışla aktardıysanız, bu olay hub'ı yeniden kullanılır.

Paylaşılan erişim ilkesi, akış mekanizmasının sahip olduğu izinleri tanımlar. Olay Hub'larına akış için Yönetim, Gönderme ve Dinleme izinleri gerektirir. Azure portalındaki Olay Hub'ları ad alanı için ortak erişim ilkeleri oluşturabilir veya değiştirebilirsiniz.

Etkinlik Günlüğü günlüğü profilini akış içerecek şekilde güncelleştirmek için, bu Olay Hub'ları yetkilendirme kuralında ListKey iznine sahip olmalısınız. Etkinlik Hub'ları ad alanı, ayarı yapılandıran kullanıcının her iki aboneye de uygun RBAC erişimine sahip olması ve her iki aboneliğin de aynı AAD kiracısında olması kaydıyla, günlükleri yayan abonelikle aynı abonelikte olması gerekmez.

Günlük Profili oluşturarak Etkinlik Günlüğü'nün Olay [Merkezi'ne](#create-a-log-profile)akışı.

## <a name="create-a-log-profile"></a>Günlük profili oluşturma
Azure Etkinlik günlüğünün günlük **profilini**kullanarak nasıl dışa aktarıldığını tanımlarsınız. Her Azure aboneliğinin yalnızca bir günlük profili olabilir. Bu ayarlar, portaldaki Etkinlik Günlüğü bıçağındaki **Dışa** Aktarma seçeneği ile yapılandırılabilir. Azure [Monitor REST API,](https://msdn.microsoft.com/library/azure/dn931927.aspx)PowerShell cmdlets veya CLI kullanılarak da programlı olarak yapılandırılabilirler.

Günlük profili aşağıdakileri tanımlar.

**Etkinlik Günlüğü'nün gönderilmesi gereken yer.** Şu anda kullanılabilir seçenekler Depolama Hesabı veya Olay Hub'larıdır.

**Hangi olay kategorileri gönderilmelidir.** Log Profilleri ve Etkinlik Günlüğü etkinliklerinde *kategorinin* anlamı farklıdır. Günlük Profilinde *Kategori* işlem türünü (Yazma, Silme, Eylem) temsil eder. Bir Etkinlik Günlüğü etkinliğinde, "* özelliği" *kategorisi*olayın kaynağını veya türünü (örneğin, Yönetim, ServiceHealth ve Alert) temsil eder.

**Hangi bölgeler (konumlar) dışa aktarılmalıdır.** Etkinlik Günlüğü'ndeki birçok olay genel olaylar olduğundan tüm konumları eklemeniz gerekir.

**Etkinlik Günlüğü'nün bir Depolama Hesabında ne kadar süreyle saklanması gerektiği.** Sıfır gün saklama, günlüklerin sınırsız süreyle tutulacağı anlamına gelir. Aksi takdirde, değer 1 ile 365 arasında herhangi bir gün sayısı olabilir.

Bekletme ilkeleri ayarlanır, ancak günlükleri bir depolama hesabında depolama devre dışı bırakılırsa, bekletme ilkeleri nin hiçbir etkisi yoktur. Bekletme ilkeleri günlük olarak uygulanır, bu nedenle günün sonunda (UTC), bekletme ilkesinin ötesindeki güne ait günlükler silinir. Örneğin, bir gün bekletme ilkeniz varsa, bugün günün başında önceki güne ait günlükler silinir. Silme işlemi gece yarısı UTC'de başlar, ancak günlüklerin depolama hesabınızdan silinmesinin 24 saat kadar sürebileceğini unutmayın.


> [!IMPORTANT]
> Microsoft.Insights kaynak sağlayıcısı kayıtlı değilse, günlük profili oluştururken bir hata alabilirsiniz. Bu sağlayıcıyı kaydetmek için [Azure kaynak sağlayıcılarına ve türlerine](../../azure-resource-manager/management/resource-providers-and-types.md) bakın.


### <a name="create-log-profile-using-the-azure-portal"></a>Azure portalını kullanarak günlük profili oluşturma

Azure portalında **Event Hub'a Dışa Aktar** seçeneğiyle bir günlük profili oluşturun veya düzenledi.

1. Azure portalındaki **Azure Monitör** menüsünden **Etkinlik günlüğü'nü**seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyim için mor banner'ı tıklatın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)

3. Görünen bıçakta aşağıdakileri belirtin:
   * İhracat etkinlikleri ile bölgeler. Etkinlik Günlüğü genel (bölgesel olmayan) bir günlük olduğundan ve çoğu olayın bunlarla ilişkili bir bölgesi olmadığından, önemli olayları kaçırmadığınızdan emin olmak için tüm bölgeleri seçmeniz gerekir.
   * Depolama hesabına yazmak istiyorsanız:
       * Olayları kaydetmek istediğiniz Depolama Hesabı.
       * Bu olayları depolama alanında tutmak istediğiniz gün sayısı. 0 gün ayarı günlükleri sonsuza kadar saklar.
   * Olay hub'ına yazmak istiyorsanız:
       * Bu olayları akışı için bir Olay Hub'ı oluşturulmasını istediğiniz Hizmet Veri Merkezi Ad Alanı.

     ![İhracat Faaliyet Günlüğü bıçak](./media/activity-logs-overview/activity-logs-portal-export-blade.png)


4. Bu ayarları kaydetmek için **Kaydet'i** tıklatın. Ayarlar aboneliğinize hemen uygulanır.


### <a name="configure-log-profile-using-powershell"></a>PowerShell'i kullanarak günlük profilini yapılandır

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Günlük profili zaten varsa, önce varolan günlük profilini kaldırmanız ve ardından yeni bir profil oluşturmanız gerekir.

1. Günlük `Get-AzLogProfile` profili olup olmadığını belirlemek için kullanın.  Günlük profili varsa, *ad* özelliğine dikkat edin.

1. Ad `Remove-AzLogProfile` özelliğinden değeri kullanarak günlük *name* profilini kaldırmak için kullanın.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzLogProfile -Name "default"
    ```

3. Yeni `Add-AzLogProfile` bir günlük profili oluşturmak için kullanın:

    ```powershell
    Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
    ```

    | Özellik | Gerekli | Açıklama |
    | --- | --- | --- |
    | Adı |Evet |Günlük profilinizin adı. |
    | Depolama Muhasebesi |Hayır |Etkinlik Günlüğü'nün kaydedilmesi gereken Depolama Hesabı'nın kaynak kimliği. |
    | serviceBusRuleId |Hayır |Olay hub'larının oluşturulmasını istediğiniz Servis Veri Servisi Veri Merkezi ad alanı için Servis Veri Servisi Kural Kimliği. Bu biçimi ile bir `{service bus resource ID}/authorizationrules/{key name}`dize: . |
    | Konum |Evet |Etkinlik Günlüğü etkinliklerini toplamak istediğiniz bölgelerin virgülle ayrılmış listesi. |
    | Bekletme Günleri |Evet |Olayların depolama hesabında 1 ile 365 arasında tutulması gereken gün sayısı. Sıfır değeri günlükleri süresiz olarak saklar. |
    | Kategori |Hayır |Toplanması gereken olay kategorilerinin virgülle ayrılmış listesi. Olası değerler _Yaz_, _Sil_ve _Eylem'dir._ |

### <a name="example-script"></a>Örnek betik
Aşağıda, Etkinlik Günlüğü'nün hem depolama hesabına hem de olay hub'ına yazdığı bir günlük profili oluşturmak için örnek bir PowerShell komut dosyası vereyim.

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```


### <a name="configure-log-profile-using-azure-cli"></a>Azure CLI kullanarak günlük profilini yapılandırma

Günlük profili zaten varsa, önce varolan günlük profilini kaldırmanız ve ardından yeni bir günlük profili oluşturmanız gerekir.

1. Günlük `az monitor log-profiles list` profili olup olmadığını belirlemek için kullanın.
2. Ad `az monitor log-profiles delete --name "<log profile name>` özelliğinden değeri kullanarak günlük *name* profilini kaldırmak için kullanın.
3. Yeni `az monitor log-profiles create` bir günlük profili oluşturmak için kullanın:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

    | Özellik | Gerekli | Açıklama |
    | --- | --- | --- |
    | ad |Evet |Günlük profilinizin adı. |
    | depolama-hesap-id |Evet |Etkinlik Günlüklerinin kaydedilmesi gereken Depolama Hesabının kaynak kimliği. |
    | Konum |Evet |Etkinlik Günlüğü etkinliklerini toplamak istediğiniz bölgelerin boşluktan ayrılmış listesi. Aboneliğiniz için tüm bölgelerin listesini kullanarak `az account list-locations --query [].name`görüntüleyebilirsiniz. |
    | gün |Evet |Olayların korunması gereken gün sayısı, 1 ile 365 arasında. Sıfır değeri günlükleri süresiz olarak (sonsuza kadar) saklar.  Sıfır ise, etkin parametre false olarak ayarlanmalıdır. |
    |enabled | Evet |Doğru veya Yanlış.  Bekletme ilkesini etkinleştirmek veya devre dışı kalmak için kullanılır.  Doğruysa, gün parametresi 0'dan büyük bir değer olmalıdır.
    | kategoriler |Evet |Toplanması gereken olay kategorilerinin boşluktan ayrılmış listesi. Olası değerler Yazma, Silme ve Eylem'dir. |



## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik Günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/view-activity-logs.md)
* [Azure Monitör Günlüklerinde Etkinlik Günlüğü Topla](activity-log-collect.md)
