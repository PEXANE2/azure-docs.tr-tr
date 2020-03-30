---
title: Azure IoT Edge Modülleri
description: Uygulama ve hizmet yayıncıları için Azure Marketi'ndeki IoT Edge Modülü Teklifi.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/22/2018
ms.author: dsindona
ms.openlocfilehash: aadbf33914f919e393a5ec88cf6fc0a6103911b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286158"
---
# <a name="iot-edge-modules"></a>IoT Edge modülleri

[Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) platformu Azure Bulut tarafından desteklenen bir platformdur.  Bu platform, kullanıcıların doğrudan IoT aygıtlarında çalışacak bulut iş yüklerini dağıtmalarına olanak tanır.  Bir IoT Edge modülü çevrimdışı iş yüklerini çalıştırabilir ve yerel olarak veri analizi yapabilir. Bu teklif türü bant genişliğinden tasarruf etmeye, yerel ve hassas verileri korumaya yardımcı olur ve düşük gecikmeli yanıt süresi sunar.  Artık bu önceden oluşturulmuş iş yüklerinden yararlanmak için seçeneklere sahipsiniz. Şimdiye kadar, Microsoft'un yalnızca bir avuç birinci taraf çözümü mevcuttü.  Kendi özel IoT çözümlerinizi oluşturmak için zaman ve kaynak yatırımı yapmak zorundaydınız.

[Azure Marketi'nde IoT Edge modüllerini](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1)tanıtarak, artık yayıncıların çözümlerini IoT kitlesine sunması ve satması için tek bir varış noktamız var. IoT geliştiricileri, çözüm geliştirmeçalışmalarını hızlandırmak için sonuçta yetenekleri bulabilir ve satın alabilir.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Azure Marketi'ndeki IoT Edge modüllerinin temel avantajları:

| **Yayıncılar için**    | **Müşteriler için (IoT geliştiricileri)**  |
| :------------------- | :-------------------|
| IoT Edge çözümleri oluşturmak ve dağıtmak isteyen milyonlarca geliştiriciye ulaşın.  | Güvenli ve test edilmiş bileşenler kullanma nın güveniyle bir IoT Edge çözümü oluşturun. |
| Bir kez yayımlayın ve kapsayıcıları destekleyen herhangi bir IoT Edge donanımı arasında çalıştırın. | Belirli ihtiyaçlar için 1. ve 3. |
| Esnek faturalandırma seçenekleriyle para kazanma <ul> <li> Ücretsiz ve Kendi Lisans (BYOL) getirin. </li> </ul> | Seçtiğiniz faturalandırma modelleri ile alışveriş yapın. <ul> <li> Ücretsiz ve Kendi Lisans (BYOL) getirin. </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>IoT Edge modülü nedir?

Azure IoT Edge, modül ler biçiminde iş mantığını kenarda dağıtmanızı ve yönetmenize olanak tanır. Azure IoT Edge modülleri, IoT Edge tarafından yönetilen en küçük hesaplama birimleridir ve Microsoft hizmetlerini (Azure Akış Analizi gibi), üçüncü taraf hizmetleri veya çözüme özel kodunuzu içerebilir. IoT Edge modülleri hakkında daha fazla bilgi edinmek için Azure [IoT Edge modüllerini anlayın.](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules)

**Konteyner teklif türü ile IoT Edge modül teklif türü arasındaki fark nedir?**

IoT Edge modül teklif türü, Bir IoT Edge aygıtında çalışan belirli bir kapsayıcı türüdür. IoT Edge bağlamında çalışmak için varsayılan yapılandırma ayarlarıyla birlikte gelir ve isteğe bağlı olarak IoT Edge modülü SDK'yı IoT Edge çalışma süresiyle entegre etmek için kullanır.

## <a name="publishing-your-iot-edge-module"></a>IoT Edge modülünüzü yayımlama

**Doğru vitrini seçme**

IoT Edge Modülleri yalnızca Azure Marketi'nde yayınlanır, AppSource geçerli değildir.  Farklar ve vitrinler arasında hedef kitle hakkında daha fazla bilgi [için, çözümünüz için yayımlama seçeneğini belirlemeye](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type)bakın.
 
**Faturalama seçenekleri**

Pazar şu anda IoT Edge modülleri için **Ücretsiz** ve **Kendi Lisansını Getir (BYOL)** fatura seçeneklerini desteklemektedir.
 
**Yayımlama seçenekleri**

Her durumda, IoT Edge modülleri **Transact** yayımlama seçeneğini seçmelidir.  Yayımlama seçenekleri hakkında daha fazla ayrıntı için [bir yayımlama seçeneği seçin.](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type)  

## <a name="eligibility-criteria"></a>Uygunluk Kriterleri

Microsoft Azure Marketi anlaşmalarının ve ilkelerinin tüm koşulları IoT Edge modül teklifleri için geçerlidir.  Ayrıca, IoT Edge modülleri için ön koşullar ve teknik gereksinimler vardır.  

**Ön koşul**

Azure Marketi'nde bir IoT Edge modülü yayınlamak için aşağıdaki ön koşulları karşılamanız gerekir:

- Bulut İş Ortağı Portalına (CPP) erişim. Daha fazla bilgi için [Azure Marketi ve AppSource yayımlama kılavuzuna](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide)bakın.
- IoT Edge modülünüzü bir Azure Kapsayıcı Kayıt Defteri'nde barındırın. 
- IoT Edge modül meta verilerinizin (kapsamlı olmayan liste) gibi hazır olması: 
    - Bir başlık
    - Açıklama (HTML biçiminde)
    - Logo görüntüsü (PNG formatı ve 40x40px, 90x90px, 115x115px, 255x115px dahil sabit görüntü boyutları)
    - Kullanım süresi ve gizlilik politikası
    - Varsayılan modül yapılandırması (rota, istenen ikiz özellikler, createOptions, ortam değişkenleri)
    - Belgeler
    - Destek kişileri

**Teknik Gereksinimler**

Bir IoT Edge Modülü için temel teknik gereksinimler, azure marketinde sertifikalandırılabilmesi ve yayımlanabilmesi için [IoT Edge modülü teknik varlıklarınızı hazırla'da](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets)ayrıntılı olarak açıklanmaktadır.  

## <a name="documentation-and-resources"></a>Dokümantasyon ve Kaynaklar

Bulut Yayıncılık Portalı ile yeni bir IoT Edge modülü teklifi yayınlama adımları olan [Bir IoT Edge modülü teklifi oluşturun.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-offer)

## <a name="next-steps"></a>Sonraki adımlar

Eğer bunu yapmadıysanız,

- Pazara [kaydolun.](https://azuremarketplace.microsoft.com/sell)

Kayıtlıysanız ve yeni bir teklif oluşturuyorsanız veya varolan bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlamak için [Cloud İş Ortağı Portalı'nda](https://cloudpartner.azure.com/) oturum açın.
- Bkz. [IoT Edge modülü, Bir IoT](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) Edge modülü teklifinin nasıl yayımlanabileceği hakkında bilgi için yayımlama genel görünümü sunar.
