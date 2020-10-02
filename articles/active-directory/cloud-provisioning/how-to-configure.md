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
ms.openlocfilehash: c8b18629a776dd98950f49b1f607cbc876abcd9c
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91628918"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Azure AD Connect bulut tabanlı sağlama için yeni bir yapılandırma oluşturma

Aracıyı yükledikten sonra, Azure portal oturum açmanız ve Azure Active Directory (Azure AD) ile bağlantı bulut sağlamasını yapılandırmanız gerekir. Aracıyı etkinleştirmek için bu adımları izleyin.

## <a name="configure-provisioning"></a>Sağlamayı Yapılandır
Sağlamayı yapılandırmak için aşağıdaki adımları izleyin.

 1. Azure portal **Azure Active Directory** seçin
 2. **Azure AD Connect**seçin.
 3. **Sağlamayı Yönet**' i seçin.

 ![Sağlamayı Yönet](media/how-to-configure/manage1.png)
 
 4. **Yeni yapılandırma**' yı seçin.
 5. Yapılandırma ekranında, etki alanınızı ve parola karması eşitlemesini etkinleştirip etkinleştirmeyeceğinizi seçin.  **Oluştur**' a tıklayın.  
 
 ![Yeni yapılandırma oluştur](media/how-to-configure/configure1.png)


 6.  Sağlama yapılandırma düzenleme ekranı açılır.

   ![Yapılandırmayı Düzenle](media/how-to-configure/configure2.png)

 7. Bir **bildirim e-postası**girin. Bu e-postaya sağlama sağlıklı olmadığında bildirim alınacaktır.
 8. Seçimi etkinleştirmek için seçiciyi taşıyın ve Kaydet ' i seçin.

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Belirli kullanıcılara ve gruplara kapsam sağlama
Şirket içi Active Directory grupları veya kuruluş birimleri kullanarak belirli kullanıcıları ve grupları eşitlenmek için aracının kapsamını belirleyebilirsiniz. Bir yapılandırma içinde grupları ve kuruluş birimlerini yapılandıramazsınız. 

 1.  Azure portal **Azure Active Directory**' ni seçin.
 2. **Azure AD Connect**seçin.
 3. **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
 4. **Yapılandırma**altında yapılandırmanızı seçin.

 ![Yapılandırma bölümü](media/how-to-configure/scope1.png)
 
 5. **Yapılandır**' ın altında, yapılandırma kuralının kapsamını değiştirmek için **kapsam Filtrelerini Düzenle** ' yi seçin.
 ![Kapsamı Düzenle](media/how-to-configure/scope3.png)
 7. Sağ tarafta, kapsamı değiştirebilirsiniz.  Bitti **' ye tıklayın**  ve Işiniz bittiğinde **kaydedin** .
 8. Kapsamı değiştirdikten sonra, değişikliklerin anında eşitlenmesini başlatmak için [sağlamayı yeniden başlatmanız](#restart-provisioning) gerekir.

## <a name="attribute-mapping"></a>Öznitelik eşlemesi
Azure AD Connect bulut sağlama, şirket içi Kullanıcı/Grup nesneleriniz ile Azure AD içindeki nesneler arasındaki öznitelikleri kolayca eşlemenizi sağlar.  Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, var olan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz.  Daha fazla bilgi için bkz. [öznitelik eşleme](how-to-attribute-mapping.md).

## <a name="on-demand-provisioning"></a>İsteğe bağlı sağlama
Azure AD Connect bulut sağlama, bu değişiklikleri tek bir kullanıcıya veya gruba uygulayarak yapılandırma değişikliklerini test etmenize olanak tanır.  Yapılandırma üzerinde yapılan değişikliklerin düzgün şekilde uygulandığını ve Azure AD ile doğru eşitlendiğini doğrulamak için bunu kullanabilirsiniz.  Daha fazla bilgi için bkz. [isteğe bağlı sağlama](how-to-on-demand-provision.md).

## <a name="restart-provisioning"></a>Sağlamayı yeniden Başlat 
Zamanlanan bir sonraki çalıştırmaya beklemek istemiyorsanız, sağlama çalıştırmasını **yeniden başlatma sağlama** düğmesini kullanarak tetikleyin. 
 1.  Azure portal **Azure Active Directory**' ni seçin.
 2. **Azure AD Connect**seçin.
 3.  **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
 4. **Yapılandırma**altında yapılandırmanızı seçin.

   ![Sağlamayı yeniden başlatmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

 5. En üstte **sağlamayı yeniden Başlat**' ı seçin.

## <a name="remove-a-configuration"></a>Yapılandırmayı kaldırma
Bir yapılandırmayı silmek için aşağıdaki adımları izleyin.

 1.  Azure portal **Azure Active Directory**' ni seçin.
 2. **Azure AD Connect**seçin.
 3. **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin.
 4. **Yapılandırma**altında yapılandırmanızı seçin.
   
   ![Yapılandırmayı kaldırmak için yapılandırma seçimi](media/how-to-configure/scope1.png)

 5. Yapılandırma ekranının üst kısmında **Sil**' i seçin.

>[!IMPORTANT]
>Bir yapılandırma silinmeden önce hiçbir onay yok. **Sil**' i seçmeden önce bunun gerçekleştirmek istediğiniz eylem olduğundan emin olun.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
