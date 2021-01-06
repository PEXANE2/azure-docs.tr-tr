---
title: Azure Sentinel 'de Kullanıcı ve varlık davranış analizi (UEBA) ile gelişmiş tehditleri tanımla | Microsoft Docs
description: Varlıklar (kullanıcılar, ana bilgisayar adları, IP adresleri) için davranış temelleri oluşturun ve anormal davranışları tespit etmek ve sıfır günlük gelişmiş kalıcı tehditleri (APT) belirlemek için bunları kullanın.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/19/2020
ms.author: yelevin
ms.openlocfilehash: 2326746d274c68225cd4c8569df6a20d6050ec1a
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97900901"
---
# <a name="identify-advanced-threats-with-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Azure Sentinel 'de Kullanıcı ve varlık davranış analizi (UEBA) ile gelişmiş tehditleri tanımla

> [!IMPORTANT]
>
> - Artık UEBA ve varlık sayfaları özellikleri şu Azure Sentinel coğrafi bölgeler ve bölgelerde **genel kullanıma** sunulmuştur:
>    - Birleşik Devletler Coğrafya
>    - Avrupa Batı Bölgesi
>    - Avustralya Coğrafya
>
> - Diğer tüm coğrafi bölgeler ve bölgelerde bu özellikler **Önizleme** aşamasında olmaya devam eder. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [**Microsoft Azure önizlemeleri için ek kullanım koşulları**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

## <a name="what-is-user-and-entity-behavior-analytics-ueba"></a>Kullanıcı ve varlık davranış analizi (UEBA) nedir?

### <a name="the-concept"></a>Kavram

Kuruluşunuzun içindeki tehditleri ve olası etkisini belirleme-güvenliği aşılmış bir varlık veya kötü amaçlı bir Insider, her zaman zaman alan ve işgücü yoğun bir işlem olup olmadığı. Uyarıları kullanarak uyarıları bağlama, noktaların bağlanması ve etkin aramaya kadar çok az miktarda zaman ve çaba, en az dönüşlerle sona ererek ve gelişmiş tehditler sayesinde bulmayı artırma olasılığı vardır. Özellikle, sıfır gün, hedeflenen ve gelişmiş kalıcı tehditler gibi gereksinimlerinden tehditleri, kuruluşunuzun en çok risk altında olduğundan, algılamalarını daha fazla kritik hale getirebilirsiniz.

Azure Sentinel 'de UEBA özelliği, analistlerin iş yüklerinden ve çalışmalarından drudgery ortadan kaldırır ve yüksek uygunluğa sahip, işlem yapılabilir zeka sunarak araştırma ve düzeltmeye odaklanabilirler.

Azure Sentinel, tüm bağlı veri kaynaklarından Günlükler ve uyarılar toplarken, bunları analiz eder ve zaman ve eş grup ufuk genelindeki kuruluşunuzun varlıklarının (kullanıcılar, konaklar, IP adresleri, uygulamalar vb.) temel davranış profillerini derler. Farklı teknikler ve makine öğrenimi özellikleri kullanarak, Sentinel etkinlikleri tanımlayabilir ve bir varlığın tehlikede olup olmadığını belirlemenize yardımcı olur. Yalnızca bu, belirli varlıkların göreli hassasiyetini belirleyebilir, varlık gruplarını tanımlayabilir ve belirli bir güvenliği aşılmış varlıkların olası etkisini ("değerlendirme yarıçapı") değerlendirebilir. Bu bilgilerle birlikte araştırmanıza ve olay işleme göre önceliklendirmenize etkin bir şekilde öncelik verebilirsiniz. 

### <a name="architecture-overview"></a>Mimariye genel bakış

:::image type="content" source="media/identify-threats-with-entity-behavior-analytics/entity-behavior-analytics-architecture.png" alt-text="Varlık davranışı analitik mimarisi":::

### <a name="security-driven-analytics"></a>Güvenlik odaklı analiz

Vartner 'ın UEBA çözümleri için paradigması sayesinde Azure Sentinel, üç başvuru çerçevesine göre "dışarıdan" bir yaklaşım sağlar:

- **Kullanım örnekleri:** İlgili saldırı vektörlerine ve senaryolarına dayalı güvenlik araştırmalarını temel alarak, çeşitli varlıkları her bir kurbana, bir, bir veya sonlandırma zincirindeki Özet noktaları olarak yerleştiren, tekniklerin ve alt tekniklerin, MITTIK ATT&CK çerçevesiyle hizalı güvenlik araştırması Azure Sentinel, her bir veri kaynağının sağlayabildiği en değerli günlüklere odaklanır.

- **Veri kaynakları:** İlk ve Azure veri kaynaklarını desteklerken, Azure Sentinel, tehdit senaryolarımızla eşleşen verileri sağlamak için üçüncü taraf veri kaynaklarını seçer.

- **Analiz:** Azure Sentinel, çeşitli makine öğrenimi (ML) algoritmaları kullanarak anormal etkinlikleri tanımlar ve açık ve öz yanı sıra bağlamsal zenginler biçiminde, aşağıda gösterilen bazı örnekler sunar.

    :::image type="content" source="media/identify-threats-with-entity-behavior-analytics/behavior-analytics-top-down.png" alt-text="Davranış analizi dış yerinde yaklaşım":::

Azure Sentinel, Güvenlik analistlerinin bağlamdaki anormal etkinlikleri net bir şekilde öğrenme ve kullanıcının temel profiliyle karşılaştırılmasının yanı sıra yapıları sunar. Bir Kullanıcı (veya bir konak ya da bir adres) tarafından gerçekleştirilen eylemler, "true" sonucunun tanımlanan bir anomali olduğunu gösterdiği bağlamsal olarak değerlendirilir:
- coğrafi konumlar, cihazlar ve ortamlar üzerinde.
- zaman ve sıklık Horizons (kullanıcının kendi geçmişiyle karşılaştırıldığında).
- eşlerin davranışına kıyasla.
- Kuruluşunuzun davranışına kıyasla.

    :::image type="content" source="media/identify-threats-with-entity-behavior-analytics/context.png" alt-text="Varlık bağlamı":::


### <a name="scoring"></a>Puanlama

Her etkinlik "araştırma önceliği puanı" ile puanlanır. Bu, belirli bir kullanıcının belirli bir etkinliği gerçekleştirme olasılığını belirleyen, kullanıcının ve eşlerinin davranış öğrenimine göre belirlenir. En olağandışı şekilde tanımlanan etkinlikler en yüksek puanları alır (0-10 ölçeğinde).

Bunun nasıl çalıştığına ilişkin bir örnek için davranış analizinin [Microsoft Cloud App Security](https://techcommunity.microsoft.com/t5/microsoft-security-and/prioritize-user-investigations-in-cloud-app-security/ba-p/700136) nasıl kullanıldığını görün.

## <a name="entities-in-azure-sentinel"></a>Azure Sentinel 'de varlıklar

### <a name="entity-identifiers"></a>Varlık tanımlayıcıları

Uyarılar Azure Sentinel 'e gönderildiğinde, Azure Sentinel 'in Kullanıcı hesapları, konaklar, IP adresleri ve diğerleri gibi varlıkları tanımladığı ve sınıflandırdığı veri öğelerini içerirler. Bazen, uyarı varlık hakkında yeterli bilgi içermiyorsa bu kimlik bir sorun olabilir.

Örneğin, Kullanıcı hesapları birden çok şekilde tanımlanabilir: bir Azure AD hesabının sayısal tanımlayıcısı (GUID) veya Kullanıcı asıl adı (UPN) değeri veya alternatif olarak, Kullanıcı adının ve onun NT etki alanı adının bir birleşimi kullanılarak. Farklı veri kaynakları aynı kullanıcıyı farklı yollarla tanımlayabilir. Bu nedenle, mümkün olduğunda Azure Sentinel bu tanımlayıcıları, düzgün şekilde tanımlanabilmesi için tek bir varlığa birleştirir.

Bu durum, kaynak sağlayıcılarınızın bir varlığın yeterince tanımlanmadığı bir uyarı oluşturduğundan (örneğin, etki alanı adı bağlamı olmayan bir Kullanıcı adı) meydana gelebilir. Böyle bir durumda, kullanıcı varlığı, ayrı bir varlık olarak tanımlanabilecek aynı kullanıcı hesabının diğer örnekleriyle birleştirilemez ve bu iki varlık Birleşik yerine ayrı kalır.

Bu meydana gelme riskini en aza indirmek için, tüm uyarı sağlayıcılarınızın oluşturdukları uyarılarda varlıkları doğru şekilde tanımladıklarından emin olmalısınız. Ayrıca, Kullanıcı hesabı varlıklarının Azure Active Directory ile eşitlenmesi, Kullanıcı hesabı varlıklarını birleştirebilecek bir dizin oluşturamayacak.

Aşağıdaki varlık türleri şu anda Azure Sentinel 'de tanımlanmıştır:

- Kullanıcı hesabı (hesap)
- Yönetici
- IP adresi (IP)
- Kötü Amaçlı Yazılımlar
- Dosya
- İşleme
- Bulut uygulaması (Cloudadpplication)
- Etki alanı adı (DNS)
- Azure kaynağı
- Dosya (FileHash)
- Kayıt defteri anahtarı
- Kayıt defteri değeri
- Güvenlik grubu
- URL
- IoT cihazı
- Mailbox
- Posta kümesi
- Posta iletisi
- Gönderim postası

### <a name="entity-pages"></a>Varlık sayfaları

Bir aramada, bir uyarıda veya bir araştırmada herhangi bir varlık (Şu anda kullanıcılarla ve konaklarla sınırlı) ile karşılaşırsanız, varlığı seçebilir ve bu varlıkla ilgili yararlı bilgiler içeren bir veri sayfası olan varlık **sayfasına** yönlendirilirsiniz. Bu sayfada bulacağınız bilgi türleri, varlıkla ilgili temel gerçekleri, bu varlıkla ilgili önemli olayların bir zaman çizelgesini ve varlığın davranışı hakkında öngörüleri içerir.
 
Varlık sayfaları üç bölümden oluşur:
- Sol taraftaki panel, varlığın Azure Active Directory, Azure Izleyici, Azure Güvenlik Merkezi ve Microsoft Defender gibi veri kaynaklarından toplanan tanımlama bilgilerini içerir.

- Orta bölmede, varlıkla ilgili, uyarılar, yer işaretleri ve etkinlikler gibi önemli olayların bir grafik ve metin zaman çizelgesi gösterilmektedir. Etkinlikler Log Analytics 'den önemli olaylar toplamalarına sahiptir. Bu etkinlikleri algılayan sorgular Microsoft güvenlik araştırma ekipleri tarafından geliştirilir.

- Sağ taraftaki panel, varlık hakkındaki davranış öngörülerini sunar. Bu Öngörüler, anomali ve güvenlik tehditlerini hızlı bir şekilde belirlemenize yardımcı olur. Öngörüler, Microsoft Security Research ekipleri tarafından geliştirilmiştir ve anomali algılama modellerini temel alır.

### <a name="the-timeline"></a>Zaman çizelgesi

:::image type="content" source="./media/identify-threats-with-entity-behavior-analytics/entity-pages-timeline.png" alt-text="Varlık sayfaları zaman çizelgesi":::

Zaman çizelgesi, varlık sayfasının Azure Sentinel 'de davranış analizinin katkılarının önemli bir parçasıdır. Varlıkla ilgili olaylar hakkında bir hikaye sunar ve varlığın etkinliğini belirli bir zaman çerçevesinde anlamanıza yardımcı olur.

Birkaç önceden ayarlanmış seçenek arasından ( *son 24 saat*) veya özel olarak tanımlanmış zaman dilimine ayarlanmış **zaman aralığını** seçebilirsiniz. Ayrıca, zaman çizelgesindeki bilgileri belirli olay veya uyarı türleriyle sınırlayan filtreler ayarlayabilirsiniz.

Aşağıdaki öğe türleri zaman çizelgesine dahildir:

- Uyarılar-varlığın **eşlenmiş bir varlık** olarak tanımlandığı tüm uyarılar. Kuruluşunuzun [analiz kurallarını kullanarak özel uyarılar](./tutorial-detect-threats-custom.md)oluşturduğuna sahip olması durumunda kuralların varlık eşlemesinin düzgün şekilde yapıldığından emin olun.

- Yer işaretleri-sayfada gösterilen belirli varlığı içeren tüm yer işaretleri.

- Etkinlikler-varlıkla ilgili önemli olaylarının toplamı. 
 
### <a name="entity-insights"></a>Varlık öngörüleri
 
Varlık öngörüleri, analistlerinizin daha verimli ve etkili bir şekilde araştırılması için Microsoft güvenlik araştırmacıları tarafından tanımlanan sorgulardır Öngörüler, varlık sayfasının bir parçası olarak sunulur ve ana bilgisayarlar ve kullanıcılar hakkında tablo verileri ve grafikler biçiminde değerli güvenlik bilgileri sağlar. Burada bilgilere sahip olmanız, Log Analytics için gezinti yapmanız gerekmediği anlamına gelir. Öngörüler, oturum açma işlemleri, Grup eklemeleri, anormal etkinlikler ve daha fazlası ile ilgili verileri içerir ve anormal davranışları algılamak için gelişmiş ML algoritmaları içerir. Öngörüler aşağıdaki veri türlerini temel alır:
- Syslog
- SecurityEvent
- Denetim Günlükleri
- Oturum açma günlükleri
- Office etkinliği
- BehaviorAnalytics (UEBA) 
 
### <a name="how-to-use-entity-pages"></a>Varlık sayfalarını kullanma

Varlık sayfaları birden çok kullanım senaryosunun parçası olacak şekilde tasarlanmıştır ve olay yönetimi, araştırma grafiği, yer işaretleri veya doğrudan Azure Sentinel ana menüsündeki **varlık davranışı Analizi** altındaki varlık arama sayfasından erişilebilir.

:::image type="content" source="./media/identify-threats-with-entity-behavior-analytics/entity-pages-use-cases.png" alt-text="Varlık sayfası kullanım örnekleri":::


## <a name="data-schema"></a>Veri şeması

### <a name="behavior-analytics-table"></a>Davranış analizi tablosu

| Alan                     | Açıklama                                                         |
|---------------------------|---------------------------------------------------------------------|
| TenantId                  | kiracının benzersiz KIMLIK numarası                                      |
| Sourcerkaydedilmiş DID            | EBA olayının benzersiz KIMLIK numarası                                   |
| TimeGenerated             | etkinliğin oluşum zaman damgası                              |
| Timeişlendi             | EBA motorunun etkinliğin işlem zaman damgası            |
| ActivityType              | etkinliğin üst düzey kategorisi                                 |
| EylemTürü                | etkinliğin normalleştirilmiş adı                                     |
| UserName                  | etkinliği başlatan kullanıcının Kullanıcı adı                    |
| UserPrincipalName         | etkinliği başlatan kullanıcının tam Kullanıcı adı               |
| EventSource               | Özgün olayı sağlayan veri kaynağı                        |
| Sourceıpaddress           | Etkinliğin başlatıldığı IP adresi                        |
| SourceIPLocation          | etkinliğin başlatıldığı ve IP adresinden zenginleştirilmiş ülke |
| SourceDevice              | etkinliği Başlatan cihazın ana bilgisayar adı                  |
| Destinationıpaddress      | Etkinliğin hedefinin IP adresi                            |
| DestinationIPLocation     | Etkinlik hedefinin, IP adresinden zenginleştirilmiş ülkesi     |
| Hedef cihaz         | hedef cihazın adı                                           |
| **Usersinsıghts**         | dahil edilen kullanıcıların bağlamsal zenginler                            |
| **Devicesınsights**       | dahil edilen cihazların bağlamsal zenginleri                          |
| **Activityınsights**      | profil oluşturma etkinliğimize göre bağlamsal analiz              |
| **InvestigationPriority** | anomali puanı, 0-10 arasında (0 = benign, 10 = yüksek düzeyde anormal)         |
|

**Kullanıcısınsıghts**, **devicesınsights** ve **activityınsights** 'ta, [ueba zenginleştirmeleri başvuru belgesinde](ueba-enrichments.md)başvurulan bağlamsal zenginlerin tam kümesini görebilirsiniz.

### <a name="querying-behavior-analytics-data"></a>Davranış analizi verilerini sorgulama

[KQL](/azure/data-explorer/kusto/query/)kullanarak, davranış analizi tablosunu sorgulayabiliriz.

Örneğin, bir Azure kaynağında oturum açmayan bir kullanıcının, kullanıcının belirli bir ülkeden ilk kez bağlanmaya çalıştığı ve bu ülkeden gelen bağlantıların, kullanıcının eşleri için de yaygın olarak olduğu tüm durumları bulmak istiyorsam, aşağıdaki sorguyu kullanabiliriz:

```Kusto
BehaviorAnalytics
| where ActivityType == "FailedLogOn"
| where FirstTimeUserConnectedFromCountry == True
| where CountryUncommonlyConnectedFromAmongPeers == True
```

### <a name="user-peers-metadata---table-and-notebook"></a>Kullanıcı eşleri Meta verileri-tablo ve Not defteri

Kullanıcı eşleri Meta verileri tehdit Algılamalarda, bir olayı araştırmak ve olası bir tehdit için önemli bir bağlam sağlar. Güvenlik analistleri, kullanıcının etkinliklerinin kendi eşleriyle karşılaştırıldığında olağan dışı olup olmadığını tespit etmek üzere kullanıcının eşlerinin normal etkinliklerini gözlemleyebilirsiniz.

Azure Sentinel, kullanıcının Azure AD güvenlik grubu üyeliğine, posta listesine, et cetera 'ya bağlı olarak bir kullanıcının eşlerini hesaplar ve sıralar ve derecelendirilen 1-20 eşlerini **Userpeeranalytics** tablosuna depolar. Aşağıdaki ekran görüntüsünde, UserPeerAnalytics tablosunun şeması gösterilmektedir ve Kullanıcı kendinin en üst sekiz dereceli eşlerini görüntüler. Azure Sentinel, dereceyi hesaplamak için ağırlığı normalleştirmek üzere *Sıklık-ters belge sıklığı* (TF-IDF) algoritmasını kullanır: grubun daha küçük olması, ağırlığa göre daha yükseği. 

:::image type="content" source="./media/identify-threats-with-entity-behavior-analytics/user-peers-metadata.png" alt-text="Kullanıcı eşleri Meta veri tablosu ekran görüntüsü":::

Kullanıcı eşi meta verilerini görselleştirmek için Azure Sentinel GitHub deposunda sunulan [Jupyter Not defterini](https://github.com/Azure/Azure-Sentinel-Notebooks/tree/master/BehaviorAnalytics/UserSecurityMetadata) kullanabilirsiniz. Not defterini kullanma hakkında ayrıntılı yönergeler için bkz. [Kılavuzlu analiz-Kullanıcı güvenliği meta verileri](https://github.com/Azure/Azure-Sentinel-Notebooks/blob/master/BehaviorAnalytics/UserSecurityMetadata/Guided%20Analysis%20-%20User%20Security%20Metadata.ipynb) Not defteri.

### <a name="permission-analytics---table-and-notebook"></a>İzin Analizi-tablo ve Not defteri

İzin analizi, bir saldırgan tarafından kurumsal bir varlığın güvenliğinin tehlikeye atması olasılığını belirlemesine yardımcı olur. Bu etki, varlığın "envanter" yarıçapını de bilinir. Güvenlik analistleri, bu bilgileri, araştırmalar ve olay işleme önceliklerini belirlemek için kullanabilir.

Azure Sentinel, kullanıcının doğrudan veya gruplar ya da hizmet sorumluları aracılığıyla erişebileceği Azure aboneliklerini değerlendirerek, belirli bir kullanıcı tarafından Azure kaynakları için tutulan doğrudan ve geçişli erişim haklarını belirler. Bu bilgilerin yanı sıra kullanıcının Azure AD güvenlik grubu üyeliğinin tam listesi, **UserAccessAnalytics** tablosunda depolanır. Aşağıdaki ekran görüntüsünde, UserAccessAnalytics tablosundaki bir örnek satır gösterilmektedir ve Kullanıcı Alex Johnson. **Kaynak varlık** Kullanıcı veya hizmet sorumlusu hesabıdır ve **hedef varlık** , kaynak varlığın erişimi olan kaynaktır. **Erişim düzeyi** ve **erişim türü** değerleri, hedef varlığın erişim denetimi modeline bağlıdır. Alex 'ın Azure aboneliği *contoso oteller kiracısına* katkıda bulunan erişimine sahip olduğunu görebilirsiniz. Aboneliğin erişim denetimi modeli, Azure RBAC ' dir.   

:::image type="content" source="./media/identify-threats-with-entity-behavior-analytics/user-access-analytics.png" alt-text="Kullanıcı erişimi Analizi tablosunun ekran görüntüsü":::

İzin analizi verilerini görselleştirmek için Azure Sentinel GitHub deposundan [Jupyter Not defterini](https://github.com/Azure/Azure-Sentinel-Notebooks/tree/master/BehaviorAnalytics/UserSecurityMetadata) (yukarıda bahsedilen aynı not defteri) kullanabilirsiniz. Not defterini kullanma hakkında ayrıntılı yönergeler için bkz. [Kılavuzlu analiz-Kullanıcı güvenliği meta verileri](https://github.com/Azure/Azure-Sentinel-Notebooks/blob/master/BehaviorAnalytics/UserSecurityMetadata/Guided%20Analysis%20-%20User%20Security%20Metadata.ipynb) Not defteri.

### <a name="hunting-queries-and-exploration-queries"></a>Sorguları ve keşif sorgularını sorgulama

Azure Sentinel, BehaviorAnalytics tablosuna dayalı sorguları, araştırma sorgularını ve bir çalışma kitabını bir dizi ele alma, kullanıma hazır bir küme sağlar. Bu araçlar, anormal davranışları belirten belirli kullanım örneklerine odaklanan şekilde zenginleştirilmiş veriler sunar. 

Azure Sentinel 'de [araştırma ve araştırma grafı](./hunting.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'in varlık davranış analizi özellikleri hakkında bilgi edindiniz. Uygulamayla ilgili pratik yönergeler için ve elde ettiğiniz öngörüleri kullanmak için aşağıdaki makalelere bakın:

- Azure Sentinel 'de [varlık davranışı analizlerini etkinleştirin](./enable-entity-behavior-analytics.md) .
- [Güvenlik tehditleri Için araya](./hunting.md).