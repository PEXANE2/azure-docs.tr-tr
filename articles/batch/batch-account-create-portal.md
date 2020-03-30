---
title: Azure portalında hesap oluşturma - Azure Toplu İş | Microsoft Dokümanlar
description: Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3723631609a04f6d12abcaac1f9d7733bf3caa01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247650"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Azure portalıyla Batch hesabı oluşturma

[Azure portalında][azure_portal] Azure Batch hesabı oluşturma hakkında bilgi alın ve işlem senaryonuza uygun hesap özelliklerini seçin. Erişim anahtarları ve hesap URL’leri gibi önemli hesap özelliklerini nerede bulabileceğinizi öğrenin.

Batch hesapları ve senaryoları hakkında arka plan bilgileri için bkz. [özelliğe genel bakış](batch-api-basics.md).

## <a name="create-a-batch-account"></a>Batch hesabı oluşturma

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. [Azure portalında][azure_portal]oturum açın.

1. Kaynak > **İşlem** > **Toplu İş Hizmeti** **Oluştur'u**seçin.

    ![Market’te Batch][marketplace_portal]

1. **Yeni Batch hesabı** ayarlarını girin. Aşağıdaki ayrıntılara bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    b. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu seçebilir ya da isterseniz yeni bir tane oluşturabilirsiniz.

    c. **Hesap adı**: Seçtiğiniz ad, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı**: Toplu Iş hesabınızla ilişkilendirdiğiniz isteğe bağlı Azure Depolama hesabı. En iyi performans için genel amaçlı bir v2 depolama hesabı önerilir. Toplu İşlem'deki tüm depolama hesabı seçenekleri için [Toplu Iş özelliğine genel bakış'a](batch-api-basics.md#azure-storage-account)bakın. Portalda, varolan bir depolama hesabı seçin veya yeni bir depo oluşturun.

      ![Depolama hesabı oluşturma][storage_account]

    f. **Havuz ayırma modu**: **Gelişmiş** ayarlar sekmesinde toplu **işlem hizmeti** veya Kullanıcı **aboneliği**olarak havuz ayırma modunu belirtebilirsiniz. Çoğu senaryoda varsayılan **Toplu İşlem hizmetini**kabul edin.

      ![Toplu havuz ayırma modu][pool_allocation]

1. Hesabı oluşturmak için **Oluştur**'u seçin.

## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme

Hesap oluşturulduktan sonra seçerek ayarlarına ve özelliklerine erişebilirsiniz. Sol menüyü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı sayfası][account_blade]

* **Batch hesabı adı, URL’si ve anahtarları**: [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development) ile uygulama geliştirirken, Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si ve anahtarı gereklidir. (Batch, Azure Active Directory kimlik doğrulamasını da destekler.)

    Batch hesabı erişim bilgilerini görüntülemek için **Anahtarlar**'ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

* Batch hesabınızla ilişkili depolama hesabının adını ve anahtarlarını görüntülemek için **Depolama hesabı**'nı seçin.

* Batch hesabı için geçerli olan kaynak kotalarını görüntülemek için **Kotalar**'ı seçin. Ayrıntılar için bkz. [Batch hizmeti kotaları ve limitleri](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Kullanıcı aboneliği modu için ek yapılandırma

Kullanıcı aboneliği modunda bir Batch hesabı oluşturmayı seçerseniz, hesabı oluşturmadan önce aşağıdaki ek adımları gerçekleştirin.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch hizmetinin aboneliğe erişmesine izin verme (tek seferlik işlem)

Kullanıcı aboneliği modunda ilk Batch hesabınızı oluştururken, aboneliğinizi Batch hizmetine kaydetmeniz gerekir. (Bunu daha önce yaptıysanız sonraki bölüme atlayın.)

1. [Azure portalında][azure_portal]oturum açın.

1. **Tüm hizmet** > **Abonelikleri'ni**seçin ve Toplu İş hesabı için kullanmak istediğiniz aboneliği seçin.

1. **Abonelik** sayfasında **Kaynak sağlayıcıları**'nı seçip **Microsoft.Batch**'i arayın. **Microsoft.Batch** kaynak sağlayıcısının aboneliğe kayıtlı olup olmadığını kontrol edin. Kayıtlı değilse **Kaydet** bağlantısını seçin.

    ![Microsoft.Batch sağlayıcısını kaydetme][register_provider]

1. **Abonelik** **sayfasında, Access denetimi (IAM)** > Rol atamaları rol**ataması** > **ekleyin'i**seçin.

    ![Abonelik erişim denetimi][subscription_access]

1. Rol **ataması ekle** sayfasında **Katılımcı** rolünü seçin, Toplu İş API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    1. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    1. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**.

1. Batch API'sini bulduktan sonra seçin ve **Kaydet**'i seçin.

    ![Batch izinleri ekleme][add_permission]

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanıcı aboneliği modunda, oluşturulacak Batch hesabı ile aynı kaynak grubuna ait olan bir Azure key vault gereklidir. Kaynak grubunun, Batch hizmetinin [mevcut](https://azure.microsoft.com/regions/services/) olduğu ve aboneliğinizin desteklediği bir bölgede olduğundan emin olun.

1. [Azure portalında][azure_portal]**Yeni** > **Güvenlik** > **Anahtar Kasası**'nı seçin.

1. **Key Vault Oluştur** sayfasında anahtar kasası için bir ad girin ve Batch hesabınız için istediğiniz bölgede bir kaynak grubu oluşturun. Kalan ayarları varsayılan değerlerinde bırakın ve ardından **Oluştur**'u seçin.

Kullanıcı abonelik modunda Toplu İşlem hesabı oluştururken, anahtar kasası için kaynak grubunu kullanın. **Kullanıcı aboneliğini** havuz ayırma modu olarak belirtin, anahtar kasasını seçin ve Azure Toplu İşlem'in anahtar kasasına erişmesini sağlamak için kutuyu işaretleyin. 

Anahtar kasasına el ile erişim izni vermek isterseniz, anahtar kasasının **Erişim ilkeleri** bölümüne gidin ve **Erişim İlkesi Ekle'yi** seçin ve Microsoft Azure Toplu **İş'i**arayın. Seçildikten sonra, açılan menüyü kullanarak **Gizli izinleri** yapılandırmanız gerekir. Azure Toplu İşlemi en az **Alın,** **Listele,** **Set**ve **Sil** izinleri verilmelidir.

![Azure Toplu İş için gizli izinler](./media/batch-account-create-portal/secret-permissions.png)


> [!NOTE]
> Dağıtım **için Azure Sanal Makineleri** ve şablon dağıtım onay kutuları için Azure Kaynak **Yöneticisi'nin** bağlantılı **Anahtar Kasası** kaynağının **Access ilkeleri** altında seçildiğinden emin olun.
> 
> ![Zorunlu Anahtar Vault](./media/batch-account-create-portal/key-vault-access-policy.png) Erişim İlkesi Azure portalında toplu iş hesabı oluştururken bu zorunlu değildir. Seçenek varsayılan olarak seçilir.



### <a name="configure-subscription-quotas"></a>Abonelik kotalarını yapılandırma

Temel kotalar, kullanıcı aboneliği Toplu iş hesaplarında varsayılan olarak ayarlanmaz. Standart Toplu Toplu toplu kotalar kullanıcı abonelik modundaki hesaplariçin geçerli olmadığından, temel kotalar el ile ayarlanmalıdır.

1. Azure [portalında,][azure_portal]ayarlarını ve özelliklerini görüntülemek için kullanıcı abonelik modu Toplu hesabınızı seçin.

1. Sol menüden, Toplu İş hesabınızla ilişkili temel kotaları görüntülemek ve yapılandırmak için **Kotaları** seçin.

Kullanıcı abonelik modu çekirdek kotaları hakkında daha fazla bilgi için [Toplu hizmet kotalarına ve sınırlarına](batch-quota-limit.md) bakın.

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri

Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdaki gibi araçlarla oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yüklerine olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar.
* [Batch .NET istemci kitaplığı](quick-run-dotnet.md) veya [Python](quick-run-python.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu hızlı başlangıçlar, bir iş yükünü birden fazla işlem düğümünde yürütmek üzere Batch hizmetini kullanan örnek uygulamalar konusunda size rehberlik sağlamanın yanı sıra, iş yükü dosyası hazırlama ve alma işlemleri için Azure Depolama kullanma ile ilgili bilgiler de içerir.

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[marketplace_portal]: ./media/batch-account-create-portal/marketplace-batch.png
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[pool_allocation]: ./media/batch-account-create-portal/batch-pool-allocation.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png
