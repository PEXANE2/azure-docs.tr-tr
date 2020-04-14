---
title: Verizon Özel Raporlar | Microsoft Dokümanlar
description: "Aşağıdaki raporları kullanarak CDN'nizin kullanım desenlerini görüntüleyebilirsiniz: Bant Genişliği, Veri Aktarımı, Hits, Önbellek Durumları, Önbellek Isabet Oranı, IPV4/IPV6 Veri Aktarımı."
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
ms.topic: article
ms.date: 10/11/2017
ms.author: allensu
ms.openlocfilehash: 737803f5b7fcddb07787afbb48354b4ca922e9ed
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259757"
---
# <a name="custom-reports-from-verizon"></a>Verizon'dan Özel Raporlar

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Verizon profilleri için Yönet portalı üzerinden Verizon Özel Raporlar kullanarak, kenar CNAMEs raporları için toplanacak veri türünü tanımlayabilirsiniz.


## <a name="accessing-verizon-custom-reports"></a>Verizon Özel Raporlara Erişim
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analytics** sekmesinin üzerine binin, ardından **Özel Raporlar** uçuşunun üzerinde gezinin. **Kenar KNAME'lerini**tıklatın.
   
    ![CDN yönetim portalı - Özel raporlar menüsü](./media/cdn-reports/cdn-custom-reports.png)

## <a name="edge-cnames-custom-report"></a>Kenar CNAMES özel raporu
Edge CNAMES özel raporu, özel rapor günlüğe kaydetmenin etkin olduğu kenar CNAM'ları için isabetler ve veri aktarılan istatistikler sağlar. Kenar KNAM'ları Azure CDN uç nokta ana bilgisayar adlarından ve ilişkili özel alan ana bilgisayar adlarından oluşur. 

Özel rapor veri günlüğe kaydetme, bir kenar CNAME'nin özel raporlama özelliğini etkinleştirdikten bir saat sonra başlar. Belirli bir platform veya tüm platformlar için edge CNAMEs raporu oluşturarak rapor verilerini görüntüleyebilirsiniz. Bu raporun kapsamı, belirtilen süre içinde özel rapor verilerinin toplandığı kenar KNAM'ları ile sınırlıdır. Kenar KNAMEs raporu, Ölçümler seçeneğinde tanımlanan metrike göre en üstteki 10 kenar KNAM'ı için bir grafik ve veri tablosundan oluşur. 

Aşağıdaki rapor seçeneklerini tanımlayarak özel bir rapor oluşturun:

- Ölçümler: Aşağıdaki seçenekler desteklenir:

   - Isabetler: Özel raporlama özelliğinin etkin olduğu bir kenar CNAME'ye yönlendirilen toplam istek sayısını gösterir. Bu ölçüm, istemciye döndürülen durum kodunu içermez.

   - Veri Aktarılan: Özel raporlama özelliğinin etkin olduğu bir kenar CNAME'ye yönlendirilen istekler için kenar sunucularından HTTP istemcilerine (örneğin, web tarayıcıları) aktarılan toplam veri miktarını gösterir. Aktarılan veri miktarı, yanıt gövdesine HTTP yanıt üstbilgileri eklenerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarı gerçek dosya boyutundan büyüktür.

- Gruplandırmalar: Çubuk grafiğin altında gösterilen istatistik türünü belirler. Aşağıdaki seçenekler desteklenir:

   - HTTP Yanıt Kodları: İstemciye döndürülen HTTP yanıt koduna göre istatistikleri düzenler (örneğin, 200, 403, vb.). 

   - Önbellek Durumu: Önbellek durumuna göre istatistikleri düzenler.


Raporun tarih aralığını ayarlamak için, açılan listeden **Bugün** veya **Bu Hafta**gibi önceden tanımlanmış bir tarih aralığı seçebilir veya Takvim simgelerini tıklatarak **Özel'i** seçip bir tarih aralığını el ile girebilirsiniz. 

Tarih aralığını seçtikten sonra, raporu oluşturmak için **Git'i** tıklatın.

**Go** düğmesinin sağındaki Excel simgesini tıklatarak verileri Excel biçiminde dışa aktarabilirsiniz.

![CNAMEs raporu](./media/cdn-reports/cdn-cnames-report.png)

## <a name="edge-cnames-custom-report-fields"></a>Kenar CNAMES özel rapor alanları

| Alan                     | Açıklama   |
|---------------------------|---------------|
| 2xx                       | 2xx HTTP durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, 200 Ok). |
| 3xx                       | 3xx HTTP durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, 302 Bulundu veya 304 Değiştirilmemiş). |
| 4xx                       | 4xx HTTP durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, 400 Kötü İstek, 403 Yasak veya 404 Bulunamadı). |
| 5xx                       | 5xx HTTP durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, 500 Dahili Sunucu Hatası veya 502 Kötü Ağ Geçidi). |
| Önbellek Isabet %               | Doğrudan önbellekten istekte bulundurucuya sunulan önbellek isteklerinin yüzdesini gösterir. |
| Önbellek Hits                | Önbellek çarpmasıyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, TCP_EXPIRED_HIT, TCP_HIT veya TCP_PARTIAL_HIT). Önbelleğe alınmış bir içerik bulunduğunda önbellek isabeti oluşur. |
| Aktarılan Veriler (MB)     | Kenar sunucularından, kenar CNAME için HTTP istemcilerine (web tarayıcıları) aktarılan toplam veri miktarını (MB) gösterir. Aktarılan veri miktarı, yanıt gövdesine HTTP yanıt üstbilgileri eklenerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarı gerçek dosya boyutundan büyüktür. |
| Açıklama               | Bir kenar CNAME'yi ana bilgisayarı adına göre tanımlar |
| İsabetler                      | Kenar CNAME'deki toplam istek sayısını gösterir |
| Özlü -yor                    | Önbellek kaçırmayla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir (örneğin, TCP_CLIENT_REFRESH_MISS, TCP_EXPIRED_MISS veya TCP_MISS). İstenilen içerik isteği onurlandıran kenar sunucusunda önbelleğe alınmadığında önbellek kalıbı oluşur. | 
| Önbellek Yok                  | CONFIG_NOCACHE önbellek durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir.  |
| Diğer                     | CNAME kenarı için aktarılan toplam istek veya veri sayısını (MB) gösterir ve 2xx - 5xx aralığının dışına düşen bir HTTP durum koduyla sonuçlanır. |
| Platform                  | Kenar CNAME trafiğini işleyen platformu gösterir. |
| Atanmamış               | Önbellek durum kodu veya HTTP durum kodu bilgilerinin günlüğe kaydolmadığı kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir.  |
| Önbelleğe yok               | Önseze edilemeyen önbellek durum koduyla sonuçlanan kenar CNAME için aktarılan toplam istek veya veri sayısını (MB) gösterir.  |


## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporlar yalnızca son 18 ay içinde oluşturulabilir.

