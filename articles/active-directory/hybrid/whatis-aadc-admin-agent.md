---
title: Azure AD Connect yönetici Aracısı nedir? Azure AD Connect | Microsoft Docs
description: Şirket içi ortamınızı Azure AD ile senkronize etmek ve izlemek için kullanılan araçları açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 09/04/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79f68635820125161ed4f5777e27a20de9e6fbe8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80049391"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Azure AD Connect Yönetim Aracısı nedir? 
Azure AD Connect Yönetim Aracısı, bir Azure Active Directory Connect sunucusuna yüklenebilen yeni bir Azure Active Directory Connect bileşenidir. Bir destek talebi açtığınızda Microsoft destek mühendisine sorunları gidermeye yardımcı olan Active Directory ortamınızdan belirli verileri toplamak için kullanılır. 

>[!NOTE]
>Yönetici aracısı yüklenmez ve varsayılan olarak etkin değildir.  Destek durumlarında yardımcı olacak verileri toplamak için aracıyı yüklemelisiniz.

Yüklendiğinde, Azure AD Connect Yönetim Aracısı Azure Active Directory verileri için belirli istekleri bekler, eşitleme ortamından istenen verileri alır ve Microsoft destek mühendisine sunulan Azure Active Directory gönderir. 

Azure AD Connect yönetim aracısının ortamınızdan aldığı bilgiler herhangi bir şekilde depolanmaz. yalnızca Microsoft destek mühendisine, Azure AD Connect yönetim aracısının Azure AD Connect sunucusunda varsayılan olarak yüklenmediğini ve bu Azure Active Directory Connect ilgili destek durumunu araştırıp sorunlarını gidermeye yardımcı olacak şekilde görünür. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>Azure AD Connect sunucusuna Azure AD Connect yönetim aracısını yükler 

Ön koşullar:
1.    Azure AD Connect sunucuya yüklendi
2.    Azure AD Connect Health sunucuya yüklendi

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent0.png)

Azure AD Connect Yönetim Aracısı ikilileri AAD Connect sunucusuna yerleştirilir. Aracıyı yüklemek için aşağıdakileri yapın:

1.    PowerShell 'i yönetici modunda aç
2.    Uygulamanın CD 'nin bulunduğu dizine gidin "C:\Program Files\Microsoft Azure Active Directory Connect\Tools"
3.    ConfigureAdminAgent. ps1 öğesini çalıştırın

İstendiğinde lütfen Azure AD Genel yönetici kimlik bilgilerinizi girin. Bu, Azure AD Connect yüklemesi sırasında girilen kimlik bilgileri ile aynı olmalıdır.

Aracı yüklendikten sonra, sunucunuzun Denetim Masası 'ndaki "Program Ekle/Kaldır" listesinde aşağıdaki iki yeni programı görürsünüz: 

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Eşitleme hizmetimde hangi veriler Microsoft hizmet mühendisine gösteriliyor? 
Microsoft Desteği mühendisinin bir destek talebi açtığınızda, belirli bir kullanıcı için, Active Directory içindeki ilgili verileri, Azure Active Directory Connect sunucusundaki Active Directory bağlayıcı alanını, Azure Active Directory sunucusundaki Azure Active Directory Connect bağlayıcı alanını ve Azure Active Directory Connect sunucusundaki meta veri deposu ' nu görebilirsiniz. 

Microsoft Desteği mühendisi sisteminizdeki verileri değiştiremez ve herhangi bir parolayı göremez. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Microsoft destek mühendisinin verilerinize erişimini istemiyorum ne yapmalıyım? 
Aracı yüklendikten sonra, Microsoft hizmet mühendisinin bir destek çağrısı için verilerinize erişmesini istemiyorsanız, hizmet yapılandırma dosyasını aşağıda açıklandığı gibi değiştirerek işlevselliği devre dışı bırakabilirsiniz: 

1.    Not defteri 'nde **C:\Program Files\Microsoft Azure AD Connect Administration Agent\AzureADConnectAdministrationAgentService.exe.config** öğesini açın.
2.    **Userdataenabled** ayarını aşağıda gösterildiği gibi devre dışı bırakın. **Userdataenabled** ayarı varsa ve true olarak ayarlanırsa, false olarak ayarlayın. Ayar yoksa, ayarı aşağıda gösterildiği gibi ekleyin.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.    Yapılandırma dosyasını kaydedin.
4.    Azure AD Connect Yönetim Aracısı hizmetini aşağıda gösterildiği gibi yeniden başlatın

![Yönetim Aracısı](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
