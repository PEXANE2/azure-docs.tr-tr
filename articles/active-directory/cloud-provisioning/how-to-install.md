---
title: Azure AD Connect bulut sağlama aracısını yükleme
description: Bu makalede, Azure AD Connect bulut sağlama aracısının nasıl yüklenir.
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
ms.openlocfilehash: e4581ab89f74425682f569425f62714ead2c27c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263354"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Azure AD Connect bulut sağlama aracısını yükleme
Bu belge, Azure Etkin Dizin (Azure AD) Bağlantı sağlama aracısı için yükleme işleminde size yol açar ve azure portalında başlangıçta nasıl yapılandırılabilirsiniz.

>[!IMPORTANT]
>Aşağıdaki yükleme yönergeleri, tüm [Ön koşulların](how-to-prerequisites.md) karşılandığını varsayar.

Azure AD Connect sağlamanın yüklenmesi ve yapılandırılması aşağıdaki adımlarla gerçekleştirilir:
    
- [Aracıyı yükleme](#install-the-agent)
- [Aracı yüklemeyi doğrula](#verify-agent-installation)


## <a name="install-the-agent"></a>Aracıyı yükleme
Aracıyı yüklemek için aşağıdaki adımları izleyin.

1. Kurumsal yönetici izinleriyle kullanacağınız sunucuda oturum açın.
1. Azure portalına gidin. Solda, Azure **Etkin Dizin'i**seçin.
1. **Provizyon'u Yönet 'i (önizleme)** > **Tüm aracıları gözden geçir'i**seçin.
1. Azure portalından Azure AD Connect sağlama aracısını indirin.

   ![Şirket içi aracıyı karşıdan yükleyin](media/how-to-install/install9.png)</br>
1. Azure AD Connect sağlama yükleyicisini (AADConnectProvisioningAgent.Installer) çalıştırın.
1. Microsoft **Azure AD Connect Provisioning Agent Package** ekranında, lisans koşullarını kabul edin ve **Yükle'yi**seçin.

   ![Microsoft Azure AD Connect Sağlama Aracısı Paketi ekranı](media/how-to-install/install1.png)</br>

1. Bu işlem bittikten sonra yapılandırma sihirbazı başlar. Azure AD global yönetici hesabınızla oturum açın.
1. Etkin **Dizin Ekle** ekranında **Dizin Ekle'yi**seçin. Ardından Active Directory yönetici hesabınızla oturum açın. Bu işlem şirket içi dizini ekler. **Sonraki'ni**seçin.

   ![Etkin Dizin ekranını birbirine bağlay](media/how-to-install/install3.png)</br>

1. Yapılandırma **tam** ekranında **Onayla'yı**seçin. Bu işlem aracıyı kaydeder ve yeniden başlatır.

   ![Yapılandırma tam ekranı](media/how-to-install/install4.png)</br>

1. Bu işlem bittikten sonra aracı **yapılandırmanızın başarıyla doğrulandığını** bildiren bildirimi görmeniz gerekir. **Çıkış'ı**seçin.

   ![Çıkış düğmesi](media/how-to-install/install5.png)</br>
1. İlk Microsoft Azure **AD Connect Provisioning Agent Paketi** ekranını hala görüyorsanız, **Kapat'ı**seçin.

## <a name="verify-agent-installation"></a>Aracı yüklemeyi doğrula
Aracı doğrulaması Azure portalında ve aracıyı çalıştıran yerel sunucuda gerçekleşir.

### <a name="azure-portal-agent-verification"></a>Azure portal aracısı doğrulaması
Aracının Azure tarafından görüldüğünü doğrulamak için aşağıdaki adımları izleyin.

1. Azure Portal’da oturum açın.
1. Solda Azure **Active Directory** > Azure**AD Connect'i**seçin. Merkezde, **hükmü yönet (önizleme) seçeneğini**belirleyin.

   ![Azure portalında](media/how-to-install/install6.png)</br>

1.  Azure **AD Sağlama (önizleme)** ekranında **tüm aracıları gözden geçir'i**seçin.

    ![Tüm aracılar seçeneğini gözden geçirin](media/how-to-install/install7.png)</br>
 
1. Şirket **içi sağlama aracıları** ekranında, yüklediğiniz aracıları görürsünüz. Söz konusu aracının orada olduğunu ve *etkin*olarak işaretlendiğini doğrulayın.

   ![Şirket içi sağlama aracıları ekranı](media/how-to-install/verify1.png)</br>

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama
Azure'un 443 bağlantı noktasından dinlediğini ve aracınızın bu bağlantı noktasıyla iletişim kurabileceğini doğrulamak için aşağıdaki adımları izleyin.

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 portu üzerinden Azure ile iletişim kurabileceğini doğrular. Bir tarayıcı açın ve aracının yüklü olduğu sunucudan önceki URL'ye gidin.

![Port ulaşılabilirliğinin doğrulanması](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1.  Yönetici hesabıyla sunucuda oturum açın.
1.  **Hizmetleri** aç, ona yönlendirerek veya **Başlat** > **Hizmetleri.msc'ye****Run** > giderek.
1.  **Hizmetler**altında, **Microsoft Azure AD Connect Agent Updater** ve **Microsoft Azure AD Connect Provisioning Agent'ın** orada olduğundan ve durumlarının *çalışmadığından*emin olun.

    ![Hizmetler ekranı](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>Aracı yüklendi, ancak kullanıcıları eşitlemeye başlamadan önce yapılandırılması ve etkinleştirilmesi gerekir. Yeni bir aracı yapılandırmak [için](how-to-configure.md)bkz.



## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
 
