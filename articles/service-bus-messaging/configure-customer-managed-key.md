---
title: Bekleyen verileri şifrelemek için kendi anahtarınızı yapılandırın Azure Service Bus
description: Bu makalede, Azure Service Bus veri kalanını şifrelemek için kendi anahtarınızı yapılandırma hakkında bilgi verilmektedir.
services: service-bus-messaging
ms.service: service-bus
documentationcenter: ''
author: axisc
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: aschhab
ms.openlocfilehash: 6d20d4031f0ed4d1be4dddf9e33946251d6dd523
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903312"
---
# <a name="configure-customer-managed-keys-for-encrypting-azure-service-bus-data-at-rest-by-using-the-azure-portal"></a>Azure portal kullanarak geri kalan Azure Service Bus verileri şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırın
Azure Service Bus Premium, Azure Depolama Hizmeti Şifrelemesi (Azure SSE) ile bekleyen verilerin şifrelenmesini sağlar. Service Bus Premium, verileri depolamak için Azure depolama 'yı kullanır ve varsayılan olarak, Azure Storage ile depolanan tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. 

## <a name="overview"></a>Genel Bakış
Azure Service Bus artık, Microsoft tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla (Kendi Anahtarını Getir-BYOK), bekleyen verileri şifreleme seçeneğini desteklemektedir. Bu özellik, bekleyen Azure Service Bus şifrelemek için kullanılan müşteri tarafından yönetilen anahtarlara erişimi oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal etmenize olanak sağlar.

BYOK özelliğinin etkinleştirilmesi, ad alanınız üzerinde bir kerelik kurulum işlemidir.

> [!NOTE]
> Hizmet tarafı şifrelemesi için müşterinin yönettiği anahtar için bazı uyarılar vardır. 
>   * Bu özellik [Azure Service Bus Premium](service-bus-premium-messaging.md) katmanı tarafından desteklenir. Standart katman Service Bus ad alanları için etkinleştirilemez.
>   * Şifreleme yalnızca yeni veya boş ad alanları için etkinleştirilebilir. Ad alanı veri içeriyorsa, şifreleme işlemi başarısız olur.

Anahtarlarınızı yönetmek ve anahtar kullanımınızı denetlemek için Azure Key Vault kullanabilirsiniz. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../key-vault/key-vault-overview.md)

Bu makalede, Azure portal kullanarak, müşteri tarafından yönetilen anahtarlarla bir anahtar kasasının nasıl yapılandırılacağı gösterilmektedir. Azure portal kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Key Vault gizli dizi ayarlama ve alma](../key-vault/quick-create-portal.md).

> [!IMPORTANT]
> Azure Service Bus ile müşteri tarafından yönetilen anahtarların kullanılması, anahtar kasasının iki gerekli özelliği yapılandırılmış olmasını gerektirir. Bunlar: **geçici silme** ve **Temizleme**. Azure portal yeni bir Anahtar Kasası oluşturduğunuzda, bu özellikler varsayılan olarak etkinleştirilir. Ancak, var olan bir anahtar kasasında bu özellikleri etkinleştirmeniz gerekiyorsa, PowerShell veya Azure CLı kullanmanız gerekir.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir
Azure portal müşteri tarafından yönetilen anahtarları etkinleştirmek için şu adımları izleyin:

1. Service Bus Premium ad alanına gidin.
2. Service Bus ad alanının **Ayarlar** sayfasında **şifreleme**' yi seçin.
3. Aşağıdaki görüntüde gösterildiği gibi **geri kalan müşteri tarafından yönetilen anahtar şifrelemesini** seçin.

    ![Müşteri tarafından yönetilen anahtarı etkinleştir](./media/configure-customer-managed-key/enable-customer-managed-key.png)


## <a name="set-up-a-key-vault-with-keys"></a>Anahtarlar içeren bir Anahtar Kasası ayarlama

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, müşteri tarafından yönetilen anahtarı Azure Service Bus ad alanıyla ilişkilendirmeniz gerekir. Service Bus yalnızca Azure Key Vault destekler. Önceki bölümde, **müşteri tarafından yönetilen anahtar seçeneğiyle şifrelemeyi** etkinleştirirseniz, anahtarın Azure Key Vault içine aktarılması gerekir. Ayrıca, anahtarlar için **yumuşak silme** ve anahtar Için de **Temizleme** yapılandırması olmalıdır. Bu ayarlar, [PowerShell](../key-vault/key-vault-soft-delete-powershell.md) veya [CLI](../key-vault/key-vault-soft-delete-cli.md#enabling-purge-protection)kullanılarak yapılandırılabilir.

1. Yeni bir Anahtar Kasası oluşturmak için Azure Key Vault [hızlı](../key-vault/key-vault-overview.md)başlangıcı ' nı izleyin. Varolan anahtarları içeri aktarma hakkında daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md).
1. Bir kasa oluştururken hem geçici silme hem de Temizleme korumasını açmak için [az keykasa Create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın.

    ```azurecli-interactive
    az keyvault create --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
    ```    
1. Var olan bir kasaya Temizleme koruması eklemek için (zaten geçici silme etkindir), [az keykasa Update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutunu kullanın.

    ```azurecli-interactive
    az keyvault update --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --enable-purge-protection true
    ```
1. Aşağıdaki adımları izleyerek anahtarlar oluşturun:
    1. Yeni bir anahtar oluşturmak için **Ayarlar**altındaki **anahtarlar** menüsünden **Oluştur/içeri aktar** ' ı seçin.
        
        ![Oluştur/Içeri Aktar düğmesini seçin](./media/configure-customer-managed-key/select-generate-import.png)

    1. Oluşturma **seçeneklerini** belirleyin ve anahtara bir ad verin.

        ![Bir anahtar oluşturma](./media/configure-customer-managed-key/create-key.png) 

    1. Şimdi, açılan listeden şifrelemek için Service Bus ad alanıyla ilişkilendirmek üzere bu anahtarı seçebilirsiniz. 

        ![Anahtar kasasından anahtar seçin](./media/configure-customer-managed-key/select-key-from-key-vault.png)
        > [!NOTE]
        > Artıklık için en fazla 3 anahtar ekleyebilirsiniz. Anahtarların birinin süresi dolduğunda veya erişilebilir durumda değilse, diğer anahtarlar şifreleme için kullanılacaktır.
        
    1. Anahtarın ayrıntılarını girin ve **Seç**' e tıklayın. Bu, müşteri tarafından yönetilen anahtar ile ad alanındaki bekleyen verilerin şifrelenmesini sağlar. 


> [!IMPORTANT]
> Coğrafi olağanüstü durum kurtarma ile birlikte müşteri tarafından yönetilen anahtar kullanmak istiyorsanız lütfen aşağıdaki adımları gözden geçirin. 
>
> Müşteri tarafından yönetilen anahtarla bekleyen şifrelemeyi etkinleştirmek için, belirtilen Azure Anahtar Kasası 'nda Service Bus ' yönetilen kimliği için bir [erişim ilkesi](../key-vault/key-vault-secure-your-key-vault.md) ayarlanır. Bu, Azure Keykasasına Azure Service Bus ad alanından denetimli erişim sağlar.
>
> Bunun nedeni:
> 
>   * Service Bus ad alanı için [coğrafi olağanüstü durum kurtarma](service-bus-geo-dr.md) zaten etkinse ve müşterinin yönettiği anahtarı etkinleştirmek istiyorsanız, 
>     * Eşleştirmeyi kes
>     * Anahtar kasasında hem birincil hem de ikincil ad alanları için yönetilen kimliğin [erişim Ilkesini ayarlayın](../key-vault/managed-identity.md) .
>     * Birincil ad alanında şifrelemeyi ayarlayın.
>     * Birincil ve ikincil ad alanlarını yeniden eşleştirin.
> 
>   * Müşteri tarafından yönetilen anahtarın zaten ayarlandığı bir Service Bus ad alanında coğrafi DR 'yi etkinleştirmek istiyorsanız-
>     * İkincil ad alanı için yönetilen kimlik için [erişim ilkesini](../key-vault/managed-identity.md) anahtar kasasında ayarlayın.
>     * Birincil ve ikincil ad alanlarını eşleştirin.


## <a name="rotate-your-encryption-keys"></a>Şifreleme anahtarlarınızı döndürün

Anahtarı anahtar kasasında Azure Anahtar Kasası döndürme mekanizmasını kullanarak döndürebilirsiniz. Daha fazla bilgi için bkz. [anahtar dönüşü ve denetimini ayarlama](../key-vault/key-vault-key-rotation-log-monitoring.md). Etkinleştirme ve sona erme tarihleri, anahtar döndürmeyi otomatik hale getirmek için de ayarlanabilir. Service Bus hizmet yeni anahtar sürümlerini algılar ve otomatik olarak kullanmaya başlar.

## <a name="revoke-access-to-keys"></a>Anahtarlara erişimi iptal et

Şifreleme anahtarlarına erişimin iptal edilmemesi Service Bus verileri temizleyemezsiniz. Ancak, verilere Service Bus ad alanından erişilemez. Şifreleme anahtarını erişim ilkesi veya anahtarı silerek iptal edebilirsiniz. Erişim ilkeleri hakkında daha fazla bilgi edinin ve anahtar kasasının güvenliğini [güvenli bir şekilde bir anahtar kasasına erişin](../key-vault/key-vault-secure-your-key-vault.md).

Şifreleme anahtarı iptal edildiğinde, şifrelenen ad alanındaki Service Bus hizmeti çalışamaz hale gelir. Anahtara erişim etkinleştirilirse veya silinen anahtar geri yüklenirse, şifrelenmiş Service Bus ad alanındaki verilere erişebilmek için Service Bus hizmet anahtarı seçer.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:
- [Service Bus genel bakış](service-bus-messaging-overview.md)
- [Key Vault genel bakış](../key-vault/key-vault-overview.md)


