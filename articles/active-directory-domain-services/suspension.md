---
title: 'Azure Active Directory Domain Services: Askıya alınan etki alanları | Microsoft Docs'
description: Yönetilen etki alanı askıya alma ve silme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 781a81589032c290cef7342e7210ee36f388b22a
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68233981"
---
# <a name="understand-the-suspended-states-for-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Domain Services yönetilen bir etki alanı için askıya alınmış durumları anlayın

Azure Active Directory Domain Services (Azure AD DS), uzun bir süre boyunca yönetilen bir etki alanına hizmet verilemediğinde, yönetilen etki alanını askıya alınma durumuna geçirir. Bu makalede, yönetilen etki alanlarının neden askıya alındığı ve askıya alınan bir etki alanının nasıl düzeltileceği açıklanmaktadır.


## <a name="states-your-managed-domain-can-be-in"></a>Yönetilen etki alanınızı içinde olabilecek durumlar

![Askıya alınan etki alanı zaman çizelgesi](media/active-directory-domain-services-suspension/suspension-timeline.PNG)

Yukarıdaki grafik, Azure AD DS yönetilen bir etki alanının içinde bulunabileceği olası durumları özetler.

### <a name="running-state"></a>"Çalışıyor" durumu
Doğru şekilde yapılandırılan ve düzenli olarak çalışan bir yönetilen etki alanı **çalışır** durumda.

**Bekleneceğiniz**
* Microsoft, yönetilen etki alanınız için düzenli olarak sistem durumunu izleyebilir.
* Yönetilen etki alanınız için etki alanı denetleyicileri düzeltme eki uygulanmış ve düzenli olarak güncelleştirilir.
* Azure Active Directory yapılan değişiklikler, yönetilen etki alanınız ile düzenli olarak eşitlenir.
* Yönetilen etki alanınız için düzenli yedeklemeler alınır.


### <a name="needs-attention-state"></a>"Ilgilenilmesi gerekiyor" durumu
Bir veya daha fazla sorun bir yöneticinin işlem yapması gerekiyorsa, yönetilen etki alanı, **dikkat edilmesi durumunda olmalıdır** . Yönetilen etki alanının sistem durumu sayfasında bu durumdaki bir veya daha fazla uyarı listelenir.

Örneğin, sanal ağınız için bir kısıtlayıcı NSG yapılandırdıysanız Microsoft, yönetilen etki alanınızı güncelleştiremeyebilir ve izleyemeyebilir. Bu geçersiz yapılandırma, yönetilen etki alanınızı "Ilgilenilmesi gerekiyor" durumuna yerleştiren bir uyarı tetikler.

Her uyarının bir çözüm adımları kümesi vardır. Bazı uyarılar geçicidir ve hizmet tarafından otomatik olarak çözümlenir. Söz konusu uyarının ilgili çözüm adımlarında yer alan yönergeleri izleyerek, bazı diğer uyarıları çözebilirsiniz. Bazı kritik uyarılarda, bir çözüm almak için Microsoft desteği 'ne başvurmanız gerekir.

Daha fazla bilgi için bkz. [yönetilen bir etki alanında uyarılarla ilgili sorun giderme](troubleshoot-alerts.md).

**Bekleneceğiniz**

Bazı durumlarda (örneğin, geçersiz bir ağ yapılandırmanız varsa), yönetilen etki alanı için etki alanı denetleyicilerine ulaşılamıyor olabilir. Yönetilen etki alanınız "Ilgilenmeniz gerekiyor" durumundaysa, Microsoft, düzenli olarak izlenebileceği, düzeltme eki uygulanmış, güncelleştirileceğinin veya yedeklendiğini garanti edemez.

* Yönetilen etki alanınız sağlıksız bir durumda ve devam eden sistem durumu izleme, uyarı çözümlenene kadar durabilir.
* Yönetilen etki alanınız için etki alanı denetleyicileri düzeltme eki uygulanabilir veya güncelleştirilemiyor.
* Azure Active Directory yapılan değişiklikler, yönetilen etki alanınız ile eşitlenmeyebilir.
* Mümkünse, yönetilen etki alanınız için yedeklemeler alınmış olabilir.
* Yönetilen etki alanınızı etkileyen uyarıları çözümlediğinizde, "çalışıyor" durumuna geri yükleyebilirsiniz.
* Kritik uyarılar, Microsoft 'un etki alanı denetleyicilerinize ulaşabileceği yapılandırma sorunları için tetiklenir. Bu tür uyarılar 15 gün içinde çözümlenmiyorsa, yönetilen etki alanınız "askıya alındı" durumuna konur.


### <a name="the-suspended-state"></a>"Askıya alındı" durumu
Yönetilen bir etki alanı aşağıdaki nedenlerden dolayı **askıya alındı** durumuna konur:

* 15 gün içinde bir veya daha fazla kritik uyarı çözülmedi. Kritik uyarılara Azure AD DS için gereken kaynaklara erişimi engelleyen bir yanlış yapılandırma neden olabilir.
    * Örneğin, uyarı [AADDS104: Yönetilen etki](alert-nsg.md) alanında 15 günden uzun süredir ağ hatası çözülmedi.
* Azure aboneliğinizle ilgili bir faturalandırma sorunu var veya Azure aboneliğinizin süresi doldu.

Microsoft, etki alanını sürekli olarak yönettemediğinde, izleyemediğinde veya yedekleyemediğinde yönetilen etki alanları askıya alınır.

**Bekleneceğiniz**
* Yönetilen etki alanınız için etki alanı denetleyicileri, sanal ağ içinde sağlanmış ve ulaşılamaz durumda değildir.
* Yönetilen etki alanına internet üzerinden erişim Güvenli LDAP (etkinse) çalışmayı durduruyor.
* Yönetilen etki alanında kimlik doğrulaması, etki alanına katılmış sanal makinelerde oturum açma veya LDAP/LDAPS üzerinden bağlantı kurma hatalarıyla ilgili sorunlar olduğunu fark edersiniz.
* Yönetilen etki alanınız için yedeklemeler artık alınmaz.
* Azure AD ile eşitleme durduruluyor.

Uyarıyı çözdükten sonra, yönetilen etki alanınız "askıya alındı" durumuna geçer. Daha sonra desteğe başvurmanız gerekir.
Destek, yalnızca 30 günden daha eski bir yedekleme varsa, yönetilen etki alanınızı geri yükleyebilir.

Yönetilen etki alanı yalnızca 15 gün boyunca askıya alınmış durumda kalır. Yönetilen etki alanınızı kurtarmak için, Microsoft kritik uyarıları hemen çözmenizi önerir.


### <a name="deleted-state"></a>"Silindi" durumu
15 gün boyunca "askıya alındı" durumunda kalan yönetilen bir etki alanı **silinir**.

**Bekleneceğiniz**
* Yönetilen etki alanı için tüm kaynaklar ve yedeklemeler silinir.
* Yönetilen etki alanını geri yüklemenize ve Azure AD DS kullanmak için yeni bir yönetilen etki alanı oluşturmanız gerekir.
* Silindikten sonra, yönetilen etki alanı için faturalandırılmaz.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Yönetilen etki alanınız askıya alınmış olup olmadığını nasıl anlarsınız?
Etki alanının askıya alındığını bildiren Azure portal Azure AD DS sistem durumu sayfasında bir [Uyarı](troubleshoot-alerts.md) görürsünüz. Etki alanının durumu da "askıya alındı" olarak gösterilir.


## <a name="restore-a-suspended-domain"></a>Askıya alınmış bir etki alanını geri yükleme
"Askıya alındı" durumundaki bir etki alanını geri yüklemek için aşağıdaki adımları uygulayın:

1. Azure portal [Azure Active Directory Domain Services sayfasına](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) gidin.
2. Yönetilen etki alanını seçin.
3. Sol bölmede **sistem durumu**' nu seçin.
4. Uyarıyı seçin. Uyarı KIMLIĞI, askıya alma nedenine bağlı olarak AADDS503 ya da AADDS504 olur.
5. Uyarıda belirtilen çözümleme bağlantısını seçin. Ardından, uyarıyı çözümlemek için adımları izleyin.

Yönetilen etki alanınız yalnızca son yedeklemenin tarihine geri yüklenebilir. Son yedeklemelerinizin tarihi, yönetilen etki alanının sistem durumu sayfasında görüntülenir. Son yedekleme sonrasında gerçekleşen tüm değişiklikler geri yüklenmez. Yönetilen bir etki alanı için yedeklemeler, en fazla 30 gün boyunca depolanır. 30 günden eski olan yedeklemeler silinir.


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınız için uyarıları çözümleyin](troubleshoot-alerts.md)
- [Azure Active Directory Domain Services hakkında daha fazla bilgi edinin](overview.md)
- [Ürün ekibine başvurun](contact-us.md)

## <a name="contact-us"></a>Bizimle iletişim kurun
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
