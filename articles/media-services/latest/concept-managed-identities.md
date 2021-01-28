---
title: Yönetilen kimlikler ve güvenilen depolama
description: Media Services, güvenilen depolamayı etkinleştirmek için yönetilen kimliklerle birlikte kullanılabilir.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 11/04/2020
ms.author: inhenkel
ms.openlocfilehash: 291508a6beaa687b3a10f55df4591ce601ab51a0
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98956183"
---
# <a name="managed-identities-and-trusted-storage-with-media-services"></a>Media Services ile yönetilen kimlikler ve güvenilen depolama

Media Services, güvenilen depolamayı etkinleştirmek için [yönetilen kimliklerle](../../active-directory/managed-identities-azure-resources/overview.md) birlikte kullanılabilir. Media Services bir hesap oluşturduğunuzda, bunu bir depolama hesabıyla ilişkilendirmeniz gerekir. Media Services, sistem kimlik doğrulamasını kullanarak bu depolama hesabına erişebilir. Media Services, Media Services hesabının ve depolama hesabının aynı abonelikte olduğunu doğrular ve ilişkilendirmeyi ekleyen kullanıcının depolama hesabına Azure Resource Manager RBAC ile erişebileceğini doğrular.

## <a name="trusted-storage"></a>Güvenilen depolama

Ancak, depolama hesabınızın güvenliğini sağlamak için bir güvenlik duvarı kullanmak istiyorsanız, yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir. Media Services, güvenilen depolama erişimi aracılığıyla bir güvenlik duvarı veya VNet kısıtlaması ile yapılandırılmış depolama hesabına erişmesini sağlar.  Güvenilen Microsoft hizmetleri hakkında daha fazla bilgi için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](../../storage/common/storage-network-security.md#trusted-microsoft-services).

## <a name="media-services-managed-identity-scenarios"></a>Media Services yönetilen kimlik senaryoları

Şu anda yönetilen kimliğin Media Services birlikte kullanılabileceği iki senaryo vardır:

- Depolama hesaplarına erişmek için Media Services hesabının yönetilen kimliğini kullanın.

- Müşteri anahtarlarına erişmek için Key Vault erişmek üzere Media Services hesabının yönetilen kimliğini kullanın.

Sonraki iki bölümde iki senaryodaki farklar açıklanır.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>Depolama hesaplarına erişmek için Media Services hesabının yönetilen kimliğini kullanın

1. Yönetilen kimliğe sahip bir Media Services hesabı oluşturun.
1. Yönetilen kimlik sorumlusu erişimini sahip olduğunuz bir depolama hesabına verin.
1. Media Services, yönetilen kimliği kullanarak sizin adınıza depolama hesabına erişebilir.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>Müşteri anahtarlarına erişmek için Key Vault erişmek üzere Media Services hesabının yönetilen kimliğini kullanın

1. Yönetilen kimliğe sahip bir Media Services hesabı oluşturun.
1. Yönetilen kimlik sorumlusuna sahip olduğunuz bir Key Vault için erişim izni verin.
1. Media Services hesabını, müşteri anahtar tabanlı hesap şifrelemesini kullanacak şekilde yapılandırın.
1. Media Services yönetilen kimliği kullanarak adınıza Key Vault erişir.

Müşteri tarafından yönetilen anahtarlar ve Key Vault hakkında daha fazla bilgi için, bkz. [kendi anahtarını getirme (müşteri tarafından yönetilen anahtarlar) Media Services](concept-use-customer-managed-keys-byok.md)

## <a name="tutorials"></a>Öğreticiler

Bu öğreticiler, yukarıda belirtilen senaryoların her ikisini de içerir.

- [Azure portal, müşteri tarafından yönetilen anahtarları veya BYOK 'u Media Services ile birlikte kullanmak için kullanın](tutorial-byok-portal.md)
- [Media Services REST API ile müşteri tarafından yönetilen anahtarları veya BYOK kullanın](tutorial-byok-postman.md).

## <a name="next-steps"></a>Sonraki adımlar

Sizin ve Azure uygulamalarınız için hangi yönetilen kimliklerin yapabilecekleri hakkında daha fazla bilgi edinmek için bkz. [Azure ad yönetilen kimlikleri](../../active-directory/managed-identities-azure-resources/overview.md).