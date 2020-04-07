---
title: Azure Akışı Analizi pencere işlevlerine giriş
description: Bu makalede, Azure Akış Analizi işlerinde kullanılan dört pencereleme işlevi (yuvarlanma, atlamalı, kaydırma, oturum) açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 872eec62e7a629d76533aa6c9906cbdb64c32236
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745549"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Akış Analizi pencere işlevlerine giriş

Zaman akışı senaryolarında, zamansal pencerelerde bulunan veriler üzerinde işlem yapmak yaygın bir dedüzendir. Akış Analizi, geliştiricilerin en az çabayla karmaşık akış işleme işleri yazmalarını sağlayan pencerelendirme işlevleri için yerel desteğe sahiptir.

Seçim için zamansal pencereler dört türü vardır: [**Yuvarlanma**](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics), [**Atlamalı**](https://docs.microsoft.com/stream-analytics-query/hopping-window-azure-stream-analytics), [**Sürgülü**](https://docs.microsoft.com/stream-analytics-query/sliding-window-azure-stream-analytics), ve [**Oturum**](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics) pencereler.  Akış Analizi işlerinizde sorgu sözdiziminin [**GROUP BY**](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) yan tümcesindeki pencere işlevlerini kullanırsınız. Ayrıca, [ **Windows()** işlevini](https://docs.microsoft.com/stream-analytics-query/windows-azure-stream-analytics)kullanarak birden çok pencere üzerinde olayları toplayabilirsiniz.

Pencerenin **sonundaki** tüm [pencere işlemleri](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) çıktısı sonuçları. Pencerenin çıktısı, kullanılan toplam işlevi temel alan tek bir olay olacaktır. Çıktı olayı pencerenin sonundaki zaman damgasına sahip olur ve tüm pencere işlevleri sabit bir uzunlukta tanımlanır. 

![Akış Analizi penceresi işlevleri kavramları](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Yuvarlanan pencere
Yuvarlanan pencere işlevleri, veri akışını farklı zaman dilimlerine ayırmak ve aşağıdaki örnekte olduğu gibi bunlara karşı bir işlev gerçekleştirmek için kullanılır. Atlayan pencerenin farkı tekrarlaması, çakışmaması ve bir olayın birden fazla atlayan pencereye ait olamamasıdır.

![Akış Analizi yuvarlanma penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Atlamalı pencere
Atlamalı pencere işlevleri, sabit bir süre kadar ileri gider. Bunlar çakışan Atlamalı pencereler olarak düşünülebilir ve olaylar, birden fazla Atlayan pencere sonuç kümesine ait olabilir. Atlamalı pencereyi Yuvarlanan pencereyle aynı yapmak için atlama boyutunupencere boyutuyla aynı olarak belirtin. 

![Akış Analizi atlamalı penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Sürgülü pencere
Yuvarlanan veya Zıplayan pencerelerin aksine kayan pencere işlevleri yalnızca bir olay **oluştuğunda** çıktı üretir. Her pencerede en az bir olay olur ve pencere sürekli olarak bir ε (epsilon) ile ilerler. Atlamalı pencereler gibi olaylar birden fazla kayan pencereye ait olabilir.

![Stream Analytics kayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>Oturum penceresi
Oturum penceresi, veri olmayan zaman dönemlerini filtreleyerek, benzer zamanlarda gelen olayları gruplandırır. Üç ana parametreye sahiptir: zaman aşımı, maksimum süre ve bölümleme anahtarı (isteğe bağlı).

![Akış Analizi oturum penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

İlk olay oluştuğunda oturum penceresi başlar. Başka bir olay, son alınan olaydan itibaren belirtilen zaman dışında oluşursa, pencere yeni olayı içerecek şekilde genişletilir. Aksi takdirde zaman anına herhangi bir olay meydana gelmezse, pencere zaman anına kapatılır.

Olaylar belirtilen zaman anına kadar oluşmaya devam ederse, oturum penceresi maksimum süreulaşılıncaya kadar uzatılmaya devam edecektir. Maksimum süre kontrol aralıkları belirtilen maksimum süreyle aynı boyutta olarak ayarlanır. Örneğin, maksimum süre 10 ise, pencere nin maksimum süreyi aşması durumunda t = 0, 10, 20, 30, vb. adresinde niçin gerçekleşeceğini kontrol eder.

Bir bölüm anahtarı sağlandığında, olaylar anahtarla birlikte gruplandırılır ve her gruba bağımsız olarak oturum penceresi uygulanır. Bu bölümleme, farklı kullanıcılar veya aygıtlar için farklı oturum pencerelerini ihtiyaç duyduğunuz durumlar için yararlıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

