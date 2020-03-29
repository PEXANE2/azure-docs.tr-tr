---
title: StorSimple Virtual Array Update 0.5 sürüm notları| Microsoft Dokümanlar
description: Güncelleme 0.5 çalıştıran StorSimple Virtual Array için kritik açık sorunları ve çözümleri açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: 385d9126d578250064659153f6f0f54eec696790
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60870681"
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>StorSimple Virtual Array Update 0.5 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Microsoft Azure StorBasit Sanal Dizi güncelleştirmeleri için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. StorSimple Virtual Array'inizi dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Güncelleme 0.5 yazılım sürümü **10.0.10290.0**karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğrar ve cihazınızı yeniden başlatın. G/Ç devam ediyorsa, aygıt kapalı kalma süresine neden olur. Güncelleştirmenin nasıl uygulanacağı yla ilgili ayrıntılı talimatlar için [Güncelleştirme 0.5'i yükleyin.](storsimple-virtual-array-install-update-05.md)


## <a name="whats-new-in-the-update-05"></a>Güncelleme 0.5'teki yenilikler
Güncelleştirme 0.5 öncelikle bir hata düzeltme yapıdır. Ana geliştirmeler ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme esnekliği iyileştirmeleri** - Bu sürüm, yedekleme esnekliğini artıran düzeltmelere sahiptir. Önceki sürümlerde, yedeklemeler yalnızca belirli özel durumlar için yeniden denendi. Bu sürüm tüm yedekleme özel durumlarını yeniden dener ve yedeklemeleri daha esnek hale getirir.

- **Depolama kullanım izleme güncellemeleri** - 30 Haziran 2017 tarihinden itibaren StorSimple Virtual Device Series için depolama kullanım izleme soracaktır. Bu, Güncelleştirme 0.4 veya daha düşük çalıştıran tüm sanal dizilerin izleme grafikleri için geçerlidir. Bu güncelleştirme, Azure portalında depolama alanı kullanımı izleme kullanımını sürdürmeniz için gereken değişiklikleri içerir. İzleme özelliğini kullanmaya devam etmek için bu kritik güncelleştirmeyi 30 Haziran 2017'den önce yükleyin.


## <a name="issues-fixed-in-the-update-05"></a>Güncelleştirme 0.5'te düzeltilen sorunlar

Aşağıdaki tablo, bu sürümde düzeltilen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme esnekliği| Önceki sürümlerde, yedeklemeler yalnızca belirli özel durumlar için yeniden denendi. Bu sürüm, tüm yedekleme özel durumlarını yeniden deneyerek yedeklemeleri daha esnek hale getirmek için bir düzeltme içerir.|
| 2 |İzleme| StorSimple Sanal Cihaz Serisi'nin depolama kullanım takibi 30 Haziran 2017 tarihinden itibaren amortismana alınacaktır. Bu eylem, StorSimple Virtual Arrays (1200 model) üzerinde çalışan StorSimple Device Manager hizmetindeki izleme grafiklerini etkiler. Bu sürümde, kullanıcının sanal dizilerde depolama kullanımı izleme kullanımını 30 Haziran 2017'den sonra kullanmaya devam etmesine olanak tanıyan güncelleştirmeler bulunmaktadır.|
| 3 |Dosya sunucusu| Önceki sürümlerde, bir kullanıcı yanlışlıkla sanal dizi şifrelenmiş dosyaları kopyalayabilirsiniz. Bu sürüm, şifreli dosyaların sanal diziye kopyalanmasına izin vermeyecek bir düzeltme içerir. Aygıtınızda güncelleştirmeden önce varolan şifreli dosyalar varsa, şifrelenmiş tüm dosyalar sistemden silinene kadar yedeklemeler başarısız olmaya devam eder. |


## <a name="known-issues-in-the-update-05"></a>Güncelleştirme 0.5'te bilinen sorunlar

Aşağıdaki tablo, StorSimple Virtual Array için bilinen sorunların bir özetini sağlar ve önceki sürümlerden belirtilen sorunları içerir.

| Hayır. | Özellik | Sorun | Geçici çözüm/yorumlar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal aygıtlar desteklenen genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal aygıtlar, olağanüstü durum kurtarma (DR) iş akışı kullanılarak Genel Kullanılabilirlik sürümü için başarısız olunmalıdır. |
| **2.** |Sağlanan veri diski |Belirli bir boyutta bir veri diski oluşturduktan ve ilgili StorSimple sanal aygıtı oluşturduktan sonra, veri diskini genişletmemeniz veya küçültmemeniz gerekir. Yapmaya çalışmak, aygıtın yerel katmanlarındaki tüm verilerin kaybolmasına neden olabilir. | |
| **3.** |Grup ilkesi |Bir aygıt etki alanına katıldığında, grup ilkesi uygulamak aygıt çalışmasını olumsuz etkileyebilir. |Sanal dizinizin Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna grup ilkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel web Web UI |Internet Explorer'da (IE ESC) gelişmiş güvenlik özellikleri etkinse, Sorun Giderme veya Bakım gibi bazı yerel web ui sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer'daki gelişmiş güvenlik özelliklerini kapatın. |
| **5.** |Yerel web Web UI |Hyper-V sanal makinede, web arabirimi ndeki ağ arabirimleri 10 Gbps arabirimi olarak görüntülenir. |Bu davranış Hyper-V'nin bir yansımasıdır. Hyper-V her zaman sanal ağ bağdaştırıcıları için 10 Gbps gösterir. |
| **6.** |Katmanlı hacimler veya hisseler |StorSimple katmanlı birimleriyle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen önlemler şunlardır: <br></br>Uygulama mantığınızda bayt aralığı kilitlemeyi kapatın.<br></br>Katmanlı birimlerin aksine, bu uygulama için verileri yerel olarak sabitlenmiş birimlere koymayı seçin.<br></br>*Uyarı*: Yerel olarak sabitlenmiş hacimler ve bayt aralığı kilitleme etkinleştirildiğinde, yerel olarak sabitlenmiş birim, geri yükleme tamamlanmadan önce bile çevrimiçi olabilir. Bu gibi durumlarda, geri yükleme devam ediyorsa, geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı hisseler |Büyük dosyalarla çalışmak, yavaş katman dışına çıkmasına neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşım boyutunun %3'ünden daha küçük olduğunu öneririz. |
| **8.** |Hisseler için kullanılan kapasite |Payla ilgili veri olmadığında hisse tüketimini görebilirsiniz. Bu tüketim, hisseler için kullanılan kapasitenin meta verileri içermesinden dir. | |
| **9.** |Olağanüstü durum kurtarma |Bir dosya sunucusunun olağanüstü durum kurtarma durumunu yalnızca kaynak aygıtla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanında hedef aygıta olağanüstü durum kurtarma desteklenmez. |Bu daha sonraki bir sürümde uygulanır. Daha fazla bilgi [için, StorSimple Virtual Array için Failover ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) gidin |
| **10.** |Azure PowerShell |StorSimple sanal aygıtları bu sürümde Azure PowerShell üzerinden yönetilemez. |Sanal cihazların tüm yönetimi Azure portalı ve yerel web web ui üzerinden yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi aygıt konsolu yalnızca en-us klavye formatında giriş kabul eder. | |
| **12.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimleri çevrimdışı duruma getirmeniz ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun daha sonraki bir sürümde ele alınmıştır. |
| **13.** |iSCSI sunucusu |iSCSI birimi için görüntülenen 'Kullanılmış depolama' StorSimple Device Manager hizmetinde ve iSCSI ana bilgisayarda farklı olabilir. |iSCSI ana bilgisayarı dosya sistemi görünümüne sahiptir.<br></br>Aygıt, birim maksimum boyuttayken ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Klasördeki bir dosyada onunla ilişkili alternatif veri akışı (ADS) varsa, ADS olağanüstü durum kurtarma, klonlama ve Öğe Düzeyi Kurtarma yoluyla yedeklenmez veya geri yüklenmez. | |
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |StorSimple Virtual Array dosya sunucusunda kopyalandığında veya depolandığında Windows Şifreleme Dosya Sistemi (EFS) tarafından korunan dosyalar, desteklenmeyen bir yapılandırmayla sonuçlanır.  | |

## <a name="next-step"></a>Sonraki adım
StorSimple Virtual Array'inize [Güncelleme 0.5'i yükleyin.](storsimple-virtual-array-install-update-05.md)

## <a name="references"></a>Başvurular
Eski bir sürüm notu mu arıyorsunuz? Şuraya gidin:

* [StorSimple Sanal Dizi Güncelleme 0.4 Sürüm Notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 0.3 Sürüm Notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 ve 0.2 Sürüm Notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Virtual Array Genel Kullanılabilirlik Sürüm Notları](storsimple-ova-pp-release-notes.md)

