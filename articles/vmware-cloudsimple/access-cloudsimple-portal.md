---
title: CloudSimple ile Azure VMware Çözümüne Erişin - Portal
description: Azure portalından CloudSimple portalıyla VMware Çözümüne nasıl erişileceği açıklanır
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 95d27bab243f6805436465f5027e519d33e20f6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244738"
---
# <a name="access-the-vmware-solution-by-cloudsimple-portal-from-the-azure-portal"></a>Azure portalından CloudSimple portalı ile VMware Çözümüne erişin

CloudSimple portalına erişmek için tek oturum açma desteklenir. Azure portalında oturum açtıktan sonra, yeniden oturum açmadan CloudSimple portalına erişebilirsiniz. CloudSimple portalına ilk erişinizde [CloudSimple Hizmet Yetkilendirme](#consent-to-cloudsimple-service-authorization-application) uygulamasını yetkilendirmeniz istenir.  Yetkilendirme tek seferlik bir eylemdir.

## <a name="before-you-begin"></a>Başlamadan önce

Yerleşik **Sahibi** ve **Katılımcı** rolleri olan kullanıcılar CloudSimple portalına erişebilir.  Roller, CloudSimple hizmetinin dağıtıldığı kaynak grubunda yapılandırılmalıdır.  Roller CloudSimple hizmet nesnesi üzerinde de yapılandırılabilir.  Rolünüzü denetleme hakkında daha fazla bilgi için [rol atamaları](https://docs.microsoft.com/azure/role-based-access-control/check-access) makalenize bakın. Yalnızca yerleşik **Sahibi** ve **Katılımcı** rolleri olan kullanıcılar CloudSimple portalına erişebilir.  Roller abonelik üzerinde yapılandırılmalıdır.  Rolünüzü denetleme hakkında daha fazla bilgi için [rol atamaları](https://docs.microsoft.com/azure/role-based-access-control/check-access) makalenize bakın.

Özel roller kullanıyorsanız, rolün altında ```Actions```aşağıdaki işlemlerden herhangi biri olmalıdır.  Özel roller hakkında daha fazla bilgi için [Azure kaynakları için Özel rolleri](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)görün.  İşlemlerden herhangi biri bir ```NotActions```parçasıysa, kullanıcı CloudSimple portalına erişemez.

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

1. **Tüm Hizmetler**’i seçin.

2. **CloudSimple Services'ı**arayın.

3. Özel Bulut'unuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.

4. Genel **Bakış** sayfasında **CloudSimple portalına git'i**tıklatın.  Azure portalından CloudSimple portalına ilk kez erişiyorsanız, [CloudSimple Hizmet Yetkilendirme](#consent-to-cloudsimple-service-authorization-application) uygulamasına yetki vermeniz istenir. 

    ![CloudSimple portalına başlat](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Doğrudan Azure portalından bir Özel Bulut işlemi (Özel Bulut oluşturma veya genişletme gibi) seçerseniz, CloudSimple portalı belirtilen sayfaya açılır.

CloudSimple portalında, Özel Bulutlarınizle ilgili özet bilgileri görüntülemek için yan menüde **Ana Sayfa'yı** seçin. Özel Bulutlarınızın kaynakları ve kapasitesi, dikkat gerektiren uyarılar ve görevlerle birlikte gösterilir. Ortak görevler için, sayfanın üst kısmındaki adlandırılmış simgeleri tıklatın.

![Ana Sayfa](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>CloudSimple Hizmet Yetkilendirme uygulamasına onay

CloudSimple portalını Azure portalından ilk kez başlatmak için CloudSimple Hizmet Yetkilendirme uygulaması için onayınız gerekiyor.  İstenen izinleri vermek ve CloudSimple portalına erişmek için **Kabul et'i** seçin.

![CloudSimple Hizmet Yetkilendirmesi için onay - yöneticiler](media/cloudsimple-azure-consent.png)

Genel yönetici ayrıcalığınız varsa, kuruluşunuz için onay alabilirsiniz.  **Kuruluşunuz adına onay verin**’i seçin.

![CloudSimple Hizmet Yetkilendirmesine Onay - global yönetici](media/cloudsimple-azure-consent-global-admin.png)

İzinleriniz CloudSimple portalına erişime izin vermiyorsa, gerekli izinleri vermek için kiracınızın genel yöneticisine başvurun.  Genel bir yönetici kuruluşunuz adına izin verebilir.

![CloudSimple Hizmet Yetkilendirmesi onayı - yöneticiler gerektirir](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluşturmayı](https://docs.azure.cloudsimple.com/create-private-cloud/) öğrenin
* Özel bulut ortamını nasıl [yapılandırılatırmayı](quickstart-create-private-cloud.md) öğrenin
