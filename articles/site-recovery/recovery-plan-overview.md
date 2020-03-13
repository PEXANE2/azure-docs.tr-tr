---
title: Azure Site Recovery 'de kurtarma planları hakkında
description: Azure Site Recovery 'de kurtarma planları hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: beb92bd62d011ef8aaf304dbb769e7694e6d7e60
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79257777"
---
# <a name="about-recovery-plans"></a>Kurtarma planları hakkında

Bu makalede [Azure Site Recovery](site-recovery-overview.md)içindeki kurtarma planlarına genel bir bakış sunulmaktadır.

Kurtarma planı, yük devretme amacıyla makineleri kurtarma gruplarına toplar. Kurtarma planı, yük devredebilmeniz gereken küçük bağımsız birimler oluşturarak sistematik bir kurtarma işlemi tanımlamanıza yardımcı olur. Birim genellikle ortamınızdaki bir uygulamayı temsil eder.

- Kurtarma planı, makinelerin yük devretmesinin nasıl başarısız olduğunu ve yük devretme sonrasında başladıkları sırayı tanımlar.
- Kurtarma planları, Azure 'a yük devretme için kullanılır, ancak Azure 'dan yeniden çalışma için kullanılamaz.
- En fazla 100 korumalı örnek, tek bir kurtarma planına eklenebilir.
- Bir planı, bu plana sıra, yönergeler ve görevler ekleyerek özelleştirebilirsiniz.
- Bir plan tanımlandıktan sonra, üzerinde bir yük devretme çalıştırabilirsiniz.
- Daha önce başka bir kurtarma planı kullanılarak dağıtılmışsa, sonraki planların bir makinenin dağıtımını/başlatılmasını atlayabileceği birden çok kurtarma planında makinelere başvurulabilir.



### <a name="why-use-a-recovery-plan"></a>Neden kurtarma planı kullanılmalıdır?

Kurtarma planlarını şu şekilde kullanın:

* Bir uygulamayı bağımlılıkları etrafında modelleyin.
* Kurtarma süresi hedefini (RTO) azaltmak için kurtarma görevlerini otomatikleştirin.
* Uygulamalarınızın bir kurtarma planının parçası olmasını sağlayarak geçiş veya olağanüstü durum kurtarma için hazırlandığınızı doğrulayın.
* Olağanüstü durum kurtarma veya geçişin beklendiği gibi çalıştığından emin olmak için kurtarma planlarında yük devretme testi gerçekleştirin.


## <a name="model-apps"></a>Model uygulamaları 
Uygulamaya özgü özellikleri yakalamak için bir kurtarma grubu planlayabilir ve oluşturabilirsiniz. Örnek olarak, SQL Server arka ucu, ara yazılım ve Web ön ucuna sahip tipik üç katmanlı bir uygulamayı ele alalım. Genellikle kurtarma planını, her katmandaki makinelerin yük devretme sonrasında doğru sırada başlayacağı şekilde özelleştirirsiniz.

- SQL arka ucu önce, sonraki ara yazılım ve son olarak Web ön ucuna başlamalıdır.
- Bu başlangıç siparişi, uygulamanın son makinenin başladığı zamana göre çalışmasını sağlar.
- Bu sipariş, ara yazılım başlatıldığında ve SQL Server katmanına bağlanmaya çalıştığında, SQL Server katmanının zaten çalışmakta olmasını sağlar. 
- Bu sıra, ön uç sunucusunun en son başlamasını sağlamaya yardımcı olur, böylece son kullanıcılar uygulama URL 'sine tüm bileşenler çalışmaya ve çalışmadan önce bağlanmaması ve uygulama istekleri kabul etmeye hazırlanmaya devam eder.

Bu sırayı oluşturmak için, kurtarma grubuna gruplar ekler ve gruplara makineler eklersiniz.
- Order belirtildiğinde sıralama kullanılır. Eylemler, uygulama kurtarma RTO 'ı geliştirmek için uygun şekilde paralel olarak çalışır.
- Tek bir gruptaki makineler paralel olarak yük devreder.
- Grup 2 makinelerinin yük devretmesini yalnızca Grup 1 ' deki tüm makinelere devreder ve başlatıldıktan sonra başlatabilmesi için, farklı gruplardaki makineler grup sırasında yük devreder.

    ![Örnek kurtarma planı](./media/recovery-plan-overview/rp.png)

Bu özelleştirmeyle birlikte, kurtarma planında bir yük devretme çalıştırdığınızda ne olur: 

1. Kapatma adımı, şirket içi makineleri kapatmaya çalışır. Özel durum, bir yük devretme testi çalıştırırsanız, bu durumda birincil site çalışmaya devam eder. 
2. Bu oturum, kurtarma planındaki tüm makinelerin paralel yük devretmesini tetikler.
3. Yük devretme, çoğaltılan verileri kullanarak sanal makine disklerini hazırlar.
4. Başlangıç grupları sırayla çalışır ve makineler her grupta başlatılır. Birincisi, 1. Grup çalışır, sonra Grup 2 ve son olarak, Grup 3. Herhangi bir grupta birden fazla makine varsa, tüm makineler paralel olarak başlar.


## <a name="automate-tasks-in-recovery-plans"></a>Kurtarma planlarındaki görevleri otomatikleştirme

Büyük uygulamaları kurtarmak karmaşık bir görev olabilir. El ile yapılan adımlar işlemi hataya açıktır ve yük devretmeyi çalıştıran kişi tüm uygulama farkınmallarını bilmeyebilir. Azure 'da yük devretme için Azure Otomasyonu runbook 'larını kullanarak her adımda sipariş getirmek ve gereken eylemleri otomatikleştirmek için bir kurtarma planı kullanabilirsiniz. Otomatikleştirilen görevler için kurtarma planlarına el ile eylemler için duraklamalar ekleyebilirsiniz. Yapılandırabilmeniz için kullanabileceğiniz birkaç görev türü vardır:

* **Yük devretmeden sonra Azure VM 'Deki görevler**: Azure 'a yük devrettikten sonra, yük DEVRETMEDEN sonra VM 'ye bağlanabilmeniz için genellikle eylemler gerçekleştirmeniz gerekir. Örnek: 
    * Azure VM 'de genel IP adresi oluşturun.
    * Azure VM 'nin ağ bağdaştırıcısına bir ağ güvenlik grubu atayın.
    * Bir kullanılabilirlik kümesine yük dengeleyici ekleyin.
* **Yük devretme SONRASıNDA VM Içindeki görevler**: Bu görevler genellikle makinede çalışan uygulamayı yeniden yapılandırarak yeni ortamda düzgün çalışmaya devam eder. Örnek:
    * Makinenin içindeki veritabanı bağlantı dizesini değiştirin.
    * Web sunucusu yapılandırmasını veya kurallarını değiştirin.


### <a name="run-a-test-failover-on-recovery-plans"></a>Kurtarma planlarında yük devretme testi çalıştırma

Yük devretme testi tetiklemesi için bir kurtarma planı kullanabilirsiniz. Aşağıdaki en iyi yöntemleri kullanın:

- Tam yük devretme çalıştırmadan önce, bir uygulamadaki yük devretme testini her zaman doldurun. Yük devretme testi, uygulamanın kurtarma sitesinde bulunup bulunmadığını kontrol etmenize yardımcı olur.
- Bir şey kaçırdıysanız, bir temizleme tetikleyin ve sonra yük devretme testini yeniden çalıştırın. 
- Uygulamanın sorunsuz bir şekilde kurtarıp kurtarana kadar bir test yük devretmesini birden çok kez çalıştırın.
- Her uygulama benzersiz olduğundan, her bir uygulama için özelleştirilmiş kurtarma planları oluşturmanız ve her biri üzerinde yük devretme testi çalıştırmanız gerekir.
- Uygulamalar ve bağımlılıkları sıklıkla değişir. Kurtarma planlarının güncel olduğundan emin olmak için her ay her bir uygulama için bir yük devretme testi çalıştırın.

    ![Site Recovery bir örnek test kurtarma planının ekran görüntüsü](./media/recovery-plan-overview/rptest.png)

## <a name="watch-a-recovery-plan-video"></a>Kurtarma planı videosunu izleyin

İki katmanlı bir WordPress uygulamasının kurtarma planına yönelik bir tıklama yük devretmesini gösteren hızlı bir örnek video izleyin.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Sonraki adımlar

- Kurtarma planı [oluşturun](site-recovery-create-recovery-plans.md) .
- Yük devretme işlemleri [gerçekleştirin](site-recovery-failover.md) . 
