---
title: Azure Etkinliği günlük olaylarını Azure Monitörü'nde görüntüleme
description: Azure Etkinliği günlüğünü Azure Monitörü'nde görüntüleyin ve PowerShell, CLI ve REST API ile alın.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 9df7593a9fd191d3a734fba5e81fb1aecba08345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275054"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Azure Etkinliği günlük olaylarını görüntüleme ve alma

[Azure Etkinlik Günlüğü,](platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyi olaylarına ilişkin öngörüler sağlar. Bu makalede, Etkinlik Günlüğü olaylarını görüntülemek ve almak için farklı yöntemler hakkında ayrıntılı bilgi verilmektedir.

## <a name="azure-portal"></a>Azure portalında
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


## <a name="activity-logs-analytics-monitoring-solution"></a>Aktivite Günlükleri Analiz izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki Etkinlik Günlüğü kayıtlarını analiz etmek için birden çok günlük sorgusu ve görünüm içerir.

### <a name="prerequisites"></a>Ön koşullar
Aboneliğiniz için Etkinlik günlüğünü bir Log Analytics çalışma alanına göndermek için bir tanılama ayarı oluşturmanız gerekir. Azure [Monitor'da Günlük Analizi çalışma alanında Azure platform günlüklerini topla'ya](resource-logs-collect-workspace.md)bakın.

### <a name="install-the-solution"></a>Çözümü yükleyin
**Etkinlik Günlüğü Analizi** çözümünü yüklemek için bir izleme çözümü [yükleyin](../insights/solutions.md#install-a-monitoring-solution) yordamını kullanın. Ek yapılandırma gerekmez.

### <a name="use-the-solution"></a>Çözümü kullanın
Abonelik için [Etkinlik Günlüğü Analizi izleme çözümünü](activity-log-collect.md) açmak için Etkinlik **Günlüğü** sayfasının üst kısmındaki **Günlükler'i** tıklatın. Veya Azure portalındaki abonelik **Monitörü** menüsündeki tüm izleme çözümlerine erişin. Çözüm kutucuklarıyla **Genel Bakış** sayfasını açmak için **Öngörüler** bölümünde **Daha Fazla'yı** seçin. **Azure Etkinlik Günlükleri döşemesi,** çalışma alanınızdaki **Azure Etkinliği** kayıtlarının sayısını görüntüler.

![Azure Etkinlik Günlükleri döşeme](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **Etkinlik Günlükleri** görünümünü açmak için Azure Etkinlik **Günlükleri** döşemesini tıklatın. Görünüm aşağıdaki tabloda görselleştirme bölümlerini içerir. Her parça, belirtilen zaman aralığı için bu parçaların ölçütleriyle eşleşen en fazla 10 öğeyi listeler. Bölümün altındaki **tümlerini gör'e** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure Etkinlik Günlükleri panosu](media/collect-activity-logs/activity-log-dash.png)

| Görselleştirme bölümü | Açıklama |
| --- | --- |
| Azure Etkinlik Günlüğü Girişleri | Seçtiğiniz tarih aralığıiçin en üst Azure Etkinliği Günlüğü giriş toplamlarının çubuk grafiğini gösterir ve en iyi 10 etkinlik arayanların listesini gösterir. Bir günlük aramasını çalıştırmak için `AzureActivity`çubuk grafiği tıklatın. Bu öğeiçin tüm Etkinlik Günlüğü girişlerini döndüren bir günlük araması çalıştırmak için bir arayan öğesini tıklatın. |
| Duruma Göre Etkinlik Günlükleri | Seçili tarih aralığı için Azure Etkinlik Günlüğü durumu için bir halka grafiği ve en iyi on durum kaydının listesini gösterir. Bir günlük sorgusunu çalıştırmak `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`için grafiği tıklatın. Bu durum kaydı için tüm Etkinlik Günlüğü girişlerini döndüren bir günlük araması çalıştırmak için bir durum öğesini tıklatın. |
| Kaynağa Göre Etkinlik Günlükleri | Etkinlik Günlükleri ile toplam kaynak sayısını gösterir ve her kaynak için kayıt sayıları ile ilk on kaynağı listeler. Çözümde kullanılabilen tüm Azure `AzureActivity | summarize AggregatedValue = count() by Resource`kaynaklarını gösteren bir günlük araması çalıştırmak için toplam alanı tıklatın. Bu kaynak için tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak tıklatın. |
| Kaynak Sağlayıcıya Göre Etkinlik Günlükleri | Etkinlik Günlükleri üreten toplam kaynak sağlayıcısı sayısını gösterir ve ilk on'u listeler. Tüm Azure kaynak sağlayıcılarını gösteren `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`bir günlük sorgusu çalıştırmak için toplam alanı tıklatın. Sağlayıcının tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak sağlayıcısını tıklatın. |




## <a name="next-steps"></a>Sonraki adımlar

* [Platform günlüklerine genel bakış](platform-logs-overview.md)
* [Etkinlik günlüklerini diğer hedeflere göndermek için tanılama ayarı oluşturma](diagnostic-settings.md)
