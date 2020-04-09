---
title: Azure IoT Edge Modülü yayımlama genel bakış | Azure Marketi
description: Azure Marketi'nde bir IoT Edge modülü teklifi yayımlama işlemine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 20c76cec82944568c1b16694bef2838626b90b03
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983356"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>IoT Edge modülü yayımlama genel bakış teklifi

>[!Important]
>13 Nisan 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureCreateIoT) yönergeleri izleyin.

<table> <tr> <td>Bu bölümde, Microsoft Azure Marketi'nde yeni bir <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>Azure IoT Edge modülü teklifinin nasıl yayımlanabileceği açıklanmaktadır. IoT Edge modülü, Docker uyumlu bir kapsayıcıdır ve IoT Edge aygıtında çalışmak üzere üretilmiştir. Azure IoT Edge modülleri, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure hizmetleri veya özel çözüm kodu içerebilir. </td> <td><img src="./media/iotedge-icon1.png"  alt="Azure IoT Edge module icon" /></td> </tr> </table>

## <a name="publishing-process"></a>Yayımlama süreci

IoT Edge modül teklifini yayınlamak için üst düzey adımlar şunlardır:

1. Teklifi oluşturma<br> Teklif hakkında ayrıntılı bilgi sağlayın. Bu bilgiler şunları içerir: teklif açıklaması, pazarlama malzemeleri, destek bilgileri ve varlık özellikleri.

2. İş ve teknik varlıkları oluşturma<br> İlişkili çözüm için iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıkları (Azure Konteyner Kayıt Defteri'nde barındırılan IoT Edge modül görüntüleri) oluşturun.

3. SKU'yu oluşturun<br> Teklifle ilişkili SKU(lar)ı oluşturun. Yayınlamayı planladığınız her resim için benzersiz bir SKU gereklidir.

4. Teklifi onaylamak ve yayınlamak <br>Teklif ve teknik varlıklar tamamlandıktan sonra, teklifi gönderebilirsiniz. Bu gönderme yayımlama işlemini başlatır. Bu işlem sırasında çözüm test edilir, doğrulanır, onaylanır ve Azure Marketi'nde "yayınlanır".

## <a name="parts-of-an-offer"></a>Teklifin bölümleri

Aşağıdaki makaleler, IoT Edge modül teklifinin önemli bölümlerini kapsamaktadır.

- [Ön koşullar](./cpp-prerequisites.md) <br>Bu makalede, bir IoT Edge modülü teklifi oluşturmadan veya yayımlamadan önce teknik ve iş gereksinimleri listeleilmektedir.
- [IoT Edge modülü teknik varlıklarını hazırlama](./cpp-create-technical-assets.md) <br>Bu makalede, bir IoT Edge modülü için teknik varlıkların nasıl hazırlanacağı açıklanmaktadır. IoT Edge modülüAzure Marketi'nde yayımlanabilmesi için bu varlıkların gerekli tüm teknik ölçütleri karşılaması gerekir.
- [IoT Edge modül teklifi oluşturma](./cpp-create-offer.md) <br>Bu makalede, [Bulut İş Ortağı Portalı](https://cloudpartner.azure.com)kullanarak yeni bir IoT Edge modülü teklif girişi oluşturmak için gereken adımlar listelemektedir.
- [IoT Edge modülü teklifini yayımlama](./cpp-publish-offer.md)<br> Bu makalede, Azure Marketi'nde yayımlanmak üzere teklifin nasıl gönderilen açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge modüllerini Microsoft Azure Marketi'nde yayımlamak için [gereken teknik ve iş gereksinimlerini](./cpp-prerequisites.md) gözden geçirin.
