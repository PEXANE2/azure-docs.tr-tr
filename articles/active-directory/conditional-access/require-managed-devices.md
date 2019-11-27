---
title: Koşullu erişim yönetilen cihaz gerektirir-Azure Active Directory
description: Cloud App Access için yönetilen cihazlar gerektiren Azure Active Directory (Azure AD) cihaz tabanlı koşullu erişim ilkelerinin nasıl yapılandırılacağını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/22/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0caf8e8d5e18efc0a7332f97acccc394051ed360
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74452405"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Nasıl yapılır: koşullu erişimle Cloud App erişimi için yönetilen cihazlar gerektirme

Mobil ve bulut öncelikli bir dünyada Azure Active Directory (Azure AD), uygulamalara ve hizmetlere her yerden çoklu oturum açma imkanı sağlar. Yetkili kullanıcılar, mobil ve ayrıca kişisel cihazlar dahil olmak üzere çok çeşitli cihazlardan bulut uygulamalarınıza erişebilirler. Ancak, birçok ortamda yalnızca güvenlik ve uyumluluk standartlarınızı karşılayan cihazlar tarafından erişilmesi gereken en az bir uygulama vardır. Bu cihazlar yönetilen cihazlar olarak da bilinir. 

Bu makalede, yönetilen cihazların ortamınızdaki belirli bulut uygulamalarına erişmesi için gereken koşullu erişim ilkelerini nasıl yapılandırabileceğiniz açıklanmaktadır. 

## <a name="prerequisites"></a>Önkoşullar

Bulut uygulama erişimi için yönetilen cihazların gerekli olması, **Azure AD koşullu erişimi** Ile **Azure AD cihaz yönetimini** birbirine bağlalardır. Henüz bu alanlardan birini bilmiyorsanız, önce aşağıdaki konuları okumanız gerekir:

- **[Azure Active Directory Koşullu erişim](../active-directory-conditional-access-azure-portal.md)** -Bu makalede, koşullu erişime ve ilgili terimlere kavramsal bir genel bakış sunulmaktadır.
- **[Azure Active Directory cihaz yönetimine giriş](../devices/overview.md)** -Bu makalede, cihazları kurumsal denetim altında almak için sahip olduğunuz çeşitli seçeneklere genel bakış sunulmaktadır. 

## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki Bakiyenin dengelenmesi bir zorluktur. Bulut kaynaklarınıza erişmek için desteklenen cihazların uzalama, kullanıcılarınızın verimliliğini artırmaya yardımcı olur. Ters çevir tarafında, ortamınızdaki belirli kaynaklara bilinmeyen koruma düzeyi olan cihazların erişmesini istemezsiniz. Etkilenen kaynaklar için kullanıcıların yalnızca yönetilen bir cihaz kullanarak erişebilmesi gerekir. 

Azure AD koşullu erişimi sayesinde, erişim izni veren tek bir ilkeyle bu gereksinimi ele alabilirsiniz:

- Seçili bulut uygulamalarına
- Seçili kullanıcılar ve gruplar için
- Yönetilen bir cihaz gerektirme

## <a name="managed-devices"></a>Yönetilen cihazlar  

Basit koşullarda, yönetilen cihazlar bazı kurumsal denetim *sıralaması* altında bulunan cihazlardır. Azure AD 'de, yönetilen bir cihaz için önkoşul Azure AD 'ye kayıtlı. Bir cihazın kaydedilmesi cihaz nesnesi biçiminde cihaz için bir kimlik oluşturur. Bu nesne, Azure tarafından bir cihazla ilgili durum bilgilerini izlemek için kullanılır. Bir Azure AD yöneticisi olarak, bir cihazın durumunu değiştirmek (etkinleştirmek/devre dışı bırakmak) için bu nesneyi zaten kullanabilirsiniz.
  
![Cihaz tabanlı koşullar](./media/require-managed-devices/32.png)

Azure AD 'ye kayıtlı bir cihaz almak için üç seçeneğiniz vardır: 

- **Azure AD kayıtlı cihazlar** -Azure AD 'ye kayıtlı kişisel bir cihaz almak için
- **Azure AD 'ye katılmış cihazlar** -Azure AD 'ye kayıtlı şirket ıçı bir ad 'ye katılmamış bir kuruluş Windows 10 cihazını almak için. 
- **Karma Azure AD 'ye katılmış cihazlar** -Azure AD 'ye kayıtlı şirket ıçı bir ad 'ye katılmış bir Windows 10 veya desteklenen bir alt düzey cihaz almak için.

Bu üç seçenek, [cihaz kimliği](../devices/overview.md) nedir makalesinde açıklanmaktadır.

Yönetilen bir cihaz olması için, kayıtlı bir cihazın **karma bir Azure AD 'ye katılmış cihaz** ya da **uyumlu olarak işaretlenmiş bir cihaz**olması gerekir.  

![Cihaz tabanlı koşullar](./media/require-managed-devices/47.png)
 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Karma Azure AD 'ye katılmış cihazlar gerektir

Koşullu erişim ilkenizde, seçilen bulut uygulamalarının yalnızca yönetilen bir cihaz kullanılarak erişilebilmesini sağlamak için **karma Azure AD 'ye katılmış cihaz gerektir** seçeneğini belirleyebilirsiniz. 

![Cihaz tabanlı koşullar](./media/require-managed-devices/10.png)

Bu ayar yalnızca, şirket içi bir AD 'ye katılmış Windows 7 veya Windows 8 gibi Windows 10 veya alt düzey cihazlar için geçerlidir. Bu cihazları yalnızca, kayıtlı bir Windows 10 cihazını almak için [otomatik bir işlem](../devices/hybrid-azuread-join-plan.md) olan karma BIR Azure AD JOIN kullanarak Azure AD 'ye kaydedebilirsiniz. 

![Cihaz tabanlı koşullar](./media/require-managed-devices/45.png)

Hibrit Azure AD 'ye katılmış cihaza yönetilen cihaz ne olur?  Şirket içi bir AD 'ye katılmış cihazlar için, bu cihazların üzerindeki denetimin **System Center Configuration Manager (SCCM)** gibi yönetim çözümlerini veya bunları yönetmek için **Grup İlkesi (GP)** ile uygulanacağını kabul edilir. Azure AD 'nin bu yöntemlerin bir cihaza uygulanıp uygulanmadığı tespit etmek için bir yöntem olmadığından, karma bir Azure AD 'ye katılmış cihazın gerekli olması, yönetilen bir cihaz gerektiren görece zayıf bir mekanizmadır. Şirket içi etki alanına katılmış cihazlara uygulanan yöntemlerin, bu tür bir cihazın karma Azure AD 'ye katılmış bir cihaz olması halinde yönetilen bir cihaz oluşturması için yeterince güçlü olup olmadığını bir yönetici olarak öğreneceksiniz.

## <a name="require-device-to-be-marked-as-compliant"></a>Cihazın uyumlu olarak işaretlenmesini gerektir

Bir *cihazın uyumlu olarak işaretlenmesini gerektirme* seçeneği, yönetilen bir cihaz istemek için en güçlü formdur.

![Cihaz tabanlı koşullar](./media/require-managed-devices/11.png)

Bu seçenek, bir cihazın Azure AD 'ye kaydedilmesini ve ayrıca, tarafından uyumlu olarak işaretlenmesini gerektirir:
         
- Intune
- Windows 10 cihazlarını Azure AD tümleştirmesi aracılığıyla yöneten bir üçüncü taraf mobil cihaz yönetimi (MDM) sistemi. Windows 10 dışındaki cihaz işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez.
 
![Cihaz tabanlı koşullar](./media/require-managed-devices/46.png)

Uyumlu olarak işaretlenen bir cihaz için şunları varsayabilirsiniz: 

- İş gücünüzün şirket verilerine erişmek için kullandığı mobil cihazlar yönetilir
- İş gücünüzün kullandığı mobil uygulamalar yönetiliyor
- Şirket bilgileriniz, iş gücünüzün eriştiği ve paylaştığı bir şekilde denetim sağlanmasına yardımcı olur.
- Cihaz ve uygulamaları, şirket güvenlik gereksinimleriyle uyumludur

> [!NOTE]
> Uyumlu cihazlar gerektirecek bir ilke yapılandırırsanız, kullanıcılar, ilke değerlendirmesi sırasında bir cihaz sertifikası seçmek için Mac, iOS ve Android 'de istenebilir. Bu bilinen bir davranıştır.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda cihaz tabanlı bir koşullu erişim ilkesini yapılandırmadan önce, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)göz atalım.
