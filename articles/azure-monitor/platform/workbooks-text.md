---
title: Azure Monitor çalışma kitapları metin parametreleri
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin. Çalışma kitabı metin parametreleri hakkında daha fazla bilgi edinin.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: c804cc8942a40e2f30c980636194daa82e0fb0e8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687337"
---
# <a name="workbook-text-parameters"></a>Çalışma kitabı metin parametreleri

Textbox parametreleri, çalışma kitabı kullanıcılarından metin girişi toplamak için basit bir yol sağlar. Girişi toplamak için açılır bir uygulama olmadığında (örneğin, rasgele bir eşik veya genel filtreler) kullanılırlar. Çalışma kitapları, yazarların bir sorgudan textbox'ın varsayılan değerini almasına olanak sağlar. Bu, metnin p95'ine göre varsayılan eşiği ayarlama gibi ilginç senaryolara izin verir.

Textboxes'ın yaygın kullanımı, diğer çalışma kitabı denetimleri tarafından kullanılan dahili değişkenler gibidir. Bu, varsayılan değerler için bir sorgudan yararlanarak ve giriş denetimini okuma modunda görünmez hale getirerek yapılır. Örneğin, bir kullanıcı bir formülden (kullanıcı değil) bir eşik gelmesini ve ardından sonraki sorgularda eşiği kullanmasını isteyebilir.

## <a name="creating-a-text-parameter"></a>Metin parametresi oluşturma
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`SlowRequestThreshold`
    2. Parametre türü:`Text`
    3. Gerekli:`checked`
    4. Sorgudan varsayılan değer alın:`unchecked`
5. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

    ![Metin parametresi oluşturulmasını gösteren resim](./media/workbooks-text/text-create.png)

Çalışma kitabı okuma modunda böyle görünecektir.

![Okuma modunda metin parametresi gösteren resim](./media/workbooks-text/text-readmode.png)

## <a name="referencing-a-text-parameter"></a>Metin parametresi gönderme
1. Mavi `Add query` bağlantıyı seçerek çalışma kitabına bir sorgu denetimi ekleyin ve bir Application Insights kaynağını seçin.
2. KQL kutusuna, bu parçacığı ekleyin:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= {SlowRequestThreshold}) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
3. Sorgu denetimi ile birleştiğinde 500 değeri olan metin parametresini kullanarak aşağıdaki sorguyu etkin bir şekilde çalıştırabilirsiniz:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= 500) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
4. Sonuçları görmek için sorguyu çalıştır

    ![KQL'de başvurulan metin parametresi gösteren resim](./media/workbooks-text/text-reference.png)

> [!NOTE]
> Yukarıdaki örnekte, `{SlowRequestThreshold}` bir sonda değerini temsil eder. Parametre alanının tırnak işareti `{ComputerName}` olmadan kabul girişine ekleyebilmesi için `"{ComputerName}"` Kusto sorgunuzu değiştirmek için Kusto sorgunuzu değiştirmeniz gerekir gibi bir dize için sorgu yapıyorsanız.

## <a name="setting-default-values"></a>Varsayılan değerleri ayarlama
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`SlowRequestThreshold`
    2. Parametre türü:`Text`
    3. Gerekli:`checked`
    4. Sorgudan varsayılan değer alın:`checked`
5. KQL kutusuna, bu parçacığı ekleyin:
    ```kusto
    requests
    | summarize round(percentile(duration, 95), 2)
    ```
    Bu sorgu, metin kutusunun varsayılan değerini uygulamadaki tüm istekler için yüzde 95'lik yüzdelik süreye ayarlar.
6. Sonucu görmek için sorguyu çalıştır
7. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

    ![KQL'den varsayılan değeri olan bir metin parametresi gösteren resim](./media/workbooks-text/text-default-value.png)

> [!NOTE]
> Bu örnek Uygulama Öngörüleri verilerini sorgularken, bu yaklaşım günlük tabanlı veri kaynağı için kullanılabilir - Günlük Analizi, Azure Kaynak Grafiği, vb.

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
