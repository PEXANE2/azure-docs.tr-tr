---
title: Media Services ile kendi anahtarınızı getirin (müşteri tarafından yönetilen anahtarlar)
description: Media Services ile müşteri tarafından yönetilen bir anahtar (kendi anahtarını getir) kullanabilirsiniz.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: a89ff56eb9e0f0a29b5b1fed7543c5f718425e51
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92326089"
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

## <a name="tutorial"></a>Öğretici
Postman ve Azure REST API ile müşteri tarafından yönetilen anahtarları ayarlama ve kullanma hakkında bilgi edinmek için [Media Services ile müşteri tarafından yönetilen anahtarları kullanın veya kendi anahtarınızı (BYOK) alın](tutorial-byok.md) .

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: müşteri tarafından yönetilen anahtarları kullanın veya Media Services ile kendi anahtarınızı (BYOK) getirin](tutorial-byok.md)
