---
title: HANA Azure SAP HANA (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için bir olağanüstü durum kurtarma sitesinde başarısızlık nasıl gerçekleştirilir (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3fe3ee79318ab9fdc9f2c0e9585051439b76b5cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617148"
---
# <a name="disaster-recovery-failover-procedure"></a>Olağanüstü durum kurtarma yük devretme yordamı


>[!IMPORTANT]
>Bu makale, SAP HANA yönetim belgeleri veya SAP Notları yerine geçmez. Özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR) için SAP HANA yönetimi ve operasyonlarında sağlam bir anlayışa ve uzmanlığa sahip olduğunuzu bekliyoruz. Bu makalede, SAP HANA Studio ekran görüntüleri gösterilir. SAP yönetim araçlarının ve araçların ekranlarının içeriği, yapısı ve doğası SAP HANA sürümünden piyasaya sürülebilir.

Bir DR sitesine başarısız olduğunuzda göz önünde bulundurulması gereken iki durum vardır:

- Verilerin en son durumuna geri dönmek için SAP HANA veritabanına ihtiyacınız vardır. Bu durumda, Microsoft'a başvurmaya gerek kalmadan başarısız olmayı gerçekleştirebileceğiniz bir self servis komut dosyası vardır. Geri dönüş için Microsoft ile çalışmanız gerekir.
- En son çoğaltılan anlık görüntü olmayan bir depolama anlık görüntüsüne geri yüklemek istiyorsunuz. Bu durumda, Microsoft ile çalışmanız gerekir. 

>[!NOTE]
>Dr birimini temsil eden HANA Büyük Örnek birimi üzerinde aşağıdaki adımlar yapılmalıdır. 
 
En son çoğaltılan depolama anlık görüntülerini geri yüklemek [için, Azure'da SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.2/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.2.1.pdf)"Tam DR failover gerçekleştirin - azure_hana_dr_failover" adımlarını izleyin. 

Birden çok SAP HANA örneğinin başarısız olmasını istiyorsanız, azure_hana_dr_failover komutunu birkaç kez çalıştırın. İstendiğinde, başarısız olmak ve geri yüklemek istediğiniz SAP HANA SID'yi girin. 


Gerçek çoğaltma ilişkisini etkilemeden DR failover'ı da test edebilirsiniz. Bir test başarısızmasını gerçekleştirmek [için, Azure'da SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.2/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.2.1.pdf)"Test DR failover gerçekleştirin - azure_hana_test_dr_failover" adımlarını izleyin. 

>[!IMPORTANT]
>Dr sitesinde oluşturduğunuz örnekte herhangi bir üretim işlemini **bir failover'ı test**etme işlemi yle *çalıştırmayın.* Komut azure_hana_test_dr_failover, birincil siteyle ilişkisi olmayan bir dizi birim oluşturur. Sonuç olarak, birincil siteye geri eşitleme mümkün *değildir.* 

Test etmek için birden çok SAP HANA örneği olmasını istiyorsanız, komut dosyasını birkaç kez çalıştırın. İstendiğinde, başarısız olmak için test etmek istediğiniz örneğin SAP HANA SID'ini girin. 

>[!NOTE]
>Saatler önce silinen bazı verileri kurtarmak için DR sitesine başarısız olmanız ve DR birimlerinin daha önceki bir anlık görüntüolarak ayarlanabilmeniz gerekiyorsa, bu yordam geçerlidir. 

1. Yürüttüğün HANA Büyük Örnekleri'nin olağanüstü durum kurtarma biriminde HANA'nın üretim dışı örneğini kapatın. Atıl durumdaki bir HANA üretim örneği önceden yüklenmiş.
1. SAP HANA işlemlerinin çalışmadığından emin olun. Bu denetim için aşağıdaki komutu kullanın:

      `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`.

      Çıktı, durmuş durumda **hdbdaemon** işlemini ve çalışan veya başlatılan durumda başka HANA işlemleri göstermeniz gerekir.
1. Olağanüstü durum kurtarma sitesinin geri yüklenmesini istediğiniz anlık görüntü adı veya SAP HANA yedekleme kimliğini belirleyin. Gerçek olağanüstü durum kurtarma durumlarında, bu anlık görüntü genellikle en son anlık görüntüdür. Kayıp verileri kurtarmanız gerekiyorsa, daha önceki bir anlık görüntü seçin.
1. Yüksek öncelikli bir destek isteği aracılığıyla Azure Destek'e başvurun. Dr sitesindeki anlık görüntünün adı ve tarihi veya HANA yedek kimliği yle birlikte bu anlık görüntünün geri yüklenmesini isteyin. Varsayılan değer, işlemler tarafının yalnızca /hana/veri hacmini geri yüklemesidir. /hana/logbackups birimlerine de sahip olmak istiyorsanız, bunu özellikle belirtmeniz gerekir. */hana/paylaşılan hacmi geri yüklemeyin.* Bunun yerine, PRD için /hana/paylaşılan birimi yeniden monte ettikten sonra **.snapshot** dizininden global.ini ve alt dizinleri gibi belirli dosyaları seçin. 

   İşlemler tarafında aşağıdaki adımlar oluşur:

   a. Anlık görüntünün üretim hacminden olağanüstü durum kurtarma birimlerine çoğaltılması durdurulur. Bu kesinti, olağanüstü durum kurtarma yordamını gerçekleştirmeniz gereken neden üretim alanında bir kesinti yse zaten olmuş olabilir.
   
   b. Seçtiğiniz yedek kimliğin bulunduğu depolama anlık görüntüsü adı veya anlık görüntüsü olağanüstü durum kurtarma birimlerinde geri yüklenir.
   
   c. Geri yüklemeden sonra, olağanüstü durum kurtarma hacimleri, olağanüstü durum kurtarma bölgesindeki HANA Büyük Örnek birimlerine monte edilebilir.
      
1. Olağanüstü durum kurtarma hacimlerini felaket kurtarma alanındaki HANA Büyük Örnek birimine monte edin. 
1. Atıl durumdaki SAP HANA üretim örneğini başlatın.
1. RPO süresini azaltmak için işlem günlüğü yedekleme günlüklerini kopyalamayı seçtiyseniz, işlem günlüğü yedeklemelerini yeni monte edilen DR /hana/logbackups dizininde birleştirin. Varolan yedeklemelerin üzerine yazmayın. Depolama anlık görüntüsünün en son çoğaltılmasıyla çoğaltılamayan yeni yedeklemeleri kopyalayın.
1. Ayrıca, DR Azure bölgesindeki /hana/shared/PRD birimine çoğaltılmayan anlık görüntülerden tek dosyaları geri yükleyebilirsiniz.

Aşağıdaki adımlar, geri yüklenen depolama anlık görüntüsünü ve kullanılabilen hareket günlüğü yedeklemelerini temel alarak SAP HANA üretim örneğini nasıl kurtarılabildiğini gösterir.

1. SAP HANA Studio'yu kullanarak yedekleme konumunu **/hana/logbackups** olarak değiştirin.

   ![DR kurtarma için yedekleme konumunu değiştirme](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA yedekleme dosya konumları arasında tarar ve geri yüklemek için en son işlem günlüğü yedekleme önerir. Aşağıdaki gibi bir ekran ortaya çıkana kadar tarama birkaç dakika sürebilir:

   ![DR kurtarma için işlem günlüğü yedeklemelistesi](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Varsayılan ayarlardan bazılarını ayarlayın:

      - Clear **Use Delta Yedeklemeleri**.
      - **Giriş Alanını Başlatma'yı**seçin.

   ![Başlatma günlüğü alanını ayarlama](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. **Bitiş'i**seçin.

   ![DR geri yüklemesini bitirin](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Burada gösterilen gibi bir ilerleme penceresi görünmelidir. Bunun, örneğin üç düğümlü ölçeklendirme SAP HANA yapılandırmasının olağanüstü durum kurtarma geri yüklemesi olduğunu unutmayın.

![İlerlemeyi geri yükleme](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Geri yükleme **Bitiş** ekranında yanıt vermeyi durdurur ve ilerleme ekranını göstermezse, alt düğümler üzerindeki tüm SAP HANA örneklerinin çalıştığını doğrulayın. Gerekirse SAP HANA örneklerini el ile başlatın.


## <a name="failback-from-a-dr-to-a-production-site"></a>DR'den üretim alanına geri dönüş
Dr'den üretim alanına geri dönebilirsiniz. Olağanüstü durum kurtarma sitesindeki başarısızlığa, kaybolan verileri kurtarma gereksiniminizin değil, üretim Azure bölgesindeki sorunlardan kaynaklandığı bir senaryoya bakalım. 

SAP üretim iş yükünüzü bir süredir olağanüstü durum kurtarma alanında çalıştırıyorsunuz. Üretim alanındaki sorunlar çözüldüğünden, üretim sitenize geri dönmek istiyorsunuz. Veri kaybedilemediğiniz için, üretim alanına geri adım, azure işlemleri ekibinde BIRKAÇ adım ve SAP HANA ile yakın işbirliği gerektirir. Sorunlar çözüldükten sonra operasyon ekibinin üretim alanına eşitlenmeye başlamasını tetiklemek size kalmış.

Şu adımları uygulayın:

1. Azure işlemleri ekibindeki SAP HANA, üretim depolama birimlerini artık üretim durumunu temsil eden olağanüstü durum kurtarma depolama birimlerinden eşitleme tetikleyicisine sahip olur. Bu durumda, üretim yerindeki HANA Büyük Örnek birimi kapatılır.
1. Azure işlemleri ekibindeki SAP HANA çoğaltmayı izler ve sizi bilgilendirmeden önce yakalandığından emin olur.
1. Felaket kurtarma sitesinde üretim HANA Örneği kullanan uygulamaları kapattınız. Daha sonra bir HANA hareket günlüğü yedekleme sın. Ardından, felaket kurtarma alanındaki HANA Büyük Örnek birimlerinde çalışan HANA örneğini durdurursunuz.
1. Olağanüstü durum kurtarma alanındaki HANA Büyük Örnek biriminde çalışan HANA örneği kapatıldıktan sonra, operasyon ekibi disk birimlerini el ile yeniden eşitler.
1. Azure işlem ekibindeki SAP HANA, üretim yerindeki HANA Büyük Örnek birimini yeniden başlatır. Sana teslim ediyorlar. SAP HANA örneğinin HANA Büyük Örnek biriminin başlangıç zamanında kapatma durumunda olduğundan emin olun.
1. Daha önce olağanüstü durum kurtarma sitesine başarısız olduğunuzda yaptığınız veritabanı geri yükleme adımlarını gerçekleştirirsiniz.

## <a name="monitor-disaster-recovery-replication"></a>Olağanüstü durum kurtarma çoğaltmaişlemini izleme

Depolama çoğaltma ilerleme nizin durumunu izlemek için `azure_hana_replication_status`komut dosyasını çalıştırın. Bu komut, beklendiği gibi çalışması için olağanüstü durum kurtarma konumunda çalışan bir birimden çalıştırılmalıdır. Komut, çoğaltma etkin olup olmadığı önemli değil. Komut, olağanüstü durum kurtarma konumundaki kiracınızın her HANA Büyük Örnek birimi için çalıştırılabilir. Önyükleme hacmi hakkında ayrıntılı bilgi edinmek için kullanılamaz. 

Komut ve çıktısı hakkında daha fazla bilgi için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.2/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.2.1.pdf)"DR çoğaltma durumunu al - azure_hana_replication_status" bilgisine bakın.


## <a name="next-steps"></a>Sonraki adımlar
- [Bkz. Monitör ve HANA tarafında sorun giderme](hana-monitor-troubleshoot.md).
