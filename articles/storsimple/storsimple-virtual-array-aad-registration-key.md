---
title: StorSimple Virtual Arrays için yeni kimlik doğrulaması
description: Hizmetiniz için AAD tabanlı kimlik doğrulamasını nasıl kullanacağınızı, yeni kayıt anahtarı oluşturmayı ve cihazların el ile kaydını nasıl gerçekleştiracağınızı açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 89f367e866c1a794f4359c76b8b8a8a9cfefd50d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76273802"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>StorSimple cihazınız için yeni kimlik doğrulamasını kullanın

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple Sanal Diziye bağlanır. Bugüne kadar, StorSimple Aygıt Yöneticisi hizmeti, hizmeti StorSimple cihazınıza doğrulamak için bir Erişim Denetimi hizmeti (ACS) kullanmıştır. ACS mekanizması yakında amortismana hazır olacak ve yerine Azure Etkin Dizin (AAD) kimlik doğrulaması yer alacak.

Bu makalede yer alan bilgiler yalnızca StorSimple 1200 Serisi Sanal Diziler için de geçerlidir. Bu makalede, AAD kimlik doğrulamasının ayrıntıları ve ilgili yeni hizmet kaydı anahtarı ve Güvenlik Duvarı kurallarında storSimple aygıtları için geçerli olan değişiklikler açıklanmaktadır.

AAD kimlik doğrulaması, Güncelleştirme 1 veya daha sonra çalıştıran StorSimple Virtual Arrays'te (model 1200) oluşur.

AAD kimlik doğrulaması nın başlatılması nedeniyle, aşağıdakilerde değişiklikler meydana gelir:

- Güvenlik duvarı kuralları için URL desenleri.
- Servis kayıt anahtarı.

Bu değişiklikler aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

## <a name="url-changes-for-aad-authentication"></a>AAD kimlik doğrulaması için URL değişiklikleri

Hizmetin AAD tabanlı kimlik doğrulaması kullandığından emin olmak için, tüm kullanıcıların güvenlik duvarı kurallarına yeni kimlik doğrulama URL'lerini eklemeleri gerekir.

StorSimple Virtual Array kullanıyorsanız, aşağıdaki URL'nin güvenlik duvarı kurallarına dahil olduğundan emin olun:

| URL deseni                         | Bulut | Bileşen/İşlevsellik         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Azure Genel |AAD kimlik doğrulama hizmeti      |
| `https://login.microsoftonline.us` | ABD Kamu |AAD kimlik doğrulama hizmeti      |

StorSimple Virtual Arrays için URL desenlerinin tam listesi [için, güvenlik duvarı kuralları için URL desenlerine](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules)gidin.

Kimlik doğrulama URL'si amortisman tarihinden sonra güvenlik duvarı kurallarına dahil edilmezse, kullanıcılar StorSimple aygıtlarının hizmetle birlikte kimlik doğrulaması yapamadığını belirten kritik bir uyarı görür. Hizmet aygıtla iletişim kuramaz. Kullanıcılar bu uyarıyı görürse, yeni kimlik doğrulama URL'sini eklemeleri gerekir. Uyarı hakkında daha fazla bilgi için [StorSimple cihazınızı izlemek için uyarıları kullanın.](storsimple-virtual-array-manage-alerts.md#networking-alerts)

## <a name="device-version-and-authentication-changes"></a>Aygıt sürümü ve kimlik doğrulama değişiklikleri

StorSimple Virtual Array kullanıyorsanız, çalıştırdığınız aygıt yazılımı sürümüne bağlı olarak hangi eylemi yapmanız gerektiğini belirlemek için aşağıdaki tabloyu kullanın.

| Cihazınız çalışıyorsa  | Aşağıdaki eylemi                                    |
|----------------------------|--------------------------------------------------------------|
| 1.0 veya daha sonra güncelleştirin ve çevrimdışı. <br> URL'nin beyaz listeye alınmadığı konusunda bir uyarı görürsünüz.| 1. Güvenlik duvarı kurallarını kimlik doğrulama URL'sini içerecek şekilde değiştirin. Kimlik [doğrulama URL'lerine](#url-changes-for-aad-authentication)bakın. <br> 2. [AAD kayıt anahtarını servisten alın.](#aad-based-registration-keys) <br> 3. [Sanal dizinin Windows PowerShell arabirimine bağlanmak için](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor)1-5 adımlarını gerçekleştirin.<br> 4. `Invoke-HcsReRegister` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet kullanın. Önceki adımda aldığınız anahtarı tedarik edin.|
| 1.0 veya sonraki güncelleme ve cihaz çevrimiçi.| İşlem yapmanız gerekmez.                                       |
| 0.6 veya daha erken güncelleştirin ve aygıt çevrimdışı. | 1. [Katalog sunucusu üzerinden Güncelleme 1.0'ı indirin.](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix)<br>2. [Yerel web Kullanıcı Durumu üzerinden Güncelleştirme 1.0 uygulayın.](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix)<br>3. [AAD kayıt anahtarını servisten alın.](#aad-based-registration-keys) <br>4. [Sanal dizinin Windows PowerShell arabirimine bağlanmak için](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor)1-5 adımlarını gerçekleştirin.<br>5. `Invoke-HcsReRegister` Windows PowerShell üzerinden cihazı kaydetmek için cmdlet kullanın. Önceki adımda aldığınız anahtarı tedarik edin.|
| 0.6 veya daha erken güncelleştirin ve cihaz çevrimiçi | Güvenlik duvarı kurallarını kimlik doğrulama URL'sini içerecek şekilde değiştirin.<br> Azure portalı üzerinden Güncelleme 1.0'ı yükleyin. |

## <a name="aad-based-registration-keys"></a>AAD tabanlı kayıt anahtarları

StorSimple Virtual Arrays için Güncelleme 1.0'ı başlayarak, yeni AAD tabanlı kayıt anahtarları kullanılır. StorSimple Device Manager hizmetinizi cihaza kaydetmek için kayıt anahtarlarını kullanırsınız.

Güncelleme 0.6 veya daha önceki bir StorSimple Virtual Arrays kullanıyorsanız, yeni AAD hizmet kayıt anahtarlarını kullanamazsınız. Hizmet kayıt anahtarını yeniden oluşturmanız gerekir. Anahtarı yeniden oluşturduktan sonra, yeni anahtar sonraki tüm aygıtları kaydetmek için kullanılır. Eski anahtar artık geçerli değil.

- Yeni AAD kayıt anahtarı 3 gün sonra sona erer.
- AAD kayıt anahtarları yalnızca Güncelleme 1 veya daha sonra çalıştıran StorSimple 1200 serisi sanal dizilerle çalışır. StorSimple 8000 serisi aygıtın AAD kayıt anahtarı çalışmaz.
- AAD kayıt anahtarları ilgili ACS kayıt anahtarlarından daha uzundur.

AAD hizmet kayıt anahtarı oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-generate-the-aad-service-registration-key"></a>AAD hizmet kayıt anahtarını oluşturmak için

1. **StorSimple Aygıt**Yöneticisi'nde ** &gt; Yönetim** **Tuşları'na**gidin.
    
    ![Tuşlara Git](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. **Oluştur tuşunu**tıklatın.

    ![Yeniden oluşturma'yı tıklatın](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Yeni anahtarı kopyalayın. Eski anahtar artık çalışmıyor.

    ![Yeniden oluşturmayı onaylayın](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple Virtual Array'i](storsimple-virtual-array-deploy1-portal-prep.md) dağıtma hakkında daha fazla bilgi edinin
