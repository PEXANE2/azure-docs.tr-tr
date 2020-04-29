---
title: Özel parametrelerle Azure Izleyici çalışma kitapları
description: Önceden oluşturulmuş ve özel parametreli çalışma kitapları ile karmaşık raporlamayı kolaylaştırın
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 4d9f6e48722f01970a90a3a1d8d8b58b5d939774
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77658288"
---
# <a name="interactive-workbooks"></a>Etkileşimli Çalışma Kitapları

Çalışma kitapları yazarların tüketicilere yönelik etkileşimli raporlar ve deneyimler oluşturmalarına olanak tanır. Etkileşim, çeşitli yollarla desteklenir.

## <a name="parameter-changes"></a>Parametre değişiklikleri
Bir çalışma kitabı kullanıcısı bir parametreyi güncelleştirdiğinde, parametresini kullanan herhangi bir denetim otomatik olarak yenilenir ve yeni durumu yansıtacak şekilde yeniden çizer. Azure portal raporlarının çoğu etkileşimi destekler. Çalışma kitapları bunu en az Kullanıcı çabalarıyla çok basit bir şekilde sağlar.

[Çalışma kitaplarında parametreler](workbooks-parameters.md) hakkında daha fazla bilgi edinin

## <a name="grid-row-clicks"></a>Kılavuz satırı tıklamaları
Çalışma kitapları yazarların bir kılavuzdaki bir satırı tıklatmak, izleyen grafikleri satır içeriğine göre güncelleştirmesine olanak tanır. 

Örneğin, bir Kullanıcı bir istek listesini ve hata sayısı gibi bazı istatistikleri gösteren bir kılavuza sahip olabilir. Bu, bir isteğe karşılık gelen bir satıra tıklanmayacak şekilde ayarlanabileceğinden, yalnızca bu isteğe filtre uygulamak için ayrıntılı grafiklerin güncelleştirilmesine neden olur.

### <a name="setting-up-interactivity-on-grid-row-click"></a>Kılavuz satırı üzerinde etkileşim kurma
1. _Düzenleme_ araç çubuğu öğesine tıklayarak çalışma kitabını düzenleme moduna geçirin.
2. Çalışma kitabına bir günlük sorgusu denetimi eklemek için _Sorgu Ekle_ bağlantısını kullanın. 
3. Sorgu türünü _günlük_, kaynak türü (örneğin, Application Insights) ve hedeflenecek kaynakları seçin.
4. Analiz etmek için KQL 'i girmek üzere sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize AllRequests = count(), FailedRequests = countif(success == false) by Request = name
    | order by AllRequests desc    
    ```
5. `Run query`sonuçları görmek için
6. Sorgu altbilgisindeki _Gelişmiş ayarlar_ simgesine tıklayın (simge dişli gibi görünür). Bu, Gelişmiş ayarlar bölmesini açar 
7. Ayarı denetleyin:`When an item is selected, export a parameter`
    1. Dışarı aktarılacak alan:`Request`
    2. Parametre adı:`SelectedRequest`
    3. Varsayılan değer:`All requests`
    
    ![Alanları parametre olarak dışarı aktarmaya yönelik ayarlarla gelişmiş düzenleyiciyi gösteren resim](./media/workbooks-interactive/advanced-settings.png)

8. `Done Editing` öğesine tıklayın.
9. 2 ve 3. adımları kullanarak başka bir sorgu denetimi ekleyin.
10. Analiz etmek için KQL 'i girmek üzere sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | where name == '{SelectedRequest}' or 'All Requests' == '{SelectedRequest}'
    | summarize ['{SelectedRequest}'] = count() by bin(timestamp, 1h)
    ```
11. `Run query`sonuçları görmek için.
12. _Görselleştirmeyi_ değiştirme`Area chart`
12. İlk kılavuzdaki bir satıra tıklayın. Aşağıdaki alan grafiğinin seçili istek için nasıl filtreleyeceğini aklınızda bulabilirsiniz.

Elde edilen rapor, düzenleme modunda şöyle görünür:

![Kılavuz satırı tıklamalarını kullanarak etkileşimli bir deneyim oluşturmayı gösteren resim](./media/workbooks-interactive//grid-click-create.png)

Aşağıdaki görüntüde, aynı ilkelere bağlı olarak okuma modunda daha ayrıntılı bir etkileşimli rapor gösterilmektedir. Raporda, parametreleri dışarı aktarmak için tıklama tıklamaları kullanılır. Bu, sırasıyla iki grafikte ve bir metin bloğunda kullanılır.

![Kılavuz satırı tıklamalarını kullanarak etkileşimli bir deneyim oluşturmayı gösteren resim](./media/workbooks-interactive/grid-click-read-mode.png)

### <a name="exporting-the-contents-of-an-entire-row"></a>Tüm satırın içeriğini dışa aktarma
Bazı durumlarda, yalnızca belirli bir sütun yerine seçili satırın tüm içeriğinin dışarı aktarılması tercih edilir. Bu gibi durumlarda, yukarıdaki 7,1 `Field to export` adımında özelliği ayarını yok olarak bırakın. Çalışma kitapları, tüm satır içeriğini parametresine JSON olarak dışa aktarır. 

Başvuran KQL denetiminde, JSON 'u ayrıştırmak ve `todynamic` ayrı sütunlara erişmek için işlevini kullanın.

 ## <a name="grid-cell-clicks"></a>Izgara hücresi tıklamaları
Çalışma kitapları, yazarlara, adlı özel bir `link renderer`ızgara sütun işleyicisi türü aracılığıyla etkileşim eklemesine olanak tanır. Bağlantı Oluşturucu, bir kılavuz hücresini hücrenin içeriğine göre köprüye dönüştürür. Çalışma kitapları, kaynak genel bakış dikey pencereleri, özellik paketi görüntüleyicileri, uygulama öngörüleri arama, kullanım, işlem izleme vb. gibi birçok bağlantı Oluşturucu türünü destekler.

### <a name="setting-up-interactivity-using-grid-cell-clicks"></a>Kılavuz hücre tıklamalarını kullanarak etkileşimi ayarlama
1. _Düzenleme_ araç çubuğu öğesine tıklayarak çalışma kitabını düzenleme moduna geçirin.
2. Çalışma kitabına bir günlük sorgusu denetimi eklemek için _Sorgu Ekle_ bağlantısını kullanın. 
3. Sorgu türünü _günlük_, kaynak türü (örneğin, Application Insights) ve hedeflenecek kaynakları seçin.
4. Analiz etmek için KQL 'i girmek üzere sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize Count = count(), Sample = any(pack_all()) by Request = name
    | order by Count desc
    ```
5. `Run query`sonuçları görmek için
6. Ayarlar bölmesini açmak için _sütun ayarları_ ' na tıklayın.
7. _Sütunlar_ bölümünde, şunu ayarlayın:
    1. _Örnek_ sütun Oluşturucu: `Link`, açılacak görünüm: `Cell Details`, bağlantı etiketi:`Sample`
    2. _Count_ -Column işleyici: `Bar`, renk paleti: `Blue`, minimum değer:`0`
    3. _İstek_ sütunu işleyici:`Automatic`
    4. Değişiklikleri uygulamak için _Kaydet ve Kapat_ ' a tıklayın
8. Kılavuzdaki `Sample` bağlantılardan birine tıklayın. Bu, Örneklenmiş bir isteğin ayrıntılarıyla birlikte bir özellik bölmesi açar.

    ![Kılavuz hücre tıklamalarını kullanarak etkileşimli bir deneyim oluşturmayı gösteren resim](./media/workbooks-interactive/grid-cell-click-create.png)

### <a name="link-renderer-actions"></a>Bağlantı Oluşturucu eylemleri
| Bağlantı eylemi | Tıklama eylemi |
|:------------- |:-------------|
| `Generic Details` | Bir özellik Kılavuzu bağlam dikey penceresinde satır değerlerini gösterir |
| `Cell Details` | Bir özellik Kılavuzu bağlam dikey penceresinde hücre değerini gösterir. Hücre (örneğin, konum, rol örneği vs. gibi istek özelliklerine sahip JSON) içeren dinamik bir tür içerdiğinde yararlı olur. |
| `Cell Details` | Bir özellik Kılavuzu bağlam dikey penceresinde hücre değerini gösterir. Hücre (örneğin, konum, rol örneği vs. gibi istek özelliklerine sahip JSON) içeren dinamik bir tür içerdiğinde yararlı olur. |
| `Custom Event Details` | Hücrede özel olay KIMLIĞI (ItemId) ile Application Insights arama ayrıntılarını açar |
| `* Details` | Bağımlılıklar, özel durumlar, sayfa görünümleri, istekler ve izlemeler dışında özel olay ayrıntılarına benzer. |
| `Custom Event User Flows` | Hücredeki özel olay adında özetlenen Application Insights Kullanıcı Akışları deneyimini açar |
| `* User Flows` | Özel durum, sayfa görünümleri ve istekler hariç özel olay Kullanıcı Akışları benzer |
| `User Timeline` | Kullanıcı zaman çizelgesini hücrede Kullanıcı KIMLIĞI (user_Id) ile açar |
| `Session Timeline` | Hücredeki değer için Application Insights arama deneyimini açar (örneğin, ABC hücresindeki değer olan ' abc ' metnini arayın) |
| `Resource overview` | Hücredeki kaynak KIMLIĞI değerini temel alarak, kaynağın genel görünümünü portalda açın |

## <a name="conditional-visibility"></a>Koşullu görünürlük
Çalışma kitabı, kullanıcıların belirli denetimleri parametre değerlerine göre görünmesine veya kaybolmasına olanak sağlar. Bu, yazarların, kullanıcı girişine veya telemetri durumuna göre raporların farklı görünmesini sağlar. Örnek, tüketicilere yalnızca bir Özet gösterir, ancak yanlış bir şeyler olduğunda tam ayrıntıları gösterir.

### <a name="setting-up-interactivity-using-conditional-visibility"></a>Koşullu görünürlük kullanarak etkileşim kurma
1. İki etkileşimli denetim ayarlamak için `Setting up interactivity on grid row click` bölümündeki adımları izleyin.
2. En üste yeni bir parametre ekleyin:
    1. Ada`ShowDetails`
    2. Parametre türü:`Drop down`
    3. Gerekli:`checked`
    4. Veri al:`JSON`
    5. JSON girişi:`["Yes", "No"]`
    6. Değişiklikleri uygulamak için Kaydet.
3. Parametre değerini ayarla`Yes`
4. Alan grafiğinin sorgu denetiminde _Gelişmiş ayarlar_ simgesine (dişli simgesi) tıklayın
5. Ayarı denetleyin`Make this item conditionally visible`
    1. Bu öğe parametre değeri `ShowDetails` `equals` ise görünür`Yes`
6. Değişiklikleri uygulamak için _düzenlemeleri bitti_ ' ye tıklayın.
7. Okuma moduna girmek için çalışma kitabı araç çubuğunda _Düzenle bitti_ ' ye tıklayın.
8. Parametresinin `ShowDetails` değerini olarak `No`değiştirin. Aşağıdaki grafiğin kaybolduğuna dikkat edin.

Aşağıdaki görüntüde olduğu `ShowDetails` gibi görünen durum gösterilmektedir`Yes`

![Grafiğin görünür olduğu koşullu görünürlüğü gösteren resim](./media/workbooks-interactive/conditional-visibility.png)

Aşağıdaki görüntüde nerede `ShowDetails` olduğu gizli durum gösterilmektedir`No`

![Grafiğin gizli olduğu koşullu görünürlüğü gösteren resim](./media/workbooks-interactive/conditional-invisible.png)

## <a name="next-steps"></a>Sonraki adımlar


* Çok sayıda zengin görselleştirmeler seçeneği hakkında daha fazla [bilgi edinmeye başlayın](workbooks-visualizations.md) .
* Çalışma kitabı kaynaklarınıza erişimi [denetleme](workbooks-access-control.md) ve paylaşma.
