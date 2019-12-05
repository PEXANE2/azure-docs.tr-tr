---
title: Koşullu erişim-uyumlu cihazlar gerektir-Azure Active Directory
description: Uyumlu cihazlar gerektirmek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83a4323c2679bdf55709aeaed82134b7b4457fee
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803708"
---
# <a name="conditional-access-require-compliant-devices"></a>Koşullu erişim: uyumlu cihazlar gerektir

Microsoft Intune dağıtılan kuruluşlar, şu gibi uyumluluk gereksinimlerini karşılayan cihazları tanımlamak için cihazlarından döndürülen bilgileri kullanabilir:

* Kilidi açmak için PIN gerektirme
* Cihaz şifrelemesi gerektirme
* Minimum veya maksimum işletim sistemi sürümü gerektirme
* Cihaz gerektirme jailbreak uygulanmış veya kökü belirtilmemiş

Bu ilke uyumluluk bilgileri, Koşullu erişimin kaynaklara erişim izni vermek veya erişimi engellemek için kararlar verebildiği Azure AD 'ye iletilir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, kaynaklara erişen cihazların kuruluşunuzun Intune uyumluluk ilkeleriyle uyumlu olarak işaretlenmesi için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişim** > **Azure Active Directory** > **Güvenliğe** gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemler** altında, **dahil** > , **tüm bulut uygulamaları**' nı seçin.
   1. İlkenizin belirli uygulamaları dışlayamazsınız, **Dışlanan bulut uygulamalarını seçin** altında **hariç tut** sekmesinden seçim yapın ve **Seç**' i seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Erişim denetimleri** > **izin**' nın altında, **cihazın uyumlu olarak işaretlenmesini gerektir**' i seçin.
   1. Seçin **seçin**.
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Cihaz uyumluluk ilkeleri Azure AD ile çalışır](https://docs.microsoft.com/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
