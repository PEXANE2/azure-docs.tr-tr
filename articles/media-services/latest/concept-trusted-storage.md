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
ms.openlocfilehash: fd92eed127ec50a3d3a86f667d9aa764b79c190a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585402"
---
# <a name="trusted-storage-for-media-services"></a>Media Services için güvenilen depolama

Media Services bir hesap oluşturduğunuzda, bunu bir depolama hesabıyla ilişkilendirmeniz gerekir. Media Services, sistem kimlik doğrulamasını veya yönetilen kimlik doğrulamasını kullanarak bu depolama hesabına erişebilir. Media Services, Media Services hesabının ve depolama hesabının aynı abonelikte olduğunu doğrular ve ilişkilendirmeyi ekleyen kullanıcının depolama hesabına Azure Resource Manager RBAC ile erişebileceğini doğrular.

>[!NOTE]
>Güvenilen depolama yalnızca API 'de kullanılabilir ve Azure portal Şu anda etkin değil.

## <a name="trusted-storage-with-a-firewall"></a>Güvenlik duvarıyla güvenilen depolama

Ancak, depolama hesabınızı güvenli hale getirmek ve güvenilen depolamayı etkinleştirmek için bir güvenlik duvarı kullanmak istiyorsanız, [Yönetilen kimlikler](concept-managed-identities.md) kimlik doğrulaması tercih edilen seçenektir. Media Services, güvenilen depolama erişimi aracılığıyla bir güvenlik duvarı veya VNet kısıtlaması ile yapılandırılmış depolama hesabına erişmesini sağlar.

## <a name="tutorial"></a>Öğretici

Güvenilen depolamayı [Media Services güvenilir depolama](tutorial-trusted-storage-rest.md) öğreticisi ile etkinleştirme hakkında daha fazla bilgi edinebilirsiniz.

> [!NOTE]
> Media Services depolama hesabına okuyup yazabilmesi için AMS tarafından yönetilen kimlik Depolama Blobu veri katılımcısı erişimine izin vermeniz gerekir.  Genel katkıda bulunan rolünün verilmesi, veri düzleminde doğru izinleri etkinleştirmediğinden çalışmaz.

## <a name="further-reading"></a>Daha fazla bilgi

Yönetilen kimliklerle güvenilir depolama oluşturma yöntemlerini anlamak için, [Yönetilen kimlikler ve Media Services](concept-managed-identities.md)okuyun.

Güvenilen Microsoft hizmetleri hakkında daha fazla bilgi için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](../../storage/common/storage-network-security.md#trusted-microsoft-services).

## <a name="next-steps"></a>Sonraki adımlar

Sizin ve Azure uygulamalarınız için hangi yönetilen kimliklerin yapabilecekleri hakkında daha fazla bilgi edinmek için bkz. [Azure ad yönetilen kimlikleri](../../active-directory/managed-identities-azure-resources/overview.md).
