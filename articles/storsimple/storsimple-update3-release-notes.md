---
title: StorSimple 8000 serisi güncelleştirme 3 sürüm notları
description: StorSimple 8000 serisi güncelleştirme 3 için yeni özellikleri, sorunları ve geçici çözümleri açıklar.
author: alkohli
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b61caecd67881eb08c82ea0c26522c63c3e8396
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79254618"
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için güncelleştirme 3 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri anlatmaktadır ve StorSimple 8000 serisi güncelleştirme 3 için kritik açık sorunları belirler. Ayrıca, bu sürüme dahil olan StorSimple yazılım güncelleştirmelerinin bir listesini içerir. 

Güncelleştirme 3, sürüm (GA) çalıştıran tüm StorSimple cihazlarına veya güncelleştirme 2,2 aracılığıyla 0,1 güncelleştirmesine uygulanabilir. Güncelleştirme 3 ile ilişkili cihaz sürümü 6.3.9600.17759.

Güncelleştirmeyi StorSimple çözümünüzde dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleştirme 3 ' te cihaz yazılımı, LSI sürücü ve üretici yazılımı ve Storport ve Spaceport güncelleştirmeleri vardır. Bu güncelleştirmeyi yüklemek yaklaşık 1,5-2 saat sürer. 
> * Güncelleştirmelerin aşamalı olarak piyasaya sürültiğimiz için yeni sürümler için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu süre yakında kullanılabilir hale gelecağından güncelleştirmeleri yeniden tarayın.
> 
> 

## <a name="whats-new-in-update-3"></a>Güncelleştirme 3 ' teki yenilikler
Güncelleştirme 3 ' te aşağıdaki önemli geliştirmeler ve hata düzeltmeleri yapılmıştır.

* **Otomatik Space geri kazanma değişiklikleri** – güncelleştirme 3 ' ten başlayarak, sistem için bekleyen denetleyicide çalıştırılan alan geri kazanma algoritmaları daha hızlı yürütmeye neden olur. Space geri kazanma ile çalışmak için gereken bağlantı noktaları hakkında daha fazla bilgi için, [StorSimple ağ gereksinimlerine](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device)bakın.
* **Performans iyileştirmeleri** – güncelleştirme 3, buluta okuma yazma performansını geliştirmiştir.
* **Geçişle ilgili geliştirmeler** – bu sürümde, 5000/7000 serisi cihazlardan 8000 serisi cihazlara geçiş özelliği için çeşitli hata düzeltmeleri ve geliştirmeler yapılmıştır. Geçiş özelliğini kullanma hakkında daha fazla bilgi için, [5000/7000 Serisi cihazdan 8000 serisi cihaza geçiş](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b)konusuna gidin. 
* **İlgili düzeltmeler izleniyor** -bu sürümde, izleme grafikleriyle ilgili hatalar, hizmet panosu ve cihaz panosu düzeltildi.

## <a name="issues-fixed-in-update-3"></a>Güncelleştirme 3 ' te düzeltilen sorunlar
Aşağıdaki tablolarda güncelleştirme 3 ' te düzeltilen sorunların özeti verilmiştir.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Konak tarafı veri geçişi |Önceki sürümde, ana bilgisayar tarafı veri geçişi sırasında StorSimple Cloud Appliance çevrimdışına gidiyor. Bu sorun bu sürümde düzeltilmiştir. |Hayır |Yes |
| 2 |Yerel olarak sabitlenmiş birimler |Önceki sürümde, yerel olarak sabitlenmiş birimler için g/ç hatalarıyla, birim dönüştürme hatalarıyla ve veri yolu hatalarıyla ilgili sorunlar vardı. Bu sorunlar köklenmiştir ve bu sürümde düzeltildi. |Yes |Hayır |
| 3 |İzleme |Raporlama birimleri ve izleme ile ilgili birden çok sorun ve yerel olarak sabitlenmiş birimlerde yanlış bilgilerin görüntülendiği cihaz panosu grafikleri. Bu sorunlar bu sürümde düzeltilmiştir. |Yes |Hayır |
| 4 |Ağır yazar g/ç |Yoğun yazma işlemleri içeren iş yükleri için StorSimple kullanırken, Kullanıcı, çalışma kümesinin buluta katmanlanmakta olduğu seyrek bir hata halinde çalışır. Bu hata bu sürümde düzeltilmiştir. |Yes |Yes |
| 5 |Backup |Nadir bazı örneklerde, yazılımın önceki sürümlerinde, Kullanıcı uzak bir kopyanın yedeklemesini gerçekleştirdiklerinde bulut hatalarıyla çalıştırılır ve işlem hata vermez. Bu sürümde, sorun düzeltildi ve işlem başarıyla tamamlanır. |Yes |Yes |
| 6 |Yedekleme ilkesi |Nadir bazı örneklerde, yazılımın önceki sürümlerinde, yedekleme ilkesinin silinmesinden ilgili bir hata oluştu. Bu sorun bu sürümde düzeltilmiştir. |Yes |Yes |

## <a name="known-issues-in-update-3"></a>Güncelleştirme 3 ' te bilinen sorunlar
Aşağıdaki tabloda, bu sürümdeki bilinen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun | Açıklamalar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdeği |Nadir örneklerde, bir 8600 cihazının EBOD muhafazasının çoğunluğunun bağlantısı kesildiyse, disk çekirdeği olmadığında depolama havuzu çevrimdışı olur. Diskler yeniden bağlansa bile çevrimdışı olarak kalır. |Cihazı yeniden başlatmanız gerekir. Sorun devam ederse, sonraki adımlar için lütfen Microsoft Desteği başvurun. |Yes |Hayır |
| 2 |Yanlış denetleyici KIMLIĞI |Bir denetleyici değişikliği gerçekleştirildiğinde, denetleyici 0, denetleyici 1 olarak görünebilir. Denetleyici değişikliği sırasında, görüntü eş düğümünden yüklendiğinde, denetleyici KIMLIĞI başlangıçta eş denetleyicinin KIMLIĞI olarak gösterilir. Nadir örneklerde, bu davranış sistem yeniden başlatıldıktan sonra da görünebilir. |Başka bir kullanıcı eylemi gerekmiyor. Bu durum, denetleyici değiştirme işlemi tamamlandıktan sonra kendisini çözmeyecektir. |Yes |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için depolama hizmetinin kullanılması desteklenmeyen bir senaryodur. Bu, Kullanıcı verilerinin alınamadığı bir duruma yol açacaktır. | |Yes |Yes |
| 4 |Cihaz yük devretmesi |Aynı kaynak cihazdan farklı hedef cihazlara bir birim kapsayıcısının çoklu yük devretme işlemleri desteklenmez. Tek bir ölü cihazdan birden çok cihaza yük devretme, ilk yük devredilen cihazdaki birim kapsayıcılarının veri sahipliğini kaybetmesine izin vermez. Bu tür bir yük devretmeden sonra, bu birim kapsayıcıları, klasik Azure portalında görüntülediğinizde görünür veya farklı davranır. | |Yes |Hayır |
| 5 |Yükleme |SharePoint için StorSimple Bağdaştırıcısı yüklemesi sırasında, yüklemenin başarıyla tamamlanabilmesi için bir cihaz IP 'si sağlamanız gerekir. | |Yes |Hayır |
| 6 |Web ara sunucusu |Web proxy yapılandırmanızda belirtilen protokol olarak HTTPS varsa, cihazdan hizmete iletişim etkilenecektir ve cihaz çevrimdışı olur. Destek paketleri, cihazınızda önemli kaynaklar tüketen işlem sırasında da oluşturulacaktır. |Web proxy URL 'sinin belirtilen protokol olarak HTTP içerdiğinden emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-8000-configure-web-proxy.md)’ya gidin. |Yes |Hayır |
| 7 |Web ara sunucusu |Kayıtlı bir cihazda Web proxy 'yi yapılandırıp etkinleştirirseniz, cihazınızda etkin denetleyiciyi yeniden başlatmanız gerekir. | |Yes |Hayır |
| 8 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple cihazınız çok yüksek bulut gecikme sürelerinin (saniye cinsinden) ve yüksek g/ç iş yükünün bir birleşimiyle karşılaştığında, cihaz birimleri düşürülmüş bir duruma geçer ve g/ç "cihaz için yok" hatasıyla başarısız olabilir. |Bu durumdan kurtarmak için cihaz denetleyicilerini el ile yeniden başlatmanız veya cihaz yük devretmesi gerçekleştirmeniz gerekir. |Yes |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet 'ini ( **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object-First 1** ) kullandığınızda, yeni bir **volumecontainer** nesnesi oluşturabilmeniz için ilk nesneyi seçmek için bekleyin, cmdlet tüm nesneleri döndürür. |Cmdlet 'i parantez içinde şu şekilde kaydırın: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object-First 1-wait** |Yes |Yes |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcısı geçirildiğinde, en son yedekleme için ETA yalnızca ilk birim kapsayıcısı için doğrudur. Ayrıca, ilk birim kapsayıcıdaki ilk 4 yedekleme geçirildikten sonra paralel geçiş başlar. |Tek seferde bir birim kapsayıcısını geçirmeniz önerilir. |Yes |Hayır |
| 11 |Geçiş |Geri yükleme işleminden sonra birimler yedekleme ilkesine veya sanal disk grubuna eklenmez. |Yedeklemeler oluşturmak için bu birimleri bir yedekleme ilkesine eklemeniz gerekir. |Yes |Yes |
| 12 |Geçiş |Geçiş işlemi tamamlandıktan sonra, 5000/7000 Serisi cihaz geçirilmiş veri kapsayıcılarına erişmemelidir. |Geçiş tamamlandıktan ve kaydedildikten sonra geçirilmiş veri kapsayıcılarını silmenizi öneririz. |Yes |Hayır |
| 13 |Kopyala ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazı, güncelleştirme öncesi 1 yazılımını çalıştıran bir cihaza olağanüstü durum kurtarması kopyalayamıyor veya gerçekleştiremiyor. |Bu işlemlere izin vermek için 1 öğesini güncelleştirmek üzere hedef cihazı güncelleştirmeniz gerekir |Yes |Yes |
| 14 |Geçiş |İlişkili birimleri olmayan birim grupları varsa, geçiş için yapılandırma yedeklemesi 5000-7000 Serisi cihazda başarısız olabilir. |İlişkili birimleri olmayan tüm boş birim gruplarını silip yapılandırma yedeklemesini yeniden deneyin. |Yes |Hayır |
| 15 |Azure PowerShell cmdlet 'leri ve yerel olarak sabitlenmiş birimler |Azure PowerShell cmdlet 'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell aracılığıyla oluşturduğunuz tüm birimler katmanlı olacaktır.) |Yerel olarak sabitlenmiş birimleri yapılandırmak için her zaman StorSimple Yöneticisi hizmetini kullanın. |Yes |Hayır |
| 16 |Yerel olarak sabitlenmiş birimler için kullanılabilir alan |Yerel olarak sabitlenmiş bir birimi silerseniz, yeni birimler için kullanılabilir alan hemen güncelleştirilmemiş olabilir. StorSimple Yöneticisi hizmeti, her saat yaklaşık olarak kullanılabilir yerel alanı güncelleştirir. |Yeni birimi oluşturmayı denemeden önce bir saat bekleyin. |Yes |Hayır |
| 17 |Yerel olarak sabitlenmiş birimler |Geri yükleme işiniz, yedekleme kataloğunda geçici anlık görüntü yedeklemesini, ancak yalnızca geri yükleme işinin süresini gösterir. Ayrıca, **yedekleme ilkeleri** sayfasında, ancak geri yükleme işinin süresi boyunca **tmpcollection** ön ekine sahip bir sanal disk grubu gösterir. |Bu davranış, geri yükleme işiniz yalnızca yerel olarak sabitlenmiş birimler veya yerel olarak sabitlenmiş ve katmanlı birimler karışımı içeriyorsa meydana gelebilir. Geri yükleme işi yalnızca katmanlı birimler içeriyorsa, bu davranış gerçekleşmeyecektir. Kullanıcı müdahalesi gerekli değildir. |Yes |Hayır |
| 18 |Yerel olarak sabitlenmiş birimler |Bir geri yükleme işini iptal ederseniz ve daha sonra bir denetleyici yük devretmesi gerçekleşirse, geri yükleme işi **iptal**etme yerine **başarısız** olarak gösterilir. Geri yükleme işi başarısız olursa ve denetleyici yük devretmesi hemen sonra gerçekleşirse, geri yükleme işi **başarısız**yerine **iptal** edilir. |Bu davranış, geri yükleme işiniz yalnızca yerel olarak sabitlenmiş birimler veya yerel olarak sabitlenmiş ve katmanlı birimler karışımı içeriyorsa meydana gelebilir. Geri yükleme işi yalnızca katmanlı birimler içeriyorsa, bu davranış gerçekleşmeyecektir. Kullanıcı müdahalesi gerekli değildir. |Yes |Hayır |
| 19 |Yerel olarak sabitlenmiş birimler |Bir geri yükleme işini iptal ederseniz veya geri yükleme başarısız olursa ve sonra bir denetleyici yük devretmesi oluşursa, **işler** sayfasında ek bir geri yükleme işi görüntülenir. |Bu davranış, geri yükleme işiniz yalnızca yerel olarak sabitlenmiş birimler veya yerel olarak sabitlenmiş ve katmanlı birimler karışımı içeriyorsa meydana gelebilir. Geri yükleme işi yalnızca katmanlı birimler içeriyorsa, bu davranış gerçekleşmeyecektir. Kullanıcı müdahalesi gerekli değildir. |Yes |Hayır |
| 20 |Yerel olarak sabitlenmiş birimler |Katmanlı bir birimi (güncelleştirme 1,2 veya önceki bir sürümü) yerel olarak sabitlenmiş bir birime dönüştürmeye çalışırsanız ve cihazınızda alan tükenmişse veya bir bulut kesintisi yaşanırsa, kopya (ler) bozulmuş olabilir. |Bu sorun yalnızca güncelleştirme öncesi 2,1 yazılımıyla oluşturulan ve kopyalanan birimlerde meydana gelir. Bu, seyrek görülen bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Birim dönüştürme devam ederken bir birime bağlı olan ACRs 'yi güncelleştirmeyin (yerel olarak sabitlenmiş veya tam tersi). ACRs 'nin güncelleştirilmesi veri bozulmasına neden olabilir. |Gerekirse, birim dönüşümden önce ACRs 'yi güncelleştirin ve dönüştürme devam ederken daha fazla ACR güncelleştirmesi yapmayın. | | |
| 22 |Güncelleştirmeler |Güncelleştirme 3 ' ü uygularken, klasik Azure portalındaki **bakım** sayfasında güncelleştirme 2-"storsimple 8000 serisi güncelleştirme 2 ile ilgili aşağıdaki ileti görüntülenir. Bu, olası sorunları algılamamız durumunda Microsoft 'un cihazınızdaki günlük bilgilerini proaktif olarak toplamasını sağlar. Bu, cihazın güncelleştirme 2 ' ye güncelleştirildiğini gösterdiği için yanıltıcı olur. Cihaz güncelleştirme 3 ' e başarıyla güncelleştirildikten sonra bu ileti kaybolur. |Bu davranış gelecekteki bir sürümde düzeltilecektir. |Yes |Hayır |

## <a name="controller-and-firmware-updates-in-update-3"></a>Güncelleştirme 3 ' te denetleyici ve bellenim güncelleştirmeleri
Bu sürümde, LSI sürücü ve bellenim güncelleştirmeleri vardır. LSI sürücü ve bellenim güncelleştirmelerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz. StorSimple cihazınıza [güncelleştirme 3](storsimple-install-update-3.md) ' ü yüklemek.

## <a name="virtual-device-updates-in-update-3"></a>Güncelleştirme 3 ' teki sanal cihaz güncelleştirmeleri
Bu güncelleştirme StorSimple Cloud Appliance uygulanamıyor (sanal cihaz olarak da bilinir). Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
StorSimple cihazınıza [güncelleştirme 3 ' ü yüklemeyi](storsimple-install-update-3.md) öğrenin.

