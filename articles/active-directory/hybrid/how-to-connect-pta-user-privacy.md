---
title: Kullanıcı Gizliliği ve Azure Active Directory Geçiş Kimlik Doğrulaması | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması ve GDPR uyumluluğu ele alınıyor.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, GDPR, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0af1c42e7e2c163e7f9e7407d0236e35bfacf8e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76931003"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Kullanıcı Gizliliği ve Azure Active Directory Doğrudan Kimlik Doğrulaması


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış

Azure AD Pass-through Kimlik Doğrulaması, Kişisel Verileri içerebilen aşağıdaki günlük türünü oluşturur:

- Azure AD Connect izleme günlüğü dosyaları.
- Kimlik Doğrulama Aracısı izleme günlüğü dosyaları.
- Windows Event günlük dosyaları.

Geçiş Kimlik Doğrulaması için kullanıcı gizliliğini iki şekilde geliştirin:

1.  İstek üzerine, bir kişi için veri ayıklayın ve yüklemelerden bu kişiden veri kaldırın.
2.  48 saatten fazla veri tutulmadığından emin olun.

İkinci seçeneği, uygulanması ve bakımı daha kolay olduğu için şiddetle tavsiye ediyoruz. Aşağıda her günlük türü için yönergeler ver:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Bağlantı günlüğü dosyalarını silme

**%ProgramData%\AADConnect** klasörünün içeriğini kontrol edin ve bu eylem GDPR tarafından kapsanan veriler oluşturabileceğinden, Azure AD Connect'i yükledikten veya yükselttikten veya Geçiş Kimlik Doğrulama yapılandırmasını değiştirdikten sonraki 48 saat içinde bu klasörün izleme günlük içeriğini **(izleme-\*.log** dosyaları) silin.

>[!IMPORTANT]
>Bu dosya Azure AD Connect'in önceki yüklemedurumunu korumak için kullanıldığından ve yükseltme yüklemesi yapıldığında kullanıldığından, bu klasördeki **PersistedState.xml** dosyasını silmeyin. Bu dosya hiçbir zaman bir kişi hakkında veri içermez ve asla silinmemelidir.

Windows Gezgini'ni kullanarak bu izleme günlüğü dosyalarını gözden geçirip silebilir veya gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Komut dosyasını ". PS1" uzantısı. Bu komut dosyalarını gerektiği gibi çalıştırın.

İlgili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi edinmek için [bu makaleye](reference-connect-user-privacy.md)bakın.

### <a name="delete-authentication-agent-event-logs"></a>Kimlik Doğrulama Aracısı olay günlüklerini silme

Bu ürün ayrıca **Windows Olay Günlükleri**oluşturabilir. Daha fazla bilgi için lütfen [bu makaleyi](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx)okuyun.

Geçiş Kimlik Doğrulama Aracısı ile ilgili günlükleri görüntülemek için sunucuda **Olay Görüntüleyicisi** uygulamasını açın ve **Uygulama ve Hizmet Günlükleri\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**altında kontrol edin.

### <a name="delete-authentication-agent-trace-log-files"></a>Kimlik Doğrulama Aracısı izleme günlüğü dosyalarını silme

**%ProgramData%\Microsoft\Azure AD Connect Authentication Agent\Trace'in** içeriğini düzenli olarak kontrol etmeli ve her 48 saatte bir bu klasörün içeriğini silmelisiniz. 

>[!IMPORTANT]
>Kimlik Doğrulama Aracısı hizmeti çalışıyorsa, klasördeki geçerli günlük dosyasını silemezsiniz. Yeniden denemeden önce hizmeti durdurun. Kullanıcı oturum açma hatalarını önlemek için, [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)için Geçiş Kimlik Doğrulaması'nı zaten yapılandırmış olmalısınız.

Windows Gezgini'ni kullanarak bu dosyaları gözden geçirip silebilirsiniz veya gerekli eylemleri gerçekleştirmek için aşağıdaki komut dosyasını kullanabilirsiniz:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Bu komut dosyasını her 48 saatte bir çalışacak şekilde zamanlamak için aşağıdaki adımları izleyin:

1.  Komut dosyasını ". PS1" uzantısı.
2.  **Kontrol Paneli'ni** açın ve **Sistem ve Güvenlik'e**tıklayın.
3.  Yönetim **Araçları** başlığı altında "**Görevleri Zamanla**" seçeneğini tıklayın.
4.  **Görev Zamanlayıcısı'nda**"**Görev Çizelgesi Kitaplığı**" na sağ tıklayın ve " Temel görev**oluştur...**" "a tıklayın.
5.  Yeni görevin adını girin ve **İleri'yi**tıklatın.
6.  **Görev Tetikleyicisi** için "**Günlük**" seçeneğini belirleyin ve **İleri'yi**tıklatın.
7.  Yinelemeyi iki güne ayarlayın ve **İleri'yi**tıklatın.
8.  Eylem olarak " Programı**Başlat**" seçeneğini belirleyin ve **İleri'yi**tıklatın.
9.  Program/komut dosyası için kutuya "**PowerShell**" yazın ve kutuda "**Bağımsız değişken ekle (isteğe bağlı)**" etiketli, daha önce oluşturduğunuz komut dosyasına tam yolu girin, ardından **İleri'yi**tıklatın.
10. Sonraki ekranda oluşturmak üzere olduğunuz görevin bir özetini gösterir. Değerleri doğrulayın ve görevi oluşturmak için **Bitir'i** tıklatın:
 
### <a name="note-about-domain-controller-logs"></a>Etki Alanı denetleyici günlükleri hakkında not

Denetim günlüğü etkinse, bu ürün Etki Alanı Denetleyicileriniz için güvenlik günlükleri oluşturabilir. Denetim ilkelerini yapılandırma hakkında daha fazla bilgi edinmek için bu [makaleyi](https://technet.microsoft.com/library/dd277403.aspx)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi'ndeki Microsoft Gizlilik ilkesini inceleyin](https://www.microsoft.com/trustcenter)
* [**Sorun Giderme**](tshoot-connect-pass-through-authentication.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
