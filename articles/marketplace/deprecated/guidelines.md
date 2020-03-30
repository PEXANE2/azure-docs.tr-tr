---
title: Azure Marketi ve AppSource yayıncısı için yönergeler | Azure
description: Uygulama ve hizmet yayıncıları için Azure Marketi ve AppSource yönergeleri
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/13/2018
ms.author: dsindona
ms.openlocfilehash: 9156126ff2a3dbe936dc426c6f5097c6bba7b7b7
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387624"
---
# <a name="guidelines"></a>Yönergeler  

<!--
## Guidelines for AppSource  
-->
---

## <a name="guidelines-for-azure-marketplace"></a>Azure Marketi Yönergeleri  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Bir pazar hesabını yönetmek için Microsoft Kimliği oluşturma yönergeleri  
Birden fazla kişi, pazar hesabınızı oluşturmak için kullanılan aynı Microsoft Kimliğine erişim gerektiriyorsa, bir şirket hesabı oluşturmanıza yardımcı olmak için bu yönergeleri izlemeniz gerekir. 

>[!IMPORTANT]
>Microsoft, birden çok kullanıcıya Microsoft Geliştirici Merkezi (Geliştirme Merkezi) hesabınıza erişme yetkisi vermek için, tek tek kullanıcılara roller atamak için Azure Active Directory (Azure AD) kullanmanızı önerir. Her kullanıcı, tek tek Azure AD kimlik bilgileriyle oturum açarak hesaba erişmelidir. Microsoft, şirketinize kayıtlı bir etki alanında bir e-posta adresi kullanarak Microsoft Kimliğinizi oluşturun, e-postanın bir kişiye atanmadığını önerir. `windowsapps@fabrikam.com` bunun bir örneğidir.  
>*   Daha fazla bilgi için, [Azure AD federe etki alanı](#issue-microsoft-id-in-an-azure-ad-federated-domain) bölümündeki Sorun: Microsoft Kimliği'ni ziyaret edin.  

*   Microsoft Kimliği'ne erişimi mümkün olan en az sayıda geliştiriciyle sınırlandırın. 
*   Dev Center hesabınıza erişmesi gereken herkesi içeren bir kurumsal e-posta dağıtım listesi (DL) ayarlayın. Güvenlik bilgilerinize DL e-posta adresini ekleyin. DL, listedeki tüm çalışanların istendiğinde güvenlik kodları almasını ve Microsoft Kimliğinizin güvenlik bilgilerini yönetmesini sağlar. Bir dağıtım listesi ayarlamak mümkün değilse, istendiğinde güvenlik koduna erişmek ve paylaşmak için tek tek e-posta hesabının sahibinin kullanılabilir olması gerekir.  
    *   Örneğin, Microsoft Kimliği'ne yeni güvenlik bilgileri eklendiğinde veya Microsoft Kimliği'ne yeni bir aygıttan erişildiğinde sahibinden istenir.  
*   Uzatma gerektirmeyen ve anahtar ekip üyeleri tarafından erişilebilen bir şirket telefon numarası ekleyin.  
*   Genel olarak, Geliştirme Merkezi hesabınızda oturum açabilmek için geliştiricilerin güvenilir aygıtlar kullanmasını zorunlu kılmalıdır. Tüm anahtar ekip üyelerinin güvenilir aygıtlara erişimi olmalıdır. Erişmek için güvenilir aygıtlar kullanmak, birisi Geliştirme Merkezi hesabına erişirken güvenlik kodları gönderme gereksinimini azaltır.  
*   Dev Center hesabına güvenilmeyen bir bilgisayardan erişim izni vermeniz gerekiyorsa, erişimi en fazla beş geliştiriciyle sınırlamanız gerekir. İdeal olarak, geliştiricilerinizin hesaba aynı coğrafi ve ağ konumunu paylaşan bilgisayarlardan erişmesi gerekir.  
*   Güvenlik bilgilerinizi sık sık gözden geçirin ve doğrulayın.  
    *   Güvenlik bilgilerinizi görüntülemek için [account.live.com/proofs/Manage'da](https://account.live.com/proofs/Manage)bulunan Güvenlik ayarları sayfasını ziyaret edin.

Dev Center hesabınıza öncelikle güvenilir bilgisayarlardan erişilmelidir. Haftalık Dev Center hesabı başına oluşturulan kod sayısının bir sınırı olduğundan, güvenilir bilgisayarlardan erişmeniz çok önemlidir. Güvenilir bilgisayarları kullanmak aynı zamanda en güvenli ve tutarlı oturum açma deneyimini de sağlar. 
*   Ek Dev Center hesap yönergeleri ve güvenliği hakkında daha fazla bilgi [için, docs.microsoft.com/windows/uwp/publish/opening-a-developer-account'da](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account)bulunan bir geliştirici hesabı açma sayfasını ziyaret edin. 

---

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Sorun: Azure AD federe etki alanında Microsoft Kimliği  
Kurumsal hesabınız Azure Etkin Dizin (Azure AD) aracılığıyla federated olabilir. Azure AD ile beslenen bir şirket e-posta adresi kullanarak bir Microsoft kimliği oluşturmaya çalışırsanız, bir hata alırsınız. Bir hata alırsanız, hesabınızın Azure AD aracılığıyla federated olduğunu doğrulamak için BT ekibinize danışmalısınız. Azure AD federe e-posta bilinen bir sorundur ve Microsoft bu sorunu çözmek için çalışıyor.  
*   Azure AD hakkında daha fazla bilgi için [docs.microsoft.com/azure/active-directory'da](https://docs.microsoft.com/azure/active-directory)bulunan Azure Etkin Dizin Belgeleri sayfasını ziyaret edin.

Microsoft bir geçici çözüm önerir. `outlook.com` Etki alanında yeni bir e-posta adresi oluşturmak ve iletişimlerinizi iletmek için bir kural oluşturmak için aşağıdaki adımları izleyin.  
1.  Hesap Oluştur sayfasına gidin ve yeni bir e-posta adresi al bağlantısını tıklayın. 
    *   Microsoft Kimliğinize kaydolmak için [signup.live.com/signup'da](https://signup.live.com/signup)bulunan hesabı oluştur sayfasını ziyaret edin.  
2.  Yeni e-posta adresini oluşturun ve bir parola girin. `outlook.com` Etki alanında yeni bir Microsoft Kimliği ve bir e-posta posta kutusu oluşturulur. Hesap oluşturulana kadar kayıt işlemine devam edin.  

    >[!IMPORTANT]
    >Dev Center'a kaydolmak için Microsoft Kimliği olarak kayıtlı bir e-posta adresi veya dağıtım listesi kullanmanız gerekir. Microsoft, bireylerden gelen bağımlılığı kaldırmak için bir dağıtım listesi kullanmanızı önerir. E-posta adresiniz veya dağıtım listeniz kayıtlı değilse, şimdi kaydolmanız gerekir.    

    >[!Important]
    >Herhangi bir e-posta adresiniz `Microsoft` şirket etki alanında bulunuyorsa, bu adresi Dev Center'da kayıt için kullanamazsınız.  

3.  Outlook e-posta adresiyle Microsoft Kimliğini oluşturduktan sonra Outlook posta kutunuzda oturum açın. E-posta yönlendirme kuralı oluşturun. E-posta yönlendirme kuralı, Outlook posta kutusunda alınan tüm e-postaları, pazar hesabınızı yönetmek için oluşturduğunuz etki alanınızdaki e-posta adresine taşımalıdır.  
    *   Outlook posta kutunuzda oturum açabilmek için [outlook.live.com/owa'da](https://outlook.live.com/owa)bulunan Outlook sayfasını ziyaret edin.  
    *   Yönlendirme kuralları hakkında daha fazla bilgi için, support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed'da bulunan başka bir hesap sayfasına [support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed)otomatik olarak iletileri iletmek için Outlook Web App'teki Kullanım kurallarını ziyaret edin.  

1.  Yönlendirme kuralı, Outlook e-posta hesabından alınan tüm e-posta ve iletişimleri şirketinize kayıtlı bir etki alanında bulunan e-posta adresine gönderir. `outlook.com` E-posta adresiniz, Hem Dev Center hem de Cloud Partner Portal'da kimlik doğrulaması yapmak için kullanılmalıdır.  

## <a name="next-steps"></a>Sonraki adımlar

*   Azure [Marketi ve AppSource Publisher Guide](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) sayfasını ziyaret edin. 
 
---
