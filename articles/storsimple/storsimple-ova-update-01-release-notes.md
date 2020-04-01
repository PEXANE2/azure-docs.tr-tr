---
title: StorSimple Virtual Array Update 0.2 & 0.1 sürüm notları
description: Update 0.2 ve 0.1 çalıştıran StorSimple Virtual Array için kritik açık sorunları ve çözümleri açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: 95386f36340aca470769c920e40bbb70e09d34fc
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397876"
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple Virtual Array Update 0.2 ve 0.1 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, Microsoft Azure StorBasit Sanal Dizi güncelleştirmeleri için kritik açık sorunları ve çözülmüş sorunları tanımlar. (Microsoft Azure StorSimple Sanal Dizi, StorSimple şirket içi sanal aygıt veya StorSimple sanal aygıtı olarak da bilinir.) 

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. StorSimple sanal cihazınızı dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Güncelleme 0.2 yazılım sürümü **10.0.10280.0**karşılık gelir; Güncelleme 0.1 sürüm **10.0.10279.0**olduğunu. Aşağıdaki bölümlerde her güncelleştirme için değişiklikler listelenir. 

> [!NOTE]
> Güncelleştirmeler kesintiye uğrar ve cihazınızı yeniden başlatacaktır. G/Ç devam ediyorsa, aygıt kapalı kalma süresine neden olur.
> 
> 

## <a name="issues-fixed-in-the-update-02"></a>Güncelleştirme 0.2'de düzeltilen sorunlar
Güncelleştirme 0.2, aşağıdaki tabloda açıklanan düzeltmeye ek olarak Güncelleştirme 0.1'deki tüm değişiklikleri içerir:

| Özellik | Sorun |
| --- | --- |
| Güncelleştirmeler |Son sürümde, güncelleştirmeler Azure klasik portalında otomatik olarak algılanmadı, bu nedenle güncelleştirmeleri yüklemek için yerel Web UI'ı kullanmanız gerekiyordu. Bu sorun bu sürümde giderilmiştir. Güncelleştirme 0.2'yi yükledikten sonra, Azure klasik portalını kullanarak gelecekteki güncelleştirmeleri yükleyebilirsiniz. |

## <a name="whats-new-in-the-update-01"></a>Güncelleme 0.1'deki yenilikler
Güncelleştirme 0.1 aşağıdaki hata düzeltmeleri ve iyileştirmeler içerir. 

* **Bulut kesintileri için geliştirilmiş esneklik**: Bu sürüm, bulut bağlantısının kesintiye uğraması durumunda olağanüstü durum kurtarma, yedekleme, geri yükleme ve katmanlama etrafında birkaç hata düzeltmesi vardır. 
* **Geliştirilmiş geri yükleme performansı**: Bu sürüm, geri yükleme işlerinin tamamlanma süresini önemli ölçüde azaltan hata düzeltmeleri vardır.
* **Otomatik alan ıslahı optimizasyonu**: Veriler ince olarak sağlanan birimlerde silindiğinde, kullanılmayan depolama bloklarının geri alınması gerekir. Bu sürüm, buluttan gelen alan ıslah işlemini iyileştirerek kullanılmayan alanın önceki sürümlere göre daha hızlı kullanılabilir hale gelmesine neden olmuştur.
* **Yeni sanal disk görüntüleri**: Yeni VHD, VHDX ve VMDK artık Azure klasik portalı üzerinden kullanılabilir. Yeni Güncelleme 0.1 aygıtlarını sağlamak için bu resimleri indirebilirsiniz.
* **Portaldaki işlerin durumunun doğruluğunun iyileştirilmesi**: Yazılımın önceki sürümünde, portaldaki iş durumu raporlaması ayrıntılı değildi. Bu sorun bu sürümde çözülür.
* **Etki alanı birleştirme deneyimi**: Aygıtın etki alanına katılması ve yeniden adlandırılmasıyla ilgili hata düzeltmeleri.

## <a name="issues-fixed-in-the-update-01"></a>Güncelleştirme 0.1'de düzeltilen sorunlar
Aşağıdaki tablo, bu sürümde düzeltilen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Vmdk |Bazı VMware sürümlerinde, işletim sistemi diski uyarılara neden olan ve normal işlemleri bozan seyrek olarak görülüyordu. Bu sürümde düzeltildi. |
| 2 |iSCSI sunucusu |Son sürümde, kullanıcının StorSimple sanal cihazınızın etkin ağ arabirimi için bir ağ geçidi belirtmesi gerekiyordu. Bu davranış, kullanıcının etkin ağ arabirimleri için en az bir ağ geçidi yapılandırması gerektirecek şekilde bu sürümde değiştirilir. |
| 3 |Destek paketi |Yazılımın önceki sürümünde, paket boyutları 1 GB'tan büyük olduğunda Destek paketi koleksiyonu başarısız oldu. Bu sorun bu sürümde giderilmiştir. |
| 4 |Bulut erişimi |Son sürümde, StorSimple Virtual Array ağ bağlantısı yoksa ve yeniden başlatıldı, yerel Kullanıcı Arabirimi bağlantı sorunları olurdu. Bu sorun bu sürümde giderilmiştir. |
| 5 |Grafikleri izleme |Önceki sürümde, bir aygıtın başarısız olduğunu takiben, bulut kapasitesi kullanım grafikleri Azure klasik portalında yanlış değerler görüntülenebilmektedir. Bu, geçerli sürümde sabittir. |

## <a name="known-issues-in-the-update-01"></a>Güncelleştirme 0.1'de bilinen sorunlar
Aşağıdaki tablo, StorSimple Virtual Array için bilinen sorunların bir özetini sağlar ve önceki sürümlerden belirtilen sorunları içerir. **Bu sürümde belirtilen sorunlar bir yıldız işareti ile işaretlenir. Bu listedeki hemen hemen tüm sorunlar StorSimple Virtual Array GA sürümünden taşınmıştır.**

| Hayır. | Özellik | Sorun | Geçici çözüm/yorumlar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal aygıtlar desteklenen genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal aygıtlar, olağanüstü durum kurtarma (DR) iş akışı kullanılarak Genel Kullanılabilirlik sürümü için başarısız olunmalıdır. |
| **2.** |Sağlanan veri diski |Belirli bir boyutta bir veri diski oluşturduktan ve ilgili StorSimple sanal aygıtı oluşturduktan sonra, veri diskini genişletmemeniz veya küçültmemeniz gerekir. Bunu yapmaya çalışmak, aygıtın yerel katmanlarındaki tüm verilerin kaybolmasına neden olur. | |
| **3.** |Grup ilkesi |Bir aygıt etki alanına katıldığında, grup ilkesi uygulamak aygıt çalışmasını olumsuz etkileyebilir. |Sanal dizinizin Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna grup ilkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel web Web UI |Internet Explorer'da (IE ESC) gelişmiş güvenlik özellikleri etkinse, Sorun Giderme veya Bakım gibi bazı yerel web ui sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer'daki gelişmiş güvenlik özelliklerini kapatın. |
| **5.** |Yerel web Web UI |Hyper-V sanal makinede, web arabirimi ndeki ağ arabirimleri 10 Gbps arabirimi olarak görüntülenir. |Bu davranış Hyper-V'nin bir yansımasıdır. Hyper-V her zaman sanal ağ bağdaştırıcıları için 10 Gbps gösterir. |
| **6.** |Katmanlı hacimler veya hisseler |StorSimple katmanlı birimleriyle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen önlemler şunlardır: <br></br>Uygulama mantığınızda bayt aralığı kilitlemeyi kapatın.<br></br>Katmanlı birimlerin aksine, bu uygulama için verileri yerel olarak sabitlenmiş birimlere koymayı seçin.<br></br>*Uyarı*: Yerel olarak sabitlenmiş hacimler ve bayt aralığı kilitleme etkinse, geri yükleme tamamlanmadan önce bile yerel olarak sabitlenmiş birimin çevrimiçi olabileceğini unutmayın. Bu gibi durumlarda, geri yükleme devam ediyorsa, geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı hisseler |Büyük dosyalarla çalışmak, yavaş katman dışına çıkmasına neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşım boyutunun %3'ünden daha küçük olduğunu öneririz. |
| **8.** |Hisseler için kullanılan kapasite |Hisse yle ilgili herhangi bir veri yokluğunda hisse tüketimini görebilirsiniz. Bunun nedeni, paylaşımlar için kullanılan kapasitenin meta verileri içermesidir. | |
| **9.** |Olağanüstü durum kurtarma |Bir dosya sunucusunun olağanüstü durum kurtarma durumunu yalnızca kaynak aygıtla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanında hedef aygıta olağanüstü durum kurtarma desteklenmez. |Bu daha sonraki bir sürümde uygulanacaktır. |
| **10.** |Azure PowerShell |StorSimple sanal aygıtları bu sürümde Azure PowerShell üzerinden yönetilemez. |Sanal cihazların tüm yönetimi Azure klasik portalı ve yerel web web ui üzerinden yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi aygıt konsolu yalnızca en-ABD klavye formatında giriş kabul eder. | |
| **12.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimleri çevrimdışı duruma getirmeniz ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bunlar daha sonraki bir sürümde ele alınacaktır. |
| **13.** |iSCSI sunucusu |iSCSI birimi için görüntülenen 'Kullanılmış depolama' StorSimple Manager hizmetinde ve iSCSI ana bilgisayarda farklı olabilir. |iSCSI ana bilgisayarı dosya sistemi görünümüne sahiptir.<br></br>Aygıt, birim maksimum boyuttayken ayrılan blokları görür. |
| **14.** |Dosya sunucusu* |Klasördeki bir dosyada onunla ilişkili alternatif veri akışı (ADS) varsa, ADS olağanüstü durum kurtarma, klonlama ve Öğe Düzeyi Kurtarma yoluyla yedeklenmez veya geri yüklenmez. | |

## <a name="next-step"></a>Sonraki adım
StorSimple Virtual Array'inize [Güncelleştirmeler yükleyin.](storsimple-ova-install-update-01.md)

