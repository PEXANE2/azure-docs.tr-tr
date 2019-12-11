---
title: Azure AD Connect bulut sağlama yeni aracı yapılandırması
description: Bu konuda, bulut sağlama 'nın nasıl yükleneceği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f08a1359cfd8a2793d92315a6b03567b0b3f847d
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997131"
---
# <a name="azure-ad-connect-cloud-provisioning-new-configuration"></a>Azure AD Connect bulut sağlama yeni yapılandırma

Aracıyı yükledikten sonra, Azure portal oturum açmanız ve sağlamayı yapılandırmanız gerekir.  Aracıyı etkinleştirmek için aşağıdaki adımları kullanın.

## <a name="configure-provisioning"></a>Sağlamayı Yapılandır
Sağlamayı yapılandırmak için aşağıdaki adımları kullanın:

1.  Azure AD portalında **Azure Active Directory** ' ye tıklayın.
2.  **Azure AD Connect** tıklayın
3.  **Sağlamayı Yönet (Önizleme)** 
![](media/how-to-configure/manage1.png) seçin

4.  **Yeni yapılandırma**' ya tıklayın.
5.  Yapılandırma ekranında şirket içi etki alanı önceden doldurulur
6. Bir **bildirim e-postası**girin. Sağlama sağlıklı değilse, bu e-postaya bildirim gönderilir.  
8. Seçicisi **etkinleştirmek** ve **Kaydet**' e gitmek için seçiciyi taşıyın.
![](media/tutorial-single-forest/configure2.png)

## <a name="scoping-provisioning-to-specific-users-and-groups"></a>Belirli kullanıcılara ve gruplara sağlama kapsamını belirleme
Aracının yalnızca belirli kullanıcıları ve grupları eşitleyecek şekilde kapsamını atamak istiyorsanız bunu yapabilirsiniz. Şirket içi AD gruplarını veya kuruluş birimlerini kullanarak kapsam yapabilirsiniz. Bir yapılandırma içinde grupları ve kuruluş birimlerini yapılandıramazsınız. 

1.  Azure AD portalında **Azure Active Directory** ' ye tıklayın.
2.  **Azure AD Connect** tıklayın
3.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin
4.  **Yapılandırma** altında yapılandırmanıza tıklayın.  
![](media/how-to-configure/scope1.png)

5.  **Yapılandır**' ın altında, yapılandırma kuralının kapsamını değiştirmek için **tüm kullanıcılar** ' ı seçin.
![](media/how-to-configure/scope2.png)

6. Sağ tarafta, grubun ayırt edici adını girip **Ekle**' ye tıklayarak kapsamı yalnızca güvenlik gruplarını içerecek şekilde değiştirebilirsiniz.
![](media/how-to-configure/scope3.png)

7. Ya da yalnızca belirli OU 'Ları içerecek şekilde değiştirin. **Bitti** ve **Kaydet**' e tıklayın.
![](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Sağlamayı yeniden Başlat 
Zamanlanan bir sonraki çalıştırmaya beklemek istemiyorsanız, sağlama çalıştırmasını yeniden başlatma sağlama düğmesini kullanarak tetikleyebilirsiniz. 
1.  Azure AD portalında **Azure Active Directory** ' ye tıklayın.
2.  **Azure AD Connect** tıklayın
3.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin
4.  **Yapılandırma** altında yapılandırmanıza tıklayın.  
![](media/how-to-configure/scope1.png)

5.  En üstte, **sağlamayı yeniden Başlat**' a tıklayın.

## <a name="removing-a-configuration"></a>Yapılandırma kaldırılıyor
Bir yapılandırmayı silmek isterseniz, aşağıdaki adımları kullanarak bunu yapabilirsiniz.

1.  Azure AD portalında **Azure Active Directory** ' ye tıklayın.
2.  **Azure AD Connect** tıklayın
3.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin
4.  **Yapılandırma** altında yapılandırmanıza tıklayın.  
![](media/how-to-configure/scope1.png)

5.  Üst kısımdaki **Sil**' e tıklayın.
![](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Bir yapılandırma silinmeden önce hiçbir onay yoktur, bu nedenle **Sil**' e tıklamadan önce bunun gerçekleştirmek istediğiniz eylem olduğundan emin olun.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
