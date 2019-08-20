---
title: 'Azure Active Directory Domain Services: Uyarı sorunlarını giderme | Microsoft Docs'
description: Azure AD Domain Services uyarıları sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 5a8f3401de0dc452193efbcf79aef87a19aed081
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617081"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services-uyarıların sorunlarını giderme
Bu makalede, yönetilen etki alanınız üzerinde karşılaşabileceğiniz tüm uyarılar için sorun giderme kılavuzu sağlanmıştır.


Uyarıdaki KIMLIĞE veya iletiye karşılık gelen sorun giderme adımlarını seçin.

| **Uyarı KIMLIĞI** | **Uyarı Iletisi** | **Çözümleme** |
| --- | --- | :--- |
| AADDS001 | *Internet üzerinden Güvenli LDAP, yönetilen etki alanı için etkinleştirilmiştir. Ancak, 636 numaralı bağlantı noktasına erişim bir ağ güvenlik grubu kullanılarak kilitlenmemiştir. Bu, yönetilen etki alanındaki Kullanıcı hesaplarını parola yanılma saldırısı saldırılarına maruz bırakabilir.* | [Yanlış Güvenli LDAP yapılandırması](alert-ldaps.md) |
| AADDS100 | *Yönetilen etki alanınız ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırmada değil. Microsoft, yönetilen etki alanınızı izleyemez, yönetemez, düzeltme eki ve eşitleyemiyor.* | [Eksik dizin](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services bir Azure AD B2C dizininde etkinleştirilemez.* | [Azure AD B2C bu dizinde çalışıyor](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD Domain Services çalışması için gereken bir hizmet sorumlusu Azure AD dizininden silindi. Bu yapılandırma, Microsoft 'un yönetilen etki alanınızı izleme, yönetme, düzeltme eki uygulama ve eşitlemeye yönelik yeteneğini etkiler.* | [Hizmet sorumlusu eksik](alert-service-principal.md) |
| AADDS103 | *Azure AD Domain Services etkinleştirdiğiniz sanal ağın IP adresi aralığı ortak bir IP aralığında. Azure AD Domain Services, özel bir IP adresi aralığına sahip bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, Microsoft 'un yönetilen etki alanınızı izleme, yönetme, düzeltme eki uygulama ve eşitlemeye yönelik yeteneğini etkiler.* | [Adres genel bir IP aralığında](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyor. Sanal ağınızda yapılandırılan bir ağ güvenlik grubu (NSG), yönetilen etki alanına erişimi engelliyorsa bu durum oluşabilir. Bunun olası bir nedeni, internet 'ten gelen trafiği engelleyen Kullanıcı tanımlı bir yol olup olmalarıdır.* | [Ağ hatası](alert-nsg.md) |
| AADDS105 | *"D87dcbc6-a371-462e-88e3-28ad15ec4e64" uygulama KIMLIĞINE sahip hizmet sorumlusu silindi ve sonra yeniden oluşturuldu. Yeniden oluşturma, yönetilen etki alanınızı hizmet etmek için gereken Azure AD Domain Services kaynaklardaki tutarsız izinlerin arkasında kalır. Yönetilen etki alanındaki parolaların eşitlenmesi etkilenebilir.* | [Parola eşitleme uygulaması güncel değil](alert-service-principal.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS106 | *Yönetilen etki alanınız ile ilişkili Azure aboneliğiniz silindi.  Azure AD Domain Services düzgün şekilde çalışmaya devam etmek için etkin bir abonelik gerektirir.* | [Azure aboneliği bulunamadı](#aadds106-your-azure-subscription-is-not-found) |
| AADDS107 | *Yönetilen etki alanınız ile ilişkili Azure aboneliğiniz etkin değil.  Azure AD Domain Services düzgün şekilde çalışmaya devam etmek için etkin bir abonelik gerektirir.* | [Azure aboneliği devre dışı](#aadds107-your-azure-subscription-is-disabled) |
| AADDS108 | *Azure AD Domain Services tarafından kullanılan abonelik başka bir dizine taşındı. Azure AD Domain Services düzgün şekilde çalışması için aynı dizinde etkin bir aboneliğe sahip olması gerekir.* | [Abonelik taşınan dizinler](#aadds108-subscription-moved-directories) |
| AADDS109 | *Yönetilen etki alanınız için kullanılan bir kaynak silindi. Azure AD Domain Services düzgün çalışması için bu kaynak gereklidir.* | [Bir kaynak silindi](#aadds109-resources-for-your-managed-domain-cannot-be-found) |
| AADDS110 | *Azure AD Domain Services dağıtımı için seçilen alt ağ dolu ve oluşturulması gereken ek etki alanı denetleyicisi için alan yok.* | [Alt ağ dolu](#aadds110-the-subnet-associated-with-your-managed-domain-is-full) |
| AADDS111 | *Azure AD Domain Services, etki alanına hizmet vermek için kullandığı bir hizmet sorumlusu, Azure aboneliğindeki kaynakları yönetme yetkisine sahip değildir. Hizmet sorumlusu, yönetilen etki alanınızı hizmetine hizmet vermek için izinleri kazanmak için gereklidir.* | [Hizmet sorumlusu yetkilendirilmemiş](#aadds111-service-principal-unauthorized) |
| AADDS112 | *Bu etki alanındaki sanal ağın alt ağının yeterli IP adresi bulunmayabilir. Azure AD Domain Services, üzerinde etkin olduğu alt ağ içinde en az iki kullanılabilir IP adresi gerektirir. Alt ağ içinde en az 3-5 yedek IP adresi olmasını öneririz. Bu durum alt ağ içinde başka sanal makineler dağıtılmışsa, bu nedenle kullanılabilir IP adresi sayısını veya alt ağdaki kullanılabilir IP adresleri sayısında bir kısıtlama varsa oluşmuş olabilir.* | [Yeterli IP adresi yok](#aadds112-not-enough-ip-address-in-the-managed-domain) |
| AADDS113 | *Azure AD Domain Services tarafından kullanılan kaynaklar beklenmeyen bir durumda algılandı ve kurtarılamıyor.* | [Kaynaklar kurtarılamaz](#aadds113-resources-are-unrecoverable) |
| AADDS114 | *Azure AD Domain Services dağıtımı için seçilen alt ağ geçersiz ve kullanılamaz.* | [Alt ağ geçersiz](#aadds114-subnet-invalid) |
| AADDS115 | *Hedef kapsam kilitlendiğinden, yönetilen etki alanı tarafından kullanılan bir veya daha fazla ağ kaynağı üzerinde çalışılamıyor.* | [Kaynaklar kilitli](#aadds115-resources-are-locked) |
| AADDS116 | *Yönetilen etki alanı tarafından kullanılan bir veya daha fazla ağ kaynağı, ilke kısıtlaması nedeniyle üzerinde çalıştırılamaz.* | [Kaynaklar kullanılamaz](#aadds116-resources-are-unusable) |
| AADDS500 | *Yönetilen etki alanı, [Date] tarihinde Azure AD ile en son eşitlendi. Kullanıcılar yönetilen etki alanında oturum açamıyor olabilir veya grup üyelikleri Azure AD ile eşitlenmemiş olabilir.* | [Eşitleme bir süredir gerçekleşmemiştir](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Yönetilen etki alanı, [Date] tarihinde en son yedeklendi.* | [Bir yedekleme bir süredir alınmadı](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Yönetilen etki alanı için Güvenli LDAP sertifikasının kullanım süreleri [Date].* | [Süresi dolan Güvenli LDAP sertifikası](alert-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Etki alanıyla ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alındı.* | [Devre dışı bırakılan abonelik nedeniyle askıya alma](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alındı. Hizmet, yönetilen etki alanı için etki alanı denetleyicilerini uzun bir süre yönetmedi, düzeltme eki veya güncelleştirme yapamadı.* | [Geçersiz bir yapılandırma nedeniyle askıya alma](#aadds504-suspension-due-to-an-invalid-configuration) |



## <a name="aadds100-missing-directory"></a>AADDS100: Eksik dizin
**Uyarı iletisi:**

*Yönetilen etki alanınız ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırmada değil. Microsoft, yönetilen etki alanınızı izleyemez, yönetemez, düzeltme eki ve eşitleyemiyor.*

**Çözüm:**

Bu hata genellikle, Azure aboneliğinizi yeni bir Azure AD dizinine doğru bir şekilde taşırken ve Azure AD Domain Services hala ilişkili olan eski Azure AD dizinini silerek oluşur.

Bu hata kurtarılamaz. Çözümlemek için, [mevcut yönetilen etki alanınızı silmeli](delete-aadds.md) ve yeni dizininizde yeniden oluşturmanız gerekir. Silme konusunda sorun yaşıyorsanız, [destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C bu dizinde çalışıyor
**Uyarı iletisi:**

*Azure AD Domain Services bir Azure AD B2C dizininde etkinleştirilemez.*

**Çözüm:**

>[!NOTE]
>Azure AD Domain Services kullanmaya devam etmek için Azure AD Domain Services örneğinizi Azure AD B2C olmayan bir dizinde yeniden oluşturmanız gerekir.

Hizmetinizi geri yüklemek için şu adımları izleyin:

1. [Yönetilen etki alanınızı](delete-aadds.md) mevcut Azure AD dizininden silin.
2. Azure AD B2C Dizin olmayan yeni bir dizin oluşturun.
3. Yönetilen bir etki alanını yeniden oluşturmak için [Başlarken](tutorial-create-instance.md) kılavuzunu izleyin.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres genel bir IP aralığında

**Uyarı iletisi:**

*Azure AD Domain Services etkinleştirdiğiniz sanal ağın IP adresi aralığı ortak bir IP aralığında. Azure AD Domain Services, özel bir IP adresi aralığına sahip bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, Microsoft 'un yönetilen etki alanınızı izleme, yönetme, düzeltme eki uygulama ve eşitlemeye yönelik yeteneğini etkiler.*

**Çözüm:**

> [!NOTE]
> Bu sorunu gidermek için, mevcut yönetilen etki alanınızı silmeli ve özel bir IP adresi aralığına sahip bir sanal ağda yeniden oluşturmanız gerekir. Bu işlem karışıklığa neden olur.

Başlamadan önce, [Bu makaledeki](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces) **özel IP v4 adres alanı** bölümünü okuyun.

Sanal ağ içinde makineler, alt ağ için yapılandırılmış olanlarla aynı IP adresi aralığındaki Azure kaynaklarına istek yapabilir. Ancak, sanal ağ bu Aralık için yapılandırıldığından, bu istekler sanal ağ içinde yönlendirilir ve hedeflenen web kaynaklarına ulaşmaz. Bu Yapılandırma Azure AD Domain Services öngörülemeyen hatalara neden olabilir.

**Sanal ağınızda yapılandırılmış Internet 'teki IP adresi aralığına sahipseniz, bu uyarı yoksayılabilir. Ancak, bu yapılandırmayla Azure AD Domain Services [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)'ya kaydedilemez] çünkü öngörülemeyen hatalara yol açabilir.**


1. [Yönetilen etki](delete-aadds.md) alanınızı dizininizden silin.
2. Alt ağ için IP adres aralığını çözün
   1. [Azure Portal sanal ağlar sayfasına](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks)gidin.
   2. Azure AD Domain Services için kullanmayı planladığınız sanal ağı seçin.
   3. Ayarlar altındaki **Adres alanı** ' na tıklayın
   4. Adres aralığını, var olan adres aralığına tıklayıp düzenleyerek veya ek bir adres aralığı ekleyerek güncelleştirin. Yeni adres aralığının özel bir IP aralığında olduğundan emin olun. Yaptığınız değişiklikleri kaydedin.
   5. Sol taraftaki **alt ağlar** ' a tıklayın.
   6. Tabloda düzenlemek istediğiniz alt ağa tıklayın.
   7. Adres aralığını güncelleştirin ve değişikliklerinizi kaydedin.
3. Yönetilen etki alanınızı yeniden oluşturmak için [Azure AD Domain Services kullanmaya başlama kılavuzunu](tutorial-create-instance.md) izleyin. Özel bir IP adresi aralığına sahip bir sanal ağ seçtiğinizden emin olun.
4. Sanal makinelerinizi etki alanına katmak için [Bu kılavuzu](join-windows-vm.md)izleyin.
8. Uyarının çözümlendiğinden emin olmak için, etki alanının sistem durumunu iki saat içinde denetleyin.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Azure aboneliğiniz bulunamadı

**Uyarı iletisi:**

*Yönetilen etki alanınız ile ilişkili Azure aboneliğiniz silindi.  Azure AD Domain Services düzgün şekilde çalışmaya devam etmek için etkin bir abonelik gerektirir.*

**Çözüm:**

Azure AD Domain Services, bir aboneliğin çalışmasını gerektirir ve farklı bir aboneliğe taşınamaz. Yönetilen etki alanınızı ilişkili olan Azure aboneliği silindiğinden, bir Azure aboneliği ve Azure AD Domain Services yeniden oluşturmanız gerekir.

1. Bir Azure aboneliği oluşturun
2. [Yönetilen etki alanınızı](delete-aadds.md) mevcut Azure AD dizininden silin.
3. Yönetilen bir etki alanını yeniden oluşturmak için [Başlarken](tutorial-create-instance.md) kılavuzunu izleyin.

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: Azure aboneliğiniz devre dışı

**Uyarı iletisi:**

*Yönetilen etki alanınız ile ilişkili Azure aboneliğiniz etkin değil.  Azure AD Domain Services düzgün şekilde çalışmaya devam etmek için etkin bir abonelik gerektirir.*

**Çözüm:**


1. [Azure aboneliğinizi yenileyin](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable).
2. Abonelik yenilendikten sonra, yönetilen etki alanınızı yeniden etkinleştirmek için Azure AD Domain Services Azure 'dan bir bildirim gönderilir.

## <a name="aadds108-subscription-moved-directories"></a>AADDS108: Abonelik taşınan dizinler

**Uyarı iletisi:**

*Azure AD Domain Services tarafından kullanılan abonelik başka bir dizine taşındı. Azure AD Domain Services düzgün şekilde çalışması için aynı dizinde etkin bir aboneliğe sahip olması gerekir.*

**Çözüm:**

Azure AD Domain Services ilişkili aboneliğini önceki dizine taşıyabilir ya da [yönetilen etki alanınızı](delete-aadds.md) var olan dizinden silip seçili dizinde yeniden oluşturmanız gerekir (yeni bir abonelik ile ya da Azure AD Domain Services örneğinizin bulunduğu dizini değiştirin).

## <a name="aadds109-resources-for-your-managed-domain-cannot-be-found"></a>AADDS109: Yönetilen etki alanınız için kaynaklar bulunamıyor

**Uyarı iletisi:**

*Yönetilen etki alanınız için kullanılan bir kaynak silindi. Azure AD Domain Services düzgün çalışması için bu kaynak gereklidir.*

**Çözüm:**

Azure AD Domain Services, genel IP adresleri, NIC 'ler ve bir yük dengeleyici dahil olmak üzere, dağıtımı sırasında belirli kaynakları oluşturur. Adlandırılmış herhangi biri silinirse, bu, yönetilen etki alanının desteklenmeyen bir durumda olmasına ve etki alanının yönetilmesini engeller. Bu uyarı, Azure AD Domain Services kaynaklarını düzenleyebilecek birisi gerekli kaynağı silerse bulunur. Aşağıdaki adımlarda, yönetilen etki alanınızı nasıl geri yükleyeceğiniz gösterilmektedir.

1. Azure AD Domain Services sistem durumu sayfasına gidin
   1.    Azure portal [Azure AD Domain Services sayfasına](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) gidin.
   2.    Sol taraftaki gezinmede **sistem durumu** ' na tıklayın.
2. Uyarının 4 saatten daha eski olup olmadığını denetleyin
   1.    Sistem durumu sayfasında KIMLIĞI **AADDS109** olan uyarıya tıklayın.
   2.    Uyarı, ilk kez bulunduğunda zaman damgasına sahip olur. Bu zaman damgası 4 saatten küçükse, Azure AD Domain Services silinen kaynağı yeniden oluşturmak için bir şansınız vardır.
3. Uyarı 4 saatten eski olursa, yönetilen etki alanı kurtarılamaz durumda olur. Azure AD Domain Services silip yeniden oluşturmanız gerekir.


## <a name="aadds110-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS110: Yönetilen etki alanınız ile ilişkili alt ağ dolu

**Uyarı iletisi:**

*Azure AD Domain Services dağıtımı için seçilen alt ağ dolu ve oluşturulması gereken ek etki alanı denetleyicisi için alan yok.*

**Çözüm:**

Bu hata kurtarılamaz. Çözümlemek için, [mevcut yönetilen etki alanınızı silmeli](delete-aadds.md) ve [yönetilen etki alanınızı yeniden oluşturmanız](tutorial-create-instance.md) gerekir

## <a name="aadds111-service-principal-unauthorized"></a>AADDS111: Hizmet sorumlusu yetkilendirilmemiş

**Uyarı iletisi:**

*Azure AD Domain Services, etki alanına hizmet vermek için kullandığı bir hizmet sorumlusu, Azure aboneliğindeki kaynakları yönetme yetkisine sahip değildir. Hizmet sorumlusu, yönetilen etki alanınızı hizmetine hizmet vermek için izinleri kazanmak için gereklidir.*

**Çözüm:**

Hizmet sorumlularımızın yönetilen etki alanında kaynakları yönetebilmeleri ve oluşturabilmeleri için erişmesi gerekir. Birisi hizmet sorumlusu erişimini reddetti ve artık kaynakları yönetmedi. Hizmet sorumlusuna erişim izni vermek için adımları izleyin.

1. [RBAC denetimi ve Azure Portal uygulamalara erişim verme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) hakkında bilgi edinin
2. Hizmet sorumlusunun kimliğe ```abba844e-bc0e-44b0-947a-dc74e5d09022``` sahip olan erişimini gözden geçirin ve daha önceki bir tarihte reddedilen erişimi verin.


## <a name="aadds112-not-enough-ip-address-in-the-managed-domain"></a>AADDS112: Yönetilen etki alanında yeterli IP adresi yok

**Uyarı iletisi:**

*Bu etki alanındaki sanal ağın alt ağının yeterli IP adresi bulunmayabilir. Azure AD Domain Services, üzerinde etkin olduğu alt ağ içinde en az iki kullanılabilir IP adresi gerektirir. Alt ağ içinde en az 3-5 yedek IP adresi olmasını öneririz. Bu durum alt ağ içinde başka sanal makineler dağıtılmışsa, bu nedenle kullanılabilir IP adresi sayısını veya alt ağdaki kullanılabilir IP adresleri sayısında bir kısıtlama varsa oluşmuş olabilir.*

**Çözüm:**

1. Yönetilen etki alanınızı kiracınızdan silin.
2. Alt ağ için IP adres aralığını çözün
   1. [Azure Portal sanal ağlar sayfasına](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks)gidin.
   2. Azure AD Domain Services için kullanmayı planladığınız sanal ağı seçin.
   3. Ayarlar altındaki **Adres alanı** ' na tıklayın
   4. Adres aralığını, var olan adres aralığına tıklayıp düzenleyerek veya ek bir adres aralığı ekleyerek güncelleştirin. Yaptığınız değişiklikleri kaydedin.
   5. Sol taraftaki **alt ağlar** ' a tıklayın.
   6. Tabloda düzenlemek istediğiniz alt ağa tıklayın.
   7. Adres aralığını güncelleştirin ve değişikliklerinizi kaydedin.
3. Yönetilen etki alanınızı yeniden oluşturmak için [Azure AD Domain Services kullanmaya başlama kılavuzunu](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) izleyin. Özel bir IP adresi aralığına sahip bir sanal ağ seçtiğinizden emin olun.
4. Sanal makinelerinizi etki alanına katmak için [Bu kılavuzu](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal)izleyin.
5. Adımları doğru tamamladığınızdan emin olmak için etki alanının sistem durumunu iki saat içinde denetleyin.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Kaynaklar kurtarılamaz

**Uyarı iletisi:**

*Azure AD Domain Services tarafından kullanılan kaynaklar beklenmeyen bir durumda algılandı ve kurtarılamıyor.*

**Çözüm:**

Bu hata kurtarılamaz. Çözümlemek için, [mevcut yönetilen etki alanınızı silmeli](delete-aadds.md) ve [yönetilen etki alanınızı yeniden oluşturmanız](tutorial-create-instance.md)gerekir.

## <a name="aadds114-subnet-invalid"></a>AADDS114: Alt ağ geçersiz

**Uyarı iletisi:**

*Azure AD Domain Services dağıtımı için seçilen alt ağ geçersiz ve kullanılamaz.*

**Çözüm:**

Bu hata kurtarılamaz. Çözümlemek için, [mevcut yönetilen etki alanınızı silmeli](delete-aadds.md) ve [yönetilen etki alanınızı yeniden oluşturmanız](tutorial-create-instance.md)gerekir.

## <a name="aadds115-resources-are-locked"></a>AADDS115: Kaynaklar kilitli

**Uyarı iletisi:**

*Hedef kapsam kilitlendiğinden, yönetilen etki alanı tarafından kullanılan bir veya daha fazla ağ kaynağı üzerinde çalışılamıyor.*

**Çözüm:**

1.  Ağ kaynaklarında Kaynak Yöneticisi işlem günlüklerini gözden geçirin (Bu, değişikliğin hangi kilit tarafından önleneceği hakkında bilgi vermelidir).
2.  Azure AD Domain Services hizmet sorumlusunun üzerinde çalışabilmesi için kaynaklardaki kilitleri kaldırın.

## <a name="aadds116-resources-are-unusable"></a>AADDS116: Kaynaklar kullanılamaz

**Uyarı iletisi:**

*Yönetilen etki alanı tarafından kullanılan bir veya daha fazla ağ kaynağı, ilke kısıtlaması nedeniyle üzerinde çalıştırılamaz.*

**Çözüm:**

1.  Yönetilen etki alanınız için ağ kaynaklarında Kaynak Yöneticisi işlem günlüklerini gözden geçirin.
2.  AAD-DS hizmet sorumlusunun üzerinde çalışabilmesi için kaynakların ilke kısıtlamalarını zayıflatın.



## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Eşitleme bir süre içinde tamamlanmadı

**Uyarı iletisi:**

*Yönetilen etki alanı, [Date] tarihinde Azure AD ile en son eşitlendi. Kullanıcılar yönetilen etki alanında oturum açamıyor olabilir veya grup üyelikleri Azure AD ile eşitlenmemiş olabilir.*

**Çözüm:**

Yönetilen etki alanınızı yapılandırmanızda sorun olduğunu gösterebilen tüm uyarılar için [etki alanı sistem durumunu kontrol edin](check-health.md) . Bazen, yapılandırmanızla ilgili sorunlar Microsoft 'un yönetilen etki alanınızı eşitlemesine engel olabilir. Uyarıları çözebiliyorsanız, iki saat bekleyip eşitlemenin tamamlanıp tamamlanmadığını görmek için yeniden denetleyin.

Yönetilen etki alanlarında eşitlemenin durdurulduğu bazı yaygın nedenler şunlardır:
- Yönetilen etki alanında ağ bağlantısı engellendi. Ağınızı sorunlar için denetleme hakkında daha fazla bilgi edinmek için bkz. [ağ güvenlik grupları](alert-nsg.md) ve [ağ gereksinimleriyle](network-considerations.md)ilgili sorun giderme Azure AD Domain Services.
-  Parola eşitleme, hiçbir zaman ayarlanmadı veya tamamlanmadı. Parola eşitlemeyi ayarlamak için [Bu makaleyi](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)okuyun.

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Bir yedekleme sırasında bir yedek alınmadı

**Uyarı iletisi:**

*Yönetilen etki alanı, [Date] tarihinde en son yedeklendi.*

**Çözüm:**

Yönetilen etki alanınızı yapılandırmanızda sorun olduğunu gösterebilen tüm uyarılar için [etki alanı sistem durumunu kontrol edin](check-health.md) . Bazen, yapılandırmanızla ilgili sorunlar Microsoft 'un yönetilen etki alanınızı yedekleme yeteneğini engelleyebilir. Uyarıları çözebiliyorsanız, iki saat bekleyin ve yedeklemenin tamamlanıp tamamlanmadığını görmek için yeniden denetleyin.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Devre dışı bırakılan abonelik nedeniyle askıya alma

**Uyarı iletisi:**

*Etki alanıyla ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alındı.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınız uzun bir süre askıya alınırsa, bu, silinmekte olan tehlike altında olur. En iyi şekilde askıya alınması mümkün olduğunca hızlı bir şekilde ele alınabilir. Daha fazla bilgi edinmek için [Bu makaleyi](suspension.md)ziyaret edin.

Hizmetinizi geri yüklemek için, yönetilen etki alanınız ile ilişkili [Azure aboneliğinizi yenileyin](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) .

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Geçersiz bir yapılandırma nedeniyle askıya alma

**Uyarı iletisi:**

*Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alındı. Hizmet, yönetilen etki alanı için etki alanı denetleyicilerini uzun bir süre yönetmedi, düzeltme eki veya güncelleştirme yapamadı.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınız uzun bir süre askıya alınırsa, bu, silinmekte olan tehlike altında olur. En iyi şekilde askıya alınması mümkün olduğunca hızlı bir şekilde ele alınabilir. Daha fazla bilgi edinmek için [Bu makaleyi](suspension.md)ziyaret edin.

Yönetilen etki alanınızı yapılandırmanızda sorun olduğunu gösterebilen tüm uyarılar için [etki alanı sistem durumunu kontrol edin](check-health.md) . Bu uyarıların hiçbirini çözebiliyorsanız, bunu yapın. Sonra, aboneliğinizi yeniden etkinleştirmek için desteğe başvurun.


## <a name="contact-us"></a>Bizimle iletişim kurun
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
