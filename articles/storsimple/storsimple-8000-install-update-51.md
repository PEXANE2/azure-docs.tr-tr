---
title: StorSimple 8000 serisi cihaza güncelleştirme 5,1 ' ü yükler | Microsoft Docs
description: StorSimple 8000 serisi cihazınızda StorSimple 8000 serisi güncelleştirme 5,1 ' nin nasıl yükleneceğini açıklar.
services: storsimple
documentationcenter: NA
author: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/05/2020
ms.author: twooley
ms.openlocfilehash: f9cc5181d6cc29ee4b3c2373dbbc91d6290fbe6e
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88782779"
---
# <a name="install-update-51-on-your-storsimple-device"></a>StorSimple cihazınıza güncelleştirme 5,1 ' ü yükler

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Azure portal aracılığıyla önceki bir yazılım sürümünü çalıştıran bir StorSimple cihazına güncelleştirme 5,1 ' nin nasıl yükleneceği açıklanmaktadır. <!--The hotfix method is used when you are trying to install Update 5.1 on a device running pre-Update 3 versions. The hotfix method is also used when a gateway is configured on a network interface other than DATA 0 of the StorSimple device and you are trying to update from a pre-Update 1 software version.-->

Güncelleştirme 5,1, kesintiye uğramayan güvenlik güncelleştirmeleri içerir. Kesintiye uğramamış veya düzenli güncelleştirmeler Azure portal aracılığıyla uygulanabilir <!--or by the hotfix method-->.

> [!IMPORTANT]
>
> * Güncelleştirme 5,1, zorunlu bir güncelleştirmedir ve hemen yüklenmesi gerekir. Daha fazla bilgi için bkz. [güncelleştirme 5,1 sürüm notları](storsimple-update51-release-notes.md).
> * Donanım durumu ve ağ bağlantısı açısından cihaz sistem durumunu tespit etmek için, yüklemeden önce bir el ile ve otomatik denetimler kümesi yapılır. Bu ön denetimler yalnızca Azure portal güncelleştirmeleri uyguladığınızda gerçekleştirilir.
> * Düzeltme yöntemini kullanarak yüklemek istiyorsanız, [Microsoft desteği](mailto:support@microsoft.com)başvurun.

<!--
> * We strongly recommend that when updating a device running versions prior to Update 3, you install the updates using hotfix method. If you encounter any issues, [log a support ticket](storsimple-8000-contact-microsoft-support.md). 


> * We recommend that you install the software and other regular updates via the Azure portal. You should only go to the Windows PowerShell interface of the device (to install updates) if the pre-update gateway check fails in the portal. Depending upon the version you are updating from, the updates may take 4 hours (or greater) to install. The maintenance mode updates must be installed through the Windows PowerShell interface of the device. As maintenance mode updates are disruptive updates, these result in a down time for your device.


> * If running the optional StorSimple Snapshot Manager, ensure that you have upgraded your Snapshot Manager version to Update 5.1 prior to updating the device.
-->

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-51-through-the-azure-portal"></a>Güncelleştirme 5,1 ' yı Azure portal aracılığıyla yükler

Cihazınızı [güncelleştirme 5,1](storsimple-update51-release-notes.md)' ye güncelleştirmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Microsoft cihazdan ek tanılama bilgileri çeker. Sonuç olarak, işletim ekibimiz sorun yaşayan cihazları tanımlarsa, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi bir şekilde donatılmıştır.

#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure portalından bir güncelleştirmeyi yüklemek için

1. StorSimple hizmet sayfasında cihazınızı seçin.

    ![Cihaz Seç](./media/storsimple-8000-install-update-51/update1.png)

2. **Cihaz ayarları**  >  **cihaz güncelleştirmeleri**' ne gidin.

    ![Cihaz güncelleştirmeleri ' ne tıklayın](./media/storsimple-8000-install-update-51/update2.png)

3. Yeni güncelleştirmeler varsa bir bildirim görüntülenir. Alternatif olarak, **cihaz güncelleştirmeleri** dikey penceresinde **Güncelleştirmeleri Tara**' ya tıklayın. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. İş başarıyla tamamlandığında size bildirilir.

    ![Cihaz güncelleştirmeleri ' ne tıklayın](./media/storsimple-8000-install-update-51/update3.png)

4. Bir güncelleştirmeyi cihazınıza uygulamadan önce sürüm notlarını gözden geçirmeniz önerilir. Güncelleştirmeleri uygulamak için **güncelleştirmeleri yüklensin**' e tıklayın. **Düzenli güncelleştirmeleri Onayla** dikey penceresinde, güncelleştirmeleri uygulamadan önce tamamlanacak önkoşulları gözden geçirin. Cihazı güncelleştirmeye hazırız olduğunu göstermek için onay kutusunu işaretleyin ve ardından **Install**' a tıklayın.

    ![Cihaz güncelleştirmeleri ' ne tıklayın](./media/storsimple-8000-install-update-51/update4.png)

5. Bir dizi önkoşul denetimi başlatılır. Bu denetimler şunlardır:
   
   * Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamaya yönelik **denetleyici durumu denetimleri**.
   * StorSimple cihazınızdaki tüm donanım bileşenlerinin sağlıklı olduğunu doğrulamaya yönelik **donanım bileşeni durum denetimleri**.
   * DATA 0’ın cihazınızda etkin olduğunu doğrulamaya yönelik **DATA 0 denetimleri**. Bu arabirim etkin değilse etkinleştirmeniz ve sonra yeniden denemeniz gerekir.

     Güncelleştirme indirilir ve yalnızca tüm denetimler başarıyla tamamlanırsa yüklenir. Denetimler devam ederken size bildirilir. Ön denetimler başarısız olursa, hata nedenleri ile birlikte sağlanacaktır. Bu sorunları çözün ve işlemi yeniden deneyin. Bu sorunları kendi başınıza çözemiyorsanız Microsoft Desteği’ne başvurmanız gerekebilir.

7. Ön denetimler başarıyla tamamlandıktan sonra bir güncelleştirme işi oluşturulur. Güncelleştirme işi başarıyla oluşturulduğunda size bildirilir.
   
    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update-51/update6.png)
   
    Bundan sonra güncelleştirme, cihazınıza uygulanır.

9. Güncelleştirmenin tamamlanması birkaç saat sürer. Güncelleştirme işini seçin ve **Ayrıntılar**’a tıklayarak dilediğiniz zaman işin ayrıntılarını görüntüleyin.

    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update-51/update8.png)

     Ayrıca, güncelleştirme işinin ilerlemesini **cihaz ayarları > işlerle**izleyebilirsiniz. **İşler** dikey penceresinde, güncelleştirme ilerlemesini görebilirsiniz.

     ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update-51/update7.png)

10. İş tamamlandıktan sonra cihaz **güncelleştirmeleri > cihaz ayarları**' na gidin. Yazılım sürümü şimdi güncellenmelidir.


Cihazınızın **StorSimple 8000 serisi güncelleştirme 5,1 (6.3.9600.17885)** çalıştığını doğrulayın. **Son güncelleme tarihi** değiştirilmelidir.
<!-- 5.1 - KB 4542887-->

<!--You will now see that the Maintenance mode updates are available (this message might continue to be displayed for up to 24 hours after you install the updates). The steps to install maintenance mode update are detailed in the next section.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## Install Update 5.1 as a hotfix

The software versions that can be upgraded using the hotfix method are:

* Update 0.1, 0.2, 0.3
* Update 1, 1.1, 1.2
* Update 2, 2.1, 2.2
* Update 3, 3.1
* Update 4
* Update 5

> [!NOTE]
> The recommended method to install Update 5.1 is through the Azure portal when trying to update from Update 3 and later version. When updating a device running versions prior to Update 3, use this procedure. You can also use this procedure if you fail the gateway check when trying to install the updates through the Azure portal. The check fails when you have a gateway assigned to a non-DATA 0 network interface and your device is running a software version earlier than Update 1.

The hotfix method involves the following three steps:

1. Download the hotfixes from the Microsoft Update Catalog.
2. Install and verify the regular mode hotfixes.
3. Install and verify the maintenance mode hotfix.

#### Download updates for your device

You must download and install the following hotfixes in the prescribed order and the suggested folders:

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Software update<br> Download both _HcsSoftwareUpdate.exe_ and _CisMSDAgent.exe_ |Regular <br></br>Non-disruptive |~ 25 mins |FirstOrderUpdate|

If updating from a device running Update 4, you only need to install the OS cumulative updates as second order updates.

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |OS cumulative updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|

If installing from a device running Update 3 or earlier, install the following in addition to the cumulative updates.

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI driver and firmware updates <br> USM firmware update (version 3.38) |Regular <br></br>Non-disruptive |~ 3 hrs <br> (includes 2A. + 2B. + 2C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |OS security updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |OS updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|



You may also need to install disk firmware updates on top of all the updates shown in the preceding tables. You can verify whether you need the disk firmware updates by running the `Get-HcsFirmwareVersion` cmdlet. If you are running these firmware versions: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, then you do not need to install these updates.

| Order | KB | Description | Update type | Install time | Install in folder|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk firmware |Maintenance <br></br>Disruptive |~ 30 mins | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * If updating from Update 4, the total install time is close to 4 hours.
> * Before using this procedure to apply the update, make sure that both the device controllers are online and all the hardware components are healthy.

Perform the following steps to download and install the hotfixes.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]
-->
<!--
[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]
-->

## <a name="next-steps"></a>Sonraki adımlar

[Güncelleştirme 5,1 sürümü](storsimple-update51-release-notes.md)hakkında daha fazla bilgi edinin.
