---
title: StorSimple Virtual Array işlerini görüntüleyin ve yönetin | Microsoft Dokümanlar
description: StorSimple Aygıt Yöneticisi hizmeti İşler sayfasını ve StorSimple Virtual Array için en son ve güncel işleri izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: dbab2aaab2c12bef07748f54e5864d042f1c982a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60302509"
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>StorSimple Virtual Array'in işlerini görüntülemek için StorSimple Device Manager hizmetini kullanın
## <a name="overview"></a>Genel Bakış
**Jobs** blade, StorSimple Device Manager hizmetinize bağlı sanal dizilerde başlatılan işleri görüntülemek ve yönetmek için tek bir merkezi portal sağlar. Birden çok sanal aygıt için çalışan, tamamlanan ve başarısız olan işleri görüntüleyebilirsiniz. Sonuçlar bir tabular biçimde sunulur.

![İşler bıçak](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Aşağıdaki gibi alanlarda filtre uygulayarak ilgilendiğiniz işleri hızlı bir şekilde bulabilirsiniz:

* **Zaman aralığı** – İşler tarih ve saat aralığına göre filtrelenebilir.
* **Cihazlar** – İşler, hizmetinize bağlı belirli bir cihazda başlatılır. Filtre uygulanmış işler daha sonra aşağıdaki özniteliklere göre tablolanır:
  
  * **Adı** – İş adı **Tüm**olabilir , **Yedekleme**, **Klon**, **Fail over**, **Güncellemeler indir**, veya **güncelleştirmeleri yükleyin**.
  * **Durum** – İşler **Tüm**olabilir , **Devam Ediyor**, **Başarılı**, veya **Başarısız**, veya **İptal .**
  * **Varlık** – İşler bir birim, paylaşım veya aygıtla ilişkilendirilebilir.
  * **Aygıt** – İşin başlatıldıği aygıtın adı.
  * **Başladı** – İşin başladığı zaman.
  * **Süre** – İşin yürütüldolduğu süre.
* **Durum** – Tüm, çalışan, tamamlanmış veya başarısız işleri arayabilirsiniz.
* **İş türü** – İş türü tümü olabilir, yedekleme, geri yükleme, başarısız, güncelleştirmeleri karşıdan yükleme veya güncelleştirmeleri yükleme.

İş listesi her 30 saniyede bir yenilenir.

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işin ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. **İşler** bıçağında, uygun filtrelere sahip bir sorgu çalıştırarak ilgilendiğiniz işi(ler) görüntüleyin. Tamamlanmış veya çalışan işleri arayabilirsiniz.
2. Tabular iş listesinden bir iş seçin.
   
    ![İş bıçağı](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Sayfanın alt kısmında **Ayrıntılar'ı**tıklatın.
4. **Ayrıntılar** iletişim kutusunda durum, ayrıntıları ve zaman istatistiklerini görüntüleyebilirsiniz. Aşağıdaki resimde Yedekleme İş **Ayrıntıları** iletişim kutusunun bir örneği gösterilmektedir.
   
    ![İş ayrıntıları](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Sanal makine hipervizörde duraklatıldığında iş hataları
StorSimple Virtual Array'inizde bir iş devam ediyorsa ve aygıt (hipervizörde bulunan sanal makine) 15 dakikadan uzun bir süre duraklatılmışsa, iş başarısız olur. Bunun nedeni, StorSimple Virtual Array saatinizin Microsoft Azure zamanıyla eşitlenmemekten kaynaklanmaktadır. 

Aşağıdaki hatayı görürsünüz: "Cihazınızın süresi Microsoft Azure süresiyle 15 dakikadan fazla eşitlenmemiştir. Hipervizörün ve aygıt sürelerinin bir NTP sunucusuyla senkronize olduğundan emin olun. Bağlantı sorunları olmadığını doğrulayın. Bağlantı sorunlarını gidermek için sanal cihazınızın yerel web arama sürümünden tanılama testleri çalıştırın."

Bu hatalar yedekleme, geri yükleme, güncelleştirme ve başarısız işler için geçerlidir. Sanal makineniz Hyper-V'de sağlanmışsa, makine sonunda zamanı hipervizörünüzle senkronize eder. Bu olduğunda, işinizi yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Virtual Array'inizi yönetmek için yerel web kullanıcı arama ayını nasıl kullanacağınızı öğrenin.](storsimple-ova-web-ui-admin.md)

