---
title: 'Azure AD Connect: Sorun giderme nesne senkronizasyonu | Microsoft Dokümanlar'
description: Bu konu, sorun giderme görevini kullanarak nesne eşitleme ile sorunları gidermek için nasıl adımlar sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e56d4d94e38e5095ef2223d0cc2875cbf1dcd46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64919112"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitleme ile nesne eşitleme sorunlarını giderme
Bu makalede, sorun giderme görevi kullanarak nesne eşitleme ile sorun giderme sorunları için adımlar sağlar. Azure Active Directory (Azure AD) Connect'te sorun giderme nin nasıl çalıştığını görmek için [bu kısa videoyu](https://aka.ms/AADCTSVideo)izleyin.

## <a name="troubleshooting-task"></a>Sorun giderme görevi
Azure AD Connect dağıtımınız sürüm 1.1.749.0 veya üzeri ise nesne eşitleme sorunlarını gidermek için sihirbazdaki sorun giderme görevini kullanın. Önceki sürümler için lütfen [burada](tshoot-connect-object-not-syncing.md)açıklandığı gibi el ile sorun giderin.

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Sihirbazdaki sorun giderme görevini çalıştırma
Sihirbazda sorun giderme görevini çalıştırmak için aşağıdaki adımları gerçekleştirin:

1.  Azure AD Connect sunucunuzda Yönetici olarak Çalıştır seçeneğiyle yeni bir Windows PowerShell oturumu açın.
2.  Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .
3.  Azure AD Bağlantısı sihirbazını başlatın.
4.  Ek Görevler sayfasına gidin, Sorun Gider'i seçin ve İleri'yi tıklatın.
5.  Sorun Giderme sayfasında, PowerShell'deki sorun giderme menüsünü başlatmak için Başlat'ı tıklatın.
6.  Ana menüde Sorun Giderme Nesneeşitlemesi'ni seçin.
![Sorun giderme nesne eşitleme](media/tshoot-connect-objectsync/objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Sorun Giderme Giriş Parametreleri
Sorun giderme görevi tarafından aşağıdaki giriş parametreleri gereklidir:
1.  **Nesne Ayırt Edici Ad** – Bu, sorun giderme gerektiren nesnenin ayırt edici adıdır
2.  **AD Bağlayıcı Adı** – Yukarıdaki nesnenin bulunduğu AD ormanının adıdır.
3.  Azure AD kiracı ![genel yönetici kimlik bilgileri genel yönetici kimlik bilgileri](media/tshoot-connect-objectsync/objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevinin sonuçlarını anlama
Sorun giderme görevi aşağıdaki denetimleri gerçekleştirir:

1.  Nesne Azure Etkin Dizini ile eşitlenirse UPN uyumsuzluğu algılama
2.  Etki alanı filtreleme nedeniyle nesnenin filtre uygulanıp uygulanmayana değdiğini denetleme
3.  OU filtreleme nedeniyle nesnenin filtre uygulanıp uygulanmayanmasını denetleme
4.  Bağlı bir posta kutusu nedeniyle nesne eşitlemesi engellendiğini denetleme
5. Nesnenin senkronize olmaması gereken dinamik dağıtım grubu olup olmadığını denetleme

Bu bölümün geri kalanı, görev tarafından döndürülen belirli sonuçları açıklar. Her durumda, görev sorunu çözmek için önerilen eylemleri izleyen bir çözüm sağlar.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Nesne Azure Etkin Dizini ile eşitlenirse UPN eşleşmesini algılama
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN Soneki Azure AD Kiracıile DOĞRULANMAZ
UserPrincipalName (UPN)/Alternatif Giriş Kimliği soneki Azure AD Kiracısı ile doğrulanmadığında, Azure Etkin Dizin, UPN soneklerini varsayılan etki alanı adı "onmicrosoft.com" ile değiştirir.

![Azure AD UPN'nin yerini alır](media/tshoot-connect-objectsync/objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>UPN Soneki'nin bir federe etki alanından başka bir federe etki alanına değiştirilmesi
Azure Active Directory, UserPrincipalName (UPN)/Alternatif Oturum Açma Kimliği sonekinin bir federe etki alanından başka bir federe etki alanına eşitlenmesine izin vermez. Bu, Azure AD Kiracısı ile doğrulanan ve Kimlik Doğrulama Türüne Federe olarak sahip olan etki alanları için geçerlidir.

![Federe bir etki alanından diğerine UPN senkronizasyonu yok](media/tshoot-connect-objectsync/objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD Kiracı DirSync Özelliği 'SynchronizeUpnForManagedUsers' devre dışı bırakılır
Azure AD KiracıSı DirSync Özelliği 'SynchronizeUpnForManagedUsers' devre dışı bırakıldığında, Azure Active Directory yönetilen kimlik doğrulaması olan lisanslı kullanıcı hesapları için UserPrincipalName/Alternatif Giriş Kimliği'nde eşitleme güncelleştirmesine izin vermez.

![SynchronizeUpnForManagedUsers](media/tshoot-connect-objectsync/objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Nesne etki alanı filtreleme nedeniyle filtrelenir
### <a name="domain-is-not-configured-to-sync"></a>Etki alanı eşitlemek için yapılandırılmamıştır
Etki alanı nın yapılandırılmamalarından nesne kapsam dışındadır. Aşağıdaki örnekte, nesne eşitleme kapsamı dışında dır, çünkü ait olduğu etki alanı eşitlemeden filtrelenir.

![Etki alanı eşitlemek için yapılandırılmamıştır](media/tshoot-connect-objectsync/objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Etki alanı eşitlemek için yapılandırıldı, ancak çalışma profilleri/çalıştır adımları eksik
Etki alanı çalışma profilleri/çalıştır adımları eksik olduğundan nesne kapsam dışındadır. Aşağıdaki örnekte, ait olduğu etki alanı Tam Alma çalıştır profili için çalışma adımları eksik olduğundan nesne eşitkapsamı dışındadır.
![eksik çalışma profilleri](media/tshoot-connect-objectsync/objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Nesne OU filtreleme nedeniyle filtrelenir
OU filtreleme yapılandırması nedeniyle nesne eşitleme kapsamı dışındadır. Aşağıdaki örnekte, nesne OU=NoSync,DC=bvtadwbackdc,DC=com'a aittir.  Bu OU eşitleme kapsamına dahil edilmez.</br>

![OU](./media/tshoot-connect-objectsync/objsynch7.png)

## <a name="linked-mailbox-issue"></a>Bağlantılı Posta Kutusu sorunu
Bağlı bir posta kutusunun, başka bir güvenilen hesap ormanında bulunan harici bir ana hesapla ilişkilendirilmesi gerekir. Böyle bir dış ana hesap yoksa, Azure AD Connect kullanıcı hesabını eşitlemez, Exchange ormanındaki bağlı posta kutusuna karşılık gelen kullanıcı hesabını Azure AD kiracısıyla karşılar.</br>
![Bağlantılı Posta Kutusu](./media/tshoot-connect-objectsync/objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dinamik Dağıtım Grubu sorunu
Şirket içi Active Directory ve Azure Etkin Dizin arasındaki çeşitli farklar nedeniyle Azure AD Connect dinamik dağıtım gruplarını Azure AD kiracısına eşitlemez.

![Dinamik Dağıtım Grubu](./media/tshoot-connect-objectsync/objsynch13.png)

## <a name="html-report"></a>HTML Raporu
Sorun giderme görevi, nesneyi çözümlemenin yanı sıra nesne hakkında her şeyi bilen bir HTML raporu da oluşturur. Bu HTML raporu, gerekirse daha fazla sorun giderme yapmak için destek ekibiyle paylaşılabilir.

![HTML Raporu](media/tshoot-connect-objectsync/objsynch8.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
