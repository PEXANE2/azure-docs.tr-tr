---
title: Azure Stream Analytics Pencereleme işlevlerine giriş
description: Bu makalede Azure Stream Analytics işlerinde kullanılan dört Pencereleme işlevi (atlayan, atlamalı, kayan, oturum) açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 872eec62e7a629d76533aa6c9906cbdb64c32236
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80745549"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Stream Analytics Pencereleme işlevlerine giriş

Zaman akışı senaryolarında, zamana bağlı Windows 'da bulunan veriler üzerinde işlem yapma ortak bir modeldir. Stream Analytics,, geliştiricilerin en düşük çabayla karmaşık akış işleme işleri yazmalarını sağlayan, Pencereleme işlevleri için yerel destek içerir.

Arasından seçim yapabileceğiniz dört tür zamana bağlı pencere [**vardır: atlayan**](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics), [**atlamalı**](https://docs.microsoft.com/stream-analytics-query/hopping-window-azure-stream-analytics), [**kayan**](https://docs.microsoft.com/stream-analytics-query/sliding-window-azure-stream-analytics)ve [**oturum**](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics) pencereleri.  Stream Analytics işlerinizde sorgu sözdiziminin [**Group By**](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) yan tümcesinde pencere işlevlerini kullanırsınız. [ **Windows ()** işlevini](https://docs.microsoft.com/stream-analytics-query/windows-azure-stream-analytics)kullanarak olayları birden çok pencere üzerinde de toplayabilirsiniz.

Tüm [Pencereleme](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) işlemleri çıkış, pencerenin **sonunda** oluşur. Pencerenin çıktısı, kullanılan toplama işlevine göre tek olay olacaktır. Çıkış olayı pencerenin sonundaki zaman damgasına sahip olur ve tüm pencere işlevleri sabit bir uzunluğa göre tanımlanır. 

![Stream Analytics pencere işlevleri kavramları](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Atlayan pencere
Atlayan pencere işlevleri, bir veri akışını ayrı zaman kesimlerine bölmek ve bunlara karşı aşağıdaki örnekte olduğu gibi bir işlev gerçekleştirmek için kullanılır. Atlayan pencerenin farkı tekrarlaması, çakışmaması ve bir olayın birden fazla atlayan pencereye ait olamamasıdır.

![Stream Analytics atlayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hopping penceresi
Atlamalı pencere işlevleri, sabit bir süre kadar ileri gider. Bunlar çakışan Atlamalı pencereler olarak düşünülebilir ve olaylar, birden fazla Atlayan pencere sonuç kümesine ait olabilir. Bir atlamalı pencereyi, atlayan bir pencereyle aynı yapmak için, pencere boyutuyla aynı olacak atlama boyutunu belirtin. 

![Stream Analytics atlamalı penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Kayan pencere
Kayan pencere işlevleri, ttan veya atlamalı pencerelerin aksine, **yalnızca** bir olay gerçekleştiğinde çıkış üretir. Her pencerede en az bir olay olur ve pencere sürekli olarak bir ε (Epsilon) ile ileriye doğru gider. Atlamalı pencereler gibi olaylar birden fazla kayan pencereye ait olabilir.

![Stream Analytics kayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>Oturum penceresi
Oturum penceresi işlevleri, benzer zamanlarda gelen olayları, verilerin olmadığı zaman ölçeğini filtreleyerek gruplar. Üç ana parametreye sahiptir: zaman aşımı, maksimum süre ve bölümleme anahtarı (isteğe bağlı).

![Stream Analytics oturum penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

İlk olay gerçekleştiğinde oturum penceresi başlar. Son alınan olaydan belirtilen zaman aşımı süresi içinde başka bir olay oluşursa, pencere yeni olayı içerecek şekilde genişletilir. Aksi takdirde, zaman aşımı içinde herhangi bir olay gerçekleşemez, pencere zaman aşımından sonra kapatılır.

Olaylar belirtilen zaman aşımı süresi içinde gerçekleşirken, en fazla süreye ulaşılana kadar oturum penceresi Genişlemeden kalır. En uzun süre denetim aralıkları, belirtilen maksimum süre ile aynı boyut olarak ayarlanır. Örneğin, en fazla süre 10 ise, pencerenin en uzun süreyi aşması durumunda üzerindeki denetimler t = 0, 10, 20, 30 vb. olur.

Bir bölüm anahtarı sağlandığında, olaylar anahtar tarafından birlikte gruplandırılır ve oturum penceresi her bir gruba bağımsız olarak uygulanır. Bu bölümlendirme, farklı kullanıcılar veya cihazlar için farklı oturum pencerelerinin olması gereken durumlarda faydalıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

