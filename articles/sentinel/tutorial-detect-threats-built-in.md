---
title: Azure Sentinel ile uyarıları araştırın| Microsoft Dokümanlar
description: Azure Sentinel ile uyarıları nasıl araştıracağımız öğrenmek için bu öğreticiyi kullanın.
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
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: df855aa768fdeb279482b8407259be1a644322d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585212"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Öğretici: Tehditleri kutudan çıkarmadan algılama


> [!IMPORTANT]
> Kullanıma açık tehdit algılama şu anda genel önizlemede.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Veri [kaynaklarınızı](quickstart-onboard.md) Azure Sentinel'e bağladıktan sonra, şüpheli bir şey olduğunda bilgilendirilmek istersiniz. Azure Sentinel, bunu yapmanızı sağlamak için size kutudan yerleşik şablonlar sağlar. Bu şablonlar, Microsoft'un güvenlik uzmanları ve analistlerinden oluşan ekibi tarafından bilinen tehditlere, yaygın saldırı vektörlerine ve şüpheli etkinlik yükseltme zincirlerine göre tasarlanmıştır. Bu şablonları etkinleştirdikten sonra, ortamınızda şüpheli görünen herhangi bir etkinliği otomatik olarak ararlar. Şablonların çoğu, gereksinimlerinize göre etkinlikleri aramak veya filtrelemek için özelleştirilebilir. Bu şablonlar tarafından oluşturulan uyarılar, ortamınızda atayabileceğiniz ve araştırabileceğiniz olaylar oluşturur.

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur:

> [!div class="checklist"]
> * Kullanıma sunalgılarını kullanma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="about-out-of-the-box-detections"></a>Hazır gelen algılamalar hakkında

Tüm kutudan çıkan algılamaları görüntülemek için **Analytics'e** gidin ve ardından **şablonları kuralın.** Bu sekme, tüm Azure Sentinel yerleşik kurallarını içerir.

   ![Azure Sentinel ile tehditleri bulmak için yerleşik algılamaları kullanma](media/tutorial-detect-built-in/view-oob-detections.png)

Aşağıdaki şablon türleri kullanılabilir:

- **Microsoft güvenlik** - Microsoft güvenlik şablonları, diğer Microsoft güvenlik çözümlerinde oluşturulan uyarılardan azure sentinel olaylarını gerçek zamanlı olarak otomatik olarak oluşturur. Benzer mantıkla yeni kurallar oluşturmak için Microsoft güvenlik kurallarını şablon olarak kullanabilirsiniz. Güvenlik kuralları hakkında daha fazla bilgi için [bkz.](create-incidents-from-alerts.md)
- **Fusion** - Fusion teknolojisine dayalı olarak, Azure Sentinel'deki gelişmiş çok aşamalı saldırı algılama, birden fazla üründeki birçok düşük doğruluk uyarısı ve olayı yüksek sadakat ve eyleme geçirilebilir olaylarla ilişkilendirebilen ölçeklenebilir makine öğrenme algoritmaları kullanır. Birleştirme varsayılan olarak etkinleştirilir. Mantık gizli olduğundan, bunu birden fazla kural oluşturmak için şablon olarak kullanamazsınız.
- **Makine öğrenimi davranış analizi** - Bu şablonlar özel Microsoft makine öğrenimi algoritmalarına dayanır, bu nedenle nasıl çalıştıklarını ve ne zaman çalıştıklarını niçin içsel mantığını göremezsiniz. Mantık gizli olduğundan, bunu birden fazla kural oluşturmak için şablon olarak kullanamazsınız.
-   **Zamanlanmış** analitik kurallar, Microsoft güvenlik uzmanları tarafından yazılmış zamanlanmış sorgulardır. Sorgu mantığını görebilir ve değişiklik yapabilirsiniz. Benzer mantıkla yeni kurallar oluşturmak için zamanlanmış kuralları şablon olarak kullanabilirsiniz.

## <a name="use-out-of-the-box-detections"></a>Kullanıma sunalgılarını kullanma

1. Yerleşik bir şablon kullanmak için, bu şablonu temel alan yeni bir etkin kural oluşturmak için **Oluştur kuralını** tıklatın. Her giriş, otomatik olarak denetlenen gerekli veri kaynaklarının bir listesine sahiptir ve bu da **Create kuralının** devre dışı edilmesine neden olabilir.
  
   ![Azure Sentinel ile tehditleri bulmak için yerleşik algılamaları kullanma](media/tutorial-detect-built-in/use-built-in-template.png)
 
1. Bu, seçili şablonu temel alan kural oluşturma sihirbazını açar. Tüm ayrıntılar otomatik olarak doldurulur ve **Zamanlanan kurallar** veya **Microsoft güvenlik kuralları**için, mantığı kuruluşunuza daha iyi uyacak şekilde özelleştirebilir veya yerleşik şablona dayalı ek kurallar oluşturabilirsiniz. Kural oluşturma sihirbazı adımları izleyerek ve şablona dayalı bir kural oluşturma yı bitirdikten sonra, yeni kural **Etkin kurallar** sekmesinde görünür.

Sihirbazdaki alanlar hakkında daha fazla bilgi için [Bkz. Öğretici: Şüpheli tehditleri algılamak için özel analitik kurallar oluşturun.](tutorial-detect-threats-custom.md)



## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure Sentinel'i kullanarak tehditleri algılamaya nasıl başlabildiğinizi öğrendiniz. 

Tehditlere karşı yanıtlarınızı nasıl otomatikleştirerinizi öğrenmek için [Azure Sentinel'de otomatik tehdit yanıtları ayarlayın.](tutorial-respond-threats-playbook.md)

