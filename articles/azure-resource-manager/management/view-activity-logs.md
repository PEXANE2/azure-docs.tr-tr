---
title: Kaynakları izlemek için Azure etkinlik günlüklerini görüntüleme
description: Kullanıcı eylemlerini ve hatalarını incelemek için etkinlik günlüklerini kullanın. Azure portalı PowerShell, Azure CLI ve REST'i gösterir.
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: 9511090099894fad6708843f106570ed029f0c1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75478142"
---
# <a name="view-activity-logs-to-monitor-actions-on-resources"></a>Kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme

Etkinlik günlükleri ile aşağıdakileri belirleyebilirsiniz:

* aboneliğinizdeki kaynaklarda hangi işlemler yapıldı?
* operasyonu kim başlattı
* işlem gerçekleştiğinde
* işlemin durumu
* işlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri

Etkinlik günlüğü, kaynaklarınız için tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Okuma işlemlerini (GET) içermez. Kaynak eylemlerinin listesi için Azure [Kaynak Yöneticisi Kaynak Sağlayıcısı işlemlerine](../../role-based-access-control/resource-provider-operations.md)bakın. Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanabilirsiniz.

Etkinlik günlükleri 90 gün boyunca tutulur. Başlangıç tarihi 90 günden eski olmamak şartıyla istediğiniz tarih aralığını sorgulayabilirsiniz.

Portal, PowerShell, Azure CLI, Insights REST API veya [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/)aracılığıyla etkinlik günlüklerinden bilgi alabilirsiniz.

## <a name="azure-portal"></a>Azure portalında

Portal üzerinden etkinlik günlüklerini görüntülemek için aşağıdaki adımları izleyin:

1. Azure portalı **menüsünde, Monitör'ü**seçin veya herhangi bir sayfadan **Monitör'ü** arayın ve seçin.

    ![Monitörü seçin](./media/view-activity-logs/select-monitor-from-menu.png)

1. **Etkinlik Günlüğü'nü**seçin.

    ![Etkinlik günlüğünü seçin](./media/view-activity-logs/select-activity-log.png)

1. Son operasyonların bir özetini görüyorsunuz. İşlemlere varsayılan bir filtre kümesi uygulanır. Özetteki bilgilere dikkat edin, eylemi kimin başlattığını ve ne zaman gerçekleştiğini içerir.

    ![Son işlemlerin özetini görüntüleme](./media/view-activity-logs/audit-summary.png)

1. Önceden tanımlanmış bir filtre kümesini hızlı bir şekilde çalıştırmak için **Hızlı Öngörüler'i**seçin.

    ![Hızlı öngörüler seçin](./media/view-activity-logs/select-quick-insights.png)

1. Seçeneklerden birini seçin. Örneğin, dağıtımlardan gelen hataları görmek için **Başarısız dağıtımları** seçin.

    ![Başarısız dağıtımları seçme](./media/view-activity-logs/select-failed-deployments.png)

1. Filtrelerin son 24 saat içinde dağıtım hatalarına odaklanacak şekilde değiştirildi. Yalnızca filtrelerle eşleşen işlemler görüntülenir.

    ![Filtreleri görüntüleme](./media/view-activity-logs/view-filters.png)

1. Belirli işlemlere odaklanmak için filtreleri değiştirin veya yenilerini uygulayın. Örneğin, aşağıdaki resim **Zaman Alanı** için yeni bir değer gösterir ve **Kaynak türü** depolama hesaplarına ayarlanır.

    ![Filtre seçeneklerini ayarlama](./media/view-activity-logs/set-filter.png)

1. Sorguyu daha sonra yeniden çalıştırmanız gerekiyorsa, **geçerli filtreleri**sabitle'yi seçin.

    ![Filtreleri sabitleme](./media/view-activity-logs/pin-filters.png)

1. Filtreye bir ad verin.

    ![Ad filtreleri](./media/view-activity-logs/name-filters.png)

1. Filtre panoda kullanılabilir. Azure portalı menüsünde **Pano** seçeneğini belirleyin.

    ![Filtreyi panoda göster](./media/view-activity-logs/activity-log-on-dashboard.png)

1. Portaldan, kaynaktaki değişiklikleri görüntüleyebilirsiniz. Monitor'da varsayılan görünüme geri dön ve kaynağı değiştirmeyi içeren bir işlem seçin.

    ![İşlemi seçin](./media/view-activity-logs/select-operation.png)

1. **Geçmişi Değiştir'i (Önizleme)** seçin ve kullanılabilir işlemlerden birini seçin.

    ![Değişiklik geçmişini seçin](./media/view-activity-logs/select-change-history.png)

1. Kaynaktaki değişiklikler görüntülenir.

    ![Değişiklikleri göster](./media/view-activity-logs/show-changes.png)

Değişiklik geçmişi hakkında daha fazla bilgi edinmek için [kaynak değişikliklerini edinin'e](../../governance/resource-graph/how-to/get-resource-changes.md)bakın.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Günlük girişlerini almak için **Get-AzLog** komutunu çalıştırın. Girişler listesini filtrelemek için ek parametreler sağlarsınız. Başlangıç ve bitiş saati belirtmezseniz, son yedi güne ait girişler döndürülür.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup
```

Aşağıdaki örnek, etkinlik günlüğünün belirli bir süre içinde gerçekleştirilen araştırma işlemlerinde nasıl kullanılacağını gösterir. Başlangıç ve bitiş tarihleri bir tarih biçiminde belirtilir.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-05-05T06:00 -EndTime 2019-05-09T06:00
```

Veya, son 14 gün gibi tarih aralığını belirtmek için tarih işlevlerini kullanabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
```

Belirli bir kullanıcı tarafından gerçekleştirilen eylemleri araştırabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
```

Başarısız işlemler için filtre uygulayabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -Status Failed
```

Bu girişiçin durum iletisi bakarak bir hata odaklanabilirsiniz.

```azurepowershell-interactive
(Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties.Content.statusMessage | ConvertFrom-Json
```

Döndürülen verileri sınırlamak için belirli değerleri seçebilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
```

Belirttiğiniz başlangıç saatine bağlı olarak, önceki komutlar kaynak grubu için uzun bir işlem listesi döndürebilir. Arama ölçütleri sağlayarak sonuçları aradığınız anın adedine filtre uygulayabilirsiniz. Örneğin, işlem türüne göre filtreleyebilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
```

Kaynak değiştirme geçmişini görmek için Kaynak Grafiği'ni kullanabilirsiniz. Daha fazla bilgi için kaynak [değişiklikleri alın'a](../../governance/resource-graph/how-to/get-resource-changes.md)bakın.

## <a name="azure-cli"></a>Azure CLI

Günlük girişlerini almak için, süreyi belirtmek için [az monitör aktivite günlüğü listesini](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) ofset ile çalıştırın.

```azurecli-interactive
az monitor activity-log list --resource-group ExampleGroup --offset 7d
```

Aşağıdaki örnek, etkinlik günlüğünün belirli bir süre içinde gerçekleştirilen araştırma işlemlerinde nasıl kullanılacağını gösterir. Başlangıç ve bitiş tarihleri bir tarih biçiminde belirtilir.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --start-time 2019-05-01 --end-time 2019-05-15
```

Artık var olmayan bir kaynak grubu için bile belirli bir kullanıcı tarafından gerçekleştirilen eylemleri arayabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
```

Başarısız işlemler için filtre uygulayabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d
```

Bu girişiçin durum iletisi bakarak bir hata odaklanabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
```

Döndürülen verileri sınırlamak için belirli değerleri seçebilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
```

Belirttiğiniz başlangıç saatine bağlı olarak, önceki komutlar kaynak grubu için uzun bir işlem listesi döndürebilir. Arama ölçütleri sağlayarak sonuçları aradığınız anın adedine filtre uygulayabilirsiniz. Örneğin, işlem türüne göre filtreleyebilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
```

Kaynak değiştirme geçmişini görmek için Kaynak Grafiği'ni kullanabilirsiniz. Daha fazla bilgi için kaynak [değişiklikleri alın'a](../../governance/resource-graph/how-to/get-resource-changes.md)bakın.

## <a name="rest-api"></a>REST API

Etkinlik günlüğü ile çalışmak için REST işlemleri [Insights REST API'nin](/rest/api/monitor/)bir parçasıdır. Etkinlik günlüğü olaylarını almak için [bkz.](/rest/api/monitor/activitylogs)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinliği günlükleri, aboneliğinizdeki eylemler hakkında daha fazla bilgi edinmek için Power BI ile kullanılabilir. Bkz. [Power BI ve daha fazlasında Azure Etkinlik Günlüklerini Görüntüle ve çözümle.](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)
* Güvenlik ilkeleri ayarlama hakkında bilgi edinmek için [Azure Rol Tabanlı Erişim Denetimi'ne](../../role-based-access-control/role-assignments-portal.md)bakın.
* Altyapı katmanından uygulama dağıtımına kadar uygulamalarınızda yapılan değişiklikler hakkında daha fazla ayrıntı görüntülemek için [Azure Monitor'da Uygulama Değişikliği Çözümlemesi'ni kullanın'](../../azure-monitor/app/change-analysis.md)a bakın.
* Dağıtım işlemlerini görüntüleme komutları hakkında bilgi edinmek için [dağıtım işlemlerini görüntüle'](../templates/deployment-history.md)ye bakın.
* Tüm kullanıcılar için bir kaynak üzerindeki silmeleri nasıl önleyeceğinizi öğrenmek için [Azure Kaynak Yöneticisi ile kaynakları kilitle](lock-resources.md)bölümüne bakın.
* Her Microsoft Azure Kaynak Yöneticisi sağlayıcısı için kullanılabilen işlemler listesini görmek için [Bkz. Azure Kaynak Yöneticisi Kaynak Sağlayıcısı işlemleri](../../role-based-access-control/resource-provider-operations.md)
