---
title: StorSimple 8000 Serisi Güncelleme 1.2 sürüm notları | Microsoft Dokümanlar
description: StorSimple 8000 Serisi Güncelleştirmesi 1.2'nin yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11138857e33eec0f854ddb61956ea24c858c49a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60531011"
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için 1.2 sürüm notlarını güncelleştirin

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncelleştirme 1.2 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılımı, sürücü ve disk firmware güncelleştirmelerinin bir listesini de içerir. 

Güncelleştirme 1.2, Sürüm (GA), Update 0.1, Update 0.2 veya Update 0.3 yazılımlarını çalıştıran herhangi bir StorSimple aygıtına uygulanabilir. Aygıtınız Güncelleştirme 1 veya Güncelleştirme 1.1'i çalıştırıyorsa Güncelleştirme 1.2 kullanılamıyor. Aygıtınız Sürüm (GA) çalıştırıyorsa, bu güncelleştirmeyi yüklemenize yardımcı olması için lütfen [Microsoft Destek'e başvurun.](storsimple-contact-microsoft-support.md)

Aşağıdaki tabloda Güncelleştirme1, 1.1 ve 1.2'ye karşılık gelen aygıt yazılımı sürümleri listelenir.

| Eğer güncelleme çalışıyor ... | bu, aygıt yazılım sürümünüzdür. |
| --- | --- |
| Güncelleştirme 1.2 |6.3.9600.17584 |
| Güncelleştirme 1.1 |6.3.9600.17521 |
| Güncelleme 1.0 |6.3.9600.17491 |

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin. Daha fazla bilgi için [StorSimple cihazınızda Güncelleştirme 1.2'yi nasıl yükleyebilirsiniz.](storsimple-install-update-1.md) 

> [!IMPORTANT]
> * Bu güncelleştirmeyi (Windows Güncelleştirmeleri dahil) yüklemek yaklaşık 5-10 saat sürer. 
> * Güncelleme 1.2 yazılım, LSI sürücüsü ve disk firmware güncellemeleri vardır. Yüklemek için [StorSimple cihazınızda Güncelleme 1.2'yi yükleyin](storsimple-install-update-1.md)yönergeleriizleyin.
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Bu yakında kullanılabilir olacak gibi birkaç gün içinde tekrar güncellemeleri için tarar.
> 
> 

## <a name="whats-new-in-update-12"></a>Güncelleme 1.2'deki yenilikler
Bu özellikler ilk olarak sınırlı sayıda kullanıcıtarafından kullanıma sunulan Update 1 ile yayımlandı. Update 1.2 sürümü ile, StorSimple kullanıcılarının çoğu aşağıdaki yeni özellikleri ve gelişmeleri görür:

* **5000-7000 serisinden 8000 serisi cihazlara geçiş** – Bu sürüm, StorSimple 5000-7000 serisi cihaz kullanıcılarının verilerini StorSimple 8000 serisi fiziksel cihaza veya sanal cihaza geçirmelerine olanak tanıyan yeni bir geçiş özelliği sunar. Geçiş özelliğinin iki temel değer önerisi vardır:                                                                  
  
  * **İş sürekliliği,** 5000-7000 serisi cihazlardaki mevcut verilerin 8000 serisi cihazlara geçişini sağlayarak.
  * StorSimple Manager hizmeti, daha iyi donanım ve güncelleştirilmiş firmware sınıfı, sanal cihazlar, veri mobilitesi ve gelecekteki yol haritasındaki özellikler aracılığıyla birden fazla cihaz verimli merkezi yönetimi gibi **8000 serisi cihazların geliştirilmiş özellik teklifleri.**
    
    StorSimple 5000-7000 serisinin 8000 serisi bir aygıta nasıl geçirilir hakkında ayrıntılar için [geçiş kılavuzuna](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b) bakın. 
* **Azure Kamu Portalı'ndaki kullanılabilirlik** - StorSimple artık Azure Kamu portalında kullanılabilir. [Azure Genel Portalı'nda bir StorSimple aygıtının nasıl dağıtılabildiğini](storsimple-deployment-walkthrough-gov.md)görün.
* **Diğer bulut hizmeti sağlayıcıları için destek** – Desteklenen diğer bulut hizmeti sağlayıcıları Amazon S3, RRS, HP ve OpenStack (beta) ile Amazon S3 vardır.
* **En son Depolama API'lerine güncelleme** – Bu sürümle StorSimple, en son Azure Depolama hizmeti API'lerine güncelleştirildi. Güncelleştirme öncesi 1 yazılım sürümlerini (Sürüm, 0.1, 0.2 ve 0.3) çalıştıran StorSimple 8000 serisi aygıtlar, Azure Depolama Hizmeti API'lerinin 17 Temmuz 2009'dan önceki sürümlerini kullanır. [Depolama hizmeti sürümlerinin kaldırılmasıyla ilgili](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)güncelduyuruda belirtildiği gibi , 1 Ağustos 2016'ya kadar, bu API'ler amortismana kalınacaktır. StorSimple 8000 Serisi Güncelleme 1'i 1 Ağustos 2016 tarihinden önce uygulamanız zorunludur. Bunu yapmazsanız, StorSimple aygıtları düzgün çalışmayı durdurur.
* **Bölge Yedekli Depolama (ZRS) desteği** – Depolama API'lerinin en son sürümüne yükseltilirken, StorSimple 8000 serisi Yerel Yedekli Depolama (LRS) ve Coğrafi yedekli Depolamaya (GRS) ek olarak Bölge Yedekli Depolamayı (ZRS) destekleyecektir. ZRS ayrıntıları için [Azure Depolama artıklığı seçenekleri yle ilgili](../storage/common/storage-redundancy.md) bu makaleye bakın.
* **Gelişmiş ilk dağıtım ve güncelleştirme deneyimi** – Bu sürümde yükleme ve güncelleştirme işlemleri geliştirilmiştir. Ağ yapılandırması ve güvenlik duvarı ayarları yanlışsa, kurulum sihirbazı aracılığıyla yükleme kullanıcıya geri bildirim sağlamak için geliştirilmiştir. Aygıtın sorun giderme ağ konusunda size yardımcı olmak için ek tanılama cmdletleri sağlanmıştır. Sorun giderme için kullanılan yeni tanılama cmdletleri hakkında daha fazla bilgi için [sorun giderme dağıtım makalesine](storsimple-troubleshoot-deployment.md) bakın.

## <a name="issues-fixed-in-update-12"></a>Güncelleştirme 1.2'de düzeltilen sorunlar
Aşağıdaki tablo, Güncelleştirmeler 1.2, 1.1 ve 1'de düzeltilen sorunların bir özetini sağlar.    

| Hayır. | Özellik | Sorun | Güncelleştirme'de düzeltildi | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |StorSimple için Windows PowerShell |Bir kullanıcı StorSimple için Windows PowerShell'i kullanarak StorSimple cihazına uzaktan erişip kurulum sihirbazını başlattığında, Data 0 IP girişi girer girmez bir kilitlenme meydana geldi. Bu hata artık Güncelleştirme 1'de düzeltildi. |Güncelleştirme 1 |Evet |Evet |
| 2 |Fabrika sıfırlaması |Bazı durumlarda, bir fabrika sıfırlama gerçekleştirdiğinizde, StorSimple aygıtı sıkışmış oldu ve bu mesajı görüntülenen: **Fabrikaya sıfırlama devam ediyor (faz 8)**. Cmdlet devam ederken CTRL+C tuşuna bastıysanız bu oldu. Bu hata artık düzeltildi. |Güncelleştirme 1 |Evet |Hayır |
| 3 |Fabrika sıfırlaması |Başarısız bir çift denetleyici fabrika sıfırlama sonra, aygıt kaydı ile devam etmek için izin verildi. Bu, desteklenmeyen bir sistem yapılandırması ile sonuçlandı. Güncelleştirme 1'de bir hata iletisi gösterilir ve başarısız fabrika sıfırlaması olan bir aygıtta kayıt engellenir. |Güncelleştirme 1 |Evet |Hayır |
| 4 |Fabrika sıfırlaması |Bazı durumlarda, yanlış pozitif uyumsuzluk uyarıları yükseltildi. Güncelleştirme 1 çalıştıran aygıtlarda artık yanlış uyumsuzluk uyarıları oluşturulmayacak. |Güncelleştirme 1 |Evet |Hayır |
| 5 |Fabrika sıfırlaması |Bir fabrika sıfırlaması tamamlanmadan önce kesintiye uğradıysa, aygıt kurtarma moduna girdi ve StorSimple için Windows PowerShell'e erişmenize izin vermedi. Bu hata artık düzeltildi. |Güncelleştirme 1 |Evet |Hayır |
| 6 |Olağanüstü durum kurtarma |Bir olağanüstü durum kurtarma (DR) hata dr hedef cihazda yedeklerin keşfi sırasında başarısız olacağını düzeltildi. |Güncelleştirme 1 |Evet |Evet |
| 7 |LED'lerin izlenmesi |Bazı durumlarda, cihazın arka sındaki LED'lerin izlenmesi doğru durumu belirtmedi. Mavi LED kapalıydı. DATA 0 ve DATA 1 LED'ler bu arabirimler yapılandırılmamadığında bile yanıp sönüyorlardı. Sorun giderildi ve LED'lerin izlenmesi artık doğru durumu gösteriyor. |Güncelleştirme 1 |Evet |Hayır |
| 8 |LED'lerin izlenmesi |Bazı durumlarda, Güncelleştirme 1'i uyguladıktan sonra, etkin denetleyicideki mavi ışık kapalı olduğundan etkin denetleyicinin tanımlanmasını zorhale getirebilirsiniz. Bu sorun bu yama sürümünde giderilmiştir. |Güncelleştirme 1.2 |Evet |Hayır |
| 9 |Ağ arabirimleri |Önceki sürümlerde, routable olmayan bir ağ geçidi ile yapılandırılan bir StorSimple aygıtı çevrimdışı olabilir. Bu sürümde, Veri 0 için yönlendirme ölçümü en düşük hale getirilmiştir; bu nedenle, diğer ağ arabirimleri bulut etkin olsa bile, aygıttan gelen tüm bulut trafiği Veri 0 üzerinden yönlendirilir. |Güncelleştirme 1 |Evet |Evet |
| 10 |Yedeklemeler |Güncelleştirme 1'de, yama sürümü Güncelleştirme 1.1'de 24 gün sonra yedeklemelerin başarısız olmasına neden olan bir hata düzeltildi. |Güncelleştirme 1.1 |Evet |Evet |
| 11 |Yedeklemeler |Önceki sürümlerde bir hata, düşük değişim oranlarına sahip bulut anlık görüntüleri için düşük performansa neden oldu. Bu hata bu yama sürümünde düzeltilmiştir. |Güncelleştirme 1.2 |Evet |Evet |
| 12 |Güncelleştirmeler |Güncelleştirme 1'de başarısız bir yükseltme bildirilen ve denetleyicilerin Kurtarma moduna geçmesine neden olan bir hata, bu yama sürümünde düzeltildi. |Güncelleştirme 1.2 |Evet |Evet |

## <a name="known-issues-in-update-12"></a>Güncelleştirme 1.2'de bilinen sorunlar
Aşağıdaki tablo, bu sürümde bilinen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çoğunluğu |Nadir durumlarda, 8600 aygıtın EBOD muhafazasındaki disklerin çoğu kesilirse ve bu da disk yeter sayısı olmadan sonuçlanırsa, depolama havuzu çevrimdışı olur. Diskler yeniden bağlansa bile çevrimdışı kalır. |Aygıtı yeniden başlatmanız gerekir. Sorun devam ederse, lütfen sonraki adımlar için Microsoft Destek'e başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değiştirme yapıldığında, denetleyici 0 denetleyici 1 olarak görünebilir. Denetleyici değiştirme sırasında, görüntü eş düğümünden yüklendiğinde, denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterilebilir. Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra da görülebilir. |Kullanıcı eylemi gerekmez. Bu durum, denetleyici değişimi tamamlandıktan sonra kendiliğinden çözülür. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama hizmetini kullanmak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerinin alınamadığı bir duruma yol açar. |Evet |Evet | |
| 4 |Cihaz arıza |Aynı kaynak aygıttan farklı hedef aygıtlara bir birim kapsayıcının birden fazla failovers desteklenmez. Tek bir ölü cihazdan birden fazla cihaza geçemeyen aygıt, aygıtın veri sahipliğini kaybetmesi nedeniyle ilk arızalı birim kapsayıcıları yapar. Böyle bir başarısızlıktan sonra, bu birim kapsayıcıları Azure klasik portalında görüntülediğinizde farklı görünür veya farklı davranacaktır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple Adaptörü sırasında, yüklemenin başarıyla tamamlanması için bir aygıt IP'si sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy |Web proxy yapılandırmanızda belirtilen protokol olarak HTTPS varsa, aygıttan hizmete iletişiminiz etkilenir ve aygıt çevrimdışı duruma geçer. Destek paketleri de bu süreçte oluşturularak cihazınızda önemli kaynaklar tüketecektir. |Belirtilen protokol olarak web proxy URL'sinin HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy |Kayıtlı bir aygıtta web proxy'yi yapılandırıp etkinleştiriseniz, cihazınızdaki etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Yüksek bulut gecikmesi ve yüksek G/Ç iş yükü |StorSimple aygıtınız çok yüksek bulut gecikmeleri (saniye sırası) ve yüksek G/Ç iş yükünün bir birleşimi ile karşılaştığında, aygıt birimleri bozulmuş bir duruma girer ve G/Ç'ler "aygıt hazır değil" hatasıyla başarısız olabilir. |Aygıt denetleyicilerini el ile yeniden başlatmanız veya bu durumdan kurtulmak için bir aygıt başarısızı gerçekleştirmeniz gerekir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object -First 1 -Yeni** bir **VolumeContainer** nesnesi oluşturabilmeniz için ilk nesneyi seçmek için bekleyin, cmdlet tüm nesneleri döndürür. |Cmdlet'i parantez içinde şu şekilde sarın: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object -First 1 -Wait** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcı geçirildiğinde, en son yedekleme için ETA yalnızca ilk birim kapsayıcı için doğrudur. Ayrıca, ilk birim kapsayıcıdaki ilk 4 yedekleme geçirildikten sonra paralel geçiş başlar. |Aynı anda bir birim kapsayıcıgeçirmenizi öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklemeden sonra, birimler yedekleme ilkesine veya sanal disk grubuna eklenmez. |Yedekleme oluşturmak için bu birimleri yedekleme ilkesine eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |Geçiş tamamlandıktan sonra, 5000/7000 serisi aygıt geçirilen veri kapsayıcıları erişmemeli. |Geçiş tamamlandıktan ve işlendikten sonra geçirilen veri kapsayıcılarını silmenizi öneririz. |Evet |Hayır |
| 13 |Klon ve DR |Güncelleştirme 1'i çalıştıran bir StorSimple aygıtı, güncelleştirme öncesi 1 yazılımı çalıştıran bir aygıta Olağanüstü Durum Kurtarma'yı klonlayamaz veya gerçekleştiremez. |Bu işlemlere izin vermek için hedef aygıtı Güncelleştirme 1'e güncelleştirmeniz gerekir |Evet |Evet |
| 14 |Geçiş |Geçiş için yapılandırma yedeklemesi, ilişkili birimleri olmayan birim grupları olduğunda 5000-7000 serisi bir aygıtta başarısız olabilir. |İlişkili birim olmadan tüm boş birim gruplarını silin ve yapılandırma yedeklemesini yeniden deneyin. |Evet |Hayır |

## <a name="physical-device-updates-in-update-12"></a>Güncelleştirme 1.2'deki fiziksel aygıt güncelleştirmeleri
Yama güncelleştirmesi 1.2 fiziksel bir aygıta uygulanırsa (Güncelleştirme 1'den önceki sürümleri çalıştıran), yazılım sürümü 6.3.9600.17584 olarak değiştirilir.

## <a name="controller-and-firmware-updates-in-update-12"></a>Güncelleştirme 1.2'deki denetleyici ve firmware güncellemeleri
Bu sürüm, cihazınızdaki sürücüyü ve disk yazılımını güncelleştirir.

* SAS denetleyici güncelleştirmesi hakkında daha fazla bilgi için, [Microsoft Azure StorSimple Appliance'daki LSI SAS denetleyicileri için Güncelleştirme 1'e](https://support.microsoft.com/kb/3043005)bakın. 
* Disk firmware güncelleştirmesi hakkında daha fazla bilgi için Microsoft [Azure StorSimple Appliance için Disk firmware Update 1'e](https://support.microsoft.com/kb/3063416)bakın.

## <a name="virtual-device-updates-in-update-12"></a>Güncelleştirme 1.2'deki sanal cihaz güncellemeleri
Bu güncelleştirme sanal aygıta uygulanamaz. Yeni sanal aygıtların oluşturulması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Güncelleştirme 1.2'yi cihazınıza yükleyin.](storsimple-install-update-1.md)

