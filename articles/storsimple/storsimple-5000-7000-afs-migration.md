---
title: StorSimple 5000-7000 serisindeki verileri Azure Dosya Eşitleme| Microsoft Dokümanlar
description: StorSimple 5000/7000 serisindeki verilerin Azure Dosya Eşitlemi'ne (AFS) nasıl geçirilmeye devam edilebildiğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: b46e9ee8fc3e14981a01cc2425a8ce55d06c5a9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65150747"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-azure-file-sync"></a>Verileri StorSimple 5000-7000 serisinden Azure Dosya Eşitleme'ye geçirin

> [!IMPORTANT]
> 9 Temmuz 2019'da StorSimple 5000/7000 serisi destek (EOS) durumuna ulaşacak. StorSimple 5000/7000 serisi müşterilerin belgede açıklanan alternatiflerden birine geçiş yapmanızı öneririz.

Veri aktarımı, verileri bir depolama konumundan diğerine taşıma işlemidir. Bu, bir kuruluşun geçerli verilerinin bir aygıttan başka bir aygıta tam bir kopyasını (tercihen etkin uygulamaları bozmadan veya devre dışı bırakmadan) yapmayı ve ardından tüm giriş/çıktı (G/Ç) etkinliğini yeni aygıta yönlendirmeyi gerektirir. 

StorSimple 5000 ve 7000 serisi depolama cihazları Temmuz 2019'da hizmete son verilecektir. Bu, Microsoft'un Temmuz 2019'dan sonra StorSimple 5000/7000 serisinin donanım ve yazılımLarını artık destekleyemeyeceği anlamına gelir. Bu aygıtları kullanan müşteriler StorSimple verilerini Azure'daki diğer karma depolama çözümlerine aktarmalıdır. Bu makale, StorSimple 5000/7000 serisi aygıtındaki verilerin Azure Dosya Eşitlemi'ne (AFS) geçişini kapsar.

## <a name="intended-audience"></a>Hedef kitle

Bu makale, veri merkezinde StorSimple 5000/7000 serisi cihazların dağıtımı ve yönetiminden sorumlu bilgi teknolojisi (BT) uzmanları ve bilgi çalışanları için hazırlanmıştır. Dosya sunucusu iş yükleri için StorSimple aygıtlarını kullanan müşteriler (Windows Server ile) bu geçiş yolunu özellikle çekici bulabilir. Azure Dosya Eşitleme özelliklerinin kuruluşunuz için iyi çalıştığını düşünüyorsanız, bu makale StorSimple'dan bu çözümlere nasıl geçeceğinizi anlamanıza yardımcı olur.

## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

Bu işlem, depolama alanı için StorSimple birimi kullanarak Windows dosya paylaşımını yapılandırmış müşteriler için çalışır. Verileri StorSimple 5000/7000'den Azure Dosya Eşitlemeye geçirmek, bu dosya paylaşım konumunu [Sunucu Bitiş Noktasına](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) dönüştürmeyi ve ardından yerel olarak bağlı olan sürücüleri yeni konumunuz olacak başka bir bitiş noktası olarak kullanmayı içerir. 

AFS'ye taşınırken aşağıdaki noktalar göz önünde bulundurulmalıdır:

1. Azure Dosyaları'nda şu anda 5 TB/paylaşım kısıtlaması vardır. Bu kısıtlama, birden çok Azure Dosyası paylaşımına yayılan verilerle Azure Dosya Eşitlemi kullanılarak aşılabilir. Daha fazla bilgi için [Azure Dosyaları dağıtımı için Veri büyüme modelini](https://docs.microsoft.com/azure/storage/files/storage-files-planning)gözden geçirin.
2. Bu geçiş, veri kopyası şirket içi aygıttan yapıldığından, birincil veri kümesinin tamamını şirket içi aygıta indirir. Bu aktarıma uyum sağlamak için yeterli bant genişliğine sahip olduğundan emin olun.
3. Bu işlem, zaten oluşturulmuş anlık görüntüleri korumaz. Yalnızca birincil verileri aktarır. İşlem, ilişkili bant genişliği şablonlarını veya yedekleme ilkelerini de korumaz. Veriler Azure Dosyası paylaşımında geçirildikten sonra yedekleme ilkelerini ayarlamak için [Azure Yedekleme'yi kullanın.](https://docs.microsoft.com/azure/backup/backup-azure-files)
4. StorSimple birinci taraf donanımı sağlar. Ancak, Azure Dosyaları/Azure Dosya Eşitlemi ile kendi yerel Windows Server donanımınızı yerel önbellek olarak kullanırsınız. Seçtiğiniz veri kümesini yerel tutmak için yeterli depolama kapasitesine sahip olduğundan emin olmalısınız. Katmanlama ve gerekli boş alan hedefini ayarlama hakkında daha fazla bilgi için Azure [Dosya Eşitlemi'ni dağıtırken sunucu bitiş noktasının nasıl oluşturulturolduğunu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal)gözden geçirin. 
5. StorSimple'dan değişiklik gösterince [Azure Dosya Eşitlemi fiyatlandırmasını](https://azure.microsoft.com/pricing/details/storage/files/) gözden geçirin. AFS StorSimple gibi çoğaltma ve sıkıştırma yok.

## <a name="migration-prerequisites"></a>Geçiş ön koşulları

Burada, eski 5000 veya 7000 serisi aygıtınız için Azure Dosya Eşitle'ye geçiş ön koşulları bulacaksınız. Başlamadan önce şunları yaptığınızdan emin olun:

- Yazılım sürümü v2.1.1.518 veya daha sonra çalışan bir StorSimple 5000/7000 serisi cihaza erişim. Önceki sürümler desteklenmez. StorSimple cihazınızın web Kullanıcı Arabirimi'nin sağ üst köşesinde çalışan yazılım sürümünü görüntülemeniz gerekir.  
    Cihazınız v2.1.1.518 çalışmıyorsa, lütfen sisteminizi gerekli minimum sürüme yükseltin. Ayrıntılı talimatlar için [sisteminizi v2.1.1.518'e yükseltin'e](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518)bakın.
- Kaynak aygıtta çalışan etkin yedekleme işlerini denetleyin. Buna StorSimple Veri Koruma Konsolu ana bilgisayardaki işler de dahildir. Geçerli işlerin tamamlanmasını bekleyin. 
- StorSimple 5000-7000 serisi cihazınıza bağlı bir Windows Server ana bilgisayarına erişim. Ana bilgisayar, [Azure Dosya Eşitleme birlikte çalışabilirliğinde](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)açıklandığı gibi desteklenen bir Windows Server sürümünü çalıştırıyor olmalıdır.
- StorBasit birimler ana bilgisayara monte edilir ve dosya paylaşımları içerir.
- Ana bilgisayar, yerel olarak önbelleğe alınan verilerinizi tutmak için yeterli yerel depolama alanına sahiptir.
- Azure Dosya Eşitlemeyi dağıtmak için kullanacağınız Azure aboneliğine sahip düzeyinde erişim. Sahip veya yönetici düzeyinde izinleriniz yoksa eşitleme grubunuz için bir bulut bitiş noktası oluştururken sorunlarla karşılaşabilirsiniz.
- Eşitlemek istediğiniz bir Azure Dosya Paylaşımı ile genel amaçlı bir [v2 depolama hesabına](https://docs.microsoft.com/azure/storage/common/storage-account-overview) erişim. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
  - Azure Dosya Paylaşımı Nasıl [Oluşturulur?](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)

## <a name="migration-process"></a>Geçiş süreci

StorSimple 5000-7000'den AFS'ye veri aktarmak iki aşamalı bir işlemdir:
1.  StorSimple birimlerinin Azure Dosyaları paylaşımına monte edildiği şirket içi dosya sunucusundaki verileri çoğaltın.  Çoğaltma, yüklediğiniz bir AFS aracısı üzerinden yapılır.
2.  StorSimple cihazının bağlantısını kesin. Yerel diskler daha sonra yerel önbellek olarak hareket eder.

### <a name="migration-steps"></a>Geçiş adımları

StorSimple birimlerinde yapılandırılan Windows dosya paylaşımını Azure Dosya Eşitleme paylaşımına geçirmek için aşağıdaki adımları gerçekleştirin. 
1.  Bu adımları, StorSimple birimlerinin monte edildiği aynı Windows Server ana bilgisayarda gerçekleştirin veya farklı bir sistem kullanın. 
    - [Windows Server'ı Azure Dosya Eşitlemi ile kullanmaya hazırlayın.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#prepare-windows-server-to-use-with-azure-file-sync)
    - [Azure Dosya Eşitleme aracısını yükleyin.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#install-the-azure-file-sync-agent)
    - [Depolama Eşitleme hizmetini dağıtın.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#deploy-the-storage-sync-service) 
    - [Windows Server'ı Depolama Eşitleme hizmetiyle kaydedin.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#register-windows-server-with-storage-sync-service) 
    - [Eşitleme grubu ve bulut bitiş noktası oluşturun.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#create-a-sync-group-and-a-cloud-endpoint) Ana bilgisayardan geçirilmesi gereken her Windows dosya paylaşımı için eşitleme gruplarının yapılması gerekir.
    - [Sunucu bitiş noktası oluşturun.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal#create-a-server-endpoint) Yolu, dosya paylaşım verilerinizi içeren StorSimple biriminin yolu olarak belirtin. Örneğin, StorSimple birimi sürücüise `J`ve verileriniz bulunursa, `J:/<myafsshare>`bu yolu sunucu bitiş noktası olarak ekleyin. **Seviyeyi** Devre **Dışı**Olarak Bırakın.
2.  Dosya sunucusu eşitlemeyi tamamlanana kadar bekleyin. Belirli bir eşitleme grubundaki her sunucu için şunları unutmayın:
    - Hem yükleme hem de karşıdan yükleme için Son Deneme Eşitleme için zaman damgaları yenidir.
    - Durum hem yükleme hem de indirme için yeşildir.
    - **Eşitleme Etkinliği,** eşitlemek için çok az dosya kaldığını veya hiç dosya kalmadığını gösterir.
    - **Eşitlemeyen Dosyalar** hem yükleme hem de indirme için 0'dır.
    Sunucu eşitlemeyi tamamlandığında daha fazla bilgi için [Azure Dosya Eşitlemeyi Sorun Giderme'ye](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#how-do-i-know-if-my-servers-are-in-sync-with-each-other)gidin. Eşitleme, veri boyutunuza ve bant genişliğinize bağlı olarak birkaç saat ile günler arasında sürebilir. Eşitleme tamamlandıktan sonra, tüm verileriniz Güvenli bir şekilde Azure Dosya Paylaşımı'nda dır. 
3.  StorSimple birimlerindeki paylaşımlara gidin. Bir paylaşım seçin, sağ tıklayın ve **Özellikler'i**seçin. **Güvenlik**altındaki paylaşım izinlerine dikkat edin. Bu izinlerin sonraki adımda yeni paya el ile uygulanması gerekir.
4.  Aynı Windows Server ana bilgisayarını mı yoksa farklı bir ana bilgisayar mı kullandığınıza bağlı olarak, sonraki adımlar farklı olacaktır.

    Farklı bir Windows Server ana bilgisayar kullanıyorsanız bu adımı atlayın ve bir sonraki adıma geçin. AFS için aynı Windows Dosya Sunucusu'nu kullanıyorsanız, şimdi birkaç dakikalık bir kapalı kalma süresi yle karşılaşırsınız. 
    - **Kapalı kalma süresi başlar** - *Adım 1F'de*oluşturduğunuz sunucu bitiş noktasını silin. 
    - Verilerin ileriye dönük olarak ikamet etmesini istediğiniz yol ile yeni bir sunucu bitiş noktası oluşturun.
    - Sunucu bitiş noktası Sağlıklı olarak gösterdiğinde (bu işlem birkaç dakika sürebilir), verileri bu yeni konumda görürsünüz. Artık Windows Server ana bilgisayarınızı bu yeni konumdan dosyalara hizmet edecek şekilde yapılandırabilirsiniz. - **Kapalı kalma süresi sona erer.**
5.  Azure Dosya Eşitlemesi için başka bir Windows Dosya Sunucusu kullanıyorsanız, herhangi bir kapalı kalma süresi yaşamazsınız. 
    - StorSimple aygıtı yerine önbellek olarak kullanmaya hazır olduğunuz yerel depolama yolunu içeren başka bir sunucu bitiş noktası ekleyin. 
    - Birkaç dakika içinde yeni sunucuda dosyaları görmek mümkün olacak. StorSimple cihazınızdan istediğiniz zaman ana bilgisayardaki bu yeni konuma geçiş yapmakta özgürsunuz.

    > [!TIP] 
    > Bu yeni dosya paylaşımını, kesintiyi en aza indirmek için, değiştirdiği yle aynı ada ve aynı yolla yapılandırmayı düşünün. DFS-N kullanıyorsanız, bu yapılandırmada değişiklik yapmanızı gerektirebilir.
6.  *3. adımda*belirtildiği gibi paylaşım izinlerini yeniden yapılandırın.

Veri geçişi sırasında herhangi bir sorunla karşılaşırsanız, lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) 



## <a name="next-steps"></a>Sonraki adımlar

AFS sizin için doğru çözüm değilse, [StorSimple 5000-7000 serisindeki verileri 8000 serisi bir cihaza](storsimple-8000-migrate-from-5000-7000.md)nasıl geçirin.

