---
title: Azure AD Connect sağlama Aracısı yükleniyor
description: Bu konuda, sağlama aracısının nasıl yükleneceğine ilişkin adım adım açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf479d4962f6d7aa9a0ba43b48f99bd67566fb6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794490"
---
# <a name="install-azure-ad-connect-provisioning-agent"></a>Azure AD Connect sağlama aracısını yükler
Bu belge, Azure AD Connect sağlama aracısının yükleme işleminde size kılavuzluk eder ve Azure portal ilk olarak nasıl yapılandırılır.

>[!IMPORTANT]
>Aşağıdaki yükleme yönergeleri tüm [önkoşulların](how-to-prerequisites.md) karşılandığını varsayar.

Azure AD Connect sağlamayı yükleme ve yapılandırma aşağıdaki adımlarda gerçekleştirilir:
    
- [Aracıyı yükler](#install-the-agent)
- [Aracı yüklemesini doğrulama](#verify-agent-installation)


## <a name="install-the-agent"></a>Aracıyı yükleme

1. Kurumsal Yönetici izinleriyle kullanacağınız sunucuda oturum açın.
2. Azure portal gidin ve sol tarafta Azure Active Directory ' i seçin.
3. **Sağlamayı Yönet (Önizleme)** seçeneğine tıklayın ve **tüm aracıları gözden geçir**' i seçin.
3. Azure AD Connect sağlama aracısını Azure portal indirin.
![Hoş Geldiniz ekranı](media/how-to-install/install9.png)</br>
3. Azure AD Connect sağlama (AADConnectProvisioningAgent. Installer) komutunu çalıştırın
3. Giriş ekranında, lisans koşullarını **kabul edin** ve **yükler**' e tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install1.png)</br>

4. Bu işlem tamamlandıktan sonra Yapılandırma Sihirbazı başlatılır.  Azure AD Genel Yönetici hesabınızla oturum açın.
5. **Bağlan Active Directory** ekranında, **Dizin Ekle** ' ye tıklayın ve Active Directory Yönetici hesabınızla oturum açın.  Bu işlem, şirket içi dizininizi ekleyecek.  **İleri**’ye tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install3.png)</br>

6. **Yapılandırma Tamam** ekranında **Onayla**' ya tıklayın.  Bu işlem aracıyı kaydedip yeniden başlatacak.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install4.png)</br>

7. Bu işlem tamamlandıktan sonra, **başarıyla doğrulandığına** ilişkin bir uyarı görmeniz gerekir.  **Çıkış**' a tıklayabilirsiniz.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install5.png)</br>
8. İlk giriş ekranını hala görüyorsanız **Kapat**' a tıklayın.


## <a name="verify-agent-installation"></a>Aracı yüklemesini doğrulama
Aracı doğrulaması Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde oluşur.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal
Aracının Azure tarafından görüldüğünü doğrulamak için şu adımları izleyin:

1. Azure Portal’da oturum açın.
2. Sol tarafta **Azure Active Directory**' ı seçin, **Azure AD Connect** ' a tıklayın ve ardından **yönetimi sağlama (Önizleme)** seçeneğini belirleyin.</br>
![Azure portalda](media/how-to-install/install6.png)</br>

3.  **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' e tıklayın.
Azure AD sağlama](media/how-to-install/install7.png) ![</br>
 
4. **Şirket içi sağlama aracıları ekranında** , yüklediğiniz aracıları görürsünüz.  Söz konusu aracının orada olduğunu ve **etkin**olarak işaretlendiğinden emin olun.
![sağlama aracıları](media/how-to-install/verify1.png)</br>

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama
Azure 'un 443 numaralı bağlantı noktasını dinlediğini ve aracınızın onunla iletişim kurabildiğini doğrulamak için aşağıdakileri kullanabilirsiniz:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 numaralı bağlantı noktası üzerinden Azure ile iletişim kurabildiğini doğrular.  Bir tarayıcı açın ve aracının yüklendiği sunucudan yukarıdaki URL 'ye gidin.
![Hizmetler](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için şu adımları izleyin:

1.  Yönetici hesabıyla sunucuda oturum açma
2.  Hizmetlere giderek veya Start/Run/Services. msc ' ye giderek **Hizmetleri** açın.
3.  **Hizmetler** ' in altında **Microsoft Azure AD aracı Güncelleştirici** ' ı bağlama ve **Microsoft Azure AD Connect sağlama aracısının** aynı ve **çalışır**durumda olduğundan emin olun.
![Hizmetler](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>Aracı yüklendi, ancak kullanıcıları eşitlemeye başlamadan önce yapılandırılması ve etkinleştirilmesi gerekir.  Yeni bir aracı yapılandırmak için bkz. [Yeni Aracı yapılandırması sağlama Azure AD Connect](how-to-configure.md).



## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
 
