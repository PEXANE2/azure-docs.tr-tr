---
title: Güvenlik bilgileri (önizleme) sayfasından uygulama parolaları oluşturma - Azure AD
description: Kuruluşunuzdaki tarayıcı dışı uygulamalarda veya iki faktörlü doğrulamayı desteklemeyen herhangi bir uygulamayla kullanmak üzere otomatik olarak oluşturulan parolalar (uygulama parolaları) oluşturun. Bu uygulama parolası normal bir paroladan ayrıdır ve Güvenlik bilgileri sayfasından ayarlanabilir.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2018
ms.author: curtand
ms.openlocfilehash: 787fa67ee77997fd1f9967db3abdbfc83d4ffad2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064079"
---
# <a name="create-app-passwords-from-the-security-info-preview-page"></a>Güvenlik bilgileri (önizleme) sayfasından uygulama parolaları oluşturma

Outlook 2010 gibi bazı uygulamalar iki aşamalı doğrulamayı desteklemez. Bu destek eksikliği, kuruluşunuzda iki aşamalı doğrulama kullanıyorsanız uygulamanın çalışmayamayacağı anlamına gelir. Bu sorunu aşmak için, normal parolanızdan ayrı olarak tarayıcı olmayan her uygulamayla kullanmak üzere otomatik olarak oluşturulmuş bir parola oluşturabilirsiniz.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Important]
>Yöneticiniz uygulama parolalarını kullanmanıza izin vermeyebilir. **Uygulama parolalarını** bir seçenek olarak görmüyorsanız, bunlar kuruluşunuzda kullanılamaz.

Uygulama parolalarını kullanırken şunları unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulur ve uygulama başına bir kez oluşturulup girilmelidir.

- Kullanıcı başına 40 parola sınırı vardır. Bu sınırdan sonra bir tane oluşturmaya çalışırsanız, yenisini oluşturmasına izin verilmeden önce varolan bir parolayı silmeniz istenir.

    >[!Note]
    >Office 2013 istemcileri (Outlook dahil) yeni kimlik doğrulama protokollerini destekler ve iki adımlı doğrulamayla kullanılabilir. Bu destek, iki aşamalı doğrulama açık olduktan sonra Office 2013 istemcileri için artık uygulama parolalarına ihtiyacınız olmayacağı anlamına gelir. Daha fazla bilgi için [Office 2013 ve Office 2016 istemci uygulamaları makalesi için modern kimlik doğrulamanın nasıl çalıştığına](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) bakın.

## <a name="create-new-app-passwords"></a>Yeni uygulama parolaları oluşturma

İş veya okul hesabınızla iki aşamalı doğrulama kullanıyorsanız ve yöneticiniz güvenlik bilgileri deneyimini açtıysa, Güvenlik **bilgileri** sayfasını kullanarak uygulama parolalarınızı oluşturabilir ve silebilirsiniz.

>[!Note]
>Yöneticiniz güvenlik bilgileri deneyimini açmadıysa, iki aşamalı doğrulama bölümü [için uygulama parolalarını yönet'teki](multi-factor-authentication-end-user-app-passwords.md) yönergeleri ve bilgileri izlemeniz gerekir.

### <a name="to-create-a-new-app-password"></a>Yeni bir uygulama parolası oluşturmak için

1. İş veya okul hesabınızda oturum açın https://myprofile.microsoft.com/ ve sayfanıza gidin.

    ![Vurgulanan Güvenlik bilgi bağlantılarını gösteren Profil sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Sol gezinti bölmesinden veya **Güvenlik bilgileri** bloğundaki bağlantıdan **Güvenlik bilgilerini** seçin ve ardından Güvenlik **bilgileri** sayfasından **ekle yöntemini** seçin.

    ![Vurgulanan Ekle yöntemi seçeneği ile güvenlik bilgileri sayfası](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Yöntem **ekle** sayfasında açılan listeden **Uygulama parolasını** seçin ve sonra **Ekle'yi**seçin.

    ![Uygulama parolası seçili yöntem kutusu ekleme](media/security-info/securityinfo-myprofile-addpassword.png)

4. Uygulama parolasını gerektiren uygulamanın adını yazın ve **sonra İleri'yi**seçin.

    ![Uygulama adı ile uygulama şifresi sayfası](media/security-info/securityinfo-myprofile-password-appname.png)

5. **Parola** kutusundan metni kopyalayın, parolayı uygulamanın parola alanına yapıştırın (bu örnekte, Outlook 2010) ve **ardından Bitti'yi**seçin.

    ![Uygulama adı ile uygulama şifresi sayfası](media/security-info/securityinfo-myprofile-password-copytext.png)

    Parola eklenir ve ileriye doğru uygulamanızda başarılı bir şekilde oturum açabilirsiniz.

## <a name="delete-your-app-passwords"></a>Uygulama parolalarınızı silme

Artık uygulama parolası gerektiren bir uygulama kullanmanız gerekmiyorsa, ilişkili uygulama parolasını silebilirsiniz. Uygulama parolasını siler, gelecekte kullanılmak üzere kullanılabilir uygulama parola noktalarından birini boşaltTır.

>[!Important]
>Bir uygulama parolasını yanlışlıkla silerseniz, bunu geri almanın bir yolu yoktur. Bu makalenin [yeni uygulama parolaları](#create-new-app-passwords) oluştur bölümündeki adımları izleyerek yeni bir uygulama parolası oluşturmanız ve uygulamaya yeniden girmeniz gerekir.

### <a name="to-delete-an-app-password"></a>Uygulama parolasını silmek için

1. Güvenlik **bilgileri** sayfasında, belirli bir uygulama için **Uygulama parolası** seçeneğinin yanındaki **Sil** bağlantısını seçin.

    ![Güvenlik bilgisinden uygulama şifresi yöntemini silmek için bağlantı](media/security-info/securityinfo-myprofile-password-appdelete.png)

2. **Uygulama parolasını**silmek için onay kutusundan **Evet'i** seçin. Uygulama parolası silindikten sonra güvenlik bilgilerinizden kaldırılır ve Güvenlik **bilgileri** sayfasından kaybolur.

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için

- **Güvenlik bilgileri** sayfası ve nasıl ayarlayınız hakkında daha fazla bilgi için [Güvenlik bilgilerine genel bakış](user-help-security-info-overview.md)
