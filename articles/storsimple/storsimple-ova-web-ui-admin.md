---
title: StorSimple Sanal dizisi Web Kullanıcı arabirimi yönetimi | Microsoft Docs
description: StorSimple Sanal dizisi Web Kullanıcı arabirimi aracılığıyla temel cihaz yönetim görevlerinin nasıl gerçekleştirileceğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 6bea9c9b7807ffb32b6071c968d3186965477009
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513051"
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>StorSimple Sanal dizinizi yönetmek için Web Kullanıcı arabirimini kullanma
![işlem akışını ayarla](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makaledeki öğreticiler, 2016 genel kullanılabilirlik (GA) sürümünü çalıştıran Microsoft Azure StorSimple Sanal dizisine (StorSimple on-şirket içi sanal cihaz olarak da bilinir) uygulanır. Bu makalede, StorSimple Sanal dizisinde gerçekleştirilebilecek karmaşık iş akışlarının ve yönetim görevlerinin bazıları açıklanmaktadır. StorSimple Sanal dizisini StorSimple Yöneticisi hizmet Kullanıcı arabirimini (Portal Kullanıcı arabirimi olarak adlandırılır) ve cihaz için yerel Web Kullanıcı arabirimini kullanarak yönetebilirsiniz. Bu makale, Web Kullanıcı arabirimini kullanarak gerçekleştirebileceğiniz görevlere odaklanır.

Bu makale aşağıdaki öğreticileri içerir:

* Hizmet veri şifreleme anahtarını al
* Web UI kurulum hatalarında sorun giderme
* Günlük paketi oluşturma
* Cihazınızı kapatma ya da yeniden başlatma

## <a name="get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarını al
İlk cihazınızı StorSimple Yöneticisi hizmetine kaydettiğinizde bir hizmet veri şifreleme anahtarı oluşturulur. Bu anahtar daha sonra StorSimple Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla istenir.

Hizmet veri şifreleme anahtarınızı yanlış girdiyseniz ve almanız gerekiyorsa, hizmetinize kayıtlı olan cihazın yerel Web Kullanıcı arabiriminde aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarını almak için
1. Yerel Web Kullanıcı arabirimine bağlanın. **Yapılandırma**  >  **bulutu ayarları**' na gidin.
2. Sayfanın alt kısmındaki **hizmet veri şifreleme anahtarını al**' a tıklayın. Bir anahtar görüntülenir. Bu anahtarı kopyalayın ve kaydedin.
   
    ![hizmet veri şifreleme anahtarını al 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Web UI kurulum hatalarında sorun giderme
Bazı örneklerde, cihazı yerel Web Kullanıcı arabirimi aracılığıyla yapılandırdığınızda hatalarla karşılaşabilirsiniz. Bu tür hataları tanılamak ve gidermek için tanılama testlerini çalıştırabilirsiniz.

#### <a name="to-run-the-diagnostic-tests"></a>Tanılama testlerini çalıştırmak için
1. Yerel Web Kullanıcı arabiriminde **sorun giderme**  >  **Tanılama Testleri**' ne gidin.
   
    ![Tanılamayı Çalıştır 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. Sayfanın alt kısmındaki **tanılama testlerini Çalıştır**' a tıklayın. Bu işlem, ağ, cihaz, Web proxy, zaman veya bulut ayarlarınızdaki olası sorunları tanılamak için testler başlatacak. Cihazın testleri çalıştırdığı bildirilir.
3. Testler tamamlandıktan sonra sonuçlar görüntülenir. Aşağıdaki örnek, tanı testlerinin sonuçlarını gösterir. Web proxy ayarlarının bu cihazda yapılandırılmadığını ve bu nedenle Web proxy testinin çalıştırılmadığını unutmayın. Ağ ayarları, DNS sunucusu ve saat ayarları için diğer tüm testler başarılı oldu.
   
    ![Tanılamayı Çalıştır 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Günlük paketi oluşturma
Bir günlük paketi, tüm cihaz sorunlarını gidermeye yönelik Microsoft Desteği yardımcı olabilecek tüm ilgili günlüklerinden oluşur. Bu sürümde, yerel Web Kullanıcı arabirimi aracılığıyla bir günlük paketi oluşturulabilir.

#### <a name="to-generate-the-log-package"></a>Günlük paketini oluşturmak için
1. Yerel Web Kullanıcı arabiriminde, **sorun giderme**  >  **Sistem günlükleri**' ne gidin.
   
    ![günlük paketi 1 oluştur](./media/storsimple-ova-web-ui-admin/image31.png)
2. Sayfanın alt kısmındaki **günlük paketi oluştur**' a tıklayın. Sistem günlüklerinin bir paketi oluşturulacaktır. Bu işlem birkaç dakika sürer.
   
    ![günlük paketi oluştur 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Paket başarıyla oluşturulduktan sonra size bildirilir ve bu, paketin oluşturulduğu saat ve tarihi gösterecek şekilde güncelleştirilir.
   
    ![günlük paketi oluştur 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. **Günlük paketini indir**' e tıklayın. Sıkıştırılmış bir paket, sisteminize indirilir.
   
    ![günlük paketi 4 oluştur](./media/storsimple-ova-web-ui-admin/image34.png)
4. İndirilen günlük paketini açabilir ve sistem günlük dosyalarını görüntüleyebilirsiniz.

## <a name="shut-down-and-restart-your-device"></a>Cihazınızı kapatıp yeniden başlatın
Yerel Web Kullanıcı arabirimini kullanarak sanal cihazınızı kapatabilir veya yeniden başlatabilirsiniz. Yeniden başlatmadan önce, konakta ve daha sonra cihazda bulunan birimleri veya paylaşımları çevrimdışına almanız önerilir. Bu, herhangi bir veri bozulması olasılığını en aza indirir. 

#### <a name="to-shut-down-your-virtual-device"></a>Sanal cihazınızı kapatmak için
1. Yerel Web Kullanıcı arabiriminde **bakım**  >  **güç ayarları**' na gidin.
2. Sayfanın alt kısmındaki **kapalı**' ya tıklayın.
   
    ![Cihaz kapanıyor 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Cihazın kapatılmasını, sürmekte olan GÇ sayısını kesintiye uğratacak ve kapalı kalma süresi ile sonuçlanan bir uyarı belirir. Onay simgesine ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Cihaz kapanıyor uyarısı](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Kapatmanın başlatıldığını size bildirilir.
   
    ![cihaz kapatması başlatıldı](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Cihaz şimdi kapatılacak. Cihazınızı başlatmak isterseniz, bunu Hyper-V Yöneticisi aracılığıyla yapmanız gerekir.

#### <a name="to-restart-your-virtual-device"></a>Sanal cihazınızı yeniden başlatmak için
1. Yerel Web Kullanıcı arabiriminde **bakım**  >  **güç ayarları**' na gidin.
2. Sayfanın en altında **Yeniden Başlat**' a tıklayın.
   
    ![cihaz yeniden başlatma](./media/storsimple-ova-web-ui-admin/image36.png)
3. Cihazın yeniden başlatılması, sürmekte olan tüm IOs 'u kesintiye uğratacak ve kapalı kalma süresi ile sonuçlanan bir uyarı görüntülenir. Onay simgesine ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![yeniden başlatma uyarısı](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Yeniden başlatmanın başlatıldığını size bildirilir.
   
    ![yeniden başlatma başlatıldı](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Yeniden başlatma işlemi devam ederken Kullanıcı arabirimine olan bağlantıyı kaybedersiniz. Kullanıcı arabirimini düzenli aralıklarla yenileyerek yeniden başlatmayı izleyebilirsiniz. Alternatif olarak, Hyper-V Yöneticisi aracılığıyla cihaz yeniden başlatma durumunu izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Cihazınızı yönetmek Için StorSimple Yöneticisi hizmetini](storsimple-virtual-array-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

