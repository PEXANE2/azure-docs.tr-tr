---
title: Microsoft Azure StorSimple Manager Sanal Dizi yönetimi | Microsoft Dokümanlar
description: Azure portalındaki StorSimple Aygıt Yöneticisi hizmetini kullanarak StorSimple şirket içi Sanal Dizinizi nasıl yöneteceklerinizi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: bb6bb491ca71e5ced5aecc8137e9e1cbd950e80b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62123814"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-virtual-array"></a>StorSimple Sanal Dizinizi yönetmek için StorSimple Device Manager hizmetini kullanın
![kurulum işlemi akışı](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makalede, storSimple Device Manager hizmet arabirimine nasıl bağlanılacak ve çeşitli seçenekler de dahil olmak üzere açıklanır ve bu kullanıcı arabirimi aracılığıyla gerçekleştirilebilecek belirli iş akışlarına bağlantılar sağlar.

Bu makaleyi okuduktan sonra, nasıl yapılacağını bilecek:

* StorSimple Device Manager hizmetine bağlanın
* StorSimple Aygıt Yöneticisi Kullanıcı UI'da gezinme
* StorSimple Device Manager hizmeti aracılığıyla StorSimple Virtual Array'inizi yönetin

> [!NOTE]
> StorSimple 8000 serisi cihaz için kullanılabilir yönetim seçeneklerini görüntülemek için [StorSimple cihazınızı yönetmek için StorSimple Manager hizmetini kullanın.](storsimple-manager-service-administration.md)
> 
> 

## <a name="connect-to-the-storsimple-device-manager-service"></a>StorSimple Device Manager hizmetine bağlanın
StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple Sanal Diziye bağlanır. Bu aygıtları yönetmek için tarayıcıda çalışan merkezi bir Microsoft Azure portalı kullanırsınız. StorSimple Device Manager hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmete bağlanmak için
1. [https://ms.portal.azure.com](https://ms.portal.azure.com)Git.
2. Microsoft hesap kimlik bilgilerinizi kullanarak, Microsoft Azure portalında oturum açın (bölmenin sağ üst kısmında bulunan).
3. Belirli bir abonelikteki tüm cihaz yöneticilerinizi görüntülemek için StorSimple Device Managers'da Gözat (> 'Filtre' olarak gidin.

## <a name="use-the-storsimple-device-manager-service-to-perform-management-tasks"></a>Yönetim görevlerini gerçekleştirmek için StorSimple Device Manager hizmetini kullanın
Aşağıdaki tablo, StorSimple Device Manager hizmet özeti bıçak içinde gerçekleştirilebilecek tüm ortak yönetim görevlerinin ve karmaşık iş akışlarının bir özetini gösterir. Bu görevler, başlatıldıkları bıçaklara göre düzenlenir.

Her iş akışı hakkında daha fazla bilgi için tablodaki uygun yordamı tıklatın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager iş akışları
| Bunu yapmak istiyorsanız ... | Bu yordamı kullanın |
| --- | --- |
| Hizmet oluşturma</br>Bir hizmeti silme</br>Hizmet kayıt anahtarı alma</br>Hizmet kayıt anahtarını yeniden oluşturma |[StorSimple Device Manager hizmetini dağıtın](storsimple-virtual-array-manage-service.md) |
| Etkinlik günlüklerini görüntüleme |[StorSimple hizmet özetini kullanma](storsimple-virtual-array-service-summary.md) |
| Sanal Diziyi Devre Dışı Bırakma</br>Sanal Dizi silme |[Sanal bir diziyi devre dışı bırakma veya silme](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve cihaz arıza</br>Failover ön koşullar</br>İş sürekliliği felaket kurtarma (BCDR)</br>Olağanüstü durum kurtarma sırasında hatalar |[StorSimple Virtual Array'iniz için olağanüstü durum kurtarma ve cihaz arızası](storsimple-virtual-array-failover-dr.md) |
| Paylaşımları ve hacimleri yedekleme</br>Manuel yedekleme alın</br>Yedekleme zamanlamasını değiştirme</br>Varolan yedeklemeleri görüntüleme |[StorSimple Sanal Dizinizi yedekle](storsimple-virtual-array-backup.md) |
| Yedekleme kümesinde kopya paylaşımları</br>Yedekleme kümesinden kopya birimleri</br>Öğe düzeyinde kurtarma (yalnızca dosya sunucusu) |[StorSimple Virtual Array'inizin bir yedeğinden klonlama](storsimple-virtual-array-clone.md) |
| Depolama hesapları hakkında</br>Depolama hesabı ekleme</br>Depolama hesabını edin</br>Bir depolama hesabını silme |[StorSimple Virtual Array için depolama hesaplarını yönetme](storsimple-virtual-array-manage-storage-accounts.md) |
| Erişim denetimi kayıtları hakkında</br>Erişim denetimi kaydı ekleme veya değiştirme </br>Erişim denetimi kaydını silme |[StorSimple Virtual Array için erişim kontrol kayıtlarını yönetme](storsimple-virtual-array-manage-acrs.md) |
| İş ayrıntılarını görüntüleme |[StorSimple Virtual Array işlerini yönetme](storsimple-virtual-array-manage-jobs.md) |
| Uyarı ayarlarını yapılandırma</br>Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[StorSimple Virtual Array için uyarıları görüntüleme ve yönetme](storsimple-virtual-array-manage-alerts.md) |
| Aygıt yöneticisi parolasını değiştirme |[StorSimple Virtual Array aygıt yöneticisi parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md) |
| Yazılım güncelleştirmelerini yükleme |[Sanal Dizinizi Güncelleyin](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Aşağıdaki görevler için [yerel web ui](storsimple-ova-web-ui-admin.md) kullanmalısınız:
> 
> * [Hizmet veri şifreleme anahtarını alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Destek paketi oluşturma](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Sanal Diziyi durdurup yeniden başlatın](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Web Kullanıcı Arabirimi ve nasıl kullanılacağı hakkında bilgi için [StorSimple Virtual Array'inizi yönetmek için StorSimple web Kullanıcı Arabirimi'ni kullanın.](storsimple-ova-web-ui-admin.md)

