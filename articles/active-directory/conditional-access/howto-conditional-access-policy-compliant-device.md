---
title: Koşullu Erişim - Uyumlu aygıtlar gerektirir - Azure Etkin Dizini
description: Uyumlu aygıtlar gerektirecek özel bir Koşullu Erişim ilkesi oluşturun
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc6bd486c1e8338eaf875c7026764c80d49e2f05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295194"
---
# <a name="conditional-access-require-compliant-devices"></a>Koşullu Erişim: Uyumlu aygıtlar gerektirir

Microsoft Intune'u dağıtan kuruluşlar, aygıtlarından döndürülen bilgileri aşağıdakiler gibi uyumluluk gereksinimlerini karşılayan aygıtları tanımlamak için kullanabilir:

* Kilidini açmak için PIN gerektirmesi
* Aygıt şifrelemesi gerektirmesi
* Minimum veya maksimum işletim sistemi sürümü gerektiren
* Bir cihaz gerektiren jailbroken veya köklü değildir

Bu ilke uyumluluk bilgileri, Koşullu Erişim'in kaynaklara erişim izni vermek veya engellemek için kararlar alabileceği Azure AD'ye iletilir. Cihaz uyumluluk ilkeleri hakkında daha fazla bilgi makalede bulunabilir, [Intune kullanarak kuruluşunuzdaki kaynaklara erişime izin vermek için cihazlarda kurallar belirleyin](/intune/protect/device-compliance-get-started)

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Aşağıdaki adımlar, kaynaklara erişen aygıtların kuruluşunuzun Intune uyumluluk ilkeleriyle uyumlu olarak işaretlenebilmek için Koşullu Erişim ilkesi oluşturulmasına yardımcı olur.

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, Tüm **bulut uygulamalarını**seçin.
   1. Belirli uygulamaları politikanızdan çıkarmanız gerekiyorsa, bunları **Dışlanmış bulut uygulamalarını seç'in** altındaki **Dışla** sekmesinden seçebilir ve **Seç'i**seçebilirsiniz.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar** > Altında**İstemci uygulamaları (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti'yi**seçin.
1. **Access denetimleri** > altında**Hibe**, uyumlu olarak işaretlenmiş cihazı **talep edin'i**seçin.
   1. **Seç**’i seçin.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

> [!NOTE]
> Yukarıdaki adımları kullanarak **Tüm kullanıcılar** ve **Tüm bulut uygulamaları** için uyumlu olarak **işaretlenecek cihazı İste'yi** seçseniz bile yeni cihazlarınızı Intune'a kaydedebilirsiniz. **Aygıtın uyumlu denetim olarak işaretlemesi** Intune kaydını engellemez. 

### <a name="known-behavior"></a>Bilinen davranış

Windows 7, iOS, Android, macOS ve bazı üçüncü taraf web tarayıcılarında Azure AD, aygıt Azure AD'ye kaydedildiğinde sağlanan bir istemci sertifikası kullanarak aygıtı tanımlar. Bir kullanıcı tarayıcı üzerinden ilk giriş yaptığında, kullanıcıdan sertifikayı seçmesi istenir. Tarayıcıyı kullanmaya devam edebilmek için son kullanıcının bu sertifikayı seçmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)

[Cihaz uyumluluk ilkeleri Azure AD ile birlikte çalışır](/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
