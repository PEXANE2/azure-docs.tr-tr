---
title: Azure Site Kurtarma'daki kurtarma planları hakkında
description: Azure Site Kurtarma'daki kurtarma planları hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: beb92bd62d011ef8aaf304dbb769e7694e6d7e60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257777"
---
# <a name="about-recovery-plans"></a>Kurtarma planları hakkında

Bu makalede, Azure Site [Kurtarma](site-recovery-overview.md)kurtarma kurtarma planları genel bir bakış sağlar.

Kurtarma planı, makineleri başarısız olmak amacıyla kurtarma gruplarına toplar. Kurtarma planı, başarısız olabileceğiniz küçük bağımsız birimler oluşturarak sistematik bir kurtarma işlemini tanımlamanıza yardımcı olur. Birim genellikle ortamınızdaki bir uygulamayı temsil eder.

- Kurtarma planı, makinelerin nasıl başarısız olduğunu ve başarısız olduktan sonra başlama sırasını tanımlar.
- Kurtarma planları Azure'a geçebilmek için kullanılır, ancak Azure'dan geri dönüş için kullanılamaz.
- Bir kurtarma planına en fazla 100 korumalı örnek eklenebilir.
- Bir planı sipariş, yönerge ve görevler ekleyerek özelleştirebilirsiniz.
- Bir plan tanımlandıktan sonra, üzerinde bir hata çalıştırabilirsiniz.
- Makineler, daha önce başka bir kurtarma planı kullanılarak dağıtıldıysa, sonraki planların bir makinenin dağıtımını/başlatılmasını atladığı birden çok kurtarma planında başvurulabilir.



### <a name="why-use-a-recovery-plan"></a>Neden bir kurtarma planı kullanıyorsun?

Kurtarma planlarını şu şekilde kullanın:

* Bir uygulamayı bağımlılıkları etrafında modelle.
* Kurtarma süresi hedefini (RTO) azaltmak için kurtarma görevlerini otomatikleştirin.
* Uygulamalarınızın kurtarma planının bir parçası olmasını sağlayarak geçiş veya olağanüstü durum kurtarma için hazır olduğunuzu doğrulayın.
* Olağanüstü durum kurtarma veya geçişin beklendiği gibi çalıştığından emin olmak için kurtarma planlarında test başarısızlığını çalıştırın.


## <a name="model-apps"></a>Uygulamaları modelleyin 
Uygulamaya özgü özellikleri yakalamak için bir kurtarma grubu planlayabilir ve oluşturabilirsiniz. Örnek olarak, SQL sunucu arka ucu, ara yazılım ve web ön ucu içeren tipik bir üç katmanlı uygulamayı ele alalım. Genellikle, kurtarma planını, her katmandaki makinelerin başarısız olduktan sonra doğru sırada başlamasını sağlayacak şekilde özelleştirin.

- SQL backend ilk, middleware sonraki ve son olarak web frontend başlamalıdır.
- Bu başlangıç sırası, uygulamanın son makine başlayana kadar çalışmasını sağlar.
- Bu sıra, ara yazılım başlatıldığında ve SQL Server katmanına bağlanmaya çalıştığında, SQL Server katmanının zaten çalışıyor olmasını sağlar. 
- Bu sipariş aynı zamanda ön uç sunucusunun en son başlamasını sağlamaya yardımcı olur, böylece son kullanıcılar tüm bileşenler çalışır durumda ve çalışmaya başlamadan önce uygulama URL'sine bağlanmaz ve uygulama istekleri kabul etmeye hazır olur.

Bu siparişi oluşturmak için kurtarma grubuna gruplar ve gruplara makineler ekleyebilirsiniz.
- Siparişin belirtildiği yerde sıralama kullanılır. Uygulama kurtarma RTO geliştirmek için, uygun olarak paralel çalışır.
- Tek bir gruptaki makineler paralel olarak başarısız olur.
- Farklı gruplardaki makineler grup sırasına göre başarısız olur, böylece Grup 2 makineleri başarısızlığa ancak Grup 1'deki tüm makineler arızalanıp başladıktan sonra başlarlar.

    ![Örnek kurtarma planı](./media/recovery-plan-overview/rp.png)

Bu özelleştirme nin yapılmasıyla, kurtarma planında bir başarısızlık çalıştırdığınızda aşağıdaki gibi olur: 

1. Kapatma adımı, şirket içi makineleri kapatmaya çalışır. Bunun istisnası, bir test başarısızlığını çalıştırDığınızve bu durumda birincil sitenin çalışmamaya devam etmesidir. 
2. Kapatma, kurtarma planındaki tüm makinelerin paralel bir şekilde başarısız olduğunu tetikler.
3. Failover çoğaltılan verileri kullanarak sanal makine diskleri hazırlar.
4. Başlangıç grupları sırayla çalışır ve her gruptaki makineleri başlatır. Önce Grup 1, sonra Grup 2 ve son olarak grup 3. Herhangi bir grupta birden fazla makine varsa, tüm makineler paralel olarak başlar.


## <a name="automate-tasks-in-recovery-plans"></a>Kurtarma planlarında görevleri otomatikleştirme

Büyük uygulamaları kurtarmak karmaşık bir görev olabilir. El ile atılan adımlar işlemi hataya yatkın hale getirin ve başarısızlığı çalıştıran kişi tüm uygulama karışıklıklarının farkında olmayabilir. Azure'da veya komut dosyasına başarısız olmak için Azure Otomasyon runbook'larını kullanarak sipariş vermek ve her adımda gereken eylemleri otomatikleştirmek için bir kurtarma planı kullanabilirsiniz. Otomatikleştirilen görevler için, el ile eylemler için duraklamaları kurtarma planlarına ekleyebilirsiniz. Yapılandırabileceğiniz birkaç tür görev vardır:

* Başarısız olduktan **sonra Azure VM'deki görevler**: Azure'da başarısız olduğunuzda, başarısız olduktan sonra VM'ye bağlanabilmeniz için genellikle eylemler gerçekleştirmeniz gerekir. Örnek: 
    * Azure VM'de herkese açık bir IP adresi oluşturun.
    * Azure VM ağ bağdaştırıcısına bir ağ güvenlik grubu atayın.
    * Kullanılabilirlik kümesine yük dengeleyicisi ekleyin.
* Başarısız olduktan **sonra VM içindeki görevler**: Bu görevler genellikle makinede çalışan uygulamayı yeniden yapılandırarak yeni ortamda doğru şekilde çalışmaya devam eder. Örnek:
    * Makineiçindeki veritabanı bağlantı dizesini değiştirin.
    * Web sunucusu yapılandırmasını veya kurallarını değiştirin.


### <a name="run-a-test-failover-on-recovery-plans"></a>Kurtarma planlarında bir test başarısızlığını çalıştırma

Bir test başarısızolmasını tetiklemek için bir kurtarma planı kullanabilirsiniz. Aşağıdaki en iyi uygulamaları kullanın:

- Tam bir başarısızlık çalıştırmadan önce her zaman bir uygulama üzerinde bir test başarısızlığını tamamlayın. Test arızaları, uygulamanın kurtarma sitesinde gelip gelmediğini kontrol etmenize yardımcı olur.
- Bir şey kaçırdığınızı bulursanız, bir temizlemeyi tetikleyin ve test başarısızlığını yeniden çalıştırın. 
- Uygulamanın sorunsuz bir şekilde iyileştiğinden emin olana kadar bir test başarısızlığını birden çok kez çalıştırın.
- Her uygulama benzersiz olduğundan, her uygulama için özelleştirilmiş kurtarma planları oluşturmanız ve her uygulamada bir test başarısızlığını çalıştırmanız gerekir.
- Uygulamalar ve bağımlılıkları sık sık değişir. Kurtarma planlarının güncel olduğundan emin olmak için, her üç ayda bir uygulama için bir test başarısızlığını çalıştırın.

    ![Site Kurtarma'da örnek bir test kurtarma planının ekran görüntüsü](./media/recovery-plan-overview/rptest.png)

## <a name="watch-a-recovery-plan-video"></a>Kurtarma planı videosunu izleyin

İki katmanlı Bir WordPress uygulaması için bir kurtarma planı için tıklamayla başarısız olduğunu gösteren hızlı bir örnek videoyu izleyin.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Sonraki adımlar

- Bir kurtarma planı [oluşturun.](site-recovery-create-recovery-plans.md)
- [Başarısız](site-recovery-failover.md) ları çalıştırın. 
