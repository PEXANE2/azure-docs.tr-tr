---
title: Azure Site Kurtarma'da Hyper-V olağanüstü durum kurtarma mimarisi
description: Bu makalede, Azure Site Kurtarma hizmetiyle şirket içi Hyper-V V M'ler (VMM olmadan) için olağanüstü durum kurtarma dağıtımı nda kullanılan bileşenlere ve mimariye genel bir bakış sunulmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: 022d6edad1e907173dfde3481e60d2523be087a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082660"
---
# <a name="hyper-v-to-azure-disaster-recovery-architecture"></a>Hyper-V'den Azure'a olağanüstü durum kurtarma mimarisi


Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi Hyper-V ana bilgisayarları ile Azure arasında Hyper-V sanal makinelerini (VM' ler) çoğaltırken, başarısız olduğunuzda ve kurtardığınızda kullanılan mimari ve işlemler açıklanmaktadır.

Hyper-V ana bilgisayarları isteğe bağlı olarak System Center Virtual Machine Manager (VMM) özel bulutlarında yönetilebilir.



## <a name="architectural-components---hyper-v-without-vmm"></a>Mimari bileşenler - VMM'siz Hyper-V

Aşağıdaki tablo ve grafik, Hyper-V ana bilgisayarlarının VMM tarafından yönetilmediğinde, Hyper-V'nin Azure'a çoğaltılması için kullanılan bileşenlerin üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Şey**
--- | --- | ---
**Azure** | Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi VM iş yüklerinden çoğaltılan veriler depolama hesabında depolanır. Azure Sanal Makineleri, şirket içi sitenizden kaynaklandığında çoğaltılan iş yükü verileriyle oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Hyper-V** | Site Kurtarma dağıtımı sırasında, Hyper-V ana bilgisayarlarını ve kümeleri Hyper-V sitelerine toplarsınız. Azure Site Kurtarma Sağlayıcısı ve Kurtarma Hizmetleri aracısını her bağımsız Hyper-V ana bilgisayarına veya her Hyper-V küme düğümüne yüklersiniz. | Sağlayıcı, İnternet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Kurtarma Hizmetleri aracısı, veri çoğaltma işlemini gerçekleştirir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V Sanal Makineleri** | Hyper-V'de çalışan bir veya daha fazla VM. | VM'lere hiçbir şeyin açıkça yüklenmesi gerekmez.


**Hyper-V'den Azure mimarisine (VMM olmadan)**

![Mimari](./media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)


## <a name="architectural-components---hyper-v-with-vmm"></a>Mimari bileşenler - VMM ile Hyper-V

Aşağıdaki tablo ve grafik, Hyper-V ana bilgisayarları VMM bulutlarında yönetildiğinde Hyper-V'nin Azure'a çoğaltılması için kullanılan bileşenlerin üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Şey**
--- | --- | ---
**Azure** | Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi VM iş yüklerinden çoğaltılan veriler depolama hesabında depolanır. Azure Sanal Taşıtları, şirket içi sitenizden kaynaklandığında çoğaltılan verilerle oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM sunucusu, Hyper-V konakları içeren bir veya daha fazla bulut içerir. | Site Kurtarma ile çoğaltma yı düzenlemek ve sunucuyu Kurtarma Hizmetleri kasasına kaydetmek için Site Kurtarma Sağlayıcısı'nı VMM sunucusuna yüklersiniz.
**Hyper-V konağı** | VMM tarafından yönetilen bir veya daha fazla Hyper-V konağı/kümesi. |  Kurtarma Hizmetleri aracısını her Hyper-V ana bilgisayar veya küme düğümüne yüklersiniz.
**Hyper-V Sanal Makineleri** | Hyper-V konağı sunucusunda çalışan bir veya daha fazla VM. | VM'lere açıkça bir şey yüklenmesi gerekmez.
**Ağ Oluşturma** | VMM sunucusunda ayarlanmış mantıksal ağlar ve VM ağları. VM ağı bulutla ilişkili mantıksal bir ağa bağlanmalıdır. | VM ağları Azure sanal ağlarına eşlenir. Azure VM'ler başarısız olduktan sonra oluşturulduğunda, VM ağına eşlenen Azure ağına eklenirler.

**Hyper-V'den Azure mimarisine (VMM ile)**

![Bileşenler](./media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)



## <a name="replication-process"></a>Çoğaltma işlemi

![Hyper-V'den Azure çoğaltmasına](./media/hyper-v-azure-architecture/arch-hyperv-azure-workflow.png)

**Çoğaltma ve kurtarma işlemi**


### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.      ![İş](media/hyper-v-azure-architecture/image1.png) ![ilanları listesi Koruma sondajı sağlama](media/hyper-v-azure-architecture/image2.png)


### <a name="initial-data-replication"></a>İlk veri çoğaltma

1. İlk çoğaltma tetiklendiğinde, [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) alınır.
2. VM'deki sanal sabit diskler, tümü Azure'a kopyalanana kadar tektek çoğaltılır. VM boyutuna ve ağ bant genişliğine bağlı olarak bu işlem biraz zaman alabilir. Ağ bant genişliğini [nasıl artıracağınızı öğrenin.](https://support.microsoft.com/kb/3056159)
3. İlk çoğaltma devam ederken disk değişiklikleri gerçekleşirse, Hyper-V Çoğaltma İzleyicisi değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) olarak izler. Bu günlük dosyaları disklerle aynı klasörde bulunur. Her diskin ikincil depolama alanına gönderilen ilişkili bir .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection-process"></a>Koruma işlemini sonuçlandırın

1. İlk çoğaltma bittikten sonra, sanal makine iş **üzerinde Finalize koruma** çalışır. VM'nin korunması için ağ ve diğer çoğaltma sonrası ayarları yapılandırır.
2. Bu aşamada, başarısız olmaya hazır olduğundan emin olmak için VM ayarlarını kontrol edebilirsiniz. Beklendiği gibi başarısız olup olmadığını kontrol etmek için VM için bir olağanüstü durum kurtarma matkabı (test failover) çalıştırabilirsiniz. 


## <a name="delta-replication"></a>Değişim çoğaltması

1. İlk çoğaltmadan sonra, çoğaltma ilkesine uygun olarak delta çoğaltma başlar.
2. Hyper-V Çoğaltma Çoğaltma İzleyicisi, sanal sabit diskteki değişiklikleri .hrl dosyaları olarak izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır.
3. Günlük müşterinin depolama hesabına gönderilir. Bir günlük Azure'a aktarılırken, birincil diskteki değişiklikler aynı klasördeki başka bir günlük dosyasında izlenir.
4. İlk ve delta çoğaltma sırasında Azure portalında VM'yi izleyebilirsiniz.

### <a name="resynchronization-process"></a>Yeniden senkronizasyon süreci

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir.
    - Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
    -  Varsayılan olarak yeniden eşitleme, çalışma saatleri dışında otomatik olarak çalışacak şekilde zamanlanır.
1.  Resynchronization yalnızca delta veri gönderir.
    - Kaynak ve hedef VM'lerin kontrolleri ile gönderilen veri miktarını en aza indirir.
    - Kaynak ve hedef dosyalarının sabit parçalara bölündüğü sabit bloklu bir yığın algoritması kullanır.
    - Her öbek için checksums oluşturulur. Bunlar, kaynaktan hangi blokların hedefe uygulanması gerektiğini belirlemek için karşılaştırılır.
2. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir.
3. Saat dışında varsayılan yeniden eşitleme beklemek istemiyorsanız, vm'yi el ile yeniden eşitleyebilirsiniz. Örneğin, bir kesinti oluşursa. Bunu yapmak için Azure portalında VM > **Resynchronize'i**seçin.

    ![El ile yeniden eşitleme](./media/hyper-v-azure-architecture/image4-site.png)


### <a name="retry-process"></a>Yeniden deneme süreci

Bir çoğaltma hatası meydana gelirse, yerleşik yeniden deneme işlevi vardır. Retry tabloda açıklandığı gibi sınıflandırılır.

**Kategori** | **Şey**
--- | ---
**Kurtarılamaz hatalar** | Yeniden deneme yapılmaya çalışılmaz. VM durumu **Kritik** olacaktır ve yönetici müdahalesi gerekir.<br/><br/> Bu hatalara örnek olarak bozuk bir VHD zinciri, yineleme VM için geçersiz bir durum, ağ kimlik doğrulama hataları, yetkilendirme hataları ve VM bulunamadı hataları (bağımsız Hyper-V sunucuları için.
**Kurtarılabilir hatalar** | Yeniden deneme aralığını ilk denemenin başlangıcına göre 1, 2, 4, 8 ve 10 dakika artıran bir üstel geri çekme kullanılarak her çoğaltma aralığında yeniden denemeler gerçekleşir. Hata devam ederse, 30 dakikada bir yeniden deneyin. Bunlara örnek olarak ağ hataları, düşük disk hataları ve düşük bellek koşulları verilebilir.



## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi Hyper-V VM’lerinden Azure’a planlanmış veya planlanmamış bir yük devretme gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır. Birincil sitenize erişilemiyorsa planlanmamış bir başarısızlık çalıştırın.
2. Birden çok makinenin başarısız olmasını sağlamak için tek bir makinede başarısız olabilir veya kurtarma planları oluşturabilirsiniz.
3. Başarısız oluyorsun. Failover'ın ilk aşaması tamamlandıktan sonra, oluşturulan çoğaltma VM'lerini Azure'da görebilirsiniz. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
4. Daha sonra, azure VM çoğaltmadan iş yüküne erişmeye başlamak için başarısız olmayı taahhüt elersiniz.

Şirket içi altyapınız yeniden çalışmaya başladıktan sonra başarısız olabilirsiniz. Failback üç aşamada oluşur:

1. Azure'dan şirket içi siteye planlanan bir başarısızlık başlatın:
    - **Kapalı kalma süresini en aza indirmek**: Bu seçeneği kullanırsanız Site Kurtarma, başarısız olmadan önce verileri eşitler. Değiştirilen veri bloklarını denetler ve bunları şirket içi siteye indirirken, Azure VM çalışmaya devam ederek kapalı kalma süresini en aza indirir. Başarısız ın tamamlanması gerektiğini el ile belirttiğiniz zaman, Azure VM kapatılır, son delta değişiklikleri kopyalanır ve hata geçersiz liği başlar.
    - **Tam indir :** Bu seçenek ile veriler failover sırasında senkronize edilir. Bu seçenek tüm diski karşıdan yükler. Daha hızlıdır, çünkü hiçbir çek kontrol süresi hesaplanmaz, ancak daha fazla kapalı kalma süresi vardır. Azure VM'lerini bir süredir çalıştırıyorsanız veya şirket içi VM silinmişse bu seçeneği kullanın.
    - **Create VM**: Aynı VM'ye veya alternatif bir VM'ye geri dönmeyi seçebilirsiniz. Site Kurtarma'nın vm'yi oluşturması gerektiğini belirtebilirsiniz.

2. İlk eşitleme tamamlandıktan sonra, başarısız olmayı tamamlamak için seçin. Tamamlandıktan sonra, her şeyin beklendiği gibi çalıştığını kontrol etmek için şirket içi VM'de oturum açabilirsiniz. Azure portalında, Azure VM'lerinin durdurulduğunu görebilirsiniz.
3.  Daha sonra, sona ermek için başarısız taahhüt ve şirket içi VM'den iş yüküne yeniden erişmeye başlarsınız.
4. İş yükleri geri başarısız olduktan sonra ters çoğaltmayı etkinleştirirsiniz, böylece şirket içi VM'ler yeniden Azure'da çoğaltılır.



## <a name="next-steps"></a>Sonraki adımlar


Hyper-V'den Azure çoğaltmasına başlamak için [bu öğreticiyi](tutorial-prepare-azure.md) izleyin.


