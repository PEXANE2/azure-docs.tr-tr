---
title: Azure Izleyici 'de Log Analytics genel bakış
description: Azure Izleyici günlüklerinde verileri çözümlemek için günlük sorgularını düzenlemek ve çalıştırmak için kullanılan Azure portal bir araç olan Log Analytics açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/04/2020
ms.openlocfilehash: 485c2cc6c95f5922025fe887f7c25131e849a01d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100625730"
---
# <a name="overview-of-log-analytics-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics genel bakış
Log Analytics, Azure Izleyici günlüklerinde verilerle günlük sorgularını düzenlemek ve çalıştırmak için kullanılan Azure portal bir araçtır. Kayıt kümesi döndüren basit bir sorgu yazabilir ve ardından bunları sıralamak, filtrelemek ve analiz etmek için Log Analytics özellikleri kullanabilirsiniz. Ya da istatistiksel analizler gerçekleştirmek için daha gelişmiş bir sorgu yazabilir ve belirli bir eğilimi tanımlamak için bir grafikteki sonuçları görselleştirebilirsiniz. Sorgularınızın sonuçlarıyla etkileşimli olarak çalışıp çalışmadığınızı veya bunları günlük sorgu uyarıları veya çalışma kitapları gibi diğer Azure Izleyici özellikleriyle birlikte kullanmaya devam etmeksizin Log Analytics, bunları yazmak ve test etmek için kullandığınız araçtır. 


> [!TIP]
> Bu makalede, Log Analytics ve özelliklerinin her birinin açıklaması sağlanmaktadır. Bir öğreticiye doğrudan gitmek istiyorsanız [Log Analytics öğreticisine](../log-query/log-analytics-tutorial.md)bakın.



## <a name="starting-log-analytics"></a>Log Analytics başlatılıyor
Azure portal **Azure izleyici** menüsünde **günlüklerden** Log Analytics başlatın. Ayrıca bu seçeneği, birçok Azure kaynağı için menüde de görürsünüz. İçinden başlattığınız yere bakılmaksızın, aynı Log Analytics aracı olacaktır. Log Analytics başlatmak için kullandığınız menü, ancak kullanılabilecek verileri belirler. Bunu **Azure izleyici** menüsünde veya **Log Analytics çalışma alanları** menüsünden başlatırsanız, çalışma alanındaki tüm kayıtlara erişebilirsiniz. Başka bir kaynak türünden **Günlükler** ' i seçerseniz verileriniz bu kaynak için günlük verileriyle sınırlandırılır. Ayrıntılar için bkz. [Azure izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics](../log-query/scope.md) .

[![Log Analytics Başlat](media/log-analytics-overview/start-log-analytics.png)](media/log-analytics-overview/start-log-analytics.png#lightbox)

Log Analytics başlattığınızda, göreceğiniz ilk şey [örnek sorgular](../logs/example-queries.md)içeren bir iletişim kutusudur. Bunlar çözüme göre kategorize edilir ve belirli gereksinimlerinize uyan sorgulara gözatabilir veya arama yapabilirsiniz. Tam olarak istediğiniz şeyi bulabilir veya bir düzenleyiciyi bir tane yükleyebilir ve gerektiği şekilde değiştirebilirsiniz. Örnek sorgulara göz atmak aslında kendi sorgularınızı yazmayı öğrenmek için harika bir yoldur. Kuşkusuz, boş bir komut dosyası ile başlamak ve kendiniz yazmak istiyorsanız örnek sorguları kapatabilirsiniz. Geri almak istiyorsanız, ekranın en üstündeki **sorgulara** tıklamanız yeterlidir.

## <a name="log-analytics-interface"></a>Log Analytics arabirimi
Aşağıdaki görüntüde Log Analytics farklı bileşenleri tanımlanmaktadır.

[![Log Analytics](media/log-analytics-overview/log-analytics.png)](media/log-analytics-overview/log-analytics.png#lightbox)

### <a name="1-top-action-bar"></a>1. üst eylem çubuğu
Sorgu penceresinde sorguyla çalışma denetimleri.

| Seçenek | Açıklama |
|:---|:---|
| Kapsam | Sorgu için kullanılan veri kapsamını belirtir. Bu, birden çok çalışma alanında belirli bir kaynak için Log Analytics çalışma alanındaki veya verilerdeki tüm veriler olabilir. Bkz. [sorgu kapsamı](../log-query/scope.md). |
| Çalıştır düğmesi | Seçili sorguyu sorgu penceresinde çalıştırmak için tıklayın. Bir sorgu çalıştırmak için SHIFT + enter tuşlarına da basabilirsiniz. |
| Saat seçici | Sorgu için kullanılabilir olan veriler için zaman aralığını seçin. Sorguya bir zaman filtresi eklerseniz bu geçersiz kılınır. Bkz. [Azure izleyici Log Analytics günlük sorgusu kapsamı ve zaman aralığı](../log-query/scope.md). |
| Kaydet düğmesi | Sorguyu çalışma alanı için sorgu Gezgini ' ne kaydedin. |
 Kopyala düğmesini | Sorgunun bir bağlantısını, sorgu metnini veya sorgu sonuçlarını panoya kopyalayın. |
| Yeni uyarı kuralı düğmesi | Boş bir sorgu ile yeni bir sekme oluşturun. |
| Dışarı aktar düğmesi | Sorgunun sonuçlarını, Power BI ile kullanmak üzere bir CSV dosyasına veya sorgu Power Query formül dil biçimine dışarı aktarın. |
| Panoya sabitle düğmesi | Sorgunun sonuçlarını bir Azure panosuna ekleyin. |
| Biçim sorgu düğmesi | Seçili metni okunabilirlik için düzenleyin. |
| Örnek sorgular düğmesi | Log Analytics ilk kez açtığınızda görüntülenen örnek sorgular iletişim kutusunu açın. |
| Sorgu Gezgini düğmesi | Çalışma alanındaki kayıtlı sorgulara erişim sağlayan **sorgu Gezginini** açın. |


### <a name="2-sidebar"></a>2. kenar çubuğu
Geçerli sorgu için çalışma alanındaki tablo listeleri, örnek sorgular ve filtre seçenekleri.

| Tab | Description |
|:---|:---|
| Tablolar | Seçilen kapsamın parçası olan tabloları listeler. Tabloların gruplandırmasını değiştirmek için **Group By** ' ı seçin. Bir tablo adının üzerine gelerek, belgelerinin açıklaması ve verilerinin önizlemesini görüntülemek için tablo ve seçeneklerin açıklamasıyla birlikte bir iletişim kutusu görüntüleyin. Sütunlarını görüntülemek için bir tablo genişletin. Sorguya eklemek için tablo veya sütun adına çift tıklayın. |
| Sorgular | Sorgu penceresinde açabileceğiniz örnek sorgularının listesi. Log Analytics açtığınızda görüntülenen liste aynıdır. Sorguların gruplandırmasını değiştirmek için **Grup ölçütü** ' nü seçin. Sorgu penceresine eklemek için bir sorguya çift tıklayın veya diğer seçenekler için üzerine gelin. |
| Filtre | Sorgunun sonuçlarına göre filtre seçenekleri oluşturur. Bir sorgu çalıştırdıktan sonra sütunlar, sonuçlardan farklı değerlerle birlikte görüntülenir. Bir veya daha fazla değer seçin ve sonra sorguya bir **WHERE** komutu eklemek ve uygulamayı yeniden çalıştırmak Için **& Çalıştır** ' a tıklayın. |

### <a name="3-query-window"></a>3. sorgu penceresi
Sorgu penceresi, sorgunuzu düzenlediğiniz yerdir. Bu, KQL komutları için IntelliSense ve okunabilirliği iyileştirmek için renk kodlaması içerir. **+** Pencerenin en üstündeki öğesine tıklayarak başka bir sekme açın.

Tek bir pencerede birden çok sorgu bulunabilir. Bir sorgu boş satır içeremez, böylece bir pencerede birden çok sorguyu bir veya daha fazla boş satır ile ayırabilirsiniz. Geçerli sorgu, imlecin içinde herhangi bir yerde konumlandırılmış olan bir sorgudur.

Geçerli sorguyu çalıştırmak için **Çalıştır** düğmesine tıklayın veya SHIFT + enter tuşlarına basın.

### <a name="4-results-window"></a>4. sonuçlar penceresi
Sorgunun sonuçları sonuçlar penceresinde görüntülenir. Varsayılan olarak, sonuçlar tablo olarak görüntülenir. Grafik olarak görüntülemek için, sonuçlar penceresinde **grafik** ' i seçin ya da sorgunuza bir **render** komutu ekleyin.

#### <a name="results-view"></a>Sonuçlar görünümü
Sorgu sonuçlarını sütunlara ve satırlara göre düzenlenmiş bir tabloda görüntüler. Değerlerini genişletmek için bir satırın soluna tıklayın. Sütun listesini değiştirmek için **sütunlar** açılan listesine tıklayın. Bir sütun adına tıklayarak sonuçları sıralayın. Sütun adının yanındaki huniye tıklayarak sonuçları filtreleyin. Filtreleri temizleyin ve sorguyu yeniden çalıştırarak sıralamayı sıfırlayın.

Sorgu sonuçlarının üzerinde gruplandırma çubuğunu göstermek için **Grup sütunları** ' nı seçin. Sonuçları çubuğa sürükleyerek herhangi bir sütuna göre gruplayın. Ek sütun ekleyerek sonuçlarda iç içe gruplar oluşturun. 

#### <a name="chart-view"></a>Grafik görünümü
Sonuçları, birden fazla kullanılabilir grafik türünden biri olarak görüntüler. Sorgunuzun bir **render** komutunda grafik türünü belirtebilir veya **görselleştirme türü** açılan listesinden seçebilirsiniz.

| Seçenek | Açıklama |
|:---|:---|
| **Görselleştirme türü** | Görüntülenecek grafiğin türü. |
| **X ekseni** | Sonuçlarda X ekseninde kullanılacak olan sütun 
| **Y ekseni** | Sonuçlarda Y ekseni için kullanılacak olan sütun. Bu genellikle sayısal bir sütun olur. |
| **Bölme ölçütü** | Sonuçlardaki seriyi, grafikte tanımlayan sütun. Sütundaki her bir değer için bir seri oluşturulur. |
| **Toplama** | Y eksenindeki sayısal değerlerde gerçekleştirilecek toplama türü. |

## <a name="relationship-to-azure-data-explorer"></a>Azure Veri Gezgini ile ilişki
Azure Veri Gezgini Web Kullanıcı arabirimine zaten alışdıysanız Log Analytics tanıdık görünmelidir. Bunun nedeni Azure Veri Gezgini üzerine kurulmuştur ve aynı kusto sorgu dilini (KQL) kullanır. Log Analytics, Azure Izleyici 'ye özgü, zaman aralığına göre filtreleme ve bir sorgudan uyarı kuralı oluşturma gibi özellikler ekler. Her iki araç de kullanılabilir tabloların yapısını taramanızı sağlayan bir gezgin içeriyordu, ancak Azure Veri Gezgini Web Kullanıcı arabirimi, bir Log Analytics çalışma alanındaki tablolarla çalışırken Log Analytics birincil olarak Azure Veri Gezgini veritabanlarındaki tablolarla birlikte çalışmaktadır. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portal Log Analytics kullanma hakkında öğreticiyi](../log-query/log-analytics-tutorial.md)adım adım inceleyin.
- [Sorgu yazma hakkında öğreticiyi](../log-query/get-started-queries.md)adım adım inceleyin.