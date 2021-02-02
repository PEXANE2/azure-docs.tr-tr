---
title: Yönetilen kimlikler
description: Media Services, Azure yönetilen kimlikleri ile kullanılabilir.
keywords: ''
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: inhenkel
ms.openlocfilehash: 71a2b8f0734de80f71dbb2372f8600b464d6c606
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99258448"
---
# <a name="managed-identities"></a>Yönetilen kimlikler

Geliştiriciler için yaygın bir zorluk, farklı hizmetler arasındaki iletişimin güvenliğini sağlamak için gizli dizileri ve kimlik bilgilerini yönetiledir. Azure 'da Yönetilen kimlikler, Azure AD 'de Azure kaynağı için bir kimlik sağlayarak ve Azure Active Directory (Azure AD) belirteçleri elde etmek üzere kullanarak kimlik bilgilerini yönetmek zorunda olan geliştiricilerin gereksinimini ortadan kaldırır.

Şu anda yönetilen kimliklerin Media Services birlikte kullanılabileceği iki senaryo vardır:

- Depolama hesaplarına erişmek için Media Services hesabının yönetilen kimliğini kullanın.

- Müşteri anahtarlarına erişmek için Key Vault erişmek üzere Media Services hesabının yönetilen kimliğini kullanın.

Sonraki iki bölümde iki senaryonun adımları açıklanır.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>Depolama hesaplarına erişmek için Media Services hesabının yönetilen kimliğini kullanın

1. Yönetilen kimliğe sahip bir Media Services hesabı oluşturun.
1. Yönetilen kimlik sorumlusu erişimini sahip olduğunuz bir depolama hesabına verin.
1. Media Services, yönetilen kimliği kullanarak sizin adınıza depolama hesabına erişebilir.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>Müşteri anahtarlarına erişmek için Key Vault erişmek üzere Media Services hesabının yönetilen kimliğini kullanın

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
