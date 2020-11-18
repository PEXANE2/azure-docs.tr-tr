---
title: Media Services ile yönetilen kimlikler ve güvenilen depolama
description: Media Services, güvenilen depolamayı etkinleştirmek için yönetilen kimliklerle birlikte kullanılabilir.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 11/04/2020
ms.author: inhenkel
ms.openlocfilehash: a0ded2a1ad5cd590ab5715edebde2ab19e399e8d
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687113"
---
# <a name="managed-identities-and-trusted-storage-with-media-services"></a>Media Services ile yönetilen kimlikler ve güvenilen depolama

Media Services, güvenilen depolamayı etkinleştirmek için [yönetilen kimliklerle](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) birlikte kullanılabilir. Media Services bir hesap oluşturduğunuzda, bunu bir depolama hesabıyla ilişkilendirmeniz gerekir. Media Services, sistem kimlik doğrulamasını kullanarak bu depolama hesabına erişebilir. Media Services, Media Services hesabının ve depolama hesabının aynı abonelikte olduğunu doğrular ve ilişkilendirmeyi ekleyen kullanıcının depolama hesabına Azure Resource Manager RBAC ile erişebileceğini doğrular.

## <a name="trusted-storage"></a>Güvenilen depolama

Ancak, depolama hesabınızın güvenliğini sağlamak için bir güvenlik duvarı kullanmak istiyorsanız, yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir. Media Services, güvenilen depolama erişimi aracılığıyla bir güvenlik duvarı veya VNet kısıtlaması ile yapılandırılmış depolama hesabına erişmesini sağlar.  Güvenilen Microsoft hizmetleri hakkında daha fazla bilgi için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services).

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

Sizin ve Azure uygulamalarınız için hangi yönetilen kimliklerin yapabilecekleri hakkında daha fazla bilgi edinmek için bkz. [Azure ad yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
