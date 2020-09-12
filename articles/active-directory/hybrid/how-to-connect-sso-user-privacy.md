---
title: Kullanıcı gizliliği ve Azure AD sorunsuz çoklu oturum açma | Microsoft Docs
description: Bu makale Azure Active Directory (Azure AD) sorunsuz SSO ve GDPR uyumluluğu ile ilgilidir.
services: active-directory
keywords: Azure AD Connect, GDPR, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2aa1a30c548ef60cd9b596031f4115297dd20844
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89278607"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Kullanıcı Gizliliği ve Azure AD Sorunsuz Çoklu Oturum Açma

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış


Azure AD sorunsuz SSO, kişisel veriler içerebilen aşağıdaki günlük türünü oluşturur: 

- İzleme günlüğü dosyalarını Azure AD Connect.

Sorunsuz SSO için Kullanıcı gizliliğini iki şekilde geliştirebilirsiniz:

1.  İstek üzerine, bir kişiye ait verileri ayıklar ve bu kişiden verileri yüklemelerden kaldırın.
2.  Hiçbir veri 48 saatten daha fazla korunmayacağından emin olun.

Uygulamanız ve bakımının daha kolay olması için ikinci seçeneği kesinlikle öneririz. Her günlük türü için aşağıdaki yönergelere bakın:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect izleme günlüğü dosyalarını sil

Bu eylem GDPR tarafından kapsanan verileri oluşturabileceği için, **%ProgramData%\AADConnect** klasörünün içeriğini denetleyin ve bu klasörün izleme günlüğü içeriğini (**Trace- \* . log** Files) Azure AD Connect yükleme veya yükseltme ya da sorunsuz SSO yapılandırmasını değiştirme gibi 48 saat içinde silin.

>[!IMPORTANT]
>Bu dosya, önceki Azure AD Connect yüklemesinin durumunu korumak için kullanıldığı ve bir yükseltme yüklemesi tamamlandığında kullanıldığı için bu klasördeki **PersistedState.xml** dosyasını silmeyin. Bu dosya asla bir kişiyle ilgili hiçbir veri içermez ve hiçbir şekilde silinmemelidir.

Bu izleme günlüğü dosyalarını Windows Gezgini 'ni kullanarak gözden geçirebilir ve silebilirsiniz ya da gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```powershell
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Betiği, "ile bir dosyaya kaydedin. PS1 "uzantısı. Bu betiği gerektiği gibi çalıştırın.

İlgili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi edinmek için [Bu makaleye](reference-connect-user-privacy.md)bakın.

### <a name="note-about-domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri hakkında

Denetim günlüğü etkinse, bu ürün etki alanı denetleyicileriniz için güvenlik günlükleri oluşturabilir. Denetim ilkelerini yapılandırma hakkında daha fazla bilgi edinmek için bu [makaleyi](/previous-versions/tn-archive/dd277403(v=technet.10))okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* [Güven Merkezi 'nde Microsoft gizlilik ilkesini gözden geçirme](https://www.microsoft.com/trustcenter)
  - [**Sorun giderme**](tshoot-connect-sso.md) -özellikle ilgili yaygın sorunları çözmeyi öğrenin.
  - [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) -yeni özellik isteklerini dosyalama.