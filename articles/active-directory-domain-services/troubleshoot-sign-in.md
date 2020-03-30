---
title: Azure AD Etki Alanı Hizmetlerinde oturum açma sorunları | Microsoft Dokümanlar
description: Azure Active Directory Domain Services'da sık karşılaşılan kullanıcı oturum açma sorunlarını ve hatalarını nasıl gidereceklerini öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: 0585ced3bc53f216ab203b4686b5800b5e14bbbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77612737"
---
# <a name="troubleshoot-account-sign-in-problems-with-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanıyla ilgili hesap oturum açma sorunlarını giderme

Azure AD DS yönetilen bir etki alanında oturum açamayan bir kullanıcı hesabının en yaygın nedenleri şunlardır:

* [Hesap henüz Azure AD DS'de senkronize değil.](#account-isnt-synchronized-into-azure-ad-ds-yet)
* [Azure AD DS'de hesabın oturum açmasına izin vermek için parola açıkları yoktur.](#azure-ad-ds-doesnt-have-the-password-hashes)
* [Hesap kilitlendi.](#the-account-is-locked-out)

> [!TIP]
> Azure AD DS, Azure AD kiracısının dışındaki hesaplar için kimlik bilgilerini eşitedemez. Harici kullanıcılar Azure AD DS yönetilen etki alanında oturum açamıyor.

## <a name="account-isnt-synchronized-into-azure-ad-ds-yet"></a>Hesap henüz Azure AD DS'de senkronize edilemedi

Dizininizin boyutuna bağlı olarak, kullanıcı hesaplarının ve kimlik bilgisi açıklarının Azure AD DS'de kullanılabilir olması biraz zaman alabilir. Büyük dizinler için Azure AD'nin bu tek yönlü eşitlemeleri birkaç saat ve bir veya iki güne kadar sürebilir. Kimlik doğrulaması yeniden denemeden önce yeterince beklemenizi unutmayın.

Azure AD Connect kullanıcının şirket içi dizin verilerini Azure AD'ye senkronize etmek için bağlandığı karma ortamlarda, Azure AD Connect'in en son sürümünü çalıştırdığınızdan ve [Azure AD DS'yi etkinleştirdikten sonra tam bir eşitleme gerçekleştirecek şekilde Azure AD Connect'i yapılandırdığınızdan][azure-ad-connect-phs]emin olun. Azure AD DS'yi devre dışı bırakıp yeniden etkinleştirmeye devam ederseniz, bu adımları yeniden izlemeniz gerekir.

Azure AD Connect aracılığıyla eşitlenmeyen hesaplarla ilgili sorunlar yaşamaya devam ederseniz, Azure AD Eşitleme Hizmetini yeniden başlatın. Azure AD Connect yüklü bilgisayardan bir komut istemi penceresi açın ve aşağıdaki komutları çalıştırın:

```console
net stop 'Microsoft Azure AD Sync'
net start 'Microsoft Azure AD Sync'
```

## <a name="azure-ad-ds-doesnt-have-the-password-hashes"></a>Azure AD DS'de parola varsları yok

Azure AD, kiracınız için Azure AD DS'yi etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gerekli biçimde parola açıkları oluşturmaz veya depolamaz. Azure AD, güvenlik nedeniyle parola kimlik bilgilerini açık metin biçiminde de depolamaz. Bu nedenle, Azure AD bu NTLM veya Kerberos parola hatalarını kullanıcıların varolan kimlik bilgilerini temel alarak otomatik olarak oluşturamaz.

### <a name="hybrid-environments-with-on-premises-synchronization"></a>Şirket içi senkronizasyona sahip karma ortamlar

Şirket içi AD DS ortamından senkronize etmek için Azure AD Connect'i kullanan karma ortamlarda, gerekli NTLM veya Kerberos parola karmalarını Azure AD'ye yerel olarak oluşturabilir ve senkronize edebilirsiniz. Azure AD DS yönetilen etki alanınızı oluşturduktan sonra, [Parola karma eşitlemesini Azure Etkin Dizin Etki Alanı Hizmetleri'ne etkinleştirin.][azure-ad-connect-phs] Bu parola karma eşitleme adımını tamamlamadan, Azure AD DS kullanarak bir hesapta oturum açamazsınız. Azure AD DS'yi devre dışı bırakıp yeniden etkinleştirmeye devam ederseniz, bu adımları yeniden izlemeniz gerekir.

Daha fazla bilgi için, [Azure AD DS için parola karma eşitlemenin nasıl çalıştığını][phs-process]görün.

### <a name="cloud-only-environments-with-no-on-premises-synchronization"></a>Şirket içi senkronizasyon olmayan yalnızca bulut ortamları

Azure AD DS yönetilen etki alanları nın şirket içi eşitlemesi olmadan, yalnızca Azure AD'deki hesapların da gerekli NTLM veya Kerberos parola hatalarını oluşturması gerekir. Yalnızca bulutlara yönelik bir hesap oturum açamazsa, Azure AD DS'yi etkinleştirdikten sonra hesap için parola değiştirme işlemi başarıyla tamamlandı mı?

* **Hayır, parola değiştirilmedi.**
    * Gerekli parola açıklarını oluşturmak için [hesabın parolasını değiştirin][enable-user-accounts] ve yeniden oturum açmayı denemeden önce 15 dakika bekleyin.
    * Azure AD DS'yi devre dışı bırakıp yeniden etkinleştiriyseniz, her hesabın parolalarını değiştirmek ve gerekli parola kalıplarını oluşturmak için adımları yeniden izlemesi gerekir.
* **Evet, parola değiştirildi.**
    * Gibi *SAMAccountName* biçimi yerine *UPN* biçimini `driley@aaddscontoso.com`kullanarak oturum açmayı `AADDSCONTOSO\deeriley`deneyin.
    * *SAMAccountName,* UPN öneki aşırı uzun olan veya yönetilen etki alanında başka bir kullanıcıyla aynı olan kullanıcılar için otomatik olarak oluşturulabilir. *UPN* biçiminin bir Azure AD kiracısı içinde benzersiz olması garanti edilir.

## <a name="the-account-is-locked-out"></a>Hesap kilitlendi

Azure AD DS'deki bir kullanıcı hesabı, başarısız oturum açma denemeleri için tanımlanmış bir eşik karşılandığında kilitlenir. Bu hesap kilitleme davranışı, otomatik bir dijital saldırıyı gösterebilecek tekrarlanan kaba kuvvet oturum açma girişimlerinden sizi korumak için tasarlanmıştır.

Varsayılan olarak, 2 dakika içinde 5 kötü parola denemesi varsa, hesap 30 dakika kilitlenir.

Daha fazla bilgi ve hesap kilitleme sorunlarını nasıl çözeceğiniz için [Azure AD DS'deki Sorun Giderme Hesabı kilitleme sorunlarına][troubleshoot-account-lockout]bakın.

## <a name="next-steps"></a>Sonraki adımlar

VM'inizi Azure AD DS yönetilen etki alanına katılmakta hala sorun yaşıyorsanız, [yardım bulun ve Azure Etkin Dizin için bir destek bileti açın.][azure-ad-support]

<!-- INTERNAL LINKS -->
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[azure-ad-connect-phs]: active-directory-ds-getting-started-password-sync-synced-tenant.md
[enable-user-accounts]:  tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[phs-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
