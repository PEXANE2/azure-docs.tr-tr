---
title: Verizon 'dan çekirdek raporlar | Microsoft Docs
description: Verizon profilleri için yönetim portalı aracılığıyla Verizon çekirdek raporlarına erişme ve görüntüleme hakkında bilgi edinin.
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
ms.topic: how-to
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5353fa70be4705b28dab9350fcdf7819b22faf54
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985538"
---
# <a name="core-reports-from-verizon"></a>Verizon Çekirdek Raporları

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Verizon profilleri için portalı Yönet aracılığıyla Verizon Core raporları kullanarak, CDN 'niz için kullanım düzenlerini aşağıdaki raporlarla görüntüleyebilirsiniz:

* Bant genişliği
* Aktarılan veriler
* İsabetler
* Önbellek durumları
* Önbellek İsabet Oranı
* Aktarılan ıPV4/ıPV6 verileri

## <a name="accessing-verizon-core-reports"></a>Verizon Core raporlarına erişme
1. CDN profili dikey penceresinde **Yönet** düğmesine tıklayın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analiz** sekmesinin üzerine gelin ve ardından **çekirdek raporlar** açılır öğesi üzerine gelin. Menüdeki bir rapora tıklayın.
   
    ![CDN yönetim portalı-çekirdek raporlar menüsü](./media/cdn-reports/cdn-core-reports.png)

3. Her rapor için, **Tarih aralığı** listesinden bir tarih aralığı seçin. **Bugün** veya **Bu hafta**gibi önceden tanımlanmış bir tarih aralığı seçebilir ya da **özel** ' i seçip takvim simgeleri ' ne tıklayarak bir tarih aralığını el ile girebilirsiniz. 

4. Bir tarih aralığı seçtikten sonra, raporu oluşturmak için **Git** ' e tıklayın. 

4. Excel biçimindeki verileri dışarı aktarmak istiyorsanız, **Git** düğmesinin üzerindeki Excel simgesine tıklayın.

## <a name="bandwidth"></a>Bant genişliği
Bant genişliği raporu, HTTP ve HTTPS için CDN bant genişliği kullanımını, MB/sn cinsinden belirli bir süre boyunca belirten bir grafik ve veri tablosundan oluşur. Bant genişliği kullanımını tüm pop 'Lar veya belirli bir POP üzerinde görüntüleyebilirsiniz. Bu rapor, pop 'Ların trafik artışlarını ve dağıtımını görüntülemenizi sağlar.

Tüm düğümlerdeki trafiği görmek için **kenar düğümleri** listesinden **Tüm kenar düğümleri** ' ni seçin veya belirli bir bölgeyi seçin.

Rapor her beş dakikada bir güncelleştirilir.

![Bant genişliği raporu](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Aktarılan veriler
Bu rapor, belirli bir süre boyunca (GB olarak) HTTP ve HTTPS için CDN trafik kullanımını gösteren bir Graf ve veri tablosundan oluşur. Trafik kullanımını tüm pop 'Lar veya belirli bir POP üzerinde görüntüleyebilirsiniz. Bu rapor, pop 'larda trafik artışlarını ve dağıtımı görüntülemenize olanak sağlar.

Tüm düğümlerdeki trafiği görmek için **kenar düğümleri** listesinden **Tüm kenar düğümleri** ' ni seçin veya belirli bir bölgeyi seçin.

Rapor her beş dakikada bir güncelleştirilir.

![Aktarılan veri raporu](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>İsabetler (durum kodları)
Bu rapor, içeriğiniz için istek durum kodlarının dağıtımını açıklar. Her içerik isteği bir HTTP durum kodu üretir. Durum kodu, uç pop 'Ların isteği nasıl işlendiğini açıklar. Örneğin, 2xx durum kodu, isteğin bir istemciye başarıyla sunulduğunu belirtir, ancak 4xx durum kodu bir hata oluştuğunu gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz. [http durum kodları listesi](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

![İsabet raporu](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Önbellek durumları
Bu rapor, istemci istekleri için önbellek isabetlerinin ve önbellek isabetlerinin dağıtımını açıklar. Önbellek isabetlerinin en hızlı performans sonuçları nedeniyle, veri teslimi hızlarını en iyi hale getirerek önbellek isabetsizliği ve süre dolma isabetli önbellek isabetlerinin en aza 

Önbellek isabetsizliği azaltmak için, aşağıdakilerin kullanımını en aza indirmek üzere kaynak sunucunuzu yapılandırın: 
 * `no-cache` Yanıt üst bilgileri
 * Query-String Caching, kesinlikle gerekmedikçe  
 * Önbelleklenmemiş yanıt kodları

Süresi geçen önbellek isabetlerinin azaltmak için bir varlığın, `max-age` kaynak sunucuya yönelik istek sayısını en aza indirmek üzere bir süreyi uzun bir süreye ayarlayın.

![Önbellek durumları raporu](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Ana önbellek durumları şunlardır:
* TCP_HIT: Edge sunucusundan sunulur. Nesne önbellekte idi ve en fazla geçerlilik süresini aşmadı.
* TCP_MISS: kaynak sunucudan sunulur. Nesne önbellekte değildi ve yanıt kaynağa geri döndürüldü.
* TCP_EXPIRED _MISS: kaynak sunucudan yeniden doğrulamadan sonra kaynak sunucudan sunulur. Nesne önbellekte idi, ancak en fazla geçerlilik süresini aşmıştı. Kaynak ile yeniden doğrulama, önbellekten yeni bir Yanıt ile değiştirilen önbellek nesnesi ile sonuçlandı.
* TCP_EXPIRED _HIT: kaynak ile yeniden doğrulamadan sonra kenarından sunulan. Nesne önbellekte idi, ancak en fazla geçerlilik süresini aşmıştı. Kaynak sunucuyla yeniden doğrulama, önbellek nesnesinin değiştirilmemiş olarak sonuçlandı.

### <a name="full-list-of-cache-statuses"></a>Önbellek durumlarının tam listesi
* TCP_HIT-bu durum, bir istek doğrudan POP 'tan istemciye sunulduğunda raporlanır. Bir varlık, bir POP 'ta istemciye en yakın zamanda önbelleğe alındığında ve geçerli yaşam süresi (TTL) olduğunda bir POP 'tan anında sunulur. TTL aşağıdaki yanıt üst bilgilerine göre belirlenir:
  
  * Cache-Control: s-maxage
  * Cache-Control: Max-Age
  * Süre Sonu
* TCP_MISS: Bu durum, istenen varlığın önbelleğe alınmış bir sürümünün istemciye en yakın POP üzerinde bulunamadığını gösterir. Varlık, bir kaynak sunucudan veya bir kaynak kalkan sunucusundan istendi. Kaynak sunucu veya kaynak kalkan sunucusu bir varlık döndürürse, istemciye sunulur ve hem istemcide hem de uç sunucuda önbelleğe alınır. Aksi takdirde, 200 olmayan bir durum kodu (örneğin, 403 yasaklanmış veya 404 bulunamadı) döndürülür.
* TCP_EXPIRED_HIT: Bu durum, süresi geçen bir TTL ile bir varlık hedefleyen bir istek doğrudan POP 'tan istemciye sunulduğunda raporlanır. Örneğin, varlığın en fazla kullanım süresi dolduğunda. 
  
   Vadesi geçen bir istek, genellikle kaynak sunucuya yeniden doğrulama isteğine neden olur. TCP_EXPIRED_HIT durumun gerçekleşmesi için, kaynak sunucu, varlığın daha yeni bir sürümünün mevcut olmadığını belirtmelidir. Bu durum genellikle varlığın Cache-Control ve Expires üst bilgilerinin güncelleştirilmesine neden olur.
* TCP_EXPIRED_MISS: Bu durum, ön belleğe alınmış bir varlığın daha yeni bir sürümü POP 'tan istemciye sunulduğunda raporlanır. Bu durum, önbelleğe alınmış bir varlık için TTL süresinin dolması durumunda (örneğin, süresi dolum ömrü dolmuşsa) ve kaynak sunucu söz konusu varlığın daha yeni bir sürümünü döndürdüğünde oluşur. Varlığın bu yeni sürümü, önbelleğe alınmış sürüm yerine istemciye sunulur. Ayrıca, uç sunucu ve istemcide önbelleğe alınır.
* CONFIG_NOCACHE: Bu durum, bir müşteriye özgü yapılandırmanın Edge POP 'un önbelleğe alınmasını engellediğini gösterir.
* HIÇBIRI-bu durum, önbellek içerik yeniliği denetiminin gerçekleştirilmediğini belirtir.
* TCP_CLIENT_REFRESH_MISS: Bu durum, bir tarayıcı gibi bir HTTP istemcisi, bir uç POP 'u kaynak sunucudan eski bir varlığın yeni bir sürümünü almaya zorlarsa bildirilir. Varsayılan olarak, sunucular bir HTTP istemcisinin, kaynak sunucudan varlığın yeni bir sürümünü almasını engeller.
* TCP_PARTIAL_HIT: Bu durum, bir bayt aralığı isteği kısmen önbelleğe alınmış bir varlık için isabet ile sonuçlanırsa bildirilir. İstenen bayt aralığı, POP 'tan istemciye anında sunulur.
* Önbelleğe ALıNMAMıŞ: Bu durum, bir varlık `Cache-Control` ve `Expires` ÜSTBILGILER bir pop 'TA veya http istemcisi tarafından önbelleğe alınmamalıdır. Bu tür istekler, kaynak sunucudan sunulur.

## <a name="cache-hit-ratio"></a>Önbellek İsabet Oranı
Bu rapor, doğrudan önbellekten sunulan önbelleğe alınmış isteklerin yüzdesini gösterir.

Rapor aşağıdaki bilgileri sağlar:

* İstenen içerik, istek sahibine en yakın POP üzerinde önbelleğe alındı.
* İstek, doğrudan ağımızın kenarından sunulur.
* İsteğin kaynak sunucu ile yeniden doğrulanması gerekmiyor.

Rapor şunları içermez:

* Ülke/bölge filtreleme seçenekleri nedeniyle reddedilen istekler.
* Üst bilgileri önbelleğe alınmamalıdır olduğunu belirten varlıklar için istekler. Örneğin,, `Cache-Control: private` , `Cache-Control: no-cache` veya `Pragma: no-cache` üst bilgiler bir varlığın önbelleğe alınmasını önler.
* Kısmen önbelleğe alınmış içerik için bayt aralığı istekleri.

Formül: (TCP_ Isabet/(TCP_ Isabet + TCP_MISS)) * 100

![Önbellek isabet oranı raporu](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Aktarılan ıPV4/ıPV6 verileri
Bu rapor, ıPV4 vs ıPV6 'daki trafik kullanım dağıtımını gösterir.

![Aktarılan ıPV4/ıPV6 verileri](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporlar yalnızca son 18 ay içinde oluşturulabilir.

