---
title: Azure kaynaklarını başka bir bölgeye taşıma
description: Azure kaynaklarını Azure bölgeleri arasında taşımaya genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: raynew
ms.openlocfilehash: 22d8bcee96b4ac52641d4f0841267195f44fe15a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485214"
---
# <a name="moving-azure-resources-across-regions"></a>Azure kaynaklarını bölgeler arasında taşıma

Bu makalede, Azure kaynaklarını Azure bölgeleri arasında taşıma hakkında bilgi verilmektedir.

Azure coğrafyaları, bölgeleri ve Kullanılabilirlik Bölgeleri Azure küresel altyapısının temelini oluşturur. Azure [coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/) genellikle iki veya daha fazla [Azure bölgesi](https://azure.microsoft.com/global-infrastructure/regions/)içerir. Bölge, kullanılabilirlik bölgeleri ve birden çok veri merkezi içeren bir coğrafya içinde bir alandır. 

Belirli Azure bölgesinde kaynakları dağıttıktan sonra, kaynakları farklı bir bölgeye taşımak istemenizin birkaç nedeni vardır.

- **Bir bölge başlatmasına hizala**: Kaynaklarınızı daha önce kullanılamayan yeni tanıtılan bir Azure bölgesine taşıyın.
- **Hizmetler/özellikler için hizala**: Belirli bir bölgede kullanılabilen hizmetlerden veya özelliklerden yararlanmak için kaynakları taşıyın.
- **İş gelişmelerine yanıt verme**: Kaynakları, birleşme veya satın alma lar gibi iş değişikliklerine yanıt olarak bir bölgeye taşıyın.
- **Yakınlık için hizala**: Kaynakları işletmenizin yerel bölgesine taşıyın.
- **Veri gereksinimlerini karşılayın**: Veri ikamet gereksinimleriveya veri sınıflandırma gereksinimleriyle uyumlu olacak şekilde kaynakları taşıyın. [Daha fazla bilgi edinin](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Dağıtım gereksinimlerine yanıt verme**: Hatalı olarak dağıtılan kaynakları taşıyın veya kapasite gereksinimlerine yanıt olarak taşıyın. 
- **Hizmet dışı bırakma yanıtı**: Bölgelerin devre dışı bırakılması nedeniyle kaynakları taşıyın.

## <a name="move-process"></a>Taşıma işlemi

Gerçek taşıma işlemi, taşımakta olduğunuz kaynaklara bağlıdır. Ancak, bazı ortak anahtar adımları vardır:

- **Ön koşulları doğrulayın**: Ön koşullar, gereksinim duyduğunuz kaynakların hedef bölgede kullanılabilir olduğundan emin olmak, yeterli kotanız olup olmadığını kontrol etmek ve aboneliğinizin hedef bölgeye erişebileceğini doğrulamaktır.
- **Bağımlılıkları çözümle**: Kaynaklarınız diğer kaynaklara bağımlı olabilir. Taşınmadan önce, taşınan kaynakların taşımadan sonra beklendiği gibi çalışmaya devam etmesi için bağımlılıkları çözün.
- **Taşımaya hazırlanın**: Bunlar taşınmadan önce birincil bölgenizde attığınız adımlardır. Örneğin, bir Azure Kaynak Yöneticisi şablonu dışa aktarmanız veya kaynakları kaynaktan hedefe çoğaltmaya başlamanız gerekebilir.
- **Kaynakları taşıma**: Kaynakları nasıl taşıyabileceğiniz, kaynakların ne olduğuna bağlıdır. Hedef bölgede bir şablon dağıtmanız veya kaynakları hedefe göndermeniz gerekebilir.
- **Hedef kaynakları atın**: Kaynakları taşındıktan sonra, hedef bölgedeki kaynaklara bir göz atmak ve ihtiyacınız olmayan bir şey olup olmadığına karar vermek isteyebilirsiniz.
- **Hareketi gerçekleştirin**: Hedef bölgedeki kaynakları doğruladıktan sonra, bazı kaynaklar son bir işleme eylemi gerektirebilir. Örneğin, şimdi birincil bölge olan bir hedef bölgede, yeni bir ikincil bölgeye olağanüstü durum kurtarma ayarlamanız gerekebilir. 
- **Kaynağı temizleme**: Son olarak, yeni bölgede her şey hazır ve çalışır hale geldikten sonra, hareket için oluşturduğunuz kaynakları ve birincil bölgenizdeki kaynakları temizleyebilir ve devre dışı açabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Kaynakların bölgeler arasında taşınmasını destekleyen bir liste [için](region-move-support.md)bkz.
