---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portal kullanın
titleSuffix: Cognitive Services
description: Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma hakkında bilgi edinin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal edebilmesini sağlar.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 04a8a8d2520376931f2bb7727c1e751b83f6315f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81455283"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma

Müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Bilişsel hizmetler kaynağı ve Anahtar Kasası aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

Bu makalede, [Azure Portal](https://portal.azure.com/)kullanılarak müşteri tarafından yönetilen anahtarlarla bir Azure Key Vault nasıl yapılandırılacağı gösterilmektedir. Azure portal kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Key Vault gizli dizi ayarlama ve alma](../../key-vault/secrets/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Azure Key Vault'u yapılandırma

Müşteri tarafından yönetilen anahtarların kullanılması, anahtar kasasında iki özellik ayarlanmasını, **geçici silme** ve **Temizleme işlemi**yapılmasını gerektirir. Bu özellikler varsayılan olarak etkinleştirilmez, ancak yeni veya var olan bir anahtar kasasında PowerShell veya Azure CLı kullanılarak etkinleştirilebilir.

> [!IMPORTANT]
> **Yumuşak silme** ve **Temizleme** özellikleri etkin değilse ve anahtarınızı silerseniz, bilişsel hizmet kaynağınızın verilerini kurtaramazsınız.

Mevcut bir anahtar kasasında bu özellikleri etkinleştirmeyi öğrenmek için aşağıdaki makalelerden birinde **geçici silme özelliğini etkinleştirme** ve **Temizleme korumasını etkinleştirme** başlıklı bölümlere bakın:

- [PowerShell ile geçici silme nasıl kullanılır](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell).
- [CLI ile geçici silme nasıl kullanılır](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli).

Azure depolama şifrelemesi ile yalnızca 2048 boyutundaki RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) **Key Vault anahtarlar** .

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Azure portal müşteri tarafından yönetilen anahtarları etkinleştirmek için şu adımları izleyin:

1. Bilişsel hizmetler kaynağına gidin.
1. Bilişsel hizmetler kaynağınızın **Ayarlar** dikey penceresinde **şifreleme**' ye tıklayın. Aşağıdaki şekilde gösterildiği gibi, **müşteri tarafından yönetilen anahtarlar** seçeneğini belirleyin.

    ![Müşteri tarafından yönetilen anahtarların nasıl seçileceğini gösteren ekran görüntüsü](../media/cognitive-services-encryption/selectcmk.png)

## <a name="specify-a-key"></a>Bir anahtar belirtin

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra bilişsel hizmetler kaynağıyla ilişkilendirilecek bir anahtar belirtme fırsatına sahip olacaksınız.

### <a name="specify-a-key-as-a-uri"></a>URI olarak bir anahtar belirtin

Bir anahtarı URI olarak belirtmek için şu adımları izleyin:

1. Azure portal anahtar URI 'sini bulmak için, anahtar kasanıza gidin ve **anahtarlar** ayarını seçin. İstediğiniz anahtarı seçin ve ardından sürümlerini görüntülemek için anahtara tıklayın. Bu sürümün ayarlarını görüntülemek için bir anahtar sürüm seçin.
1. URI sağlayan **anahtar tanımlayıcı** alanının değerini kopyalayın.

    ![Anahtar Kasası anahtar URI 'sini gösteren ekran görüntüsü](../media/cognitive-services-encryption/key-uri-portal.png)

1. Depolama hesabınızın **şifreleme** ayarları ' nda **anahtar URI 'si girin** seçeneğini belirleyin.
1. Kopyaladığınız URI 'yi **anahtar URI** alanına yapıştırın.

   ![Anahtar URI 'sini girmeyi gösteren ekran görüntüsü](../media/cognitive-services-encryption/ssecmk2.png)

1. Anahtar kasasını içeren aboneliği belirtin.
1. Yaptığınız değişiklikleri kaydedin.

### <a name="specify-a-key-from-a-key-vault"></a>Anahtar kasasından anahtar belirtme

Anahtar kasasından bir anahtar belirtmek için öncelikle anahtar içeren bir anahtar kasanızın olduğundan emin olun. Anahtar kasasından bir anahtar belirtmek için şu adımları izleyin:

1. **Key Vault seçin** seçeneğini belirleyin.
1. Kullanmak istediğiniz anahtarı içeren anahtar kasasını seçin.
1. Anahtar kasasından anahtarı seçin.

   ![Müşteri tarafından yönetilen anahtar seçeneğini gösteren ekran görüntüsü](../media/cognitive-services-encryption/ssecmk3.png)

1. Yaptığınız değişiklikleri kaydedin.

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, bilişsel hizmetler kaynağını yeni sürümü kullanacak şekilde güncelleştirin. Şu adımları uygulayın:

1. Bilişsel hizmetler kaynağınız ' ne gidin ve **şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar sürümünün URI 'sini girin. Alternatif olarak, sürümü güncelleştirmek için anahtar kasasını ve anahtarı yeniden seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Şifreleme için kullanılan anahtarı değiştirmek için şu adımları izleyin:

1. Bilişsel hizmetler kaynağınız ' ne gidin ve **şifreleme** ayarlarını görüntüleyin.
1. Yeni anahtar için URI girin. Alternatif olarak, anahtar kasasını seçip yeni bir anahtar seçebilirsiniz.
1. Yaptığınız değişiklikleri kaydedin.

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı bırak

Müşteri tarafından yönetilen anahtarları devre dışı bıraktığınızda, bilişsel hizmetler kaynağınız daha sonra Microsoft tarafından yönetilen anahtarlarla şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakmak için şu adımları izleyin:

1. Bilişsel hizmetler kaynağınız ' ne gidin ve **şifreleme** ayarlarını görüntüleyin.
1. **Kendi anahtarınızı kullanın** ayarının yanındaki onay kutusunun işaretini kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
* [Bilişsel hizmetler müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Yüz Hizmetleri, bekleyen verilerin şifrelenmesi](../Face/face-encryption-of-data-at-rest.md)
* [Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Bekleyen verilerin hizmet şifrelemesini Language Understanding](../LUIS/luis-encryption-of-data-at-rest.md)
* [Bekleyen verilerin şifrelenmesi Content Moderator](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Bekleyen verilerin Translator şifrelemesi](../translator/translator-encryption-of-data-at-rest.md)
* [Bekleyen verilerin kişiselleştirici şifrelemesi](../personalizer/personalizer-encryption-of-data-at-rest.md)
