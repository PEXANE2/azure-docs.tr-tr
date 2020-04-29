---
title: Azure IoT Edge modül teklifi yayımlamaya genel bakış | Azure Marketi
description: Azure Marketi 'nde IoT Edge modülü teklifini yayımlama sürecine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: e9116e5cdb3bd9ed61205ceabd4d51c96c6aadc2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82144663"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>IoT Edge modül teklifi yayımlamaya genel bakış

>[!Important]
>13 Nisan 2020 ' den itibaren, IoT Edge modülü tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [IoT Edge modülü oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) bölümündeki yönergeleri izleyin.

<table> <tr> <td>Bu bölümde, Microsoft <a href="https://azuremarketplace.microsoft.com">Azure Marketi</a>'ne yeni bir Azure IoT Edge modülü teklifinin nasıl yayımlanacağı açıklanmaktadır. IoT Edge modülü, bir IoT Edge cihazında çalıştırılmak üzere yapılan Docker ile uyumlu bir kapsayıcıdır. Azure IoT Edge modüller, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure hizmetleri veya özel çözüm kodu içerebilir. </td> <td><img src="./media/iotedge-icon1.png"  alt="Azure IoT Edge module icon" /></td> </tr> </table>

## <a name="publishing-process"></a>Yayımlama işlemi

IoT Edge modülü teklifinin yayımlanması için üst düzey adımlar şunlardır:

1. Teklifi oluşturma<br> Teklif hakkında ayrıntılı bilgi sağlayın. Bu bilgiler şunları içerir: teklif açıklaması, pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri.

2. İşletme ve teknik varlıkları oluşturma<br> İlişkili çözüme yönelik iş varlıklarını (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıkları (bir Azure Container Registry barındırılan IoT Edge modül görüntüleri) oluşturun.

3. SKU 'YU oluşturma<br> Teklifle ilişkili SKU 'ları oluşturun. Yayımlamayı planladığınız her görüntü için benzersiz bir SKU gereklidir.

4. Teklifi onaylama ve yayımlama <br>Teklif ve teknik varlıklar tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderim, yayımlama işlemini başlatır. Bu işlem sırasında çözüm test edildi, onaylanır, onaylandı ve sonra Azure Marketi 'nde "canlı gidiyor".

## <a name="parts-of-an-offer"></a>Teklifin parçaları

Aşağıdaki makaleler bir IoT Edge modülü teklifinin temel kısımlarını kapsar.

- [Ön koşullar](./cpp-prerequisites.md) <br>Bu makalede, IoT Edge bir modül teklifi oluşturabilmeniz veya yayımlayabilmeniz için teknik ve iş gereksinimleri listelenmektedir.
- [IoT Edge modülü teknik varlıklarını hazırlama](./cpp-create-technical-assets.md) <br>Bu makalede, teknik varlıkların bir IoT Edge modülü için nasıl hazırlanacağı açıklanmaktadır. Bu varlıkların, IoT Edge modülünün Azure Marketi 'nde yayımlanabilmesi için gereken tüm teknik kriterleri karşılaması gerekir.
- [IoT Edge modül teklifi oluşturma](./cpp-create-offer.md) <br>Bu makalede, [bulut iş ortağı portalı](https://cloudpartner.azure.com)kullanarak yeni bir IoT Edge modülü teklif girişi oluşturmak için gereken adımlar listelenmektedir.
- [IoT Edge modülü teklifini yayımlama](./cpp-publish-offer.md)<br> Bu makalede, teklifi Azure Marketi 'nde yayımlama için nasıl göndereceğiniz açıklanır.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Azure Market IoT Edge modülünü yayımlamaya yönelik [Teknik ve iş gereksinimlerini](./cpp-prerequisites.md) gözden geçirin.
