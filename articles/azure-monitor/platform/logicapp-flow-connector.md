---
title: Azure Mantık Uygulamaları yla Azure Monitör Günlüklerini Kullanın ve Güç Otomatikleştir
description: Azure Monitor konektörünü kullanarak tekrarlanabilir işlemleri hızla otomatikleştirmek için Azure Logic Apps ve Power Otomatikleştirme'yi nasıl kullanabileceğinizi öğrenin.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/13/2020
ms.openlocfilehash: 6961b7bd94c9b3fe70365055851c488efa2cbeca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480020"
---
# <a name="azure-monitor-logs-connector-for-logic-apps-and-flow"></a>Mantıksal Uygulamalar ve Akış için Azure Monitör Günlükleri Konektörü
[Azure Mantık Uygulamaları](/azure/logic-apps/) ve [Güç Otomatikleştirme,](https://ms.flow.microsoft.com) çeşitli hizmetler için yüzlerce eylem kullanarak otomatik iş akışları oluşturmanıza olanak tanır. Azure Monitör Günlükleri konektörü, Azure Monitor'da Bir Log Analytics çalışma alanından veya Uygulama Öngörüleri uygulamasından veri alan iş akışları oluşturmanıza olanak tanır. Bu makalede, bağlayıcı ile birlikte eylemleri açıklar ve bu verileri kullanarak bir iş akışı oluşturmak için bir gözden geçirme sağlar.

Örneğin, Office 365'ten gelen bir e-posta bildiriminde Azure Monitor günlük verilerini kullanmak, Azure DevOps'lerde hata oluşturmak veya Bir Slack iletisi göndermek için bir mantık uygulaması oluşturabilirsiniz.  Bir iş akışını basit bir zamanlamayla veya bağlı bir hizmetteki bazı eylemlerden örneğin posta veya tweet alınması gibi bir işlemle tetikleyebilirsiniz. 

## <a name="actions"></a>Eylemler
Aşağıdaki tabloda Azure Monitör Günlükleri bağlayıcısı ile birlikte gelen eylemler açıklanmaktadır. Her ikisi de bir Log Analytics çalışma alanı veya Application Insights uygulamasına karşı bir günlük sorgusu çalıştırmanızı sağlar. Fark, verilerin döndürülme şeklidir.

> [!NOTE]
> Azure Monitör Günlükleri konektörü, [Azure Günlük Analizi bağlayıcısının](https://docs.microsoft.com/connectors/azureloganalytics/) ve [Azure Uygulama Öngörüleri bağlayıcısının](https://docs.microsoft.com/connectors/applicationinsights/)yerini alır. Bu bağlayıcı, diğerleriyle aynı işlevselliği sağlar ve bir Log Analytics çalışma alanına veya Application Insights uygulamasına karşı sorgu çalıştırmak için tercih edilen yöntemdir.


| Eylem | Açıklama |
|:---|:---|
| [Sorgu ve liste sonuçlarını çalıştırma](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-list-results) | Her satırı kendi nesnesi olarak döndürür. İş akışının geri kalanında her satırla ayrı ayrı çalışmak istediğinizde bu eylemi kullanın. Eylem genellikle her etkinlik [için](../../logic-apps/logic-apps-control-flow-loops.md#foreach-loop)bir tarafından takip edilir. |
| [Sorgu çalıştırma ve sonuçları görselleştir](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-visualize-results) | Tek biçimlendirilmiş nesne olarak ayarlanan sonuç tüm satırları döndürür. Sonuçları postayla göndermek gibi iş akışının geri kalanında birlikte ayarlanan sonucu kullanmak istediğinizde bu eylemi kullanın.  |

## <a name="walkthroughs"></a>İzlenecek Yollar
Aşağıdaki öğreticiler Azure Mantıksal Uygulamaları'nda Azure Monitor bağlayıcılarının kullanımını göstermektedir. Bu aynı örneği Power Automate ile gerçekleştirebilirsiniz, tek fark ilk iş akışını nasıl oluşturup tamamlandığında çalıştırabileceğinizdir. İş akışının ve eylemlerin yapılandırması her ikisi arasında aynıdır. Bkz. Başlamak için [Power Automate'deki şablondan akış oluştur.](https://docs.microsoft.com/power-automate/get-started-logic-template)


### <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma

Azure portalındaki **Mantık Uygulamaları'na** gidin ve **Ekle'yi**tıklatın. Yeni mantık uygulamasını depolamak ve ardından benzersiz bir ad vermek için **abonelik,** **kaynak grubu**ve **Bölge'yi** seçin. Azure Monitor günlüklerini ayarla'da açıklandığı gibi çalışma zamanı verileri ve olaylar hakkında bilgi toplamak [ve Azure Logic Apps için tanılama verileri toplamak için](../../logic-apps/monitor-logic-apps-log-analytics.md)Log **Analytics** ayarını açabilirsiniz. Bu ayar, Azure Monitör Günlükleri konektörünü kullanmak için gerekli değildir.

![Mantıksal uygulama oluşturma](media/logicapp-flow-connector/create-logic-app.png)


**Gözden Geçir + oluştur'u** tıklatın ve sonra **Oluştur'** seçeneğini tıklatın. Dağıtım tamamlandığında, **Logic Apps Tasarımcısı'nı**açmak **için kaynağa git'i** tıklatın.

### <a name="create-a-trigger-for-the-logic-app"></a>Mantık uygulaması için tetikleyici oluşturma
**Ortak bir tetikleyiciyle Başlat** **altında, Yineleme'yi**seçin. Bu, düzenli aralıklarla otomatik olarak çalışan bir mantık uygulaması oluşturur. Eylemin **Sıklık** kutusunda, **Gün'ü** seçin ve **Aralık** kutusuna iş akışını günde bir kez çalıştırmak için **1** girin.

![Yineleme eylemi](media/logicapp-flow-connector/recurrence-action.png)

## <a name="walkthrough-mail-visualized-results"></a>Walkthrough: Posta görselleştirilmiş sonuçlar
Aşağıdaki öğretici, Azure Monitor günlük sorgusunun sonuçlarını e-posta ile gönderen bir mantık uygulamasının nasıl oluşturulabileceğinizi gösterir. 

### <a name="add-azure-monitor-logs-action"></a>Azure Monitör Günlükleri eylemi ekleme
Yineleme eyleminden sonra çalışan bir eylem eklemek için **+ Yeni adımı** tıklatın. Bir **eylem seçin,** **azure monitör** yazın ve ardından Azure Monitör **Günlükleri'ni**seçin.

![Azure Monitör Günlükleri eylemi](media/logicapp-flow-connector/select-azure-monitor-connector.png)

Azure Günlük Analizi ' ni tıklatın **– Sorguyı çalıştırın ve sonuçları görselleştirin.**

![Sorgu çalıştırma ve sonuç eylemini görselleştir](media/logicapp-flow-connector/select-query-action-visualize.png)


### <a name="add-azure-monitor-logs-action"></a>Azure Monitör Günlükleri eylemi ekleme

Günlük Analizi çalışma alanınız için **Abonelik** ve **Kaynak Grubu'nu** seçin. **Kaynak Türü** için Günlük Analizi *Çalışma Alanı'nı* seçin ve ardından **Kaynak Adı**altında çalışma alanının adını seçin.

**Sorgu** penceresine aşağıdaki günlük sorgusunu ekleyin.  

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1day)
| summarize TotalErrors=count() by Computer
| sort by Computer asc   
```

**Zaman Aralığı** için *sorguda Ayarla'yı* ve Grafik Türü için **HTML Tablosu'nu** seçin. **Chart Type**
   
![Sorgu çalıştırma ve sonuç eylemini görselleştir](media/logicapp-flow-connector/run-query-visualize-action.png)

Posta, geçerli bağlantıyla ilişkili hesap tarafından gönderilecektir. **Bağlantıyı Değiştir'i**tıklatarak başka bir hesap belirtebilirsiniz.

### <a name="add-email-action"></a>E-posta eylemi ekleme

**+ Yeni adım'ı**tıklatın ve ardından + **Eylem Ekle'yi**tıklatın. Altında **bir eylem seçin,** **görünüm** yazın ve ardından **Office 365 Outlook'u**seçin.

![Outlook bağlayıcısı seçin](media/logicapp-flow-connector/select-outlook-connector.png)

**E-posta Gönder 'i (V2) seçin.**

![Office 365 Outlook seçim penceresi](media/logicapp-flow-connector/select-mail-action.png)

Dinamik **içerik** penceresi açmak için **Body** kutusunda herhangi bir yere tıklayın mantık uygulamasında önceki eylemlerden gelen değerlerle açılır. Log Analytics eyleminde sorgunun sonuçları olan **daha fazlasını** ve ardından **Gövde'yi** görün'ü seçin.

![Gövde seçin](media/logicapp-flow-connector/select-body.png)

**To** penceresinde bir alıcının e-posta adresini ve **Özne'deki**e-postanın konusunu belirtin. 

![Posta eylemi](media/logicapp-flow-connector/mail-action.png)


### <a name="save-and-test-your-logic-app"></a>Mantık uygulamanızı kaydedin ve test edin
Mantık uygulamasının test çalışmasını gerçekleştirmek için **Kaydet'i** ve ardından **Çalıştır'ı** tıklatın.

![Kaydet ve çalıştır](media/logicapp-flow-connector/save-run.png)


Mantık uygulaması tamamlandığında, belirttiğiniz alıcının postasını kontrol edin.  Aşağıdakilere benzer bir gövdeye sahip bir posta almış olmalısınız:

![Örnek e-posta](media/logicapp-flow-connector/sample-mail.png)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'da günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
- [Logic Apps](/azure/logic-apps/) hakkında daha fazla bilgi edinin
- [Microsoft Flow](https://ms.flow.microsoft.com)hakkında daha fazla bilgi edinin.

