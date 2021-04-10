---
title: Azure kaynakları için yönetilen kimlikler
description: Azure kaynakları için yönetilen kimliklere genel bakış.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 10/06/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5390811c8da4a8cace32e0e7ba4524e8c537a26a
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106055595"
---
# <a name="what-are-managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler nelerdir?

Geliştiriciler için yaygın bir zorluk, farklı hizmetler arasındaki iletişimin güvenliğini sağlamak için gizli dizileri ve kimlik bilgilerini yönetiledir. Azure 'da Yönetilen kimlikler, Azure AD 'de Azure kaynağı için bir kimlik sağlayarak ve Azure Active Directory (Azure AD) belirteçleri elde etmek üzere kullanarak kimlik bilgilerini yönetmek zorunda olan geliştiricilerin gereksinimini ortadan kaldırır. Bu Ayrıca, geliştiricilerin kimlik bilgilerini güvenli bir şekilde depolayabilecekleri [Azure Key Vault](../../key-vault/general/overview.md) erişilmesine de yardımcı olur. Azure kaynakları için Yönetilen kimlikler, Azure AD 'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlayarak bu sorunu çözer.

Yönetilen bir kimlik ne için kullanılabilir?

   > [!VIDEO https://www.youtube.com/embed/5lqayO_oeEo]

Yönetilen kimlikler kullanmanın avantajlarından bazıları şunlardır:

- Kimlik bilgilerini yönetmeniz gerekmez. Kimlik bilgilerine, sizin için de erişilebilir.
- Azure Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir Azure hizmetinde kimlik doğrulaması yapmak için Yönetilen kimlikler kullanabilirsiniz.
- Yönetilen kimlikler herhangi bir ek maliyet olmadan kullanılabilir.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="managed-identity-types"></a>Yönetilen kimlik türleri

İki tür yönetilen kimlik vardır:

- **Sistem tarafından atanan** Bazı Azure Hizmetleri, yönetilen bir kimliği doğrudan bir hizmet örneği üzerinde etkinleştirmenizi sağlar. Sistem tarafından atanan bir yönetilen kimliği etkinleştirdiğinizde, Azure AD 'de bu hizmet örneğinin yaşam döngüsüne bağlı bir kimlik oluşturulur. Bu nedenle, kaynak silindiğinde Azure kimliği sizin için otomatik olarak siler. Tasarım yaparak, yalnızca bu Azure kaynağı Azure AD 'den belirteç istemek için bu kimliği kullanabilir.
- **Kullanıcı tarafından atanan** Ayrıca, tek başına Azure kaynağı olarak yönetilen bir kimlik de oluşturabilirsiniz. [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturabilir](how-to-manage-ua-identity-portal.md) ve bunu bir Azure hizmetinin bir veya daha fazla örneğine atayabilirsiniz. Kullanıcı tarafından atanan Yönetilen kimlikler söz konusu olduğunda, kimlik onu kullanan kaynaklardan ayrı olarak yönetilir. </br></br>

  > [!VIDEO https://www.youtube.com/embed/OzqpxeD3fG0]

Aşağıdaki tabloda, iki tür yönetilen kimliğin arasındaki farklar gösterilmektedir.

|  Özellik    | Sistem tarafından atanan yönetilen kimlik | Kullanıcı tarafından atanan yönetilen kimlik |
|------|----------------------------------|--------------------------------|
| Oluşturma |  Bir Azure kaynağının parçası olarak oluşturulur (örneğin, bir Azure sanal makinesi veya Azure App Service) | Tek başına Azure kaynağı olarak oluşturulur |
| Yaşam döngüsü | Yönetilen kimliğin birlikte oluşturulduğu Azure kaynağıyla paylaşılan yaşam döngüsü. <br/> Üst kaynak silindiğinde, yönetilen kimlik de silinir. | Bağımsız yaşam döngüsü. <br/> Açıkça silinmelidir. |
| Azure kaynakları arasında paylaşım | Paylaştırılamaz. <br/> Yalnızca tek bir Azure kaynağıyla ilişkilendirilebilir. | Paylaşılabilir <br/> Aynı kullanıcı tarafından atanan yönetilen kimlik birden fazla Azure kaynağıyla ilişkilendirilebilir. |
| Genel kullanım örnekleri | Tek bir Azure kaynağı içinde yer alan iş yükleri <br/> Bağımsız kimliklere ihtiyacınız olan iş yükleri. <br/> Örneğin, tek bir sanal makinede çalışan bir uygulama | Birden çok kaynak üzerinde çalışan ve tek bir kimliği paylaşabilen iş yükleri. <br/> Sağlama akışının bir parçası olarak güvenli bir kaynağa ön kimlik doğrulaması gerektiren iş yükleri. <br/> Kaynakların sık geri dönüştürüleceği iş yükleri, ancak izinlerin tutarlı kalması gerekir. <br/> Örneğin, birden çok sanal makinenin aynı kaynağa erişmesi gereken bir iş yükü |

>[!IMPORTANT]
>Bir yönetilen kimliği seçtiğiniz kimlik türünden bağımsız olarak, yalnızca Azure kaynaklarıyla kullanılabilecek özel bir türün hizmet sorumlusu olur. Yönetilen kimlik silindiğinde, buna karşılık gelen hizmet sorumlusu otomatik olarak kaldırılır.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri nasıl kullanabilirim?

![bir geliştiricinin kimlik doğrulama bilgilerini yönetmeksizin koddan kaynaklara erişim sağlamak için yönetilen kimlikleri nasıl kullanabileceği hakkında bazı örnekler](media/overview/azure-managed-identities-examples.png)

## <a name="what-azure-services-support-the-feature"></a>Bu özelliği hangi Azure hizmetleri destekliyor?<a name="which-azure-services-support-managed-identity"></a>

Azure kaynakları için yönetilen kimlikler, Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmak için kullanılabilir. Azure kaynakları için yönetilen kimlikler özelliğini destekleyen Azure hizmetlerinin listesi için bkz. [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](./services-support-managed-identities.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Resource Manager’a erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-windows-vm-access-arm.md)
* [Resource Manager’a erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-linux-vm-access-arm.md)
* [App Service ve Azure Işlevleri için Yönetilen kimlikler kullanma](../../app-service/overview-managed-identity.md)
* [Azure Container Instances ile yönetilen kimlikleri kullanma](../../container-instances/container-instances-managed-identity.md)
* [Microsoft Azure kaynakları Için Yönetilen kimlikler uygulama](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing).