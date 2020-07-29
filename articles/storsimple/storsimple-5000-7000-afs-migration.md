---
title: StorSimple 5000-7000 serilerinden verileri Azure Dosya Eşitleme 'e geçirin | Microsoft Docs
description: StorSimple 5000/7000 serisinden Azure Dosya Eşitleme (AFS) öğesine nasıl veri geçirileceği açıklanmaktadır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 5d656fd8757580b8ce96acf168e92fc847d400ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514063"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-azure-file-sync"></a>StorSimple 5000-7000 serilerinden verileri Azure Dosya Eşitleme 'e geçirme

> [!IMPORTANT]
> 9 Temmuz 2019 ' de, StorSimple 5000/7000 Serisi destek (EOS) durumunun sonuna ulaşacaktır. StorSimple 5000/7000 Serisi müşterilerinin belgede açıklanan alternatifden birine geçiş yapmanızı öneririz.

Veri geçişi, verileri bir depolama konumundan diğerine taşıma işlemidir. Bu, bir kuruluşun geçerli verilerinin bir cihazdan başka bir cihaza (tercihen etkin uygulamaları bozmadan veya devre dışı bırakmaksızın) ve ardından tüm giriş/çıkış (g/ç) etkinliğini yeni cihaza yeniden yönlendirmeden tam bir kopyasını yapmayı gerektirir. 

StorSimple 5000 ve 7000 Serisi depolama cihazları hizmet sonuna kadar, 2019. Bu, Microsoft 'un 2019 Haziran 'dan sonra StorSimple 5000/7000 Serisi için donanım ve yazılımı desteklemeye artık erişememesi anlamına gelir. Bu cihazları kullanan müşteriler, StorSimple verilerini Azure 'daki diğer karma depolama çözümlerine geçirmelidir. Bu makalede, StorSimple 5000/7000 Serisi cihazından Azure Dosya Eşitleme (AFS) sürümüne veri geçişi ele alınmaktadır.

## <a name="intended-audience"></a>Hedef kitle

Bu makale, veri merkezi 'nde StorSimple 5000/7000 serisi cihazları dağıtmaktan ve yönetmekten sorumlu Bilişim Teknolojileri (BT) uzmanlarına ve bilgi çalışanlarına yöneliktir. Dosya sunucusu iş yükleri (Windows Server ile) için StorSimple cihazlarını kullanan müşteriler bu geçiş yolunu özellikle çekici bir şekilde bulabilir. Azure Dosya Eşitleme özelliklerinin kuruluşunuz için uygun olduğunu düşünüyorsanız, bu makale StorSimple 'tan bu çözümlere nasıl taşınacağını anlamanıza yardımcı olur.

## <a name="migration-considerations"></a>Geçiş fikirleri

Bu işlem, depolama için bir StorSimple birimi kullanarak Windows dosya paylaşımının yapılandırdığı müşteriler için geçerlidir. StorSimple 5000/7000 ' den Azure Dosya Eşitleme veri geçişi, bu dosya paylaşımının konumunu bir [sunucu uç noktasına](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) dönüştürmeyi ve ardından yerel olarak eklenmiş sürücüleri başka bir uç nokta olarak kullanmayı, daha sonra yeni konumunuz olacak şekilde kullanmanızı içerir. 

AFS 'ye geçiş yaparken aşağıdaki noktaların göz önünde bulundurulmalıdır:

1. Azure dosyaları şu anda 5 TB/Share kısıtlamasına sahiptir. Bu kısıtlama, birden çok Azure dosya paylaşımı arasında veri yayarak Azure Dosya Eşitleme kullanarak aşılabilecek. Daha fazla bilgi için [Azure dosyaları dağıtımı Için veri büyüme modelini](https://docs.microsoft.com/azure/storage/files/storage-files-planning)inceleyin.
2. Bu geçiş, şirket içi cihazdan veri kopyası yapıldığından, tüm birincil veri kümesini şirket içi bir cihaza indirir. Bu aktarıma uyum sağlamak için yeterli bant genişliğine sahip olduğunuzdan emin olun.
3. Bu işlem, zaten oluşturulmuş olan anlık görüntüleri korumaz. Yalnızca birincil verileri geçirir. İşlem ayrıca ilişkili bant genişliği şablonlarını veya yedekleme ilkelerini korumaz. Azure dosya paylaşımında veriler geçirildikten sonra yedekleme ilkelerini ayarlamak için [Azure Backup kullanın](https://docs.microsoft.com/azure/backup/backup-azure-files) .
4. StorSimple birinci taraf donanımlar sağlar. Ancak, Azure dosyaları/Azure Dosya Eşitleme ile yerel önbellek olarak kendi yerel Windows Server donanımınızı kullanıyorsunuz. Seçtiğiniz yerel veri kümesini tutmak için yeterli depolama kapasitesine sahip olduğunuzdan emin olmanız gerekir. Katmanlama ve gereken boş alan hedefini ayarlama hakkında daha fazla bilgi için, [Azure dosya eşitleme dağıtımı sırasında sunucu uç noktası oluşturma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal)konusunu inceleyin. 
5. StorSimple 'tan farklı olduğundan [Azure dosya eşitleme için fiyatlandırmayı](https://azure.microsoft.com/pricing/details/storage/files/) gözden geçirin. AFS, StorSimple gibi yinelenenleri kaldırma ve sıkıştırmaya sahip değildir.

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

Burada, Azure Dosya Eşitleme için eski 5000 veya 7000 Serisi cihazınız için geçiş önkoşullarını bulacaksınız. Başlamadan önce şunları sağlayın:

- Yazılım sürümü v 2.1.1.518 veya üstünü çalıştıran bir StorSimple 5000/7000 Serisi cihaza erişim. Önceki sürümler desteklenmez. StorSimple cihazınızın Web Kullanıcı arabiriminin sağ üst köşesinde, çalıştıran yazılım sürümü görüntülenmelidir.  
    Cihazınız v 2.1.1.518 çalıştırıyorsa, lütfen sisteminizi gerekli en düşük sürüme yükseltin. Ayrıntılı yönergeler için [sisteminizi v 2.1.1.518 'ye yükseltme](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518)bölümüne bakın.
- Kaynak cihazda çalışan tüm etkin yedekleme işlerini denetleyin. Bu, StorSimple veri koruma konsolu ana bilgisayarındaki işleri içerir. Geçerli işlerin tamamlanmasını bekleyin. 
- StorSimple 5000-7000 Serisi cihazınıza bağlı bir Windows Server konağına erişin. Konağın [Azure dosya eşitleme birlikte çalışabilirlik](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)bölümünde açıklandığı gibi desteklenen bir Windows Server sürümü çalıştırması gerekir.
- StorSimple birimleri konağa bağlanır ve dosya paylaşımları içerir.
- Konağın yerel olarak önbelleğe alınmış verilerinizi tutmak için yeterli yerel depolama alanı vardır.
- Azure Dosya Eşitleme dağıtmak için kullanacağınız Azure aboneliğine sahip düzeyi erişimi. Sahip veya yönetici düzeyi izinleriniz yoksa, eşitleme grubunuz için bir bulut uç noktası oluştururken sorunlarla karşılaşabilirsiniz.
- Eşitlemek istediğiniz bir Azure dosya paylaşımıyla [genel amaçlı v2 depolama hesabına](https://docs.microsoft.com/azure/storage/common/storage-account-overview) erişim. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
  - [Azure dosya paylaşımının](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)nasıl oluşturulacağı.

## <a name="migration-process"></a>Geçiş süreci

StorSimple 5000-7000 ' den AFS 'ye veri geçirme iki adımlı bir işlemdir:
1.  StorSimple birimlerinin bir Azure dosya paylaşımında takılı olduğu şirket içi dosya sunucusundan verileri çoğaltın.  Çoğaltma, yüklediğiniz bir AFS Aracısı aracılığıyla yapılır.
2.  StorSimple cihazını sökün. Yerel diskler daha sonra yerel önbellek işlevi görür.

### <a name="migration-steps"></a>Geçiş adımları

StorSimple birimlerinde yapılandırılmış Windows dosya paylaşımının Azure Dosya Eşitleme bir paylaşıma geçirilmesi için aşağıdaki adımları gerçekleştirin. 
1.  Bu adımları, StorSimple birimlerinin bağlı olduğu Windows Server ana bilgisayarında gerçekleştirin veya farklı bir sistem kullanın. 
    - [Windows Server 'ı Azure dosya eşitleme ile kullanılacak şekilde hazırlayın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#prepare-windows-server-to-use-with-azure-file-sync).
    - [Azure dosya eşitleme aracısını yükler](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#install-the-azure-file-sync-agent).
    - [Depolama eşitleme hizmeti 'Ni dağıtın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#deploy-the-storage-sync-service). 
    - [Windows Server 'ı depolama eşitleme hizmeti Ile kaydedin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#register-windows-server-with-storage-sync-service). 
    - [Bir eşitleme grubu ve bir bulut uç noktası oluşturun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#create-a-sync-group-and-a-cloud-endpoint). Konaktan geçirilmesi gereken her Windows dosya paylaşımında eşitleme gruplarının yapılması gerekir.
    - [Sunucu uç noktası oluşturun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=portal#create-a-server-endpoint). Dosya paylaşma verilerinizi içeren StorSimple biriminin yolu olarak yolunu belirtin. Örneğin, StorSimple birimi sürücü ise ve verileriniz ' de yer alıyorsa, `J` `J:/<myafsshare>` Bu yolu sunucu uç noktası olarak ekleyin. **Katmanlamayı** **devre dışı**olarak bırakın.
2.  Dosya sunucusu eşitlemesi tamamlanana kadar bekleyin. Belirli bir eşitleme grubundaki her bir sunucu için şunları yaptığınızdan emin olun:
    - Hem karşıya yükleme hem de indirme için denenen son eşitlemeye ilişkin zaman damgaları son tarih.
    - Durum hem karşıya yükleme hem de indirme için yeşil olur.
    - **Eşitleme etkinliği** , eşitlenmek üzere kalan çok az veya hiç dosya gösterir.
    - **Dosya eşitleme** , hem karşıya yükleme hem de indirme için 0 ' dır.
    Sunucu eşitlemesi tamamlandığında hakkında daha fazla bilgi için [Azure dosya eşitleme sorun giderme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#how-do-i-know-if-my-servers-are-in-sync-with-each-other)bölümüne gidin. Eşitleme, veri boyutunuza ve bant genişliğine bağlı olarak birkaç saat ile gün sürebilir. Eşitleme tamamlandıktan sonra tüm verileriniz Azure dosya paylaşımında güvenli bir şekilde gerçekleştirilir. 
3.  StorSimple birimlerindeki paylaşımlara gidin. Bir paylaşma seçin, sağ tıklayın ve **Özellikler**' i seçin. **Güvenlik**altındaki paylaşma izinlerini aklınızda yapın. Sonraki adımda bu izinlerin yeni paylaşıma el ile uygulanması gerekir.
4.  Aynı Windows Server konağını mı yoksa farklı bir tane mi kullandığınıza bağlı olarak, sonraki adımlar farklı olacaktır.

    Farklı bir Windows Server Konağı kullanıyorsanız, bu adımı atlayın ve sonraki adıma gidin. AFS için aynı Windows dosya sunucusunu kullanıyorsanız, artık birkaç dakika kapalı kalma süresine sahip olursunuz. 
    - **Kapalı kalma süresi başlar** - *Adım 1f*içinde oluşturduğunuz sunucu uç noktasını silin. 
    - Verilerin ileri doğru olmasını istediğiniz yolu içeren yeni bir sunucu uç noktası oluşturun.
    - Sunucu uç noktası sağlıklı olarak gösterildikten sonra (Bu işlem birkaç dakika sürebilir), bu yeni konumdaki verileri görürsünüz. Artık Windows Server konağını, dosyaları bu yeni konumdan sunacak şekilde yapılandırabilirsiniz. - **Kapalı kalma süresi sona erer**.
5.  Azure Dosya Eşitleme için başka bir Windows dosya sunucusu kullanıyorsanız, herhangi bir kesinti yaşanmaz. 
    - StorSimple cihazı yerine önbellek olarak kullanmaya hazırlanmakta olduğunuz yerel depolamanın yolunu içeren başka bir sunucu uç noktası ekleyin. 
    - Yeni sunucudaki dosyaları birkaç dakika içinde görebileceksiniz. StorSimple cihazınızdan herhangi bir zamanda bu yeni konuma geçiş yapmak ücretsizdir.

    > [!TIP] 
    > Bu yeni dosya paylaşımının aynı ada ve aynı yol ile aynı şekilde yapılandırılmasını, kesintiyi en aza indirmek için göz önünde bulundurun. DFS-N kullanılıyorsa, bu, yapılandırmada değişiklik yapmanız gerekebilir.
6.  *Adım 3*' te belirtilen paylaşım izinlerini yeniden yapılandırın.

Veri geçişi sırasında herhangi bir sorunla karşılaşırsanız lütfen [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md). 



## <a name="next-steps"></a>Sonraki adımlar

AFS sizin için doğru çözüm değilse, [StorSimple 5000-7000 serilerinden bir 8000 serisi cihaza veri geçirmeyi](storsimple-8000-migrate-from-5000-7000.md)öğrenin.

