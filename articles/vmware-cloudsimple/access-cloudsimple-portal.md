---
title: Azure VMware çözümlerine erişme (AVS)-Portal
description: Azure portal Azure VMware çözümlerine (AVS) nasıl erişebileceğinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0b0601b9d2cf505aa5b25e10875338352cab771
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015959"
---
# <a name="access-azure-vmware-solutions-avs-from-the-azure-portal"></a>Azure portal Azure VMware çözümlerine (AVS) erişin

Çoklu oturum açma, AVS portalına erişim için desteklenir. Azure portal oturum açtıktan sonra, AVS portalına yeniden oturum açmadan erişebilirsiniz. AVS portalına ilk kez eriştiğinizde, [AVS hizmet Yetkilendirme](#consent-to-avs-service-authorization-application) uygulamasını yetkilendirmeniz istenir. Yetkilendirme, tek seferlik bir işlemdir.

## <a name="before-you-begin"></a>Başlamadan önce

Yerleşik **sahibi** ve **katkıda bulunan** rollerine sahip kullanıcılar AVS portalına erişebilir. Rollerin, AVS hizmetinin dağıtıldığı kaynak grubunda yapılandırılması gerekir. Roller, AVS hizmet nesnesi üzerinde de yapılandırılabilir. Rolünüzü denetleme hakkında daha fazla bilgi için [rol atamalarını görüntüleme](https://docs.microsoft.com/azure/role-based-access-control/check-access) makalesine bakın. Yalnızca yerleşik **sahibi** ve **katkıda bulunan** rollerine sahip kullanıcılar AVS portalına erişebilir. Rolün abonelikte yapılandırılması gerekir. Rolünüzü denetleme hakkında daha fazla bilgi için [rol atamalarını görüntüleme](https://docs.microsoft.com/azure/role-based-access-control/check-access) makalesine bakın.

Özel roller kullanıyorsanız, rol ```Actions```altında aşağıdaki işlemlerden herhangi birine sahip olmalıdır.  Özel roller hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). İşlemlerden herhangi biri ```NotActions```bir parçasıysa, Kullanıcı AVS portalına erişemez. 

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-avs-portal"></a>AVS portalına erişme

1. **Tüm Hizmetler**’i seçin.

2. **AVS Hizmetleri**için arama yapın.

3. Özel bulutunuzu oluşturmak istediğiniz AVS hizmetini seçin.

4. **Genel bakış** SAYFASıNDA, **AVS portalına git ' e**tıklayın. AVS portalına ilk kez Azure portal erişiyorsanız, [AVS hizmet Yetkilendirme](#consent-to-avs-service-authorization-application) uygulamasına yetki vermeniz istenir. 

    ![AVS portalını Başlat](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Doğrudan Azure portal özel bir bulut işlemi (özel bir bulut oluşturmak veya genişletmek gibi) seçerseniz, AVS portalı belirtilen sayfada açılır.

AVS portalında, AVS özel bulutunuz hakkındaki özet bilgileri göstermek için yan menüden **giriş** ' i seçin. AVS özel bulutunuzun kaynakları ve kapasitesi, dikkat gerektiren uyarılarla ve görevlerle birlikte gösterilir. Ortak görevler için sayfanın üst kısmındaki adlandırılmış simgeler ' e tıklayın.

![Giriş sayfası](media/cloudsimple-portal-home.png)

## <a name="consent-to-avs-service-authorization-application"></a>AVS Hizmeti yetkilendirme uygulamasına izin verme

Azure portal AVS portalını ilk kez başlatmak, AVS hizmet Yetkilendirme uygulaması için izin vermenizi gerektirir. İstenen izinleri vermek ve AVS portalına erişmek için **kabul et** ' i seçin.

![AVS hizmet yetkilendirmesi-Yöneticiler için onay](media/cloudsimple-azure-consent.png)

Genel yönetici ayrıcalıklarınız varsa, kuruluşunuza izin verebilirsiniz. **Kuruluşunuz adına izin '** yı seçin.

![AVS hizmet yetkilendirmesi-genel yönetici](media/cloudsimple-azure-consent-global-admin.png)

İzinleriniz AVS portalına erişime izin vermezse, gerekli izinleri vermek için kiracınızın genel yöneticisine başvurun. Genel yönetici, kuruluşunuzun adına izin verebilir.

![AVS hizmet yetkilendirmesi onayı-Yöneticiler gerektirir](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluşturmayı](https://docs.azure.cloudsimple.com/create-private-cloud/) öğrenin
* [Özel bir bulut ortamını yapılandırmayı](quickstart-create-private-cloud.md) öğrenin
