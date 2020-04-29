---
title: Azure VM 'den Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümlerini ekleme
description: Azure Otomasyonu 'nun bir parçası olan Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümleriyle Azure sanal makinesini nasıl ekleyeceğinizi öğrenin.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: f33f829b6cb86cb01c848e5fc48e1618a3e00a2c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537041"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Azure sanal makinesinden Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümlerini ekleme

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetmenize, değişiklikleri izlemenize ve bilgisayarlarınıza nelerin yüklü olduğunu izlemenize yardımcı olan çözümler sunar. Makineleri eklemek için birden çok yol vardır. Çözümü bir sanal makineden, [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), [birden çok makineye göz atarak](automation-onboard-solutions-from-browse.md)veya bir [runbook](automation-onboard-solutions.md)kullanarak ekleyebilirsiniz. Bu makalede, Azure sanal makinesinden bu çözümlerin eklenmesi ele alınmaktadır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-the-solutions"></a>Çözümleri etkinleştirme

İlk olarak, VM 'inizdeki çözümlerin birini veya birkaçını etkinleştirin:

1. [Azure Portal](https://portal.azure.com), **sanal makineler** ' i seçin veya giriş sayfasından **sanal makineler** ' i arayıp seçin.
2. Çözümünü etkinleştirmek istediğiniz VM 'yi seçin.
3. VM sayfasında, **işlemler**altında **güncelleştirme yönetimi**, **Envanter**veya **değişiklik izleme**' yi seçin. Otomasyon hesabınızın konumu ne olduğuna bakılmaksızın, sanal makine herhangi bir bölgede bulunabilir. Bir VM 'den bir çözüm eklerken, sanal makinenin bir çalışma alanına eklendi `Microsoft.OperationalInsights/workspaces/read` olup olmadığını belirleme izninizin olması gerekir. Gerekli ek izinler hakkında daha fazla bilgi edinmek için bkz. [makineler eklemek için gereken izinler](automation-role-based-access-control.md#onboarding-permissions). Aynı anda birden çok makine ekleme hakkında bilgi edinmek için bkz. [güncelleştirme yönetimi, değişiklik izleme ve envanter çözümlerini](automation-onboard-solutions-from-automation-account.md)ekleme.

4. Azure Log Analytics çalışma alanını ve otomasyon hesabını seçin ve ardından çözümü etkinleştirmek için **Etkinleştir** ' e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Güncelleştirme Yönetimi çözümü ekleme](media/automation-tutorial-update-management/manageupdates-update-enable.png)

5. Diğer çözümlere gidin ve **Etkinleştir**' i seçin. Log Analytics çalışma alanı ve Otomasyon hesabı açılır listeleri devre dışıdır çünkü bu çözümler, önceden etkinleştirilmiş çözümle aynı çalışma alanını ve otomasyon hesabını kullanır.

> [!NOTE]
> Değişiklik izleme ve stok aynı çözümü kullanır. Bu çözümlerden biri etkinleştirildiğinde, diğeri de etkinleştirilir.

## <a name="scope-configuration"></a>Kapsam yapılandırması

Her çözüm, çözümü alan bilgisayarları hedeflemek için çalışma alanında bir kapsam yapılandırması kullanır. Kapsam yapılandırması, çözümün kapsamını belirli bilgisayarlarla sınırlandırmak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Kapsam yapılandırmalarına erişmek için:

1. Otomasyon hesabınızda, **ilgili kaynaklar**altında, **çalışma alanı**' nı seçin. 
2. Çalışma alanında, **çalışma alanı veri kaynakları**altında **kapsam yapılandırması**' nı seçin.
3. Seçilen çalışma alanında Güncelleştirme Yönetimi veya Değişiklik İzleme çözümü yoksa, aşağıdaki kapsam konfigürasyonları oluşturulur:

    * `MicrosoftDefaultScopeConfig-ChangeTracking`
    * `MicrosoftDefaultScopeConfig-Updates`

    Seçilen çalışma alanında çözüm zaten varsa, çözüm yeniden dağıtılır ve kapsam yapılandırması eklenmez.

4. Yapılandırmaların herhangi birinde üç nokta (**...**) simgesini seçin ve ardından **Düzenle**' ye tıklayın. 
5. **Kapsam yapılandırmasını düzenle** bölmesinde **bilgisayar gruplarını Seç**' i seçin. **Bilgisayar grupları** bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilmiş aramaları gösterir.

## <a name="saved-searches"></a>Kayıtlı aramalar

Güncelleştirme Yönetimi, Değişiklik İzleme veya envanter çözümüne bir bilgisayar eklendiğinde, bilgisayar çalışma alanınızdaki iki kayıtlı aramadan birine eklenir. Kayıtlı aramalar, bu çözümler için hedeflenen bilgisayarları içeren sorgulardır.

Çalışma alanınıza gidin. **Genel**altında **kaydedilmiş aramalar**' ı seçin. Bu çözümler tarafından kullanılan iki kayıtlı arama aşağıdaki tabloda gösterilmiştir:

|Adı     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik izleme dosyanız       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grubu doldurmak için kullanılan sorguyu görüntülemek için kayıtlı aramalardan birini seçin. Aşağıdaki görüntüde sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Aşağıdaki çözümler bir Log Analytics çalışma alanına bağımlıdır:

* [Güncelleştirme Yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma](automation-solution-vm-management.md)

Artık Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmenize karar verirseniz, Hesabınızın bağlantısını doğrudan Azure portal kaldırabilirsiniz.  Devam etmeden önce önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlemin devam etmesini engellenemez. Kaldırmak için gereken adımları anlamak için, içeri aktardığınız belirli çözüme yönelik makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon Hesabınızın bağlantısını kaldırmak için aşağıdaki adımları uygulayabilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler Otomasyon varlıkları oluşturmuş olabilir ve çalışma alanının bağlantısı kaldırılmadan önce de kaldırılması gerekebilir.

1. Azure portal Otomasyon hesabınızı açın ve soldaki **Ilgili kaynaklar** bölümünde **bağlantılı çalışma alanı** ' nı seçin.

2. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır**' a tıklayın.

   ![Çalışma alanının bağlantısını Kaldır sayfası](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızdaki hesabın bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümünü kullandıysanız, isteğe bağlı olarak, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamayı Güncelleştir-her biri, oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adlara sahip olur.

* Çözüm için oluşturulan karma çalışan grupları-her biri, machine1. contoso. com_9ceb8108-26c9-4051-B6B3-227600d715c8 ' e benzer şekilde adlandırılır.

VM'leri çalışma saatleri dışında başlat/durdur çözümünü kullandıysanız, isteğe bağlı olarak, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM runbook zamanlamalarını başlatma ve durdurma
* VM runbook 'larını başlatma ve durdurma
* Değişkenler

Alternatif olarak, Log Analytics çalışma alanınızdan Otomasyon hesabınızdan çalışma alanınızın bağlantısını da kaldırabilirsiniz. Çalışma alanınızda **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sanal makineyi Güncelleştirme Yönetimi kaldırmak için:

* Log Analytics çalışma alanınızda, VM 'yi, kapsam yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan kaldırın. Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.
* [Windows için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Bunları nasıl kullanacağınızı öğrenmek için çözümler için öğreticilere geçin:

* [Öğretici-VM 'niz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici-bir VM 'de yazılım tanımla](automation-tutorial-installed-software.md)

* [Öğretici-VM 'deki değişikliklerle Ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
