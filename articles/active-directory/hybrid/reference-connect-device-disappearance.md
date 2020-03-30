---
title: Azure AD Connect 1.4.xx.x'i anlama ve cihazın kaybolması | Microsoft Dokümanlar
description: Bu belge, Azure AD Connect sürümü 1.4.xx.x ile ortaya çıkan bir sorunu açıklar
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: bc159452c81a673ca4a7ed46aa7eff19fd9209eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73176033"
---
# <a name="understanding-azure-ad-connect-14xxx-and-device-disappearance"></a>Azure AD Connect 1.4.xx.x'i anlama ve aygıt kaybolması
Azure AD Connect sürümü 1.4.xx.x ile bazı müşteriler Windows aygıtlarının bir kısmının veya tamamının Azure AD'den kaybolduğunu görebilir. Bu aygıt kimlikleri Koşullu Erişim yetkilendirmesi sırasında Azure AD tarafından kullanılmadığı için bu endişe verici bir neden değildir. Bu değişiklik, Karma Azure AD Join için Azure AD'ye doğru şekilde kaydedilmiş windows aygıtlarını silmez.

Azure AD'de aygıt nesnelerinin Silme Silme Eşiğini aştığını görürseniz, müşterinin silme işleminin geçmesine izin verdiği önerilir. [Nasıl Yapılsın: silme eşiğini aştıklarında silmelerin akmasına izin verir](how-to-connect-sync-feature-prevent-accidental-deletes.md)

## <a name="background"></a>Arka plan
Karma Azure AD Joined olarak kayıtlı Windows aygıtları Azure AD'de aygıt nesnesi olarak temsil edilir. Bu aygıt nesneleri Koşullu Erişim için kullanılabilir. Windows 10 aygıtları Azure AD Connect üzerinden bulutla senkronize edilir, alt düzey Windows aygıtları doğrudan AD FS veya sorunsuz tek oturum açma kullanılarak kaydedilir.

## <a name="windows-10-devices"></a>Windows 10 cihazları
Yalnızca Karma Azure AD Join tarafından yapılandırılan belirli bir kullanıcı Sertifikası öznitelik değerine sahip Windows 10 cihazlarının Azure AD Connect tarafından bulutla eşitlenilmesi gerekir. Azure AD Connect'in önceki sürümlerinde bu gereksinim sıkı bir şekilde uygulanmadı ve azure AD'de gereksiz aygıt nesnelerine neden oldu. Azure AD'deki bu tür aygıtlar, Azure AD'ye kaydedilmesi amaçlanmadığından her zaman "beklemede" durumunda kalır. 

Azure AD Connect'in bu sürümü, yalnızca Karma Azure AD Joined olarak doğru şekilde yapılandırılan Windows 10 aygıtlarını eşitler. Azure AD'ye katılmadan Windows 10 aygıt nesneleri Belirli userCertificate Azure AD'den kaldırılır.

## <a name="down-level-windows-devices"></a>Alt Düzey Windows aygıtları
Azure AD Connect hiçbir zaman [alt düzey Windows aygıtlarını](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices)eşitlememelidir. Azure AD'de daha önce yanlış senkronize edilmiş tüm aygıtlar artık Azure AD'den silinir. Azure AD Connect [alt düzey Windows aygıtlarını](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices)silmeye çalışıyorsa, aygıt Windows [10 dışındaki bilgisayarlar için Microsoft İşyeri Birleştirme](https://www.microsoft.com/download/details.aspx?id=53554) tarafından oluşturulan aygıt değildir ve başka bir Azure AD özelliği tarafından tüketilemez.

Bazı müşterilerin Windows aygıtlarının doğru şekilde kaydedilmesini sağlamak ve bu tür aygıtların aygıt tabanlı Koşullu Erişim'e tam olarak katılabilmesini sağlamak için [karma Azure Etkin Dizininizi yeniden](../devices/hybrid-azuread-join-plan.md) gözden geçirmeleri gerekebilir. 

## <a name="how-can-i-verify-which-devices-are-deleted-with-this-update"></a>Bu güncelleştirmeyle hangi cihazların silindiği nasıl doğrulanabilir?

Hangi aygıtların silindiğinden doğrular, bu PowerShell komut dosyasını kullanabilirsiniz:https://gallery.technet.microsoft.com/scriptcenter/Export-Hybrid-Azure-AD-f8e51436

Bu komut dosyası, Active Directory Computer nesnelerinde depolanan sertifikalar hakkında bir rapor oluşturur, özellikle Karma Azure AD birleştirme özelliği tarafından verilen sertifikalar.
AD'deki bir Bilgisayarın nesnesinin UserCertificate özelliğinde bulunan sertifikaları denetler ve süresi dolmamış her sertifika için Karma Azure AD birleştirme özelliği için sertifika verilip verilmeseydi (yani Konu Adı CN={ObjectGUID} ile eşleşip eşleşmediği doğrular.
Azure AD Connect, en az bir geçerli sertifika içeren ancak Azure AD Connect sürüm 1.4'ten başlayarak Azure AD Join sertifikalarını tanımlayabilir ve Azure AD birleştirme sertifikalarını 'bulut filtresi' içeren herhangi bir Bilgisayarı Azure AD ile senkronize eder geçerli bir Karma Azure AD birleştirme sertifikası yoksa bilgisayar nesnesi Azure AD'ye eşitlemeden Azure AD'ye eşittir.
Zaten AD ile senkronize edilmiş ancak geçerli bir Karma Azure AD birleştirme sertifikasına sahip olmayan Azure AD Aygıtları eşitleme motoru tarafından silinir (CloudFiltered=TRUE).

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect Sürüm geçmişi](reference-connect-version-history.md)
