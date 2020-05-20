---
title: Azure AD Connect bulut sağlama aracısını yükleme
description: Bu makalede Azure AD Connect bulut sağlama aracısının nasıl yükleneceği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/19/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 764071eeaf368ecf78679632cffbd6964db40aa5
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681040"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Azure AD Connect bulut sağlama aracısını yükleme
Bu belgede Azure Active Directory (Azure AD) Connect sağlama aracısının yükleme işleminde ve bu dosyayı ilk olarak Azure portal nasıl yapılandıracağınız açıklanmaktadır.

>[!IMPORTANT]
>Aşağıdaki yükleme yönergeleri tüm [önkoşulların](how-to-prerequisites.md) karşılandığını varsayar.

Azure AD Connect sağlamayı yükleme ve yapılandırma aşağıdaki adımlarda gerçekleştirilir:
    
- [Aracıyı yükleme](#install-the-agent)
- [Aracı yüklemesini doğrulama](#verify-agent-installation)


## <a name="install-the-agent"></a>Aracıyı yükleme
Aracıyı yüklemek için aşağıdaki adımları izleyin.

1. Kurumsal Yönetici izinleriyle kullanacağınız sunucuda oturum açın.
1. Azure portalına gidin. Sol tarafta **Azure Active Directory**' yi seçin.
1. **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin  >  **tüm aracıları gözden geçirin**.
1. Azure AD Connect sağlama aracısını Azure portal indirin.

   ![Şirket içi aracıyı indir](media/how-to-install/install9.png)</br>
1. Azure AD Connect sağlama yükleyicisini (AADConnectProvisioningAgent. Installer) çalıştırın.
1. **Microsoft Azure AD Connect aracı paketi** ekranında, lisans koşullarını kabul edin ve **yüklemeyi**seçin.

   ![Microsoft Azure AD bağlama Aracısı paketi ekranı](media/how-to-install/install1.png)</br>

1. Bu işlem tamamlandıktan sonra Yapılandırma Sihirbazı başlatılır. Azure AD Genel Yönetici hesabınızla oturum açın.
1. **Bağlan Active Directory** ekranında **Dizin Ekle**' yi seçin. Ardından Active Directory Yönetici hesabınızla oturum açın. Bu işlem, şirket içi dizininizi ekler. **İleri**’yi seçin.

   ![Active Directory ekranına Bağlan](media/how-to-install/install3.png)</br>

1. **Yapılandırma tamamlanma** ekranında **Onayla**' yı seçin. Bu işlem aracıyı kaydeder ve yeniden başlatır.

   ![Yapılandırma tamamlanma ekranı](media/how-to-install/install4.png)</br>

1. Bu işlem tamamlandıktan sonra, **Aracı yapılandırmanızın başarıyla doğrulandığını görürsünüz.** **Çıkış**' ı seçin.

   ![Çıkış düğmesi](media/how-to-install/install5.png)</br>
1. Hala ilk **Microsoft Azure AD Connect sağlama Aracısı paketi** ekranını görüyorsanız, **Kapat**' ı seçin.

## <a name="verify-agent-installation"></a>Aracı yüklemesini doğrulama
Aracı doğrulaması Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde oluşur.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal
Aracının Azure tarafından görüldüğünü doğrulamak için aşağıdaki adımları izleyin.

1. Azure Portal’da oturum açın.
1. Sol tarafta **Azure Active Directory**  >  **Azure AD Connect**' yı seçin. Merkezinde **Yönet sağlama (Önizleme)** öğesini seçin.

   ![Azure portal](media/how-to-install/install6.png)</br>

1.  **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' i seçin.

    ![Tüm aracıları İncele seçeneği](media/how-to-install/install7.png)</br>
 
1. **Şirket içi sağlama aracıları** ekranında, yüklediğiniz aracıları görürsünüz. Söz konusu aracının orada olduğunu ve *etkin*olarak işaretlendiğinden emin olun.

   ![Şirket içi sağlama aracıları ekranı](media/how-to-install/verify1.png)</br>



### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1.  Sunucuda yönetici hesabıyla oturum açın.
1.  **Hizmetlere** giderek veya Hizmetleri **Start**  >  **Çalıştır**  >  **. msc**' ye giderek Hizmetleri açın.
1.  **Hizmetler**' in altında **Microsoft Azure AD aracı güncelleştiricisi bağlama** ve **Microsoft Azure AD Connect sağlama aracısının** aynı olduğundan ve durumlarının *çalıştığından*emin olun.

    ![Hizmetler ekranı](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>Aracı yüklendi, ancak kullanıcıları eşitlemeye başlamadan önce yapılandırılması ve etkinleştirilmesi gerekir. Yeni bir aracı yapılandırmak için bkz. [Azure AD Connect bulut tabanlı sağlama için yeni yapılandırma oluşturma](how-to-configure.md).



## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
 
