---
title: Azure AD Etki Alanı Hizmetlerinde hesap kilitleme sorunu | Microsoft Dokümanlar
description: Kullanıcı hesaplarının Azure Etkin Dizin Etki Alanı Hizmetlerinde kilitlenemelerine neden olan sık karşılaşılan sorunları nasıl gidereceğinizi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: 2e274aa353f6c3e485ae10a6a67ee2940eb88b08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246331"
---
# <a name="troubleshoot-account-lockout-problems-with-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanıyla hesap kilitleme sorunlarını giderme

Azure AD DS, yinelenen kötü amaçlı oturum açma girişimlerini önlemek için hesapları tanımlı bir eşiğe göre kilitler. Bu hesap kilitleme de bir oturum açma saldırısı olayı olmadan kazara gerçekleşebilir. Örneğin, bir kullanıcı sürekli olarak yanlış parola girerse veya bir hizmet eski bir parola kullanmaya çalışırsa, hesap kilitlenir.

Bu sorun giderme makalesi, hesap kilitlemelerinin neden meydana gelip davranışı nasıl yapılandırabileceğinizi ve kilitlenme olaylarını gidermek için güvenlik denetimlerini nasıl gözden geçireceğinizi özetleyilmiştir.

## <a name="what-is-an-account-lockout"></a>Hesap kilitleme nedir?

Azure AD DS'deki bir kullanıcı hesabı, başarısız oturum açma denemeleri için tanımlanmış bir eşik karşılandığında kilitlenir. Bu hesap kilitleme davranışı, otomatik bir dijital saldırıyı gösterebilecek tekrarlanan kaba kuvvet oturum açma girişimlerinden sizi korumak için tasarlanmıştır.

**Varsayılan olarak, 2 dakika içinde 5 kötü parola denemesi varsa, hesap 30 dakika kilitlenir.**

Varsayılan hesap kilitleme eşikleri, ince taneli parola ilkesi kullanılarak yapılandırılır. Belirli bir gereksinim ler kümesiniz varsa, bu varsayılan hesap kilitleme eşiklerini geçersiz kılabilirsiniz. Ancak, numara hesabı kilitlemelerini azaltmak için eşik sınırlarını artırmanın önerilmez. Önce hesap kilitleme davranışının kaynağını giderin.

### <a name="fine-grained-password-policy"></a>İnce taneli parola ilkesi

Ayrıntılı parola ilkeleri (FGPPs), bir etki alanında farklı kullanıcılara parola ve hesap kilitleme ilkeleri için belirli kısıtlamalar uygulamanıza izin verir. FGPP yalnızca Azure AD DS'de oluşturulan kullanıcıları etkiler. Azure AD'den Azure AD DS yönetilen etki alanına senkronize edilen bulut kullanıcıları ve etki alanı kullanıcıları parola ilkelerinden etkilenmez.

İlkeler, Azure AD DS yönetilen etki alanında grup ilişkilendirme yoluyla dağıtılır ve yaptığınız tüm değişiklikler bir sonraki kullanıcı oturum açma sırasında uygulanır. İlkenin değiştirilmesi, zaten kilitlenmiş bir kullanıcı hesabının kilidini açmaz.

İnce taneli parola ilkeleri hakkında daha fazla bilgi için [parolayı ve hesap kilitleme ilkelerini yapılandır'a][configure-fgpp]bakın.

## <a name="common-account-lockout-reasons"></a>Ortak hesap kilitleme nedenleri

Bir hesabın kötü niyetli veya etken olmadan kilitlenemesinin en yaygın nedenleri aşağıdaki senaryoları içerir:

* **Kullanıcı kendini kilitledi.**
    * Son parola değişikliğinden sonra, kullanıcı önceki bir parolayı kullanmaya devam etti mi? 2 dakika içinde 5 başarısız denemenin varsayılan hesap kilitleme ilkesi, kullanıcının yanlışlıkla eski bir parolayı yeniden denemesinden kaynaklanabilir.
* **Eski bir parolası olan bir uygulama veya hizmet vardır.**
    * Bir hesap uygulamalar veya hizmetler tarafından kullanılıyorsa, bu kaynaklar sürekli olarak eski bir parola kullanarak oturum açmayı deneyebilir. Bu davranış, hesabın kilitlenensine neden olur.
    * Birden çok farklı uygulama veya hizmet arasında hesap kullanımını en aza indirmeyi deneyin ve kimlik bilgilerinin kullanıldığı yeri kaydedin. Hesap parolası değiştirilirse, ilişkili uygulamaları veya hizmetleri buna göre güncelleştirin.
* **Parola farklı bir ortamda değiştirildi ve yeni parola henüz eşitlenmedi.**
    * Bir hesap parolası, ön hazırlık AD DS ortamında olduğu gibi Azure AD DS dışında değiştirilirse, parola değişikliğinin Azure AD üzerinden eşitleme ve Azure AD DS'ye dönüştürülmesi birkaç dakika sürebilir.
    * Parola eşitleme işlemi tamamlanmadan önce Azure AD DS aracılığıyla bir kaynakta oturum açmaya çalışan bir kullanıcı, hesaplarının kilitlenmesine neden olur.

## <a name="troubleshoot-account-lockouts-with-security-audits"></a>Güvenlik denetimleriyle hesap kilitlemelerini giderme

Hesap kilitleme olaylarının ne zaman ve nereden geldiklerini gidermek [için Azure AD DS için güvenlik denetimlerini etkinleştirin.][security-audit-events] Denetim olayları yalnızca özelliği etkinleştirdiğiniz andan itibaren yakalanır. İdeal olarak, sorun giderme niz için bir hesap kilitleme sorunu olmadan *önce* güvenlik denetimlerini etkinleştirmelisiniz. Bir kullanıcı hesabının sürekli olarak kilitleme sorunları varsa, güvenlik denetimlerinin bir sonraki durum oluştuğunda hazır olmasını sağlayabilirsiniz.

Güvenlik denetimlerini etkinleştirdikten sonra, aşağıdaki örnek sorgular *Hesap Kilitleme Olaylarını*, kod *4740'ı*nasıl gözden geçireceğinizi gösterir.

Son yedi gündeki tüm hesap kilitleme olaylarını görüntüleyin:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

*Driley*adlı hesap için son yedi güne ait tüm hesap kilitleme olaylarını görüntüleyin.

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
| where "driley" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

26 Haziran 2019 saat 09:00 arasındaki tüm hesap kilitleme olaylarını görüntüleyebilirsiniz. ve 1 Temmuz 2019 gece yarısı, tarih ve saate göre artan:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2019-06-26 09:00) and TimeGenerated <= datetime(2019-07-01)
| where OperationName has "4740"
| sort by TimeGenerated asc
```

## <a name="next-steps"></a>Sonraki adımlar

Hesap kilitleme eşiklerini ayarlamak için ince taneli parola ilkeleri hakkında daha fazla bilgi için [parolayı ve hesap kilitleme ilkelerini yapılandır'a][configure-fgpp]bakın.

VM'inizi Azure AD DS yönetilen etki alanına katılmakta hala sorun yaşıyorsanız, [yardım bulun ve Azure Etkin Dizin için bir destek bileti açın.][azure-ad-support]

<!-- INTERNAL LINKS -->
[configure-fgpp]: password-policy.md
[security-audit-events]: security-audit-events.md
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
