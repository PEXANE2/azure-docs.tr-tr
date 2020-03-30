---
title: Azure AD Koşullu Erişim'de özel denetimler
description: Azure Active Directory Koşullu Erişim'de özel denetimlerin nasıl çalıştığını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8c149279a755eb186a3fdc7891e9b511d18c7f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050543"
---
# <a name="custom-controls-preview"></a>Özel denetimler (önizleme)

Özel denetimler, Azure Etkin Dizini'nin önizleme yeteneğidir. Özel denetimleri kullanırken, kullanıcılarınız Azure Etkin Dizini dışındaki kimlik doğrulama gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirilir. Bu denetimi karşılamak için, bir kullanıcının tarayıcısı dış hizmete yönlendirilir, gerekli kimlik doğrulaması gerçekleştirir ve daha sonra Azure Etkin Dizini'ne geri yönlendirilir. Azure Etkin Dizin yanıtı doğrular ve kullanıcı başarıyla doğrulanmış veya doğrulanmışsa, kullanıcı Koşullu Erişim akışında devam eder.

> [!NOTE]
> Özel Denetim özelliğine yönelik planladığımız değişiklikler hakkında daha fazla bilgi için Şubat 2020'deki [yeni güncelleştirmeye](../fundamentals/whats-new.md#upcoming-changes-to-custom-controls)bakın.

## <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel Denetimler, sınırlı sayıda onaylı kimlik doğrulama sağlayıcısıyla çalışır. Özel bir denetim oluşturmak için öncelikle kullanmak istediğiniz sağlayıcıya başvurmalısınız. Microsoft üyesi olmayan her sağlayıcının kaydolmak, abone olmak veya başka bir şekilde hizmetin bir parçası olmak ve Koşullu Erişim ile tümleştirmek istediğinizi belirtmek için kendi süreci ve gereksinimleri vardır. Bu noktada, sağlayıcı JSON biçiminde bir veri bloğu sağlayacaktır. Bu veriler, sağlayıcının ve Koşullu Erişim'in kiracınız için birlikte çalışmasını sağlar, yeni denetimi oluşturur ve Koşullu Erişim'in kullanıcılarınızın sağlayıcıyla doğrulamayı başarıyla gerçekleştirip gerçekleştirmedığını nasıl anlamasını tanımlar.

JSON verilerini kopyalayın ve ilgili textbox'a yapıştırın. Yaptığınız değişikliği açıkça anlamadığınız sürece JSON'da herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmak sağlayıcı ve Microsoft arasındaki bağlantıyı bozabilir ve sizi ve kullanıcılarınızı hesaplarınızdan kilitleyebilir.

Özel denetim oluşturma seçeneği, **Koşullu Erişim** sayfasının **Yönet** bölümündedir.

![Denetim](./media/controls/82.png)

Yeni **özel denetimi**tıklattığınızda, denetiminizin JSON verileri için bir textbox ile bir bıçak açılır.  

![Denetim](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>Özel denetimleri silme

Özel bir denetimi silmek için öncelikle herhangi bir Koşullu Erişim ilkesinde kullanılmadığından emin olmalısınız. Tamamlandıktan sonra:

1. Özel denetimler listesine gitme
1. Tıklatın...  
1. **Sil**’i seçin.

## <a name="editing-custom-controls"></a>Özel denetimleri düzenleme

Özel denetimi düzenleyebilmek için geçerli denetimi silmeniz ve güncelleştirilmiş bilgilerle yeni bir denetim oluşturmanız gerekir.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Azure Çok Faktörlü Kimlik Doğrulama, Azure AD self servis parola sıfırlama (SSPR) gerektiren kimlik koruması otomasyonu yla, çok faktörlü kimlik doğrulama talebi gereksinimlerini karşılayarak veya Ayrıcalıklı'daki rolleri yükseltmek için özel denetimler kullanılamaz Kimlik Yöneticisi (PIM).

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)

- [Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
