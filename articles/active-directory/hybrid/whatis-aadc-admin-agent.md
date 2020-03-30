---
title: Azure AD Connect Yönetici Aracısı Nedir - Azure AD Connect | Microsoft Dokümanlar
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80049391"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Azure AD Connect Yönetim Aracısı nedir? 
Azure AD Connect Yönetim Aracısı, Azure Active Directory Connect'in bir Azure Active Directory Connect sunucusuna yüklenebilen yeni bir bileşenidir. Bir destek servis talebi açtığınızda microsoft destek mühendisinin sorunları gidermesine yardımcı olan Etkin Dizin ortamınızdan belirli verileri toplamak için kullanılır. 

>[!NOTE]
>Yönetici aracısı yüklenmez ve varsayılan olarak etkinleştirilir.  Destek durumlarına yardımcı olmak için veri toplamak için aracıyı yüklemeniz gerekir.

Azure AD Connect Yönetim Aracısı yüklendiğinde Azure Active Directory'den belirli veri isteklerini bekler, eşitleme ortamından istenen verileri alır ve Microsoft desteğine sunulduğu Azure Active Directory'ye gönderir Mühendisi. 

Azure AD Connect Yönetim Aracısı'nın ortamınızdan aldığı bilgiler hiçbir şekilde depolanmaz - Azure Active Directory Connect'i araştırmave sorun gidermede yardımcı olmak için yalnızca Microsoft destek mühendisine görüntülenir Azure AD Connect Yönetim Aracısını açtığınız ilgili destek durumu varsayılan olarak Azure AD Connect Server'da yüklenmez. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>Azure AD Connect Yönetim Aracısını Azure AD Connect sunucusuna yükleme 

Ön koşullar:
1.    Azure AD Connect sunucuda yüklü
2.    Azure AD Connect Health sunucuda yüklü

![yönetici ajan](media/whatis-aadc-admin-agent/adminagent0.png)

Azure AD Connect Yönetim Aracısı ikilileri AAD Connect sunucusuna yerleştirilir. Aracıyı yüklemek için aşağıdakileri yapın:

1.    Yönetici modunda powershell'i açın
2.    Uygulamanın bulunduğu dizin "C:\Program Files\Microsoft Azure Active Directory Connect\Tools" dizinine gidin
3.    YapılandırılanAdminAgent.ps1 çalıştırın

İstendiğinde, lütfen Azure AD genel yönetici kimlik bilgilerinizi girin. Bu, Azure AD Connect yüklemesi sırasında girilen kimlik bilgileriyle aynı olmalıdır.

Aracı yüklendikten sonra sunucunuzun Denetim Masası'nda "Program Ekle/Kaldır" listesinde aşağıdaki iki yeni programı görürsünüz: 

![yönetici ajan](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Eşitleme hizmetimdeki veriler Microsoft servis mühendisine gösterilir? 
Bir destek örneği açtığınızda Microsoft Destek Mühendisi belirli bir kullanıcı için Active Directory'deki ilgili verileri, Azure Active Directory Connect sunucusundaki Active Directory bağlayıcı alanını, Azure'daki Azure Etkin Dizin bağlayıcı alanını görebilir Active Directory Connect sunucusu ve Azure Active Directory Connect sunucusundaki Metaverse. 

Microsoft Destek Mühendisi sisteminizdeki hiçbir veriyi değiştiremez ve parola ları göremez. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Microsoft destek mühendisinin verilerime erişmesini istemiyorsam ne olur? 
Aracı yüklendikten sonra, Microsoft hizmet mühendisinin bir destek çağrısı için verilerinize erişmesini istemiyorsanız, hizmet config dosyasını aşağıda açıklandığı şekilde değiştirerek işlevselliği devre dışı kullanabilirsiniz: 

1.    **C:\Program Dosyalarını Aç\Microsoft Azure AD Connect Yönetim Aracısı\AzureADConnectAdministrationService.exe.config** in not defteri.
2.    Aşağıda gösterildiği gibi **UserDataEnabled** ayarını devre dışı kılabilir. **UserDataEnabled** ayarı varsa ve doğru olarak ayarlanmışsa, yanlış olarak ayarlayın. Ayar yoksa, ayarı aşağıda gösterildiği gibi ekleyin.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.    Config dosyasını kaydedin.
4.    Azure AD Connect Yönetim Aracısı hizmetini aşağıda gösterildiği gibi yeniden başlatın

![yönetici ajan](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
