---
title: Sayfadaki bağlantılar Bir Uygulama Proxy uygulaması için çalışmıyor
description: Azure AD ile tümleştirdiğiniz Application Proxy uygulamalarında bozuk bağlantılarla ilgili sorunları giderme
services: active-directory
author: msmimart
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca782b5a190704911472d70c414d12afa7e558f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430237"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Sayfadaki bağlantılar Bir Uygulama Proxy uygulaması için çalışmıyor

Bu makale, Azure Etkin Dizin Uygulaması Proxy uygulamanızdaki bağlantıların neden düzgün çalışmadığını gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Bir Uygulama Proxy uygulaması yayımlandıktan sonra, uygulamada varsayılan olarak çalışan tek bağlantılar, yayınlanan kök URL'de bulunan hedeflere bağlantılardır. Uygulamalar içindeki bağlantılar çalışmıyor, uygulamanın iç URL'si büyük olasılıkla uygulama içindeki bağlantıların tüm hedeflerini içermiyor.

**Bu neden oluyor?** Uygulamadaki bir bağlantıyı tıklattığınızda, Uygulama Proxy'si URL'yi aynı uygulama içinde dahili URL veya harici olarak kullanılabilen bir URL olarak çözmeye çalışır. Bağlantı, aynı uygulama içinde olmayan bir dahili URL'yi işaret ediyorsa, bu kovalardan birine ait değildir ve bulunamadı bir hataya neden olur.

## <a name="ways-you-can-resolve-broken-links"></a>Bozuk bağlantıları çözmenin yolları

Bu sorunu çözmenin üç yolu vardır. Aşağıdaki seçenekler artan karmaşıklık listelenmiştir.

1.  Dahili URL'nin uygulama yla ilgili tüm bağlantıları içeren bir kök olduğundan emin olun. Bu, tüm bağlantıların aynı uygulama içinde yayınlanan içerik olarak çözülmesini sağlar.

    Dahili URL'yi değiştirir, ancak kullanıcılar için açılış sayfasını değiştirmek istemiyorsanız, Giriş sayfası NıN URL'sini daha önce yayınlanmış dahili URL olarak değiştirin. Bu "Azure Active Directory" - Uygulama&gt; Kayıtları -&gt; uygulama seçin&gt; - Markaing giderek yapılabilir. Marka bölümünde, istediğiniz açılış sayfası olarak ayarlayabildiğiniz "Giriş Sayfası URL'si" alanını görürsünüz. Eski Uygulama kayıtlarını hala kullanıyorsanız, özellikler sekmesinde "Ana Sayfa URL" ayrıntıları gösterilir. 
    
    > [!IMPORTANT]
    > Yukarıdaki değişiklikleri yapmak için Azure AD'deki uygulama nesnelerini değiştirme haklarına ihtiyacınız vardır. Kullanıcıya, Azure AD'deki uygulama değiştirme haklarını kullanıcıya veren [Uygulama Yöneticisi](../users-groups-roles/roles-delegate-app-roles.md#assign-built-in-application-admin-roles) rolü atanması gerekir.
    >

2.  Uygulamalarınız tam nitelikli alan adları (FQDNs) kullanıyorsa, uygulamalarınızı yayınlamak için [özel alan adlarını](application-proxy-configure-custom-domain.md) kullanın. Bu özellik, aynı URL'nin hem dahili hem de harici olarak kullanılmasını sağlar.

    Bu seçenek, uygulama içindeki iç URL'lere bağlantılar da dışarıdan tanındığından, uygulamanızdaki bağlantılara Application Proxy aracılığıyla harici olarak erişilebilir olmasını sağlar. Tüm bağlantıların yine de yayınlanmış bir uygulamaya ait olması gerekir. Ancak, bu seçenekle bağlantıların aynı uygulamaya ait olması gerekmez ve birden çok uygulamaya ait olabilir.

3.  Bu seçeneklerden hiçbiri uygun değilse, satır ara bağlantı çevirisini etkinleştirmek için birden çok seçenek vardır. Bu seçenekler arasında Intune Yönetilen Tarayıcı, Uygulamalarım uzantısı veya uygulamanızdaki bağlantı çeviri ayarını kullanmak yer almaktadır. Bu seçeneklerin her biri ve bunları nasıl etkinleştireceklerini öğrenmek [için Azure AD Application Proxy ile yayınlanan uygulamalar için yeniden yönlendir kodlu bağlantılara](application-proxy-configure-hard-coded-link-translation.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Varolan şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)

