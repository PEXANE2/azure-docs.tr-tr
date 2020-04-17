---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını kullanma
titleSuffix: Azure Storage
description: Azure Depolama şifrelemesi için Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 192e58b101b824ca0cc0c732e02647838be6dc35
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81456490"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, [Azure portalını](https://portal.azure.com/)kullanarak müşteri tarafından yönetilen anahtarlarla azure anahtar kasası nasıl yapılandırılanın gösterilmektedir. Azure portalını kullanarak önemli bir kasa oluşturmayı öğrenmek için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../../key-vault/secrets/quick-create-portal.md)

## <a name="configure-azure-key-vault"></a>Azure Key Vault'u yapılandırma

Azure Depolama şifrelemesi ile müşteri tarafından yönetilen anahtarların kullanılması, anahtar kasasında iki özellik ayarlanmasını gerektirir: **Yumuşak Silme** ve **Temizleme.** Bu özellikler varsayılan olarak etkinleştirilmez, ancak yeni veya varolan bir anahtar kasasında PowerShell veya Azure CLI kullanılarak etkinleştirilebilir.

Varolan bir anahtar kasasında bu özellikleri nasıl etkinleştireceklerini öğrenmek için aşağıdaki makalelerden birinde **yumuşak silmeyi etkinleştirme** ve **Temizleme Korumasını Etkinleştirme** başlıklı bölümlere bakın:

- [PowerShell ile yumuşak silme nasıl kullanılır.](../../key-vault/general/soft-delete-powershell.md)
- [CLI ile yumuşak silme nasıl kullanılır.](../../key-vault/general/soft-delete-cli.md)

Azure Depolama şifrelemesi ile yalnızca 2048 bit RSA ve RSA-HSM anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys)Key Vault **tuşlarına** bakın.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

Azure portalında müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin.
1. Depolama hesabı için **Ayarlar** bıyıküzerinde **Şifreleme'yi**tıklatın. Aşağıdaki resimde gösterildiği gibi **Müşteri Yönetilen Anahtarlar** seçeneğini seçin.

    ![Şifreleme seçeneğini gösteren portal ekran görüntüsü](./media/storage-encryption-keys-portal/portal-configure-encryption-keys.png)

## <a name="specify-a-key"></a>Bir anahtar belirtin

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, depolama hesabıyla ilişkilendirmek için bir anahtar belirtme fırsatınız olur.

### <a name="specify-a-key-as-a-uri"></a>Uri olarak bir anahtar belirtin

Bir anahtarı URI olarak belirtmek için aşağıdaki adımları izleyin:

1. Azure portalındaki URI anahtarını bulmak için anahtar kasanıza gidin ve **Keys** ayarını seçin. İstenilen tuşu seçin ve ardından sürümlerini görüntülemek için tuşa tıklayın. Bu sürümün ayarlarını görüntülemek için önemli bir sürüm seçin.
1. URI'yi sağlayan **Anahtar Tanımlayıcı** alanının değerini kopyalayın.

    ![Anahtar kasa anahtarı URI'yi gösteren ekran görüntüsü](media/storage-encryption-keys-portal/portal-copy-key-identifier.png)

1. Depolama hesabınızın **Şifreleme** ayarlarında **URI tuşuna girin** seçeneğini belirleyin.
1. **Anahtar URI** alanına kopyaladığınız URI'yi yapıştırın.

   ![URI tuşuna nasıl girilir gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/portal-specify-key-uri.png)

1. Anahtar kasasını içeren aboneliği belirtin.
1. Yaptığınız değişiklikleri kaydedin.

### <a name="specify-a-key-from-a-key-vault"></a>Anahtar kasasından bir anahtar belirtin

Bir anahtar kasasından bir anahtar belirtmek için, önce anahtar içeren bir anahtar kasanız olduğundan emin olun. Anahtar kasasından bir anahtar belirtmek için aşağıdaki adımları izleyin:

1. Anahtar **Kasası seçeneğini** seçin.
1. Kullanmak istediğiniz anahtarı içeren anahtar kasasını seçin.
1. Anahtar kasasından anahtarı seçin.

   ![Müşteri tarafından yönetilen anahtar seçeneğini gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/portal-select-key-from-key-vault.png)

1. Yaptığınız değişiklikleri kaydedin.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabını güncelleştirin. Şu adımları uygulayın:

1. Depolama hesabınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar sürümü için URI'yi girin. Alternatif olarak, sürümü güncelleştirmek için anahtar kasasını ve anahtarı yeniden seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure Depolama şifrelemesi için kullanılan anahtarı değiştirmek için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar için URI'yi girin. Alternatif olarak, anahtar kasasını seçebilir ve yeni bir anahtar seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı

Müşteri tarafından yönetilen anahtarları devre dışı dışı bettiğinizde, depolama hesabınız bir kez daha Microsoft tarafından yönetilen anahtarlarla şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakabilmek için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. **Kendi anahtar ayarınızı Kullan'ın** yanındaki onay kutusunu seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md)
- [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
