---
title: Azure Etkinliği günlük olaylarını Azure Monitörü'nde görüntüleme
description: Azure Etkinliği günlüğünü Azure Monitörü'nde görüntüleyin ve PowerShell, CLI ve REST API ile alın.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: d2423d04ead9040cce53d847d24efe75be680d94
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397316"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Azure Etkinliği günlük olaylarını görüntüleme ve alma

[Azure Etkinlik Günlüğü,](platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyi olaylarına ilişkin öngörüler sağlar. Bu makalede, Etkinlik Günlüğü olaylarını görüntülemek ve almak için farklı yöntemler hakkında ayrıntılı bilgi verilmektedir.

## <a name="azure-portal"></a>Azure portal
Azure portalındaki **Monitör** menüsünden tüm kaynaklar için Etkinlik Günlüğü'nü görüntüleyin. Söz konusu kaynağın menüsündeki **Etkinlik Günlüğü** seçeneğinden belirli bir kaynağın Etkinlik Günlüğü'nü görüntüleyin.

![Etkinlik Günlüğünü Görüntüle](./media/activity-logs-overview/view-activity-log.png)

Etkinlik Günlüğü olaylarını aşağıdaki alanlara göre filtreleyebilirsiniz:

* **Zaman sonu**: Olayların başlangıç ve bitiş saati.
* **Kategori**: [Etkinlik Günlüğündeki Kategorilerde](activity-log-view.md#categories-in-the-activity-log)açıklandığı gibi etkinlik kategorisi .
* **Abonelik**: Bir veya daha fazla Azure abonelik adı.
* **Kaynak grubu**: Seçili abonelikler içinde bir veya daha fazla kaynak grubu.
* **Kaynak (ad)**: - Belirli bir kaynağın adı.
* **Kaynak türü**: Kaynak türü, örneğin _Microsoft.Compute/virtualmachines._
* **İşlem adı** - _Microsoft.SQL/servers/Write_gibi bir Azure Kaynak Yöneticisi işleminin adı.
* **Önem derecesi**: Olayın önem düzeyi. Kullanılabilir değerler _Informational_, _Warning_, _Error_, _Critical_.
* **Başlatan Olay**: İşlemi gerçekleştiren kullanıcı.
* **Açık arama**: Tüm olaylardaki tüm alanlarda bu dizeyi arayan metin arama kutusunu açın.

## <a name="categories-in-the-activity-log"></a>Etkinlik günlüğündeki kategoriler
Etkinlik Günlüğü'ndeki her olayın, aşağıdaki tabloda açıklanan belirli bir kategorisi vardır. Bu kategorilerin şema hakkında tüm ayrıntılar için [Azure Etkinlik Günlüğü etkinlik şemasına](activity-log-schema.md)bakın. 

| Kategori | Açıklama |
|:---|:---|
| Yönetim | Kaynak Yöneticisi aracılığıyla gerçekleştirilen tüm oluşturma, güncelleştirme, silme ve eylem işlemlerinin kaydını içerir. İdari olaylara örnek olarak _sanal makine oluşturma_ ve ağ güvenlik grubunu _silme_verilebilir.<br><br>Kaynak Yöneticisi'ni kullanan bir kullanıcı veya uygulama tarafından gerçekleştirilen her eylem, belirli bir kaynak türünde bir işlem olarak modellenir. İşlem türü _Yazma,_ _Sil_veya _Eylem_ise, bu işlemin hem başlangıç hem de başarı veya başarısız kayıtları Yönetim kategorisinde kaydedilir. Yönetim olayları, abonelikteki rol tabanlı erişim denetiminde yapılan değişiklikleri de içerir. |
| Hizmet Durumu | Azure'da meydana gelen hizmet durumu olaylarının kaydını içerir. _Doğu ABD'de_bir Hizmet Durumu olay SQL Azure bir örnek kapalı kalma yaşıyor. <br><br>Hizmet Sağlık olayları altı çeşit gelir: _Eylem Gerekli_, _Yardımlı Kurtarma_, _Olay_, _Bakım_, _Bilgi_, veya _Güvenlik_. Bu olaylar yalnızca abonelikte olaydan etkilenecek bir kaynağınız varsa oluşturulur.
| Kaynak Durumu | Azure kaynaklarınızda meydana gelen kaynak durumu olaylarının kaydını içerir. Kaynak Durumu olayına örnek olarak _Sanal Makine sistem durumu kullanılamaz olarak değiştirilmiştir._<br><br>Kaynak Durumu olayları dört sağlık durumundan birini temsil edebilir: _Kullanılabilir_, _Kullanılamıyor,_ _Bozulmuş_ve _Bilinmeyen_. Ayrıca, Kaynak Durumu olayları _Platform Başlatılan_ veya _Kullanıcı Başlatılan_olarak kategorize edilebilir. |
| Uyarı | Azure uyarıları için etkinleştirme kaydını içerir. Bir Uyarı olayına örnek olarak _myVM'deki CPU % son 5 dakika boyunca 80'in üzerinde olmuştur._|
| Otomatik Ölçeklendirme | Aboneliğinizde tanımladığınız otomatik ölçek ayarlarını temel alan otomatik ölçeklendirme motorunun çalışmasıyla ilgili tüm olayların kaydını içerir. Otomatik Ölçekolayının bir örneği, _otomatik ölçeklendirme ölçeğinde başarısız olan eylemi_ölçeklendirmedir. |
| Öneri | Azure Danışmanı'ndan tavsiye etkinlikleri içerir. |
| Güvenlik | Azure Güvenlik Merkezi tarafından oluşturulan tüm uyarıların kaydını içerir. Bir Güvenlik olayı nın bir _örneği, yürütülen Şüpheli çift uzatma dosyasıdır._ |
| İlke | Azure İlkesi tarafından gerçekleştirilen tüm efekt eylem işlemlerinin kayıtlarını içerir. İlke olaylarına örnek olarak _Denetim_ ve _Reddet_verilebilir. İlke tarafından gerçekleştirilen her eylem, bir kaynak üzerinde bir işlem olarak modellenir. |

## <a name="view-change-history"></a>Değişiklik geçmişini görüntüleme

Etkinlik Günlüğü'nün gözden geçirilmesisırasında, bu olay sırasında ne gibi değişikliklerin gerçekleştiğini görmenize yardımcı olabilir. Bu bilgileri Değiştir **geçmişiyle**görüntüleyebilirsiniz. Daha derine bakmak istediğiniz Etkinlik Günlüğü'nden bir olay seçin. Bu olayla ilişkili değişiklikleri görüntülemek için **Geçmişi Değiştir (Önizleme)** sekmesini seçin.

![Bir olayın geçmiş listesini değiştirme](media/activity-logs-overview/change-history-event.png)

Olayla ilişkili değişiklikler varsa, seçebileceğiniz değişikliklerin listesini görürsünüz. Bu, **Geçmiş Değiştir (Önizleme)** sayfasını açar. Bu sayfada kaynak değişiklikleri bakın. Aşağıdaki örnekten de görebileceğiniz gibi, sadece VM'nin boyutlarını değiştirdiğini değil, önceki VM boyutunun değişiklikten önce ne olduğunu ve ne olarak değiştirildiğini de görebiliriz.

![Farklılıkları gösteren geçmiş sayfasını değiştirme](media/activity-logs-overview/change-history-event-details.png)

Değişiklik geçmişi hakkında daha fazla bilgi edinmek için [kaynak değişikliklerini al'a](../../governance/resource-graph/how-to/get-resource-changes.md)bakın.






## <a name="powershell"></a>PowerShell
PowerShell'den Etkinlik Günlüğü'ne ulaşmak için [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet'ini kullanın. Aşağıda bazı yaygın örnekler verilmiştir.

> [!NOTE]
> `Get-AzLog`sadece 15 günlük bir tarih sağlar. 15 gün sonraki son N olaylarını sorgulamak için **-MaxEvents** parametresini kullanın. 15 günden eski olaylara erişmek için REST API veya SDK'yı kullanın. **StartTime'ı**dahil etmiyorsanız, varsayılan değer **Bitiş Saati** eksi bir saattir. **EndTime'ı**içermiyorsanız, varsayılan değer geçerli saattir. Her zaman UTC bulunmaktadır.


Belirli bir tarih saatinden sonra oluşturulan günlük girişlerini alın:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Tarih aralığı arasında günlük girişleri alın:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir kaynak grubundan günlük girişleri alın:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Tarih aralığı arasında belirli bir kaynak sağlayıcıdan günlük girişleri alın:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir arayanla günlük girişleri alın:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Son 1000 etkinliği alın:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
CLI'den Etkinlik Günlüğü'ne almak için [az monitör etkinlik günlüğü](cli-samples.md#view-activity-log-for-a-subscription) kullanın. Aşağıda bazı yaygın örnekler verilmiştir.


Kullanılabilir tüm seçenekleri görüntüleyin.

```azurecli
az monitor activity-log list -h
```

Belirli bir kaynak grubundan günlük girişleri alın:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Belirli bir arayanla günlük girişleri alın:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Bir tarih aralığında, kaynak türünde arayanın günlüklerini alın:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>REST API
Etkinlik Günlüğünü bir REST istemcisinden almak için [Azure Monitor REST API'sını](https://docs.microsoft.com/rest/api/monitor/) kullanın. Aşağıda bazı yaygın örnekler verilmiştir.

Filtreile Etkinlik Günlükleri alın:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Filtreile Etkinlik Günlükleri alın ve seçin:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Select ile Etkinlik Günlükleri alın:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Filtre olmadan Etkinlik Günlükleri alın veya seçin:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>Sonraki adımlar

* [Platform günlüklerine genel bakış](platform-logs-overview.md)
* [Etkinlik günlüklerini diğer hedeflere göndermek için tanılama ayarı oluşturma](diagnostic-settings.md)
