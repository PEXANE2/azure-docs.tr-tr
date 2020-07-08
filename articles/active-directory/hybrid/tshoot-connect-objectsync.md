---
title: 'Azure AD Connect: nesne eşitleme sorunlarını giderme | Microsoft Docs'
description: Bu konuda, sorun giderme görevini kullanarak nesne eşitlemeyle ilgili sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73d4239dd34f2a64aa7b3edbf88bad4348e01291
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85356211"
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitleme ile nesne eşitleme sorunlarını giderme
Bu makalede, sorun giderme görevi kullanılarak nesne eşitlemeyle ilgili sorunları gidermeye yönelik adımlar sağlanmaktadır. Sorun gidermeyi Azure Active Directory (Azure AD) Connect 'te nasıl çalıştığını görmek için [Bu kısa videoyu](https://aka.ms/AADCTSVideo)izleyin.

## <a name="troubleshooting-task"></a>Sorun giderme görevi
Azure AD Connect dağıtımınız sürüm 1.1.749.0 veya üzeri ise nesne eşitleme sorunlarını gidermek için sihirbazdaki sorun giderme görevini kullanın. Önceki sürümler için, [burada](tshoot-connect-object-not-syncing.md)açıklandığı gibi lütfen el ile sorun giderin.

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Sihirbazdaki sorun giderme görevini çalıştırma
Sihirbazda sorun giderme görevini çalıştırmak için aşağıdaki adımları gerçekleştirin:

1.  Yönetici olarak çalıştır seçeneğiyle Azure AD Connect sunucunuzda yeni bir Windows PowerShell oturumu açın.
2.  `Set-ExecutionPolicy RemoteSigned`Veya çalıştırın `Set-ExecutionPolicy Unrestricted` .
3.  Azure AD Connect Sihirbazı 'nı başlatın.
4.  Ek görevler sayfasına gidin, sorun gider ' i seçin ve Ileri ' ye tıklayın.
5.  Sorun giderme sayfasında, PowerShell 'de sorun giderme menüsünü başlatmak için Başlat ' a tıklayın.
6.  Ana menüde, nesne eşitlemesini sorun gider ' i seçin.
![Nesne eşitleme sorunlarını giderme](media/tshoot-connect-objectsync/objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Giriş parametrelerini sorun giderme
Aşağıdaki giriş parametreleri sorun giderme görevi için gereklidir:
1.  **Nesne ayırt edici adı** – bu, sorun giderme gereken nesnenin ayırt edici adıdır
2.  **Ad Bağlayıcısı adı** – yukarıdaki NESNENIN bulunduğu ad ormanının adıdır.
3.  Azure AD kiracısı genel yönetici kimlik bilgileri ![ genel yönetici kimlik bilgileri](media/tshoot-connect-objectsync/objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevinin sonuçlarını anlayın
Sorun giderme görevi aşağıdaki denetimleri gerçekleştirir:

1.  Nesne Azure Active Directory eşitlediği takdirde UPN uyuşmazlığını Algıla
2.  Etki alanı filtreleme nedeniyle nesnenin filtrelenerek filtrelendiğine bakın
3.  Nesne, OU filtrelemesi nedeniyle filtrelenerek filtrelendiğine bakın
4.  Bağlı bir posta kutusu nedeniyle nesne eşitlemesinin engellenip engellenmediğini denetle
5. Nesnenin eşitlenmesi beklenen dinamik dağıtım grubu olup olmadığını denetleyin

Bu bölümün geri kalanında, görev tarafından döndürülen belirli sonuçlar açıklanmaktadır. Her durumda, görev bir analiz sağlar ve ardından sorunu çözmek için önerilen eylemleri izler.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Nesne Azure Active Directory eşitlendiğinde UPN uyuşmazlığını Algıla
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>UPN soneki Azure AD kiracısı ile doğrulanmadı
UserPrincipalName (UPN)/alternatif oturum açma KIMLIĞI soneki Azure AD kiracısı ile doğrulanmadığı zaman Azure Active Directory, UPN soneklerini varsayılan etki alanı adı "onmicrosoft.com" ile değiştirir.

![Azure AD UPN 'yi değiştirir](media/tshoot-connect-objectsync/objsynch2.png)

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD kiracısı DirSync özelliği ' SynchronizeUpnForManagedUsers ' devre dışı
Azure AD kiracısı DirSync özelliği ' SynchronizeUpnForManagedUsers ' devre dışı bırakıldığında, Azure Active Directory yönetilen kimlik doğrulaması olan Lisanslı Kullanıcı hesapları için UserPrincipalName/alternatif oturum açma KIMLIĞI ile eşitleme güncelleştirmelerine izin vermez.

![SynchronizeUpnForManagedUsers](media/tshoot-connect-objectsync/objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Etki alanı filtreleme nedeniyle nesne filtrelendi
### <a name="domain-is-not-configured-to-sync"></a>Etki alanı eşitlenecek şekilde yapılandırılmadı
Etki alanı yapılandırılmadığı için nesne kapsam dışında. Aşağıdaki örnekte, ait olduğu etki alanı eşitlemeden filtrelendiğinde, nesne eşitleme kapsamı dışında olur.

![Etki alanı eşitlenecek şekilde yapılandırılmadı](media/tshoot-connect-objectsync/objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Etki alanı eşitlenecek şekilde yapılandırılmış ancak eksik çalıştırma profilleri/çalıştırma adımları
Etki alanında eksik Çalıştır profilleri/çalıştırma adımları olduğundan nesne kapsam dışında. Aşağıdaki örnekte, ait olduğu etki alanı, tam Içeri aktarma çalıştırma profili için çalıştırma adımları eksik olduğundan, nesne eşitleme kapsamı dışında.
![eksik çalıştırma profilleri](media/tshoot-connect-objectsync/objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Nesne, OU filtrelemesi nedeniyle filtrelendi
OU filtreleme yapılandırması nedeniyle nesne, eşitleme kapsamı dışında. Aşağıdaki örnekte, nesnesi OU = NoSync, DC = bvtadwbackdc, DC = com ' a aittir.  Bu OU, eşitleme kapsamına dahil değildir.</br>

![OU](./media/tshoot-connect-objectsync/objsynch7.png)

## <a name="linked-mailbox-issue"></a>Bağlı posta kutusu sorunu
Bağlı bir posta kutusunun, başka bir güvenilen hesap ormanında bulunan bir dış ana hesap ile ilişkilendirilmesi gerekir. Böyle bir dış ana hesap yoksa, Azure AD Connect Kullanıcı hesabı, Exchange ormanındaki bağlantılı posta kutusuna Azure AD kiracısına karşılık gelir.</br>
![Bağlı posta kutusu](./media/tshoot-connect-objectsync/objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Dinamik dağıtım grubu sorunu
Şirket içi Active Directory ve Azure Active Directory arasındaki çeşitli farklılıklar nedeniyle Azure AD Connect dinamik dağıtım gruplarını Azure AD kiracısıyla eşitlemez.

![Dinamik dağıtım grubu](./media/tshoot-connect-objectsync/objsynch13.png)

## <a name="html-report"></a>HTML raporu
Nesneyi çözümlemenin yanı sıra, sorun giderme görevi, nesne hakkında bilinen her şeyi içeren bir HTML raporu da oluşturur. Bu HTML raporu, gerekirse daha fazla sorun giderme yapmak için destek ekibi ile paylaşılabilir.

![HTML raporu](media/tshoot-connect-objectsync/objsynch8.png)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
