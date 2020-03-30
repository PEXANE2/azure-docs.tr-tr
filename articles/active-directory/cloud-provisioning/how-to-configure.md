---
title: Azure AD Connect bulut sağlama yeni aracı yapılandırması
description: Bu makalede, bulut sağlama nın nasıl yüklenir.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c086227d23901cb7904d8cc0a768e9f4b5ad43db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77620983"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Azure AD Connect bulut tabanlı sağlama için yeni bir yapılandırma oluşturma

Aracıyı yükledikten sonra Azure portalında oturum açmanız ve Azure Active Directory (Azure AD) Connect bulut sağlamayı yapılandırmanız gerekir. Aracıyı etkinleştirmek için aşağıdaki adımları izleyin.

## <a name="configure-provisioning"></a>Hükmü yapılandırma
Sağlamayapılandırmak için aşağıdaki adımları izleyin.

1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
1.  **Azure AD Connect'i**seçin.
1.  **Hükmü Yönet 'i (Önizleme)** seçin.

    ![Sağlamayı yönetme (Önizleme)](media/how-to-configure/manage1.png)

1.  **Yeni yapılandırmayı**seçin.
1.  Yapılandırma ekranında, şirket içi etki alanı önceden doldurulur.
1.  Bir **Bildirim e-postası**girin. Bu e-posta, sağlama nın sağlıklı olmadığı zaman bildirilir.
1.  Seçiciyi **Etkinleştir'e**taşıyın ve **Kaydet'i**seçin.

    ![Azure AD sağlama (Önizleme)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Belirli kullanıcılara ve gruplara kapsam sağlama
Aracıyı, şirket içi Etkin Dizin grupları veya kuruluş birimleri kullanarak belirli kullanıcıları ve grupları eşitlemek için kapsamda izleyebilirsiniz. Grupları ve kuruluş birimlerini yapılandırma içinde yapılandıramazsınız. 

1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
1.  **Azure AD Connect'i**seçin.
1.  **Hükmü Yönet 'i (Önizleme)** seçin.
1.  **Yapılandırma**altında, yapılandırmanızı seçin.

    ![Yapılandırma bölümü](media/how-to-configure/scope1.png)

1.  **Yapılandırma**altında, yapılandırma kuralının kapsamını değiştirmek için **Tüm kullanıcıları** seçin.

    ![Tüm kullanıcı seçeneği](media/how-to-configure/scope2.png)

1. Sağda, kapsamı yalnızca güvenlik gruplarını içerecek şekilde değiştirebilirsiniz. Grubun ayırt edici adını girin ve **Ekle'yi**seçin.

    ![Seçili güvenlik grupları seçeneği](media/how-to-configure/scope3.png)

1.  Veya kapsamı yalnızca belirli kuruluş birimlerini içerecek şekilde değiştirebilirsiniz. Bitti ve **Kaydet'i** seçin. **Save**  
2.  Kapsamı değiştirdikten sonra, değişikliklerin hemen eşitlenmesine başlamak için sağlama işlemini [yeniden başlatmanız](#restart-provisioning) gerekir.

    ![Seçili kuruluş birimleri seçeneği](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Sağlamayı yeniden başlatma 
Bir sonraki zamanlanmış çalıştırmayı beklemek istemiyorsanız, **yeniden başlat sağlama** düğmesini kullanarak yürütmeyi tetikleyin. 
1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
1.  **Azure AD Connect'i**seçin.
1.  **Hükmü Yönet 'i (Önizleme)** seçin.
1.  **Yapılandırma**altında, yapılandırmanızı seçin.

    ![Sağlamayı yeniden başlatmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

1.  En üstte, **yeniden başlat'ı**seçin.

## <a name="remove-a-configuration"></a>Yapılandırmayı kaldırma
Yapılandırmayı silmek için aşağıdaki adımları izleyin.

1.  Azure portalında **Azure Active Directory** seçeneğini belirleyin.
1.  **Azure AD Connect'i**seçin.
1.  **Hükmü Yönet 'i (Önizleme)** seçin.
1.  **Yapılandırma**altında, yapılandırmanızı seçin.

    ![Yapılandırmayı kaldırmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

1.  Yapılandırma ekranının üst kısmında **Sil'i**seçin.

    ![Sil düğmesi](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Yapılandırmayı silmeden önce onay yok. **Sil'i**seçmeden önce bunun yapmak istediğiniz eylem olduğundan emin olun.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
