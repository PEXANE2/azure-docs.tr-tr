---
title: Azure Uygulama teklifi | Azure Marketi
description: Azure Marketi'nde bir Azure Uygulaması teklifi yayımlama işlemine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: dsindona
ms.openlocfilehash: ed086ffdc49e21b819c0ee05b38ad882b4e269d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285325"
---
# <a name="azure-application-offer"></a>Azure uygulama teklifi

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| <div class="body"> Bu bölümde, Azure Marketi'nde yeni [Azure Marketplace](https://azuremarketplace.microsoft.com)bir Azure uygulama teklifinin nasıl yayımlanabileceği açıklanmaktadır.  Her Azure uygulaması, uygulama tarafından kullanılan tüm teknik varlıkları tanımlayan ve genellikle bir veya daha fazla sanal makine ve diğer desteklenen Azure veya Web tabanlı hizmetleri içeren bir Azure Kaynak Yöneticisi şablonu içerir. Tüm Azure uygulama [teklifleri, Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)aracılığıyla erişim güvenliğini etkinleştirmelidir.  </div> | ![Azure uygulamaları simgesi](./media/azureapp-icon1.png)  |

## <a name="publishing-overview"></a>Yayımlama genel bakış

Aşağıdaki video, [Oluşturma Çözümü Şablonları ve Azure Marketi için Yönetilen Uygulamalar,](https://channel9.msdn.com/Events/Build/2018/BRK3603)bir giriştir: hangi teklif türleri kullanılabilir, hangi teknik varlıklar gereklidir, Bir Azure Kaynak Yöneticisi şablonu nasıl yazılır, uygulama kullanıcı arabirimi geliştirme ve test etme, uygulama teklifinin nasıl yayımlanması ve uygulama gözden geçirme işlemi.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


## <a name="types-of-azure-applications"></a>Azure uygulama türleri

İki tür Azure uygulaması vardır: yönetilen uygulamalar ve çözüm şablonları. 

- Çözüm şablonları, Çözüm Market'te yayımlanmanın başlıca yollarından biridir. Bu teklif türü, çözümünüz tek bir sanal makinenin (VM) ötesinde ek dağıtım ve yapılandırma otomasyonu gerektirdiğinde kullanılır. Çözüm şablonu kullanarak birden fazla VM sağlayarak otomatikleştirebilirsiniz. Bu otomasyon, karmaşık IaaS çözümleri sağlamak için ağ ve depolama kaynaklarının sağlanmasını içerir. Çözüm şablonu gereksinimlerine ve faturalandırma modeline genel bakış için Azure [Uygulamaları: çözüm şablonları](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates)bölümüne bakın.

- Yönetilen uygulamalar, tek bir önemli farkla çözüm şablonlarına benzer. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Müşterilerinize tam olarak yönetilen anahtar teslim uygulamalar oluşturmak ve sunmak için Azure Yönetilen uygulamaları kullanın.

Azure Marketi'ne ek olarak, bir hizmet kataloğunda yönetilen uygulamalar da sunabilirsiniz. Hizmet kataloğu, bir kuruluştaki kullanıcılar için onaylı çözümleri içeren bir dahili katalogdur. Bir kuruluştaki gruplar için çözümler sunarken kataloğu kuruluş standartlarını karşılamak için kullanırsınız. Çalışanlar, kataloğu kullanarak BT bölümleri tarafından önerilen ve onaylanan uygulamaları kolayca bulabilir.

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](../../cloud-solution-providers.md) bakın.

Yönetilen uygulamaların avantajları ve türleri hakkında daha fazla bilgi için [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview)bölümüne bakın.


## <a name="publishing-process-workflow"></a>Yayımlama süreci iş akışı

Aşağıdaki diyagram, Azure uygulama teklifini yayımlama işlemine yönelik üst düzey işlemi gösterir.

![Yayımlama teklifi için iş akışı](./media/new-offer-process.png)

Azure uygulama teklifini yayımlamanın üst düzey adımları şunlardır:

1. Ön [koşulları](./cpp-prerequisites.md) karşılayın - (Gösterilmedi) Azure uygulamasını Azure Marketi'nde yayınlamak için iş ve teknik gereksinimleri karşıladığınızı doğrulayın. 

1. [Teklifi oluşturun](./cpp-create-offer.md) - Teklif hakkında ayrıntılı bilgi sağlayın. Bu bilgiler şunları içerir: teklif açıklaması, pazarlama malzemeleri, destek bilgileri ve varlık özellikleri.

1. [Mevcut iş ve teknik varlıkları oluşturun veya topla](./cpp-create-technical-assets.md) - İlişkili çözüm için iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıklar oluşturun.

1. [SKU oluşturun](./cpp-skus-tab.md) - Teklifle ilişkili SKU(lar)ı oluşturun. Yayınlamayı planladığınız her resim için benzersiz bir SKU gereklidir.

1. Teklifi onaylayın ve [yayınlayın](./cpp-publish-offer.md) - Teklif ve teknik varlıklar tamamlandıktan sonra teklifi iletebilirsiniz. Bu gönderme yayımlama işlemini başlatır. Bu işlem sırasında çözüm test edilir, doğrulanır, onaylanır ve Azure Marketi'nde "yayınlanır".

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce, yönetilen bir uygulamayı Microsoft Azure Marketi'nde yayınlamak için [teknik ve iş gereksinimlerini](./cpp-prerequisites.md) karşılamanız gerekir.
