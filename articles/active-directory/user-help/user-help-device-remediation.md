---
title: "\"Buradan oraya ulaşamazsınız\" hatası na yönelik düzeltmeler - Azure AD"
description: "\"Buradan ulaşamazsınız\" hata iletisini neden aldığınıza dair olası düzeltmeleri bulun."
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: curtand
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 2484de4e554d16ba049d206981a44654ede28a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190021"
---
# <a name="possible-fixes-for-the-you-cant-get-there-from-here-error-message"></a>"Buradan oraya ulaşamaz" hata iletisi için olası düzeltmeler

Kuruluşunuzun dahili web uygulamalarına veya hizmetlerine erişirken, buradan **ulaşamayacağınızı**söyleyen bir hata iletisi alabilirsiniz. Bu ileti, kuruluşunuzun cihazınızın kuruluşunuzun kaynaklarına erişmesini engelleyen bir ilke koyduğu anlamına gelir. Bu sorunu gidermek için Yardım Masanızla iletişime geçmek zorunda kalırken, önce deneyebileceğiniz birkaç şey aşağıda verilmiştir.

## <a name="make-sure-youre-using-a-supported-browser"></a>Desteklenen bir tarayıcı kullandığınızdan emin olun
Kuruluşunuzun sitelerine desteklenmeyen bir tarayıcıdan erişmeye çalıştığınızı belirten **you ilebu mesajı buradan alırsanız,** hangi tarayıcıyı çalıştırdığınızı kontrol edin.

![Tarayıcı desteğiyle ilgili hata iletisi](media/user-help-device-remediation/browser-version.png)

Bu sorunu gidermek için, işletim sisteminize dayalı olarak desteklenen bir tarayıcı yüklemeniz ve çalıştırmanız gerekir. Windows 10 kullanıyorsanız, desteklenen tarayıcılar Microsoft Edge, Internet Explorer ve Google Chrome'u içerir. Farklı bir işletim sistemi kullanıyorsanız, [desteklenen tarayıcıların](../conditional-access/concept-conditional-access-conditions.md#supported-browsers)tam listesini kontrol edebilirsiniz.

## <a name="make-sure-youre-using-a-supported-operating-system"></a>Desteklenen bir işletim sistemi kullandığınızdan emin olun
Aşağıdakiler de dahil olmak üzere işletim sisteminin desteklenen bir sürümünü çalıştırdığınızdan emin olun:

- **Windows İstemci.** Windows 7 veya sonrası.

- **Windows Sunucusu.** Windows Server 2008 R2 veya sonrası.

- **Macos.** macOS X veya sonrası

- **Android ve iOS.** Android ve iOS mobil işletim sistemlerinin en son sürümü

Bu sorunu gidermek için desteklenen bir işletim sistemi yüklemeniz ve çalıştırmanız gerekir.

## <a name="make-sure-your-device-is-joined-to-your-network"></a>Cihazınızın ağınıza katıldığından emin olun
Cihazınızın kuruluşunuzun erişim ilkesine uygun olmadığını belirten You mesajını **buradan alırsanız,** cihazınızı kuruluşunuzun ağına katıldığınızdan emin olun.

![Ağınızda olup olmadığınızla ilgili hata iletisi](media/user-help-device-remediation/network-version.png)

### <a name="to-check-whether-your-device-is-joined-to-your-network"></a>Cihazınızın ağınıza katılıp katılmadığını kontrol etmek için
1. İş veya okul hesabınızı kullanarak Windows'da oturum açın. Örneğin, alain@contoso.com.

2. Sanal özel ağ (VPN) veya DirectAccess üzerinden kuruluşunuzun ağına bağlanın.

3. Bağlandıktan sonra cihazınızı kilitlemek için **Windows logo tuşuna+L** tuşuna basın.

4. İş veya okul hesabınızı kullanarak cihazınızın kilidini açın ve sorunlu uygulamaya veya hizmete yeniden erişmeye çalışın.

    Buradan tekrar hata mesajı **alamıyorum** görürseniz, **Daha Fazla ayrıntı** bağlantısını seçin ve ardından ayrıntılarla ilgili Yardım Masanıza başvurun.

### <a name="to-join-your-device-to-your-network"></a>Cihazınızı ağınıza katılmak için
Aygıtınız kuruluşunuzun ağına katılmamışsa, iki şeyden birini yapabilirsiniz:

- **İş cihazınıza katılın.** İş için sahip olunan Windows 10 aygıtınızı kuruluşunuzun ağına katlayın, böylece kısıtlı olabilecek kaynaklara erişebilirsiniz. Daha fazla bilgi ve adım adım talimatlar için [bkz.](user-help-join-device-on-network.md)

- **Kişisel cihazınızı iş için kaydedin.** Kişisel cihazınızı (genellikle bir telefon veya tablet) kuruluşunuzun ağına kaydedin. Aygıtınız kaydedildikten sonra kuruluşunuzun kısıtlı kaynaklarına erişebilir. Daha fazla bilgi ve adım adım talimatlar için [bkz.](user-help-register-device-on-network.md)

## <a name="next-steps"></a>Sonraki adımlar
- [MyApps portalı nedir?](active-directory-saas-access-panel-introduction.md)

- [Parolanızla değil telefonunuzla oturum açma](user-help-auth-app-sign-in.md)
