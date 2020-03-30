---
title: İş cihazınıza kuruluşunuzun ağına katılın - AD
description: İş aygıtınızı kuruluşunuzun ağına nasıl katılacağınızı öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: curtand
ms.reviewer: jairoc
ms.openlocfilehash: 0ff8b85a15d94ded2d702e0df247f9ebc4d3f923
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266318"
---
# <a name="join-your-work-device-to-your-organizations-network"></a>İş cihazınızı kuruluşunuzun ağına katılma
İş için sahip olunan Windows 10 aygıtınızı kuruluşunuzun ağına katlayın, böylece kısıtlı olabilecek kaynaklara erişebilirsiniz.

## <a name="what-happens-when-you-join-your-device"></a>Cihazınıza katıldığınızda ne olur?
Windows 10 aygıtınızı kuruluşunuzun ağına katılırken aşağıdaki eylemler gerçekleşir:

- Windows cihazınızı kuruluşunuzun ağına kaydeder ve kişisel hesabınızı kullanarak kaynaklarınıza erişmenizi sağlar. Cihazınız kaydedildikten sonra, Windows cihazınızı ağa katlar, böylece oturum açma ve kısıtlı kaynaklara erişmek için kuruluşunuzun kullanıcı adını ve parolasını kullanabilirsiniz.

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, [Çok Faktörlü Kimlik Doğrulama](multi-factor-authentication-end-user-first-time.md) veya güvenlik [bilgileri](user-help-security-info-overview.md)aracılığıyla iki aşamalı doğrulama ayarlamanız istenebilir.

- İsteğe bağlı olarak, kuruluşunuzun seçimlerine bağlı olarak, otomatik olarak Microsoft Intune gibi mobil aygıt yönetimine kaydolabilirsiniz. Microsoft Intune'a kaydolma hakkında daha fazla bilgi için [cihazınızı Intune'a kaydet'e](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all)bakın.

- Kuruluş hesabınızla otomatik oturum açma işlemini kullanarak oturum açma işleminden geçersiniz.

## <a name="to-join-a-brand-new-windows-10-device"></a>Yepyeni bir Windows 10 cihazına katılmak için
Aygıtınız yepyeniyse ve henüz ayarlanmadıysa, cihazınızı ağa katılmak için Windows Out of Box Experience (OOBE) işleminden geçebilirsiniz.

1. Yeni cihazınızı başlatın ve OOBE işlemine başlayın.

2. Microsoft **ekranında Oturum** Aç'ta, iş veya okul e-posta adresinizi yazın.

    ![E-posta adresiyle oturum açma ekranı](./media/user-help-join-device-on-network/join-device-oobe-signin.png)

3. **Parolanızı girin** ekranına şifrenizi yazın.

    ![Parola ekranınızı girin](./media/user-help-join-device-on-network/join-device-oobe-password.png)

4. Mobil cihazınızda, hesabınıza erişebilsin diye cihazınızı onaylayın. 

    ![Mobil bildirim ekranı](./media/user-help-join-device-on-network/join-device-oobe-mobile.png)

5. Gizlilik ayarlarınızı ayarlama ve Windows Hello'yu ayarlama (gerekirse) dahil olmak üzere OOBE işlemini tamamlayın.

    Aygıtınız artık kuruluşunuzun ağına katıldı.

## <a name="to-make-sure-youre-joined"></a>Birleştiğinden emin olmak için
Ayarlarınıza bakarak biraraya geldiğinizden emin olabilirsiniz.

1. **Ayarları**açın ve ardından **Hesaplar'ı**seçin.

    ![Ayarlar ekranındaki hesaplar](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. **Access work veya school'u**seçin ve Azure ** * \<AD'>your_organization'e* bağlı olarak (Your_organization'a bağlı**gibi bir şey yazan bir metin gördüğünüzden emin olun.

    ![Bağlı contoso hesabıyla iş veya okul ekranına erişin](./media/user-help-join-device-on-network/join-device-oobe-verify.png)


## <a name="to-join-an-already-configured-windows-10-device"></a>Önceden yapılandırılmış bir Windows 10 aygıtına katılmak için
Cihazınız bir süredir sizdeyse ve cihaz zaten ayarlanmışsa, cihazınızı ağa katılmak için aşağıdaki adımları izleyebilirsiniz.

1. **Ayarları**açın ve ardından **Hesaplar'ı**seçin.

2. **Access çalışması veya okul'u**seçin ve ardından **Bağlan'ı**seçin.

    ![İş veya okula erişin ve bağlantılara bağlanın](./media/user-help-join-device-on-network/join-device-access-work-school-connect.png)

3. İş **veya okul hesabı** ayarla ekranında, **bu aygıtı Azure Etkin Dizini'ne katıl'ı**seçin.

    ![Çalışma veya okul hesabı ekranı ayarlama](./media/user-help-join-device-on-network/join-device-setup-join-aad.png)

4. Ekranda **oturumunuzu imzalayalım,** e-posta adresinizi yazın (örneğin, alain@contoso.com) ve sonra **İleri'yi**seçin.

    ![Ekranda imzanızı atalım.](./media/user-help-join-device-on-network/join-device-setup-get-signed-in.png)

5. Parola **gir** ekranında parolanızı yazın ve oturum **aç'ı**seçin.

    ![Parolayı girin](./media/user-help-join-device-on-network/join-device-setup-password.png)

6. Mobil cihazınızda, hesabınıza erişebilsin diye cihazınızı onaylayın. 

    ![Mobil bildirim ekranı](./media/user-help-join-device-on-network/join-device-setup-mobile.png)

7. Bunun **kuruluş ekranınız olduğundan emin olun,** doğru olduğundan emin olmak için bilgileri gözden geçirin ve ardından **Katıl'ı**seçin.

    ![Bunun kuruluş doğrulama ekranınız olduğundan emin olun](./media/user-help-join-device-on-network/join-device-setup-confirm.png)

8. Tüm **set** ekranında **Bitti'yi**tıklatın.

    ![Hepiniz ayarlanmış ekransınız](./media/user-help-join-device-on-network/join-device-setup-finish.png)

## <a name="to-make-sure-youre-joined"></a>Birleştiğinden emin olmak için
Ayarlarınıza bakarak biraraya geldiğinizden emin olabilirsiniz.

1. **Ayarları**açın ve ardından **Hesaplar'ı**seçin.

    ![Ayarlar ekranındaki hesaplar](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. **Access work veya school'u**seçin ve Azure ** * \<AD'>your_organization'e* bağlı olarak (Your_organization'a bağlı**gibi bir şey yazan bir metin gördüğünüzden emin olun.

    ![Bağlı contoso hesabıyla iş veya okul ekranına erişin](./media/user-help-join-device-on-network/join-device-setup-verify.png)

## <a name="next-steps"></a>Sonraki adımlar
Cihazınızı kuruluşunuzun ağına katıldıktan sonra, iş veya okul hesabı bilgilerinizi kullanarak tüm kaynaklarınıza erişebilmelisiniz.

- Kuruluşunuz telefonunuz gibi kişisel cihazınızı kaydetmenizi istiyorsa, [bkz.](user-help-register-device-on-network.md)

- Kuruluşunuz Microsoft Intune kullanılarak yönetiliyorsa ve kayıt, oturum açma veya Intune ile ilgili diğer konularla ilgili sorularınız varsa, [Intune kullanıcı yardım içeriğine](https://docs.microsoft.com/intune-user-help/use-managed-devices-to-get-work-done)bakın.