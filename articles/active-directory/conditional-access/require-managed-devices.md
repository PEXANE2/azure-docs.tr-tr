---
title: Koşullu Erişim yönetilen aygıt gerektirir - Azure Etkin Dizini
description: Bulut uygulaması erişimi için yönetilen aygıtlar gerektiren Azure Etkin Dizin (Azure AD) aygıt tabanlı Koşullu Erişim ilkelerini nasıl yapılandırabilirsiniz öğrenin.
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
ms.openlocfilehash: 8a3c71534febc3cdb6429d3092225ebc73f6cbe7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481492"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Nasıl Yapılsın: Koşullu Erişim ile bulut uygulaması erişimi için yönetilen aygıtları gerektirme

Azure Active Directory (Azure AD), mobil ilk, bulut ilk dünyasında, uygulamaları ve her yerden tek oturum açma olanağı sağlar. Yetkili kullanıcılar bulut uygulamalarınıza mobil cihazlar ve kişisel cihazlar da dahil olmak üzere çok çeşitli cihazlardan erişebilir. Ancak, birçok ortamda yalnızca güvenlik ve uyumluluk standartlarınızı karşılayan aygıtlar tarafından erişilmesi gereken en az birkaç uygulama vardır. Bu aygıtlar yönetilen aygıtlar olarak da bilinir. 

Bu makalede, ortamınızdaki belirli bulut uygulamalarına erişmek için yönetilen aygıtlar gerektiren Koşullu Erişim ilkelerini nasıl yapılandırabileceğiniz açıklanmaktadır. 

## <a name="prerequisites"></a>Ön koşullar

Bulut uygulaması erişimi için yönetilen aygıtlar gerektirmesi, **Azure AD Koşullu Erişim** ve Azure AD aygıt **yönetimini** birbirine bağlar. Henüz bu alanlardan birine aşina değilseniz, öncelikle aşağıdaki konuları okumalısınız:

- **[Azure Etkin Dizinde Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)** - Bu makalede, Koşullu Erişim ve ilgili terminolojiye kavramsal bir genel bakış sağlanır.
- **[Azure Active Directory'de cihaz yönetimine giriş](../devices/overview.md)** - Bu makalede, aygıtları kuruluş denetimi altında almak için sahip olduğunuz çeşitli seçeneklere genel bir bakış sunulmaktadır. 
- **Windows 10 Creators Update (sürüm 1703)** veya daha sonra Chrome desteği için [Windows 10 Hesapları uzantısını](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)yükleyin. Koşullu Erişim ilkesi aygıta özel ayrıntılar gerektiriyorsa, bu uzantı gereklidir.

>[!NOTE] 
> İlk aygıt kimlik doğrulaması sonrasında en iyi zorlamayı elde etmek için Azure AD aygıt tabanlı Koşullu Erişim ilkesini kullanmanızı öneririz. Bu, aygıt uyumluluğu ve aygıt kodu akışının dışına düşerse oturumları kapatmayı içerir.


## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi mastering bir meydan okumadır. Bulut kaynaklarınıza erişmek için desteklenen cihazların yaygınlaşması, kullanıcılarınızın üretkenliğini artırmaya yardımcı olur. Diğer taraftan, büyük olasılıkla ortamınızdaki belirli kaynaklara bilinmeyen koruma düzeyine sahip aygıtlar tarafından erişilmesini istemezsiniz. Etkilenen kaynaklar için, kullanıcıların bunlara yalnızca yönetilen bir aygıt kullanarak erişebilmeleri gerekir. 

Azure AD Koşullu Erişim ile bu gereksinimi erişim sağlayan tek bir ilkeyle giderebilirsiniz:

- Seçili bulut uygulamalarına
- Seçili kullanıcılar ve gruplar için
- Yönetilen bir aygıt gerektirmesi

## <a name="managed-devices"></a>Yönetilen cihazlar  

Basit bir ifadeyle, yönetilen aygıtlar *bir tür* kuruluş denetimi altında olan aygıtlardır. Azure AD'de, yönetilen bir aygıtın ön koşulu Azure AD'ye kaydedilmiş olmasıdır. Aygıtı kaydetmek, aygıt nesnesi biçiminde aygıt için bir kimlik oluşturur. Bu nesne, bir aygıt hakkındaki durum bilgilerini izlemek için Azure tarafından kullanılır. Azure AD yöneticisi olarak, aygıtın durumunu kaydırmak (etkinleştirmek/devre dışı etmek) için bu nesneyi zaten kullanabilirsiniz.
  
![Cihaz tabanlı koşullar](./media/require-managed-devices/32.png)

Bir aygıtıAzure AD'ye kaydettirmek için üç seçeneğiniz vardır: 

- **Azure AD kayıtlı cihazlar** - Azure AD'ye kayıtlı bir kişisel cihaz almak için
- **Azure AD aygıtlara katıldı** - Azure AD'ye kayıtlı şirket içi bir REKLAM'a katılmayan bir kuruluş windows 10 aygıtı almak için. 
- **Karma Azure AD aygıtlara katıldı** - Windows 10 almak için veya Azure AD'ye kayıtlı şirket içi bir REKLAM'a katılan alt düzey aygıtlar desteklendi.

Bu üç seçenek makalede ele alınmıştır [aygıt kimliği nedir?](../devices/overview.md)

Yönetilen bir aygıt olmak için, kayıtlı bir aygıtın **Karma Azure AD'ye katılan aygıt** veya uyumlu olarak işaretlenmiş bir **aygıt**olması gerekir.  

![Cihaz tabanlı koşullar](./media/require-managed-devices/47.png)
 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Karma Azure AD'nin birleştirilmiş aygıtları gerektirmesi

Koşullu Erişim ilkenizde, seçili bulut uygulamalarına yalnızca yönetilen bir aygıt kullanılarak erişilebileni belirtmek için **Karma Azure AD'ye katılmayı İste'yi** seçebilirsiniz. 

![Cihaz tabanlı koşullar](./media/require-managed-devices/10.png)

Bu ayar yalnızca windows 10 veya windows 7 veya Windows 8 gibi şirket içi BIR AD'ye katılan alt düzey aygıtlar için geçerlidir. Bu aygıtları yalnızca, Windows 10 aygıtının kaydedilmesi için otomatik bir [işlem](../devices/hybrid-azuread-join-plan.md) olan Karma Azure AD birleştirmesini kullanarak Azure AD'ye kaydedebilirsiniz. 

![Cihaz tabanlı koşullar](./media/require-managed-devices/45.png)

Karma Azure AD'nin birleştirilmiş aygıtı yönetilen bir aygıt haline getiren nedir?  Şirket içi BIR AD'ye katılan aygıtlar için, bu aygıtlar üzerindeki **denetimin,** bunları yönetmek için Configuration Manager veya grup **ilkesi (GP)** gibi yönetim çözümleri kullanılarak zorlanır. Azure AD'nin bu yöntemlerden herhangi birinin bir aygıta uygulanıp uygulanmadığını belirlemesi için bir yöntem olmadığından, karma Azure AD birleştirilmiş aygıt gerektiren, yönetilen bir aygıt gerektiren nispeten zayıf bir mekanizmadır. Şirket içi etki alanı birleştirilmiş aygıtlarınıza uygulanan yöntemlerin, bu tür bir aygıt aynı zamanda Karma Azure AD'ye katılan bir aygıtsa yönetilen bir aygıt oluşturacak kadar güçlü olup olmadığını değerlendirmek bir yönetici olarak size bağlıdır.

## <a name="require-device-to-be-marked-as-compliant"></a>Aygıtın uyumlu olarak işaretlemesi

Bir *aygıtın uyumlu olarak işaretlenmesini isteme* seçeneği, yönetilen bir aygıt istemek için en güçlü formdur.

![Cihaz tabanlı koşullar](./media/require-managed-devices/11.png)

Bu seçenek, bir aygıtın Azure AD'ye kaydolmasını ve ayrıca şu lar tarafından uyumlu olarak işaretleneceğini gerektirir:
         
- Intune
- Windows 10 aygıtlarını Azure AD tümleştirmesi ile yöneten üçüncü taraf mobil aygıt yönetimi (MDM) sistemi. Windows 10 dışındaki aygıt işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez.
 
![Cihaz tabanlı koşullar](./media/require-managed-devices/46.png)

Uyumlu olarak işaretlenmiş bir aygıt için şunları varsayabilirsiniz: 

- İş gücünüzün şirket verilerine erişmek için kullandığı mobil cihazlar yönetilir
- İş gücünüzünüzün kullandığı mobil uygulamalar yönetilir
- Şirket bilgileriniz, iş gücünüzün bu bilgilere erişimini ve paylaşma şeklini kontrol etmeye yardımcı olarak korunur
- Cihaz ve uygulamaları şirket güvenlik gereksinimleriyle uyumlu

### <a name="known-behavior"></a>Bilinen davranış

Windows 7, iOS, Android, macOS ve bazı üçüncü taraf web tarayıcılarında Azure AD, aygıt Azure AD'ye kaydedildiğinde sağlanan bir istemci sertifikası kullanarak aygıtı tanımlar. Bir kullanıcı tarayıcı üzerinden ilk giriş yaptığında, kullanıcıdan sertifikayı seçmesi istenir. Tarayıcıyı kullanmaya devam edebilmek için son kullanıcının bu sertifikayı seçmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda aygıt tabanlı Koşullu Erişim ilkesini yapılandırmadan önce, [Azure Etkin Dizininde Koşullu Erişim için en iyi uygulamalara](best-practices.md)göz atmalısınız.
