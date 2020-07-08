---
title: Verizon 'dan özel raporlar | Microsoft Docs
description: "Aşağıdaki raporları kullanarak CDN 'niz için kullanım düzenlerini görüntüleyebilirsiniz: bant genişliği, aktarılan veri, Isabet sayısı, önbellek durumları, Isabetli önbellek okuması oranı, ıPV4/ıPV6 verileri aktarıldı."
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/11/2017
ms.author: allensu
ms.openlocfilehash: cff355aeb74efc9dec2780840ae57fea4c5764cc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84888717"
---
# <a name="custom-reports-from-verizon"></a>Verizon Özel Raporları

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Verizon profilleri için yönetim portalı aracılığıyla Verizon özel raporları kullanarak, Edge CNAMEs raporları için toplanacak veri türünü tanımlayabilirsiniz.


## <a name="accessing-verizon-custom-reports"></a>Verizon özel raporlarına erişme
1. CDN profili dikey penceresinde **Yönet** düğmesine tıklayın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analiz** sekmesinin üzerine gelin ve ardından **özel raporlar** açılır öğesi üzerine gelin. **Edge CNAMEs**' e tıklayın.
   
    ![CDN yönetim portalı-özel raporlar menüsü](./media/cdn-reports/cdn-custom-reports.png)

## <a name="edge-cnames-custom-report"></a>Edge CNAMES özel raporu
Edge CNAMES özel raporu, özel rapor günlüğü 'nün etkinleştirildiği Edge CNAMEs için isabet ve veri aktarılan istatistikleri sağlar. Edge CNAMEs Azure CDN uç nokta ana bilgisayar adları ve ilişkili özel etki alanı ana bilgisayar adlarından oluşur. 

Özel rapor veri günlüğü, bir sınır CNAME 'in özel raporlama özelliğini etkinleştirdikten sonra bir saat başlar. Belirli bir platform veya tüm platformlar için bir kenar CNAMEs raporu oluşturarak rapor verilerini görüntüleyebilirsiniz. Bu raporun kapsamı, belirli bir süre boyunca özel rapor verilerinin toplandığı kenar cisimleriyle sınırlıdır. Edge CNAMEs raporu, ölçüm seçeneğinde tanımlanan ölçüye göre ilk 10 Edge CNAMEs için bir grafik ve veri tablosundan oluşur. 

Aşağıdaki rapor seçeneklerini tanımlayarak özel bir rapor oluşturun:

- Ölçümler: aşağıdaki seçenekler desteklenir:

   - İsabetler: özel raporlama yeteneğinin etkinleştirildiği bir kenar CNAME 'e yönlendirilen isteklerin toplam sayısını gösterir. Bu ölçüm istemciye döndürülen durum kodunu içermez.

   - Aktarılan veriler: özel raporlama yeteneğinin etkinleştirildiği bir kenar CNAME 'e yönlendirilen istekler için uç sunuculardan HTTP istemcilerine (örneğin, Web tarayıcıları) aktarılan toplam veri miktarını gösterir. Aktarılan veri miktarı, yanıt gövdesine HTTP yanıt üst bilgileri eklenerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarı gerçek dosya boyutundan daha büyük.

- Gruplandırmalar: çubuk grafiğin altında gösterilen istatistiklerin türünü belirler. Aşağıdaki seçenekler desteklenir:

   - HTTP yanıt kodları: istemciye döndürülen HTTP yanıt koduna (örneğin, 200, 403, vb.) göre istatistikleri düzenler. 

   - Önbellek durumu: istatistikleri önbellek durumuna göre düzenler.


Raporun tarih aralığını ayarlamak için, açılan listeden **bugün** veya **Bu hafta**gibi önceden tanımlanmış bir tarih aralığı seçebilir ya da **özel** ' i seçip, takvim simgeleri ' ne tıklayarak bir tarih aralığını el ile girebilirsiniz. 

Tarih aralığını seçtikten sonra, raporu oluşturmak için **Git** ' e tıklayın.

**Git** düğmesinin sağındaki Excel simgesine tıklayarak Excel biçimindeki verileri dışarı aktarabilirsiniz.

![CNAMEs raporu](./media/cdn-reports/cdn-cnames-report.png)

## <a name="edge-cnames-custom-report-fields"></a>Edge CNAMES özel rapor alanları

| Alan                     | Açıklama   |
|---------------------------|---------------|
| 2xx                       | Edge CNAME için bir 2xx HTTP durum kodu ile sonuçlanan toplam istek sayısını veya (MB) verileri belirtir (örneğin, 200 Tamam). |
| 3xx                       | Edge CNAME için bir 3xx HTTP durum kodu ile sonuçlanan toplam istek sayısını veya (MB) verileri belirtir (örneğin, 302 bulundu veya 304 değiştirilmez. |
| 4xx                       | Edge CNAME için bir 4xx HTTP durum kodu (örneğin, 400 Hatalı Istek, 403 yasaklanmış veya 404 bulunamadı) ile sonuçlanan toplam istek sayısını veya aktarılan verileri gösterir. |
| 5xx                       | X xx HTTP durum koduna (örneğin, 500 Iç sunucu hatası veya 502 hatalı ağ geçidi) neden olan Edge CNAME için aktarılan toplam istek veya veri sayısını gösterir (MB). |
| İsabetli önbellek okuması yüzdesi               | Doğrudan önbellekten istek sahibine sunulan önbelleklenebilir isteklerin yüzdesini gösterir. |
| İsabetli Önbellek Okuma Sayısı                | Uç CNAME için (örneğin, TCP_EXPIRED_HIT, TCP_HIT veya TCP_PARTIAL_HIT) sonuçlanan toplam istek sayısını veya aktarılan verileri gösterir. İstenen içeriğin önbelleğe alınmış bir sürümü bulunduğunda önbellek okuması oluşur. |
| Aktarılan veri (MB)     | Sınır CNAME için uç sunuculardan HTTP istemcilerine (Web tarayıcıları) aktarılan toplam veri miktarını (MB) gösterir. Aktarılan veri miktarı, yanıt gövdesine HTTP yanıt üst bilgileri eklenerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarı gerçek dosya boyutundan daha büyük. |
| Açıklama               | Ana bilgisayar adına göre bir Edge CNAME 'i tanımlar |
| İsabetler                      | Uç CNAME 'e gönderilen toplam istek sayısını gösterir |
| Kaç                    | Uç CNAME için bir önbellek isabetsizliği (örneğin, TCP_CLIENT_REFRESH_MISS, TCP_EXPIRED_MISS veya TCP_MISS) ile sonuçlanan toplam istek veya aktarılan veri sayısını belirtir. İstenen içerik, isteği karşılayan uç sunucuda önbelleğe alınmamışsa önbellek isabetsizliği oluşur. | 
| Önbellek yok                  | Edge CNAME için CONFIG_NOCACHE önbellek durum kodu ile sonuçlanan toplam istek sayısını veya (MB) verileri gösterir.  |
| Diğer                     | Edge CNAME için aktarılan toplam istek sayısını veya verileri (MB), 2xx-5xx aralığının dışında kalan bir HTTP durum kodu ile sonuçlandığını gösterir. |
| Platform                  | Sınır CNAME 'in trafiği işleyen platformu gösterir. |
| Atanmamış               | Önbellek durum kodu veya HTTP durum kodu bilgilerinin günlüğe kaydedildiği uç CNAME için toplam istek veya veri aktarımı (MB) sayısını gösterir.  |
| Önbelleklenebilir               | Sınır olmayan önbellek durum koduna neden olan Edge CNAME için aktarılan toplam istek sayısını veya verileri (MB) gösterir.  |


## <a name="considerations"></a>Önemli noktalar
Raporlar yalnızca son 18 ay içinde oluşturulabilir.

