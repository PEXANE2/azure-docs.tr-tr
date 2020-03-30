---
title: Azure Log Analytics'te verileri görüntüleme ve analiz etme | Microsoft Dokümanlar
description: Log Analytics kullanıcıları için Azure Monitor günlük sorgusu deneyiminde arama günlüğü desteği.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/16/2019
ms.openlocfilehash: 6cc7fc94162ed101d06fbbbc4722867be46987b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670143"
---
# <a name="transition-from-log-analytics-log-search-to-azure-monitor-logs"></a>Log Analytics günlük aramasından Azure Monitor günlüklerine geçiş
Log Analytics'teki günlük araması, kısa bir süre önce Azure Monitor günlüklerini analiz etmek için yeni bir deneyimle değiştirildi. Günlük arama sayfasına Şu anda Azure portalındaki Log **Analytics çalışma alanları** sayfasındaki **Günlükler (klasik)** menü öğesi aracılığıyla erişilebiliyor, ancak 15 Şubat 2019'da kaldırılacak. Bu makalede, günlük arama geçiş yardımcı olmak için iki deneyim arasındaki farklar açıklanmaktadır. 

## <a name="filter-results-of-a-query"></a>Sorgunun filtreleme sonuçları
Günlük Arama'da, arama sonuçları teslim edilirken filtrelerin listesi görüntülenir. Bir filtre seçin ve seçili filtreyle sorguyu çalıştırmak için **Uygula'yı** tıklatın.

![Günlük Arama filtresi](media/log-search-transition/filter-log-search.png)

Azure Monitor günlüklerinde, filtreleri görüntülemek için *Filtre (önizleme)* seçeneğini belirleyin. Ek filtreleri görüntülemek için filtre simgesine tıklayın. Bir filtre seçin ve seçili filtreyle sorguyu çalıştırmak için **Çalıştır &** uygula'yı tıklatın.

![Günlükleri filtresi](media/log-search-transition/filter-logs.png)

## <a name="extract-custom-fields"></a>Özel alanları ayıklama 
Günlük Arama'da, bir alanın menüsünde _Tablo'dan_eylem Ayıklama alanlarını içeren Liste görünümünden [özel alanlar](../platform/custom-fields.md) ayıklarsınız.

![Günlük Arama ayıklama alanları](media/log-search-transition/extract-fields-log-search.png)

Azure Monitor günlüklerinde, tablo görünümünden özel alanlar ayıklarsınız. Soldaki oku tıklatarak bir kaydı genişletin ve _ayıklama alanları_ eylemine erişmek için elipsleri tıklatın.

![Günlükleri ayıklama alanları](media/log-search-transition/extract-fields-logs.png)

## <a name="functions-and-computer-groups"></a>Fonksiyonlar ve bilgisayar grupları
Günlük Arama'da bir aramayı kaydetmek **için, bir** ad, kategori ve sorgu metni sağlamak için Kaydedilmiş aramalar ve **Ekle'yi** seçin. İşlev takma adı ekleyerek bir [bilgisayar grubu](../platform/computer-groups.md) oluşturun.

![Günlük aramakaydet](media/log-search-transition/save-search-log-search.png)

Geçerli sorguyu Azure Monitor günlüklerine kaydetmek için **Kaydet'i**seçin. _İşlev_ **olarak Kaydet'i** değiştirin ve bir [işlev](functions.md)oluşturmak için bir **Işlev Adı** sağlayın. Bir [bilgisayar grubu](../platform/computer-groups.md)için işlev diğer adını kullanmak için _bu sorguyu bilgisayar grubu olarak kaydet'i_ seçin.

![Günlük sorgusuna kaydet](media/log-search-transition/save-query-logs.png)

## <a name="saved-queries"></a>Kaydedilen sorgular
Günlük Arama'da, kaydedilen sorgularınız eylem çubuğu öğesi **Kaydedilmiş aramalar**aracılığıyla kullanılabilir. Azure Monitor günlüklerinde, [Sorgu Gezgini'nden](../log-query/get-started-portal.md#save-queries)kaydedilen sorgulara erişin.

![Sorgu gezgini](media/log-search-transition/query-explorer.png)

## <a name="drill-down-on-summarized-rows"></a>Özetlenen satırları ayrıntıya göre detaylandırma
Günlük Arama'da, özetlenmiş bir sorgudaki bir satıra tıklayarak, bu satırdaki ayrıntılı kayıtları listeleyen başka bir sorgu başlatabilirsiniz.

![Günlük Arama detaylandırma](media/log-search-transition/drilldown-search.png)

Azure Monitor günlüklerinde, bu kayıtları döndürmek için sorguyu değiştirmeniz gerekir. Sonuçlardaki satırlardan birini genişletin ve **+** sorguya eklemek için değerin yanındaki nitı tıklatın. Sonra **özet** komutu dışarı yorum ve sorguyu yeniden çalıştırın.

![Azure Monitor, ayrıntılı olarak günlüklerini kaydeder](media/log-search-transition/drilldown-logs.png)

## <a name="take-action"></a>Harekete geçin
Günlük Arama'da, **Eyleme Al'ı**seçerek bir arama sonucundan [bir runbook başlatabilirsiniz.](take-action.md)

![Harekete geçin](media/log-search-transition/take-action-log-search.png)

Azure Monitor günlüklerinde, [günlük sorgusundan bir uyarı oluşturun.](../platform/alerts-log.md) Bir eylem grubunu uyarıya yanıt olarak çalışacak bir veya daha fazla eylemle yapılandırın.

![Eylem grubu](media/log-search-transition/action-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- Yeni [Azure Monitör günlükleri deneyimi](get-started-portal.md)hakkında daha fazla bilgi edinin.
