---
title: Azure Izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme seçenekleri
description: ''
author: austonli
ms.author: aul
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: f04ebc1a4a53825709479ca3f1dc7ce1245fc67f
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171559"
---
# <a name="azure-monitor-view-designer-to-workbooks-conversion-options"></a>Azure Izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme seçenekleri
[Görünüm Tasarımcısı](view-designer.md) , Log Analytics çalışma alanınızdaki verileri grafiklerle, listelerle ve zaman çizelgeleriyle görselleştirmenize yardımcı olmak üzere özel görünümler oluşturmanıza olanak sağlayan bir Azure izleyici özelliğidir. Bunlar kullanıma alınır ve ek işlevsellik sağlayan çalışma kitapları ile değiştirilmiştir. Bu makalede, görünümleri çalışma kitaplarına dönüştürmek için iki ve seçenek arasındaki temel kavramlar karşılaştırılır.

## <a name="basic-workbook-designs"></a>Temel çalışma kitabı tasarımları

Görünüm Tasarımcısı sabit bir temsil stiline sahiptir ve çalışma kitapları, verilerin nasıl temsil edileceğini ve verilerin nasıl gösterileceğini değiştirebilir. Aşağıdaki görüntüler, görünümleri dönüştürürken çalışma kitaplarını nasıl düzenlediğiniz hakkında iki örnek oluşturur.

Dikey](media/view-designer-conversion-options/view-designer-vertical.png) ![dikey [çalışma kitabı](view-designer-conversion-examples.md#vertical)


[Sekmeli çalışma kitabı](view-designer-conversion-examples.md#tabbed)
![veri türü dağıtım sekmesi](media/view-designer-conversion-options/distribution-tab.png)
zaman sekmesi içinde veri türleri ![](media/view-designer-conversion-options/over-time-tab.png)

## <a name="tile-conversion"></a>Kutucuk dönüştürmesi
Görünüm Tasarımcısı genel durumu göstermek ve özetlemek için genel bakış kutucuğu özelliğini kullanır. Bunlar, sayılardan ve grafiklerin arasında değişen yedi kutucukla temsil edilir. Kullanıcılar, çalışma kitaplarında benzer görselleştirmeler oluşturabilir ve bunları genel bakış kutucuklarının özgün stiline benzeecek şekilde sabitleyebilir. 

![Galeri](media/view-designer-conversion-options/overview.png)


## <a name="view-dashboard-conversion"></a>Pano dönüştürmeyi görüntüle
Görünüm Tasarımcısı kutucukları genellikle iki bölümden oluşur; örneğin **halka & liste** kutucuğu gibi görselleştirmedeki verilerle eşleşen bir görselleştirme ve bir liste.

![halka](media/view-designer-conversion-options/donut-example.png)

Çalışma kitaplarında, kullanıcının görünümün bir kısmını veya her ikisini birden sorgulamasına izin veririz. Çalışma kitaplarındaki sorguları formülleyici basit bir iki adımlı işlemdir. İlk olarak, veriler sorgudan oluşturulur ve ikincisi bir görselleştirme olarak işlenir.  Çalışma kitaplarında bu görünümün nasıl yeniden oluşturulacağına ilişkin bir örnek aşağıdaki gibidir:

![Dönüştür](media/view-designer-conversion-options/convert-donut.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Çalışma kitaplarına erişme & izinleri](view-designer-conversion-access.md)