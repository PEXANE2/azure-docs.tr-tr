---
title: Device Manager'da StorSimple 8000 için Azure AD kimlik doğrulaması
description: Hizmetiniz için AAD tabanlı kimlik doğrulamasını nasıl kullanacağınızı, yeni kayıt anahtarı oluşturmayı ve cihazların el ile kaydını nasıl gerçekleştiracağınızı açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: b16132c24d35ee2c9902fa2b21c44416d8376b4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77470913"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>StorSimple cihazınız için yeni kimlik doğrulamasını kullanın

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple aygıtına bağlanır. Bugüne kadar, StorSimple Aygıt Yöneticisi hizmeti, hizmeti StorSimple cihazınıza doğrulamak için bir Erişim Denetimi hizmeti (ACS) kullanmıştır. ACS mekanizması yakında amortismana hazır olacak ve yerine Azure Etkin Dizin (AAD) kimlik doğrulaması yer alacak. Daha fazla bilgi için, ACS amortismanı ve AAD kimlik doğrulaması kullanımı için aşağıdaki duyurulara gidin.

- [Azure ACS'nin geleceği Azure Active Directory](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Microsoft Erişim Denetim Hizmeti'nde yapılacak değişiklikler](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/)

Bu makalede, AAD kimlik doğrulamasının ayrıntıları ve ilgili yeni hizmet kaydı anahtarı ve Güvenlik Duvarı kurallarında storSimple aygıtları için geçerli olan değişiklikler açıklanmaktadır. Bu makalede yer alan bilgiler yalnızca StorSimple 8000 serisi aygıtlar için geçerlidir.

AAD kimlik doğrulaması, Update 5 veya daha sonra çalıştıran StorSimple 8000 serisi aygıtta oluşur. AAD kimlik doğrulaması nın başlatılması nedeniyle, aşağıdakilerde değişiklikler meydana gelir:

- Güvenlik duvarı kuralları için URL desenleri.
- Servis kayıt anahtarı.

Bu değişiklikler aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmetin AAD tabanlı kimlik doğrulaması kullandığından emin olmak için, tüm kullanıcıların güvenlik duvarı kurallarına yeni kimlik doğrulama URL'lerini eklemeleri gerekir.

StorSimple 8000 serisini kullanıyorsanız, aşağıdaki URL'nin güvenlik duvarı kurallarına dahil olduğundan emin olun:

| URL deseni                         | Bulut | Bileşen/İşlevsellik         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Azure Genel |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Kamu |AAD kimlik doğrulama hizmeti      |

StorSimple 8000 serisi aygıtlar için URL desenlerinin tam listesi [için, güvenlik duvarı kuralları için URL desenlerine](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules)gidin.

Kimlik doğrulama URL'si, amortisman tarihinden sonra güvenlik duvarı kurallarına dahil edilmediyse ve aygıt Güncelleştirme 5 çalıştırıyorsa, kullanıcılar bir URL uyarısı görür. Kullanıcıların yeni kimlik doğrulama URL'sini eklemesi gerekir. Aygıt Güncelleştirme 5'ten önce bir sürüm çalıştırıyorsa, kullanıcılar bir sinyal uyarısı görür. Her durumda, StorSimple aygıtı hizmetle kimlik doğrulaması yapamaz ve hizmet aygıtla iletişim kuramaz.

## <a name="device-version-and-authentication-changes"></a>Aygıt sürümü ve kimlik doğrulama değişiklikleri

StorSimple 8000 serisi bir aygıt kullanıyorsanız, çalıştırdığınız aygıt yazılım sürümüne bağlı olarak hangi eylemi yapmanız gerektiğini belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınız çalışıyorsa| Aşağıdaki eylemi                                    |
|--------------------------|------------------------|
| 5 veya daha sonra güncelleştirin ve aygıt çevrimdışı. <br> URL'nin beyaz listeye alınmadığı konusunda bir uyarı görürsünüz.|1. Güvenlik duvarı kurallarını kimlik doğrulama URL'sini içerecek şekilde değiştirin. Kimlik [doğrulama URL'lerine](#url-changes-for-aad-authentication)bakın.<br>2. [AAD kayıt anahtarını servisten alın.](#aad-based-registration-keys)<br>3. [StorSimple 8000 serisi cihazın Windows PowerShell arayüzüne bağlanın.](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)<br>4. `Redo-DeviceRegistration` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet kullanın. Önceki adımda aldığınız anahtarı tedarik edin.|
| 5 veya daha sonra ve aygıtı çevrimiçi güncelleştirin.| İşlem yapmanız gerekmez.                                       |
| 4 veya daha önce güncelleştirin ve aygıt çevrimdışı. |1. Güvenlik duvarı kurallarını kimlik doğrulama URL'sini içerecek şekilde değiştirin.<br>2. [Katalog sunucusu üzerinden Update 5'i indirin.](storsimple-8000-install-update-5.md#download-updates-for-your-device)<br>3. [Düzeltme yöntemi ile Güncelleştirme 5'i uygulayın.](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix)<br>4. [AAD kayıt anahtarını servisten alın.](#aad-based-registration-keys)<br>5. [StorSimple 8000 serisi cihazın Windows PowerShell arayüzüne bağlanın.](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) <br>6. `Redo-DeviceRegistration` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet kullanın. Önceki adımda aldığınız anahtarı tedarik edin.|
| 4 veya daha erken güncelleştirin ve aygıt çevrimiçi. |Güvenlik duvarı kurallarını kimlik doğrulama URL'sini içerecek şekilde değiştirin.<br> Azure portalı üzerinden Güncelleştirme 5'i yükleyin.              |
| Fabrika Güncelleştirme 5'ten önce bir sürüme sıfırla.      |Portal, aygıt eski yazılımları çalıştırırken AAD tabanlı bir kayıt anahtarı gösterir. Aygıtın 4 veya daha önce güncelleştir'i çalıştırırken önceki senaryodaki adımları izleyin.              |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

StorSimple 8000 serisi cihazlar için Güncelleme 5'ten başlayarak, Yeni AAD tabanlı kayıt anahtarları kullanılır. StorSimple Device Manager hizmetinizi cihaza kaydetmek için kayıt anahtarlarını kullanırsınız.

Yeni AAD hizmet kayıt anahtarlarını, Update 4 veya daha önce çalıştıran bir StorSimple 8000 serisi aygıtı kullanıyorsanız (şimdi etkinleştirilmekte olan eski bir aygıtı içerir) kullanamazsınız.
Bu senaryoda, hizmet kayıt anahtarını yeniden oluşturmanız gerekir. Anahtarı yeniden oluşturduktan sonra, yeni anahtar sonraki tüm aygıtları kaydetmek için kullanılır. Eski anahtar artık geçerli değil.

- Yeni AAD kayıt anahtarı 3 gün sonra sona erer.
- AAD kayıt anahtarları yalnızca Update 5 veya daha sonra çalışan StorSimple 8000 serisi aygıtlarla çalışır.
- AAD kayıt anahtarları ilgili ACS kayıt anahtarlarından daha uzundur.

AAD hizmet kayıt anahtarı oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. **StorSimple Aygıt**Yöneticisi'nde ** &gt; Yönetim** **Tuşları'na**gidin. _Anahtarları_aramak için arama çubuğunu da kullanabilirsiniz.
    
2. **Oluştur tuşunu**tıklatın.

    ![Yeniden oluşturma'yı tıklatın](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışmaz.

    ![Yeniden oluşturmayı onaylayın](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > StorSimple 8000 serisi cihazınıza kayıtlı hizmette bir StorSimple Cloud Appliance oluşturuyorsanız, oluşturma devam ederken bir kayıt anahtarı oluşturmayın. Oluşturmanın tamamlanmasını bekleyin ve ardından kayıt anahtarını oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple 8000 serisi cihazı](storsimple-8000-deployment-walkthrough-u2.md)nasıl dağıtılayın hakkında daha fazla bilgi edinin.

