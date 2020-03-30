---
title: Azure Site Kurtarma ile Azure'dan Hyper-V V'leri geri başarısız
description: Azure Site Kurtarma ile Azure'dan şirket içi bir siteye Hyper-V VM'leri geri verme nasıl başarısız olun.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 09/12/2019
ms.author: rajanaki
ms.openlocfilehash: 4b005ae308576db6fd26fcf079161430b266ec3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281788"
---
# <a name="run-a-failback-for-hyper-v-vms"></a>Hyper-V VM’leri için yeniden çalışma işlemi çalıştırma

Bu makalede, Hyper-V VM'lerin şirket içi bir siteden Azure'a geçememesi sonrasında oluşturulan Azure VM'lerinin Azure [Site Kurtarma](site-recovery-overview.md)ile nasıl başarısız oluncaya kadar başarısız olunması açıklanmaktadır.

- Azure'dan şirket içi siteye planlanmış bir başarısızlık çalıştırarak Azure'dan Hyper-V VM'leri geri alatırsınız. Başarısız yön Azure'dan şirket içiyse, geri dönüş olarak kabul edilir.
- Azure son derece kullanılabilir bir ortam olduğundan ve VM'ler her zaman kullanılabilir olduğundan, Azure'dan geri dönüş planlanmış bir etkinliktir. İş yüklerinin şirket içinde yeniden çalışmaya başlayabilmesi için küçük bir kapalı kalma süresi planlayabilirsiniz. 
- Planlanan geri dönüş Azure'daki VM'leri kapatır ve en son değişiklikleri karşıdan yükler. Veri kaybı beklenmiyor.

## <a name="before-you-start"></a>Başlamadan önce

1. Kullanabileceğiniz [geri dönüş türlerini gözden geçirin](failover-failback-overview.md#hyper-v-reprotectionfailback) - özgün konum kurtarma ve alternatif konum kurtarma.
2. Azure VM'lerinin yönetilen diskler değil, bir depolama hesabı kullandığından emin olun. Yönetilen diskler kullanılarak çoğaltılan Hyper-V VM'lerin geri tepmesi desteklenmez.
3. Şirket içi Hyper-V ana bilgisayar (veya Site Kurtarma ile kullanıyorsanız System Center VMM sunucusu) çalışıyor ve Azure'a bağlı olup olmadığını kontrol edin. 
4. Bu failover ve taahhüt VM'ler için tam olduğundan emin olun. Azure'dan Hyper-V V'm'lerin geri alınması için belirli bir Site Kurtarma bileşeni ayarlamanız gerekmez.
5. Veri eşitlemesi tamamlamak ve şirket içi VM'yi başlatmak için gereken süre bir dizi etkene bağlıdır. Veri karşıdan yüklemeyi hızlandırmak için, Microsoft Kurtarma Hizmetleri aracısını karşıdan yüklemeyi paralelleştirmek için daha fazla iş parçacığı kullanacak şekilde yapılandırabilirsiniz. [Daha fazla bilgi edinin](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage).


## <a name="fail-back-to-the-original-location"></a>Özgün konuma geri dön

Azure'daki Hyper-V V M'leri orijinal şirket içi VM'ye geri döndürebilmek için, Azure'dan şirket içi siteye planlı bir arıza çalıştırın:

1. **Çoğaltılan öğeleri**> kasasında VM'yi seçin. VM > Planlı **Failover'a**sağ tıklayın. Kurtarma planını geri döndürtüyseniz, plan adını seçin ve **Failover** > **Planlı Failover'ı**tıklatın.
2. **Planlı Failover'ı Onayla'da**kaynak ve hedef konumları seçin. Başarısız yöne dikkat edin. Birincil den failover beklendiği gibi çalıştı ve tüm sanal makineler ikincil konumda ise bu sadece bilgi içindir.
3. **Veri Eşitleme'de**bir seçenek seçin:
    - Verileri başarısız olmadan **önce eşitleyin (yalnızca delta değişikliklerini eşitleyin)**—Bu seçenek, vm'leri kapatmadan eşitleme den en aza indirir.
        - **Aşama 1**: Azure VM'nin anlık görüntüsünü alır ve şirket içi Hyper-V ana bilgisayara kopyalar. Makine Azure'da çalışmaya devam ediyor.
        - **Aşama 2**: Azure VM'yi kapatır, böylece orada yeni değişiklikler olmaz. Delta değişikliklerinin son kümesi şirket içi sunucuya aktarılır ve şirket içi VM başlatılır.
    - **Yalnızca başarısız (tam indirme) sırasında verileri senkronize edin**—Bu seçenek daha hızlıdır çünkü diskin çoğunun değiştiğini varsayıyoruz ve checksum'ları hesaplayarak zaman harcamak istemiyoruz. Bu seçenek herhangi bir checksum hesaplaması gerçekleştirmez.
        - Diskin karşıdan yükleniyor. 
        - Azure'u bir süredir (bir ay veya daha uzun) çalıştırıyorsanız veya şirket içi VM silinmişse bu seçeneği kullanmanızı öneririz.

4. Yalnızca VMM için, bulut için veri şifreleme sayılsa, **Şifreleme Anahtarı'nda,** VMM sunucusunda Sağlayıcı yüklemesi sırasında veri şifrelemeyi etkinleştirdiğinizde verilen sertifikayı seçin.
5. Yük devretmeyi başlatın. Yük devretme işleminin ilerleme durumunu **İşler** sekmesinden takip edebilirsiniz.
6. Başarısız olmadan önce verileri eşitleme seçeneğini seçtiyseniz, ilk veri eşitlemesi tamamlandıktan sonra ve Azure'daki sanal makineleri kapatmaya hazırsanız, **İş** > iş adını > **Complete Failover'ı**tıklatın. Bu aşağıdakileri yapar:
    - Azure makinesini kapatır.
    - En son değişiklikleri şirket içi VM'ye aktarın.
    - Şirket içi VM'yi başlatır.
7. Artık beklendiği gibi kullanılabilir olup olmadığını kontrol etmek için şirket içi VM makinesinde oturum açabilirsiniz.
8. Sanal makine bekleyen bir durumda. Başarısız olmak için **Commit'i** tıklatın.
9. Hata geri ödemesini tamamlamak için, şirket içi VM'yi yeniden Azure'a çoğaltmaya başlamak için **Ters Çoğaltma'yı** tıklatın.



## <a name="fail-back-to-an-alternate-location"></a>Alternatif bir konuma geri dön 

Aşağıdaki gibi alternatif bir konuma geri başarısız:

1. Yeni donanım ayarlıyorsanız, [Windows'un desteklenen](hyper-v-azure-support-matrix.md#replicated-vms)bir sürümünü ve Hyper-V rolünü makineye yükleyin.
2. Orijinal sunucuda sahip olduğunuz ada sahip bir sanal ağ anahtarı oluşturun.
3. **Korumalı Öğeler** > Koruma Grubu Koruma \<Grubu'ndaGroupName**Protection Group** > \<> -> VirtualMachineName>, başarısız olmak istediğiniz VM'yi seçin ve ardından **Planlı Failover'ı**seçin.
4. **Planlı Failover'ı Onayla'da,** **yoksa şirket içi sanal makine oluştur'u**seçin.
5. **Ana Bilgisayar Adı'nda,** VM'yi yerleştirmek istediğiniz yeni Hyper-V ana bilgisayar sunucusunu seçin.
6. **Veri Eşitleme'de,** başarısız olmadan önce verileri eşitleme seçeneğini seçmenizi öneririz. Bu, VM'leri kapatmadan eşitleden izin süresini en aza indirir. Aşağıdakileri yapar:
    - **Aşama 1**: Azure VM'nin anlık görüntüsünü alır ve şirket içi Hyper-V ana bilgisayara kopyalar. Makine Azure'da çalışmaya devam ediyor.
    - **Aşama 2**: Azure VM'yi kapatır, böylece orada yeni değişiklikler olmaz. Son değişiklik kümesi şirket içi sunucuya aktarılır ve şirket içi sanal makine başlatılır.
    
7. Failover (failback) başlatmak için onay işaretini tıklatın.
8. İlk eşitleme bittikten ve Azure VM'yi kapatmaya hazır olduktan sonra, **İş** > \<Planlı Başarısız İş> > **Complete Failover'ı**tıklatın. Bu, Azure makinesini kapatır, şirket içi VM'ye en son değişiklikleri aktarAbilir ve başlatır.
9. Her şeyin beklendiği gibi çalıştığını doğrulamak için şirket içi VM'de oturum açabilirsiniz.
10. Başarısız olmayı bitirmek için **Commit'i** tıklatın. Commit, Azure VM'sini ve disklerini siler ve şirket içi VM'yi yeniden korunmaya hazırlar.
10. Şirket içi VM'yi Azure'a çoğaltmaya başlamak için **Ters Çoğaltma'yı** tıklatın. Azure'da VM kapatıldığından beri yalnızca delta değişiklikleri çoğaltılır.

    > [!NOTE]
    > Veri eşitleme sırasında geri dönüş işini iptal ederseniz, şirket içi VM bozuk durumda olacaktır. Bunun nedeni, veri eşitlemesi Azure VM disklerinden gelen en son verileri şirket içi veri disklerine kopyalar ve eşitleme tamamlanana kadar disk verileri tutarlı bir durumda olmayabilir. Şirket içi VM, veri eşitlemesi iptal edildikten sonra başlarsa, önyükleme olmayabilir. Bu durumda, veri eşitlemesi tamamlamak için failover yeniden çalıştırın.


## <a name="next-steps"></a>Sonraki adımlar
Şirket içi VM Azure'a çoğaldıktan sonra, gerektiğinde Azure'da [başka bir hata yürütebilirsiniz.](site-recovery-failover.md)
