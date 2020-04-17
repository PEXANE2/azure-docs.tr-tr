---
title: Azure Otomasyonu'nda Güncelleştirme Yönetimi, İzleme yi değiştirme ve Envanter çözümlerinin nasıl dahil olduğunu öğrenin
description: Azure Otomasyonunun bir parçası olan Güncelleştirme Yönetimi, İzlemeyi Değiştirme ve Envanter çözümleriyle Azure Sanal makinesinde nasıl araç layayarı kullanabilirsiniz öğrenin
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 57378005bd668fa9c0f2aea70c411bbf911130db
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457663"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Dahili Güncelleme Yönetimi, Değişiklik İzleme ve Stok çözümleri

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetmek, değişiklikleri izlemek ve bilgisayarlarınızda yüklü olan envanteri yönetmek için çözümler sağlar. Onboard makineleri için birçok yolu vardır, bir [sanal makineden](automation-onboard-solutions-from-vm.md)çözüm onboard olabilir , [birden fazla makine tarama gelen](automation-onboard-solutions-from-browse.md), Otomasyon hesabınızdan, ya da [runbook](automation-onboard-solutions.md)tarafından . Bu makale, otomasyon hesabınızdan bu çözümleri kapsayan.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum https://portal.azure.comaç.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Otomasyon hesabınıza gidin ve **Yapılandırma Yönetimi**altında **Stok** veya **Değiştir izlemeyi** seçin.

Log Analytics çalışma alanı ve Otomasyon hesabını seçin ve çözümü etkinleştirmek için **Etkinleştir'i** tıklatın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Yerleşik Stok çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

> [!NOTE]
> Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
>
> Desteklenen eşleme çiftleri listesi [için, Otomasyon Hesabı ve Günlük Analizi çalışma alanı için Bölge eşleme](how-to/region-mappings.md)bölümüne bakın.

Değişiklik İzleme ve Sayım çözümü, sanal makinelerinizde [değişiklikleri izleme](automation-vm-change-tracking.md) ve [sayım](automation-vm-inventory.md) olanağı sağlar. Bu adımda çözümü bir sanal makine üzerinde etkinleştirirsiniz.

Değişiklik izleme ve stok çözümü onboarding bildirimi tamamlandığında, **Güncelleştirme yönetimi**altında Güncelleştirme **yönetimini** seçin.

Güncelleştirme Yönetimi çözümü, Azure ve karma VM'leriniz için güncelleştirmeleri ve yamaları yönetmenize olanak tanır. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin başarılı bir şekilde uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Çözüm etkinleştir sayfasında, seçilen Log Analytics çalışma alanı önceki adımda kullanılan çalışma alanıyla aynıdır. Update Management çözümüne dahil olmak için **Etkinleştir'i** tıklatın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Dahili güncelleştirme çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Kapsam Yapılandırması

Her çözüm, çözümü alan bilgisayarları hedeflemek için çalışma alanı içinde bir Kapsam Yapılandırması kullanır. Kapsam Yapılandırması, çözümün kapsamını belirli bilgisayarlarla sınırlamak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Kapsam Yapılandırmalarına erişmek için, İlgili **kaynaklar**altındaki Otomasyon hesabınızda **Çalışma Alanı'nı**seçin. Daha sonra Çalışma **Alanı veri kaynakları**altında çalışma alanında, **Kapsam Yapılandırmaları'nı**seçin.

Seçili çalışma alanında henüz Güncelleştirme Yönetimi veya Değişiklik İzleme çözümleri yoksa, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Güncelleştirmeleri**

Seçili çalışma alanı zaten çözüme sahipse, çözüm yeniden dağıtılanmaz ve kapsam yapılandırması eklenmez.

## <a name="saved-searches"></a>Kaydedilen aramalar

Bir bilgisayar Güncelleştirme Yönetimi'ne veya İzleme ve Envanter çözümlerine eklendiğinde, bunlar çalışma alanınızdaki kayıtlı iki aramadan birine eklenir. Kaydedilen bu aramalar, bu çözümleri hedefleyen bilgisayarları içeren sorgulardır.

Günlük Analizi çalışma alanınıza gidin ve **Genel**altında **Kayıtlı aramaları** seçin. Bu çözümler tarafından kullanılan kaydedilmiş iki arama aşağıdaki tabloda görülebilir:

|Adı     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik İzleme       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grubu doldurmak için kullanılan sorguyu görüntülemek için kaydedilmiş aramayı seçin. Aşağıdaki resim sorguyu ve sonuçlarını gösterir:

![Kaydedilen aramalar](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Yerleşik Azure VM'ler

Otomasyon hesabınızdan **Configuration Management**altında **Stok** veya **Değişiklik izlemeyi** seçin veya Update yönetimi altında **güncelleştir** **yönetimini güncelleştirin.**

+ **Azure VM ekle'yi**tıklatın, listeden bir veya daha fazla VM seçin. Etkinleştirilemeyen sanal makineler soluklanır ve seçilemez. Azure VM'leri, Otomasyon Hesabınızın konumu ne olursa olsun herhangi bir bölgede bulunabilir. Güncelleştirme **Yönetimini Etkinleştir** sayfasında **Etkinleştir'i**tıklatın. Bu eylem, seçilen VM'leri bilgisayar grubuna ekler ve çözüm aramasını kaydetti.

![Azure VM'leri etkinleştirme](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Azure'da olmayan makinelerin el ile eklenmesi gerekir. Otomasyon hesabınızdan **Configuration Management**altında **Stok** veya **Değişiklik izlemeyi** seçin veya Update yönetimi altında **güncelleştir** **yönetimini güncelleştirin.**

**Azure olmayan makine ekle'yi**tıklatın. Bu eylem, makinenin çözüme raporlamaya başlayabilmesi [için Windows için Log Analytics aracısını yüklemek ve yapılandırmak için yönergeleri](../azure-monitor/platform/log-analytics-agent.md) içeren yeni bir tarayıcı penceresi açar. Şu anda System Center Operations Manager tarafından yönetilen bir makinede yatmaktaysanız, yeni bir aracı gerekmez ve çalışma alanı bilgileri varolan aracıya girilir.

## <a name="onboard-machines-in-the-workspace"></a>Çalışma alanında yerleşik makineler

Çözümün etkinolabilmesi için, çalışma alanınıza rapor vermiş olan el ile yüklenen makinelerin veya makinelerin Azure Otomasyonu'na eklenmesi gerekir. Otomasyon hesabınızdan **Configuration Management**altında **Stok** veya **Değişiklik izlemeyi** seçin veya Update yönetimi altında **güncelleştir** **yönetimini güncelleştirin.**

**Makineleri Yönet'i**seçin. Bu eylem, **Makineleri Yönet** sayfasını açar. Bu sayfa, çözümü belirli bir makine kümesinde, mevcut tüm makinelerde etkinleştirmenize veya mevcut tüm makineleriçin çözümü etkinleştirmenize ve gelecekteki tüm makinelerde etkinleştirmenize olanak tanır. Makineleri **Yönet** düğmesi, daha önce tüm kullanılabilir ve **gelecekteki makinelerde Etkinleştir**seçeneğini seçtiyseniz soluk olabilir.

![Kaydedilen aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Mevcut tüm makineler

Kullanılabilir tüm makineler için çözümü etkinleştirmek **için, kullanılabilir tüm makinelerde Etkinleştir'i**seçin. Bu eylem, tek tek makine eklemek için denetimi devre dışı katıyor. Bu görev, çalışma alanına rapor eden tüm makinelerin adlarını bilgisayar grubuna ekler ve arama sorgusu nu kaydetti. Seçildiğinde, bu eylem Makineleri **Yönet** düğmesini devre dışı kılabilir.

### <a name="all-available-and-future-machines"></a>Tüm mevcut ve gelecekteki makineler

Mevcut tüm makineler ve gelecekteki makineler için çözümü etkinleştirmek için, **mevcut ve gelecekteki tüm makinelerde Etkinleştir'i**seçin. Bu seçenek, kaydedilen aramaları ve Kapsam Yapılandırmalarını çalışma alanından siler. Bu eylem, çözümü çalışma alanına rapor eden tüm Azure ve Azure olmayan makinelere açar. Seçildiğinde, kapsam yapılandırması kalmadığınız için bu eylem **Makineleri Yönet** düğmesini kalıcı olarak devre dışı kılabilir.

İlk kaydedilen aramaları geri ekleyerek Kapsam Yapılandırmalarını geri ekleyebilirsiniz. Daha fazla bilgi [için, Kaydedilmiş aramalara](#saved-searches)bakın.

### <a name="selected-machines"></a>Seçilen makineler

Bir veya daha fazla makine için çözümü etkinleştirmek **için, seçili makinelerde Etkinleştir'i** seçin ve çözüme eklemek istediğiniz her makinenin yanına **Ekle'yi** tıklatın. Bu görev, bilgisayar grubuna seçilen makine adlarını çözüm için arama sorgusunu kaydettirin.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Aşağıdaki çözümler Bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme Yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Mesai saatleri dışında VM'leri Başlatma/Durdurma](automation-solution-vm-management.md)

Otomasyon hesabınızı artık bir Log Analytics çalışma alanıyla tümleştirmek istemediğinize karar verirseniz, hesabınızın bağlantısını doğrudan Azure portalından çıkarabilirsiniz.  Devam etmeden önce, önce daha önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlemin devam etmesi engellenir. Kaldırmak için gereken adımları anlamak için içe aktardığınız belirli çözüm için makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon hesabınızın bağlantısını kaldırmak için aşağıdaki adımları tamamlayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler otomasyon varlıkları oluşturmuş olabilir ve çalışma alanını bağlamadan önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve Otomasyon hesabı sayfasında soldaki **İlgili Kaynaklar** etiketli bölümün altında Bağlantılı **çalışma alanı** seçin.

2. Bağlantıyı Bırakma çalışma alanı sayfasında, **Bağlantıyı Çöz çalışma alanını**tıklatın.

   ![Bağlantıyı açın çalışma alanı sayfası](media/automation-onboard-solutions-from-automation-account/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir istem alırsınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını açmaya çalışırken, **Bildirimler** altındaki ilerlemeyi menüden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümlerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamaları güncelleştir - Her birinde oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adlar olacaktır.

* Çözüm için oluşturulan hibrid işçi grupları - Her benzer machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8) adlandırılacaktır.

Mesai saatleri dışında başlat ve durdur çözümü sırasında VM'leri kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM çalışma kitabı zamanlamalarını başlatın ve durdurun
* VM runbook'larını başlatVe durdur
* Değişkenler

Alternatif olarak, Çalışma alanınızı Otomasyon Hesabınızdan Günlük Analizi çalışma alanınızdan da çıkarabilirsiniz. Çalışma alanınızda, **İlgili Kaynaklar**altında **Otomasyon Hesabı'nı** seçin. Otomasyon Hesabı sayfasında **Bağlantıyı Aç hesabını**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güncelleştirme Yönetimi'nden bir VM kaldırmak için:

* Günlük Analizi çalışma alanınızda, Kapsam Yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan VM'yi kaldırın. Kaydedilen aramalar çalışma alanınızda **Genel** altında bulunabilir.
* Windows [için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanılacağını öğrenmek için çözümlerle ilgili eğitimlere devam edin.

* [Öğretici - VM'niz için Güncellemeleri Yönet](automation-tutorial-update-management.md)

* [Öğretici - VM'deki yazılımı tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - VM'deki sorun giderme değişiklikleri](automation-tutorial-troubleshoot-changes.md)
