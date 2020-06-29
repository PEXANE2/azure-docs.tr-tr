---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portal kullanın
titleSuffix: Azure Storage
description: Azure depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırmak üzere Azure portal nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 4af70a4e2a698bd280c8c41018bc5aaa1bfa27f8
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85512551"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, [Azure Portal](https://portal.azure.com/)kullanılarak müşteri tarafından yönetilen anahtarlarla bir Azure Key Vault nasıl yapılandırılacağı gösterilmektedir. Azure portal kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Key Vault gizli dizi ayarlama ve alma](../../key-vault/secrets/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Azure Key Vault'u yapılandırma

Azure depolama şifrelemesi ile müşteri tarafından yönetilen anahtarların kullanılması, anahtar kasasında iki özellik ayarlanmasını, **geçici silme** ve **Temizleme işlemi**yapılmasını gerektirir. Bu özellikler varsayılan olarak etkinleştirilmez, ancak yeni veya var olan bir anahtar kasasında PowerShell veya Azure CLı kullanılarak etkinleştirilebilir.

Mevcut bir anahtar kasasında bu özellikleri etkinleştirmeyi öğrenmek için aşağıdaki makalelerden birinde **geçici silme özelliğini etkinleştirme** ve **Temizleme korumasını etkinleştirme** başlıklı bölümlere bakın:

- [PowerShell ile geçici silme nasıl kullanılır](../../key-vault/general/soft-delete-powershell.md).
- [CLI ile geçici silme nasıl kullanılır](../../key-vault/general/soft-delete-cli.md).

Azure depolama şifrelemesi, 2048, 3072 ve 4096 boyutlarının RSA ve RSA-HSM anahtarlarını destekler. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys) **Key Vault anahtarlar** .

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Azure portal müşteri tarafından yönetilen anahtarları etkinleştirmek için şu adımları izleyin:

1. Depolama hesabınıza gidin.
1. Depolama hesabının **Ayarlar** dikey penceresinde **şifreleme**' ye tıklayın. Aşağıdaki görüntüde gösterildiği gibi, **müşteri tarafından yönetilen anahtarlar** seçeneğini belirleyin.

    ![Şifreleme seçeneğini gösteren Portal ekran görüntüsü](./media/storage-encryption-keys-portal/portal-configure-encryption-keys.png)

## <a name="specify-a-key"></a>Bir anahtar belirtin

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, depolama hesabıyla ilişkilendirilecek bir anahtar belirtme fırsatına sahip olacaksınız.

### <a name="specify-a-key-as-a-uri"></a>URI olarak bir anahtar belirtin

Bir anahtarı URI olarak belirtmek için şu adımları izleyin:

1. Azure portal anahtar URI 'sini bulmak için, anahtar kasanıza gidin ve **anahtarlar** ayarını seçin. İstediğiniz anahtarı seçin ve ardından sürümlerini görüntülemek için anahtara tıklayın. Bu sürümün ayarlarını görüntülemek için bir anahtar sürüm seçin.
1. URI sağlayan **anahtar tanımlayıcı** alanının değerini kopyalayın.

    ![Anahtar Kasası anahtar URI 'sini gösteren ekran görüntüsü](media/storage-encryption-keys-portal/portal-copy-key-identifier.png)

1. Depolama hesabınızın **şifreleme** ayarları ' nda **anahtar URI 'si girin** seçeneğini belirleyin.
1. Kopyaladığınız URI 'yi **anahtar URI** alanına yapıştırın.

   ![Anahtar URI 'sini girmeyi gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/portal-specify-key-uri.png)

1. Anahtar kasasını içeren aboneliği belirtin.
1. Yaptığınız değişiklikleri kaydedin.

### <a name="specify-a-key-from-a-key-vault"></a>Anahtar kasasından anahtar belirtme

Anahtar kasasından bir anahtar belirtmek için öncelikle anahtar içeren bir anahtar kasanızın olduğundan emin olun. Anahtar kasasından bir anahtar belirtmek için şu adımları izleyin:

1. **Key Vault seçin** seçeneğini belirleyin.
1. Kullanmak istediğiniz anahtarı içeren anahtar kasasını seçin.
1. Anahtar kasasından anahtarı seçin.

   ![Müşteri tarafından yönetilen anahtar seçeneğini gösteren ekran görüntüsü](./media/storage-encryption-keys-portal/portal-select-key-from-key-vault.png)

1. Yaptığınız değişiklikleri kaydedin.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabını güncelleştirin. Şu adımları uygulayın:

1. Depolama hesabınıza gidin ve **şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar sürümünün URI 'sini girin. Alternatif olarak, sürümü güncelleştirmek için anahtar kasasını ve anahtarı yeniden seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure depolama şifrelemesi için kullanılan anahtarı değiştirmek için şu adımları izleyin:

1. Depolama hesabınıza gidin ve **şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar için URI girin. Alternatif olarak, anahtar kasasını seçip yeni bir anahtar seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı bırak

Müşteri tarafından yönetilen anahtarları devre dışı bıraktığınızda, depolama hesabınız Microsoft tarafından yönetilen anahtarlarla yeniden şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakmak için şu adımları izleyin:

1. Depolama hesabınıza gidin ve **şifreleme** ayarlarını görüntüleyin.
1. **Kendi anahtarınızı kullanın** ayarının yanındaki onay kutusunun işaretini kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](storage-service-encryption.md)
- [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
