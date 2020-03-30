---
title: StorSimple 8000 Serisi Güncelleme 2 sürüm notları | Microsoft Dokümanlar
description: StorSimple 8000 Serisi Güncelleştirmesi 2'nin yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: carmonm
editor: ''
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: twooley
ms.openlocfilehash: 4e57fffd2f74ae1b14f51537c92299607f193ad5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934053"
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 Serisi Güncelleme 2 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncellemesi 2 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılımı, sürücüsü ve disk firmware güncelleştirmelerinin bir listesini de içerir. 

Güncelleştirme 2, Sürüm (GA) veya Güncelleme 0.1 ile Güncelleme 1.2 arasında çalıştıran herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 2 ile ilişkili aygıt sürümü 6.3.9600.17673'dür.

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Bu güncelleştirmeyi (Windows güncelleştirmeleri dahil) yüklemek yaklaşık 4-7 saat sürer. 
> * Güncelleme 2 yazılımı, LSI sürücüsü ve SSD firmware güncellemeleri vardır.
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu yakında kullanılabilir olacak gibi yeniden güncelleştirmeleri için tarar.
> 
> 

## <a name="whats-new-in-update-2"></a>Güncelleştirme 2'deki yenilikler
Güncelleştirme 2 aşağıdaki yeni özellikleri sunar.

* **Yerel olarak sabitlenmiş hacimler** – StorSimple 8000 serisinin önceki sürümlerinde, veri blokları kullanıma göre buluta katmanlandı. Blokların yerel tarafta kalacağını garanti etmenin bir yolu yoktu. Güncelleştirme 2'de, bir birim oluşturduğunuzda, bir birimi yerel olarak sabitlenmiş olarak atayabilirsiniz ve bu birimden gelen birincil veriler buluta katmanlandırılmaz. Yerel olarak sabitlenmiş birimlerin anlık görüntüleri, bulutun veri hareketliliği ve olağanüstü durum kurtarma amaçları için kullanılabilmesi için yedekleme için buluta kopyalanır. Ayrıca, birim türünü değiştirebilir (diğer bir şekilde katmanlı birimleri yerel olarak sabitlenmiş birimlere dönüştürebilir ve yerel olarak sabitlenmiş birimleri katmanlı olarak dönüştürebilirsiniz). 
* **StorSimple sanal cihaz geliştirmeleri** – Daha önce StorSimple 8000 serisi sanal cihazı olağanüstü durum kurtarma veya geliştirme/test çözümü olarak konumlandırdı. Sanal cihazın yalnızca bir modeli (model 1100) vardı. Güncelleme 2 iki sanal cihaz modelleri tanıttı: 
  
  * 8010 (eski 1100 denir) - Değişiklik yok; 30 TB kapasiteye sahiptir ve Azure standart depolama kullanır.
  * 8020 – 64 TB kapasiteye sahiptir ve daha iyi performans için Azure Premium depolamayı kullanır.
    
    Her iki sanal cihaz modeli (8010/8020) için tek bir VHD vardır. Sanal aygıtı ilk başlattığınızda, platform parametrelerini algılar ve doğru model sürümünü uygular.
* **Ağ Geliştirmeleri** – Update 2 aşağıdaki ağ geliştirmelerini içerir:
  
  * Bir NIC başarısız olursa, başarısız oluşabilmesi için bulut için birden çok NIC etkinleştirilebilir.
  * Bulut özellikli bloklar için sabit ölçümlerle yönlendirme geliştirmeleri.
  * Başarısız kaynaklar dan önce çevrimiçi yeniden deneme.
  * Hizmet hataları için yeni uyarılar.
* **Güncelleme Geliştirmeleri** – Update 1.2 ve daha önce, StorSimple 8000 serisi iki kanal üzerinden güncellendi: kümeleme için Windows Update, iSCSI, ve benzeri, ve microsoft update ikili ve firmware için.
    Güncelleştirme 2, tüm güncelleştirme paketleri için Microsoft Update kullanır. Bu daha az zaman yama veya failovers yapıyor yol açmalıdır. 
* **Firmware güncellemeleri** – Aşağıdaki firmware güncellemeleri dahildir:
  
  * LSI: lsi_sas2.sys Ürün Sürüm 2.00.72.10
  * Yalnızca SSD (HDD güncellemesi yok): XMGG, XGEG, KZ50, F6C2 ve VR08
* **Proaktif Destek** – Update 2, Microsoft'un aygıttan ek tanılama bilgilerini çekmesini sağlar. Operasyon ekibimiz sorun olan aygıtları tanımladığında, cihazdan bilgi toplamak ve sorunları tanılamak için daha iyi donanıma sahip olacağız. **Güncelleştirme 2'yi kabul ederek, bu proaktif desteği sağlamamıza izin verirsiniz.**    

## <a name="issues-fixed-in-update-2"></a>Güncelleştirme 2'de düzeltilen sorunlar
Aşağıdaki tablolar Güncelleştirmeler 2'de düzeltilen sorunların bir özetini sağlar.    

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ağ arabirimleri |StorSimple Manager hizmeti, Güncelleştirme 1'e yükseltme yapıldıktan sonra, Veri2 ve Data3 bağlantı noktalarının tek bir denetleyicide başarısız olduğunu bildirdi. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 2 |Güncelleştirmeler |Güncelleştirme 1'e yükseltmeden sonra, birden çok cihazda Azure klasik portalında sesli alarm uyarıları oluştu. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 3 |Openstack kimlik doğrulaması |Openstack'i bulut hizmeti sağlayıcınız olarak kullanırken, bulut kimlik doğrulama dizenizin çok uzun olduğu bir hata alabilirsiniz. Bu sorun düzeltildi. |Evet |Hayır |

## <a name="known-issues-in-update-2"></a>Güncelleştirme 2'de bilinen sorunlar
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
| 20 |Yerel olarak sabitlenmiş birimler |Katmanlı bir birimi (Güncelleştirme 1.2 veya daha öncesiyle oluşturulmuş ve klonlanmış) yerel olarak sabitlenmiş bir ses e dönüştürmeye çalışırsanız ve cihazınızda yer bitiyorsa veya bir bulut kesintisi varsa, klon(lar) bozulabilir. |Bu sorun yalnızca güncelleştirme öncesi 2 yazılımıyla oluşturulan ve klonlanan birimlerde oluşur. Bu seyrek bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Birim dönüştürme devam ederken bir birime eklenen AcR'ları güncelleştirin (yerel olarak sabitlenmiş veya tam tersi katmanlı). ARG'ların güncellenmesi veri bozulmasına neden olabilir. |Gerekirse, birim dönüştürmeden önce AC'leri güncelleştirin ve dönüştürme devam ederken başka ACR güncelleştirmesi yapmayın. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Güncelleme 2'deki denetleyici ve firmware güncellemeleri
Bu sürüm, cihazınızdaki sürücüyü ve disk yazılımını güncelleştirir.

* LSI firmware güncelleştirmesi hakkında daha fazla bilgi için Microsoft Knowledge base madde 3121900'a bakın. 
* Disk firmware güncelleştirmesi hakkında daha fazla bilgi için Microsoft Knowledge base article 3121899'a bakın.

## <a name="virtual-device-updates-in-update-2"></a>Güncelleştirme 2'deki sanal cihaz güncellemeleri
Bu güncelleştirme sanal aygıta uygulanamaz. Yeni sanal aygıtların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
StorSimple cihazınıza Update 2'yi nasıl [yükleyin](storsimple-install-update-2.md) öğrenin.

