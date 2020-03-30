---
title: Azure AD Connect eşitleme hizmeti özellikleri ve yapılandırması | Microsoft Dokümanlar
description: Azure AD Connect eşitleme hizmetiiçin hizmet yan özelliklerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5486a8d8bd4c295f49e0ab847daf45d0fcab47ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300545"
---
# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect eşitleme hizmeti özellikleri

Azure AD Connect'in eşitleme özelliği iki bileşenden oluşur:

* Azure AD Connect **eşitleme**adlı şirket içi bileşen , **eşitleme altyapısı**olarak da adlandırılır.
* Azure AD **Connect eşitleme hizmeti** olarak da bilinen Azure AD'de bulunan hizmet

Bu konu, Azure AD **Connect eşitleme hizmetinin** aşağıdaki özelliklerinin nasıl çalıştığını ve Windows PowerShell kullanarak bunları nasıl yapılandırabileceğinizi açıklar.

Bu ayarlar [Windows PowerShell için Azure Active Directory Modülü](https://aka.ms/aadposh)tarafından yapılandırılır. Azure AD Connect'ten ayrı olarak indirin ve yükleyin. Bu konuda belgelenen cmdletler [2016 Mart sürümünde tanıtıldı (inşa 9031.1)](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Cmdlets bu konuda belgelenmiş yoksa veya aynı sonucu üretmek değil, o zaman en son sürümü çalıştırmak emin olun.

Yapılandırmayı Azure REKLAM dizininizde görmek `Get-MsolDirSyncFeatures`için çalıştırın.  
![Get-MsolDirSyncFeatures sonucu](./media/how-to-connect-syncservice-features/getmsoldirsyncfeatures.png)

Bu ayarların çoğu yalnızca Azure AD Connect tarafından değiştirilebilir.

Aşağıdaki ayarlar tarafından `Set-MsolDirSyncFeature`yapılandırılabilir:

| DirsyncFeature | Açıklama |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Birincil SMTP adresine ek olarak nesnelerin userPrincipalName'de katılmasına izin verir. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Eşitleme motorunun yönetilen/lisanslı (federe olmayan) kullanıcılar için userPrincipalName özniteliğini güncelleştirmesine olanak tanır. |

Bir özelliği etkinleştirdikten sonra, yeniden devre dışı tutulamaz.

> [!NOTE]
> 24 Ağustos 2016'dan itibaren *yinelenen öznitelik esnekliği* özelliği, yeni Azure REKLAM dizinleri için varsayılan olarak etkinleştirilir. Bu özellik, bu tarihten önce oluşturulan dizinlerde de kullanıma sunulacak ve etkinleştirilir. Dizininiz bu özelliği etkinleştirmek üzereyken bir e-posta bildirimi alırsınız.
> 
> 

Aşağıdaki ayarlar Azure AD Connect tarafından yapılandırılır `Set-MsolDirSyncFeature`ve aşağıdakiler tarafından değiştirilemez:

| DirsyncFeature | Açıklama |
| --- | --- |
| DeviceWriteback |[Azure AD Bağlantısı: Aygıt geri yazmayı etkinleştirme](how-to-connect-device-writeback.md) |
| DizinUzantıları |[Azure AD Connect eşitleme: Dizin uzantıları](how-to-connect-sync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Dışa aktarma sırasında tüm nesneyi başarısız lığa uğratacak başka bir nesnenin kopyası olduğunda bir özniteliğin karantinaya alınmasına izin verir. |
| Parola Karması Eşitleme |[Azure AD Connect eşitlemeile parola karma senkronizasyonu uygulama](how-to-connect-password-hash-synchronization.md) |
|Doğrudan Kimlik Doğrulama|[Azure Active Directory Geçişli Kimlik Doğrulaması ile kullanıcı oturumu açma](how-to-connect-pta.md)|
| UnifiedGroupWriteback |[Önizleme: Grup yazma](how-to-connect-preview.md#group-writeback) |
| Kullanıcı Yazma |Şu anda desteklenmiyor. |

## <a name="duplicate-attribute-resiliency"></a>Yinelenen öznitelik esnekliği

Yinelenen UPN'ler / proxyAdresler ile nesneleri sağlamabaşarısız yerine, yinelenen öznitelik "karantinaya" ve geçici bir değer atanır. Çakışma çözüldüğünde, geçici UPN otomatik olarak uygun değere değiştirilir. Daha fazla ayrıntı için [Kimlik eşitleme ve yinelenen öznitelik esnekliğine](how-to-connect-syncservice-duplicate-attribute-resiliency.md)bakın.

## <a name="userprincipalname-soft-match"></a>UserPrincipalName yumuşak maç

Bu özellik etkinleştirildiğinde, her zaman etkin olan birincil [SMTP adresine](https://support.microsoft.com/kb/2641663)ek olarak UPN için yumuşak eşleşme etkinleştirilir. Yumuşak eşleştirme, Azure AD'deki varolan bulut kullanıcılarını şirket içi kullanıcılarla eşleştirmek için kullanılır.

Şirket içi REKLAM hesaplarını bulutta oluşturulan varolan hesaplarla eşleştirmeniz gerekiyorsa ve Exchange Online kullanmıyorsanız, bu özellik yararlıdır. Bu senaryoda, genellikle bulutta SMTP özniteliği ayarlamak için bir nedeniniz yok.

Bu özellik, yeni oluşturulan Azure REKLAM dizinleri için varsayılan olarak açıktır. Bu özelliğin sizin için etkinleştirilip etkinleştirilemediğinizi çalıştırarak görebilirsiniz:  

```powershell
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Azure REKLAM dizininiz için bu özellik etkinleştirilemezse, çalıştırarak etkinleştirebilirsiniz:  

```powershell
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Kullanıcıyı senkronize etmePrincipalName güncelleştirmeleri

Geçmişte, bu koşulların her ikisi de doğru olmadığı sürece, şirket içi eşitleme hizmetini kullanarak UserPrincipalName özniteliğindeki güncelleştirmeler engellendi:

* Kullanıcı yönetilir (federe olmayan).
* Kullanıcıya lisans atanmadı.

Daha fazla ayrıntı için [Bkz. Office 365, Azure veya Intune'daki Kullanıcı adları şirket içi UPN veya alternatif giriş kimliğiyle eşleşmiyor.](https://support.microsoft.com/kb/2523192)

Bu özelliği etkinleştirmek, senkronizasyon motorunun şirket içinde değiştirildiğinde userPrincipalName'yi güncelleştirmesine olanak tanır ve parola karma eşitleme veya geçiş kimlik doğrulaması kullanırsınız.

Bu özellik, yeni oluşturulan Azure REKLAM dizinleri için varsayılan olarak açıktır. Bu özelliğin sizin için etkinleştirilip etkinleştirilemediğinizi çalıştırarak görebilirsiniz:  

```powershell
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Azure REKLAM dizininiz için bu özellik etkinleştirilemezse, çalıştırarak etkinleştirebilirsiniz:  

```powershell
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Bu özelliği etkinleştirdikten sonra, varolan kullanıcıPrincipalName değerleri olduğu gibi kalır. UserPrincipalName özniteliğinin şirket içinde bir sonraki değişikliğinde, kullanıcılar üzerindeki normal delta eşitleme UPN'yi günceller.  

## <a name="see-also"></a>Ayrıca bkz.

* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirme.](whatis-hybrid-identity.md)
