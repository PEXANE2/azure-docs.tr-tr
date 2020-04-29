---
title: Koşullu erişim-uyumlu cihazlar gerektir-Azure Active Directory
description: Uyumlu cihazlar gerektirmek için özel bir koşullu erişim ilkesi oluşturma
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80295194"
---
# <a name="conditional-access-require-compliant-devices"></a>Koşullu erişim: uyumlu cihazlar gerektir

Microsoft Intune dağıtılan kuruluşlar, şu gibi uyumluluk gereksinimlerini karşılayan cihazları tanımlamak için cihazlarından döndürülen bilgileri kullanabilir:

* Kilidi açmak için PIN gerektirme
* Cihaz şifrelemesi gerektirme
* Minimum veya maksimum işletim sistemi sürümü gerektirme
* Cihaz gerektirme jailbreak uygulanmış veya kökü belirtilmemiş

Bu ilke uyumluluk bilgileri, Koşullu erişimin kaynaklara erişim izni vermek veya erişimi engellemek için kararlar verebildiği Azure AD 'ye iletilir. Cihaz uyumluluk ilkeleri hakkında daha fazla bilgi [için, Intune kullanarak kuruluşunuzdaki kaynaklara erişime izin vermek üzere cihazlarda kuralları ayarlama](/intune/protect/device-compliance-get-started) bölümünde bulabilirsiniz

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, kaynaklara erişen cihazların kuruluşunuzun Intune uyumluluk ilkeleriyle uyumlu olarak işaretlenmesi için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **tüm bulut uygulamaları**' nı seçin.
   1. İlkenizin belirli uygulamaları dışlayamazsınız, **Dışlanan bulut uygulamalarını seçin** altında **hariç tut** sekmesinden seçim yapın ve **Seç**' i seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar** > **istemci uygulamaları (Önizleme)** altında **Yapılandır** ' ı **Evet**olarak ayarlayın ve **bitti**' yi seçin.
1. **Erişim denetimleri** > **izni**altında **cihazın uyumlu olarak işaretlenmesini gerektir**' i seçin.
   1. **Seç**’i seçin.
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

> [!NOTE]
> Yukarıdaki adımları kullanarak cihazın **tüm kullanıcılar** ve **tüm bulut uygulamaları** Için **uyumlu olarak işaretlenmesini gerektir** ' i seçseniz bile yeni cihazlarınızı Intune 'a kaydedebilirsiniz. **Cihazın uyumlu denetim olarak Işaretlenmesini gerektir** Intune kaydını engellemez. 

### <a name="known-behavior"></a>Bilinen davranış

Windows 7, iOS, Android, macOS ve bazı üçüncü taraf web tarayıcılarında, Azure AD, cihaz Azure AD 'ye kaydedildiğinde sağlanan bir istemci sertifikası kullanarak cihazı tanımlar. Kullanıcı tarayıcıda ilk kez oturum açtığında, kullanıcıdan sertifikayı seçmesi istenir. Son Kullanıcı, tarayıcıyı kullanmaya devam edebilmek için bu sertifikayı seçmelidir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Cihaz uyumluluk ilkeleri Azure AD ile birlikte çalışır](/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
