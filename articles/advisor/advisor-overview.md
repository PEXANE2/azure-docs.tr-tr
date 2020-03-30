---
title: Azure Danışmanına Giriş
description: Azure dağıtımlarınızı optimize etmek için Azure Danışmanı'nı kullanın.
ms.topic: article
ms.date: 02/01/2019
ms.openlocfilehash: 600bda282d46f86979d0366719826c3a6c1323e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443090"
---
# <a name="introduction-to-azure-advisor"></a>Azure Danışmanına Giriş

Azure Danışmanı'nın temel özellikleri hakkında bilgi edinin ve sık sorulan soruların yanıtlarını alın.

## <a name="what-is-advisor"></a>Danışman nedir?
Danışman, Azure dağıtımlarınızı optimize etmek için en iyi uygulamaları izlemenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

Advisor ile şunları yapabilirsiniz:
* Proaktif, eyleme dönüştürülebilir ve kişiselleştirilmiş en iyi yöntem önerileri alabilirsiniz. 
* Genel Azure harcamanızı azaltmak için fırsatları tanımlarken, kaynaklarınızın performansını, güvenliğini ve yüksek kullanılabilirliğini artırın.
* İşlem sırasında önerilen eylemler halinde öneriler alabilirsiniz.

Danışman'a Azure [portalı](https://aka.ms/azureadvisordashboard)üzerinden erişebilirsiniz. [Portalda](https://portal.azure.com)oturum açın, navigasyon menüsünde **Danışman'ı** bulun veya **Tüm hizmetler** menüsünde arayın.

Danışman panosu, tüm abonelikleriniz için kişiselleştirilmiş öneriler görüntüler.  Belirli abonelikler ve kaynak türleri için öneriler görüntülemek için filtreler uygulayabilirsiniz.  Öneriler dört kategoriye ayrılır: 

* **Yüksek Kullanılabilirlik**: İş açısından kritik uygulamalarınızın sürekliliğini sağlamak ve geliştirmek. Daha fazla bilgi için [Danışman Yüksek Kullanılabilirlik önerilerine](advisor-high-availability-recommendations.md)bakın.
* **Güvenlik**: Güvenlik ihlallerine yol açabilecek tehditleri ve güvenlik açıklarını tespit etmek. Daha fazla bilgi için [Danışman Güvenlik önerilerine](advisor-security-recommendations.md)bakın.
* **Performans**: Uygulamalarınızın hızını artırmak için. Daha fazla bilgi için [Danışman Performansı önerilerine](advisor-performance-recommendations.md)bakın.
* **Maliyet**: Genel Azure harcamalarınızı optimize etmek ve azaltmak için. Daha fazla bilgi için [Danışman Maliyet önerilerine](advisor-cost-recommendations.md)bakın.
* **Operasyonel Mükemmellik**: Süreç ve iş akışı verimliliği, kaynak yönetilebilirliği ve dağıtım en iyi uygulamaları elde yardımcı olmak için. . Daha fazla bilgi için [Danışman Operasyonel Mükemmellik önerilerine](advisor-operational-excellence-recommendations.md)bakın.

  ![Danışman öneri türleri](./media/advisor-overview/advisor-dashboard.png)

Bu kategorideki öneriler listesini görüntülemek için bir kategoriyi tıklatabilir ve bu kategori hakkında daha fazla bilgi edinmek için bir öneri seçebilirsiniz.  Ayrıca, bir fırsattan yararlanmak veya bir sorunu çözmek için gerçekleştirebileceğiniz eylemler hakkında bilgi edinebilirsiniz.

![Danışman tavsiye kategorisi](./media/advisor-overview/advisor-ha-category-example.png) 

Öneriyi uygulamak için bir öneri için önerilen eylemi seçin.  Öneriyi uygulamanızı sağlayan veya uygulamada size yardımcı olan belgelere yönlendiren basit bir arabirim açılır.  Bir öneri uyguladığınız da, Danışmanın bunu tanıması bir gün kadar sürebilir.

Bir öneri üzerine derhal harekete geçmeyi düşünmüyorsanız, bunu belirli bir süre için erteleyebilir veya görevden alabilirsiniz.  Belirli bir abonelik veya kaynak grubu için öneriler almak istemiyorsanız, Danışman'ı yalnızca belirtilen abonelikler ve kaynak grupları için öneriler oluşturacak şekilde yapılandırabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-access-advisor"></a>Danışman'a nasıl erişebilirim?
Danışman'a Azure [portalı](https://aka.ms/azureadvisordashboard)üzerinden erişebilirsiniz. [Portalda](https://portal.azure.com)oturum açın, navigasyon menüsünde **Danışman'ı** bulun veya **Tüm hizmetler** menüsünde arayın.

Danışman önerilerini sanal makine kaynak arabirimi üzerinden de görüntüleyebilirsiniz. Sanal bir makine seçin ve ardından menüdeki Danışman önerilerine gidin. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Danışman'a erişmek için hangi izinlere ihtiyacım var?
 
Danışmanı önerilerine *Aboneliğin Sahibi,* *Katılımcısı*veya *Okuyucusu* olarak erişebilirsiniz.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Danışman hangi kaynaklar için öneriler sağlar?

Danışman Uygulama Ağ Geçidi, Uygulama Hizmetleri, kullanılabilirlik kümeleri, Azure Önbelleği, Azure Veri Fabrikası, MySQL için Azure Veritabanı, PostgreSQL için Azure Veritabanı, MariaDB için Azure Veritabanı, Azure ExpressRoute, Azure Cosmos DB, Azure genel i IP adresleri, SQL Veri Ambarı, SQL sunucuları, depolama hesapları, Trafik Yöneticisi profilleri ve sanal makineler.

Azure Danışmanı, Azure [Güvenlik Merkezi'nden](https://docs.microsoft.com/azure/security-center/security-center-recommendations) ek kaynak türleri için öneriler içerebilecek önerilerinizi de içerir.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Bir öneriyi erteleyebilir veya reddedebilir miyim?

Bir öneriyi ertelemek veya reddetmek için **Ertele** bağlantısını tıklatın. Bir erteleme dönemi belirtebilir veya öneriyi asla reddetmeyi **seçebilirsiniz.**

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz:

* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Danışman Yüksek Kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
