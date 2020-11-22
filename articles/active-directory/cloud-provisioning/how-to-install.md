---
title: Azure AD Connect bulut sağlama aracısını yükleme
description: Bu makalede Azure AD Connect bulut sağlama aracısının nasıl yükleneceği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5322e5ce1bb124387931eac666cf9e5510cb2463
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2020
ms.locfileid: "95237654"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Azure AD Connect bulut sağlama aracısını yükleme
Bu belgede Azure Active Directory (Azure AD) Connect sağlama aracısının yükleme işleminde ve bu dosyayı ilk olarak Azure portal nasıl yapılandıracağınız açıklanmaktadır.

>[!IMPORTANT]
>Aşağıdaki yükleme yönergeleri tüm [önkoşulların](how-to-prerequisites.md) karşılandığını varsayar.

Azure AD Connect sağlamayı yükleme ve yapılandırma aşağıdaki adımlarda gerçekleştirilir:

- [Grup tarafından yönetilen hizmet hesapları](#group-managed-service-accounts) 
- [Aracıyı yükleme](#install-the-agent)
- [Aracı yüklemesini doğrulama](#verify-agent-installation)


## <a name="group-managed-service-accounts"></a>Grup Tarafından Yönetilen Hizmet Hesapları
Grup tarafından yönetilen hizmet hesabı, otomatik parola yönetimi, Basitleştirilmiş hizmet asıl adı (SPN) yönetimi, yönetimi diğer yöneticilere devretmek ve ayrıca bu işlevselliği birden çok sunucuya genişleten bir yönetilen etki alanı hesabıdır.  Azure AD Connect bulut eşitlemesi, aracıyı çalıştırmak için Grup tarafından yönetilen bir hizmet hesabının kullanımını destekler ve önerir.  Bir gMSA hakkında daha fazla bilgi için bkz. [Grup yönetilen hizmet hesapları](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) 


### <a name="upgrading-an-existing-agent-to-use-the-gmsa-account"></a>Mevcut bir aracıyı gMSA hesabını kullanacak şekilde yükseltme
Mevcut bir aracıyı yükleme sırasında oluşturulan gMSA hesabını kullanacak şekilde yükseltmek için, AADConnectProvisioningAgent.msi çalıştırarak aracı hizmetini en son sürüme güncelleştirmeniz yeterlidir.  Bu işlem hizmeti en son sürüme yükseltir.  Şimdi Yükleme Sihirbazı 'nı yeniden çalıştırın ve istendiğinde hesabı oluşturmak için kimlik bilgilerini sağlayın.



## <a name="install-the-agent"></a>Aracıyı yükleme
Aracıyı yüklemek için aşağıdaki adımları izleyin.

 1. Kurumsal Yönetici izinleriyle kullanacağınız sunucuda oturum açın.
 2. Azure portal oturum açın ve **Azure Active Directory**' a gidin.
 3. Sol menüden **Azure AD Connect**' yi seçin.
 4. **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin  >  **tüm aracıları gözden geçirin**.
 5. Azure AD Connect sağlama aracısını Azure portal indirin.
   ![Şirket içi aracıyı indir](media/how-to-install/install-9.png)</br>
 6. Azure AD Connect sağlama yükleyicisi AADConnectProvisioningAgent.msi çalıştırın.
 7. **Microsoft Azure AD Connect aracı paketi** ekranında, lisans koşullarını kabul edin ve **yüklemeyi** seçin.
   ![Microsoft Azure AD bağlama Aracısı paketi ekranı](media/how-to-install/install-1.png)</br>
 8. Bu işlem tamamlandıktan sonra Yapılandırma Sihirbazı başlatılır. Azure AD Genel Yönetici hesabınızla oturum açın.
 9. **Hizmet hesabını Yapılandır ekranında** , **gMSA oluştur** ' u veya **özel gMSA kullanın**' ı seçin.  Aracının hesap oluşturmasına izin verirseniz, provAgentgMSA $ olarak adlandırılır. **Özel gMSA kullan** 'ı belirtirseniz, bu hesabı sağlamanız istenir.
 10. Aracı hizmetini çalıştırmak için kullanılacak grup tarafından yönetilen hizmet hesabını oluşturmak için etki alanı yöneticisi kimlik bilgilerini girin. **İleri**’ye tıklayın.  
   ![GMSA oluştur](media/how-to-install/install-12.png)</br>
 11. **Bağlan Active Directory** ekranında **Dizin Ekle**' yi seçin. Ardından Active Directory Yönetici hesabınızla oturum açın. Bu işlem, şirket içi dizininizi ekler. 
 12. İsteğe bağlı olarak, etki alanı **denetleyicisi önceliğini Seç** ' i seçerek ve etki alanı denetleyicilerinin listesini sipariş ederek aracının kullanacağı etki alanı denetleyicileri tercihini yönetebilirsiniz.   **Tamam** düğmesine tıklayın.
  ![Etki alanı denetilicileri siparişi](media/how-to-install/install-2a.png)</br>
 13. **İleri**’yi seçin.
  ![Active Directory ekranına Bağlan](media/how-to-install/install-3a.png)</br>
 14.  **Aracı yükleme** ekranında ayarları ve oluşturulacak hesabı onaylayın ve **Onayla**' ya tıklayın.
  ![Settngs doğrulayın](media/how-to-install/install-11.png)</br>
 15. Bu işlem tamamlandıktan sonra, **Aracı yüklemenizin tamamlandığını** görmeniz gerekir. **Çıkış**' ı seçin.
  ![Yapılandırma tamamlanma ekranı](media/how-to-install/install-4a.png)</br>
1. Hala ilk **Microsoft Azure AD Connect sağlama Aracısı paketi** ekranını görüyorsanız, **Kapat**' ı seçin.

## <a name="verify-agent-installation"></a>Aracı yüklemesini doğrulama
Aracı doğrulaması Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde oluşur.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal
Aracının Azure tarafından görüldüğünü doğrulamak için aşağıdaki adımları izleyin.

1. Azure portalında oturum açın.
1. Sol tarafta **Azure Active Directory**  >  **Azure AD Connect**' yı seçin. Merkezinde **Yönet sağlama (Önizleme)** öğesini seçin.

   ![Azure portal](media/how-to-install/install-6.png)</br>

1.  **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' i seçin.

    ![Tüm aracıları İncele seçeneği](media/how-to-install/install-7.png)</br>
 
1. **Şirket içi sağlama aracıları** ekranında, yüklediğiniz aracıları görürsünüz. Söz konusu aracının orada olduğunu ve *etkin* olarak işaretlendiğinden emin olun.

   ![Şirket içi sağlama aracıları ekranı](media/how-to-install/verify-1.png)</br>



### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1.  Sunucuda yönetici hesabıyla oturum açın.
1.  **Hizmetlere** giderek veya Hizmetleri **Start**  >  **Çalıştır**  >  **. msc**' ye giderek Hizmetleri açın.
1.  **Hizmetler**' in altında **Microsoft Azure AD aracı güncelleştiricisi bağlama** ve **Microsoft Azure AD Connect sağlama aracısının** aynı olduğundan ve durumlarının *çalıştığından* emin olun.

    ![Hizmetler ekranı](media/how-to-install/troubleshoot-1.png)

>[!IMPORTANT]
>Aracı yüklendi, ancak kullanıcıları eşitlemeye başlamadan önce yapılandırılması ve etkinleştirilmesi gerekir. Yeni bir aracı yapılandırmak için bkz. [Azure AD Connect bulut tabanlı sağlama için yeni yapılandırma oluşturma](how-to-configure.md).




## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
 
