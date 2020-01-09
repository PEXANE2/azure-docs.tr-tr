---
title: Azure kaynaklarını başka bir bölgeye taşıma
description: Azure bölgelerinde Azure kaynakları taşımaya genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: raynew
ms.openlocfilehash: 22d8bcee96b4ac52641d4f0841267195f44fe15a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75485214"
---
# <a name="moving-azure-resources-across-regions"></a>Azure kaynaklarını bölgeler arasında taşıma

Bu makalede, Azure bölgelerine Azure bölgeleri arasında taşıma hakkında bilgi sağlanır.

Azure coğrafi bölgesi, bölgeler ve Kullanılabilirlik Alanları Azure genel altyapısının temelini oluşturur. Azure [geographıes](https://azure.microsoft.com/global-infrastructure/geographies/) genellikle iki veya daha fazla [Azure bölgesi](https://azure.microsoft.com/global-infrastructure/regions/)içerir. Bölge, Kullanılabilirlik Alanları ve birden çok veri merkezi içeren coğrafya içindeki bir alandır. 

Belirli Azure bölgesindeki kaynakları dağıttıktan sonra, kaynakları farklı bir bölgeye taşımak isteyebileceğiniz birçok neden vardır.

- **Bir bölge başlatma kısmına hizalayın**: kaynaklarınızı önceden kullanılamayan yeni bir sunulan Azure bölgesine taşıyın.
- **Hizmetler/Özellikler Için hizalayın**: belirli bir bölgede bulunan hizmetlerden veya özelliklerden yararlanmak için kaynakları taşıyın.
- **İş gelişmeleri yanıtlayın**: birleşmeler veya alımlar gibi iş değişikliklerine yanıt olarak kaynakları bir bölgeye taşıyın.
- **Yakınlık Için Hizala**: kaynakları işletmenize yerel bir bölgeye taşıyın.
- **Veri gereksinimlerini karşılayın**: kaynakları, veri yerleşimi gereksinimleriyle veya veri sınıflandırması ihtiyaçlarına göre hizalamak için taşıyın. [Daha fazla bilgi edinin](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Dağıtım gereksinimlerine yanıt**: hata halinde dağıtılan kaynakları taşıyın veya kapasite ihtiyaçlarına yanıt olarak taşıyın. 
- **Yetki alma Işlemine yanıt ver**: bölgelerin yetkisini alma nedeniyle kaynakları taşıyın.

## <a name="move-process"></a>İşlemi taşı

Gerçek taşıma işlemi, taşıdığınız kaynaklara bağlıdır. Ancak bazı yaygın anahtar adımları vardır:

- **Önkoşulları doğrulama**: Önkoşullar, ihtiyacınız olan kaynakların hedef bölgede kullanılabilir olduğundan emin olmanızı, yeterli kotayı sahip olduğunuzu kontrol etmeyi ve aboneliğinizin hedef bölgeye erişebildiğini doğrulamayı içerir.
- **Bağımlılıkları çözümle**: kaynaklarınızın diğer kaynaklara bağımlılıkları olabilir. Taşımadan önce, taşınan kaynakların taşıma işleminden sonra beklendiği gibi çalışmaya devam etmesi için bağımlılıkları şekilsiz yapın.
- **Taşımaya hazırlanma**: Bu, taşımadan önce birincil bölgenizde yaptığınız adımlardır. Örneğin, bir Azure Resource Manager şablonu dışarı aktarmanız veya kaynakları kaynaktan hedefe çoğaltmaya başlamanız gerekebilir.
- **Kaynakları taşıma**: kaynakları taşıma işlemlerinin ne olduğuna bağlıdır. Hedef bölgede bir şablon dağıtmanız ya da kaynakları hedefe devretmek gerekebilir.
- **Hedef kaynakları at**: kaynakları taşıdıktan sonra hedef bölgede şu anda kaynaklara göz atabilir ve ihtiyaç duymayacağınıza karar vermeniz gerekebilir.
- **Taşımayı Yürüt**: hedef bölgedeki kaynakları doğruladıktan sonra bazı kaynaklar için son bir işleme eylemi gerekebilir. Örneğin, artık birincil bölge olan bir hedef bölgede, yeni bir ikincil bölgeye olağanüstü durum kurtarmayı ayarlamanız gerekebilir. 
- **Kaynağı Temizleme**: son olarak, her şey yeni bölgede çalışmaya başladıktan sonra, taşıma için oluşturduğunuz kaynakları ve birincil bölgenizdeki kaynakları temizleyebilir ve yetkisini alabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bölgeler arasında taşınmayı destekleyen kaynakların listesi için bkz. [kaynaklar Için taşıma işlemi desteği](region-move-support.md).
