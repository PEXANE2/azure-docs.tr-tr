---
title: Azure Sentinel 'de yerleşik analiz kuralları ile tehditleri algılama | Microsoft Docs
description: Şüpheli bir sorun olduğunda size bildirimde bulunan yerleşik şablonları temel alan yerleşik tehdit algılama kurallarını nasıl kullanacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2020
ms.author: yelevin
ms.openlocfilehash: b8a304d523052f1fe839f94608806df51d35fdf9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104589931"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Öğretici: Hemen tehditleri algılayabilme

Veri kaynaklarınızı Azure Sentinel 'e [bağladıktan](quickstart-onboard.md) sonra, şüpheli bir sorun oluştuğunda bildirim almak isteyeceksiniz. Azure Sentinel, tehdit algılama kuralları oluşturmanıza yardımcı olacak yerleşik, yerleşik şablonlar sağlar. Bu şablonlar, Microsoft 'un, bilinen tehditler, yaygın saldırı vektörleri ve şüpheli etkinlik yükseltme zincirlerine göre güvenlik uzmanı ve analistlerinin ekibi tarafından tasarlanmıştır. Bu şablonlardan oluşturulan kurallar, şüpheli görünen herhangi bir etkinlik için ortamınızda otomatik olarak arama yapar. Şablonların birçoğu, etkinlikleri aramak veya gereksinimlerinize göre onları filtrelemek için özelleştirilebilir. Bu kurallar tarafından oluşturulan uyarılar, ortamınızda atayabileceğiniz ve araştırabilmeniz için olaylar oluşturur.

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur:

> [!div class="checklist"]
> * Kullanıma hazır tehdit algılamalarını kullanma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="about-out-of-the-box-detections"></a>Hazır gelen algılamalar hakkında

Kullanıma hazır tüm algılamaları görüntülemek için analizler **' e ve ardından** **kural şablonları**' na gidin. Bu sekme, tüm Azure Sentinel yerleşik kurallarını içerir.

   :::image type="content" source="media/tutorial-detect-built-in/view-oob-detections.png" alt-text="Azure Sentinel ile tehditleri bulmak için yerleşik algılamaları kullanın":::

Aşağıdaki şablon türleri kullanılabilir:

- **Microsoft Güvenlik**
   
   Microsoft güvenlik şablonları, diğer Microsoft Güvenlik çözümlerinde oluşturulan uyarılardan gerçek zamanlı olarak Azure Sentinel olaylarını otomatik olarak oluşturur. Benzer mantığa sahip yeni kurallar oluşturmak için Microsoft güvenlik kurallarını şablon olarak kullanabilirsiniz. Güvenlik kuralları hakkında daha fazla bilgi için bkz. [Microsoft Güvenlik uyarılarından olayları otomatik olarak oluşturma](create-incidents-from-alerts.md).

- **Fusion** 

    Fusion teknolojisine bağlı olarak, Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama, birden fazla ürün genelinde çok sayıda düşük kaliteli uyarı ve olayı Yüksek uygunluğa ve eyleme dönüştürülebilir olaylara ilişkilendirebilen ölçeklenebilir makine öğrenimi algoritmaları kullanır. Fusion varsayılan olarak etkindir. Logic Hidden ve bu nedenle özelleştirilebilir olmadığından, bu şablonla yalnızca bir kural oluşturabilirsiniz.

    > [!IMPORTANT]
    > Fusion kural şablonundaki algılamalardan bazıları şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.
    >
    > Hangi algılamaların önizlemede olduğunu görmek için bkz. [Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama](fusion.md).

- **Makine öğrenmesi davranış analizi**

    Bu şablonlar, Microsoft makine öğrenimi algoritmalarını temel alır. bu nedenle, nasıl çalıştıkları ve ne zaman çalıştıkları hakkında iç mantığı göremezsiniz. Logic Hidden ve bu nedenle özelleştirilebilir olmadığından, bu türden her şablonla yalnızca bir kural oluşturabilirsiniz.

    > [!IMPORTANT]
    > Machine Learning davranış analizi kural şablonları Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

- **Zamanlanan**

    Zamanlanan analiz kuralları, Microsoft güvenlik uzmanları tarafından yazılan yerleşik sorguları temel alır. Sorgu mantığını görebilir ve üzerinde değişiklik yapabilirsiniz. Zamanlanan kurallar şablonunu kullanabilir ve yeni kurallar oluşturmak için Sorgu mantığı ve zamanlama ayarlarını özelleştirebilirsiniz.

## <a name="use-out-of-the-box-detections"></a>Kullanıma hazır algılamaları kullanma

1. Yerleşik bir şablon kullanmak için şablon adına tıklayın ve ardından Ayrıntılar bölmesindeki **kural oluştur** düğmesine tıklayarak bu şablonu temel alan yeni bir etkin kural oluşturun. Her şablonda gerekli veri kaynakları listesi bulunur. Şablonu açtığınızda, veri kaynakları otomatik olarak kullanılabilirlik için denetlenir. Bir kullanılabilirlik sorunu varsa, **kural oluştur** düğmesi devre dışı bırakılabilir veya bu etkiye bir uyarı görebilirsiniz.
  
    :::image type="content" source="media/tutorial-detect-built-in/use-built-in-template.png" alt-text="Algılama kuralı Önizleme bölmesi":::
 
1. **Kural oluştur** düğmesine tıklamak, seçilen şablonu temel alan kural oluşturma Sihirbazı ' nı açar. Tüm ayrıntılar, yeniden doldurulur ve **zamanlanan** veya **Microsoft Güvenlik** şablonlarıyla, mantığın ve diğer kural ayarlarının özel gereksinimlerinize daha uygun olması için özelleştirebilirsiniz. Bu işlemi, yerleşik şablonu temel alan ek kurallar oluşturmak için yineleyebilirsiniz. Kural oluşturma sihirbazındaki adımları sona erdikten sonra, şablonu temel alan bir kural oluşturmayı bitirirsiniz. Yeni kurallar, **etkin kurallar** sekmesinde görünür.

    Kural oluşturma Sihirbazı 'nda kurallarınızı özelleştirme hakkında daha fazla bilgi için bkz. [öğretici: tehditleri algılamak için özel analiz kuralları oluşturma](tutorial-detect-threats-custom.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel kullanarak tehditleri algılamaya nasıl başlacağınız hakkında daha fazla öğrenirsiniz. 

Tehditlere yönelik yanıtlarınızı otomatik hale getirmeyi öğrenmek için [Azure Sentinel 'de otomatik tehdit yanıtları ayarlayın](tutorial-respond-threats-playbook.md).
