---
title: Çalışma kitabı dönüştürme özeti ve erişim için Azure Izleyici Görünüm Tasarımcısı
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 7614dd1d60bad5c124269ae2af02d30a5aacfe3e
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564041"
---
# <a name="view-designer-to-workbooks-conversion-summary-and-access"></a>Çalışma kitabı dönüştürme özeti ve erişim için tasarımcıyı görüntüleyin
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve başka işlevler sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, çalışma kitaplarına erişmek için gereken bir genel bakış Özeti ve izinleri nasıl oluşturabileceğiniz açıklanır.

## <a name="creating-your-workspace-summary-from-azure-dashboard"></a>Azure panosundan çalışma alanı özetinizi oluşturma
Görünüm Tasarımcısı kullanıcıları, bir görünüm kümesini temsil eden bir genel bakış kutucuğuna sahip olmaya alışmış olabilir. Görünüm Tasarımcısı çalışma alanı Özeti gibi görsel bir genel bakışı sürdürmek için çalışma kitapları, [Azure Portal panonuza](../../azure-portal/azure-portal-dashboards.md)sabitlenebilir sabitlenmiş adımlar sunar. Çalışma alanı özetindeki genel bakış kutucuklarına benzer şekilde, sabitlenmiş çalışma kitabı öğeleri doğrudan çalışma kitabı görünümüne bağlanır.

Azure panolarıyla birlikte sunulan, sabitlenmiş öğeleriniz ve Görselleştirmeleriniz için otomatik yenileme, taşıma, boyutlandırma ve ek filtrelemeye olanak sağlayan yüksek düzeyde özelleştirme özelliklerinden yararlanabilirsiniz. 

![Ekran görüntüsü, çalışma alanı Özeti başlıklı özelleştirilmiş bir Azure panosunu gösterir.](media/view-designer-conversion-access/dashboard.png)

Çalışma kitabı öğelerini sabitlemeye başlamak için yeni bir Azure panosu oluşturun veya var olan bir panoyu seçin.

Tek bir öğeyi sabitlemek için, belirli adımınız için PIN simgesini etkinleştirmeniz gerekir. Bunu yapmak için adımlarınız için ilgili **düzenleme** düğmesini seçin, ardından **Gelişmiş ayarları**açmak için dişli simgesini seçin. **Bu adımda PIN simgesini her zaman gösterme**seçeneğini işaretleyin ve adımlarınızın sağ üst köşesinde bir pin simgesi görünür. Bu pin, özel görselleştirmeleri panonuza, genel bakış kutucukları gibi sabitlemenize olanak sağlar.

![PIN adımı](media/view-designer-conversion-access/pin-step.png)


Ayrıca, çalışma kitabından veya tüm çalışma kitabı içeriğinin bir panoya birden fazla görselleştirmeyi sabitlemek isteyebilirsiniz. Çalışma kitabının tamamını sabitlemek için, **düzenleme modunu**değiştirmek üzere üstteki araç çubuğunda **Düzenle** ' yi seçin. Çalışma kitabı öğesinin tamamını veya çalışma kitabındaki tüm adımları ve görselleştirmeleri sabitlemenize olanak tanıyan bir PIN simgesi görüntülenir.

![Tümünü sabitle](media/view-designer-conversion-access/pin-all.png)



## <a name="sharing-and-viewing-permissions"></a>Paylaşım ve görüntüleme Izinleri 
Çalışma kitapları, özel veya paylaşılan bir belge olmasının avantajına sahiptir. Varsayılan olarak, kaydedilen çalışma kitapları **Raporlarım**altına kaydedilir, yani yalnızca oluşturucunun bu çalışma kitabını görüntüleyebileceği anlamına gelir.

Çalışma kitaplarınızı, **düzenleme modundayken**üstteki araç çubuğundan **paylaşma** simgesini seçerek paylaşabilirsiniz. Çalışma kitabınızı **paylaşılan raporlara**taşımanız istenir ve bu, çalışma kitabına doğrudan erişim sağlayan bir bağlantı oluşturur.

Bir kullanıcının paylaşılan bir çalışma kitabını görüntülemesi için, çalışma kitabının kaydedildiği abonelik ve kaynak grubuna her ikisi de erişebilmelidir.

![Abonelik tabanlı erişim](media/view-designer-conversion-access/subscription-access.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Genel görevler](view-designer-conversion-tasks.md)