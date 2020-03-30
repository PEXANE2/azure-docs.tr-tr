---
title: Özel parametrelere sahip Azure Monitör çalışma kitapları
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 4d9f6e48722f01970a90a3a1d8d8b58b5d939774
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658288"
---
# <a name="interactive-workbooks"></a>Etkileşimli Çalışma Kitapları

Çalışma kitapları, yazarların tüketicileri için etkileşimli raporlar ve deneyimler oluşturmasına olanak sağlar. Etkileşim çeşitli şekillerde desteklenir.

## <a name="parameter-changes"></a>Parametre Değişiklikleri
Çalışma kitabı kullanıcısı bir parametreyi güncellediğinde, parametreyi kullanan tüm denetimler yeni durumu yansıtacak şekilde otomatik olarak yenilenir ve yeniden çizer. Azure portalı raporlarının çoğu etkileşimi bu şekilde destekler. Çalışma kitapları en az kullanıcı çabası ile çok düz bir şekilde bu sağlar.

[Çalışma Kitaplarındaki Parametreler](workbooks-parameters.md) hakkında daha fazla bilgi edinin

## <a name="grid-row-clicks"></a>Izgara satır tıklamaları
Çalışma kitapları, yazarların ızgaradaki bir satırı tıklatmanın sonraki grafikleri satırın içeriğine göre güncellediği senaryolar oluşturmalarına olanak tanır. 

Örneğin, bir kullanıcı istekleri ve hata sayıları gibi bazı istatistikleri listesini gösteren bir ızgara olabilir. Bir isteğe karşılık gelen bir satırı tıklatmak, sadece bu isteğe filtre lemek için güncelleştirme altında ayrıntılı grafikler neden olacak şekilde ayarlayabilirsiniz.

### <a name="setting-up-interactivity-on-grid-row-click"></a>Kılavuz satırında etkileşim ayarlama
1. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için _sorgu ekle_ bağlantısını kullanın. 
3. Sorgu türünü _Günlük,_ kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize AllRequests = count(), FailedRequests = countif(success == false) by Request = name
    | order by AllRequests desc    
    ```
5. `Run query`sonuçları görmek için
6. Sorgu altbilgisindeki _Gelişmiş Ayarlar_ simgesine tıklayın (simge dişli ye benzer). Bu gelişmiş ayarlar bölmesini açar 
7. Ayarı kontrol edin:`When an item is selected, export a parameter`
    1. Dışa aktarılabilmek için alan:`Request`
    2. Parametre adı:`SelectedRequest`
    3. Varsayılan değer:`All requests`
    
    ![Parametreler olarak alanları dışa aktarma ayarlarıile gelişmiş düzenleyiciyi gösteren resim](./media/workbooks-interactive/advanced-settings.png)

8. `Done Editing` öğesine tıklayın.
9. 2 ve 3 adımlarını kullanarak başka bir sorgu denetimi ekleyin.
10. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | where name == '{SelectedRequest}' or 'All Requests' == '{SelectedRequest}'
    | summarize ['{SelectedRequest}'] = count() by bin(timestamp, 1h)
    ```
11. `Run query`sonuçları görmek için.
12. _Görselleştirmeyi_ değiştir`Area chart`
12. İlk Kılavuzdaki bir satıra tıklayın. Aşağıdaki alan grafiğinin seçili isteğe nasıl filtre uygulayabildiğini unutmayın.

Elde edilen rapor, edit modunda şuna benzer:

![Kılavuz satır tıklamalarını kullanarak oluşturmayı etkileşimli bir deneyim gösteren resim](./media/workbooks-interactive//grid-click-create.png)

Aşağıdaki resimde, aynı ilkelere dayalı okuma modunda daha ayrıntılı bir etkileşimli rapor gösterilmektedir. Rapor, sırayla iki grafik ve bir metin bloğunda kullanılan parametreleri dışa aktarmak için ızgara tıklamalarını kullanır.

![Kılavuz satır tıklamalarını kullanarak oluşturmayı etkileşimli bir deneyim gösteren resim](./media/workbooks-interactive/grid-click-read-mode.png)

### <a name="exporting-the-contents-of-an-entire-row"></a>Tüm satırın içeriğini dışa aktarma
Bazen yalnızca belirli bir sütun yerine seçili satırın tüm içeriğini dışa aktarmak istenir. Bu gibi durumlarda, `Field to export` yukarıdaki adım 7.1'de mülkü açık bırakın. Çalışma kitapları tüm satır içeriğini json olarak parametreye dışa aktaracaktır. 

Başvuru kql denetimi, json `todynamic` ayrışdırmak ve tek tek sütunlara erişmek için işlevi kullanın.

 ## <a name="grid-cell-clicks"></a>Izgara Hücre Tıklamaları
Çalışma kitapları, yazarların özel `link renderer`bir ızgara sütun uyrucu türü aracılığıyla etkileşim eklemelerine olanak sağlar. Bağlantı işleyicisi, ızgara hücresini hücrenin içeriğine göre bir köprüye dönüştürür. Çalışma kitapları, kaynak genel bakış bıçakları, özellik çantası görüntüleyiciler, App Insights arama, kullanım, işlem izleme, vb. açılmasına izin verenler de dahil olmak üzere birçok bağlantı görüntüleyicisini destekler.

### <a name="setting-up-interactivity-using-grid-cell-clicks"></a>Izgara hücre tıklamalarını kullanarak etkileşim ayarlama
1. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için _sorgu ekle_ bağlantısını kullanın. 
3. Sorgu türünü _Günlük,_ kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize Count = count(), Sample = any(pack_all()) by Request = name
    | order by Count desc
    ```
5. `Run query`sonuçları görmek için
6. Ayarlar bölmesini açmak için _Sütun Ayarları'nı_ tıklatın.
7. _Sütunlar_ bölümünde, ayarlayın:
    1. _Örnek_ - Sütun `Link`İşleyici: , `Cell Details`Açmak için görüntüle: , Bağlantı Etiketi:`Sample`
    2. _Sayı_ - Sütun `Bar`İşleyicisi: `Blue`, Renk paleti: , Minimum değer:`0`
    3. _İstek_ - Sütun İşleyicisi:`Automatic`
    4. Değişiklikleri uygulamak için _Kaydet ve Kapat'ı_ tıklatın
8. Izgaradaki `Sample` bağlantılardan birine tıklayın. Bu, örneklenmiş bir isteğin ayrıntılarını içeren bir özellik bölmesi açar.

    ![Izgara hücre tıklamalarını kullanarak oluşturmayı etkileşimli bir deneyim gösteren resim](./media/workbooks-interactive/grid-cell-click-create.png)

### <a name="link-renderer-actions"></a>Bağlantı Renderer Eylemler
| Bağlantı eylemi | Tıklama üzerine eylem |
|:------------- |:-------------|
| `Generic Details` | Özellik ızgara bağlam bıçaksatır değerlerini gösterir |
| `Cell Details` | Özellik ızgara bağlam bıçak hücre değerini gösterir. Hücre bilgi içeren dinamik bir tür içerdiğinde yararlıdır (örneğin, konum, rol örneği, vb. gibi istek özelliklerine sahip json). |
| `Cell Details` | Özellik ızgara bağlam bıçak hücre değerini gösterir. Hücre bilgi içeren dinamik bir tür içerdiğinde yararlıdır (örneğin, konum, rol örneği, vb. gibi istek özelliklerine sahip json). |
| `Custom Event Details` | Hücredeki özel olay kimliği (itemId) ile Uygulama Öngörüleri arama ayrıntılarını açar |
| `* Details` | Bağımlılıklar, özel durumlar, sayfa görünümleri, istekler ve izlemeler dışında Özel Olay Ayrıntıları'na benzer. |
| `Custom Event User Flows` | Uygulama Öngörüleri Kullanıcı Akışları deneyimini hücredeki özel olay adı üzerinde döner açar |
| `* User Flows` | Özel Etkinlik Kullanıcı Akışlarına benzer özel durumlar, sayfa görünümleri ve istekleri dışında |
| `User Timeline` | Hücrede kullanıcı kimliği (user_Id) ile kullanıcı zaman çizelgesini açar |
| `Session Timeline` | Hücredeki değer için Application Insights arama deneyimini açar (örneğin, hücredeki değerin ABC olduğu 'abc' metnini arayın) |
| `Resource overview` | Hücredeki kaynak kimliği değerini temel alan kaynağın genel görünümünü portalda açma |

## <a name="conditional-visibility"></a>Koşullu Görünürlük
Çalışma kitabı, kullanıcıların parametrelerin değerlerine bağlı olarak belirli denetimlerin görünmesini veya kaybolmasını sağlar. Bu, yazarların kullanıcı girişine veya telemetri durumuna göre raporların farklı görünmesini sağlar. Bir örnek, tüketicilere her şey yolunda yken sadece bir özet göstermek, ancak bir sorun olduğunda tüm ayrıntıları göstermektir.

### <a name="setting-up-interactivity-using-conditional-visibility"></a>Koşullu görünürlüğü kullanarak etkileşim ayarlama
1. İki etkileşimli `Setting up interactivity on grid row click` denetim ayarlamak için bölümdeki adımları izleyin.
2. En üstte yeni bir parametre ekleyin:
    1. Adı:`ShowDetails`
    2. Parametre türü:`Drop down`
    3. Gerekli:`checked`
    4. Veri alın:`JSON`
    5. JSON Girişi:`["Yes", "No"]`
    6. Değişiklikleri işlemek için kaydedin.
3. Parametre değerini`Yes`
4. Alan grafiğiyle sorgu denetiminde _Gelişmiş Ayarlar_ simgesini (vites simgesi) tıklatın
5. Ayarı kontrol edin`Make this item conditionally visible`
    1. Bu öğe görünür `ShowDetails` ise `equals` parametre değeri`Yes`
6. Değişiklik işlemek için _Bitti Düzenlemesi'ni_ tıklatın.
7. Okuma moduna girmek için çalışma kitabı araç çubuğunda _Bitti Düzenleme'yi_ tıklatın.
8. Parametrenin `ShowDetails` değerini ' `No`ye çevirin Aşağıdaki grafiğin kaybolduğuna dikkat edin.

Aşağıdaki resimde görünür durumda `ShowDetails` nerede olduğunu gösterir`Yes`

![Grafiğin görünür olduğu koşullu görünürlüğü gösteren resim](./media/workbooks-interactive/conditional-visibility.png)

Aşağıdaki resimde gizli servis `ShowDetails` talebi`No`

![Grafiğin gizlendiği koşullu görünürlüğü gösteren resim](./media/workbooks-interactive/conditional-invisible.png)

## <a name="next-steps"></a>Sonraki adımlar


* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
