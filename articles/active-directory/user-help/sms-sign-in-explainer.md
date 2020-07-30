---
title: Telefon numarası için SMS oturum açma kullanıcı deneyimi (Önizleme)-Azure AD
description: Yeni veya mevcut telefon numaraları için SMS oturum açma kullanıcı deneyimi hakkında daha fazla bilgi edinin
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 864225238c10ab2fda96e95448790201cc8a16ae
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423006"
---
# <a name="use-your-phone-number-as-a-user-name-preview"></a>Telefon numaranızı Kullanıcı adı olarak kullanma (Önizleme)

Bir cihazın kaydedilmesi, kuruluşunuzun hizmetlerine yönelik telefondan erişim sağlar ve kuruluşunuzun telefonunuza erişmesine izin vermez. Yönetici değilseniz, [SMS tabanlı kimlik doğrulaması için kullanıcıları yapılandırma ve etkinleştirme](../authentication/howto-authentication-sms-signin.md)bölümünde daha fazla bilgi bulabilirsiniz.

Kuruluşunuz kullanılabilir SMS oturum açma yapmadıysa, hesabınız ile telefon kaydedilirken bir seçenek görmezsiniz.  

## <a name="when-you-have-a-new-phone-number"></a>Yeni bir telefon numaranız olduğunda

Yeni bir telefon veya yeni numara alırsanız ve bunu SMS oturum açma 'nın kullanılabildiği bir kuruluşa kaydettiğinizde, normal telefon kayıt işlemine karşılaşırsınız:

1. **Add metodunu**seçin.
1. **Telefon**seçin.
1. Telefon numarası girin ve **bana bir kod**seçin.
1. Kodu girdikten sonra **İleri**' yi seçin.
1. "SMS doğrulandı" ifadesini bildiren bir istem görürsünüz. Telefonunuz başarıyla kaydedildi. "

> [!Important]
> Önizlemedeki bilinen bir sorundan dolayı telefon numarası eklenirken kısa bir süre için SMS oturum açma numarası kayıt etmez. Ek numara ile oturum açmanız ve sonra SMS oturum açma için numarayı kaydetmek üzere istemleri izlemeniz gerekir.

### <a name="when-the-phone-number-is-in-use"></a>Telefon numarası kullanımda olduğunda

Kuruluşunuzdaki başka birinin kullandığı bir telefon numarası kullanmayı denerseniz, aşağıdaki iletiyi görürsünüz:

![Telefon numaranız zaten kullanıldığında oluşan hata iletisi](media/sms-sign-in-explainer/sms-sign-in-error.png)

Sorunu gidermek için yöneticinize ulaşın.

## <a name="when-you-have-an-existing-number"></a>Mevcut bir numaranız varsa

Zaten bir kuruluşla telefon numarası kullanıyorsanız ve telefon numaranızı Kullanıcı adı olarak kullanıyorsanız, aşağıdaki adımlar oturum açmanıza yardımcı olabilir.

1. SMS oturum açma kullanılabilir olduğunda, SMS oturum açma için telefon numarasını etkinleştirmek isteyip istemediğinizi soran bir başlık görüntülenir:

    [![Telefon numarası için SMS oturum açma özelliğini etkinleştirmek için başlık.](media/sms-sign-in-explainer/sms-sign-in-banner.png)](media/sms-sign-in-explainer/sms-sign-in-banner.png#lightbox)

1. Ayrıca, telefon yöntemi kutucuğunda giriş işaretini seçerseniz bir **Etkinleştir** düğmesi görünür:

    [![Telefon numarası için SMS oturum açma özelliğini etkinleştirmek için başlık.](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Yöntemi etkinleştirmek için **Etkinleştir**' i seçin. Eylemi onaylamanız istenir:

    ![Telefon numarası için SMS oturum açmayı etkinleştirmek üzere onay iletişim kutusu](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. **Etkinleştir**’i seçin.

## <a name="when-you-remove-your-phone-number"></a>Telefon numaranızı kaldırdığınızda

1. Telefon numarasını silmek için SMS oturum açma telefonu yöntemi kutucuğunda Sil düğmesini seçin.

    [![Telefon numarası için SMS oturum açma silmek için başlık.](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Eylemi onaylamanız istendiğinde **Tamam**' ı seçin.

Varsayılan oturum açma yöntemi olarak kullanımda olan bir telefon numarasını kaldıramazsınız. Sayıyı kaldırmak için, varsayılan oturum açma yöntemini değiştirmeniz ve sonra telefon numarasını yeniden kaldırmanız gerekir.
