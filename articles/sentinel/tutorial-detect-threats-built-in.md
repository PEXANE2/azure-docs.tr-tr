---
title: Azure Sentinel ile uyarıları araştırın | Microsoft Docs
description: Şüpheli bir sorun olduğunda size bildirimde bulunan yerleşik, yerleşik Azure tehdit algılama şablonlarını nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 5d73337c25c812363b7a542bf42372ca3baa10e8
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605445"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Öğretici: kullanıma hazır tehditleri algılama


> [!IMPORTANT]
> Kullanıma hazır tehdit algılama Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Veri kaynaklarınızı](quickstart-onboard.md)   Azure Sentinel 'e bağladıktan sonra, şüpheli bir sorun oluştuğunda bildirim almak isteyeceksiniz. Azure Sentinel, tehdit algılama kuralları oluşturmanıza yardımcı olacak yerleşik, yerleşik şablonlar sağlar. Bu şablonlar, Microsoft 'un, bilinen tehditler, yaygın saldırı vektörleri ve şüpheli etkinlik yükseltme zincirlerine göre güvenlik uzmanı ve analistlerinin ekibi tarafından tasarlanmıştır. Bu şablonlardan oluşturulan kurallar, şüpheli görünen herhangi bir etkinlik için ortamınızda otomatik olarak arama yapar. Şablonların birçoğu, etkinlikleri aramak veya gereksinimlerinize göre onları filtrelemek için özelleştirilebilir. Bu kurallar tarafından oluşturulan uyarılar, ortamınızda atayabileceğiniz ve araştırabilmeniz için olaylar oluşturur.

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

- **Machine Learning davranış analizi**

    Bu şablonlar, Microsoft makine öğrenimi algoritmalarını temel alır. bu nedenle, nasıl çalıştıkları ve ne zaman çalıştıkları hakkında iç mantığı göremezsiniz. Logic Hidden ve bu nedenle özelleştirilebilir olmadığından, bu türden her şablonla yalnızca bir kural oluşturabilirsiniz.

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

