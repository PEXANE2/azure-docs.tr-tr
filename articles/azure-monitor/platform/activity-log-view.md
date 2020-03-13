---
title: Azure Izleyici 'de Azure etkinlik günlüğü olaylarını görüntüleme
description: Azure Izleyici 'de Azure etkinlik günlüğünü görüntüleyin ve PowerShell, CLı ve REST API alın.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 9df7593a9fd191d3a734fba5e81fb1aecba08345
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79275054"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Azure etkinlik günlüğü olaylarını görüntüleme ve alma

[Azure etkinlik günlüğü](platform-logs-overview.md) , Azure 'da oluşan abonelik düzeyi olaylar hakkında öngörüler sağlar. Bu makalede, etkinlik günlüğü olaylarını görüntülemek ve almak için farklı yöntemlerle ilgili ayrıntılar sağlanmaktadır.

## <a name="azure-portal"></a>Azure portalı
Tüm kaynaklar için etkinlik günlüğünü Azure portal **izleme** menüsünden görüntüleyin. Söz konusu kaynak menüsündeki **etkinlik günlüğü** seçeneğinden belirli bir kaynağın etkinlik günlüğünü görüntüleyin.

![Etkinlik günlüğünü görüntüle](./media/activity-logs-overview/view-activity-log.png)

Etkinlik günlüğü olaylarını aşağıdaki alanlara göre filtreleyebilirsiniz:

* **TimeSpan**: olaylar için başlangıç ve bitiş zamanı.
* **Kategori**: [etkinlik günlüğündeki kategorilerde](activity-log-view.md#categories-in-the-activity-log)açıklandığı şekilde olay kategorisi.
* **Abonelik**: bir veya daha fazla Azure abonelik adı.
* **Kaynak grubu**: seçili abonelikler içindeki bir veya daha fazla kaynak grubu.
* **Kaynak (ad)** :-belirli bir kaynağın adı.
* **Kaynak türü**: kaynak türü, örneğin _Microsoft. COMPUTE/virtualmachines_.
* **İşlem adı** -Azure Resource Manager bir işlemin adı, örneğin _Microsoft. SQL/Servers/Write_.
* **Önem derecesi**: etkinliğin önem düzeyi. Kullanılabilir değerler _bilgilendirme_, _Uyarı_, _hata_, _kritik_.
* **Olay tarafından başlatılan olay**: işlemi gerçekleştiren kullanıcı.
* **Arama aç**: tüm olaylardaki tüm alanlarda bu dizeyi arayan metin arama kutusunu açın.

## <a name="categories-in-the-activity-log"></a>Etkinlik günlüğündeki Kategoriler
Etkinlik günlüğündeki her olayın, aşağıdaki tabloda açıklanan belirli bir kategorisi vardır. Bu kategorilerin şemaların serileştirilmesi hakkında tam Ayrıntılar için bkz. [Azure etkinlik günlüğü olay şeması](activity-log-schema.md). 

| Kategori | Açıklama |
|:---|:---|
| Yönetim | Kaynak Yöneticisi aracılığıyla gerçekleştirilen tüm oluşturma, güncelleştirme, silme ve eylem işlemlerinin kaydını içerir. Yönetim olayları örnekleri arasında _sanal makine oluşturma_ ve _ağ güvenlik grubu silme_sayılabilir.<br><br>Kaynak Yöneticisi kullanarak bir kullanıcı veya uygulama tarafından gerçekleştirilen her eylem, belirli bir kaynak türündeki işlem olarak modellenir. İşlem türü _yazma_, _silme_veya _eylem_ise, bu işlemin hem başlangıç hem de başarı veya başarısızlık kayıtları yönetim kategorisine kaydedilir. Yönetim olayları, bir abonelikte rol tabanlı erişim denetimine yapılan tüm değişiklikleri de içerir. |
| Hizmet Durumu | Azure 'da oluşan hizmet durumu olaylarının kaydını içerir. Doğu ABD bir hizmet sistem durumu olay SQL Azure bir örnek _kapalı kalma süresi yaşıyor_. <br><br>Hizmet durumu olayları altı değişen elikler halinde gelir: _eylem gereklidir_, _yardımlı kurtarma_, _olay_, _bakım_, _bilgi_veya _güvenlik_. Bu olaylar yalnızca abonelikte olaydan etkilenecek bir kaynağınız varsa oluşturulur.
| Kaynak Durumu | Azure kaynaklarınızda oluşan herhangi bir kaynak sistem durumu olayının kaydını içerir. Kaynak Durumu olayına bir örnek, _sanal makine sistem durumu kullanılamaz olarak değişir_.<br><br>Kaynak Durumu olaylar şu dört sistem durumunu temsil edebilir: _kullanılabilir_, _kullanılamaz_, _düşürülmüş_ve _bilinmiyor_. Ayrıca, Kaynak Durumu olaylar _platform tarafından başlatılmış_ veya _Kullanıcı tarafından başlatılmış_olarak kategorize edilebilir. |
| Uyarı | Azure uyarıları için etkinleştirme kaydını içerir. _Son 5 dakika boyunca, myVM 'deki% CPU 'su %80_üzerinde bir uyarı olayına bir örnektir.|
| Otomatik Ölçeklendirme | Aboneliğinizde tanımladığınız otomatik ölçeklendirme ayarlarına bağlı olarak, otomatik ölçeklendirme altyapısının işlemiyle ilgili olayların kaydını içerir. Otomatik ölçeklendirme olayına bir örnek, _Otomatik ölçeklendirme ölçeği artırma eylemi başarısız oldu_. |
| Öneri | Azure Advisor 'ın öneri olaylarını içerir. |
| Güvenlik | Azure Güvenlik Merkezi tarafından oluşturulan uyarıların kaydını içerir. Bir güvenlik olayına örnek olarak, _şüpheli çift uzantı dosyası yürütülür_. |
| İlke | Azure Ilkesi tarafından gerçekleştirilen tüm etki eylemi işlemlerinin kayıtlarını içerir. Ilke olayları örnekleri _Denetim_ ve _reddetme_içerir. Ilke tarafından gerçekleştirilen her eylem, bir kaynaktaki işlem olarak modellenir. |

## <a name="view-change-history"></a>Değişiklik geçmişini görüntüle

Etkinlik günlüğü 'Nü gözden geçirirken, bu olay saati sırasında hangi değişikliklerin meydana geldiğini görmek için yardımcı olabilir. **Değişiklik geçmişiyle**bu bilgileri görüntüleyebilirsiniz. Daha ayrıntılı olarak görmek istediğiniz etkinlik günlüğünden bir olay seçin. Söz konusu olayla ilişkili tüm değişiklikleri görüntülemek için **değişiklik geçmişi (Önizleme)** sekmesini seçin.

![Olay için değişiklik geçmişi listesi](media/activity-logs-overview/change-history-event.png)

Olayla ilişkili herhangi bir değişiklik varsa, seçebileceğiniz değişikliklerin bir listesini görürsünüz. Bu, **değişiklik geçmişi (Önizleme)** sayfasını açar. Bu sayfada, kaynakta yapılan değişiklikleri görürsünüz. Aşağıdaki örnekte görebileceğiniz gibi, yalnızca VM 'nin boyutlarının değiştiğini, ancak önceki VM boyutunun değişiklikten önce ne olduğunu ve ne değiştiğini görebileceksiniz.

![Farkları gösteren değişiklik geçmişi sayfası](media/activity-logs-overview/change-history-event-details.png)

Değişiklik geçmişi hakkında daha fazla bilgi edinmek için bkz. [kaynak değişikliklerini alma](../../governance/resource-graph/how-to/get-resource-changes.md).






## <a name="powershell"></a>PowerShell
PowerShell 'den etkinlik günlüğünü almak için [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet 'ini kullanın. Yaygın olarak kullanılan bazı örnekler aşağıda verilmiştir.

> [!NOTE]
> `Get-AzLog` yalnızca 15 gün geçmiş bir geçmişi sağlar. Son N olayı 15 günden daha fazla sorgulamak için **-maxevents** parametresini kullanın. 15 günden eski olaylara erişmek için REST API veya SDK 'Yı kullanın. **StartTime**'i eklemezseniz, varsayılan değer **bitişsaati** eksi bir saattir. **Bitişsaati**eklemezseniz, varsayılan değer geçerli süredir. Her zaman UTC 'de.


Belirli bir tarih zamanından sonra oluşturulan günlük girişlerini al:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Tarih Saat aralığı arasında günlük girişlerini al:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir kaynak grubundaki günlük girdilerini al:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Belirli bir kaynak sağlayıcısından tarih saat aralığı arasında günlük girişleri al:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir çağıran günlük girdilerini al:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Son 1000 olayını alın:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
CLı 'dan etkinlik günlüğünü almak için [az Monitor Activity-Log](cli-samples.md#view-activity-log-for-a-subscription) kullanın. Yaygın olarak kullanılan bazı örnekler aşağıda verilmiştir.


Tüm kullanılabilir seçenekleri görüntüleyin.

```azurecli
az monitor activity-log list -h
```

Belirli bir kaynak grubundaki günlük girdilerini al:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Belirli bir çağıran günlük girdilerini al:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Bir tarih aralığı içinde bir kaynak türü üzerinde arayana göre Günlükler alın:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>REST API
REST istemcisinden etkinlik günlüğünü almak için [Azure izleyici REST API](https://docs.microsoft.com/rest/api/monitor/) kullanın. Yaygın olarak kullanılan bazı örnekler aşağıda verilmiştir.

Filtre ile etkinlik günlüklerini al:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Filtreli etkinlik günlüklerini alın ve şunları seçin:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Select ile etkinlik günlüklerini al:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Filtre olmadan etkinlik günlüklerini Al veya Seç:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="activity-logs-analytics-monitoring-solution"></a>Etkinlik günlükleri analitik izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki etkinlik günlüğü kayıtlarını çözümlemek için birden çok günlük sorgusu ve görünümü içerir.

### <a name="prerequisites"></a>Önkoşullar
Aboneliğinizin etkinlik günlüğünü bir Log Analytics çalışma alanına göndermek için bir tanılama ayarı oluşturmanız gerekir. Bkz. Azure [izleyici 'de Log Analytics çalışma alanında Azure platformu günlüklerini toplama](resource-logs-collect-workspace.md).

### <a name="install-the-solution"></a>Çözümü yükler
**Etkinlik günlüğü Analizi** çözümünü yüklemek için [bir izleme çözümü yüklerken](../insights/solutions.md#install-a-monitoring-solution) yordamı kullanın. Ek yapılandırma gerekmez.

### <a name="use-the-solution"></a>Çözüm kullanın
Abonelik için [etkinlik günlüğü Analizi izleme çözümünü](activity-log-collect.md) açmak üzere **etkinlik günlüğü** sayfasının en üstündeki **Günlükler** ' e tıklayın. Veya Azure portal abonelik **İzleyicisi** menüsündeki tüm izleme çözümlerine erişin. **Genel bakış** sayfasını çözüm kutucukları ile açmak için **Öngörüler** bölümünde **daha fazla** ' yı seçin. **Azure etkinlik günlükleri** kutucuğu, çalışma alanınızdaki **AzureActivity** kayıt sayısının sayısını görüntüler.

![Azure etkinlik günlüklerini kutucuğu](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **etkinlik günlükleri görünümünü açmak** için Azure etkinlik **günlükleri** kutucuğuna tıklayın. Görünüm, aşağıdaki tablodaki görselleştirme parçalarını içerir. Her bölümde, belirtilen zaman aralığı için parçaların ölçütlerine uyan 10 ' a kadar öğe listelenir. Bölümün en altında bulunan **Tümünü görüntüle ' ye** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure etkinlik günlüklerini Panosu](media/collect-activity-logs/activity-log-dash.png)

| Görselleştirme bölümü | Açıklama |
| --- | --- |
| Azure etkinlik günlüğü girdileri | Seçtiğiniz tarih aralığı için en üstteki Azure etkinlik günlüğü girişi kayıt toplamlarını gösteren bir çubuk grafiği gösterir ve ilk 10 etkinlik çağıranlarının bir listesini gösterir. `AzureActivity`için bir günlük araması çalıştırmak için çubuk grafiğine tıklayın. Bu öğe için tüm etkinlik günlüğü girdilerini döndüren bir günlük araması çalıştırmak için bir arayan öğesine tıklayın. |
| Duruma göre etkinlik günlükleri | Seçili tarih aralığı için Azure etkinlik günlüğü durumu ve ilk on durum kayıtlarının listesi için bir halka grafiği gösterir. `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`günlük sorgusu çalıştırmak için grafiğe tıklayın. Bu durum kaydı için tüm etkinlik günlüğü girdilerini döndüren bir günlük araması çalıştırmak için bir durum öğesine tıklayın. |
| Kaynağa göre etkinlik günlükleri | Etkinlik günlüklerinin bulunduğu toplam kaynak sayısını gösterir ve her bir kaynak için kayıt sayısı olan ilk on kaynağı listeler. Çözüm için kullanılabilen tüm Azure kaynaklarını gösteren `AzureActivity | summarize AggregatedValue = count() by Resource`için bir günlük araması çalıştırmak için Toplam alanına tıklayın. Kaynak için tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için kaynağa tıklayın. |
| Kaynak sağlayıcısı tarafından etkinlik günlükleri | Etkinlik günlükleri üreten ve ilk on olan kaynak sağlayıcılarının toplam sayısını gösterir. Tüm Azure Kaynak sağlayıcılarını gösteren `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`bir günlük sorgusu çalıştırmak için Toplam alanına tıklayın. Sağlayıcıya yönelik tüm etkinlik kayıtlarını döndüren bir günlük sorgusu çalıştırmak için bir kaynak sağlayıcısına tıklayın. |




## <a name="next-steps"></a>Sonraki adımlar

* [Platform günlüklerine Genel Bakış bölümünü okuyun](platform-logs-overview.md)
* [Diğer hedeflere etkinlik günlükleri göndermek için tanılama ayarı oluştur](diagnostic-settings.md)
