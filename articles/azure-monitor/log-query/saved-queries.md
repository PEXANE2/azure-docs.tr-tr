---
title: Azure Izleyici 'de kaydedilmiş sorgular Log Analytics
description: Gereksinimlerinize göre başlayabilmeniz ve değiştirebileceğiniz sorgular
ms.subservice: logs
ms.topic: article
author: rboucher
ms.author: robb
ms.date: 06/16/2020
ms.openlocfilehash: 462358ab967d4ba26a265d7b5f401b995223815e
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959851"
---
# <a name="saved-queries-in-azure-monitor-log-analytics"></a>Azure Izleyici 'de kaydedilmiş sorgular Log Analytics

Log Analytics kendi sorgularınız için kendi içinde çalıştırabileceğiniz veya bir başlangıç noktası olarak kullanabileceğiniz örnek sorgu kümesi sunar. Bu makalede örnek sorgular ve bunların nasıl kullanılacağı açıklanmaktadır.

Log Analytics veya KQL sorgu dili hakkında bilgi sahibi değilseniz, örnek sorgular başlamak için harika bir yoldur. Bir kaynağa anında Öngörüler sunabilir ve KQL 'yi öğrenmenin ve kullanmanın kolay bir yolunu sunduklarında, Log Analytics kullanmaya başlamak için gereken süreyi kısaltabilirler. Size anında değer sağlamak için tasarlanan 250 örnek sorgu ve bu sayıda örnek sorgu sürekli büyümekte olan üzerinde toplandık.

## <a name="in-context-queries"></a>Bağlam içi sorgular

Yeni deneyim filtreler ve bağlamdaki sorguları önerir. Diğer bir deyişle, sistem otomatik olarak yalnızca seçtiğiniz kapsamla ilgili sorguları gösterir.

- Tek bir **kaynak** için: sorgular kaynak türüne göre filtrelenmiştir.
- Bir **kaynak grubu** için-sorgular, belirli kaynak grubundaki kaynaklara göre filtrelenmiştir.
- Bir **çalışma alanı** için – sorgular, çalışma alanına yüklenmiş çözümlere göre filtrelenmiştir.

Bu davranış tüm Log Analytics kapsamları için tutarlıdır. İstediğiniz kaynak türü için örnek bir sorgu görmüyorsanız, bunun nedeni kapsam dışı olması nedeniyle filtreler olabilir. Sonraki bölümde, tüm olası sorguları görüntüleyebilmeniz için bağlam kapsamındaki kapsamları nasıl kaldırabileceğinizi açıklar.

> [!TIP]
> Kapsamınızda daha fazla kaynak varsa portalın örnek sorgu iletişim kutusunu filtrelemesine ve göstermesi daha uzun zaman.

## <a name="example-query-user-interface"></a>Örnek kullanıcı arabirimini sorgulama

İki farklı konumdan örnek sorgulara ulaşabilirsiniz.

### <a name="example-query-dialog"></a>Örnek sorgu iletişim kutusu

Log Analytics deneyimini ilk kez girdiğinizde, *örnek sorgular iletişim kutusu* otomatik olarak gösterilir.  Ayrıca, ekranın sağ üst köşesinde **örnek sorgular**' a tıklanarak da erişilebilir.

![Kutusuna](media/saved-queries/sidebar-2.png)

Örnek sorgu iletişim kutusu daha sonra aşağıda gösterildiği gibi görünür:  

![Örnek sorgular ekranı](media/saved-queries/example-query-start.png)

Önceki ekran görüntüsünde bir Azure Key Vault örneğine ait Günlükler ekranı görüntülenir. Daha önce Bu makalede belirtildiği gibi, sorgular bağlam içinde gösterilir.  Sonuç olarak, ekran görüntüsü yalnızca ilgili Key Vault örnekleri gösterir. Bir aboneliğin tamamını seçerseniz, Bu abonelikteki tüm kaynak türleri için sorgular görüntülenir.  

Her örnek sorgu bir kart tarafından temsil edilir. İhtiyaç duyduğunuz şeyi bulmak için sorguları hızlıca tarayabilirsiniz. Sorguyu doğrudan iletişim kutusundan çalıştırabilir veya ek ayarlamalar yapmak ve daha fazla ayarlama yapmak için sorgu düzenleyicisine yüklemeyi tercih edebilirsiniz.

### <a name="sidebar-query-experience"></a>Kenar çubuğu sorgu deneyimi

İletişim deneyiminin tüm işlevlerine, Log Analytics sol taraftaki kenar çubuğu 'ndaki sorgular bölmesinden erişilebilir. Sorgu açıklamasını ve ek işlevselliği almak için bir sorgu adının üzerine geldiğinizde.

![Kutusuna](media/saved-queries/sidebar-3.png)

## <a name="finding-and-filtering-queries"></a>Sorguları bulma ve filtreleme

Bu bölümdeki seçenekler hem iletişim kutusu hem de yan çubuk sorgu deneyiminde, ancak biraz farklı bir kullanıcı arabirimi ile kullanılabilir.  

### <a name="use-favorites"></a>Sık kullanılanları kullan

Daha hızlı erişim sağlamak için sık kullanılan sorguları sık kullanılanlara ekleyebilirsiniz.

> [!TIP]
> Daha sonra sorguları toplama ve görüntüleme, başlamak için iyi bir yoldur. İhtiyacınız olan sorguları bulun ve sık kullanılanlara eklemek için sorgunun yanındaki yıldıza tıklayın. Sorgu daha sonra sizin için yararlı değilse, bunu sık kullanılanlara ekleyebilirsiniz.  

### <a name="filtering-and-group-by"></a>Filtreleme ve gruplama ölçütü

Sorgu iletişim kutusu, filtrelerin 'yi yalnızca doğru bağlamla sorguları gösterecek şekilde, filtre kaldırmayı ve tüm sorguları görmeyi seçebilirsiniz.

### <a name="group-by"></a>Gruplandırma ölçütü:

Gruplandırma *ölçütü* açılır listesine tıklayarak sorguların gruplandırmasını değiştirin:

![Örnek sorgular ekran GroupBy](media/saved-queries/example-query-groupby.png)

İletişim kutusu gruplama şunları destekler:

- **Kaynak türü** : Azure 'Da bir sanal makine gibi tanımlanan bir kaynaktır. Azure Izleyici günlüklerinin/Log Analytics tablolarının kaynak türüne tam eşlenmesinin [Azure Izleyici Tablo başvurusuna](/azure/azure-monitor/reference/tables/tables-resourcetype) bakın.  
- **Category** : *güvenlik* veya *Denetim*gibi bir bilgi türü. Kategoriler, tablolar yan bölmesinde tanımlanan kategorilerle aynıdır. Kategorilerin tam listesi için bkz. [Azure Izleyici tablo başvurusu](/azure/azure-monitor/reference/tables/tables-category) .  
- **Çözüm** : sorgularla Ilişkili bir Azure izleyici çözümü
- **Konu** : *etkinlik günlükleri* veya *uygulama günlükleri*gibi örnek sorgunun konusu. Konu özelliği, örnek sorgular için benzersizdir ve belirli kaynak türüne göre farklılık gösterebilir.

Gruplandırma değerleri Ayrıca etkin bir içindekiler tablosu olarak davranır. Ekranın sol tarafındaki değerlerden birine tıkladığınızda sorgular görünümü ' ni tıklanan öğeye kaydırır.

### <a name="filter"></a>Filtre

Sorguları daha önce bahsedilen GroupBy değerlerine göre de filtreleyebilirsiniz. Örnek sorgu iletişim kutusunda, filtreler en üstte bulunur.

![Örnek sorgular ekran filtresi](media/saved-queries/example-query-filter.png)

### <a name="combining-group-by-and-filter"></a>Gruplandırma ölçütü ve filtresi

Filtre ve gruplandırma işlevleri, birlikte çalışmak üzere tasarlanmıştır. Bunlar, sorguların nasıl düzenlendiğine esneklik sağlar. Örneğin, birden çok kaynakla bir kaynak grubu kullanıyorsanız, belirli bir kaynak türüne filtre uygulamak ve sonuçta elde edilen sorguları konuya göre düzenlemek isteyebilirsiniz.

## <a name="sample-query-dialog-appearance-behavior"></a>Örnek sorgu iletişim kutusu görünüm davranışı

Bir KQL Pro kullanıyorsanız ve doğrudan sorgu düzenleyicisine almayı tercih ediyorsanız, sorgu iletişim kutusunu "kapalı" olarak değiştirebilirsiniz. İki durumlu düğme, Log Analytics ekran yüklenirken örnek sorgu iletişim kutusu yüklenmez.

![Örnek kapalı](media/saved-queries/examples-on-off.png)

Örnek sorgu açılan deneyimine her zaman Log Analytics en üstteki çubukta *örnek sorgular* düğmesinden erişebilirsiniz.

## <a name="query-explorer"></a>Sorgu Gezgini

Kullanıcı tarafından oluşturulan sorguları kaydetme ve paylaşma için sorgu Gezgini deneyimi, zamanında değişmeden kalır.

## <a name="next-steps"></a>Sonraki adımlar

[KQL sorgularını kullanmaya başlama](get-started-queries.md)

