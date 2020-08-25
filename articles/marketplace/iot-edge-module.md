---
title: Azure Marketi IoT Edge modülü teklifleri
description: Azure Marketi 'nde IoT Edge modülü tekliflerini yayımlama hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 04/15/2020
ms.openlocfilehash: f7a4997117c8730b767f245050caa3f3157fb6fd
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750251"
---
# <a name="iot-edge-modules"></a>IoT Edge modülleri

[Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) platformu Microsoft Azure tarafından desteklenir.  Bu platform, kullanıcıların doğrudan IoT cihazlarında çalışacak bulut iş yüklerini dağıtmasına olanak sağlar.  IoT Edge bir modül çevrimdışı iş yüklerini çalıştırabilir ve veri analizini yerel olarak gerçekleştirebilir. Bu teklif türü, bant genişliğini kaydetmeye, yerel ve hassas verilerin korunmasına yardımcı olur ve düşük Gecikmeli yanıt süresi sağlar.  Artık önceden oluşturulmuş bu iş yüklerinden faydalanmak için seçenekleriniz vardır. Bu aşamada, Microsoft 'tan yalnızca birkaç birinci taraf çözüm kullanıma sunulmuştur.  Kendi özel IoT çözümlerinizi oluşturmak için zaman ve kaynakları yatırım yapmanız gerekiyordu.

[Azure Marketi 'nde IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1)sayesinde, artık yayımcıların IoT hedef kitlesi için çözümlerini kullanıma sunma ve satma için tek bir hedef sunuyoruz. IoT geliştiricileri, son olarak çözüm geliştirmeyi hızlandırmaya yönelik yetenekler bulabilir ve satın alabilir.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Azure Marketi 'nde IoT Edge modüllerinin önemli avantajları

| **Yayımcılar için**    | **Müşteriler için (IoT geliştiricileri)**  |
| :------------------- | :-------------------|
| IoT Edge çözümleri derlemek ve dağıtmak isteyen milyonlarca geliştiricilere ulaşın.  | Güvenli ve test edilmiş bileşenleri kullanma güveniyle bir IoT Edge çözümü oluşturun. |
| Bir kez yayımlayın ve kapsayıcıları destekleyen tüm IoT Edge donanımları üzerinde çalıştırın. | Belirli gereksinimlere göre 1. ve 3. taraf IoT Edge modüllerini bularak ve dağıtarak pazarlamaya yönelik süreyi azaltın. |
| Esnek faturalandırma seçenekleriyle monetize <ul> <li> Ücretsiz ve kendi lisansınızı getirin (KLG). </li> </ul> | Satın alımları, fatura modellerinizi tercih ettiğiniz şekilde yapın. <ul> <li> Ücretsiz ve kendi lisansınızı getirin (KLG). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>IoT Edge modülü nedir?

Azure IoT Edge, çalışma mantığını uç üzerinde modüller biçiminde dağıtmanıza ve yönetmenize olanak tanır. Azure IoT Edge modüller, IoT Edge tarafından yönetilen en düşük hesaplama birimleridir ve Microsoft Hizmetleri (Azure Stream Analytics gibi), 3. taraf hizmetleri veya kendi çözüme özgü kodunuzla bulunabilir. IoT Edge modüller hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge modülleri anlama](../iot-edge/iot-edge-modules.md).

**Kapsayıcı teklif türü ve IoT Edge modülü teklif türü arasındaki fark nedir?**

IoT Edge modülü teklif türü, bir IoT Edge cihazında çalışan belirli bir kapsayıcı türüdür. IoT Edge bağlamında çalıştırılacak varsayılan yapılandırma ayarlarıyla birlikte gelir ve isteğe bağlı olarak IoT Edge çalışma zamanına tümleştirilecek IoT Edge modülü SDK 'sını kullanır.

## <a name="publishing-your-iot-edge-module"></a>IoT Edge modülünüzü yayımlama

**Doğru çevrimiçi mağazayı seçme**

IoT Edge modüller yalnızca Azure Marketi 'Nde yayımlanır, AppSource uygulanmaz.  Çevrimiçi mağazalardaki farklılıklar ve hedef kitleler hakkında daha fazla bilgi için bkz. [Yayımlama seçeneğinizi belirleme](determine-your-listing-type.md).
 
**Faturalandırma seçenekleri**

Market şu anda **ücretsiz** olarak destekler ve IoT Edge modüller Için **kendi LISANSıNı getir (KLG)** faturalandırma seçenekleridir.
 
**Yayımlama seçenekleri**

Her durumda IoT Edge modüller **Transact** yayımlama seçeneğini seçmelidir.  Yayımlama seçenekleri hakkında daha fazla bilgi için bkz. [yayımlama seçeneği seçme](determine-your-listing-type.md) .  

## <a name="eligibility-criteria"></a>Uygunluk ölçütleri

Microsoft Azure Market sözleşmelerinin ve ilkelerin tüm koşulları IoT Edge modül teklifleri için geçerlidir.  Ayrıca, IoT Edge modüller için önkoşulları ve teknik gereksinimleri vardır.  

**Önkoşulların önkoşulları**

Azure Marketi 'Nde bir IoT Edge modülünü yayımlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Iş Ortağı Merkezi 'ne erişin. Daha fazla bilgi için bkz. [Azure Marketi ve AppSource Yayımlama Kılavuzu](marketplace-publishers-guide.md).
- IoT Edge modülünüzü bir Azure Container Registry barındırın. 
- IoT Edge modülünüzün meta verileri (ayrıntılı olmayan liste) için hazırlayın: 
    - Bir başlık
    - Bir açıklama (HTML biçiminde)
    - Logo resmi (PNG biçimi ve 40x40px, 90x90px, 115x115px, 255x115px dahil)
    - Kullanım Koşulları ve Gizlilik ilkesi
    - Varsayılan modül yapılandırması (Route, ikizi istenen özellikler, createOptions, ortam değişkenleri)
    - Belgeler
    - Destek kişileri

**Teknik gereksinimler**

Azure Market 'te sertifikalı ve yayımlanmış olması için bir IoT Edge modülü için birincil teknik gereksinimler, [IoT Edge modülü teknik varlıklarınızı hazırlama](./partner-center-portal/create-iot-edge-module-asset.md)bölümünde ayrıntılı olarak açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

- Ticari Market hakkında [bilgi edinin](https://azuremarketplace.microsoft.com/sell) .
- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) ' nde oturum açın.
- Iş Ortağı Merkezi 'nde [bir IoT Edge modülü teklifi oluşturun](./partner-center-portal/azure-iot-edge-module-creation.md) .
