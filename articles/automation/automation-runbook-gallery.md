---
title: PowerShell Galerisi 'de Azure Otomasyonu runbook 'larını ve modüllerini kullanma
description: Bu makalede, Microsoft 'tan ve topluluk PowerShell Galerisi içindeki runbook 'ları ve modülleri nasıl kullanacağınız açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 03/04/2021
ms.topic: conceptual
ms.openlocfilehash: afa782df8666413356fa334bf4e9dcb989b87c2f
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102441478"
---
# <a name="use-runbooks-and-modules-in-powershell-gallery"></a>PowerShell Galerisi’nde runbook’ları ve modülleri kullanma

Azure Otomasyonu 'nda kendi runbook 'larınızı ve modüllerinizi oluşturmak yerine, Microsoft ve Community tarafından önceden oluşturulmuş senaryolara erişebilirsiniz. Azure Otomasyonu GitHub kuruluşundaki PowerShell Galerisi ve [Python runbook](#use-python-runbooks) 'lardan PowerShell runbook 'ları ve [modülleri](#modules-in-powershell-gallery) alabilirsiniz. Ayrıca, [geliştirdiğiniz senaryoları](#add-a-powershell-runbook-to-the-gallery)paylaşarak topluluğa katkıda bulunabilirsiniz.

> [!NOTE]
> TechNet Betik Merkezi devre dışı bırakılıyor. Runbook galerisindeki betik merkezi 'ndeki tüm runbook 'lar, daha fazla [bilgi için bkz](https://github.com/azureautomation) . [buraya](https://techcommunity.microsoft.com/t5/azure-governance-and-management/azure-automation-runbooks-moving-to-github/ba-p/2039337)bakın.

## <a name="runbooks-in-powershell-gallery"></a>PowerShell Galerisi runbook 'lar

[PowerShell Galerisi](https://www.powershellgallery.com/packages) , Microsoft 'un yanı sıra Azure Otomasyonu 'na aktarabileceğiniz topluluk için çeşitli runbook 'lar sağlar. Bir tane kullanmak için, Galeriden bir runbook indirin veya Runbook 'tan veya Azure portal Otomasyon hesabınızdan doğrudan runbook 'ları içeri aktarabilirsiniz.

> [!NOTE]
> Grafik runbook 'ları PowerShell Galerisi desteklenmez.

Azure portal kullanarak yalnızca PowerShell Galerisi doğrudan içeri aktarabilirsiniz. PowerShell kullanarak bu işlevi gerçekleştiremezsiniz.

> [!NOTE]
> PowerShell Galerisi aldığınız tüm runbook 'ların içeriğini doğrulamanız ve bunları bir üretim ortamına yüklemek ve çalıştırmak için çok dikkatli olmanız gerekir.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi modüller

PowerShell modülleri, runbook 'larınızda kullanabileceğiniz cmdlet 'leri içerir. Azure Otomasyonu 'nda yükleyebileceğiniz mevcut modüller [PowerShell Galerisi](https://www.powershellgallery.com)kullanılabilir. Bu galeriyi Azure portal başlatabilir ve modülleri doğrudan Azure Otomasyonu 'na yükleyebilir veya el ile indirip yükleyebilirsiniz.

## <a name="common-scenarios-available-in-powershell-gallery"></a>PowerShell Galerisi 'de bulunan yaygın senaryolar

Aşağıdaki liste, yaygın senaryoları destekleyen birkaç runbook içerir. Azure Otomasyonu ekibi tarafından oluşturulan runbook 'ların tam listesi için bkz. [Azureautomationteam profile](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

   * [Update-ModulesInAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) -PowerShell Galerisi bir Otomasyon hesabındaki tüm modüllerin en son sürümünü içeri aktarır.
   * [Enable-AzureDiagnostics](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) -iş durumu ve iş akışları Içeren Azure Otomasyonu günlüklerini almak için Azure Tanılama ve Log Analytics yapılandırır.
   * [Copy-ıtemfrolaurevm](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) -bir Windows Azure sanal makinesinden uzak bir dosyayı kopyalar.
   * [Copy-ItemToAzureVM](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) -bir Azure sanal makinesine yerel bir dosya kopyalar.

## <a name="import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portal ile runbook galerisinden bir PowerShell runbook 'unu içeri aktarın

1. Azure portalında, Otomasyon hesabınızı açın.
1. **Işlem Otomasyonu** altında **runbook 'lar Galerisi** ' ni seçin.
1. **Kaynak seçin: PowerShell Galerisi**. Bu, gözatabilmeniz için kullanılabilir runbook 'ların bir listesini gösterir.
1. Listeyi daraltmak için listenin üzerindeki arama kutusunu kullanabilir veya daha sonra görüntüyü Yayımcı, yazma ve sıralama ile daraltmak için filtreleri kullanabilirsiniz. İstediğiniz Galeri öğesini bulun ve ayrıntılarını görüntülemek için seçin.

   :::image type="content" source="media/automation-runbook-gallery/browse-gallery-sm.png" alt-text="Runbook galerisine göz atma" lightbox="media/automation-runbook-gallery/browse-gallery-lg.png":::

1. Bir öğeyi içeri aktarmak için Ayrıntılar dikey penceresinde **Içeri aktar** ' a tıklayın.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-detail-sm.png" alt-text="Bir Runbook Galerisi öğe ayrıntısı göster" lightbox="media/automation-runbook-gallery/gallery-item-detail-lg.png":::

1. İsteğe bağlı olarak, runbook 'un adını değiştirin ve ardından runbook 'u içeri aktarmak için **Tamam** ' ı tıklatın.
1. Runbook, Otomasyon hesabı için **runbook 'lar** sekmesinde görünür.

## <a name="import-a--powershell-runbook-from-github-with-the-azure-portal"></a>Azure portal ile bir PowerShell runbook 'unu GitHub 'dan içeri aktarın

1. Azure portalında, Otomasyon hesabınızı açın.
1. **Işlem Otomasyonu** altında **runbook 'lar Galerisi** ' ni seçin.
1. **Kaynak: GitHub**' ı seçin.
1. Görünümü yayımcıya, türüne ve sıralamaya göre daraltmak için listenin üzerindeki filtreleri kullanabilirsiniz. İstediğiniz Galeri öğesini bulun ve ayrıntılarını görüntülemek için seçin.

   :::image type="content" source="media/automation-runbook-gallery/browse-gallery-github-sm.png" alt-text="GitHub galerisine göz atma" lightbox="media/automation-runbook-gallery/browse-gallery-github-lg.png":::

1. Bir öğeyi içeri aktarmak için Ayrıntılar dikey penceresinde **Içeri aktar** ' a tıklayın.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-details-blade-github-sm.png" alt-text="GitHub galerisinden bir runbook 'un ayrıntılı görünümü" lightbox="media/automation-runbook-gallery/gallery-item-details-blade-github-lg.png":::

1. İsteğe bağlı olarak, runbook 'un adını değiştirin ve ardından runbook 'u içeri aktarmak için **Tamam** ' ı tıklatın.
1. Runbook, Otomasyon hesabı için **runbook 'lar** sekmesinde görünür.

## <a name="add-a-powershell-runbook-to-the-gallery"></a>Galeriye PowerShell runbook 'u ekleme

Microsoft, diğer müşteriler için yararlı olacağını düşündüğünüz PowerShell Galerisi runbook eklemenizi önerir. PowerShell Galerisi PowerShell modüllerini ve PowerShell betiklerini kabul eder. Bir runbook 'u [PowerShell Galerisi yükleyerek](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package)ekleyebilirsiniz.

## <a name="import-a-module-from-the-module-gallery-with-the-azure-portal"></a>Modül galerisinden bir modülü Azure portal içeri aktarma

1. Azure portalında, Otomasyon hesabınızı açın.
1. Modüllerin listesini açmak için **paylaşılan kaynaklar** altındaki **modüller** ' i seçin.
1. Sayfanın üst kısmından **tarayıcı Galerisi** ' ne tıklayın.

      :::image type="content" source="media/automation-runbook-gallery/modules-blade-sm.png" alt-text="Modül galerisinin görünümü" lightbox="media/automation-runbook-gallery/modules-blade-lg.png":::

1. Galeriye gözatamıyorum sayfasında, aşağıdaki alanlardan herhangi birinde eşleşmeleri bulmak için arama kutusunu kullanabilirsiniz:

   * Modül Adı
   * Etiketler
   * Yazma
   * Cmdlet/DSC kaynak adı

1. İlgilendiğiniz bir modül bulun ve ayrıntılarını görüntülemek için seçin.

   Belirli bir modülün detayına göre daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler PowerShell Galerisi, tüm gerekli bağımlılıkları ve modülün içerdiği tüm cmdlet 'leri ya da DSC kaynaklarını geri bir bağlantı içerir.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-details-blade-sm.png" alt-text="Galeriden bir modülün ayrıntılı görünümü" lightbox="media/automation-runbook-gallery/gallery-item-details-blade-lg.png":::

1. Modülü doğrudan Azure Otomasyonu 'na yüklemek için **Içeri aktar**' a tıklayın.
1. Içeri aktarma bölmesinde, içeri aktarılacak modülün adını görebilirsiniz. Tüm bağımlılıklar yüklüyse, **Tamam** düğmesi etkinleştirilir. Bağımlılıklarınız eksikse, bu modülü içeri aktarmadan önce bu bağımlılıkları içeri aktarmanız gerekir.
1. Içeri aktarma bölmesinde, modülü içeri aktarmak için **Tamam** ' ı tıklatın. Azure Otomasyonu hesabınıza bir modül içeri aktarırken, modül ve cmdlet 'ler hakkında meta verileri ayıklar. Bu eylem, her etkinliğin ayıklanması gerektiğinden birkaç dakika sürebilir.
1. Modülün dağıtılmakta olduğu bir başlangıç bildirimi ve tamamlandığında başka bir bildirim alırsınız.
1. Modül içeri aktarıldıktan sonra, kullanılabilir etkinlikleri görebilirsiniz. Runbook 'larınızda ve DSC kaynaklarınızın modül kaynaklarını kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell Core 'u destekleyen modüller Azure Otomasyonu 'nda desteklenmez ve Azure portal içeri aktarılamaz ya da doğrudan PowerShell Galerisi dağıtılır.

## <a name="use-python-runbooks"></a>Python runbook 'larını kullanma

Python runbook 'Ları, [Azure Otomasyonu GitHub kuruluşunda](https://github.com/azureautomation)bulunabilir. GitHub deponuzda katkıda bulundığınızda, katkılarınızı karşıya yüklerken Tag **(GitHub konu): Python3** ekleyin.

## <a name="request-a-runbook-or-module"></a>Runbook veya modül isteme

İstekleri [Kullanıcı seslerine](https://feedback.azure.com/forums/246290-azure-automation/)gönderebilirsiniz.  Bir Runbook yazma veya PowerShell hakkında sorularınız ile ilgili yardıma ihtiyacınız varsa [Microsoft Q&soru sayfasına](/answers/topics/azure-automation.html)bir soru gönderin.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell runbook 'unu kullanmaya başlamak için bkz. [öğretici: PowerShell runbook 'U oluşturma](learn/automation-tutorial-runbook-textual-powershell.md).
* Runbook 'larla çalışmak için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).
* PowerShell ayrıntıları için bkz. [PowerShell belgeleri](/powershell/scripting/overview).
* PowerShell cmdlet başvurusu için bkz. [az. Automation](/powershell/module/az.automation).
