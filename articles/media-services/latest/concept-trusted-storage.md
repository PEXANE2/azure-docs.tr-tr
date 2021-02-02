---
title: Media Services için güvenilen depolama
description: Yönetilen kimlikler kimlik doğrulaması Media Services, güvenilen depolama erişimi aracılığıyla bir güvenlik duvarı veya VNet kısıtlaması ile yapılandırılmış depolama hesabına erişmesini sağlar.
keywords: Güvenilen depolama, Yönetilen kimlikler
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: inhenkel
ms.openlocfilehash: 59c1eb7936bc113f8935d6fa2ad378c6994c3ca9
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99408528"
---
# <a name="trusted-storage-for-media-services"></a>Media Services için güvenilen depolama

Media Services bir hesap oluşturduğunuzda, bunu bir depolama hesabıyla ilişkilendirmeniz gerekir. Media Services, sistem kimlik doğrulamasını kullanarak bu depolama hesabına erişebilir. Media Services, Media Services hesabının ve depolama hesabının aynı abonelikte olduğunu doğrular ve ilişkilendirmeyi ekleyen kullanıcının depolama hesabına Azure Resource Manager RBAC ile erişebileceğini doğrular.

Ancak, depolama hesabınızı güvenli hale getirmek ve güvenilen depolamayı etkinleştirmek için bir güvenlik duvarı kullanmak istiyorsanız, [Yönetilen kimlikler](concept-managed-identities.md) kimlik doğrulamasını kullanmanız gerekir. Media Services, güvenilen depolama erişimi aracılığıyla bir güvenlik duvarı veya VNet kısıtlaması ile yapılandırılmış depolama hesabına erişmesini sağlar.

Yönetilen kimliklerle güvenilir depolama oluşturma yöntemlerini anlamak için, [Yönetilen kimlikler ve Media Services](concept-managed-identities.md)okuyun.

Müşteri tarafından yönetilen anahtarlar ve Key Vault hakkında daha fazla bilgi için, bkz. [kendi anahtarını getirme (müşteri tarafından yönetilen anahtarlar) Media Services](concept-use-customer-managed-keys-byok.md)

Güvenilen Microsoft hizmetleri hakkında daha fazla bilgi için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](../../storage/common/storage-network-security.md#trusted-microsoft-services).

## <a name="tutorials"></a>Öğreticiler

Bu öğreticiler, yukarıda belirtilen senaryoların her ikisini de içerir.

- [Azure portal, müşteri tarafından yönetilen anahtarları veya BYOK 'u Media Services ile birlikte kullanmak için kullanın](tutorial-byok-portal.md)
- [Media Services REST API ile müşteri tarafından yönetilen anahtarları veya BYOK kullanın](tutorial-byok-postman.md).

## <a name="next-steps"></a>Sonraki adımlar

Sizin ve Azure uygulamalarınız için hangi yönetilen kimliklerin yapabilecekleri hakkında daha fazla bilgi edinmek için bkz. [Azure ad yönetilen kimlikleri](../../active-directory/managed-identities-azure-resources/overview.md).