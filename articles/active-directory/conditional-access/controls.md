---
title: Azure AD koşullu erişim 'de özel denetimler
description: Koşullu erişim Azure Active Directory içindeki özel denetimlerin nasıl çalıştığını öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80050543"
---
# <a name="custom-controls-preview"></a>Özel denetimler (Önizleme)

Özel denetimler Azure Active Directory önizleme özelliğidir. Özel denetimler kullanılırken, kullanıcılarınız Azure Active Directory dışında kimlik doğrulama gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirilir. Bu denetimi karşılamak için, bir kullanıcının tarayıcısı dış hizmete yönlendirilir, gerekli kimlik doğrulamasını gerçekleştirir ve ardından Azure Active Directory yeniden yönlendirilir. Azure Active Directory yanıtı doğrular ve kullanıcının kimliği başarıyla doğrulanır veya doğrulandıysa, Kullanıcı koşullu erişim akışında devam eder.

> [!NOTE]
> Özel denetim özelliğiyle ilgili planlama yaptığımız değişiklikler hakkında daha fazla bilgi için, Şubat 2020 ' de [yeni güncelleştirme](../fundamentals/whats-new.md#upcoming-changes-to-custom-controls)bölümüne bakın.

## <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel denetimler sınırlı bir onaylanan kimlik doğrulama sağlayıcıları kümesiyle birlikte kullanılabilir. Özel bir denetim oluşturmak için önce kullanmak istediğiniz sağlayıcıya başvurmanız gerekir. Her Microsoft dışı sağlayıcının, hizmetin bir parçası haline gelmesi, abone olması veya başka bir şekilde hizmet vermek ve koşullu erişimle tümleştirileceğini belirtmek için kendi işlemi ve gereksinimleri vardır. Bu noktada, sağlayıcı size JSON biçiminde bir veri bloğu sağlar. Bu veriler, sağlayıcının ve Koşullu erişimin kiracınız için birlikte çalışmasına izin verir, yeni denetimi oluşturur ve kullanıcılarınızın sağlayıcı ile doğrulamayı başarıyla gerçekleştirdiyse Koşullu erişimin nasıl bildirebileceğini tanımlar.

JSON verilerini kopyalayın ve ilgili metin kutusuna yapıştırın. Yaptığınız değişikliği açıkça anlamadığınız müddetçe JSON üzerinde herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmak, sağlayıcı ile Microsoft arasındaki bağlantıyı bozabilir ve sizin ve kullanıcılarınızı sizin hesaplarınızdan geçirebilir.

Özel denetim oluşturma seçeneği, **koşullu erişim** sayfasının **Yönet** bölümünde bulunur.

![Denetim](./media/controls/82.png)

**Yeni özel denetim**' i tıklattığınızda, denetiminizin JSON verileri için TextBox ile bir dikey pencere açılır.  

![Denetim](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>Özel denetimleri silme

Özel bir denetimi silmek için, önce herhangi bir koşullu erişim ilkesinde kullanılmadığından emin olmalısınız. Tamamlandıktan sonra:

1. Özel denetimler listesine git
1. Tıklayın...  
1. **Sil**’i seçin.

## <a name="editing-custom-controls"></a>Özel denetimleri Düzenle

Özel bir denetimi düzenlemek için geçerli denetimi silmeniz ve güncelleştirilmiş bilgilerle yeni bir denetim oluşturmanız gerekir.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Özel denetimler, Azure Multi-Factor Authentication, Azure AD self servis parola sıfırlama (SSPR), çok faktörlü kimlik doğrulama talep gereksinimlerini karşılayan veya ayrıcalıklı Identity Manager 'daki (PıM) rolleri yükseltmek için kimlik koruması 'nın otomasyonu ile kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)

- [Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
