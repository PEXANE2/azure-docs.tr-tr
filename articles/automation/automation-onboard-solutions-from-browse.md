---
title: Azure Otomasyonunda birden fazla VM için Güncelleştirme Yönetimi, İzlemeyi Değiştirme ve Envanter çözümlerinin nasıl dahil olduğunu öğrenin
description: Azure Otomasyonunun bir parçası olan Güncelleştirme Yönetimi, İzlemeyi Değiştirme ve Envanter çözümleriyle Azure Sanal makinesinde nasıl araç layayarı kullanabilirsiniz öğrenin
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: f5f18e9365b09f06c1bd4f25a8efe909cc308dad
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537024"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Birden çok VM'de Güncelleştirme Yönetimi, İzlemeyi Değiştir ve Envanter çözümlerini etkinleştirme

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetmek, değişiklikleri izlemek ve bilgisayarlarınızda yüklü olan envanteri yönetmek için çözümler sağlar. Onboard makineleri için birden fazla yolu vardır, [bir sanal makineden](automation-onboard-solutions-from-vm.md)çözüm onboard olabilir , [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), sanal makinelerde gezinirken, ya da [runbook](automation-onboard-solutions.md)tarafından . Bu makalede, Azure'da sanal makinelerde gezinirken bu çözümlere binme yer alıyor.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure'da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Azure portalında Sanal **makinelere**gidin.

Onay kutularını kullanarak, Değişiklik İzleme ve Envanter veya Güncelleme Yönetimi ile dahil olmak istediğiniz sanal makineleri seçin. Onboarding aynı anda en fazla üç farklı kaynak grubu için kullanılabilir. Azure VM'leri, Otomasyon Hesabınızın konumu ne olursa olsun herhangi bir bölgede bulunabilir.

![VM listesi](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Sanal makinelerin listesini değiştirmek için filtre denetimlerini kullanın ve ardından listedeki tüm sanal makineleri seçmek için üstteki onay kutusunu tıklatın.

Komut çubuğundan **Hizmetler'i** tıklatın ve **Izlemeyi Değiştir,** **Envanteri**veya Yönetimi **Güncelleştir'i**seçin.

> [!NOTE]
> İzleme ve Envanteri Değiştir aynı çözümü kullanın. Biri etkinleştirildiğinde, diğeri de etkinleştirilir.

Aşağıdaki resim Güncelleştirme Yönetimi içindir. Değişiklik İzleme ve Envanter aynı düzene ve davranışa sahiptir.

Sanal makinelerin listesi, yalnızca aynı abonelik ve konumda bulunan sanal makineleri göstermek için filtrelenir. Sanal makineleriniz üçten fazla kaynak grubundaysa, ilk üç kaynak grubu seçilir.

### <a name="onboarding-limitations"></a><a name="resource-group-limit"></a>Onboarding sınırlamaları

Onboarding için kullanabileceğiniz kaynak gruplarının sayısı [Kaynak Yöneticisi dağıtım sınırları](../azure-resource-manager/templates/cross-resource-group-deployment.md)ile sınırlıdır. Güncelleştirme dağıtımlarıyla karıştırılmayan Kaynak Yöneticisi dağıtımları, dağıtım başına 5 kaynak grubuyla sınırlıdır. Onboarding bütünlüğünü sağlamak için, bu kaynak gruplarından 2'si Log Analytics çalışma alanını, Otomasyon hesabını ve ilgili kaynakları yapılandırmak için ayrılmıştır. Bu, dağıtım için seçilecek 3 kaynak grubu bırakır. Bu sınır, bir Otomasyon çözümü tarafından yönetilebilen kaynak gruplarının sayısı için değil, yalnızca eşzamanlı onboarding için geçerlidir.

Ayrıca, daha fazla bilgi için yerleşik bir çalışma kitabı kullanabilir, [Dahili güncelleştirmeye bakın ve izleme çözümlerini Azure Otomasyonu olarak değiştirebilirsiniz.](automation-onboard-solutions.md)

Farklı aboneliklerden, konumlardan ve kaynak gruplarından sanal makineleri seçmek için filtre denetimlerini kullanın.

![Dahili Güncelleme yönetimi çözümü](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Log Analytics çalışma alanı ve Otomasyon hesabının seçeneklerini gözden geçirin. Varolan bir çalışma alanı ve Otomasyon Hesabı varsayılan olarak seçilir. Farklı bir Log Analytics çalışma alanı ve Otomasyon Hesabı kullanmak istiyorsanız, **Bunları Özel Yapılandırma** sayfasından seçmek için **CUSTOM'ı** tıklatın. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon Hesabı ile bağlantılı olup olmadığını belirlemek için bir denetim yapılır. Bağlantılı bir Otomasyon Hesabı bulunursa, aşağıdaki ekranı görürsünüz. Bittiğinde, **Tamam'ı**tıklatın.

![Çalışma alanı ve hesap seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Seçilen çalışma alanı bir Otomasyon Hesabı'na bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon Hesabı seçin ve tamamlandığında **Tamam'ı** tıklatın.

![Çalışma alanı yok](media/automation-onboard-solutions-from-browse/no-workspace.png)

> [!NOTE]
> Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
>
> Desteklenen eşleme çiftleri listesi [için, Otomasyon Hesabı ve Günlük Analizi çalışma alanı için Bölge eşleme](how-to/region-mappings.md)bölümüne bakın.

Etkinleştirmek istemediğiniz herhangi bir sanal makinenin yanındaki onay kutusunu seçin. Etkinleştirilen sanal makineler zaten seçili değil.

Çözümü etkinleştirmek için **Etkinleştir'i** tıklatın. Çözümün etkinleştirilmesi 15 dakika sürer.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Aşağıdaki çözümler Bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme Yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Mesai saatleri dışında VM'leri Başlatma/Durdurma](automation-solution-vm-management.md)

Otomasyon hesabınızı artık bir Log Analytics çalışma alanıyla tümleştirmek istemediğinize karar verirseniz, hesabınızın bağlantısını doğrudan Azure portalından çıkarabilirsiniz. Devam etmeden önce, önce daha önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlemin devam etmesi engellenir. Kaldırmak için gereken adımları anlamak için içe aktardığınız belirli çözüm için makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon hesabınızın bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler otomasyon varlıkları oluşturmuş olabilir ve çalışma alanını bağlamadan önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve Otomasyon hesabı sayfasında soldaki **İlgili Kaynaklar** bölümünün altında Bağlantılı **çalışma alanı** seçin.

2. Bağlantıyı Bırakma çalışma alanı sayfasında, **Bağlantıyı Çöz çalışma alanını**tıklatın.

   ![Bağlantıyı açın çalışma alanı sayfası](media/automation-onboard-solutions-from-browse/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını açmaya çalışırken, **Bildirimler** altındaki ilerlemeyi menüden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümlerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamaları güncelleştir - Her birinde oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adlar olacaktır)

* Çözüm için oluşturulan hibrid işçi grupları - Her benzer machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8) adlandırılacaktır.

Mesai saatleri dışında başlat/durdur vm'lerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM çalışma kitabı zamanlamalarını başlatın ve durdurun
* VM runbook'larını başlatVe durdur
* Değişkenler

Alternatif olarak, Çalışma alanınızı Otomasyon Hesabınızdan Günlük Analizi çalışma alanınızdan da çıkarabilirsiniz. Çalışma alanınızda, **İlgili Kaynaklar**altında **Otomasyon Hesabı'nı** seçin. Otomasyon Hesabı sayfasında **Bağlantıyı Aç hesabını**seçin.

## <a name="troubleshooting"></a>Sorun giderme

Birden fazla makineye binerken, '' olarak `Cannot enable`gösteren makineler olabilir. Bazı makinelerin etkinleştirilemememelerinin farklı nedenleri vardır. Aşağıdaki bölümlerde, gemiye `Cannot enable` binmeye çalışırken VM'deki durum için olası nedenler gösterilmektedir.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM farklı bir çalışma alanına\<rapor:\>' workspaceName '.  Etkinleştirmek için yapılandırmayı değiştirme

**Neden**: Bu hata, onboard'da çalıştığınız VM'nin başka bir çalışma alanına rapor verdiğini gösterir.

**Çözüm**: Hedeflenen Otomasyon hesabını ve Log Analytics çalışma alanını değiştirmek için **yapılandırma olarak Kullan'ı** tıklatın.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>VM, bu abonelikte bulunmayan bir çalışma alanına rapor veriyor

**Neden**: Sanal makinenin bildirdiği çalışma alanı:

* Farklı bir abonelikte veya
* Artık yok, ya da
* Bir kaynak grubunda, erişim izniniz yok mu?

**Çözüm**: Kapsam yapılandırmasını değiştirerek VM'nin sanal makineye rapor verdiği çalışma alanıyla ilişkili otomasyon hesabını bulun.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM işletim sistemi sürümü veya dağıtımı desteklenmiyor

**Sebep:** Çözüm, tüm Linux dağıtımları veya Windows'un tüm sürümleri için desteklenmez.

**Çözüm:** Çözüm için [desteklenen istemcilerin listesine](automation-update-management.md#clients) bakın.

### <a name="classic-vms-cannot-be-enabled"></a>Klasik VM'ler etkinleştirilemez

**Neden**: Klasik dağıtım modelini kullanan sanal makineler desteklenmez.

**Çözüm**: Sanal makineyi Kaynak Yöneticisi dağıtım modeline geçirin. Bunu nasıl yapacağınızı öğrenmek [için, bkz.](../virtual-machines/windows/migration-classic-resource-manager-overview.md)

### <a name="vm-is-stopped-deallocated"></a>VM durdurulur. (tahsis edildi)

**Neden**: **Çalışan** durumunda olmayan sanal makine.

**Çözüm**: Bir VM'den çözeltiye binebilmek için VM çalışıyor olmalıdır. Sayfadan uzağa gezinmeden VM'yi başlatmak için **VM'yi başlat** bağlantısını tıklatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güncelleştirme Yönetimi'nden bir VM kaldırmak için:

* Günlük Analizi çalışma alanınızda, Kapsam Yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan VM'yi kaldırın. Kaydedilen aramalar çalışma alanınızda **Genel** altında bulunabilir.
* Windows [için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için çözüm etkinleştirildiğinden, makineleriniz için Güncelleştirme **Dağıtımı'nı** nasıl oluşturabilirsiniz öğrenmek için Yönetime Genel Bakış makalesini ziyaret edin.

> [!div class="nextstepaction"]
> [Update Management - Azure VM'leriniz için güncelleştirmeleri ve yamaları yönetme](./automation-tutorial-update-management.md)

Çözümler ve bunların nasıl kullanılacağı hakkında ek öğreticiler:

* [Öğretici - VM'niz için Güncellemeleri Yönet](automation-tutorial-update-management.md)

* [Öğretici - VM'deki yazılımı tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - VM'deki sorun giderme değişiklikleri](automation-tutorial-troubleshoot-changes.md)
