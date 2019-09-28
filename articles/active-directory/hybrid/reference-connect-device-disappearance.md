---
title: Azure AD Connect 1.4. xx. x ve cihaz görünümünü anlama | Microsoft Docs
description: Bu belgede, Azure AD Connect 1.4. xx. x sürümüyle ilgili bir sorun açıklanmaktadır
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: b0ef3dd2f39802d07c4ae04ad1eca23e40db502a
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345510"
---
# <a name="understanding-azure-ad-connect-14xxx-and-device-disappearance"></a>Azure AD Connect 1.4. xx. x ve cihaz görünümlerini anlama
Azure AD Connect 1.4. xx. x sürümü sayesinde bazı müşteriler Windows cihazlarının bazı veya tümünün Azure AD 'den kaybolabileceğini görebilirler. Bu cihaz kimlikleri, koşullu erişim yetkilendirmesi sırasında Azure AD tarafından kullanılmadığından bu sorun için bir neden değildir. Bu değişiklik, hibrit Azure AD katılımı için Azure AD 'ye doğru şekilde kayıtlı olan tüm Windows cihazlarını silmez.

Azure AD 'de cihaz nesneleri silme işlemini dışarı aktarma silme eşiğini aştıktan sonra, müşterinin silme işlemlerinin devam yapmasına izin verilmesi önerilir. [Nasıl yapılır: silme eşiğini aştıklarında akışa silmeye izin ver](how-to-connect-sync-feature-prevent-accidental-deletes.md)

## <a name="background"></a>Arka Plan
Karma Azure AD 'ye katılmış olarak kaydedilen Windows cihazları, Azure AD 'de cihaz nesneleri olarak gösterilir. Bu cihaz nesneleri, koşullu erişim için kullanılabilir. Windows 10 cihazları Azure AD Connect aracılığıyla buluta eşitlenir, alt düzey Windows cihazları doğrudan AD FS veya kesintisiz çoklu oturum açma kullanılarak kaydedilir.

## <a name="windows-10-devices"></a>Windows 10 cihazları
Yalnızca karma Azure AD katılımı tarafından yapılandırılan belirli bir userCertificate özniteliği değerine sahip Windows 10 cihazlarının Azure AD Connect tarafından buluta eşitlenmesi gerekir. Azure AD Connect önceki sürümlerinde bu gereksinim, Azure AD 'de gereksiz cihaz nesnelerine neden olacak şekilde, daha dikkatli bir şekilde zorlanmıştı. Azure AD 'deki bu cihazlar, bu cihazların Azure AD 'ye kaydolmayı amaçlandığı için her zaman "beklemede" durumunda olur. 

Azure AD Connect bu sürümü, karma Azure AD 'ye katılmış olarak doğru yapılandırılmış Windows 10 cihazlarını eşitler. Azure AD JOIN 'e özel kullanıcı sertifikası olmadan Windows 10 cihaz nesneleri Azure AD 'den kaldırılacaktır.

## <a name="down-level-windows-devices"></a>Alt düzey Windows cihazları
Azure AD Connect hiçbir şekilde [alt düzey Windows cihazlarını](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices)eşitlemiyor olmalıdır. Azure AD 'de daha önce yanlış eşitlenmiş olan cihazlar artık Azure AD 'den silinecek. Azure AD Connect [alt düzey Windows cihazlarını](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices)silmeye çalışıyorsa, cihaz [Windows 10 olmayan bilgisayarlar için Microsoft WORKPLACE JOIN](https://www.microsoft.com/download/details.aspx?id=53554) tarafından oluşturulan MSI DEĞILDIR ve diğer Azure AD özellikleri tarafından tüketilemiyor.

Bazı müşterilerin nasıl yapılacağını yeniden ziyaret [etmeniz gerekebilir: Karma Azure Active Directory katılma](../devices/hybrid-azuread-join-plan.md) Uygulamanızı planlayın ve bu tür cihazların cihaz tabanlı koşullu erişime tam olarak katılmasını sağlayın. 

## <a name="how-can-i-verify-which-devices-are-deleted-with-this-update"></a>Bu güncelleştirmeyle hangi cihazların silineceğini nasıl doğrulayabilirim?

Hangi cihazların silindiğini doğrulamak için bu PowerShell betiğini kullanabilirsiniz: https://gallery.technet.microsoft.com/scriptcenter/Export-Hybrid-Azure-AD-f8e51436

Bu betik, karma Azure AD JOIN özelliği tarafından verilen sertifikalar, özellikle Active Directory bilgisayar nesnelerinde depolanan sertifikalar hakkında bir rapor oluşturur.
AD 'de bir bilgisayar nesnesinin UserCertificate özelliğinde bulunan sertifikaları denetler ve bu durumda, bir süre sonu olmayan her sertifika için, sertifikanın karma Azure AD JOIN özelliği için verildiğini doğrular (konu adı CN = {Objectguıd} ile eşleşir).
Azure AD Connect, en az bir geçerli sertifika içeren, ancak Azure AD Connect 1,4 ' den başlayarak, eşitleme altyapısı karma Azure AD JOIN sertifikalarını tanımlayabilir ve ' cloudfilter ' olur geçerli bir karma Azure AD katılımı sertifikası olmadıkça, bilgisayar nesnesinin Azure AD 'ye eşitlenmesini sağlar.
Zaten AD ile eşitlenen ancak geçerli bir karma Azure AD JOIN sertifikasına sahip olmayan Azure AD cihazları, eşitleme altyapısı tarafından silinecek (Cloudfıltered = TRUE).

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect sürümü geçmişi](reference-connect-version-history.md)
