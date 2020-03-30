---
title: Azure Site Kurtarma ile Hyper-V olağanüstü durum kurtarma için kapasite planlama
description: Azure Site Kurtarma hizmetiyle olağanüstü durum kurtarma kurarken kapasiteyi tahmin etmek için bu makaleyi kullanın.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: 843d5da26d6791cea880e5dfb654fe27b74f5d9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73936039"
---
# <a name="plan-capacity-for-hyper-v-vm-disaster-recovery"></a>Hyper-V VM olağanüstü durum kurtarma için plan kapasitesi 

Hyper-V'den Azure dağıtımına yönelik [Azure Sitesi Kurtarma Dağıtım Planlayıcısı] (site-recovery-hyper-v-deployment-planner.md) aşağıdakileri sağlar:

* Disk sayısına, disk boyutuna, IOPS'ye, karmaşaya ve birkaç VM özelliğine bağlı olarak VM uygunluk değerlendirmesi
* Ağ bant genişliği ile RPO değerlendirmesi karşılaştırması
* Azure altyapı gereksinimleri
* Şirket içi altyapı gereksinimleri
* İlk çoğaltma işlem grubu oluşturma rehberi
* Azure için tahmini toplam olağanüstü durum kurtarma maliyeti


Azure Site Kurtarma Kapasite Planlayıcısı, Hyper-V VM'leri Azure Site Kurtarma ile çoğaltırken kapasite gereksinimlerinizi belirlemenize yardımcı olur.

Kaynak ortamınızı ve iş yüklerinizi analiz etmek için Site Kurtarma Kapasite Planlayıcısı'nı kullanın. Bant genişliği gereksinimlerini, kaynak konum için ihtiyacınız olan sunucu kaynaklarını ve hedef konumda ihtiyacınız olan kaynakları (VM'ler ve depolama gibi) tahmin edebilirsiniz.

Aracı iki modda çalıştırabilirsiniz:

* **Hızlı planlama**: Ortalama vm, disk, depolama ve değişim oranına göre ağ ve sunucu projeksiyonları sağlar.
* **Ayrıntılı planlama**: VM düzeyinde her iş yükünün ayrıntılarını sağlar. VM uyumluluğunu analiz edin ve ağ ve sunucu projeksiyonları alın.

## <a name="before-you-start"></a>Başlamadan önce

* VM'ler, VM başına diskler, disk başına depolama dahil olmak üzere ortamınız hakkında bilgi toplayın.
* Çoğaltılan veriler için günlük değişim (karmaşa) oranınızı belirleyin. Değişim oranını almak için [Hyper-V kapasite planlama aracını](https://www.microsoft.com/download/details.aspx?id=39057) indirin. Bu araç hakkında [daha fazla bilgi edinin](site-recovery-capacity-planning-for-hyper-v-replication.md). Ortalamaları yakalamak için bu aracı bir hafta boyunca çalıştırmanızı öneririz.


## <a name="run-the-quick-planner"></a>Hızlı Planlayıcıyı Çalıştırın
1. Site [Kurtarma Kapasite Planlayıcısı'nı](https://aka.ms/asr-capacity-planner-excel)indirin ve açın. Makroları çalıştırmanız gerekir. Sizden istendiğinde, düzenlemeyi ve içeriği etkinleştirmek için seçimler yapın.

2. **Planlayıcı türü** liste seç kutusunda **Hızlı Planlayıcı'yı**seçin.

   ![Kullanmaya başlayın](./media/site-recovery-capacity-planner/getting-started.png)

3. Kapasite **Planlayıcısı** çalışma sayfasında, gerekli bilgileri girin. Aşağıdaki ekran görüntüsünde kırmızı ile daire içine alınmış tüm alanları doldurun:

   a. **Senaryonuzu seçin,** **Hyper-V'den Azure'a** veya **VMware/Physical'tan Azure'a**seçin.

   b. **Ortalama günlük veri değişim hızında (%)** [Hyper-V kapasite planlama aracını](site-recovery-capacity-planning-for-hyper-v-replication.md) veya [Site Kurtarma Dağıtım Planlayıcısı'nı](./site-recovery-deployment-planner.md)kullanarak topladığınız bilgileri girin.

   c. Hyper-V VM'leri Azure'da çoğaltTırken **Sıkıştırma** ayarı kullanılmaz. Sıkıştırma için Riverbed gibi üçüncü taraf bir cihaz kullanın.

   d. **Gün içinde Bekletme,** yinelemeler tutmak için ne kadar süre belirtin.

   e. **Sanal makinelerin toplu işlemi için ilk çoğaltmanın tamamlanması gereken saat sayısında** ve ilk çoğaltma toplu işlemi başına sanal makine **sayısı,** ilk çoğaltma gereksinimlerini hesaplamak için kullanılan ayarları girin. Site Kurtarma dağıtıldığında, tüm ilk veri kümesi yüklenir.

   ![Girişler](./media/site-recovery-capacity-planner/inputs.png)

4. Kaynak ortamın değerlerini girdikten sonra görüntülenen çıktı şunları içerir:

   * **Delta çoğaltma için gerekli bant genişliği (Megabit/sn cinsinden)**: Delta çoğaltma için ağ bant genişliği ortalama günlük veri değişim hızı üzerinden hesaplanır.
   * **İlk çoğaltma için gereken bant genişliği (Megabit/sn cinsinden)**: İlk çoğaltma için ağ bant genişliği girdiğiniz ilk çoğaltma değerleri üzerinden hesaplanır.
   * **Depolama gerekli (GBs)**: Toplam Azure depolama gerekli.
   * **Standart Depolamada Toplam IOPS**: Toplam standart depolama hesaplarındaki 8K IOPS birim boyutuna göre hesaplanır. Hızlı Planlayıcı için, sayı tüm kaynak VM diskleri ve günlük veri değişim hızına göre hesaplanır. Ayrıntılı Planlayıcı için sayı, standart Azure VM'lerine eşlenen toplam VM sayısına ve bu Sanal Taşıtlardaki veri değiştirme oranına göre hesaplanır.
   * **Gerekli Standart depolama hesabı sayısı:** VM'leri korumak için gereken toplam standart depolama hesabı sayısı. Standart depolama hesabı, standart depolamadaki tüm VM'lerde 20.000 IOPS'ye kadar tutabilir. Disk başına en fazla 500 IOPS desteklenir.
   * **Gerekli Blob disk sayısı**: Azure depolama da oluşturulan disk sayısı.
   * **Gerekli prim hesap sayısı**: VM'leri korumak için gereken toplam premium depolama hesabı sayısı. Yüksek IOPS'ye (20.000'den fazla) sahip bir kaynak VM'nin birinci sınıf bir depolama hesabına ihtiyacı vardır. Premium depolama hesabı 80.000 IOPS'ye kadar çıkabilir.
   * **Premium Depolamada Toplam IOPS**: Toplam premium depolama hesaplarındaki 256K IOPS birim boyutuna göre hesaplanır. Hızlı Planlayıcı için, sayı tüm kaynak VM diskleri ve günlük veri değişim hızına göre hesaplanır. Ayrıntılı Planlayıcı için sayı, premium Azure VM'lerine (DS ve GS serisi) eşlenen toplam VM sayısına ve bu Sanal Taşıtlardaki veri değiştirme oranına göre hesaplanır.
   * **Gerekli Yapılandırma Sunucusu Sayısı**: Dağıtım için kaç yapılandırma sunucusu nun gerekli olduğunu gösterir.
   * **Gerekli ek İşlem Sunucularının Sayısı**: Varsayılan olarak yapılandırma sunucusunda çalışan işlem sunucusuna ek olarak ek işlem sunucularının gerekli olup olmadığını gösterir.
   * **Kaynakta %100 ek depolama**alanı : Kaynak konumda ek depolama gerekip gerekmediğini gösterir.

      ![Çıktı](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Ayrıntılı Planlayıcıyı Çalıştırın

1. Site [Kurtarma Kapasite Planlayıcısı'nı](https://aka.ms/asr-capacity-planner-excel)indirin ve açın. Makroları çalıştırmanız gerekir. Sizden istendiğinde, düzenlemeyi ve içeriği etkinleştirmek için seçimler yapın.

2. **Planlayıcı türünü seçin'** de liste kutusundan **Ayrıntılı Planlayıcı'yı** seçin.

   ![Başlarken kılavuzu](./media/site-recovery-capacity-planner/getting-started-2.png)

3. İş **Yükü Yeterliliği** çalışma sayfasında, gerekli bilgileri girin. Tüm işaretli alanları doldurmanız gerekir.

   a. **İşlemci Çekirdekleri'nde,** kaynak sunucudaki toplam çekirdek sayısını belirtin.

   b. **Bellek ayırmada (MB'lerde)** kaynak sunucunun RAM boyutunu belirtin.

   c. **NIC**Sayısı'nda, kaynak sunucudaki ağ bağdaştırıcılarının sayısını belirtin.

   d. **Toplam Depolama'da (GB olarak)** VM depolama alanının toplam boyutunu belirtin. Örneğin, kaynak sunucuda her biri 500 GB olan üç disk varsa, toplam depolama alanı boyutu 1.500 GB'dır.

   e. **Ekli disk sayısı,,** kaynak sunucunun toplam disk sayısını belirtin.

   f. **Disk kapasitesi kullanımında (%)** ortalama kullanımı belirtin.

   g. **Günlük veri değişim hızında (%)** kaynak sunucunun günlük veri değişim oranını belirtin.

   h. **Azure VM boyutunu eşlemede,** eşlemek istediğiniz Azure VM boyutunu girin. Bunu el ile yapmak istemiyorsanız, **Compute IaaS VM'leri**seçin. Bir el ile ayar girdikten sonra **Compute IaaS VM'leri**seçerseniz, manuel ayar üzerine yazılmış olabilir. İşlem işlemi, Azure VM boyutundaki en iyi eşleşmeyi otomatik olarak tanımlar.

   ![İş Yükü Yeterlilik çalışma sayfası](./media/site-recovery-capacity-planner/workload-qualification.png)

4. **Compute IaaS VM'leri**seçerseniz, şunları yapar:

   * Zorunlu girişleri doğrular.
   * IOPS'yi hesaplar ve Azure'a çoğaltmaiçin uygun olan her VM için en iyi Azure VM boyutu eşleşmesini önerir. Uygun boyutta bir Azure VM algılanamazsa, bir hata görüntülenir. Örneğin, ekli disk sayısı 65 ise, bir Azure VM'nin en yüksek boyutu 64 olduğundan bir hata görüntülenir.
   * Azure VM için kullanılabilecek bir depolama hesabı önerir.
   * İş yükü için gereken toplam standart depolama hesabı ve premium depolama hesabı sayısını hesaplar. Azure depolama türünü ve kaynak sunucu için kullanılabilecek depolama hesabını görüntülemek için aşağı kaydırın.
   * Bir VM için atanan gerekli depolama türüne (standart veya premium) ve ekli disk sayısına göre tablonun geri kalanını tamamlar ve sıralar. Azure gereksinimlerini karşılayan tüm VM'ler için **VM niteliklimi?** **Yes** Bir VM Azure'a yedeklenemiyorsa, bir hata görüntülenir.

AA'dan AE'ye kadar olan sütunlar çıktıdır ve her VM için bilgi sağlar.

![AE'ye çıkış sütunları AA](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Örnek
Örnek olarak, tabloda gösterilen değerlere sahip altı VM için araç, en iyi Azure VM eşleşmesini ve Azure depolama gereksinimlerini hesaplar ve atar.

![İş Yükü Yeterlilik atamaları](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Örnek çıktıda aşağıdakileri dikkate alar:

  * İlk sütun, VM'ler, diskler ve karmaşa için bir doğrulama sütunudur.
  * Beş VM için iki standart depolama hesabı ve bir premium depolama hesabı gereklidir.
  * Bir veya daha fazla disk 1 TB'den fazla olduğundan VM3 koruma için uygun değildir.
  * VM1 ve VM2 ilk standart depolama hesabını kullanabilir
  * VM4 ikinci standart depolama hesabını kullanabilir.
  * VM5 ve VM6'nın birinci sınıf bir depolama hesabına ihtiyacı var ve her ikisi de tek bir hesap kullanabilir.

    > [!NOTE]
    > Standart ve premium depolama daki IOPS, disk düzeyinde değil, VM düzeyinde hesaplanır. Standart bir VM, disk başına en fazla 500 IOPS işleyebilir. Bir disk için IOPS 500'den büyükse, birinci sınıf depolama alanına ihtiyacınız vardır. Bir disk için IOPS 500'den fazla ysa, ancak toplam VM diskleri için IOPS destek standardı Azure VM sınırları içindeyse, planlayıcı DS veya GS serisi değil standart bir VM seçer. (Azure VM sınırları VM boyutu, disk sayısı, bağdaştırıcı sayısı, CPU ve bellektir.) Haritalama Azure boyutu hücresini uygun DS veya GS serisi VM ile el ile güncelleştirmeniz gerekir.


Tüm bilgiler girilen sonra, Kapasite **Planlayıcısı'nı açmak için planlayıcı aracına veri gönder'i** seçin. İş yükleri, korunmaya uygun olup olmadıklarını göstermek için vurgulanır.

### <a name="submit-data-in-capacity-planner"></a>Kapasite Planlayıcısı'nda veri gönderme
1. **Kapasite Planlayıcısı** çalışma sayfasını açtığınızda, belirttiğiniz ayarlara göre doldurulur. Girişin **İş Yükü Niteliği** çalışma sayfası olduğunu göstermek için **Infra girişleri kaynak** hücresinde "İş Yükü" sözcüğü görüntülenir.

2. Değişiklik yapmak istiyorsanız, **İş Yükü Niteliği** çalışma sayfasını değiştirmeniz gerekir. Ardından, **verileri yeniden planlayıcı aracına gönder'i** seçin.

   ![Kapasite Planlayıcısı](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Sonraki adımlar
Kapasite planlama aracını [nasıl çalıştırıştırılabildiğini öğrenin.](site-recovery-capacity-planning-for-hyper-v-replication.md)
