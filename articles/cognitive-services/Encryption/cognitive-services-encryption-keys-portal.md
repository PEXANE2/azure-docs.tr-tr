---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını kullanma
titleSuffix: Cognitive Services
description: Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını nasıl kullanacağınızı öğrenin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bettirmenize ve iptal etmenizi sağlar.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 04a8a8d2520376931f2bb7727c1e751b83f6315f
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455283"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault'u kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Bilişsel Hizmetler kaynağı ve anahtar kasası aynı bölgede ve aynı Azure Etkin Dizini (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

Bu makalede, [Azure portalını](https://portal.azure.com/)kullanarak müşteri tarafından yönetilen anahtarlarla azure anahtar kasası nasıl yapılandırılanın gösterilmektedir. Azure portalını kullanarak önemli bir kasa oluşturmayı öğrenmek için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../../key-vault/secrets/quick-create-portal.md)

## <a name="configure-azure-key-vault"></a>Azure Key Vault'u yapılandırma

Müşteri tarafından yönetilen anahtarların kullanılması, anahtar kasasında iki özellik ayarlanmasını gerektirir, **Yumuşak Silme** ve **Temizleme.** Bu özellikler varsayılan olarak etkinleştirilmez, ancak yeni veya varolan bir anahtar kasasında PowerShell veya Azure CLI kullanılarak etkinleştirilebilir.

> [!IMPORTANT]
> **Yumuşak Silme** ve **Temizleme** özelliklerini etkinleştirmediyseniz ve anahtarınızı silerseniz, Bilişsel Hizmet kaynağınızdaki verileri kurtaramazsınız.

Varolan bir anahtar kasasında bu özellikleri nasıl etkinleştireceklerini öğrenmek için aşağıdaki makalelerden birinde **yumuşak silmeyi etkinleştirme** ve **Temizleme Korumasını Etkinleştirme** başlıklı bölümlere bakın:

- [PowerShell ile yumuşak silme nasıl kullanılır.](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell)
- [CLI ile yumuşak silme nasıl kullanılır.](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli)

Azure Depolama şifrelemesi ile yalnızca 2048 boyutunda RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys)Key Vault **tuşlarına** bakın.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

Azure portalında müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Bilişsel Hizmetler kaynağınıza gidin.
1. Bilişsel Hizmetler kaynağınızın **Ayarlar** bıyığınızda **Şifreleme'yi**tıklatın. Aşağıdaki şekilde gösterildiği gibi **Müşteri Yönetilen Anahtarlar** seçeneğini seçin.

    ![Müşteri Yönetilen Anahtarları'nın nasıl seçilebildiğini gösteren ekran görüntüsü](../media/cognitive-services-encryption/selectcmk.png)

## <a name="specify-a-key"></a>Bir anahtar belirtin

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, Bilişsel Hizmetler kaynağıyla ilişkilendirmek için bir anahtar belirtme fırsatınız olur.

### <a name="specify-a-key-as-a-uri"></a>Uri olarak bir anahtar belirtin

Bir anahtarı URI olarak belirtmek için aşağıdaki adımları izleyin:

1. Azure portalındaki URI anahtarını bulmak için anahtar kasanıza gidin ve **Keys** ayarını seçin. İstenilen tuşu seçin ve ardından sürümlerini görüntülemek için tuşa tıklayın. Bu sürümün ayarlarını görüntülemek için önemli bir sürüm seçin.
1. URI'yi sağlayan **Anahtar Tanımlayıcı** alanının değerini kopyalayın.

    ![Anahtar kasa anahtarı URI'yi gösteren ekran görüntüsü](../media/cognitive-services-encryption/key-uri-portal.png)

1. Depolama hesabınızın **Şifreleme** ayarlarında **URI tuşuna girin** seçeneğini belirleyin.
1. **Anahtar URI** alanına kopyaladığınız URI'yi yapıştırın.

   ![URI tuşuna nasıl girilir gösteren ekran görüntüsü](../media/cognitive-services-encryption/ssecmk2.png)

1. Anahtar kasasını içeren aboneliği belirtin.
1. Yaptığınız değişiklikleri kaydedin.

### <a name="specify-a-key-from-a-key-vault"></a>Anahtar kasasından bir anahtar belirtin

Bir anahtar kasasından bir anahtar belirtmek için, önce anahtar içeren bir anahtar kasanız olduğundan emin olun. Anahtar kasasından bir anahtar belirtmek için aşağıdaki adımları izleyin:

1. Anahtar **Kasası seçeneğini** seçin.
1. Kullanmak istediğiniz anahtarı içeren anahtar kasasını seçin.
1. Anahtar kasasından anahtarı seçin.

   ![Müşteri tarafından yönetilen anahtar seçeneğini gösteren ekran görüntüsü](../media/cognitive-services-encryption/ssecmk3.png)

1. Yaptığınız değişiklikleri kaydedin.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için Bilişsel Hizmetler kaynağını güncelleştirin. Şu adımları uygulayın:

1. Bilişsel Hizmetler kaynağınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar sürümü için URI'yi girin. Alternatif olarak, sürümü güncelleştirmek için anahtar kasasını ve anahtarı yeniden seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Şifreleme için kullanılan anahtarı değiştirmek için aşağıdaki adımları izleyin:

1. Bilişsel Hizmetler kaynağınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar için URI'yi girin. Alternatif olarak, anahtar kasasını seçebilir ve yeni bir anahtar seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı

Müşteri tarafından yönetilen anahtarları devre dışı dışı bettiğinizde, Bilişsel Hizmetler kaynağınız Microsoft tarafından yönetilen anahtarlarla şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakabilmek için aşağıdaki adımları izleyin:

1. Bilişsel Hizmetler kaynağınıza gidin ve **Şifreleme** ayarlarını görüntüleyin.
1. **Kendi anahtar ayarınızı Kullan'ın** yanındaki onay kutusunu seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Bilişsel Hizmetler Müşteri Tarafından Yönetilen Anahtar Talep Formu](https://aka.ms/cogsvc-cmk)
* [Yüz Hizmetleri'nde veri şifrelemesi](../Face/face-encryption-of-data-at-rest.md)
* [QnA Maker veri şifrelemesi](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Dil Anlama hizmeti şifrelemesi veri lerini istirahatte](../LUIS/luis-encryption-of-data-at-rest.md)
* [İçerik Moderatör veri şifreleme stoyon](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Veri şifrelemesi](../translator/translator-encryption-of-data-at-rest.md)
* [Veri personalizer şifreleme stoyat](../personalizer/personalizer-encryption-of-data-at-rest.md)
