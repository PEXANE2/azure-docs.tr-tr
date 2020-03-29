---
title: StorSimple 8000 Serisi Güncelleme 2.2 sürüm notları | Microsoft Dokümanlar
description: StorSimple 8000 Serisi Güncelleştirmesi 2.2'nin yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 12d11cddf077d4d07732490255d44e89ddaf3217
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60531089"
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 Serisi Güncelleme 2.2 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncelleştirme 2.2 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılım güncelleştirmelerinin bir listesini de içerirler.

Güncelleştirme 2.2, Sürüm (GA) veya Güncelleme 0.1 ile Güncelleme 2.1'i çalıştıran herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 2.2 ile ilişkili aygıt sürümü 6.3.9600.17708'dir.

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Update 2.2 yalnızca yazılım güncelleştirmeleri vardır. Bu güncelleştirmeyi yüklemek yaklaşık 1,5-2 saat sürer. 
> * Güncelleştirme 2.1'i çalıştırıyorsanız, Güncelleştirme 2.2'yi mümkün olan en kısa sürede uygulamanızı öneririz.
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu yakında kullanılabilir olacak gibi yeniden güncelleştirmeleri için tarar.
> 
> 

## <a name="whats-new-in-update-22"></a>Güncelleme 2.2'deki yenilikler
Güncelleştirme 2.2'de aşağıdaki önemli iyileştirmeler yapılmıştır.

* **Otomatik alan ıslahı optimizasyonu** – Veriler ince olarak sağlanan birimlerde silindiğinde, kullanılmayan depolama bloklarının geri alınması gerekir. Bu sürüm, buluttan gelen alan ıslah işlemini iyileştirerek kullanılmayan alanın önceki sürümlere göre daha hızlı kullanılabilir hale gelmesine neden olmuştur.
* **Anlık görüntü performans geliştirmeleri** – Update 2.2, büyük hacimlerin kullanıldığı ve veri karmaşasının en az olduğu belirli senaryolarda bulut anlık görüntüsünü işleme süresini artırdı. Bu geliştirmeden yararlanacak bir senaryo arşiv birimleri olacaktır.
* **Destek paketinin toplanmasının sertleştirilmesi** – Destek paketinin bu sürümde toplanma ve yükleme şeklinde iyileştirmeler yapılmıştır. 
* **Güvenilirlik iyileştirmelerini güncelleştir** – Bu sürümde, daha iyi güncelleştirme güvenilirliğiyle sonuçlanan hata düzeltmeleri vardır.

## <a name="issues-fixed-in-update-22"></a>Güncelleştirme 2.2'de düzeltilen sorunlar
Aşağıdaki tablolar Güncelleştirmeler 2.2 ve 2.1'de düzeltilen sorunların bir özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ev sahibi performansı |Önceki sürümde, yerel olarak sabitlenmiş bir birimin oluşturulması sırasında ve katmanlı bir birimin yerel olarak sabitlenmiş bir sese dönüştürülmesi sırasında ana bilgisayar tarafı performans sorunları gözlenmiştir. Bu sorunlar bu sürümde giderilir ve böylece birim oluşturma ve dönüştürme yordamları sırasında ana bilgisayar performansında bir iyileşme ye neden olabilir. |Evet |Hayır |
| 2 |Yerel olarak sabitlenmiş birimler |Nadir durumlarda, sistem yerel olarak sabitlenmiş bir ses düzeyi oluştururken çöker. Bu hata bu sürümde düzeltildi. |Evet |Hayır |
| 3 |Katmanlama |StorSimple Cloud Appliances (8010 ve 8020) meta verileri buluta katmanlandığında düzensiz çökmeler yaşandı. Bu sorun bu sürümde giderilmiştir. |Hayır |Evet |
| 4 |Anlık görüntü oluşturma |Büyük hacimli ve en az ve hiçbir veri karmaşası ile senaryolarda artımlı anlık görüntü oluşturulması ile ilgili sorunlar vardı. Bu sorunlar bu sürümde giderilmiştir. |Evet |Evet |
| 5 |Openstack kimlik doğrulaması |Openstack'i bulut hizmeti sağlayıcısı olarak kullanırken, kullanıcı JSON parer'inin kilitlenmeyle sonuçlandığı kimlik doğrulamasıyla ilgili seyrek bir hatayla karşılaştı. Bu hata bu sürümde düzeltilir. |Evet |Hayır |
| 6 |Ana bilgisayar tarafı kopyası |Yazılımın önceki sürümlerinde, verileri bir birimden başka bir birimden kopyalarken ODX zamanlaması ile ilgili seyrek bir hata görülmüştür. Bu bir denetleyici başarısız lığa neden olur ve sistem potansiyel kurtarma moduna gidebilir. Bu hata bu sürümde düzeltilir. |Evet |Hayır |
| 7 |Windows Yönetim Araçları (WMI) |Yazılımın önceki sürümlerinde, "ManagementException> Sağlayıcı yük hatası"\<dışında web proxy arızası birkaç örneği vardı. Bu hata bir WMI bellek sızıntısı atfedilen ve şimdi sabittir. |Evet |Hayır |
| 8 |Güncelleştirme |Bazı nadir durumlarda, yazılımın önceki sürümlerinde, kullanıcı güncellemeleri tatmaya veya yüklemeye çalışırken bir "CisPowershellHcsscripterror" aldı. Bu sorun bu sürümde giderilmiştir. |Evet |Evet |
| 9 |Destek paketi |Bu sürümde, Destek paketinin toplanma ve yüklenme şeklinde iyileştirmeler yapılmıştır. |Evet |Evet |

## <a name="known-issues-in-update-22"></a>Güncelleştirme 2.2'de bilinen sorunlar
Aşağıdaki tablo, bu sürümde bilinen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çoğunluğu |Nadir durumlarda, 8600 aygıtın EBOD muhafazasındaki disklerin çoğu kesilirse ve bu da disk yeter sayısı olmadan sonuçlanırsa, depolama havuzu çevrimdışı olur. Diskler yeniden bağlansa bile çevrimdışı kalır. |Aygıtı yeniden başlatmanız gerekir. Sorun devam ederse, lütfen sonraki adımlar için Microsoft Destek'e başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değiştirme yapıldığında, denetleyici 0 denetleyici 1 olarak görünebilir. Denetleyici değiştirme sırasında, görüntü eş düğümünden yüklendiğinde, denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterilebilir. Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra da görülebilir. |Kullanıcı eylemi gerekmez. Bu durum, denetleyici değişimi tamamlandıktan sonra kendiliğinden çözülür. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama hizmetini kullanmak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerinin alınamadığı bir duruma yol açar. | |Evet |Evet |
| 4 |Cihaz arıza |Aynı kaynak aygıttan farklı hedef aygıtlara bir birim kapsayıcının birden fazla failovers desteklenmez. Tek bir ölü aygıttan birden fazla cihaza geçememesi, aygıtın veri sahipliğini kaybetmesi nedeniyle ilk arızalı birim kapsayıcılarına neden olur. Böyle bir başarısızlıktan sonra, bu birim kapsayıcıları Azure klasik portalında görüntülediğinizde farklı görünür veya farklı davranacaktır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple Adaptörü sırasında, yüklemenin başarıyla tamamlanması için bir aygıt IP'si sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy |Web proxy yapılandırmanızda belirtilen protokol olarak HTTPS varsa, aygıttan hizmete iletişiminiz etkilenir ve aygıt çevrimdışı duruma geçer. Destek paketleri de bu süreçte oluşturularak cihazınızda önemli kaynaklar tüketecektir. |Belirtilen protokol olarak web proxy URL'sinin HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
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

## <a name="controller-and-firmware-updates-in-update-22"></a>Güncelleştirme 2.2'deki denetleyici ve firmware güncellemeleri
Bu sürümde yalnızca yazılım güncelleştirmeleri vardır. Ancak, Güncelleştirme 2'den önceki bir sürümden güncelliyorsanız, cihazınıza sürücü, Storport, Spaceport ve (bazı durumlarda) disk firmware güncelleştirmelerini yüklemeniz gerekir.

Sürücü, Storport, Spaceport ve disk firmware güncelleştirmelerini nasıl yükleyebilirsiniz hakkında daha fazla bilgi için StorSimple cihazınıza [Güncelleme 2.2'yi](storsimple-install-update-21.md) yükleyin'e bakın.

## <a name="virtual-device-updates-in-update-22"></a>Güncelleştirme 2.2'deki sanal cihaz güncellemeleri
Bu güncelleştirme sanal aygıta uygulanamaz. Yeni sanal aygıtların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
StorSimple cihazınıza [Update 2.2'yi](storsimple-install-update-21.md) nasıl yükleyin öğrenin.

