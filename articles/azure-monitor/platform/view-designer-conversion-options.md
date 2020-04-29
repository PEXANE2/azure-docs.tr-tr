---
title: Azure Izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme seçenekleri
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 7bfa831332451718c0c9c05023b90104d2b8b02b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77658719"
---
# <a name="azure-monitor-view-designer-to-workbooks-conversion-options"></a>Azure Izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme seçenekleri
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve ek işlevsellik sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, görünümleri çalışma kitaplarına dönüştürmek için iki ve seçenek arasındaki temel kavramlar karşılaştırılır.

## <a name="basic-workbook-designs"></a>Temel çalışma kitabı tasarımları

Görünüm Tasarımcısı sabit bir temsil stiline sahiptir ve çalışma kitapları, verilerin nasıl temsil edileceğini ve verilerin nasıl gösterileceğini değiştirebilir. Aşağıdaki görüntüler, görünümleri dönüştürürken çalışma kitaplarını nasıl düzenlediğiniz hakkında iki örnek oluşturur.

[Dikey çalışma kitabı](view-designer-conversion-examples.md#vertical)
![dikey](media/view-designer-conversion-options/view-designer-vertical.png)

[Sekmeli çalışma kitabı](view-designer-conversion-examples.md#tabbed)
![veri türü dağıtım](media/view-designer-conversion-options/distribution-tab.png)
![sekmesi zaman içinde veri türleri sekmesi](media/view-designer-conversion-options/over-time-tab.png)

## <a name="tile-conversion"></a>Kutucuk dönüştürmesi
Görünüm Tasarımcısı genel durumu göstermek ve özetlemek için genel bakış kutucuğu özelliğini kullanır. Bunlar, sayılardan ve grafiklerin arasında değişen yedi kutucukla temsil edilir. Kullanıcılar, çalışma kitaplarında benzer görselleştirmeler oluşturabilir ve bunları genel bakış kutucuklarının özgün stiline benzeecek şekilde sabitleyebilir. 

![Galeri](media/view-designer-conversion-options/overview.png)


## <a name="view-dashboard-conversion"></a>Pano dönüştürmeyi görüntüle
Görünüm Tasarımcısı kutucukları genellikle iki bölümden oluşur; örneğin **halka & liste** kutucuğu gibi görselleştirmedeki verilerle eşleşen bir görselleştirme ve bir liste.

![Halka](media/view-designer-conversion-options/donut-example.png)

Çalışma kitaplarında, kullanıcının görünümün bir kısmını veya her ikisini birden sorgulamasına izin veririz. Çalışma kitaplarındaki sorguları formülleyici basit bir iki adımlı işlemdir. İlk olarak, veriler sorgudan oluşturulur ve ikincisi bir görselleştirme olarak işlenir.  Çalışma kitaplarında bu görünümün nasıl yeniden oluşturulacağına ilişkin bir örnek aşağıdaki gibidir:

![Dönüştür](media/view-designer-conversion-options/convert-donut.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Çalışma kitaplarına erişme & izinleri](view-designer-conversion-access.md)