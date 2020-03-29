---
title: Kullanıcı Gizliliği ve Azure REKLAM Sorunsuz Tek Oturum Açma | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) Sorunsuz SSO ve GDPR uyumluluğu ele al.)
services: active-directory
keywords: Azure AD Connect, GDPR, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenler nedir
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9311c1060b953e87f163cb482db14cdd43f50d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60242098"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Kullanıcı Gizliliği ve Azure AD Sorunsuz Çoklu Oturum Açma

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış


Azure AD Seamless SSO, Kişisel Verileri içerebilen aşağıdaki günlük türünü oluşturur: 

- Azure AD Connect izleme günlüğü dosyaları.

Sorunsuz SSO için kullanıcı gizliliğini iki şekilde geliştirin:

1.  İstek üzerine, bir kişi için veri ayıklayın ve yüklemelerden bu kişiden veri kaldırın.
2.  48 saatten fazla veri tutulmadığından emin olun.

İkinci seçeneği, uygulanması ve bakımı daha kolay olduğu için şiddetle tavsiye ediyoruz. Her günlük türü için aşağıdaki talimatlara bakın:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Bağlantı günlüğü dosyalarını silme

**%ProgramData%\AADConnect** klasörünün içeriğini kontrol edin ve bu eylem GDPR tarafından kapsanan veriler oluşturabileceğinden, Azure AD Connect'i yükledikten veya yükselttikten veya Sorunsuz SSO yapılandırmasını değiştirdikten sonraki 48 saat içinde bu klasörün izleme günlüğü içeriğini **(izleme-\*.log** dosyaları) silin.

>[!IMPORTANT]
>Bu dosya Azure AD Connect'in önceki yüklemedurumunu korumak için kullanıldığından ve yükseltme yüklemesi yapıldığında kullanıldığından, bu klasördeki **PersistedState.xml** dosyasını silmeyin. Bu dosya hiçbir zaman bir kişi hakkında veri içermez ve asla silinmemelidir.

Windows Gezgini'ni kullanarak bu izleme günlüğü dosyalarını gözden geçirip silebilir veya gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz:

```powershell
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Komut dosyasını ". PS1" uzantısı. Bu komut dosyalarını gerektiği gibi çalıştırın.

İlgili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi edinmek için [bu makaleye](reference-connect-user-privacy.md)bakın.

### <a name="note-about-domain-controller-logs"></a>Etki Alanı denetleyici günlükleri hakkında not

Denetim günlüğü etkinse, bu ürün Etki Alanı Denetleyicileriniz için güvenlik günlükleri oluşturabilir. Denetim ilkelerini yapılandırma hakkında daha fazla bilgi edinmek için bu [makaleyi](https://technet.microsoft.com/library/dd277403.aspx)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* [Güven Merkezi'ndeki Microsoft Gizlilik ilkesini inceleyin](https://www.microsoft.com/trustcenter)
  - [**Sorun Giderme**](tshoot-connect-sso.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
  - [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - Yeni özellik istekleri dosyalama için.