---
title: DirSync ve Azure AD Sync'den yükseltme | Microsoft Dokümanlar
description: DirSync ve Azure AD Sync'den Azure AD Connect'e nasıl yükseltilir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 803fcc0161f2a092006e60db5a98f5bf18dce1c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60381187"
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Windows Azure Active Directory Eşitleme’yi ve Azure Active Directory Eşitleme’yi yükseltme
Azure AD Connect, size Azure AD ve Office 365 ile şirket içi dizininize bağlanmak için en iyi yolu sunmaktadır. Windows Azure Active Directory Sync (DirSync) veya Azure AD Sync'den Azure AD Connect'e yükseltme nin tam zamanıdır, çünkü bu araçlar artık amortismana hazır ve 13 Nisan 2017 itibariyle desteklenmez.

Amortismana yenik alan iki kimlik eşitleme aracı, tek orman müşterileri (DirSync) ve çoklu orman ve diğer gelişmiş müşteriler (Azure AD Sync) için sunulmuştur. Bu eski araçlar, tüm senaryolar için kullanılabilen tek bir çözümle değiştirildi: Azure AD Connect. Yeni işlevler, özellik geliştirmeleri ve yeni senaryolar için destek sunar. Şirket içi kimlik verilerinizi Azure AD ve Office 365 ile eşitlemeyi sürdürebilmek için Azure AD Connect'e yükseltmenizi şiddetle öneririz. Microsoft, bu eski sürümlerin 31 Aralık 2017'den sonra çalışmasını garanti etmez.

DirSync'in son sürümü Temmuz 2014'te, Azure AD Sync'in son sürümü ise Mayıs 2015'te yayımlandı.

## <a name="what-is-azure-ad-connect"></a>Azure AD Connect nedir?
Azure AD Connect, DirSync ve Azure AD Sync'in halefidir. Bu iki desteklenen tüm senaryoları birleştirir. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)de bu konuda daha fazla bilgi edinebilirsiniz.

## <a name="deprecation-schedule"></a>Amortisman çizelgesi
| Tarih | Açıklama |
| --- | --- |
| 13 Nisan 2016 |Windows Azure Active Directory Sync ("DirSync") ve Microsoft Azure Active Directory Sync ("Azure AD Sync") azat edilmiş olarak duyurulur. |
| 13 Nisan 2017 |Destek sona erer. Müşteriler artık Azure AD Connect'e yükseltmeden bir destek örneği açamayacaktır. |
|31 Aralık 2017|Azure AD artık Windows Azure Active Directory Sync ("DirSync") ve Microsoft Azure Etkin Dizin Eşitlemi ("Azure AD Eşitlemi") iletişimlerini kabul edemeyecektir.

## <a name="how-to-transition-to-azure-ad-connect"></a>Azure AD Connect'e geçiş nasıl
DirSync çalıştırıyorsanız, yükseltme nizin iki yolu vardır: Yerinde yükseltme ve paralel dağıtım. Çoğu müşteri için yerinde yükseltme önerilir ve yeni bir işletim sisteminiz ve 50.000'den az nesneniz varsa. Diğer durumlarda, DirSync yapılandırmanızın Azure AD Connect çalıştıran yeni bir sunucuya taşındığı paralel bir dağıtım yapmanız önerilir.

| Çözüm | Senaryo |
| --- | --- |
| [DirSync’ten yükseltme](how-to-dirsync-upgrade-get-started.md) |<li>Zaten çalışan varolan bir DirSync sunucunuz varsa.</li> |
| [Azure AD Eşitleme'den yükseltme](how-to-upgrade-previous-version.md) |<li>Azure AD Eşitle'den hareket ediyorsanız.</li> |

DirSync'den Azure AD Connect'e yerinde yükseltme nin nasıl yapılacağını görmek istiyorsanız, şu Kanal 9 videosunu izleyin:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>SSS
**S: Azure Ekibi'nden bir e-posta bildirimi ve/veya Office 365 ileti merkezinden bir ileti aldım, ancak Connect'i kullanıyorum.**  
Bildirim, 1.0 yapı numarasıyla Azure AD Connect kullanan müşterilere de gönderildi. \*.0 (ön-1.1 sürümü kullanarak). Microsoft, müşterilerin Azure AD Connect sürümlerinde güncel kalmalarını önerir. 1.1'de tanıtılan [otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) özelliği, Azure AD Connect'in en yeni sürümünün her zaman yüklenmesini kolaylaştırır.

**S: DirSync/Azure AD Sync 13 Nisan 2017 tarihinde çalışmayı durdurur mu?**  
DirSync/Azure AD Sync 13 Nisan 2017 tarihinde çalışmaya devam edecektir.  Ancak Azure AD, 31 Aralık 2017 tarihinden sonra DirSync/Azure AD Sync'den gelen iletişimleri artık kabul edemeyebilir.

**S: Hangi DirSync sürümlerini yükseltebilirim?**  
Şu anda kullanılmakta olan herhangi bir DirSync sürümünden yükseltmek için desteklenir. 

**S: FIM/MIM için Azure AD Bağlayıcısı ne olacak?**  
FIM/MIM için Azure AD Bağlayıcısı amortismana kalınmış olarak **duyurulmedi.** Bu **özellik dondurma**olduğunu ; yeni bir işlev eklenmez ve hata düzeltmeleri almaz. Microsoft, müşterilerin azure AD Connect'e geçmeyi planlamak için bu sistemi kullanmasını önerir. Bunu kullanarak yeni dağıtımlar başlatmamaönerilir. Bu Bağlayıcı gelecekte amortismana hazır ilan edilecektir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
