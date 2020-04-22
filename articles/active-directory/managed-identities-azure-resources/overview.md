---
title: Azure kaynakları için yönetilen kimlikler
description: Azure kaynakları için yönetilen kimliklere genel bakış.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 04/18/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2231d70e6c4368a7c896f9063b58cc97ee292f53
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682585"
---
# <a name="what-are-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler nelerdir?

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bulut uygulamaları oluştururken yaygın olarak karşılaşılan bir zorluk, bulut hizmetlerinde kimlik doğrulaması yapmak için kodunuzda bulunan kimlik bilgilerinin yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olan kimlik bilgilerinin geliştirici iş istasyonlarında asla gösterilmemesi ve kaynak denetimine kaydedilmemesidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir.

Azure Active Directory (Azure AD) hizmetindeki Azure kaynakları yönetilen hizmetleri bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz.

Azure kaynakları için yönetilen kimlikler özelliği, Azure abonelikleri için Azure AD ile ücretsiz olarak sunulmaktadır. Ek ücret alınmaz.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="terminology"></a>Terminoloji

Azure kaynakları belgeleri kümesi için yönetilen kimlikler boyunca aşağıdaki terimler kullanılır:

- **Müşteri Kimliği** - Azure AD tarafından oluşturulan ve ilk sağlama sırasında bir uygulama ve hizmet ilkesine bağlı benzersiz bir tanımlayıcıdır.
- **Asıl kimlik** - Azure kaynağına rol tabanlı erişim sağlamak için kullanılan yönetilen kimliğiniz için hizmet ana nesnesinin nesne kimliği.
- **Azure Örneği Meta Veri Hizmeti (IMDS),** Azure Kaynak Yöneticisi aracılığıyla oluşturulan tüm IaaS VM'ler tarafından erişilebilen bir REST bitiş noktasıdır. Bitiş noktası, yalnızca VM içinden erişilebilen, iyi bilinen, routable olmayan BIR IP adresinde (169.254.169.254) kullanılabilir.

## <a name="how-does-the-managed-identities-for-azure-resources-work"></a>Azure kaynakları için yönetilen kimlikler nasıl çalışır?

İki tür yönetilen kimlik vardır:

- **Sistem tarafından atanan yönetilen kimlik** doğrudan bir Azure hizmet örneğinde etkinleştirilir. Kimlik etkinleştirildiğinde Azure, örneğin aboneliğinin güvendiği Azure AD kiracısında örnek için bir kimlik oluşturur. Kimlik oluşturulduktan sonra, kimlik bilgileri örneğe sağlanır. Sistem tarafından atanan kimliğin yaşam döngüsü, içinde etkinleştirildiği Azure hizmet örneğine doğrudan bağlıdır. Örnek silinirse, Azure AD'deki kimlik bilgileri ve kimlik Azure tarafından otomatik olarak temizlenir.
- **Kullanıcı tarafından atanan yönetilen kimlik**, tek başına bir Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir. Kullanıcı tarafından atanan kimliğin yaşam döngüsü, bu kimliğin atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir.

Dahili olarak, yönetilen kimlikler yalnızca Azure kaynaklarıyla kullanılmak üzere kilitlenmiş özel bir türdeki hizmet ilkeleridir. Yönetilen kimlik silindiğinde, ilgili hizmet sorumlusu otomatik olarak kaldırılır.
Ayrıca, Kullanıcı Tarafından Atanan veya Sisteme Atanan Kimlik oluşturulduğunda, Yönetilen Kimlik Kaynak Sağlayıcısı (MSRP) bu kimliğe dahili olarak bir sertifika verir. 

Kodunuzda Azure AD kimlik doğrulamasını destekleyen hizmetler için erişim belirteci istemek üzere yönetilen kimlik kullanılabilir. Hizmet örneği tarafından kullanılan kimlik bilgilerinin dağıtımıyla Azure ilgilenir. 

## <a name="credential-rotation"></a>Kimlik Bilgisi Döndürme
Kimlik bilgisi döndürme, Azure kaynağını barındıran kaynak sağlayıcısı tarafından denetlenir. Kimlik bilgisinin varsayılan dönüşü her 46 günde bir gerçekleşir. Kaynak sağlayıcısının 46 günden daha uzun süre beklemesi için yeni kimlik bilgileri çağırmak kaynak sağlayıcısına kalmıştır.

Aşağıdaki diyagramda yönetilen hizmet kimliklerinin Azure sanal makineleriyle (VM) nasıl çalıştığı gösterilmektedir:

![Yönetilen hizmet kimlikleri ve Azure VM’leri](media/overview/data-flow.png)

|  Özellik    | Sistem le atanmış yönetilen kimlik | Kullanıcı tarafından atanan yönetilen kimlik |
|------|----------------------------------|--------------------------------|
| Oluşturma |  Azure kaynağının bir parçası olarak oluşturulmuş (örneğin, bir Azure sanal makinesi veya Azure Uygulama Hizmeti) | Tek başına bir Azure kaynağı olarak oluşturuldu |
| Yaşam döngüsü | Yönetilen kimliğin oluşturulduğu Azure kaynağıyla paylaşılan yaşam döngüsü. <br/> Üst kaynak silindiğinde, yönetilen kimlik de silinir. | Bağımsız yaşam döngüsü. <br/> Açıkça silinmelidir. |
| Azure kaynakları arasında paylaşım | Paylaştırılamaz. <br/> Yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir. | Paylaşılabilir <br/> Aynı kullanıcı tarafından atanan yönetilen kimlik, birden fazla Azure kaynağıyla ilişkilendirilebilir. |
| Genel kullanım örnekleri | Tek bir Azure kaynağında bulunan iş yükleri <br/> Bağımsız kimliklere ihtiyaç duyduğunuz iş yükleri. <br/> Örneğin, tek bir sanal makinede çalışan bir uygulama | Birden çok kaynakta çalışan ve tek bir kimliği paylaşabilen iş yükleri. <br/> Sağlayan bir akışın parçası olarak güvenli bir kaynağa ön yetkilendirme gerektiren iş yükleri. <br/> Kaynakların sık sık geri dönüştürüldüğü, ancak izinlerin tutarlı kaldığı iş yükleri. <br/> Örneğin, birden çok sanal makinenin aynı kaynağa erişmeihtiyacı olan bir iş yükü |

### <a name="how-a-system-assigned-managed-identity-works-with-an-azure-vm"></a>Sistem tarafından atanmış yönetilen kimliğin Azure VM ile çalışma şekli

1. Azure Resource Manager sanal makinede sistem tarafından atanan yönetilen kimliği etkinleştirmek için bir istek alır.

2. Azure Resource Manager, Azure AD'de sanal makinenin kimliği için bir hizmet sorumlusu oluşturur. Hizmet sorumlusu, bu abonelik tarafından güvenilen Azure AD kiracısında oluşturulur.

3. Azure Kaynak Yöneticisi, Azure Örneği Meta veri hizmeti kimlik bitiş noktasını hizmet yöneticisi kimliği ve sertifikasıyla güncelleyerek VM'deki kimliği yapılandırır.

4. VM bir kimliğe sahip olduktan sonra hizmet sorumlusunu kullanarak Azure kaynaklarına VM erişimi sağlayabilirsiniz. Azure Resource Manager'ı çağırmak için Azure AD'de rol tabanlı erişim denetimini (RBAC) kullanarak VM hizmet sorumlusuna uygun rolü atayabilirsiniz. Key Vault'u çağırmak için kodunuza Key Vault'ta belirli bir gizli diziye veya anahtara erişim verebilirsiniz.

5. VM'de çalışan kodunuz, yalnızca VM içinden erişilebilen Azure Örneği Meta veri hizmeti bitiş noktasından bir belirteç isteyebilir:`http://169.254.169.254/metadata/identity/oauth2/token`
    - Resource parametresi belirtecin gönderildiği hizmeti belirtir. Azure Resource Manager ile kimlik doğrulaması için `resource=https://management.azure.com/` kullanın.
    - API version parametresi IMDS sürümünü belirtir; api-version=2018-02-01 veya üstünü kullanın.

6. Azure AD'ye erişim belirteci isteyen bir çağrı yapılır (5. adımda belirtildiği gibi) ve bu çağrıda 3. adımda yapılandırılan istemci kimliği ve sertifikası kullanılır. Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.

7. Kodunuz erişim belirtecini bir çağrıda Azure AD kimlik doğrulamasını destekleyen hizmete gönderir.

### <a name="how-a-user-assigned-managed-identity-works-with-an-azure-vm"></a>Kullanıcı tarafından atanmış yönetilen kimliğin Azure VM ile çalışma şekli

1. Azure Resource Manager kullanıcı tarafından atanan bir yönetilen kimlik oluşturma isteği alır.

2. Azure Resource Manager, Azure AD'de kullanıcı tarafından atanan yönetilen kimlik için bir hizmet sorumlusu oluşturur. Hizmet sorumlusu, bu abonelik tarafından güvenilen Azure AD kiracısında oluşturulur.

3. Azure Kaynak Yöneticisi, kullanıcı tarafından atanan yönetilen kimliği bir VM'de yapılandırmak için bir istek alır ve Azure Örneği Meta veri hizmeti kimlik bitiş noktasını kullanıcı tarafından atanan yönetilen kimlik hizmeti asıl istemci kimliği ve sertifikasıyla güncelleştirir.

4. Kullanıcı tarafından atanan yönetilen kimlik oluşturulduktan sonra hizmet sorumlusu bilgilerini kullanarak Azure kaynaklarına erişim verir. Azure Resource Manager'ı çağırmak için Azure AD'de RBAC özelliğini kullanarak kullanıcı tarafından atanan kimliğin hizmet sorumlusuna uygun rolü atayabilirsiniz. Key Vault'u çağırmak için kodunuza Key Vault'ta belirli bir gizli diziye veya anahtara erişim verebilirsiniz.

   > [!Note]
   > Bu adımı 3. adımdan önce de gerçekleştirebilirsiniz.

5. VM'de çalışan kodunuz, yalnızca VM içinden erişilebilen Azure Örneği Meta veri hizmeti kimlik bitiş noktasından bir belirteç isteyebilir:`http://169.254.169.254/metadata/identity/oauth2/token`
    - Resource parametresi belirtecin gönderildiği hizmeti belirtir. Azure Resource Manager ile kimlik doğrulaması için `resource=https://management.azure.com/` kullanın.
    - Client ID parametresi belirtecin hangi kimlik için istendiğini belirtir. Tek bir sanal makinede birden çok kullanıcı tarafından atanan kimlik olduğunda, belirsizliği ortadan kaldırmak için bu değer gereklidir.
    - API version parametresi, Azure Instance Metadata Service sürümünü belirtir. `api-version=2018-02-01` veya daha yüksek bir değer kullanın.

6. Azure AD'ye erişim belirteci isteyen bir çağrı yapılır (5. adımda belirtildiği gibi) ve bu çağrıda 3. adımda yapılandırılan istemci kimliği ve sertifikası kullanılır. Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuz erişim belirtecini bir çağrıda Azure AD kimlik doğrulamasını destekleyen hizmete gönderir.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri nasıl kullanabilirim?

Yönetilen kimlikleri farklı Azure kaynaklarına erişme amacıyla kullanmayı öğrenmek için aşağıdaki öğreticileri deneyin.

> [!NOTE]
> Desteklenen çeşitli senaryoların ayrıntılı video gözden geçirmeleri de dahil olmak üzere yönetilen kimlikler hakkında daha fazla bilgi için [Microsoft Azure Kaynakları için Yönetilen Kimlikler](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing) Uygulaması kursuna göz atın.

Yönetilen kimlikleri Windows VM ile kullanmayı öğrenin:

* [Azure Data Lake Store’a erişme](tutorial-windows-vm-access-datalake.md)
* [Azure Resource Manager’a erişme](tutorial-windows-vm-access-arm.md)
* [Azure SQL’e erişme](tutorial-windows-vm-access-sql.md)
* [Erişim anahtarı kullanarak Azure Depolama’ya erişme](tutorial-vm-windows-access-storage.md)
* [Paylaşılan erişim anahtarlarını kullanarak Azure Depolama’ya erişme](tutorial-windows-vm-access-storage-sas.md)
* [Azure Key Vault ile Azure AD dışı bir kaynağa erişme](tutorial-windows-vm-access-nonaad.md)

Yönetilen kimlikleri Linux VM ile kullanmayı öğrenin:

* [Azure Container Registry’ye erişme](../../container-registry/container-registry-authentication-managed-identity.md)
* [Azure Data Lake Store’a erişme](tutorial-linux-vm-access-datalake.md)
* [Azure Resource Manager’a erişme](tutorial-linux-vm-access-arm.md)
* [Erişim anahtarı kullanarak Azure Depolama’ya erişme](tutorial-linux-vm-access-storage.md)
* [Paylaşılan erişim anahtarlarını kullanarak Azure Depolama’ya erişme](tutorial-linux-vm-access-storage-sas.md)
* [Azure Key Vault ile Azure AD dışı bir kaynağa erişme](tutorial-linux-vm-access-nonaad.md)

Yönetilen kimlikleri diğer Azure hizmetleri ile kullanmayı öğrenin:

* [Azure App Service](/azure/app-service/overview-managed-identity)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)
* [Azure Container Instances](../../container-instances/container-instances-managed-identity.md)
* [Azure Container Registry Görevleri](../../container-registry/container-registry-tasks-authentication-managed-identity.md)
* [Azure Event Hubs](../../event-hubs/authenticate-managed-identity.md)
* [Azure İşlevleri](/azure/app-service/overview-managed-identity)
* [Azure Kubernetes Service](/azure/aks/use-managed-identity)
* [Azure Logic Apps](/azure/logic-apps/create-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)
* [Azure Data Factory](../../data-factory/data-factory-service-identity.md)


## <a name="what-azure-services-support-the-feature"></a>Bu özelliği hangi Azure hizmetleri destekliyor?<a name="which-azure-services-support-managed-identity"></a>

Azure kaynakları için yönetilen kimlikler, Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmak için kullanılabilir. Azure kaynakları için yönetilen kimlikler özelliğini destekleyen Azure hizmetlerinin listesi için bkz. [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](services-support-msi.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları için yönetilen kimlikler özelliğini kullanmaya başlamak için aşağıdaki hızlı başlangıçlardan yararlanın:

* [Resource Manager’a erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-windows-vm-access-arm.md)
* [Resource Manager’a erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-linux-vm-access-arm.md)
