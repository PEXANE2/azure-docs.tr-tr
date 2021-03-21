---
title: Logic App kullanarak Log Analytics çalışma alanındaki verileri Azure depolama 'ya arşivleme
description: Bir Log Analytics çalışma alanındaki verileri sorgulamak ve Azure Storage 'a göndermek için Azure Logic Apps kullanma yöntemi açıklanmaktadır.
ms.service: azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/02/2020
ms.openlocfilehash: fd66aa1f10a32d94d515a1f0aa25c25331796a8d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102035696"
---
# <a name="archive-data-from-log-analytics-workspace-to-azure-storage-using-logic-app"></a>Logic App kullanarak Log Analytics çalışma alanındaki verileri Azure depolama 'ya arşivleme
Bu makalede, Azure Izleyici 'deki bir Log Analytics çalışma alanındaki verileri sorgulamak ve Azure Storage 'a göndermek için [Azure Logic Apps](../../logic-apps/index.yml) kullanma yöntemi açıklanır. Denetim ve uyumluluk senaryolarında Azure Izleyici günlük verilerinizi dışarı aktarmanız veya başka bir hizmetin bu verileri almasına izin vermek istediğinizde bu işlemi kullanın.  

## <a name="other-export-methods"></a>Diğer dışarı aktarma yöntemleri
Bu makalede açıklanan yöntem, bir mantıksal uygulama kullanılarak günlük sorgusundan zamanlanan bir dışarı aktarma işlemini açıklar. Belirli senaryolara yönelik verileri dışarı aktarmaya yönelik diğer seçenekler şunlardır:

- Log Analytics çalışma alanınızdan tüm verileri Azure depolama hesabına veya Olay Hub 'ına aktarmak için Azure Izleyici günlüklerinin Log Analytics çalışma alanı verileri dışarı aktarma özelliğini kullanın. Bkz. [Log Analytics çalışma alanı verilerini dışarı aktarma Azure izleyici (Önizleme)](logs-data-export.md)
- Bir mantıksal uygulama kullanarak bir kerelik dışarı aktarma. [Logic Apps ve güç otomatikleştirme için bkz. Azure Izleyici günlükleri Bağlayıcısı](logicapp-flow-connector.md).
- PowerShell betiği kullanılarak yerel makineye bir kerelik dışarı aktarma. Bkz. [Invoke-Azoperationalınsightsqueryexport]] ( https://www.powershellgallery.com/packages/Invoke-AzOperationalInsightsQueryExport) .

## <a name="overview"></a>Genel Bakış
Bu yordam, bir mantıksal uygulamadan bir günlük sorgusu çalıştırmanızı ve çıkışını iş akışındaki diğer eylemlerde kullanmanızı sağlayan [Azure Izleyici günlükleri bağlayıcısını](/connectors/azuremonitorlogs/) kullanır. [Azure Blob depolama Bağlayıcısı](/connectors/azureblob/) , bu yordamda sorgu çıktısını Azure depolama 'ya göndermek için kullanılır. Diğer eylemler aşağıdaki bölümlerde açıklanmıştır.

![Mantıksal uygulamaya genel bakış](media/logs-export-logic-app/logic-app-overview.png)

Verileri bir Log Analytics çalışma alanından dışa aktardığınızda, günlük verilerinizi filtreleyip toplamalı ve mantıksal uygulama iş akışınız tarafından işlenen veri miktarını gerekli verilere göre sınırlamak için sorgunuzu en uygun hale getirin. Örneğin, oturum açma olaylarını arşivlemeniz gerekiyorsa, gerekli olayları ve yalnızca gerekli alanları aşağıdaki sorguyla filtreleyebilirsiniz. 

```json
SecurityEvent
| where EventID == 4624 or EventID == 4625
| project TimeGenerated , Account , AccountType , Computer
```

Verileri bir zamanlamaya göre dışarı aktardığınızda, geç gelen verileri kaçırmadığınızdan emin olmak için Sorgunuzdaki ingestion_time () işlevini kullanın. Ağ veya platform sorunları nedeniyle veriler gecikirse, alma süresinin kullanılması, bir sonraki mantıksal uygulama yürütmeye dahil edilmesini sağlar. Bir örnek için bkz. [Azure Izleyici günlükleri ekleme eylemi](#add-azure-monitor-logs-action) .

## <a name="prerequisites"></a>Önkoşullar
Bu yordam tamamlanmadan önce tamamlanması gereken önkoşullar aşağıda verilmiştir.

- Log Analytics çalışma alanı. Mantıksal uygulamayı oluşturan kullanıcının çalışma alanına en azından okuma izni olması gerekir. 
- Azure depolama hesabı. Depolama hesabının Log Analytics çalışma alanıyla aynı abonelikte olması gerekmez. Mantıksal uygulamayı oluşturan kullanıcının depolama hesabına yazma izni olması gerekir. 


## <a name="connector-limits"></a>Bağlayıcı limitleri
Azure Izleyici 'de Log Analytics çalışma alanı ve günlük sorguları, müşterileri koruyan ve yalıtmak ve hizmet kalitesini sürdürmek için sınırlar içeren çok kiracılı hizmetlerdir. Büyük miktarda veri sorgulanırken, mantıksal uygulama tekrarını ve günlük sorgunuzu nasıl yapılandıracağınızı etkileyebilecek aşağıdaki limitleri göz önünde bulundurmanız gerekir:

- Günlük sorguları 500.000 satırdan fazla satır döndüremez.
- Günlük sorguları 64.000.000 bayttan fazlasını döndüremez.
- Günlük sorguları varsayılan olarak 10 dakikadan uzun bir süre çalışabilir. 
- Log Analytics Bağlayıcısı, dakikada 100 çağrısıyla sınırlıdır.


## <a name="create-container-in-the-storage-account"></a>Depolama hesabında kapsayıcı oluşturma
İçe aktarılmış verileri tutmak üzere depolama hesabınıza bir kapsayıcı eklemek için [kapsayıcı oluşturma](../../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container) bölümündeki yordamı kullanın. Bu makaledeki kapsayıcı için kullanılan ad **loganalytics-Data**, ancak herhangi bir ad kullanabilirsiniz.


## <a name="create-logic-app"></a>Mantıksal Uygulama oluşturma

Azure portal **Logic Apps** gidin ve **Ekle**' ye tıklayın. Yeni mantıksal uygulamayı depolamak için bir **abonelik**, **kaynak grubu** ve **bölge** seçin ve benzersiz bir ad verin. [Azure izleyici günlüklerini ayarlama ve Azure Logic Apps için tanılama verilerini toplama](../../logic-apps/monitor-logic-apps-log-analytics.md)bölümünde açıklandığı gibi çalışma zamanı verileri ve olayları hakkında bilgi toplamak için **Log Analytics** ayarını açabilirsiniz. Bu ayar, Azure Izleyici günlükleri bağlayıcısının kullanılması için gerekli değildir.

![Mantıksal uygulama oluşturma](media/logs-export-logic-app/create-logic-app.png)


**Gözden geçir + oluştur** ve sonra **Oluştur**' a tıklayın. Dağıtım tamamlandığında, **Logic Apps tasarımcısını** açmak Için **Kaynağa Git** ' e tıklayın.

## <a name="create-a-trigger-for-the-logic-app"></a>Mantıksal uygulama için bir tetikleyici oluşturma
**Ortak bir tetikleyiciden başla**' nın altında **yinelenme**' yi seçin. Bu, düzenli aralıklarla otomatik olarak çalışan bir mantıksal uygulama oluşturur. İşlemin **Sıklık** kutusunda, **gün** ' yı seçin, **zaman aralığı** kutusuna **1** girerek iş akışını günde bir kez çalıştırın.

![Yinelenme eylemi](media/logs-export-logic-app/recurrence-action.png)


### <a name="add-azure-monitor-logs-action"></a>Azure Izleyici günlükleri ekleme eylemi
Yineleme eyleminden sonra çalışacak bir eylem eklemek için **+ yeni adım** ' a tıklayın. **Eylem seçin** altında **Azure izleyici** yazın ve ardından **Azure izleyici günlükleri**' ni seçin.

![Azure Izleme günlükleri eylemi](media/logs-export-logic-app/select-azure-monitor-connector.png)

Azure Log Analytics ' ye tıklayın **: sorgu Çalıştır ve sonuçları listele**.

![Mantıksal uygulama tasarımcısında bir adıma eklenen yeni bir eylemin ekran görüntüsü. Azure Izleyici günlükleri bir eylem Seç altında vurgulanır.](media/logs-export-logic-app/select-query-action-list.png)

Bir kiracı seçmeniz ve iş akışının sorguyu çalıştırmak için kullanacağı hesapla Log Analytics çalışma alanına erişim vermeniz istenir.


## <a name="add-azure-monitor-logs-action"></a>Azure Izleyici günlükleri ekleme eylemi
Azure Izleyici günlükleri eylemi, çalıştırılacak sorguyu belirtmenizi sağlar. Bu örnekte kullanılan günlük sorgusu saatlik yinelenme için iyileştirilmiştir ve belirli yürütme süresi boyunca alınan verileri toplar. Örneğin, iş akışı 4:35 ' de çalışıyorsa, zaman aralığı 4:00 ile 5:00. Mantıksal uygulamayı farklı bir sıklıkta çalışacak şekilde değiştirirseniz, sorguyu da değiştirmeniz gerekir. Örneğin, yinelemeyi günlük olarak çalışacak şekilde ayarlarsanız, sorgudaki startTime öğesini startofday (make_datetime (year, month, Day, 0, 0)) olarak ayarlamanız gerekir. 

Log Analytics çalışma alanınız için **aboneliği** ve **kaynak grubunu** seçin. **Kaynak türü** Için *Log Analytics çalışma alanı* ' nı seçin ve ardından **kaynak adı** altında çalışma alanının adını seçin.

**Sorgu** penceresine aşağıdaki günlük sorgusunu ekleyin.  

```Kusto
let dt = now();
let year = datetime_part('year', dt);
let month = datetime_part('month', dt);
let day = datetime_part('day', dt);
let hour = datetime_part('hour', dt);
let startTime = make_datetime(year,month,day,hour,0)-1h;
let endTime = startTime + 1h - 1tick;
AzureActivity
| where ingestion_time() between(startTime .. endTime)
| project 
    TimeGenerated,
    BlobTime = startTime, 
    OperationName ,
    OperationNameValue ,
    Level ,
    ActivityStatus ,
    ResourceGroup ,
    SubscriptionId ,
    Category ,
    EventSubmissionTimestamp ,
    ClientIpAddress = parse_json(HTTPRequest).clientIpAddress ,
    ResourceId = _ResourceId 
```

**Zaman aralığı** , **TimeGenerated** sütununa göre sorguya dahil edilecek kayıtları belirtir. Bu, sorguda seçilen zaman aralığına eşit veya daha yüksek bir değere ayarlanmalıdır. Bu sorgu **TimeGenerated** sütununu kullanmıyor, sonra **sorgu seçeneğinde ayarla** seçeneği kullanılamaz. Zaman aralığı hakkında daha fazla ayrıntı için bkz. [sorgu kapsamı](./scope.md) . 

**Zaman aralığı** için **son 4 saati** seçin. Bu, zaman **üretilmeden** daha büyük bir alım süresi içeren tüm kayıtların sonuçlara dahil edilmesini sağlayacaktır.
   
![Sorgu Çalıştır ve sonuçları görselleştirin adlı yeni Azure Izleyici günlükleri eyleminin ayarlarının ekran görüntüsü.](media/logs-export-logic-app/run-query-list-action.png)


### <a name="add-parse-json-activity-optional"></a>Ayrıştırma JSON etkinliği Ekle (isteğe bağlı)
**Sorgu ve liste sonuçlarını Çalıştır** EYLEMININ çıktısı JSON olarak biçimlendirilir. Bu verileri ayrıştırarak **oluşturma** hazırlığı 'nin bir parçası olarak değiştirebilirsiniz. 

Almayı düşündüğünüz yükü açıklayan bir JSON şeması sağlayabilirsiniz. Tasarımcı, bu şemayı kullanarak JSON içeriğini ayrıştırır ve JSON içeriğinizin özelliklerini temsil eden Kullanıcı dostu belirteçler oluşturur. Daha sonra mantıksal uygulamanızın iş akışının tamamında bu özellikleri kolayca başvurabilir ve kullanabilirsiniz. 


**+ Yeni adım**' a ve ardından **+ Eylem Ekle**' ye tıklayın. **Eylem seçin** altında **JSON** yazın ve ardından **JSON 'u Ayrıştır**' ı seçin.

![JSON ayrıştırma etkinliğini seçin](media/logs-export-logic-app/select-parse-json.png)

Önceki etkinliklerdeki değerlerin listesini göstermek için **içerik** kutusuna tıklayın. **Sorgu Çalıştır ve sonuçları listele** eyleminden **gövde** ' yi seçin. Bu, günlük sorgusunun çıktıdır.

[![Gövde Seç](media/logs-export-logic-app/select-body.png)](media/logs-export-logic-app/select-body.png#lightbox)

5.  **Şema oluşturmak için örnek yük kullan**' a tıklayın. Günlük sorgusunu çalıştırın ve örnek yük için kullanılacak çıktıyı kopyalayın.  Buradaki örnek sorgu için aşağıdaki çıktıyı kullanabilirsiniz:


```json
{
    "TimeGenerated": "2020-09-29T23:11:02.578Z",
    "BlobTime": "2020-09-29T23:00:00Z",
    "OperationName": "Returns Storage Account SAS Token",
    "OperationNameValue": "MICROSOFT.RESOURCES/DEPLOYMENTS/WRITE",
    "Level": "Informational",
    "ActivityStatus": "Started",
    "ResourceGroup": "monitoring",
    "SubscriptionId": "00000000-0000-0000-0000-000000000000",
    "Category": "Administrative",
    "EventSubmissionTimestamp": "2020-09-29T23:11:02Z",
    "ClientIpAddress": "192.168.1.100",
    "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/monitoring/providers/microsoft.storage/storageaccounts/my-storage-account"
}
```



![JSON yükünü Ayrıştır](media/logs-export-logic-app/parse-json-payload.png)

## <a name="add-the-compose-action"></a>Oluşturma eylemini ekleme
**Oluşturma** eylemi, ayrıştırılmış JSON çıkışını alır ve bloba depolamanız gereken nesneyi oluşturur.

**+ Yeni adım**' a ve ardından **+ Eylem Ekle**' ye tıklayın. **Eylem seçin** altında **Oluştur** yazın ve ardından **oluşturma** eylemini seçin.

![Oluşturma eylemini seçin](media/logs-export-logic-app/select-compose.png)


Önceki etkinliklerdeki değerlerin listesini görüntüleyen **girişler** kutusuna tıklayın. **JSON Ayrıştır** eyleminden **gövde** ' yi seçin. Bu, günlük sorgusunun ayrıştırılmış çıktıdır.

[![Oluşturma eylemi için gövde seçin](media/logs-export-logic-app/select-body-compose.png)](media/logs-export-logic-app/select-body-compose.png#lightbox)


## <a name="add-the-create-blob-action"></a>Blob oluştur eylemini ekleyin
Blob oluştur eylemi, oluşturulan JSON 'ı depolamaya yazar.

**+ Yeni adım**' a ve ardından **+ Eylem Ekle**' ye tıklayın. **Eylem seçin** altında **BLOB** yazın ve ardından **BLOB oluştur** eylemini seçin.

![Blob oluştur ' u seçin](media/logs-export-logic-app/select-create-blob.png)

**Bağlantı adı** alanına depolama hesabınıza bağlantı için bir ad yazın ve ardından Depolama hesabınızdaki kapsayıcıyı seçmek için **klasör yolu** kutusunda klasör simgesine tıklayın. Önceki etkinliklerdeki değerlerin listesini görmek için **BLOB adına** tıklayın. **İfade** ' e tıklayın ve zaman aralığından eşleşen bir ifade girin. Saatlik olarak çalıştırılan bu sorgu için aşağıdaki ifade, bir önceki saat başına blob adını ayarlar: 

```json
subtractFromTime(formatDateTime(utcNow(),'yyyy-MM-ddTHH:00:00'), 1,'Hour')
```

[![Blob ifadesi](media/logs-export-logic-app/blob-expression.png)](media/logs-export-logic-app/blob-expression.png#lightbox)

Önceki etkinliklerdeki değerlerin listesini göstermek için **BLOB içeriği** kutusuna tıklayın ve ardından **Oluştur** bölümünde **çıktılar** ' i seçin.


![Blob ifadesi oluştur](media/logs-export-logic-app/create-blob.png)


## <a name="test-the-logic-app"></a>Mantıksal uygulamayı test etme
**Çalıştır**' a tıklayarak iş akışını test edin. İş akışında hata varsa, sorun ile ilgili adımda belirtilir. Sorunları araştırmak üzere giriş ve çıktıyı görüntülemek için her adımın yürütme ve detaylarını görüntüleyebilirsiniz. Gerekirse bkz. [Azure Logic Apps iş akışı hatalarının sorunlarını giderme ve tanılama](../../logic-apps/logic-apps-diagnosing-failures.md) .

[![Çalışma geçmişi](media/logs-export-logic-app/runs-history.png)](media/logs-export-logic-app/runs-history.png#lightbox)


## <a name="view-logs-in-storage"></a>Depolama alanındaki günlükleri görüntüleme
Azure portal **depolama hesapları** menüsüne gidin ve depolama hesabınızı seçin. **Bloblar** kutucuğuna tıklayın ve BLOB oluştur eyleminde belirttiğiniz kapsayıcıyı seçin. Blobların birini seçin ve ardından **blobu düzenleyin**.

[![Blob verileri](media/logs-export-logic-app/blob-data.png)](media/logs-export-logic-app/blob-data.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici 'de günlük sorguları](./log-query-overview.md)hakkında daha fazla bilgi edinin.
- [Logic Apps](../../logic-apps/index.yml) hakkında daha fazla bilgi edinin
- [Güç otomatikleştirme](https://flow.microsoft.com)hakkında daha fazla bilgi edinin.