---
title: Koşullu Erişim - Erişimi engelle - Azure Etkin Dizini
description: Özel bir Koşullu Erişim ilkesi oluşturun
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb,
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2834fd3d4901b6394eabe000f9efc572c2efd497
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80755089"
---
# <a name="conditional-access-block-access"></a>Koşullu Erişim: Erişimi engelle

Muhafazakar bulut geçişi yaklaşımı olan kuruluşlar için blok tüm ilke kullanılabilir bir seçenektir. 

> [!CAUTION]
> Blok ilkesinin yanlış yapılandırılması, kuruluşların Azure portalının dışında kalmalarına neden olabilir.

Bu gibi politikaların istenmeyen yan etkileri olabilir. Etkinleştirmeden önce doğru test ve doğrulama çok önemlidir. Yöneticiler, değişiklik yaparken [Koşullu Erişim yalnızca rapor modu](concept-conditional-access-report-only.md) ve [Koşullu Erişim'de "Eğer" aracı](what-if-tool.md) gibi araçları kullanmalıdır.

## <a name="user-exclusions"></a>Kullanıcı hariç tutmalar

Koşullu Erişim ilkeleri güçlü araçlardır, aşağıdaki hesapları politikanızdan hariç öneririz:

* Kiracı genelinde hesap kilitlemesini önlemek için **acil durum erişimi** veya kesme **hesabı.** Olası senaryoda tüm yöneticiler kiracınızın dışında kilitli, acil erişim yönetim hesabınız kiracı giriş yapmak için kullanılabilir erişimi kurtarmak için adımlar atın.
   * Daha fazla bilgi makalede bulunabilir, [Azure AD acil erişim hesaplarını yönet](../users-groups-roles/directory-emergency-access.md).
* **Azure** AD Connect Sync Hesabı gibi hizmet hesapları ve **hizmet ilkeleri.** Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Normalde uygulamalara programlı erişim sağlayan arka uç hizmetleri tarafından kullanılır, ancak aynı zamanda yönetim amaçlı sistemlerde oturum açmaiçin kullanılır. MFA programlı olarak tamamlanamadığından, bu gibi hizmet hesapları hariç tutulmalıdır. Hizmet ilkeleri tarafından yapılan aramalar Koşullu Erişim tarafından engellenmez.
   * Kuruluşunuzun bu hesapları komut dosyası veya kodda kullanılıyorsa, [bunları yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi düşünün. Geçici bir geçici çözüm olarak, bu belirli hesapları temel ilkeden hariç tutabilirsiniz.

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Aşağıdaki adımlar, kullanıcılar güvenilir bir ağda değilse [Office 365](concept-conditional-access-cloud-apps.md#office-365-preview) dışındaki tüm uygulamalara erişimi engellemek için Koşullu Erişim ilkeleri oluşturulmasına yardımcı olur. Bu ilkeler, yöneticilerin varolan kullanıcılar üzerindeki etkilerini belirleyebilmeleri için yalnızca [Bildir moduna](howto-conditional-access-report-only.md) alınır. Yöneticiler, ilkelerin istedikleri gibi uygulanmasından memnun olduklarında, bu ilkeleri **Açık'a**geçirebilirsiniz.

İlk ilke, güvenilir bir konumda değilse Office 365 uygulamaları dışındaki tüm uygulamalara erişimi engeller.

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri**altında aşağıdaki seçenekleri seçin:
   1. **Ekle**altında, **Tüm bulut uygulamalarını**seçin.
   1. **Dışla, Dışla'nın**altında **Office 365 'i seçin (önizleme)** seçin, **Seç'i**seçin, ardından **Bitti'yi**seçin.
1. **Koşullar**Altında:
   1. **Koşullar** > **Altında Yer**.
      1. **Yapılandırmayı** **Evet** olarak ayarlama
      1. **Altında Ekle**, Herhangi **bir konum**seçin.
      1. **Dışla,** **tüm güvenilen konumları**seçin.
      1. **Done** (Bitti) öğesini seçin.
   1. **İstemci uygulamaları altında (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti**seçin , sonra **Bitti**.
1. **Access denetimleri** > altında**Hibe**, **Erişimi Engelle'yi**seçin, ardından **Seç'i**seçin.
1. Ayarlarınızı onaylayın ve yalnızca **Rapor'a** **etkinleştir ilkesini** ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

Office 365 kullanıcıları için çok faktörlü kimlik doğrulaması veya uyumlu bir aygıt gerektirmek için aşağıda ikinci bir ilke oluşturulur.

1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
   1. **Altında Ekle**, **Tüm kullanıcıları**seçin.
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, **Select uygulamaları**seçin , Office **365 'i seçin (önizleme)** ve **Seç'i**seçin , sonra Bitti' seçeneğini **belirleyin.**
1. **Access denetimleri** > altında**Hibe**, **Erişim Ver'i**seçin.
   1. **Çok faktörlü kimlik doğrulamayı gerek** ve uyumlu olarak **işaretlenecek aygıtı n için** dene'yi **seçin.**
   1. **Seçili tüm denetimlerin seçildiğini** seve.
   1. **Seç**’i seçin.
1. Ayarlarınızı onaylayın ve yalnızca **Rapor'a** **etkinleştir ilkesini** ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
