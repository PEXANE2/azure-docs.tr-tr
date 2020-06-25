---
title: Azure AD Connect bulut sağlama yeni aracı yapılandırması
description: Bu makalede, bulut sağlama 'nın nasıl yükleneceği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50f02ea42bb792320da6e2523b733f09afd412a0
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85360971"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Azure AD Connect bulut tabanlı sağlama için yeni bir yapılandırma oluşturma

Aracıyı yükledikten sonra, Azure portal oturum açmanız ve Azure Active Directory (Azure AD) ile bağlantı bulut sağlamasını yapılandırmanız gerekir. Aracıyı etkinleştirmek için bu adımları izleyin.

## <a name="configure-provisioning"></a>Sağlamayı Yapılandır
Sağlamayı yapılandırmak için aşağıdaki adımları izleyin.

1.  Azure portal **Azure Active Directory**' ni seçin.
1.  **Azure AD Connect**seçin.
1.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.

    ![Sağlamayı yönetme (Önizleme)](media/how-to-configure/manage1.png)

1.  **Yeni yapılandırma**' yı seçin.
1.  Yapılandırma ekranında şirket içi etki alanı önceden doldurulur.
1.  Bir **bildirim e-postası**girin. Bu e-postaya sağlama sağlıklı olmadığında bildirim alınacaktır.
1.  Seçimi **etkinleştirmek**için seçiciyi taşıyın ve **Kaydet**' i seçin.

    ![Azure AD sağlama (Önizleme)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Belirli kullanıcılara ve gruplara kapsam sağlama
Şirket içi Active Directory grupları veya kuruluş birimleri kullanarak belirli kullanıcıları ve grupları eşitlenmek için aracının kapsamını belirleyebilirsiniz. Bir yapılandırma içinde grupları ve kuruluş birimlerini yapılandıramazsınız. 

1.  Azure portal **Azure Active Directory**' ni seçin.
1.  **Azure AD Connect**seçin.
1.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
1.  **Yapılandırma**altında yapılandırmanızı seçin.

    ![Yapılandırma bölümü](media/how-to-configure/scope1.png)

1.  **Yapılandır**' ın altında, yapılandırma kuralının kapsamını değiştirmek için **tüm kullanıcılar** ' ı seçin.

    ![Tüm kullanıcılar seçeneği](media/how-to-configure/scope2.png)

1. Sağ tarafta, kapsamı yalnızca güvenlik gruplarını içerecek şekilde değiştirebilirsiniz. Grubun ayırt edici adını girin ve **Ekle**' yi seçin.

    ![Seçili güvenlik grupları seçeneği](media/how-to-configure/scope3.png)

1.  Ya da kapsamı yalnızca belirli kuruluş birimlerini içerecek şekilde değiştirebilirsiniz. **Bitti** ve **Kaydet**' i seçin.  
2.  Kapsamı değiştirdikten sonra, değişikliklerin anında eşitlenmesini başlatmak için [sağlamayı yeniden başlatmanız](#restart-provisioning) gerekir.

    ![Seçili kuruluş birimleri seçeneği](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Sağlamayı yeniden Başlat 
Zamanlanan bir sonraki çalıştırmaya beklemek istemiyorsanız, sağlama çalıştırmasını **yeniden başlatma sağlama** düğmesini kullanarak tetikleyin. 
1.  Azure portal **Azure Active Directory**' ni seçin.
1.  **Azure AD Connect**seçin.
1.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
1.  **Yapılandırma**altında yapılandırmanızı seçin.

    ![Sağlamayı yeniden başlatmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

1.  En üstte **sağlamayı yeniden Başlat**' ı seçin.

## <a name="remove-a-configuration"></a>Yapılandırmayı kaldırma
Bir yapılandırmayı silmek için aşağıdaki adımları izleyin.

1.  Azure portal **Azure Active Directory**' ni seçin.
1.  **Azure AD Connect**seçin.
1.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
1.  **Yapılandırma**altında yapılandırmanızı seçin.

    ![Yapılandırmayı kaldırmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

1.  Yapılandırma ekranının üst kısmında **Sil**' i seçin.

    ![Sil düğmesi](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Bir yapılandırma silinmeden önce hiçbir onay yok. **Sil**' i seçmeden önce bunun gerçekleştirmek istediğiniz eylem olduğundan emin olun.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
