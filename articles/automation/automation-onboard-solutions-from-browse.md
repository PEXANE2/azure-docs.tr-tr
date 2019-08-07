---
title: Azure Otomasyonu 'nda birden çok VM için Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümlerini nasıl ekleyeceğinizi öğrenin
description: Azure Otomasyonu 'nun bir parçası olan Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümleriyle Azure sanal makinesini nasıl ekleyeceğinizi öğrenin
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/11/2019
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: cf063311e5559ddf5706df397ce744a726610000
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667338"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Birden çok VM 'de Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümlerini etkinleştirme

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetmek, değişiklikleri izlemek ve bilgisayarlarınıza nelerin yüklü olduğunu envantere almak için çözümler sağlar. Makineleri eklemek için birden çok yol vardır, çözümü [bir sanal makineden](automation-onboard-solutions-from-vm.md), [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), sanal makinelere gözatarken veya [runbook](automation-onboard-solutions.md)'a ekleyebilirsiniz. Bu makalede, Azure 'daki sanal makinelere göz atarken bu çözümleri ekleme ele alınmaktadır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure'da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Azure portal, **sanal makineler**' e gidin.

Onay kutularını kullanarak Değişiklik İzleme ve envanter veya Güncelleştirme Yönetimi birlikte eklemek istediğiniz sanal makineleri seçin. Tek seferde üç farklı kaynak grubu için ekleme kullanılabilir. Azure VM 'Ler, Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın herhangi bir bölgede bulunabilir.

![VM 'lerin listesi](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Sanal makinelerin listesini değiştirmek için filtre denetimlerini kullanın ve ardından listedeki tüm sanal makineleri seçmek için üst onay kutusuna tıklayın.

Komut çubuğundan **Hizmetler** ' e tıklayın ve **değişiklik izleme**, **Stok**veya **güncelleştirme yönetimi**seçin.

> [!NOTE]
> **Değişiklik izleme** ve **Stok** , etkinleştirildiğinde aynı çözümü kullanır, diğeri de etkindir.

Aşağıdaki görüntü Güncelleştirme Yönetimi içindir. Değişiklik izleme ve stok aynı düzen ve davranışa sahiptir.

Sanal makinelerin listesi, yalnızca aynı abonelikte ve konumda bulunan sanal makineleri gösterecek şekilde filtrelenmiştir. Sanal makinelerinizde üçten fazla kaynak grubu varsa, ilk üç kaynak grubu seçilir.

### <a name="resource-group-limit"></a>Ekleme sınırlamaları

Ekleme için kullanabileceğiniz kaynak gruplarının sayısı [Kaynak Yöneticisi dağıtım limitleriyle](../azure-resource-manager/resource-manager-cross-resource-group-deployment.md)sınırlıdır. Güncelleştirme dağıtımlarıyla karıştırılmamalıdır Kaynak Yöneticisi dağıtımlar, dağıtım başına 5 kaynak grubu ile sınırlıdır. Ekleme bütünlüğünden emin olmak için bu kaynak gruplarının 2 ' nin, Log Analytics çalışma alanını, Otomasyon hesabını ve ilgili kaynakları yapılandırmak üzere ayrılmıştır. Bu, dağıtım için seçilecek 3 kaynak grubu ile aynı kalır.

Bir runbook 'u ekleme için de kullanabilirsiniz. daha fazla bilgi için bkz. [Azure Otomasyonu 'nda güncelleştirme ve değişiklik izleme çözümlerini](automation-onboard-solutions.md)ekleme.

Farklı aboneliklerden, konumlardan ve kaynak gruplarından sanal makineler seçmek için filtre denetimlerini kullanın.

![Güncelleştirme yönetimi çözümünü ekleme](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Log Analytics çalışma alanı ve Otomasyon hesabı için seçimleri gözden geçirin. Mevcut bir çalışma alanı ve Otomasyon hesabı varsayılan olarak seçilidir. Farklı bir Log Analytics çalışma alanı ve Otomasyon hesabı kullanmak istiyorsanız özel **yapılandırma** sayfasından seçmek için **özel** ' e tıklayın. Bir Log Analytics çalışma alanı seçtiğinizde, bir Otomasyon hesabıyla bağlanıp bağlanmadığını belirlemek için bir onay yapılır. Bağlı bir Otomasyon hesabı bulunursa, aşağıdaki ekranı görürsünüz. İşiniz bittiğinde **Tamam**' a tıklayın.

![Çalışma alanını ve hesabı seçin](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Seçilen çalışma alanı bir Otomasyon hesabına bağlı değilse, aşağıdaki ekranı görürsünüz. Bir Otomasyon hesabı seçin ve tamamlandığında **Tamam** ' a tıklayın.

![Çalışma alanı yok](media/automation-onboard-solutions-from-browse/no-workspace.png)

> [!NOTE]
> Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
>
> Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

Etkinleştirmek istemediğiniz herhangi bir sanal makinenin yanındaki onay kutusunun işaretini kaldırın. Etkinleştirimeyen sanal makinelerin zaten seçimi kaldırılmış.

Çözümü etkinleştirmek için **Etkinleştir** ' e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldır

Aşağıdaki çözümler bir Log Analytics çalışma alanına bağımlıdır:

* [Güncelleştirme yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [VM 'Leri çalışma saatleri dışında Başlat/Durdur](automation-solution-vm-management.md)

Artık Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmenize karar verirseniz, Hesabınızın bağlantısını doğrudan Azure portal kaldırabilirsiniz. Devam etmeden önce önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlemin devam etmesini engellenemez. Kaldırmak için gereken adımları anlamak için, içeri aktardığınız belirli çözüme yönelik makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon Hesabınızın bağlantısını kaldırmak için aşağıdaki adımları uygulayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler Otomasyon varlıkları oluşturmuş olabilir ve çalışma alanının bağlantısı kaldırılmadan önce de kaldırılması gerekebilir.

1. Azure portal Otomasyon hesabınızı açın ve Otomasyon hesabı sayfasında, sol taraftaki **Ilgili kaynaklar** bölümünde **bağlantılı çalışma alanı** ' nı seçin.

2. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır**' a tıklayın.

   ![Çalışma alanının bağlantısını Kaldır sayfası](media/automation-onboard-solutions-from-browse/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızdaki hesabın bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümünü kullandıysanız, isteğe bağlı olarak, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamayı Güncelleştir-her birinin, oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adları olur)

* Çözüm için oluşturulan karma çalışan grupları-her biri, machine1. contoso. com _9ceb8108-26c9-4051-B6B3-227600d715c8 ' e benzer şekilde adlandırılır.

VM 'Leri çalışma saatleri dışında Başlat/Durdur çözümünü kullandıysanız, isteğe bağlı olarak, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM runbook zamanlamalarını başlatma ve durdurma
* VM runbook 'larını başlatma ve durdurma
* Değişkenler

Alternatif olarak, Log Analytics çalışma alanınızdan Otomasyon hesabınızdan çalışma alanınızın bağlantısını da kaldırabilirsiniz. Çalışma alanınızda **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="troubleshooting"></a>Sorun giderme

Birden çok makine eklerken, **etkinleştirilemiyor**olarak gösteren makineler olabilir. Bazı makinelerin etkinleştirilmemesinin farklı nedenleri vardır. Aşağıdaki bölümlerde, eklenmeye çalışılırken bir VM 'de durumu etkinleştirememek için olası nedenler gösterilmektedir.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>VM, farklı bir çalışma alanına rapor verebilir\<:\>' çalışmaalanıadı '.  Yapılandırmayı etkinleştirmek için kullanacak şekilde değiştirin

**Neden**: Bu hata, başka bir çalışma alanına rapor eklemek istediğiniz VM 'nin olduğunu gösterir.

**Çözüm**: Hedeflenen Otomasyon hesabını ve Log Analytics çalışma alanını değiştirmek için **yapılandırma olarak kullan** ' a tıklayın.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>VM, bu abonelikte kullanılamayan bir çalışma alanına raporlar

**Neden**: Sanal makinenin rapor aldığı çalışma alanı:

* Farklı bir abonelikte veya
* Artık yok veya
* Bir kaynak grubunda, erişim izinleriniz yok

**Çözüm**: VM 'nin rapor veren çalışma alanıyla ilişkili Otomasyon hesabını bulun ve kapsam yapılandırmasını değiştirerek sanal makineyi ekleyin.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>VM işletim sistemi sürümü veya dağıtımı desteklenmiyor

**Sağlamak** Çözüm tüm Linux dağıtımları veya tüm Windows sürümlerinde desteklenmez.

**Çözümden** Çözüm için [Desteklenen istemciler listesine](automation-update-management.md#clients) bakın.

### <a name="classic-vms-cannot-be-enabled"></a>Klasik VM 'Ler etkinleştirilemiyor

**Neden**: Klasik dağıtım modelini kullanan sanal makineler desteklenmez.

**Çözüm**: Sanal makineyi Kaynak Yöneticisi dağıtım modeline geçirin. Bunun nasıl yapılacağını öğrenmek için bkz. [klasik dağıtım modeli kaynaklarını geçirme](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>VM durduruldu. iptal

**Neden**: Sanal makine **çalışır** durumda değil.

**Çözüm**: Bir çözüme VM eklemek için VM 'nin çalışıyor olması gerekir. Sayfadan çıkmadan sanal makineyi başlatmak için VM satır içi **Başlat** bağlantısına tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için çözüm etkinleştirilmiş olduğuna göre, makineleriniz için güncelleştirme değerlendirmesini nasıl görüntüleyeceğinizi öğrenmek için Güncelleştirme Yönetimi Genel Bakış makalesini ziyaret edin.

> [!div class="nextstepaction"]
> [Güncelleştirme Yönetimi-güncelleştirme değerlendirmesini görüntüle](./automation-update-management.md#viewing-update-assessments)

Çözümlere ve bunların nasıl kullanılacağına ilişkin öğreticiler ekleme:

* [Öğretici-VM 'niz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici-bir VM 'de yazılım tanımla](automation-tutorial-installed-software.md)

* [Öğretici-VM 'deki değişikliklerle Ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
