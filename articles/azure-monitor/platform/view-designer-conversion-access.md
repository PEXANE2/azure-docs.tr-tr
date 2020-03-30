---
title: Çalışma kitapları dönüşüm özeti ve erişim için Azure Monitor görünüm tasarımcısı
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: fb53a966b0dc4959253ac0786ef09ef3c497e809
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658855"
---
# <a name="view-designer-to-workbooks-conversion-summary-and-access"></a>Tasarımcıyı çalışma kitapları dönüşüm özetine ve erişime görüntüleme
[Görünüm tasarımcısı,](view-designer.md) Azure Monitor'un, Log Analytics çalışma alanınızdaki grafikler, listeler ve zaman çizelgeleriyle verileri görselleştirmenize yardımcı olacak özel görünümler oluşturmanıza olanak tanıyan bir özelliğidir. Bunlar aşamalı olarak devre dışı ediliyor ve ek işlevsellik sağlayan çalışma kitaplarıyla değiştiriliyor. Bu makalede, çalışma kitaplarına erişmek için gereken genel bakış özetini ve izinleri nasıl oluşturabileceğiniz ayrıntılı olarak açıklanmıştır.

## <a name="creating-your-workspace-summary-from-azure-dashboard"></a>Azure Panosundan Çalışma Alanı Özetinizi Oluşturma
Görünüm tasarımcısı kullanıcılar, bir görünüm kümesini temsil edecek genel bir döşemeye sahip olmaya aşina olabilir. Görünüm tasarımcısı çalışma alanı özeti gibi görsel bir genel bakış sağlamak için, çalışma kitapları [Azure portal panonuza](../../azure-portal/azure-portal-dashboards.md)sabitlenebilecek sabitlenmiş adımlar sunar. Çalışma Alanı özetindeki genel bakış kutucukları gibi, sabitlenmiş çalışma kitabı öğeleri de doğrudan çalışma kitabı görünümüne bağlanır.

Sabitlenmiş öğeleriniz ve görselleştirmeleriniz için otomatik yenileme, taşıma, boyutlandırma ve ek filtreleme sağlayan Azure panolarıyla sağlanan yüksek düzeydeözelleştirme özelliklerinden yararlanabilirsiniz. 

![Pano](media/view-designer-conversion-access/dashboard.png)

Yeni bir Azure panosu oluşturun veya çalışma kitabı öğelerini sabitlemeye başlamak için varolan bir pano seçin.

Tek tek öğeyi sabitlemek için, belirli bir adımın pin simgesini etkinleştirmeniz gerekir. Bunu yapmak için, adımınız için ilgili **Edit** düğmesini seçin ve ardından **Gelişmiş Ayarlar'ı**açmak için vites simgesini seçin. **Bu adımda her zaman pin simgesini gösterme**seçeneğini işaretleyin ve adımınızın sağ üst köşesinde bir pin simgesi görünür. Bu pin, genel bakış döşemeleri gibi belirli görselleri panonuza sabitlemenizi sağlar.

![Adımı sabitle](media/view-designer-conversion-access/pin-step.png)


Çalışma Kitabı'ndan veya Çalışma Kitabı içeriğinin tamamından bir panoya birden çok görseli sabitlemek isteyebilirsiniz. Çalışma kitabının tamamını sabitlemek için, Düzenleme Modu'nu geçiş yapmak için üst araç çubuğunda **Edit'i** seçin. **Edit Mode** Çalışma kitabı öğesinin tamamını veya çalışma kitabındaki tüm tek adımlar ve görselöğeleri sabitlemenize olanak tanıyan bir pin simgesi görüntülenir.

![Tümlerini sabitle](media/view-designer-conversion-access/pin-all.png)



## <a name="sharing-and-viewing-permissions"></a>İzinleri Paylaşma ve Görüntüleme 
Çalışma kitapları, özel veya paylaşılan bir belge olmanın avantajına sahiptir. Varsayılan olarak, kaydedilen çalışma kitapları **Raporlarım**altında kaydedilir, yani bu çalışma kitabını yalnızca oluşturucu görüntüleyebilir.

**Düzenleme Modundayken**üst araç çubuğundan **Paylaş** simgesini seçerek çalışma kitaplarınızı paylaşabilirsiniz. Çalışma kitabınızı **Paylaşılan Raporlar'a**taşımanız istenir ve bu da çalışma kitabına doğrudan erişim sağlayan bir bağlantı oluşturur.

Bir kullanıcının paylaşılan bir çalışma kitabını görüntüleyebilmesi için, çalışma kitabının altında kayıtlı olduğu abonelik ve kaynak grubuna erişimi olması gerekir.

![Abonelik tabanlı erişim](media/view-designer-conversion-access/subscription-access.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Genel görevler](view-designer-conversion-tasks.md)