---
title: Azure için yönetilen kimlikler
description: Service Fabric ile Azure için Yönetilen kimlikleri kullanma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 06ebcfdf3d6a3815908752153acb09437d745d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986759"
---
# <a name="using-managed-identities-for-azure-with-service-fabric-preview"></a>Hizmet Kumaşı ile Azure için Yönetilen kimlikleri kullanma (Önizleme)

Bulut uygulamaları oluşturmak, çeşitli hizmetlere kimlik doğrulama için kodunuzdaki kimlik bilgilerini, geliştirici iş istasyonunda veya kaynak denetiminde yerel olarak kaydetmeden güvenli bir şekilde yönetmektir. *Azure için yönetilen kimlikler,* Azure Etkin Dizini'ndeki (Azure AD) tüm kaynaklarınız için bu sorunu, Azure AD içinde otomatik olarak yönetilen kimlikler sağlayarak çözer. Anahtarı Atlama da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini, kodunuzda depolanan herhangi bir kimlik belgesi olmadan kullanabilirsiniz.

*Azure kaynakları için yönetilen kimlikler,* Azure abonelikleri için Azure AD ile ücretsizdir. Ek ücret alınmaz.

> [!NOTE]
> *Azure için yönetilen kimlikler,* eskiden Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="concepts"></a>Kavramlar

Azure için yönetilen kimlikler birkaç temel kavrama dayanmaktadır:

- **Müşteri Kimliği** - Azure AD tarafından oluşturulan ve ilk sağlama sırasında bir uygulama ve hizmet ilkesine bağlı benzersiz bir tanımlayıcı (ayrıca [bkz. uygulama kimliği](/azure/active-directory/develop/developer-glossary#application-id-client-id).)

- **Asıl Kimlik** - Yönetilen Kimliğiniz için hizmet ana nesnesinin nesne kimliği, bir Azure kaynağına rol tabanlı erişim sağlamak için kullanılır.

- **Hizmet Sorumlusu** - Belirli bir kiracıda bir AAD uygulamasının projeksiyonunu temsil eden bir Azure Etkin Dizin [nesnesi](../active-directory/develop/developer-glossary.md#service-principal-object)(ayrıca bkz. hizmet sorumlusu .)

İki tür yönetilen kimlik vardır:

- **Sistem tarafından atanmış yönetilen kimlik,** doğrudan bir Azure hizmet örneğinde etkinleştirilir.  Sistemle atanmış bir kimliğin yaşam döngüsü, etkinleştirilen Azure hizmet örneğine özgüdür.
- **Kullanıcı tarafından atanan yönetilen kimlik**, tek başına bir Azure kaynağı olarak oluşturulur. Kimlik bir veya daha fazla Azure hizmet örneğine atanabilir ve bu örneklerin yaşam döngülerinden ayrı olarak yönetilir.

Yönetilen kimlik türleri arasındaki farkı daha iyi anlamak için Azure [kaynakları için yönetilen kimliklerin nasıl çalıştığını](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) görmek için bkz.

## <a name="supported-scenarios-for-service-fabric-applications"></a>Service Fabric uygulamaları için desteklenen senaryolar

Hizmet Kumaşı için yönetilen kimlikler yalnızca Azure tarafından dağıtılan Hizmet Kumaşı kümelerinde ve yalnızca Azure kaynağı olarak dağıtılan uygulamalar için desteklenir; Azure kaynağı olarak dağıtılmayan bir uygulamaya kimlik atanamaz. Kavramsal olarak konuşursak, Azure Hizmet Kumaşı kümesinde yönetilen kimlikler için destek iki aşamadan oluşur:

1. Uygulama kaynağına bir veya daha fazla yönetilen kimlik atama; bir uygulamaya, sırasıyla sistem tarafından atanmış tek bir kimlik ve/veya en fazla 32 kullanıcı tarafından atanmış kimlik atanabilir.

2. Uygulamanın tanımı içinde, başvuruyu oluşturan herhangi bir bireysel hizmete uygulamaya atanan kimliklerden birini haritala.

Bir uygulamanın sistem tarafından atanan kimliği bu uygulamaya özgüdür; kullanıcı tarafından atanan kimlik, birden çok uygulamaya atanabilecek bağımsız bir kaynaktır. Bir uygulama içinde, tek bir kimlik (sistem atanmış veya kullanıcı tarafından atanmış olsun) uygulamanın birden çok hizmetine atanabilir, ancak her hizmete yalnızca bir kimlik atanabilir. Son olarak, bir hizmete bu özelliğe erişebilmek için açıkça bir kimlik atanması gerekir. Sonuç olarak, bir uygulamanın kimliklerinin kurucu hizmetlerle eşlenemeuygulamasının yalıtımına izin verir — bir hizmet yalnızca eşlenen kimliği kullanabilir.  

Şu anda, aşağıdaki senaryolar bu önizleme özelliği için desteklenir:

- Bir veya daha fazla hizmet ve bir veya daha fazla atanmış kimlikiçeren yeni bir uygulama dağıtma

- Azure kaynaklarına erişmek için varolan (Azure tarafından dağıtılan) bir uygulamaya bir veya daha fazla yönetilen kimlik atama

Aşağıdaki senaryolar desteklenmez veya önerilmez; bu eylemlerin engellenmeyebileceğini, ancak uygulamalarınızda kesintilere yol açabileceğini unutmayın:

- Bir uygulamaya atanan kimlikleri kaldırma veya değiştirme; değişiklik yapmak zorundaysanız, önce yeni bir kimlik ataması eklemek ve daha sonra daha önce atanmış bir atamayı kaldırmak için ayrı dağıtımlar gönderin. Bir kimliğin varolan bir uygulamadan kaldırılması, uygulamanızı yükseltilemez bir durumda bırakmak da dahil olmak üzere istenmeyen etkilere sahip olabilir. Bir kimliğin kaldırılması gerekiyorsa, uygulamanın tamamen silinebilir; bu uygulama ile ilişkili sistem atanmış kimlik (eğer öyleyse) siler ve uygulamaya atanan kullanıcı tarafından atanan kimlikler ile herhangi bir çağrışım kaldıracaktır unutmayın.

- Yönetilen kimlikler için Hizmet Kumaşı desteği şu anda [AzureServiceTokenProvider'a](../key-vault/service-to-service-authentication.md)entegre edilmez; tümleştirme, yönetilen kimlik özelliği için önizleme döneminin sonuna kadar elde edilecektir.

>
> [!NOTE]
>
> Bu özellik önizlemede. Sık sık değişikliklere maruz kalabilir ve üretim dağıtımları için uygun olmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen kimlik desteğiyle yeni bir Azure Hizmet Kumaşı kümesini dağıtma](./configure-new-azure-service-fabric-enable-managed-identity.md)
- [Varolan bir Azure Hizmet Kumaşı kümesinde yönetilen kimlik desteğini etkinleştirme](./configure-existing-cluster-enable-managed-identity-token-service.md)
- [Sistem tarafından atanmış yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulamasını dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
- [Kullanıcı tarafından atanan yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
- [Hizmet kodundan Hizmet Kumaşı uygulamasının yönetilen kimliğinden yararlanma](./how-to-managed-identity-service-fabric-app-code.md)
- [Azure Hizmet Kumaşı uygulamasına diğer Azure kaynaklarına erişim hakkı verme](./how-to-grant-access-other-resources.md)
- [Uygulama sırlarını KeyVaultReferences olarak bildirme ve kullanma](./service-fabric-keyvault-references.md)
