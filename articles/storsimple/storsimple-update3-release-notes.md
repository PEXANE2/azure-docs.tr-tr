---
title: StorSimple 8000 Serisi Güncelleme 3 sürüm notları
description: StorSimple 8000 Serisi Güncelleştirmesi 3'ün yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
author: alkohli
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b61caecd67881eb08c82ea0c26522c63c3e8396
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254618"
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için 3 sürüm notunu güncelleştirin

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncelleştirme 3 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılım güncelleştirmelerinin bir listesini de içerirler. 

Güncelleştirme 3, Sürüm (GA) veya Güncelleme 0.1 ile Güncelleme 2.2 arasında çalıştıran herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 3 ile ilişkili aygıt sürümü 6.3.9600.17759'dur.

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleme 3 cihaz yazılımı, LSI sürücüsü ve firmware ve Storport ve Spaceport güncellemeleri vardır. Bu güncelleştirmeyi yüklemek yaklaşık 1,5-2 saat sürer. 
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu yakında kullanılabilir olacak gibi yeniden güncelleştirmeleri için tarar.
> 
> 

## <a name="whats-new-in-update-3"></a>Güncelleme 3'teki yenilikler
Güncelleştirme 3'te aşağıdaki önemli geliştirmeler ve hata düzeltmeleri yapılmıştır.

* **Otomatik alan ıslahı değişiklikleri** – Güncelleştirme 3'ü başlatma, sistemin bekleme denetleyicisinde çalışan alan ıslah algoritmaları daha hızlı yürütmeyle sonuçlanır. Alan ıslahı ile çalışmak için gereken bağlantı noktaları hakkında daha fazla bilgi için [StorSimple ağ gereksinimlerine](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device)bakın.
* **Performans geliştirmeleri** – Update 3, buluta okuma yazma performansını artırdı.
* **Geçişle ilgili iyileştirmeler** – Bu sürümde, 5000/7000 serisi cihazlardan 8000 seri aygıta Geçiş özelliği için çeşitli hata düzeltmeleri ve iyileştirmeler yapılmıştır. Geçiş özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi [için, 5000/7000 serisi cihazdan 8000 serisi aygıta Geçiş'e](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b)gidin. 
* **İlgili düzeltmelerin izlenmesi** - Bu sürümde, izleme grafikleri, servis panosu ve aygıt panosuyla ilgili hatalar düzeltildi.

## <a name="issues-fixed-in-update-3"></a>Güncelleştirme 3'te düzeltilen sorunlar
Aşağıdaki tablolar Güncelleştirme 3'te düzeltilen sorunların bir özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ana bilgisayar tarafı veri geçişi |Önceki sürümde, StorSimple Cloud Appliance ana bilgisayar tarafı veri geçişi sırasında çevrimdışı gidiyordu. Bu sorun bu sürümde giderilmiştir. |Hayır |Evet |
| 2 |Yerel olarak sabitlenmiş birimler |Önceki sürümde, yerel olarak sabitlenmiş birimler için G/Ç hataları, toplu dönüştürme hataları ve veri yolu hatalarıyla ilgili sorunlar vardı. Bu sorunlar kök kaynaklı ve bu sürümde düzeltildi. |Evet |Hayır |
| 3 |İzleme |Raporlama birimleri ve izlemenin yanı sıra yerel olarak sabitlenmiş birimler için yanlış bilgilerin görüntülendiği aygıt pano grafikleri ile ilgili birden çok sorun vardı. Bu sorunlar bu sürümde giderilmiştir. |Evet |Hayır |
| 4 |Ağır i/O yazıyor |Ağır yazmaiçeren iş yükleri için StorSimple kullanırken, kullanıcı çalışma kümesibulut katmanlı ediliyordu seyrek bir hata ile karşılaştı. Bu hata bu sürümde düzeltilir. |Evet |Evet |
| 5 |Backup |Bazı nadir durumlarda, yazılımın önceki sürümlerinde, kullanıcı uzak bir klonun yedeğini aldığında, bulut hatalarıyla karşılarına çıkar ve işlem hata çıkarır. Bu sürümde, sorun giderilir ve işlem başarıyla tamamlanır. |Evet |Evet |
| 6 |Yedekleme ilkesi |Bazı nadir durumlarda, yazılımın önceki sürümlerinde, yedekleme ilkesinin silinmesi ile ilgili bir hata vardı. Bu sorun bu sürümde giderilmiştir. |Evet |Evet |

## <a name="known-issues-in-update-3"></a>Güncelleştirme 3'te bilinen sorunlar
Aşağıdaki tablo, bu sürümde bilinen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çoğunluğu |Nadir durumlarda, 8600 aygıtın EBOD muhafazasındaki disklerin çoğu kesilirse ve bu da disk yeter sayısı olmadan sonuçlanırsa, depolama havuzu çevrimdışı olur. Diskler yeniden bağlansa bile çevrimdışı kalır. |Aygıtı yeniden başlatmanız gerekir. Sorun devam ederse, lütfen sonraki adımlar için Microsoft Destek'e başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değiştirme yapıldığında, denetleyici 0 denetleyici 1 olarak görünebilir. Denetleyici değiştirme sırasında, görüntü eş düğümünden yüklendiğinde, denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterilebilir. Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra da görülebilir. |Kullanıcı eylemi gerekmez. Bu durum, denetleyici değişimi tamamlandıktan sonra kendiliğinden çözülür. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama hizmetini kullanmak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerinin alınamadığı bir duruma yol açar. | |Evet |Evet |
| 4 |Cihaz arıza |Aynı kaynak aygıttan farklı hedef aygıtlara bir birim kapsayıcının birden fazla failovers desteklenmez. Tek bir ölü aygıttan birden fazla cihaza geçememesi, aygıtın veri sahipliğini kaybetmesi nedeniyle ilk arızalı birim kapsayıcılarına neden olur. Böyle bir başarısızlıktan sonra, bu birim kapsayıcıları Azure klasik portalında görüntülediğinizde farklı görünür veya farklı davranacaktır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple Adaptörü sırasında, yüklemenin başarıyla tamamlanması için bir aygıt IP'si sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy |Web proxy yapılandırmanızda belirtilen protokol olarak HTTPS varsa, aygıttan hizmete iletişiminiz etkilenir ve aygıt çevrimdışı duruma geçer. Destek paketleri de bu süreçte oluşturularak cihazınızda önemli kaynaklar tüketecektir. |Belirtilen protokol olarak web proxy URL'sinin HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-8000-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy |Kayıtlı bir aygıtta web proxy'yi yapılandırıp etkinleştiriseniz, cihazınızdaki etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Yüksek bulut gecikmesi ve yüksek G/Ç iş yükü |StorSimple aygıtınız çok yüksek bulut gecikmeleri (saniye sırası) ve yüksek G/Ç iş yükünün bir birleşimi ile karşılaştığında, aygıt birimleri bozulmuş bir duruma girer ve G/Ç'ler "aygıt hazır değil" hatasıyla başarısız olabilir. |Aygıt denetleyicilerini el ile yeniden başlatmanız veya bu durumdan kurtulmak için bir aygıt başarısızı gerçekleştirmeniz gerekir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object -First 1 -Yeni** bir **VolumeContainer** nesnesi oluşturabilmeniz için ilk nesneyi seçmek için bekleyin, cmdlet tüm nesneleri döndürür. |Cmdlet'i parantez içinde şu şekilde sarın: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object -First 1 -Wait** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcı geçirildiğinde, en son yedekleme için ETA yalnızca ilk birim kapsayıcı için doğrudur. Ayrıca, ilk birim kapsayıcıdaki ilk 4 yedekleme geçirildikten sonra paralel geçiş başlar. |Aynı anda bir birim kapsayıcıgeçirmenizi öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklemeden sonra, birimler yedekleme ilkesine veya sanal disk grubuna eklenmez. |Yedekleme oluşturmak için bu birimleri yedekleme ilkesine eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |Geçiş tamamlandıktan sonra, 5000/7000 serisi aygıt geçirilen veri kapsayıcıları erişmemeli. |Geçiş tamamlandıktan ve işlendikten sonra geçirilen veri kapsayıcılarını silmenizi öneririz. |Evet |Hayır |
| 13 |Klon ve DR |Güncelleştirme 1'i çalıştıran bir StorSimple aygıtı, güncelleştirme öncesi 1 yazılımı çalıştıran bir aygıtta genel durum kurtarma yıkılamaz veya olağanüstü durum kurtarma gerçekleştiremez. |Bu işlemlere izin vermek için hedef aygıtı Güncelleştirme 1'e güncelleştirmeniz gerekir |Evet |Evet |
| 14 |Geçiş |Geçiş için yapılandırma yedeklemesi, ilişkili birimleri olmayan birim grupları olduğunda 5000-7000 serisi bir aygıtta başarısız olabilir. |İlişkili birim olmadan tüm boş birim gruplarını silin ve yapılandırma yedeklemesini yeniden deneyin. |Evet |Hayır |
| 15 |Azure PowerShell cmdlets ve yerel olarak sabitlenmiş hacimler |Azure PowerShell cmdlets üzerinden yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell üzerinden oluşturduğunuz tüm birim katmanlanır.) |Yerel olarak sabitlenmiş birimleri yapılandırmak için StorSimple Manager hizmetini her zaman kullanın. |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimler için kullanılabilir alan |Yerel olarak sabitlenmiş bir birimi silerseniz, yeni birimler için kullanılabilir alan hemen güncelleştirilemeyebilir. StorSimple Manager hizmeti, yaklaşık her saat kullanılabilen yerel alanı güncelleştirir. |Yeni birim oluşturmaya çalışmadan önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimler |Geri yükleme işiniz, Yedekleme Kataloğu'ndaki geçici anlık görüntü yedeklemesini, ancak yalnızca geri yükleme işi süresince ortaya çıkarır. Ayrıca, **Yedekleme İlkeleri** sayfasında **tmpCollection** öneki ile sanal bir disk grubu ortaya çıkarır, ancak yalnızca geri yükleme işi süresince. |Geri yükleme işinizyalnızca yerel olarak sabitlenmiş birimlere veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımına sahipse, bu davranış oluşabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış oluşmaz. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimler |Bir geri yükleme işini iptal ederseniz ve denetleyici nin başarısızlığı hemen ardından gerçekleşirse, geri yükleme işi **İptal Yerine** **Başarısız'ı** gösterir. Geri yükleme işi başarısız olursa ve denetleyici nin hemen ardından başarısız olursa, geri yükleme işi **Başarısız**yerine **İptal edilmiş** olarak gösterecektir. |Geri yükleme işinizyalnızca yerel olarak sabitlenmiş birimlere veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımına sahipse, bu davranış oluşabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış oluşmaz. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimler |Geri yükleme işini iptal ederseniz veya geri yükleme başarısız olursa ve ardından bir denetleyici başarısız olursa, **İşler** sayfasında ek bir geri yükleme işi görüntülenir. |Geri yükleme işinizyalnızca yerel olarak sabitlenmiş birimlere veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımına sahipse, bu davranış oluşabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış oluşmaz. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimler |Katmanlı bir birimi (Güncelleştirme 1.2 veya daha öncesiyle oluşturulmuş ve klonlanmış) yerel olarak sabitlenmiş bir ses e dönüştürmeye çalışırsanız ve cihazınızda yer bitiyorsa veya bir bulut kesintisi varsa, klon(lar) bozulabilir. |Bu sorun yalnızca Güncelleştirme öncesi 2.1 yazılımıyla oluşturulan ve klonlanan birimlerde oluşur. Bu seyrek bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Birim dönüştürme devam ederken bir birime eklenen AcR'ları güncelleştirin (yerel olarak sabitlenmiş veya tam tersi katmanlı). ARG'ların güncellenmesi veri bozulmasına neden olabilir. |Gerekirse, birim dönüştürmeden önce AC'leri güncelleştirin ve dönüştürme devam ederken başka ACR güncelleştirmesi yapmayın. | | |
| 22 |Güncelleştirmeler |Azure klasik portalındaki **Bakım** sayfası, Güncelleştirme 2 ile ilgili aşağıdaki iletiyi görüntüler : "StorSimple 8000 serisi Güncelleştirme 2, Microsoft'un olası sorunları tespit ettiğimizde cihazınızdan günlük bilgilerini proaktif olarak toplama özelliğini içerir". Aygıtın Güncelleştirme 2 olarak güncelleştirildiğini gösterdiği nden yanıltıcıdır. Aygıt Güncelleştirme 3'e başarıyla güncelleştirildikten sonra bu ileti kaybolur. |Bu davranış, gelecekteki bir sürümde düzeltilecektir. |Evet |Hayır |

## <a name="controller-and-firmware-updates-in-update-3"></a>Güncelleme 3'te denetleyici ve firmware güncellemeleri
Bu sürümde LSI sürücüsü ve firmware güncelleştirmeleri vardır. LSI sürücüsü ve firmware güncelleştirmelerinin nasıl yüklenirhakkında daha fazla bilgi için StorSimple cihazınıza [Update 3](storsimple-install-update-3.md) yükleyin'e bakın.

## <a name="virtual-device-updates-in-update-3"></a>Güncelleştirme 3'te sanal cihaz güncellemeleri
Bu güncelleştirme StorSimple Cloud Appliance 'a (sanal aygıt olarak da bilinir) uygulanamaz. Yeni sanal aygıtların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Update 3'u StorSimple cihazınıza nasıl [yükleyin](storsimple-install-update-3.md) öğrenin.

