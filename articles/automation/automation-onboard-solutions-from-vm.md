---
title: Bir Azure VM'den Yerleşik Güncelleme Yönetimi, İzlemeyi Değiştir ve Envanter çözümleri
description: Azure Otomasyonunun bir parçası olan Update Management, Change Tracking ve Inventory çözümleriyle bir Azure sanal makinede nasıl araç lanıngerektiğini öğrenin.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: f33f829b6cb86cb01c848e5fc48e1618a3e00a2c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537041"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Bir Azure sanal makineden Yerleşik Güncelleme Yönetimi, İzlemeyi Değiştir ve Envanter çözümleri

Azure Otomasyonu, işletim sistemi güvenlik güncelleştirmelerini yönetmenize, değişiklikleri izlemenize ve bilgisayarlarınızda yüklenenleri envanterlemenize yardımcı olacak çözümler sunar. Gemideki makineleriçin birden fazla yol vardır. Çözümü sanal bir [makineden, Otomasyon hesabınızdan,](automation-onboard-solutions-from-automation-account.md) [birden fazla makineye göz atarak](automation-onboard-solutions-from-browse.md)veya bir [runbook](automation-onboard-solutions.md)kullanarak dahil edebilirsiniz. Bu makalede, bir Azure sanal makineden bu çözümleri bindirinkapsar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-the-solutions"></a>Çözümleri etkinleştirme

İlk olarak, VM'nizdeki çözümlerden birini veya üçünü etkinleştirin:

1. Azure [portalında](https://portal.azure.com) **Sanal makineleri** seçin veya Ana Sayfa'dan **Sanal makineleri** arayın ve seçin.
2. Bir çözümü etkinleştirmek istediğiniz VM'yi seçin.
3. VM sayfasında, **Operations**altında, **Yönetim Güncelleştir,** **Envanter**veya **İzlemeyi Değiştir'i**seçin. Sanal makine, Otomasyon hesabınızın konumu ne olursa olsun, herhangi bir bölgede bulunabilir. Bir VM'den bir çözüme binerken, `Microsoft.OperationalInsights/workspaces/read` VM'nin bir çalışma alanına bindirilip bindirilen olup olmadığını belirlemek için izin almanız gerekir. Gerekli olan ek izinler hakkında bilgi edinmek [için, yerleşik makineler için gereken izinlere](automation-role-based-access-control.md#onboarding-permissions)bakın. Aynı anda birden çok makinede nasıl bineceğinizi öğrenmek için, [Yerleşik Güncelleme Yönetimi, İzlemeyi Değiştir ve Envanter çözümlerine](automation-onboard-solutions-from-automation-account.md)bakın.

4. Azure Günlük Analizi çalışma alanı ve Otomasyon hesabını seçin ve ardından çözümü etkinleştirmek için **Etkinleştir'i** tıklatın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Güncelleştirme Yönetimi çözümünde yerleşik](media/automation-tutorial-update-management/manageupdates-update-enable.png)

5. Diğer çözümlere gidin ve sonra **Etkinleştir'i**seçin. Log Analytics çalışma alanı ve Otomasyon hesap açılır listeleri devre dışı bırakılır, çünkü bu çözümler daha önce etkinleştirilen çözümle aynı çalışma alanı ve Otomasyon hesabını kullanır.

> [!NOTE]
> İzlemeyi değiştir in ve Envanter aynı çözümü kullanın. Bu çözümlerden biri etkinleştirildiğinde, diğeri de etkinleştirilir.

## <a name="scope-configuration"></a>Kapsam yapılandırması

Her çözüm, çözümü alan bilgisayarları hedeflemek için çalışma alanında bir kapsam yapılandırması kullanır. Kapsam yapılandırması, çözümün kapsamını belirli bilgisayarlarla sınırlamak için kullanılan bir veya daha fazla kayıtlı arama grubudur. Kapsam yapılandırmalarına erişmek için:

1. Otomasyon hesabınızda, **İlgili kaynaklar** **altında, Çalışma Alanı'nı**seçin. 
2. Çalışma alanında, **Çalışma Alanı veri kaynakları**altında Kapsam **Yapılandırmaları'nı**seçin.
3. Seçili çalışma alanında Güncelleştirme Yönetimi veya İzleme çözümünü değiştir zaten yoksa, aşağıdaki kapsam yapılandırmaları oluşturulur:

    * `MicrosoftDefaultScopeConfig-ChangeTracking`
    * `MicrosoftDefaultScopeConfig-Updates`

    Seçili çalışma alanı zaten çözüme sahipse, çözüm yeniden dağıtılanmaz ve kapsam yapılandırması eklenmez.

4. Yapılandırmaların herhangi birinde elipsleri (**...**) seçin ve sonra **Edit'i**tıklatın. 
5. Kapsamı **Düzenleme yapı** bölmesi'nde **Bilgisayar Gruplarını Seçin'i**seçin. **Bilgisayar Grupları** bölmesi, kapsam yapılandırmasını oluşturmak için kullanılan kaydedilen aramaları gösterir.

## <a name="saved-searches"></a>Kaydedilen aramalar

Bir bilgisayar Güncelleştirme Yönetimi, İzlemeyi Değiştir veya Envanter çözümüne eklendiğinde, bilgisayar çalışma alanınızdaki kayıtlı iki aramadan birine eklenir. Kaydedilen aramalar, bu çözümleri hedefleyen bilgisayarları içeren sorgulardır.

Çalışma alanınıza gidin. **Genel**altında, **Kaydedilmiş aramaları**seçin. Bu çözümler tarafından kullanılan kaydedilmiş iki arama aşağıdaki tabloda gösterilmiştir:

|Adı     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik İzleme       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grubu doldurmak için kullanılan sorguyu görüntülemek için kaydedilen aramalardan birini seçin. Aşağıdaki resim sorguyu ve sonuçlarını gösterir:

![Kaydedilen aramalar](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını kaldırma

Aşağıdaki çözümler Bir Log Analytics çalışma alanına bağlıdır:

* [Güncelleştirme Yönetimi](automation-update-management.md)
* [Değişiklik İzleme](automation-change-tracking.md)
* [Mesai saatleri dışında VM'leri Başlatma/Durdurma](automation-solution-vm-management.md)

Otomasyon hesabınızı artık bir Log Analytics çalışma alanıyla tümleştirmek istemediğinize karar verirseniz, hesabınızın bağlantısını doğrudan Azure portalından çıkarabilirsiniz.  Devam etmeden önce, önce daha önce bahsedilen çözümleri kaldırmanız gerekir, aksi takdirde bu işlemin devam etmesi engellenir. Kaldırmak için gereken adımları anlamak için içe aktardığınız belirli çözüm için makaleyi gözden geçirin.

Bu çözümleri kaldırdıktan sonra, Otomasyon hesabınızın bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri de dahil olmak üzere bazı çözümler otomasyon varlıkları oluşturmuş olabilir ve çalışma alanını bağlamadan önce kaldırılması gerekebilir.

1. Azure portalından Otomasyon hesabınızı açın ve soldaki **İlgili Kaynaklar** bölümünün altında Bağlantılı **çalışma alanını** seçin.

2. Bağlantıyı Bırakma çalışma alanı sayfasında, **Bağlantıyı Çöz çalışma alanını**tıklatın.

   ![Bağlantıyı açın çalışma alanı sayfası](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

3. Azure Otomasyonu, Log Analytics çalışma alanınızın hesabının bağlantısını açmaya çalışırken, **Bildirimler** altındaki ilerlemeyi menüden izleyebilirsiniz.

Güncelleştirme Yönetimi çözümlerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Zamanlamaları güncelleştir - Her birinde oluşturduğunuz güncelleştirme dağıtımlarıyla eşleşen adlar olacaktır.

* Çözüm için oluşturulan hibrid işçi grupları - Her benzer machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8) adlandırılacaktır.

Mesai saatleri dışında başlat/durdur vm'lerini kullandıysanız, isteğe bağlı olarak çözümü kaldırdıktan sonra artık gerekmeyen aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* VM çalışma kitabı zamanlamalarını başlatın ve durdurun
* VM runbook'larını başlatVe durdur
* Değişkenler

Alternatif olarak, Çalışma alanınızı Otomasyon Hesabınızdan Günlük Analizi çalışma alanınızdan da çıkarabilirsiniz. Çalışma alanınızda, **İlgili Kaynaklar**altında **Otomasyon Hesabı'nı** seçin. Otomasyon Hesabı sayfasında **Bağlantıyı Aç hesabını**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güncelleştirme Yönetimi'nden bir VM kaldırmak için:

* Günlük Analizi çalışma alanınızda, Kapsam Yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan VM'yi kaldırın. Kaydedilen aramalar çalışma alanınızda **Genel** altında bulunabilir.
* Windows [için Log Analytics aracısını](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) veya [Linux için Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanılacağını öğrenmek için çözümler için öğreticilere devam edin:

* [Öğretici - VM'niz için güncellemeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici - VM'deki yazılımı tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - VM'deki sorun giderme değişiklikleri](automation-tutorial-troubleshoot-changes.md)
