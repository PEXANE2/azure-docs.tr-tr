---
title: Azure etkinlik günlüğünü dışarı aktarma
description: Azure etkinlik günlüğü 'nü arşivleme için depolama alanına veya Azure 'un dışından dışarı aktarmak için Azure Event Hubs dışa aktarın.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: e46574ae7f8faa67c2cc0c1afef1917270f69175
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715888"
---
# <a name="export-azure-activity-log-to-storage-or-azure-event-hubs"></a>Azure etkinlik günlüğünü depolamaya veya Azure Event Hubs dışarı aktarma

> [!IMPORTANT]
> Azure etkinlik günlüğü 'nü Azure depolama 'ya gönderme yöntemi ve Azure Event Hubs, [Tanılama ayarları](diagnostic-settings.md)olarak değiştirilmiştir. Bu makalede kullanım dışı bırakılmakta olan eski yöntem açıklanmaktadır. Bir karşılaştırma için bkz. [Azure etkinlik günlüğü koleksiyonuna güncelleştirme ve dışarı aktarma](diagnostic-settings-legacy.md) .


[Azure etkinlik günlüğü](platform-logs-overview.md) , Azure aboneliğinizde oluşan abonelik düzeyi olaylar hakkında öngörüler sağlar. Etkinlik günlüğünü Azure portal görüntülemeye veya Azure Izleyici tarafından toplanan diğer verilerle çözümlenebileceği bir Log Analytics çalışma alanına kopyalamaya ek olarak, etkinlik günlüğü 'nü bir Azure depolama hesabına arşivlemek veya bir  Olay Hub 'ı.

## <a name="archive-activity-log"></a>Etkinlik günlüğünü Arşivle
Denetim, statik analiz veya yedekleme için günlük verilerinizi 90 günden daha uzun süre saklamak istiyorsanız (bekletme ilkesi üzerinde tam denetim ile), etkinlik günlüğünü bir depolama hesabına arşivleme yararlı olur. Yalnızca 90 gün boyunca olaylarınızı tutmanız gerekiyorsa veya daha az etkinlik günlüğü olayları, 90 gün boyunca Azure platformunda korunduğundan, bir depolama hesabına arşiv ayarlamanız gerekmez.

## <a name="stream-activity-log-to-event-hub"></a>Etkinlik günlüğünü Olay Hub 'ına akış
[Azure Event Hubs](/azure/event-hubs/) , saniye başına milyonlarca olayı alabilen ve işleyesağlayan bir veri akışı platformu ve olay alma hizmetidir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Etkinlik günlüğü için akış özelliğini kullanmanın iki yolu şunlardır:
* **Üçüncü taraf günlük ve telemetri sistemlerine akış**: zaman içinde Azure Event Hubs akışı, etkinlik oturumunuzu üçüncü taraf sıems ve Log Analytics çözümlerine yöneltmek için mekanizma olur.
* **Özel telemetri ve günlüğe kaydetme platformu oluşturma**: zaten özel olarak oluşturulmuş bir telemetri platformudur veya bir tane oluşturmayı düşünüyorsanız, Event Hubs yüksek düzeyde ölçeklenebilir yayımla-abone ol, etkinlik günlüğünü esnek bir şekilde almanızı sağlar.

## <a name="prerequisites"></a>Ön koşullar

### <a name="storage-account"></a>Depolama hesabı
Etkinlik günlüğliğinizi arşivlerken, yoksa [bir depolama hesabı oluşturmanız](../../storage/common/storage-account-create.md) gerekir. İzleme verilerine erişimi daha iyi denetleyebilmeniz için, içinde depolanan diğer, izleme olmayan verileri olan mevcut bir depolama hesabını kullanmamalısınız. Ayrıca günlükleri ve ölçümleri de bir depolama hesabına arşivlerken, tüm izleme verilerini merkezi bir konumda tutmak için aynı depolama hesabını kullanmayı tercih edebilirsiniz.

Ayarı yapılandıran kullanıcının her iki aboneliğe de uygun RBAC erişimi olduğu sürece, depolama hesabının, abonelik yaymasıyla aynı abonelikte olması gerekmez.
> [!NOTE]
>  Şu anda, verileri güvenli bir sanal ağın arkasındaki bir depolama hesabına arşivleme işlemi yapılamaz.

### <a name="event-hubs"></a>Event Hubs
Etkinlik Günlüğliğinizi bir olay hub 'ına gönderiyorsanız, henüz yoksa [bir olay hub 'ı oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir. Daha önce bu Event Hubs ad alanına etkinlik günlüğü olayları akıdıysanız, bu olay hub 'ı yeniden kullanılacaktır.

Paylaşılan erişim ilkesi, akış mekanizmanın sahip olduğu izinleri tanımlar. Event Hubs akışının yönetilmesi, gönderilmesi ve dinlemesi izinlerinin olması gerekir. Event Hubs ad alanı için Yapılandır sekmesinin altındaki Azure portal Event Hubs ad alanı için paylaşılan erişim ilkeleri oluşturabilir veya değiştirebilirsiniz.

Etkinlik günlüğü günlük profilini akışı içerecek şekilde güncelleştirmek için, bu Event Hubs yetkilendirme kuralında ListKey izninizin olması gerekir. Ayarı yapılandıran kullanıcının her iki aboneliğe ve her iki aboneliğe de aynı AAD kiracısına sahip olması koşuluyla, Event Hubs ad alanının, günlükleri yayan abonelikle aynı abonelikte olması gerekmez.

[Bir günlük profili oluşturarak](#create-a-log-profile)etkinlik günlüğünü bir olay hub 'ına akış.

## <a name="create-a-log-profile"></a>Günlük profili oluşturma
Azure etkinlik Günlüğliğinizin bir **günlük profili**kullanarak nasıl verildiğini tanımlarsınız. Her Azure aboneliğinin yalnızca bir günlük profili olabilir. Bu ayarlar, portaldaki etkinlik günlüğü dikey penceresinde **dışarı aktarma** seçeneği aracılığıyla yapılandırılabilir. Ayrıca, [Azure izleyici REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell cmdlet 'LERI veya CLI kullanılarak programlı bir şekilde yapılandırılabilir.

Günlük profili aşağıdakileri tanımlar.

**Etkinlik günlüğünün gönderilmesi gereken yer.** Şu anda, kullanılabilir seçenekler depolama hesabı veya Event Hubs.

**Hangi olay kategorilerinin gönderilmesi gerekir.** Günlük profillerindeki *kategorinin* anlamı ve etkinlik günlüğü olayları farklıdır. Günlük profilinde, *kategorisi* işlem türünü temsil eder (yazma, silme, eylem). Bir etkinlik günlüğü olayında, "* özellik *kategorisi*kaynağı veya olay türünü (örneğin, yönetim, Servicehealth ve uyarı) temsil eder.

**Hangi bölgeler (konumlar) içe aktarılmalıdır.** Etkinlik günlüğündeki çok sayıda olay genel olaylar olduğundan tüm konumları dahil etmelisiniz.

**Etkinlik günlüğünün bir depolama hesabında tutulacağı süre.** Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değer 1 ile 365 arasında herhangi bir sayıda gün olabilir.

Bekletme ilkeleri ayarlandıysa, ancak günlükleri bir depolama hesabında depolamak devre dışıysa, bekletme ilkelerinin hiçbir etkisi olmaz. Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar.


> [!IMPORTANT]
> Microsoft. Insights kaynak sağlayıcısı kayıtlı değilse bir günlük profili oluştururken bir hata alabilirsiniz. Bu sağlayıcıyı kaydetmek için bkz. [Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/management/resource-providers-and-types.md) .


### <a name="create-log-profile-using-the-azure-portal"></a>Azure portal kullanarak günlük profili oluşturma

Azure portal **Olay Hub 'ına ver** seçeneğiyle bir günlük profili oluşturun veya düzenleyin.

1. Azure portal **Azure izleyici** menüsünde **etkinlik günlüğü**' nü seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyimin mor başlığına tıklayın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)

3. Görüntülenen dikey pencerede şunları belirtin:
   * Dışarı aktarılacak olaylara sahip bölgeler. Etkinlik günlüğü genel (bölgesel olmayan) bir günlük olduğundan ve çoğu olayın bunlarla ilişkili bir bölgesi olmadığından, anahtar olaylarını kaçırmadığınızdan emin olmak için tüm bölgeler ' ı seçmeniz gerekir.
   * Depolama hesabına yazmak istiyorsanız:
       * Olaylarını kaydetmek istediğiniz depolama hesabı.
       * Bu olayları depolamada depolamak istediğiniz gün sayısı. 0 günlük bir ayar, günlükleri süresiz olarak korur.
   * Olay Hub 'ına yazmak istiyorsanız:
       * Bu olayları akışa almak için bir olay hub 'ının oluşturulmasını istediğiniz Service Bus ad alanı.

     ![Etkinlik günlüğünü dışarı aktar dikey penceresi](./media/activity-logs-overview/activity-logs-portal-export-blade.png)


4. Bu ayarları kaydetmek için **Kaydet** ' e tıklayın. Ayarlar aboneliğinize hemen uygulanır.


### <a name="configure-log-profile-using-powershell"></a>PowerShell kullanarak günlük profilini yapılandırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Zaten bir günlük profili varsa, önce mevcut günlük profilini kaldırmalı ve ardından yeni bir tane oluşturmanız gerekir.

1. Bir günlük profilinin mevcut olup olmadığını belirlemek için `Get-AzLogProfile` kullanın.  Bir günlük profili varsa, *Name* özelliğine göz önünde varın.

1. *Ad* özelliğinden değeri kullanarak günlük profilini kaldırmak için `Remove-AzLogProfile` kullanın.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzLogProfile -Name "default"
    ```

3. Yeni bir günlük profili oluşturmak için `Add-AzLogProfile` kullanın:

    ```powershell
    Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
    ```

    | Özellik | Gereklidir | Açıklama |
    | --- | --- | --- |
    | Ad |Evet |Günlük profilinizin adı. |
    | StorageAccountId |Hayır |Etkinlik günlüğünün kaydedilmesi gereken depolama hesabının kaynak KIMLIĞI. |
    | serviceBusRuleId |Hayır |İçinde Olay Hub 'larının oluşturulmasını istediğiniz Service Bus ad alanı için kural KIMLIĞI Service Bus. Bu şu biçimdeki bir dizedir: `{service bus resource ID}/authorizationrules/{key name}`. |
    | Konum |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgelerin virgülle ayrılmış listesi. |
    | Retentionındays |Evet |Depolama hesabında olayların saklanacağı gün sayısı (1 ile 365 arasında). Sıfır değeri, günlükleri süresiz olarak depolar. |
    | Kategori |Hayır |Toplanması gereken olay kategorilerinin virgülle ayrılmış listesi. Olası değerler _Write_, _Delete_, and _Action_. |

### <a name="example-script"></a>Örnek betik
Aşağıda, etkinlik günlüğünü hem depolama hesabına hem de Olay Hub 'ına yazan bir günlük profili oluşturmak için örnek bir PowerShell betiği verilmiştir.

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


### <a name="configure-log-profile-using-azure-cli"></a>Azure CLı kullanarak günlük profilini yapılandırma

Zaten bir günlük profili varsa, önce mevcut günlük profilini kaldırmanız ve ardından yeni bir günlük profili oluşturmanız gerekir.

1. Bir günlük profilinin mevcut olup olmadığını belirlemek için `az monitor log-profiles list` kullanın.
2. *Ad* özelliğinden değeri kullanarak günlük profilini kaldırmak için `az monitor log-profiles delete --name "<log profile name>` kullanın.
3. Yeni bir günlük profili oluşturmak için `az monitor log-profiles create` kullanın:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

    | Özellik | Gereklidir | Açıklama |
    | --- | --- | --- |
    | ad |Evet |Günlük profilinizin adı. |
    | storage-account-id |Evet |Etkinlik günlüklerinin kaydedileceği depolama hesabının kaynak KIMLIĞI. |
    | locations |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgelerin boşlukla ayrılmış listesi. `az account list-locations --query [].name`kullanarak aboneliğiniz için tüm bölgelerin bir listesini görüntüleyebilirsiniz. |
    | days |Evet |Olayların saklanacağı gün sayısı, 1 ile 365 arasında. Sıfır değeri günlükleri süresiz olarak depolar (süresiz).  Sıfır ise, etkin parametre false olarak ayarlanmalıdır. |
    |enabled | Evet |TRUE veya False.  Bekletme ilkesini etkinleştirmek veya devre dışı bırakmak için kullanılır.  True ise Days parametresi 0 ' dan büyük bir değer olmalıdır.
    | kategoriler |Evet |Toplanması gereken olay kategorilerinin boşlukla ayrılmış listesi. Olası değerler yazma, silme ve eylem. |



## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/view-activity-logs.md)
* [Azure Izleyici günlüklerine etkinlik günlüğü toplayın](activity-log-collect.md)
