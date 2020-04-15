---
title: Telefon numarası (önizleme) için SMS oturum açma kullanıcı deneyimi - Azure AD
description: Yeni veya mevcut telefon numaraları için SMS oturum açma kullanıcı deneyimi hakkında daha fazla bilgi edinin
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: e5369247fc1e31bafb158e83a54bf8badf721d0d
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81378840"
---
# <a name="use-your-phone-number-as-a-user-name-preview"></a>Telefon numaranızı kullanıcı adı olarak kullanma (önizleme)

Bir aygıtı kaydettirmek, telefonunuzun kuruluşunuzun hizmetlerine erişmesine izin verir ve kuruluşunuzun telefonunuza erişmesine izin vermez. Yöneticiyseniz, Yapılandırma'da daha fazla bilgi bulabilir [ve SMS tabanlı kimlik doğrulaması için kullanıcıları etkinleştirebilirsiniz.](../authentication/howto-authentication-sms-signin.md)

Kuruluşunuz SMS oturum açma'yı kullanıma sunmuyorsa, hesabınıza bir telefon kaydederken bunun için bir seçenek görmezsiniz.  

## <a name="when-you-have-a-new-phone-number"></a>Yeni bir telefon numaranız olduğunda

Yeni bir telefon veya yeni bir numara alır sanız ve sms oturum açabilirsiniz bir kuruluşa kaydederseniz, normal telefon kayıt işlemi yle karşılaşırsınız:

1. **Yöntem Ekle'yi**seçin.
1. **Telefon**seçin.
1. Telefon numarasını girin ve **bana bir kod gönder'i**seçin.
1. Kodu girdikten sonra **İleri'yi**seçin.
1. "SMS doğrulandı" yazan bir istem görürsünüz. Telefonunuz başarıyla kaydedildi."

> [!Important]
> Önizlemede bilinen bir sorun nedeniyle, kısa bir süre için telefon numarası eklemek SMS oturum açma için numarayı kaydetmez. Eklenen numarayla oturum açmanız ve ardından SMS oturum açma için numarayı kaydetmek için istemleri izlemeniz gerekir.

### <a name="when-the-phone-number-is-in-use"></a>Telefon numarası kullanımda olduğunda

Kuruluşunuzdaki başka birinin kullandığı bir telefon numarasını kullanmaya çalışırsanız, aşağıdaki iletiyi görürsünüz:

![Telefon numaranız zaten kullanıldığında hata iletisi](media/sms-sign-in-explainer/sms-sign-in-error.png)

Sorunu düzeltmek için yöneticinize ulaşın.

## <a name="when-you-have-an-existing-number"></a>Varolan bir numaranız varsa

Zaten bir kuruluşla bir telefon numarası kullanıyorsanız ve kullanıcı adı kullanılabilir hale geldikçe telefon numaranızı kullanıyorsanız, aşağıdaki adımlar oturum açmanıza yardımcı olabilir.

1. SMS oturum açma kullanılabilir olduğunda, SMS oturum açma için telefon numarasını etkinleştirmek isteyip istediğinizi soran bir başlık görüntülenir:

    [![](media/sms-sign-in-explainer/sms-sign-in-banner.png "Banner to enable SMS sign-in for a phone number")](media/sms-sign-in-explainer/sms-sign-in-banner.png#lightbox)

1. Ayrıca, telefon yöntemi döşemesindeki bakıcıyı seçerseniz **Etkinleştir** düğmesi görüntülenir:

    [![](media/sms-sign-in-explainer/sms-sign-in-phone-method.png "Banner to enable SMS sign-in for a phone number")](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Yöntemi etkinleştirmek için **Etkinleştir'i**seçin. Eylemi onaylamanız istenir:

    ![Bir telefon numarası için SMS oturum açmayı etkinleştirmek için onay iletişim kutusu](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. **Etkinleştir**’i seçin.

## <a name="when-you-remove-your-phone-number"></a>Telefon numaranızı kaldırdığınızda

1. Telefon numarasını silmek için, SMS oturum açma yöntemi ndeki telefon döşemesindeki sil düğmesini seçin.

    [![](media/sms-sign-in-explainer/sms-sign-in-delete-method.png "Banner to delete SMS sign-in for a phone number")](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Eylemi onaylamak istendiğinde **Tamam'ı**seçin.

Varsayılan oturum açma yöntemi olarak kullanılan bir telefon numarasını kaldıramaz. Numarayı kaldırmak için varsayılan oturum açma yöntemini değiştirmeniz ve ardından telefon numarasını yeniden kaldırmanız gerekir.
