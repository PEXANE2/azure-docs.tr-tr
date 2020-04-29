---
title: StorSimple Sanal dizisi güncelleştirme 0,3 sürüm notları
description: Güncelleştirme 0,3 çalıştıran StorSimple Sanal dizisine yönelik kritik açık sorunlar ve çözümleri açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: f56c36f18379449409f4989eab9510da1f686d0d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80397799"
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple Sanal dizisi güncelleştirme 0,3 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, Microsoft Azure StorSimple Sanal dizi güncelleştirmelerine yönelik kritik açık sorunları ve çözümlenen sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. StorSimple Sanal dizinizi dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0,3, yazılım sürümü **10.0.10288.0**'e karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan ve cihazınızı yeniden başlatacak. G/ç devam ediyorsa, cihaz kapalı kalma süresini doğurur.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Güncelleştirme 0,3 ' deki yenilikler
Güncelleştirme 0,3, birincil olarak bir hata düzeltmesini derleme. Bu sürümde, önceki sürümde yedekleme hatalarıyla sonuçlanan bazı hatalar giderilmiştir.

## <a name="issues-fixed-in-the-update-03"></a>Güncelleştirme 0,3 ' de düzeltilen sorunlar
Aşağıdaki tabloda, bu sürümde düzeltilen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedeklemeler |Önceki sürümde, yedeklemelerin dosya paylaşımında tamamlanmasının başarısız olduğu bir sorun görüldü. Bu sorun oluştuysa, yedekleme işi başarısız olur ve kullanıcıya bildirimde bulunan StorSimple Yöneticisi hizmetinde kritik bir uyarı tetiklenir. Bu sorun, paylaşımlardaki verileri veya verilere erişimi etkilemez. Bu sürümde kök nedeni tanımlandı ve düzeltildi. <br></br> Bu sorunu zaten gördüğünüz paylaşımlara daha etkin bir şekilde uygulanmaz. Bu sorunu yaşayan müşteriler öncelikle güncelleştirme 0,3 ' i uyguladıktan sonra sorunu çözmesi için tam sistem yedeklemesi gerçekleştirmek üzere Microsoft Desteği başvurun. Müşteriler, Microsoft Desteği iletişim yerine, etkilenen paylaşımlar için sağlıklı bir yedekten yeni bir paylaşıma de geri yüklenebilir. |
| 2 |iSCSI |Önceki sürümde, StorSimple Sanal dizisindeki bir birime veri kopyalanırken birimlerin kaybolacağı bir sorun görüldü. Bu sorun bu sürümde düzeltildi. <br></br> Düzeltmeler yalnızca yeni oluşturulan birimlerde geçerli olur. Düzeltmeler zaten bu sorunu görmekte olan birimlere geriye dönük olarak uygulanmaz. Müşterilerin klasik Azure portalı aracılığıyla etkilenen birimleri çevrimiçi hale getirmek, bu birimler için bir yedekleme gerçekleştirmesi ve sonra bu birimleri yeni birimlere geri yüklemesi önerilir. |

## <a name="known-issues-in-the-update-03"></a>Güncelleştirme 0,3 ' de bilinen sorunlar
Aşağıdaki tabloda, StorSimple Sanal dizisi için bilinen sorunların özeti verilmiştir ve önceki sürümlerden gelen sorunlar sürümü dahildir. 

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar desteklenen bir genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal cihazlar, olağanüstü durum kurtarma (DR) iş akışı kullanan genel kullanılabilirlik sürümü için yük devretmelidir. |
| **2.** |Sağlanan veri diski |Belirtilen boyutta bir veri diski sağladıktan ve karşılık gelen StorSimple Sanal cihazını oluşturduktan sonra, veri diskini genişletmemelidir veya küçültmemelidir. Bu durum, cihazın yerel katmanlarında tüm verilerin kaybedilmesine neden olur. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir grup ilkesi uygulandığında cihaz işlemi olumsuz etkilenebilir. |Sanal diziniz Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna hiçbir Grup İlkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel Web Kullanıcı arabirimi |Internet Explorer 'da (IE ESC) Artırılmış güvenlik özellikleri etkinse, sorun giderme veya bakım gibi bazı yerel Web Kullanıcı arabirimi sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer 'da gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **e.** |Yerel Web Kullanıcı arabirimi |Hyper-V sanal makinesinde, Web Kullanıcı arabirimindeki ağ arabirimleri 10 Gbps arabirim olarak görüntülenir. |Bu davranış, Hyper-V ' y i bir yansıdır. Hyper-V, sanal ağ bağdaştırıcıları için her zaman 10 Gbps gösterir. |
| **inç.** |Katmanlı birimler veya paylaşımlar |StorSimple katmanlı birimlerle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüler şunlardır: <br></br>Uygulama mantığınızdaki bayt aralığı kilitlemeyi devre dışı bırakın.<br></br>Bu uygulamaya yönelik verileri katmanlı birimlerin aksine yerel olarak sabitlenmiş birimlerde koymak için seçin.<br></br>*Desteklenmediği uyarısıyla*: yerel olarak sabitlenmiş birimler kullanılırken ve bayt aralığı kilitleme etkinleştirildiğinde, geri yükleme tamamlanmadan önce yerel olarak sabitlenmiş birim çevrimiçi olabilir. Bu tür örneklerde, bir geri yükleme işlemi devam ediyorsa geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımlar |Büyük dosyalarla çalışma, yavaş katmana neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşma boyutunun %3 ' inden küçük olmasını öneririz. |
| **240.** |Paylaşımlar için kullanılan kapasite |Paylaşımda veri olmadığında paylaşma tüketimi görebilirsiniz. Bunun nedeni, paylaşımlar için kullanılan kapasitenin meta verileri içerir. | |
| **tuşlarına.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusunun olağanüstü durum kurtarma işlemini kaynak cihazla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanındaki hedef cihaza olağanüstü durum kurtarma desteklenmiyor. |Bu, sonraki bir sürümde uygulanır. |
| **(.** |Azure PowerShell |StorSimple Sanal cihazları bu sürümdeki Azure PowerShell tarafından yönetilemez. |Sanal cihazların tüm yönetimi, klasik Azure portalı ve yerel Web Kullanıcı arabirimi aracılığıyla yapılmalıdır. |
| **üst.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimindeki girişi kabul eder. | |
| **+.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamıyor. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, bu birimleri çevrimdışına almanız ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun, sonraki bir sürümde giderilmiştir. |
| **hatası.** |Iscsı sunucusu |Bir Iscsı birimi için görünen ' kullanılan depolama alanı, StorSimple Yöneticisi hizmeti ve Iscsı ana bilgisayarında farklı olabilir. |Iscsı konağında dosya sistemi görünümü bulunur.<br></br>Cihaz, birim en büyük boyuttan ayrıldığı blokları görür. |
| **May.** |Dosya sunucusu |Bir klasördeki bir dosya ile ilişkili alternatif veri akışı (ADS) varsa, reklamlar olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde kurtarma aracılığıyla yedeklenmez veya geri yüklenmez. | |

## <a name="next-step"></a>Sonraki adım
StorSimple Sanal diziniz üzerinde [0,3 güncelleştirmesini yükler](storsimple-ova-install-update-01.md) .

## <a name="references"></a>Başvurular
Daha eski bir sürüm notuna mi bakıyorsunuz? Şuraya gidin: 

* [StorSimple Sanal dizisi güncelleştirme 0,1 ve 0,2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

