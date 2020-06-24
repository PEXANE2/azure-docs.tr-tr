---
title: Azure portal hesap oluşturma
description: Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin
ms.topic: how-to
ms.date: 06/10/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1205de2b800588b735aeb20d388ba4b64bc6b078
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84711349"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Azure portalıyla Batch hesabı oluşturma

Bu konuda, işlem senaryonuza uyan hesap özelliklerini seçerek [Azure portal](https://portal.azure.com)Azure Batch hesabının nasıl oluşturulacağı gösterilmektedir. Ayrıca erişim anahtarları ve hesap URL 'Leri gibi önemli hesap özelliklerini nerede bulacağınızı öğreneceksiniz.

Batch hesapları ve senaryoları hakkında arka plan için bkz. [Batch hizmeti iş akışı ve kaynakları](batch-service-workflow-features.md).

## <a name="create-a-batch-account"></a>Batch hesabı oluşturma

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Kaynak oluştur**' u seçin ve ardından **Işlem** ve **Batch hizmeti**' ni seçin.

1. **Yeni Batch hesabı** ayarlarını girin. Aşağıdaki ayrıntılara bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    b. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu seçebilir ya da isterseniz yeni bir tane oluşturabilirsiniz.

    c. **Hesap adı**: Seçtiğiniz ad, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı**: Batch hesabınızla ilişkilendirdiğiniz isteğe bağlı bir Azure depolama hesabı. En iyi performans için genel amaçlı v2 depolama hesabı önerilir. Toplu Işteki tüm depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](accounts.md#azure-storage-accounts). Portalda, var olan bir depolama hesabını seçin veya yeni bir tane oluşturun.

      ![Depolama hesabı oluşturma][storage_account]

    f. **Havuz ayırma modu**: **Gelişmiş** ayarlar sekmesinde, havuz ayırma modunu **Batch hizmeti** veya **Kullanıcı aboneliği**olarak belirtebilirsiniz. Çoğu senaryo için varsayılan **Batch hizmetini**kabul edin.

      ![Toplu işlem havuzu ayırma modu][pool_allocation]

1. Hesabı oluşturmak için **Oluştur**'u seçin.

## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme

Hesap oluşturulduktan sonra seçerek ayarlarına ve özelliklerine erişebilirsiniz. Sol menüyü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

> [!NOTE]
> Batch hesabının adı kendi KIMLIĞIDIR ve değiştirilemez. Bir Batch hesabının adını değiştirmeniz gerekiyorsa, hesabı silmeniz ve hedeflenen ada sahip yeni bir tane oluşturmanız gerekir.

![Azure portalında Batch hesabı sayfası][account_blade]

* **Batch hesabı adı, URL’si ve anahtarları**: [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development) ile uygulama geliştirirken, Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si ve anahtarı gereklidir. (Batch, Azure Active Directory kimlik doğrulamasını da destekler.)

    Batch hesabı erişim bilgilerini görüntülemek için **Anahtarlar**'ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

* Batch hesabınızla ilişkili depolama hesabının adını ve anahtarlarını görüntülemek için **Depolama hesabı**'nı seçin.

* Batch hesabı için geçerli olan kaynak kotalarını görüntülemek için **Kotalar**'ı seçin. Ayrıntılar için bkz. [Batch hizmeti kotaları ve limitleri](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Kullanıcı aboneliği modu için ek yapılandırma

Kullanıcı aboneliği modunda bir Batch hesabı oluşturmayı seçerseniz, hesabı oluşturmadan önce aşağıdaki ek adımları gerçekleştirin.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch hizmetinin aboneliğe erişmesine izin verme (tek seferlik işlem)

Kullanıcı aboneliği modunda ilk Batch hesabınızı oluştururken, aboneliğinizi Batch hizmetine kaydetmeniz gerekir. (Zaten bunu yaptıysanız sonraki bölüme atlayın.)

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Tüm hizmet**  >  **abonelikleri**' ni seçin ve Batch hesabı için kullanmak istediğiniz aboneliği seçin.

1. **Abonelik** sayfasında **Kaynak sağlayıcıları**'nı seçip **Microsoft.Batch**'i arayın. **Microsoft.Batch** kaynak sağlayıcısının aboneliğe kayıtlı olup olmadığını kontrol edin. Kayıtlı değilse **Kaydet** bağlantısını seçin.

    ![Microsoft.Batch sağlayıcısını kaydetme][register_provider]

1. **Abonelik** sayfasında, **erişim denetimi (IAM)**  >  **rol atamaları**  >  **rol ataması Ekle**' yi seçin.

    ![Abonelik erişim denetimi][subscription_access]

1. **Rol ataması Ekle** sayfasında, **katkıda** bulunan veya **sahip** rolü ' nü seçin ve Batch API 'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    1. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    1. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**.

1. Batch API'sini bulduktan sonra seçin ve **Kaydet**'i seçin.

    ![Batch izinleri ekleme][add_permission]

### <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Kullanıcı aboneliği modunda bir [Azure Key Vault](../key-vault/general/overview.md) gereklidir. Key Vault, oluşturulacak Batch hesabıyla aynı abonelikte ve bölgede olmalıdır. 

1. [Azure portalında](https://portal.azure.com)**Yeni** > **Güvenlik** > **Anahtar Kasası**'nı seçin.

1. **Key Vault oluştur** sayfasında, Key Vault için bir ad girin ve Batch hesabınız için istediğiniz bölgede bir kaynak grubu oluşturun. Kalan ayarları varsayılan değerlerinde bırakın ve ardından **Oluştur**'u seçin.

Kullanıcı aboneliği modunda Batch hesabını oluştururken, havuz ayırma modu olarak **Kullanıcı aboneliğini** belirtin, Key Vault seçin ve Key Vault Azure Batch erişim izni vermek için kutuyu işaretleyin.

Key Vault el ile erişim izni vermeyi tercih ediyorsanız, Key Vault **erişim ilkeleri** bölümüne gidin, **erişim ilkesi Ekle** ' yi seçin ve **Microsoft Azure Batch**için arama yapın. Seçildikten sonra, açılan menüyü kullanarak **gizli izinleri** yapılandırmanız gerekecektir. Azure Batch, en az **Get**, **list**, **set**ve **Delete** izinleri olarak verilmelidir.

![Azure Batch için gizli izinler](./media/batch-account-create-portal/secret-permissions.png)

> [!NOTE]
> **Dağıtım Için Azure sanal makinelerinin** ve **şablon dağıtımı için Azure Resource Manager** , bağlantılı **Key Vault** kaynağı için **erişim ilkeleri** altında seçili olduğundan emin olun.
>
> ![Zorunlu Key Vault erişim Ilkesi](./media/batch-account-create-portal/key-vault-access-policy.png)

### <a name="configure-subscription-quotas"></a>Abonelik kotalarını yapılandırma

Kullanıcı aboneliği Batch hesapları için çekirdek kotaların el ile ayarlanması gerekir. Standart Batch çekirdek kotalar, kullanıcı aboneliği modundaki hesaplara uygulanmaz.

1. [Azure Portal](https://portal.azure.com), ayarlarını ve özelliklerini göstermek için Kullanıcı abonelik modu Batch hesabınızı seçin.
1. Sol taraftaki menüden **Kotalar** ' ı seçerek Batch hesabınızla ilişkili çekirdek kotalarını görüntüleyin ve yapılandırın.

Kullanıcı aboneliği modu çekirdek kotaları hakkında daha fazla bilgi için bkz. [Batch hizmeti kotaları ve limitleri](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri

Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdaki gibi araçlarla oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
* [Batch .NET istemci kitaplığı](quick-run-dotnet.md) veya [Python](quick-run-python.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu hızlı başlangıçlar, bir iş yükünü birden fazla işlem düğümünde yürütmek üzere Batch hizmetini kullanan örnek uygulamalar konusunda size rehberlik sağlamanın yanı sıra, iş yükü dosyası hazırlama ve alma işlemleri için Azure Depolama kullanma ile ilgili bilgiler de içerir.

[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[pool_allocation]: ./media/batch-account-create-portal/batch-pool-allocation.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png
