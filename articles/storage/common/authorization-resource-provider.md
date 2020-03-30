---
title: Yönetim kaynaklarına erişmek için Azure Depolama kaynak sağlayıcısını kullanın
description: Azure Depolama kaynak sağlayıcısı, Azure Depolama için yönetim kaynaklarına erişim sağlayan bir hizmettir. Azure Depolama kaynak sağlayıcısını, depolama hesapları, özel uç noktalar ve hesap erişim anahtarları gibi kaynakları oluşturmak, güncelleştirmek, yönetmek ve silmek için kullanabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f5d42a6a0567d3949bc4b0fb1947450a9c957f18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972358"
---
# <a name="use-the-azure-storage-resource-provider-to-access-management-resources"></a>Yönetim kaynaklarına erişmek için Azure Depolama kaynak sağlayıcısını kullanın

Azure Resource Manager, Azure için dağıtım ve yönetim hizmetidir. Azure Depolama kaynak sağlayıcısı, Azure Kaynak Yöneticisi'ni temel alan ve Azure Depolama için yönetim kaynaklarına erişim sağlayan bir hizmettir. Azure Depolama kaynak sağlayıcısını, depolama hesapları, özel uç noktalar ve hesap erişim anahtarları gibi kaynakları oluşturmak, güncelleştirmek, yönetmek ve silmek için kullanabilirsiniz. Azure Kaynak Yöneticisi hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](/azure/azure-resource-manager/resource-group-overview)bilgisi ne bürünülsin.

Bir depolama hesabı oluşturma veya silme veya abonelikteki depolama hesaplarının listesini alma gibi eylemleri gerçekleştirmek için Azure Depolama kaynak sağlayıcısını kullanabilirsiniz. Talepleri Azure Depolama kaynak sağlayıcısına karşı yetkilendirmek için Azure Etkin Dizin (Azure AD) kullanın. Bu makalede, yönetim kaynaklarına izinlerin nasıl atandığı açıklanmaktadır ve Azure Depolama kaynak sağlayıcısına karşı nasıl istekte bulunur gösteriş yapan örneklere işaret eder.

## <a name="management-resources-versus-data-resources"></a>Yönetim kaynakları ve veri kaynakları

Microsoft, Azure Depolama kaynaklarıyla çalışmak için iki REST API'si sağlar. Bu API'ler, Azure Depolama'ya karşı gerçekleştirebileceğiniz tüm eylemlerin temelini oluşturur. Azure Depolama REST API,blob, kuyruk, dosya ve tablo verileri de dahil olmak üzere depolama hesabınızdaki verilerle çalışmanızı sağlar. Azure Depolama kaynak sağlayıcısı REST API, depolama hesabı ve ilgili kaynaklarla çalışmanıza olanak tanır.

Blob verilerini okuyan veya yazan bir istek, yönetim işlemi gerçekleştiren bir istekten farklı izinler gerektirir. RBAC, her iki kaynak türüne izinler üzerinde ince taneli denetim sağlar. Bir güvenlik ilkesine bir RBAC rolü atadığınızda, bu müdürün hangi izinlerin verolacağını anladığınızdan emin olun. Yerleşik RBAC rolüyle hangi eylemlerin ilişkili olduğunu açıklayan ayrıntılı bir başvuru için Azure [kaynakları için Yerleşik rollere](../../role-based-access-control/built-in-roles.md)bakın.

Azure Depolama, Blob ve Queue depolamasına karşı istekleri yetkilendirmek için Azure AD'yi kullanmayı destekler. Blob ve sıra veri işlemleri için RBAC rolleri hakkında bilgi için [bkz.](storage-auth-aad.md)

## <a name="assign-management-permissions-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile yönetim izinleri atama

Her Azure aboneliğinde kullanıcıları, grupları ve uygulamaları yöneten ilişkili bir Azure Etkin Dizini vardır. Bir kullanıcı, grup veya uygulama, [Microsoft kimlik platformu](/azure/active-directory/develop/)bağlamında güvenlik ilkesi olarak da adlandırılır. Rol tabanlı erişim denetimi (RBAC) kullanarak Active Directory'de tanımlanan bir güvenlik ilkesine abonelikteki kaynaklara erişim izni verebilirsiniz.

Bir güvenlik ilkesine bir RBAC rolü atadığınızda, rol tarafından verilen izinlerin geçerli olduğu kapsamı da belirtirsiniz. Yönetim işlemleri için, abonelik düzeyinde bir rol atayabilirsiniz, kaynak grubu veya depolama hesabı. [Azure portalı,](https://portal.azure.com/) [Azure CLI araçları](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs)veya Azure Depolama kaynak sağlayıcısı REST [API'yi](/rest/api/storagerp)kullanarak bir güvenlik ilkesine RBAC rolü atayabilirsiniz.

RBAC hakkında daha fazla bilgi için Azure kaynakları için rol tabanlı erişim [Classic subscription administrator roles, Azure RBAC roles, and Azure AD administrator roles](../../role-based-access-control/rbac-and-directory-admin-roles.md) [denetimi (RBAC) nedir?](../../role-based-access-control/overview.md)

### <a name="built-in-roles-for-management-operations"></a>Yönetim işlemleri için yerleşik roller

Azure, yönetim işlemlerini çağırmak için izin veren yerleşik roller sağlar. Azure Depolama, Azure Depolama kaynak sağlayıcısında kullanılmak üzere özel olarak yerleşik roller de sağlar.

Depolama yönetimi operasyonlarını çağırmak için izin veren yerleşik roller, aşağıdaki tabloda açıklanan rolleri içerir:

|    RBAC rolü    |    Açıklama    |    Hesap anahtarlarına erişimi içerir mi?    |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| **Sahibi** | Tüm depolama kaynaklarını ve kaynaklara erişimi yönetebilirsiniz.  | Evet, depolama hesabı anahtarlarını görüntülemek ve yeniden oluşturmak için izinler sağlar. |
| **Katkıda Bulunan**  | Tüm depolama kaynaklarını yönetebilir, ancak kaynaklara atamayı yönetemez. | Evet, depolama hesabı anahtarlarını görüntülemek ve yeniden oluşturmak için izinler sağlar. |
| **Okuyucu** | Depolama hesabı yla ilgili bilgileri görüntüleyebilir, ancak hesap anahtarlarını görüntüleyemez. | Hayır. |
| **Depolama Hesabı Katılımcısı** | Depolama hesabını yönetebilir, aboneliğin kaynak grupları ve kaynakları hakkında bilgi alabilir ve abonelik kaynak grubu dağıtımları oluşturabilir ve yönetebilir. | Evet, depolama hesabı anahtarlarını görüntülemek ve yeniden oluşturmak için izinler sağlar. |
| **Kullanıcı Erişimi Yöneticisi** | Depolama hesabına erişimi yönetebilirsiniz.   | Evet, bir güvenlik müdürünün kendilerine ve başkalarına izin atamasını izin verir. |
| **Sanal Makine Katılımcısı** | Sanal makineleri yönetebilir, ancak bağlı oldukları depolama hesabını yönetemez.   | Evet, depolama hesabı anahtarlarını görüntülemek ve yeniden oluşturmak için izinler sağlar. |

Tablodaki üçüncü sütun, yerleşik rolün **Microsoft.Storage/storageAccounts/listkeys/action'ı**destekleyip desteklemediğini gösterir. Bu eylem, depolama hesabı anahtarlarını okuma ve yenileme izinleri verir. Azure Depolama yönetim kaynaklarına erişim izinleri, verilere erişim izinlerini de içermez. Ancak, bir kullanıcı hesap anahtarlarına erişebiliyorsa, Paylaşılan Anahtar yetkilendirmesi aracılığıyla Azure Depolama verilerine erişmek için hesap anahtarlarını kullanabilir.

### <a name="custom-roles-for-management-operations"></a>Yönetim işlemleri için özel roller

Azure, yönetim kaynaklarına erişmek için özel RBAC rollerinin tanımlanmasını da destekler. Özel roller hakkında daha fazla bilgi için [Azure kaynakları için Özel rolleri](../../role-based-access-control/custom-roles.md)görün.

## <a name="code-samples"></a>Kod örnekleri

Azure Depolama yönetim kitaplıklarından yönetim işlemlerini nasıl onaylayıp çağıracaklarını gösteren kod örnekleri için aşağıdaki örneklere bakın:

- [.NET](https://github.com/Azure-Samples/storage-dotnet-resource-provider-getting-started)
- [Java](https://github.com/Azure-Samples/storage-java-manage-storage-accounts)
- [Node.js](https://github.com/Azure-Samples/storage-node-resource-provider-getting-started)
- [Python](https://github.com/Azure-Samples/storage-python-manage)

## <a name="azure-resource-manager-versus-classic-deployments"></a>Azure Kaynak Yöneticisi ve klasik dağıtımlar

Resource Manager ve klasik dağıtım modelleri, Azure çözümlerinizi dağıtmanın ve yönetmenin iki farklı yolunu temsil eder. Microsoft, yeni bir depolama hesabı oluşturduğunuzda Azure Kaynak Yöneticisi dağıtım modelini kullanmanızı önerir. Mümkünse, Microsoft, Kaynak Yöneticisi modeliyle varolan klasik depolama hesaplarını yeniden oluşturmanızı da önerir. Klasik dağıtım modelini kullanarak bir depolama hesabı oluşturabiliyor olsanız da, klasik model daha az esnektir ve sonunda amortismana hazır olacaktır.

Azure dağıtım modelleri hakkında daha fazla bilgi için [Kaynak Yöneticisi ve klasik dağıtım](../../azure-resource-manager/management/deployment-models.md)abakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kaynak Yöneticisi'ne genel bakış](/azure/azure-resource-manager/resource-group-overview)
- [Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md)
- [Azure Depolama kaynak sağlayıcısı için ölçeklenebilirlik hedefleri](scalability-targets-resource-provider.md)
