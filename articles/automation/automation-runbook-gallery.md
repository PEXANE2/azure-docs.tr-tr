---
title: Azure Otomasyonu için runbook ve modül galerileri
description: Microsoft ve topluluk runbook 'ları ve modülleri, Azure Otomasyonu ortamınızda yükleyip kullanabilmeniz için kullanılabilir.  Bu makalede, bu kaynaklara nasıl erişebileceğiniz ve Runbook 'larınızın galeriye nasıl katkıda bulunduğu açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 03/20/2019
ms.topic: conceptual
ms.openlocfilehash: 90b475e275598363314c8f131911fe12650cd3df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81535562"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Otomasyonu için runbook ve modül galerileri

Azure Otomasyonu 'nda kendi runbook 'larınızı ve modüllerinizi oluşturmak yerine, Microsoft ve Community tarafından önceden oluşturulmuş senaryolara erişebilirsiniz. Betik Merkezi galerisindeki PowerShell Galerisi ve [Python runbook](#use-python-runbooks) 'lardan PowerShell runbook 'ları ve [modülleri](#modules-in-powershell-gallery) alabilirsiniz. Ayrıca, [geliştirdiğiniz senaryoları](#add-a-powershell-runbook-to-the-gallery)paylaşarak topluluğa katkıda bulunabilirsiniz. 

## <a name="runbooks-in-powershell-gallery"></a>PowerShell Galerisi runbook 'lar

[PowerShell Galerisi](https://www.powershellgallery.com/packages) , Microsoft 'un yanı sıra Azure Otomasyonu 'na aktarabileceğiniz topluluk için çeşitli runbook 'lar sağlar. Bir tane kullanmak için, Galeriden bir runbook indirin veya Runbook 'tan veya Azure portal Otomasyon hesabınızdan doğrudan runbook 'ları içeri aktarabilirsiniz.

> [!NOTE]
> Grafik runbook 'ları PowerShell Galerisi desteklenmez.

Azure portal kullanarak yalnızca PowerShell Galerisi doğrudan içeri aktarabilirsiniz. PowerShell kullanarak bu işlevi gerçekleştiremezsiniz.

> [!NOTE]
> PowerShell Galerisi aldığınız tüm runbook 'ların içeriğini doğrulamanız ve bunları bir üretim ortamına yüklemek ve çalıştırmak için çok dikkatli olmanız gerekir.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi modüller

PowerShell modülleri, runbook 'larınızda kullanabileceğiniz cmdlet 'leri içerir ve Azure Otomasyonu 'nda yükleyebileceğiniz mevcut modüller [PowerShell Galerisi](https://www.powershellgallery.com)kullanılabilir. Bu galeriyi Azure portal başlatabilir ve doğrudan Azure Otomasyonu 'na yükleyebilirsiniz. Ayrıca, bunları indirip el ile yükleyebilirsiniz.

## <a name="common-solutions-available-in-powershell-gallery"></a>PowerShell Galerisi 'de kullanılabilen ortak çözümler

Aşağıdaki liste, yaygın senaryolar için çözümler sağlayan birkaç runbook içerir. Azure Otomasyonu ekibi tarafından oluşturulan runbook 'ların tam listesi için bkz. [Azureautomationteam profile](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

   * [Update-ModulesInAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) -PowerShell Galerisi bir Otomasyon hesabındaki tüm modüllerin en son sürümünü içeri aktarır.
   * [Enable-AzureDiagnostics](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) -iş durumu ve iş akışları Içeren Azure Otomasyonu günlüklerini almak için Azure Tanılama ve Log Analytics yapılandırır.
   * [Copy-ıtemfrolaurevm](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) -bir Windows Azure sanal makinesinden uzak bir dosyayı kopyalar.
   * [Copy-ıtemfrolaurevm](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) -yerel bir dosyayı bir Azure sanal makinesine kopyalar.

## <a name="import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portal ile runbook galerisinden bir PowerShell runbook 'unu içeri aktarın

1. Azure portalında, Otomasyon hesabınızı açın.
2. **Işlem Otomasyonu**altında **runbook 'lar Galerisi** ' ni seçin.
3. **Kaynak seçin: PowerShell Galerisi**.
4. İstediğiniz Galeri öğesini bulun ve ayrıntılarını görüntülemek için seçin. Sol tarafta yayımcı için ek arama parametreleri girebilir ve yazın.

   ![Galeriye göz atma](media/automation-runbook-gallery/browse-gallery.png)

5. Öğeyi [TechNet Betik Merkezi](https://gallery.technet.microsoft.com/)' nde görüntülemek için **kaynak projeyi görüntüle** ' ye tıklayın.
6. Bir öğeyi içeri aktarmak için üzerine tıklayarak ayrıntılarını görüntüleyin ve ardından **Içeri aktar**' a tıklayın.

   ![İçeri Aktar düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)

7. İsteğe bağlı olarak, runbook 'un adını değiştirin ve ardından runbook 'u içeri aktarmak için **Tamam** ' ı tıklatın.
8. Runbook, Otomasyon hesabı için **runbook 'lar** sekmesinde görünür.

## <a name="add-a-powershell-runbook-to-the-gallery"></a>Galeriye PowerShell runbook 'u ekleme

Microsoft, diğer müşteriler için yararlı olacağını düşündüğünüz PowerShell Galerisi runbook eklemenizi önerir. PowerShell Galerisi PowerShell modüllerini ve PowerShell betiklerini kabul eder. Bir runbook 'u [PowerShell Galerisi yükleyerek](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package)ekleyebilirsiniz.

## <a name="import-a-module-from-the-module-gallery-with-the-azure-portal"></a>Modül galerisinden bir modülü Azure portal içeri aktarma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Modüllerin listesini açmak için **paylaşılan kaynaklar** altındaki **modüller** ' i seçin.
3. Sayfanın üst kısmından **tarayıcı Galerisi** ' ne tıklayın.

   ![Modül Galerisi](media/automation-runbook-gallery/modules-blade.png)

4. Galeriye gözatamıyorum sayfasında, aşağıdaki alanlara göre arama yapabilirsiniz:

   * Modül Adı
   * Etiketler
   * Yazma
   * Cmdlet/DSC kaynak adı

5. İlgilendiğiniz bir modül bulun ve ayrıntılarını görüntülemek için seçin.

   Belirli bir modülün detayına göre daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler PowerShell Galerisi, tüm gerekli bağımlılıkları ve modülün içerdiği tüm cmdlet 'leri ya da DSC kaynaklarını geri bir bağlantı içerir.

   ![PowerShell modülü ayrıntıları](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Modülü doğrudan Azure Otomasyonu 'na yüklemek için **Içeri aktar**' a tıklayın.
7. Içeri aktarma bölmesinde, içeri aktarılacak modülün adını görebilirsiniz. Tüm bağımlılıklar yüklüyse, **Tamam** düğmesi etkinleştirilir. Bağımlılıklarınız eksikse, bu modülü içeri aktarmadan önce bu bağımlılıkları içeri aktarmanız gerekir.
8. Içeri aktarma bölmesinde, modülü içeri aktarmak için **Tamam** ' ı tıklatın. Azure Otomasyonu hesabınıza bir modül içeri aktarırken, modül ve cmdlet 'ler hakkında meta verileri ayıklar. Bu eylem, her etkinliğin ayıklanması gerektiğinden birkaç dakika sürebilir.
9. Modülün dağıtılmakta olduğu bir başlangıç bildirimi ve tamamlandığında başka bir bildirim alırsınız.
10. Modül içeri aktarıldıktan sonra, kullanılabilir etkinlikleri görebilirsiniz. Runbook 'larınızda ve DSC kaynaklarınızın modül kaynaklarını kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell Core 'u destekleyen modüller Azure Otomasyonu 'nda desteklenmez ve Azure portal içeri aktarılamaz ya da doğrudan PowerShell Galerisi dağıtılır.

## <a name="use-python-runbooks"></a>Python runbook 'larını kullanma

Python runbook 'Ları, [betik Merkezi galerisinde](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=)bulunur. **Bir katkıyı karşıya yükle**' ye tıklayarak Python runbook 'Larını betik merkezi galerisine katkıda bulabilirsiniz. Bunu yaptığınızda, katkılarınızı karşıya yüklerken etiketi `Python` eklemediğinizden emin olun.

> [!NOTE]
> [Komut dosyası merkezine](https://gallery.technet.microsoft.com/scriptcenter)içerik yüklemek için en az 100 noktaya ihtiyacınız vardır.

## <a name="request-a-runbook-or-module"></a>Runbook veya modül isteme

İstekleri [Kullanıcı seslerine](https://feedback.azure.com/forums/246290-azure-automation/)gönderebilirsiniz.  Bir Runbook yazma veya PowerShell hakkında sorularınız konusunda yardıma ihtiyacınız varsa [forumumuza](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)soru gönderin.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'ları kullanmaya başlamak için bkz. [Azure Otomasyonu 'nda runbook 'U yönetme](manage-runbooks.md).
* Runbook 'larla PowerShell ve PowerShell Iş akışı arasındaki farkları anlamak için bkz. [PowerShell iş akışını öğrenme](automation-powershell-workflow.md).
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
