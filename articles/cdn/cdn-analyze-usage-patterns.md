---
title: Verizon Gelen Çekirdek Raporlar | Microsoft Dokümanlar
description: "Aşağıdaki raporları kullanarak CDN'nizin kullanım desenlerini görüntüleyebilirsiniz: Bant Genişliği, Veri Aktarımı, Hits, Önbellek Durumları, Önbellek Isabet Oranı, IPV4/IPV6 Veri Aktarımı."
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d48ddafdc1ec30ae1533b3a3101582f33e7f4b5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67594153"
---
# <a name="core-reports-from-verizon"></a>Verizon Gelen Çekirdek Raporlar

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Verizon profilleri için Yönet portalı üzerinden Verizon Çekirdek Raporları kullanarak, aşağıdaki raporlar ile CDN için kullanım desenleri görüntüleyebilirsiniz:

* Bant genişliği
* Aktarılan Veriler
* İsabetler
* Önbellek Durumları
* Önbellek Isabet Oranı
* IPV4/IPV6 Veri Aktarıldı

## <a name="accessing-verizon-core-reports"></a>Verizon Core Raporlara Erişim
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analytics** sekmesinin üzerine binin, ardından **Core Reports** uçuşunun üzerinde gezinin. Menüdeki bir rapora tıklayın.
   
    ![CDN yönetim portalı - Çekirdek Raporlar menüsü](./media/cdn-reports/cdn-core-reports.png)

3. Her rapor **için, Tarih Aralığı** listesinden bir tarih aralığı seçin. **Bugün** veya **Bu Hafta**gibi önceden tanımlanmış bir tarih aralığı seçebilir veya Takvim simgelerini tıklatarak **Özel'i** seçip bir tarih aralığını el ile girebilirsiniz. 

4. Bir tarih aralığı seçtikten sonra, raporu oluşturmak için **Git'i** tıklatın. 

4. Verileri Excel biçiminde dışa aktarmak istiyorsanız, **Git** düğmesinin üzerindeki Excel simgesini tıklatın.

## <a name="bandwidth"></a>Bant genişliği
Bant genişliği raporu, Http ve HTTPS için belirli bir zaman diliminde, Mbps'de CDN bant genişliği kullanımını gösteren bir grafik ve veri tablosundan oluşur. Bant genişliği kullanımını tüm POP'lar arasında veya belirli bir POP için görüntüleyebilirsiniz. Bu rapor, trap'ların trafik artışlarını ve dağıtımını görüntülemenizi sağlar.

Kenar **Düğümleri** listesinden, tüm düğümlerden trafiği görmek veya belirli bir bölge seçmek için **Tüm Kenar Düğümleri'ni** seçin.

Rapor her beş dakikada bir güncellenir.

![Bant genişliği raporu](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Aktarılan veriler
Bu rapor, BELIRLI bir zaman diliminde HTTP ve HTTPS için GB'deki CDN trafik kullanımını gösteren bir grafik ve veri tablosundan oluşur. Tüm POP'lar veya belirli bir POP için trafik kullanımını görüntüleyebilirsiniz. Bu rapor, traplar arasında trafik artışlarını ve dağıtımını görüntülemenizi sağlar.

Kenar **Düğümleri** listesinden, tüm düğümlerden trafiği görmek veya belirli bir bölge seçmek için **Tüm Kenar Düğümleri'ni** seçin.

Rapor her beş dakikada bir güncellenir.

![Aktarılan veri raporu](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Isabetler (durum kodları)
Bu rapor, içeriğiniz için istek durum kodlarının dağılımını açıklar. İçerik için her istek bir HTTP durum kodu oluşturur. Durum kodu, kenar Lı POP'ların isteği nasıl ele adedinde olduğunu açıklar. Örneğin, 2xx durum kodu isteğin bir istemciye başarıyla sunulduğunu gösterirken, 4xx durum kodu bir hata oluştuğunu gösterir. HTTP durum kodları hakkında daha fazla bilgi [için, http durum kodları listesine](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)bakın.

![Hits raporu](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Önbellek durumları
Bu rapor, istemci istekleri için önbellek isabetleri ve önbellek özleri dağıtımını açıklar. Önbellek isabetlerinden en hızlı performans sonuçları elde ettiğinden, önbellek eksikliklerini ve süresi dolmuş önbellek isabetlerini en aza indirerek veri teslim hızlarını en iyi duruma getirebilirsiniz. 

Önbellek özlemenlerini azaltmak için, aşağıdakilerin kullanımını en aza indirecek şekilde başlangıç sunucunuzu yapılandırın: 
 * `no-cache`yanıt üstbilgi
 * Sorgu dize önbelleğe alma, kesinlikle gerekli olmadıkça  
 * Önbellek dışı yanıt kodları

Süresi dolmuş önbellek isabetlerini azaltmak `max-age` için, kaynağı sunucudaki istek sayısını en aza indirmek için bir varlığınkini uzun bir süreye ayarlayın.

![Önbellek durumları raporu](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Ana önbellek durumları şunlardır:
* TCP_HIT: Kenar sunucusundan servis edilir. Nesne önbellekteydi ve maksimum yaşını aşmadı.
* TCP_MISS: Origin sunucusundan servis edilir. Nesne önbellekte değildi ve yanıt kaynağına geri döndü.
* TCP_EXPIRED _MISS: Orijinle yeniden validasyondan sonra origin sunucusundan servis edilir. Nesne önbellekteydi, ancak maksimum yaşını aşmıştı. Kaynakla yeniden doğrulama, önbellek nesnesinin kaynaktan gelen yeni bir yanıtla değiştirilmesiyle sonuçlandı.
* TCP_EXPIRED _HIT: Menşei ile revalidation sonra Edge servis edilir. Nesne önbellekteydi, ancak maksimum yaşını aşmıştı. Kaynak sunucusuyla yeniden doğrulama, önbellek nesnesinin değiştirilmeden sonuçlanmasına neden oldu.

### <a name="full-list-of-cache-statuses"></a>Önbellek durumlarının tam listesi
* TCP_HIT - Bir istek doğrudan POP'tan istemciye sunulduğunda bu durum bildirilir. Bir varlık, istemciye en yakın POP'ta önbelleğe alınıp geçerli bir zamana (TTL) sahip olduğunda hemen bir POP'tan sunulur. TTL aşağıdaki yanıt başlıkları tarafından belirlenir:
  
  * Önbellek Kontrolü: s-maxage
  * Önbellek Kontrolü: maksimum yaş
  * Süre Sonu
* TCP_MISS: Bu durum, istenen varlığın önbelleğe alınmış bir sürümünün istemciye en yakın POP'ta bulunmadığını gösterir. Varlık, bir başlangıç sunucusundan veya bir başlangıç kalkanı sunucusundan istenir. Kaynak sunucusu veya kaynak kalkanı sunucusu bir varlığı döndürürse, istemciye sunulur ve hem istemcide hem de kenar sunucusunda önbelleğe alınır. Aksi takdirde, 200 olmayan bir durum kodu (örneğin, 403 Yasak veya 404 Bulunamadı) döndürülür.
* TCP_EXPIRED_HIT: Süresi dolmuş TTL'si olan bir kıymeti hedefleyen bir istek doğrudan POP'tan istemciye sunulduğunda bu durum bildirilir. Örneğin, varlığın maksimum yaşının süresi dolduğunda. 
  
   Süresi dolan bir istek genellikle kaynak sunucuya bir yeniden doğrulama isteğiyle sonuçlanır. TCP_EXPIRED_HIT durumunun oluşması için kaynak sunucunun varlığın daha yeni bir sürümünün bulunmadığını göstermesi gerekir. Bu durum genellikle varlığın Önbellek Denetimi'nin güncelleştirmesi ile sonuçlanır ve üstbilgileri sona erer.
* TCP_EXPIRED_MISS: Süresi dolmuş önbelleğe alınmış bir varlığın daha yeni bir sürümü POP'tan istemciye sunulduğunda bu durum bildirilir. Bu durum, önbelleğe alınmış bir varlığın TTL'sinin süresi dolduğunda (örneğin, süresi dolmuş maksimum yaş) ve kaynak sunucusu bu varlığın daha yeni bir sürümünü döndürdüğünde oluşur. Varlığın bu yeni sürümü önbelleğe alınmış sürüm yerine istemciye sunulur. Ayrıca, kenar sunucusunda ve istemcide önbelleğe alınır.
* CONFIG_NOCACHE: Bu durum, müşteriye özgü bir yapılandırmanın POP kenarının varlığın önbelleğe alınmasını engellediğini gösterir.
* YOK - Bu durum, önbellek içeriği tazelik denetiminin yapılmadığını gösterir.
* TCP_CLIENT_REFRESH_MISS: Bu durum, tarayıcı gibi bir HTTP istemcisi bir kenar POP'u başlangıç sunucusundan eski bir varlığın yeni bir sürümünü almaya zorladığında bildirilir. Varsayılan olarak, sunucular bir HTTP istemcisinin kenar sunucularını kaynağı sunucudan varlığın yeni bir sürümünü almaya zorlanmasını engeller.
* TCP_PARTIAL_HIT: Bir bayt aralığı isteği kısmen önbelleğe alınmış bir varlık için bir isabetle sonuçlandığında bu durum bildirilir. İstenen bayt aralığı hemen POP'tan istemciye sunulur.
* ÖNSEMEZ: Bu durum, bir `Cache-Control` varlığın `Expires` ve üstbilginin bir POP'ta veya HTTP istemcisi tarafından önbelleğe alınmaması gerektiğini belirttiği nde bildirilir. Bu tür istekler kaynak sunucudan sunulur.

## <a name="cache-hit-ratio"></a>Önbellek Isabet Oranı
Bu rapor, önbellekten doğrudan sunulan önbelleğe alınmış isteklerin yüzdesini gösterir.

Rapor aşağıdaki ayrıntıları sağlar:

* İstenen içerik, istekte bulundurucuya en yakın POP'ta önbelleğe alındı.
* İstek doğrudan ağımızın kenarından sunuldu.
* İstek, kaynak sunucusuyla yeniden doğrulama gerektirmedi.

Rapor şunları içermez:

* Ülke/bölge filtreleme seçenekleri nedeniyle reddedilen istekler.
* Üstbilgileri önbelleğe alınmaması gerektiğini gösteren varlıklar için istekler. Örneğin, `Cache-Control: private`, `Cache-Control: no-cache`, `Pragma: no-cache` veya üstbilgi bir varlığın önbelleğe alınmasını engeller.
* Kısmen önbelleğe alınmış içerik için bayt aralığı istekleri.

Formül: (TCP_ HIT/(TCP_ HIT+TCP_MISS))*100

![Önbellek isabet oranı raporu](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPV4/IPV6 Aktarılan Veriler
Bu rapor, IPV4 vs IPV6'daki trafik kullanım dağılımını gösterir.

![IPV4/IPV6 Aktarılan Veriler](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporlar yalnızca son 18 ay içinde oluşturulabilir.

