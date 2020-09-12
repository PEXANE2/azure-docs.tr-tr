---
title: Azure kaynaklarını başka bir bölgeye taşıma
description: Azure bölgelerinde Azure kaynakları taşımaya genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 7a71502ec361004079e0962d8bc6433316a4ba81
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007647"
---
# <a name="moving-azure-resources-across-regions"></a>Azure kaynaklarını bölgeler arasında taşıma

Bu makalede, Azure bölgelerine Azure bölgeleri arasında taşıma hakkında bilgi sağlanır.

Azure coğrafi bölgeler, bölgeler ve kullanılabilirlik alanları, Azure genel altyapısının temelini oluşturur. Azure [geographıes](https://azure.microsoft.com/global-infrastructure/geographies/) genellikle iki veya daha fazla [Azure bölgesi](https://azure.microsoft.com/global-infrastructure/regions/)içerir. Bölge, Kullanılabilirlik Alanları ve birden çok veri merkezi içeren coğrafya içindeki bir alandır. 

Belirli Azure bölgesindeki kaynakları dağıttıktan sonra, kaynakları farklı bir bölgeye taşımak isteyebileceğiniz birçok neden vardır.

- **Bir bölge başlatma kısmına hizalayın**: kaynaklarınızı önceden kullanılamayan yeni bir sunulan Azure bölgesine taşıyın.
- **Hizmetler/Özellikler Için hizalayın**: belirli bir bölgede bulunan hizmetlerden veya özelliklerden yararlanmak için kaynakları taşıyın.
- **İş gelişmeleri yanıtlayın**: birleşmeler veya alımlar gibi iş değişikliklerine yanıt olarak kaynakları bir bölgeye taşıyın.
- **Yakınlık Için Hizala**: kaynakları işletmenize yerel bir bölgeye taşıyın.
- **Veri gereksinimlerini karşılayın**: kaynakları, veri yerleşimi gereksinimleriyle veya veri sınıflandırması ihtiyaçlarına göre hizalamak için taşıyın. [Daha fazla bilgi edinin](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf).
- **Dağıtım gereksinimlerine yanıt**: hata halinde dağıtılan kaynakları taşıyın veya kapasite ihtiyaçlarına yanıt olarak taşıyın. 
- **Yetki alma Işlemine yanıt ver**: bölgelerin yetkisini alma nedeniyle kaynakları taşıyın.

## <a name="move-resources-with-resource-mover"></a>Kaynak taşıyıcısı ile kaynakları taşıma

Kaynakları [Azure Kaynak taşıyıcısı](../../resource-mover/overview.md)ile farklı bir bölgeye taşıyabilirsiniz. Kaynak taşıyıcısı şunları sağlar:

- Kaynakları bölgeler arasında taşımak için tek bir hub.
- Daha az taşıma süresi ve karmaşıklığı. İhtiyacınız olan her şey tek bir konumda.
- Farklı türlerde Azure kaynaklarını taşımak için basit ve tutarlı bir deneyim.
- Taşımak istediğiniz kaynaklar arasında bağımlılıkları belirlemenin kolay bir yolu. Bu, ilgili kaynakları bir arada taşımanızı sağlar, böylece taşıma işleminden sonra her şeyin hedef bölgede beklendiği gibi çalışması gerekir.
- Taşıma işleminden sonra silmek istiyorsanız kaynak bölgedeki kaynakların otomatik olarak temizlenmesi.
- Edici. Bir taşımayı deneyebilir ve tam bir taşıma yapmak istemiyorsanız bu uygulamayı atabilirsiniz.

Birkaç farklı yöntem kullanarak kaynakları başka bir bölgeye taşıyabilirsiniz:

- **Kaynakları bir kaynak grubundan taşımaya başlayın**: Bu yöntem ile bir kaynak grubu içindeki bölge hareketini başlatabilirsiniz. Taşımak istediğiniz kaynakları seçtikten sonra işlem, Kaynak bağımlılıklarını denetlemek ve taşıma işlemini düzenlemek için kaynak taşıyıcısı hub 'ında devam eder. [Daha fazla bilgi edinin](../../resource-mover/move-region-within-resource-group.md).
- **Kaynakları doğrudan kaynak taşıyıcısı hub 'ından taşımaya başlayın**: Bu yöntemle, bölge taşıma işlemini doğrudan hub 'da başlatabilirsiniz. [Daha fazla bilgi edinin](../../resource-mover/tutorial-move-region-virtual-machines.md).


## <a name="support-for-region-move"></a>Bölge taşıma desteği

Şu anda bu kaynakları başka bir bölgeye taşımak için kaynak taşıyıcısı kullanabilirsiniz:

- Azure VM 'Leri ve ilişkili diskler
- NIC’ler
- Kullanılabilirlik kümeleri
- Azure sanal ağları
- Genel IP adresleri
- Ağ güvenlik grupları (NSG)
- İç ve genel yük dengeleyiciler
- Azure SQL veritabanları ve elastik havuzlar

## <a name="region-move-process"></a>Bölge taşıma işlemi

Kaynakları bölgeler arasında taşıma işleminin gerçek süreci, taşıdığınız kaynaklara bağlıdır. Ancak bazı yaygın anahtar adımları vardır:

1. **Önkoşulları doğrulama**: Önkoşullar, ihtiyacınız olan kaynakların hedef bölgede kullanılabilir olduğundan emin olmanızı, yeterli kotayı sahip olduğunuzu kontrol etmeyi ve aboneliğinizin hedef bölgeye erişebildiğini doğrulamayı içerir.
2. **Bağımlılıkları çözümle**: kaynaklarınızın diğer kaynaklara bağımlılıkları olabilir. Taşımadan önce, taşınan kaynakların taşıma işleminden sonra beklendiği gibi çalışmaya devam etmesi için bağımlılıkları şekilsiz yapın.
3. **Taşımaya hazırlanma**: Bu, taşımadan önce birincil bölgenizde yaptığınız adımlardır. Örneğin, bir Azure Resource Manager şablonu dışarı aktarmanız veya kaynakları kaynaktan hedefe çoğaltmaya başlamanız gerekebilir.
4. **Kaynakları taşıma**: kaynakları taşıma işlemlerinin ne olduğuna bağlıdır. Hedef bölgede bir şablon dağıtmanız ya da kaynakları hedefe devretmek gerekebilir.
5. **Hedef kaynakları at**: kaynakları taşıdıktan sonra hedef bölgede şu anda kaynaklara göz atabilir ve ihtiyaç duymayacağınıza karar vermeniz gerekebilir.
6. **Taşımayı Yürüt**: hedef bölgedeki kaynakları doğruladıktan sonra bazı kaynaklar için son bir işleme eylemi gerekebilir. Örneğin, artık birincil bölge olan bir hedef bölgede, yeni bir ikincil bölgeye olağanüstü durum kurtarmayı ayarlamanız gerekebilir. 
7. **Kaynağı Temizleme**: son olarak, her şey yeni bölgede çalışmaya başladıktan sonra, taşıma için oluşturduğunuz kaynakları ve birincil bölgenizdeki kaynakları temizleyebilir ve yetkisini alabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı içinde taşıma işlemi hakkında [daha fazla bilgi edinin](../../resource-mover/about-move-process.md) .
