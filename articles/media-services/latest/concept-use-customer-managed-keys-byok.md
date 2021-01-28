---
title: Kendi anahtarınızı getirin (müşteri tarafından yönetilen anahtarlar)
description: Media Services ile müşteri tarafından yönetilen bir anahtar (kendi anahtarını getir) kullanabilirsiniz.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: c11bb0e369075c3024824d65290e8aecfa68e6f4
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98956149"
---
# <a name="bring-your-own-key-customer-managed-keys-with-media-services"></a>Media Services ile kendi anahtarınızı getirin (müşteri tarafından yönetilen anahtarlar)

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Kendi Anahtarını Getir (BYOK), müşterilerin iş yüklerini buluta taşımasına yardımcı olan bir Azure genelindeki girişimdir. Müşteri tarafından yönetilen anahtarlar müşterilerin sektör uyumluluğu düzenlemelerine bağlı kalmalarını sağlar ve bir hizmetin kiracı yalıtımını geliştirir. Kullanıcılara şifreleme anahtarları denetimi sağlamak, Microsoft hizmetlerinde gereksiz erişimi ve denetimi ve güvenle derlemeyi en aza indirmenin bir yoludur.

## <a name="keys-and-key-management"></a>Anahtarlar ve anahtar yönetimi

Media Services 2020-05-01 API 'sini kullandığınızda kendi anahtarınızı Media Services kullanabilirsiniz. Media Services sahip bir sistem anahtarı tarafından şifrelenen tüm hesaplar için varsayılan hesap anahtarı oluşturulur. Kendi anahtarınızı kullandığınızda, hesap anahtarı anahtarınızla şifrelenir. İçerik anahtarları hesap anahtarı tarafından şifrelenir. Jobınputhttp URL 'leri ve simetrik belirteç doğrulama anahtarları da şifrelenir.

:::image type="content" source="./media/customer-managed-key/customer-managed-key.svg" alt-text="Müşteri tarafından yönetilen anahtar, sistem tarafından yönetilen anahtarın yerini alır":::

Media Services, size ait bir Key Vault anahtarınızı okumak için Media Services hesabının yönetilen kimliğini kullanır. Media Services, Key Vault hesap ile aynı bölgede olmasını ve geçici silme ve Temizleme korumasının etkinleştirilmesini gerektirir.

Anahtarınız 2048, 3072 veya 4096 RSA anahtarı olabilir ve hem HSM hem de yazılım anahtarları desteklenir.

> [!NOTE]
> EC anahtarları desteklenmez.

Anahtar adı ve anahtar sürümü ya da yalnızca anahtar adı belirtebilirsiniz. Yalnızca bir anahtar adı kullandığınızda Media Services en son anahtar sürümünü kullanacaktır. Müşteri anahtarlarının yeni sürümleri otomatik olarak algılanır ve hesap anahtarı yeniden şifrelenir.

> [!WARNING]
> Media Services müşteri anahtarına erişimi izler. Müşteri anahtarı erişilemez hale gelirse (örneğin, anahtar silinmişse veya Key Vault silinmişse veya erişim izni kaldırılmışsa), Media Services hesabı müşteri anahtarına erişilemez duruma geçirecek (hesabı etkin bir şekilde devre dışı bırakır). Ancak, hesap bu durumda silinebilir. Yalnızca hesap al, LISTELE ve SIL işlemleri desteklenir; Hesap anahtarına erişim geri yüklenene kadar diğer tüm istekler (kodlama, akış vb.) başarısız olur.

## <a name="tutorials"></a>Öğreticiler

- [Azure portal, müşteri tarafından yönetilen anahtarları veya BYOK 'u Media Services ile birlikte kullanmak için kullanın](tutorial-byok-portal.md)
- [Media Services REST API ile müşteri tarafından yönetilen anahtarları veya BYOK kullanın](tutorial-byok-postman.md).

## <a name="next-steps"></a>Sonraki adımlar

[Media Services dinamik şifreleme ile içeriğinizi koruyun](content-protection-overview.md)
