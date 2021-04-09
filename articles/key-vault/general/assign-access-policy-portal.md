---
title: Azure Key Vault erişim ilkesi atama (portal)
description: Bir hizmet sorumlusuna veya uygulama kimliğine Key Vault erişim ilkesi atamak için Azure portal nasıl kullanılır.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 910b8dae10036cc2e396be13495fd28363dc971d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97934569"
---
# <a name="assign-a-key-vault-access-policy-using-the-azure-portal"></a>Azure portal kullanarak Key Vault erişim ilkesi atama

Key Vault erişim ilkesi, belirli bir hizmet sorumlusunun bir uygulama veya Kullanıcı grubu tarafından Key Vault [gizli](../secrets/index.yml)diziler, [anahtarlar](../keys/index.yml)ve [Sertifikalar](../certificates/index.yml)üzerinde farklı işlemler yapıp gerçekleştiremeyeceğini belirler. Azure portal (Bu makale), [Azure CLI](assign-access-policy-cli.md)veya [Azure PowerShell](assign-access-policy-powershell.md)kullanarak erişim ilkeleri atayabilirsiniz.

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Azure portal üzerinden Azure Active Directory Grup oluşturma hakkında daha fazla bilgi için bkz. [temel Grup oluşturma ve üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

## <a name="assign-an-access-policy"></a>Erişim ilkesi atama

1.  [Azure portal](https://portal.azure.com)Key Vault kaynağına gidin. 

1.  **Ayarlar** altında **erişim ilkeleri**' ni seçin ve ardından **erişim ilkesi Ekle**' yi seçin:

    ![Erişim ilkeleri ' ni seçin, rol ataması Ekle seçeneğini belirleyin](../media/authentication/assign-policy-portal-01.png)

1.  **Sertifika izinleri**, **anahtar izinleri** ve **gizli izinler** altında istediğiniz izinleri seçin. Ayrıca, ortak izin birleşimlerini içeren bir şablon seçebilirsiniz:

    ![Erişim ilkesi izinlerini belirtme](../media/authentication/assign-policy-portal-02.png)

1. **Asıl seçin**' in altında, **Seçili** değil bağlantısını seçerek **sorumlu** seçim bölmesini açın. Arama alanına uygulama veya hizmet sorumlusu adını girin, uygun sonucu seçin ve ardından **Seç**' i seçin.

    ![Erişim ilkesi için hizmet sorumlusu seçme](../media/authentication/assign-policy-portal-03.png)

    Uygulama için yönetilen bir kimlik kullanıyorsanız, uygulamanın adını arayıp seçin. (Yönetilen kimlik ve hizmet sorumluları hakkında daha fazla bilgi için bkz. [Key Vault kimlik doğrulaması-uygulama kimliği ve hizmet sorumluları](authentication.md#app-identity-and-security-principals).)
 
1.  **Erişim Ilkesi Ekle** bölmesine geri dönün, erişim ilkesini kaydetmek için **Ekle** ' yi seçin.

    ![Erişim ilkesini atanan hizmet sorumlusu ile ekleme](../media/authentication/assign-policy-portal-04.png)

1. **Erişim ilkeleri** sayfasına geri döndüğünüzde, erişim Ilkenizin **Geçerli erişim ilkeleri** altında listelendiğini doğrulayın ve **Kaydet**' i seçin. Erişim ilkeleri, kaydedilinceye kadar uygulanmaz.

    ![Erişim ilkesi değişiklikleri kaydediliyor](../media/authentication/assign-policy-portal-05.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](security-overview.md#identity-management)
- [Anahtar kasanızın güvenliğini sağlayın](secure-your-key-vault.md).
- [Geliştirici Kılavuzu Azure Key Vault](developers-guide.md)