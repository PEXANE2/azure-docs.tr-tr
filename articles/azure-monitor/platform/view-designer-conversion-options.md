---
title: Çalışma kitapları dönüştürme seçeneklerine Azure Monitor görünüm tasarımcısı
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 7bfa831332451718c0c9c05023b90104d2b8b02b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658719"
---
# <a name="azure-monitor-view-designer-to-workbooks-conversion-options"></a>Çalışma kitapları dönüştürme seçeneklerine Azure Monitor görünüm tasarımcısı
[Görünüm tasarımcısı,](view-designer.md) Azure Monitor'un, Log Analytics çalışma alanınızdaki grafikler, listeler ve zaman çizelgeleriyle verileri görselleştirmenize yardımcı olacak özel görünümler oluşturmanıza olanak tanıyan bir özelliğidir. Bunlar aşamalı olarak devre dışı ediliyor ve ek işlevsellik sağlayan çalışma kitaplarıyla değiştiriliyor. Bu makalede, iki ve çalışma kitaplarına görünümleri dönüştürme seçenekleri arasındaki temel kavramları karşılaştırır.

## <a name="basic-workbook-designs"></a>Temel çalışma kitabı tasarımları

Görünüm tasarımcısı sabit statik bir gösterim stiline sahipken, çalışma kitapları verilerin nasıl temsil edildiğini ekleme ve değiştirme özgürlüğüne olanak sağlar. Aşağıdaki resimler, görünümleri dönüştürürken çalışma kitaplarını nasıl düzenleyebileceğiniz iki örneği gösterecektir.

[Dikey çalışma kitabı](view-designer-conversion-examples.md#vertical)
![Dikey](media/view-designer-conversion-options/view-designer-vertical.png)

[Sekmeli çalışma kitabı](view-designer-conversion-examples.md#tabbed)
![Veri](media/view-designer-conversion-options/distribution-tab.png)
![türü dağıtım sekmesi Zaman sekmesinde veri türleri](media/view-designer-conversion-options/over-time-tab.png)

## <a name="tile-conversion"></a>Döşeme dönüştürme
Görünüm tasarımcısı genel durumu temsil etmek ve özetlemek için genel görünüm döşemeözelliğini kullanır. Bunlar, sayılardan grafiklere kadar yedi kutucukta temsil edilir. Çalışma kitaplarında, kullanıcılar benzer görseller oluşturabilir ve bunları genel bakış kutucuklarının orijinal stiline benzeyecek şekilde sabitleyebilir. 

![Galeri](media/view-designer-conversion-options/overview.png)


## <a name="view-dashboard-conversion"></a>Pano dönüşümlerini görüntüleme
Görünüm tasarımcı sıdabı genellikle iki bölümden oluşur, bir görselleştirme ve görselleştirme verileri eşleşen bir liste ( örneğin **Donut & Listesi** döşemesi.

![Halka](media/view-designer-conversion-options/donut-example.png)

Çalışma kitaplarında, kullanıcının görünümün bir veya her iki bölümünü sorgulamayı seçmesine izin veririz. Çalışma kitaplarında sorguformüle basit bir iki adımlı işlemdir. İlk olarak, veriler sorgudan oluşturulur ve ikinci olarak, veriler görselleştirme olarak işlenir.  Bu görünümün çalışma kitaplarında nasıl yeniden oluşturulacağına bir örnek aşağıdaki gibidir:

![Dönüştür](media/view-designer-conversion-options/convert-donut.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Çalışma kitaplarına & izinlerine erişim](view-designer-conversion-access.md)