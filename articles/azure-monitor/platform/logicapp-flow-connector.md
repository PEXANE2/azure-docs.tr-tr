---
title: Azure Logic Apps ve güç otomatikleştirme ile Azure Izleyici günlüklerini kullanma
description: Azure Izleyici bağlayıcısını kullanarak tekrarlanabilir işlemleri hızlı bir şekilde otomatikleştirmek için Azure Logic Apps ve güç otomatikleştirmesini nasıl kullanabileceğinizi öğrenin.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/13/2020
ms.openlocfilehash: 193aa168cff436512dc2044d0986df508fd6bfa9
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248745"
---
# <a name="azure-monitor-logs-connector-for-logic-apps-and-flow"></a>Logic Apps ve Flow için Azure Izleyici günlükleri Bağlayıcısı
[Azure Logic Apps](/azure/logic-apps/) ve [Güç otomatikleştirme](https://ms.flow.microsoft.com) , çeşitli hizmetler için yüzlerce eylemi kullanarak otomatik iş akışları oluşturmanıza olanak tanır. Azure Izleyici günlükleri Bağlayıcısı, Azure Izleyici 'deki bir Log Analytics çalışma alanından veya bir Application Insights uygulamasından veri alan iş akışları oluşturmanıza olanak tanır. Bu makalede, bağlayıcıya dahil edilen eylemler açıklanmakta ve bu verileri kullanarak iş akışı oluşturmaya yönelik bir yol sunulmaktadır.

Örneğin, Azure Izleyici günlük verilerini Office 365 ' den bir e-posta bildiriminde kullanmak için bir mantıksal uygulama oluşturabilir, Azure DevOps 'da bir hata oluşturabilir veya bir bolluk iletisi postalayabilirsiniz.  Bir iş akışını basit bir zamanlamaya göre veya bir e-posta ya da tweet alınması gibi bağlı bir hizmette bazı bir eylemden tetikleyebilirsiniz. 

## <a name="connector-limits"></a>Bağlayıcı limitleri
Azure Izleyici günlükleri Bağlayıcısı şu sınırlara sahiptir:
* En fazla veri boyutu: 16 MB
* En fazla sorgu yanıt boyutu 100 MB
* En fazla kayıt sayısı: 500.000
* En fazla sorgu zaman aşımı 110 saniye.

Verilerinizin boyutuna ve kullandığınız sorguya bağlı olarak bağlayıcı, sınırlarına ve başarısız olabilir. Tetikleyici tekrarlarını daha sık çalıştırmak ve daha az veri sorgulamak için ayarlama yaparken bu gibi durumlarda çözüm yapabilirsiniz. Daha az kayıt ve sütun döndürmek için verilerinizi toplayan sorguları kullanabilirsiniz.

## <a name="actions"></a>Eylemler
Aşağıdaki tabloda, Azure Izleyici günlükleri bağlayıcısına dahil edilen eylemler açıklanmaktadır. Her ikisi de Log Analytics çalışma alanında veya Application Insights uygulamasında bir günlük sorgusu çalıştırmanızı sağlar. Fark, verilerin döndürülme yoludur.

> [!NOTE]
> Azure Izleyici günlükleri Bağlayıcısı, [azure Log Analytics bağlayıcısının](https://docs.microsoft.com/connectors/azureloganalytics/) ve [Azure Application Insights bağlayıcısının](https://docs.microsoft.com/connectors/applicationinsights/)yerini alır. Bu bağlayıcı diğerleri ile aynı işlevleri sağlar ve bir Log Analytics çalışma alanında veya Application Insights uygulamasında bir sorgu çalıştırmak için tercih edilen yöntemdir.


| Eylem | Açıklama |
|:---|:---|
| [Sorgu ve liste sonuçlarını Çalıştır](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-list-results) | Her bir satırı kendi nesnesi olarak döndürür. Bu eylemi, iş akışının geri kalanında her bir satırla ayrı olarak çalışmak istediğinizde kullanın. Eylem genellikle [her etkinlik için](../../logic-apps/logic-apps-control-flow-loops.md#foreach-loop)bir olur. |
| [Sorgu Çalıştır ve sonuçları görselleştirin](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-visualize-results) | Sonuç kümesindeki tüm satırları tek biçimli bir nesne olarak döndürür. Sonuç kümesini, sonuçları bir e-postaya göndermek gibi iş akışı geri kalanında birlikte kullanmak istediğinizde bu eylemi kullanın.  |

## <a name="walkthroughs"></a>İzlenecek Yollar
Aşağıdaki öğreticilerde Azure Logic Apps içindeki Azure Izleyici bağlayıcılarının kullanımı gösterilmektedir. Bu aynı örneği, Power otomatikleştirmede gerçekleştirebilirsiniz, ancak ilk iş akışını oluşturma ve tamamlandığında çalıştırma gibi tek fark. İş akışı ve eylemlerin yapılandırması her ikisi arasında aynıdır. Başlamak için bkz. [Power otomatikleştirmede bir şablondan akış oluşturma](https://docs.microsoft.com/power-automate/get-started-logic-template) .


### <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma

Azure portal **Logic Apps** gidin ve **Ekle**' ye tıklayın. Yeni mantıksal uygulamayı depolamak için bir **abonelik**, **kaynak grubu**ve **bölge** seçin ve benzersiz bir ad verin. [Azure izleyici günlüklerini ayarlama ve Azure Logic Apps için tanılama verilerini toplama](../../logic-apps/monitor-logic-apps-log-analytics.md)bölümünde açıklandığı gibi çalışma zamanı verileri ve olayları hakkında bilgi toplamak için **Log Analytics** ayarını açabilirsiniz. Bu ayar, Azure Izleyici günlükleri bağlayıcısının kullanılması için gerekli değildir.

![Mantıksal uygulama oluşturma](media/logicapp-flow-connector/create-logic-app.png)


**Gözden geçir + oluştur** ve sonra **Oluştur**' a tıklayın. Dağıtım tamamlandığında, **Logic Apps tasarımcısını**açmak Için **Kaynağa Git** ' e tıklayın.

### <a name="create-a-trigger-for-the-logic-app"></a>Mantıksal uygulama için bir tetikleyici oluşturma
**Ortak bir tetikleyiciden başla**' nın altında **yinelenme**' yi seçin. Bu, düzenli aralıklarla otomatik olarak çalışan bir mantıksal uygulama oluşturur. İşlemin **Sıklık** kutusunda, **gün** ' yı seçin, **zaman aralığı** kutusuna **1** girerek iş akışını günde bir kez çalıştırın.

![Yinelenme eylemi](media/logicapp-flow-connector/recurrence-action.png)

## <a name="walkthrough-mail-visualized-results"></a>İzlenecek yol: posta görselleştirilmemiş sonuçlar
Aşağıdaki öğreticide, bir Azure Izleyici günlük sorgusunun sonuçlarını e-posta ile gönderen bir mantıksal uygulama oluşturma gösterilmektedir. 

### <a name="add-azure-monitor-logs-action"></a>Azure Izleyici günlükleri ekleme eylemi
Yineleme eyleminden sonra çalışacak bir eylem eklemek için **+ yeni adım** ' a tıklayın. **Eylem seçin**altında **Azure izleyici** yazın ve ardından **Azure izleyici günlükleri**' ni seçin.

![Azure Izleme günlükleri eylemi](media/logicapp-flow-connector/select-azure-monitor-connector.png)

Azure Log Analytics ' e tıklayın **– sorgu çalıştırın ve sonuçları görselleştirin**.

![Sorgu Çalıştır ve sonuçları görselleştirin eylemi](media/logicapp-flow-connector/select-query-action-visualize.png)


### <a name="add-azure-monitor-logs-action"></a>Azure Izleyici günlükleri ekleme eylemi

Log Analytics çalışma alanınız için **aboneliği** ve **kaynak grubunu** seçin. **Kaynak türü** Için *Log Analytics çalışma alanı* ' nı seçin ve ardından **kaynak adı**altında çalışma alanının adını seçin.

**Sorgu** penceresine aşağıdaki günlük sorgusunu ekleyin.  

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1day)
| summarize TotalErrors=count() by Computer
| sort by Computer asc   
```

**Grafik türü**Için **zaman aralığı** ve **HTML tablosu** *sorgusunda ayarla '* yı seçin.
   
![Sorgu Çalıştır ve sonuçları görselleştirin eylemi](media/logicapp-flow-connector/run-query-visualize-action.png)

Posta, geçerli bağlantıyla ilişkili hesap tarafından gönderilir. **Bağlantıyı Değiştir**' i tıklayarak başka bir hesap belirtebilirsiniz.

### <a name="add-email-action"></a>E-posta eylemi Ekle

**+ Yeni adım**' a ve ardından **+ Eylem Ekle**' ye tıklayın. **Eylem seçin**altında **Outlook** yazın ve ardından **Office 365 Outlook**' u seçin.

![Outlook bağlayıcısını seçin](media/logicapp-flow-connector/select-outlook-connector.png)

**E-posta gönder (v2)** seçeneğini belirleyin.

![Office 365 Outlook seçim penceresi](media/logicapp-flow-connector/select-mail-action.png)

Bir **dinamik içerik** penceresi açmak için **gövde** kutusunda herhangi bir yere tıklayın, mantıksal uygulamadaki önceki eylemlerden alınan değerlerle açılır. Log Analytics eyleminde sorgu sonuçları olan **daha fazla göster** ve **ilet** ' i seçin.

![Gövde Seç](media/logicapp-flow-connector/select-body.png)

**Kime** penceresinde bir alıcının e-posta adresini **ve ilgili e**-postanın konusunu belirtin. 

![Posta eylemi](media/logicapp-flow-connector/mail-action.png)


### <a name="save-and-test-your-logic-app"></a>Mantıksal Uygulamanızı kaydetme ve test etme
**Kaydet** ' e ve ardından **Çalıştır** ' a tıklayıp mantıksal uygulamanın test çalıştırmasını gerçekleştirin.

![Kaydet ve Çalıştır](media/logicapp-flow-connector/save-run.png)


Mantıksal uygulama tamamlandığında, belirttiğiniz alıcının postasını kontrol edin.  Aşağıdakine benzer bir gövde içeren bir e-posta almış olmanız gerekir:

![Örnek e-posta](media/logicapp-flow-connector/sample-mail.png)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici 'de günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
- [Logic Apps](/azure/logic-apps/) hakkında daha fazla bilgi edinin
- [Microsoft Flow](https://ms.flow.microsoft.com)hakkında daha fazla bilgi edinin.

