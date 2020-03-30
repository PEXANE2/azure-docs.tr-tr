---
title: Azure Hizmet Yönetimi Sertifikası Yükleme | Microsoft Dokümanlar
description: Azure portalı için Hizmet Yönetimi sertifikasını nasıl yükleydiğinizi öğrenin.
services: cloud-services
documentationcenter: .net
author: georgewallace
manager: carmonm
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: article
ms.date: 08/01/2017
ms.author: gwallace
ms.openlocfilehash: 4b49a9b391eeca2d2e249b171d99f231bda6fdff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329129"
---
# <a name="upload-an-azure-service-management-certificate"></a>Azure Hizmet Yönetimi Sertifikası Yükleme
Yönetim sertifikaları, Azure tarafından sağlanan klasik dağıtım modeliyle kimlik doğrulaması nızı belirlemenize olanak tanır. Birçok program ve araç (Visual Studio veya Azure SDK gibi), çeşitli Azure hizmetlerinin yapılandırılmasını ve dağıtımını otomatik hale getirmek için bu sertifikaları kullanır. 

> [!WARNING]
> Dikkat et! Bu tür sertifikalar, kimlik doğrulaması yapan herkesin ilişkili oldukları aboneliği yönetmesine olanak sağlar.
>
>

Azure sertifikaları hakkında daha fazla bilgi (kendi imzalanmış sertifika oluşturma dahil) hakkında daha fazla bilgi istiyorsanız, [Azure Bulut Hizmetleri için Sertifikalara genel bakış'a](cloud-services/cloud-services-certs-create.md#what-are-management-certificates)bakın.

Müşteri kodunu otomasyon amacıyla doğrulamak için [Azure Active Directory'yi](https://azure.microsoft.com/services/active-directory/) de kullanabilirsiniz.

**Not:** Yönetim Sertifikaları kapsamındaki işlemleri gerçekleştirmek için abonelikte Yardımcı yönetici olmalısınız. Yeni Azure Portalından Yardımcı Yönetici Ekleme veya Kaldırma Hakkında [Daha Fazla Bilgi Edinin](https://go.microsoft.com/fwlink/?linkid=849300) 

## <a name="upload-a-management-certificate"></a>Yönetim sertifikası yükleme
Bir yönetim sertifikası oluşturulduktan sonra, (.cer dosyası sadece ortak anahtar) portala yükleyebilirsiniz. Sertifika portalda kullanılabilir olduğunda, eşleşen bir sertifikaya (özel anahtar) sahip olan herkes Yönetim API'si üzerinden bağlantı kurabilir ve ilişkili abonelik kaynaklarına erişebilir.

1. [Azure portalına](https://portal.azure.com)giriş yapın.
2. Alttaki Azure hizmet listesindeki **Tüm hizmetleri** tıklatın ve ardından _Genel_ hizmet grubunda **Abonelikler'i** seçin.

    ![Abonelik menüsü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Sertifikayla ilişkilendirmek istediğiniz doğru aboneliği seçtiğinizden emin olun.     
4. Doğru aboneliği seçtikten sonra _Ayarlar_ grubunda **Yönetim sertifikalarına** basın.

    ![Ayarlar](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. **Yükle** düğmesine basın.

    ![Sertifikalar sayfasına yükleme](./media/azure-api-management-certs/certificates_page.png)
6. İletişim bilgilerini doldurun ve **Yükle'ye**basın.

    ![Ayarlar](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bir abonelikle ilişkili bir yönetim sertifikanız olduğuna göre, (eşleşen sertifikayı yerel olarak yükledikten sonra) [klasik dağıtım modeli REST API'ye](/azure/?pivot=sdkstools) programlı olarak bağlanabilir ve bu abonelikle ilişkili çeşitli Azure kaynaklarını otomatikleştirebilirsiniz.
