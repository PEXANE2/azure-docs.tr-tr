---
title: Azure Otomasyonu için runbook ve modül galerileri
description: Microsoft ve topluluktan runbook'lar ve modüller Azure Otomasyon ortamınızda yüklemeniz ve kullanmanız için kullanılabilir.  Bu makalede, bu kaynaklara nasıl erişebileceğiniz ve runbook'larınızı galeriye nasıl katkıda bulunabileceğiniz açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 03/20/2019
ms.topic: conceptual
ms.openlocfilehash: 52a0ab0a31600c1548283c7d899b17e497811b5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75421477"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Otomasyonu için runbook ve modül galerileri

Azure Otomasyonu'nda kendi runbook'larınızı ve modüllerinizi oluşturmak yerine, Microsoft ve topluluk tarafından oluşturulmuş senaryolara erişebilirsiniz.

PowerShell runbook'larını ve modüllerini PowerShell Gallery'den ve [Python runbook'larından](#python-runbooks) Script Center Gallery'den alabilirsiniz. [modules](#modules-in-powershell-gallery) Ayrıca, geliştirdiğiniz senaryoları paylaşarak topluluğa katkıda bulunabilirsiniz, bkz.

## <a name="runbooks-in-powershell-gallery"></a>PowerShell Galerisi'ndeki Runbook'lar

[PowerShell Galerisi,](https://www.powershellgallery.com/packages) Microsoft'tan ve Azure Otomasyonuna aktarabileceğiniz topluluktan çeşitli runbook'lar sağlar. Birini kullanmak için galeriden bir runbook indirin veya runbook'ları doğrudan galeriden veya Azure portalındaki Otomasyon Hesabınızdan içe aktarabilirsiniz.

Azure portalını kullanarak doğrudan PowerShell Galerisi'nden içe aktarabilirsiniz. PowerShell kullanarak bu işlevi gerçekleştiremezsiniz.

> [!NOTE]
> PowerShell Galerisi'nden aldığınız runbook'ların içeriğini doğrulamalı ve bunları bir üretim ortamında yükler ve çalıştırabiliyorsanız çok dikkatli olmalısınız.

### <a name="to-import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portalı ile Runbook Galerisi'nden bir PowerShell runbook almak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. **Proses Otomasyonu**altında **Runbooks galerisine** tıklayın
3. Kaynak **Seçiniz: PowerShell Galerisi.**
4. İstediğiniz galeri öğesini bulun ve ayrıntılarını görüntülemek için seçin. Solda, yayımcı ve türü için ek arama parametreleri girebilirsiniz.

   ![Galeriye göz atma](media/automation-runbook-gallery/browse-gallery.png)

5. Öğeyi [TechNet Script Center'da](https://gallery.technet.microsoft.com/)görüntülemek için **kaynak projeyi** görüntüle'yi tıklatın.
6. Bir öğeyi almak için ayrıntılarını görüntülemek için öğeyi tıklatın ve sonra **İçe Aktar** düğmesini tıklatın.

   ![İçe aktarma düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)

7. İsteğe bağlı olarak, runbook adını değiştirin ve sonra runbook almak için **Tamam'ı** tıklatın.
8. Runbook, Otomasyon Hesabı'nın **Runbook** sekmesinde görünür.

### <a name="adding-a-powershell-runbook-to-the-gallery"></a>Galeriye PowerShell runbook ekleme

Microsoft, PowerShell Galerisi'ne diğer müşteriler için yararlı olacağını düşündüğünüz runbook'lar eklemenizi öneririz. PowerShell Gallery, PowerShell modüllerini ve PowerShell komut dosyalarını kabul eder. PowerShell [Galerisi'ne yükleyerek](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package)bir runbook ekleyebilirsiniz.

> [!NOTE]
> PowerShell Gallery'de grafik çalışma kitapları desteklenmez.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi'ndeki Modüller

PowerShell modülleri runbook'larınızda kullanabileceğiniz cmdletler içerir ve Azure Otomasyonu'nda yükleyebileceğiniz varolan modüller [PowerShell Galerisi'nde](https://www.powershellgallery.com)mevcuttur. Bu galeriyi Azure portalından başlatabilir ve doğrudan Azure Otomasyonu'na yükleyebilirsiniz. Ayrıca bunları indirebilir ve el ile yükleyebilirsiniz.

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Azure portalı ile Otomasyon Modülü Galerisi'nden bir modül almak için

1. Azure portalında, Otomasyon hesabınızı açın.
2. Modül listesini açmak için **Paylaşılan Kaynaklar** altında **Modülleri** seçin.
3. Sayfanın üst kısmından **Galeriye Gözat'ı** tıklatın.

   ![Modül galerisi](media/automation-runbook-gallery/modules-blade.png)

4. **Galeriye Gözat** sayfasında aşağıdaki alanlara göre arama yapabilirsiniz:

   * Modül Adı
   * Etiketler
   * Yazma
   * Cmdlet/DSC kaynak adı

5. İlgilendiğiniz bir modülü bulun ve ayrıntılarını görüntülemek için modülü seçin.

   Belirli bir modülü deldiğinizde, daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler PowerShell Galerisi'ne, gerekli bağımlılıklara ve modülün içerdiği tüm cmdlets veya DSC kaynaklarına bir bağlantı içerir.

   ![PowerShell modül detayları](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Modülü doğrudan Azure Otomasyonu'na yüklemek için **İçe Aktar** düğmesini tıklatın.
7. **İçe Aktar** düğmesini tıklattığınızda, Alma bölmesinde, almak üzere olduğunuz modül adını görürsünüz. Tüm bağımlılıklar yüklenirse, **Tamam** düğmesi etkinleştirilir. Bağımlılıkları kaçırıyorsanız, bu modülü içe aktarabilmeniz için önce bu bağımlılıkları almanız gerekir.
8. **İçe Aktarma** sayfasında, modülü almak için **Tamam'ı** tıklatın. Azure Otomasyon hesabınıza bir modül aktarırken, modül ve cmdlets hakkında meta veriler ayıklar. Bu eylem, her etkinliğin ayıklanması gerektiğinden birkaç dakika sürebilir.
9. Modülün dağıtıldığına dair ilk bildirim ve tamamlandığında başka bir bildirim alırsınız.
10. Modül alındıktan sonra, kullanılabilir etkinlikleri görebilirsiniz. Kaynaklarını runbook'larınızda ve İstenilen Durum Yapılandırmasında kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell çekirdeğini destekleyen modüller Azure Otomasyonu'nda desteklenmez ve Azure portalında içe aktarılamaz veya doğrudan PowerShell Galerisi'nden dağıtılamaz.

## <a name="python-runbooks"></a>Python Runbooks

Python Runbook'lar [Script Center galerisinde](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=)mevcuttur. Python runbook'larını, **bir katkı**yükle'yi tıklatarak Script Center galerisine katkıda bulunabilirsiniz. Bunu yaptığınızda, katkınızı yüklerken **Python** etiketini eklediğinizden emin olun.

> [!NOTE]
> [İçerikleri Script Center'a](https://gallery.technet.microsoft.com/scriptcenter) yüklemek için en az 100 puan gereklidir.

## <a name="requesting-a-runbook-or-module"></a>Runbook veya modül isteme

[Kullanıcı Sesi'ne](https://feedback.azure.com/forums/246290-azure-automation/)istek gönderebilirsiniz.  Eğer bir runbook yazma konusunda yardıma ihtiyacınız varsa veya PowerShell hakkında bir sorunuz varsa, [bizim foruma](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)bir soru gönderin.

## <a name="common-solutions-available-in-the-runbook-gallery"></a>Runbook galerisinde bulunan ortak çözümler

Aşağıdaki liste, sık karşılaşılan senaryolara çözümler sağlayan birkaç runbook içerir. Azure Otomasyon ekibi tarafından oluşturulan runbook'ların tam listesi için [AzureAutomationTeam profiline](https://www.powershellgallery.com/profiles/AzureAutomationTeam)bakın.

* [Update-ModulesInAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) - Bir Otomasyon hesabındaki tüm modüllerin PowerShell Galerisi'ndeki en son sürümünü alır.
* [Azure TanılamaYı Etkinleştir](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) - Bu komut dosyası, iş durumu ve iş akışları içeren Azure Otomasyon günlüklerini alacak şekilde Azure Diagnostics ve Log Analytics'i yapılandırır.
* [Copy-ItemFromAzureVM](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) - Bu runbook, Windows Azure sanal makinesinden uzak bir dosyayı kopyalar.
* [Copy-ItemFromAzureVM](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) - Bu runbook, yerel bir dosyayı Azure sanal makinesine kopyalar.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'lara başlamak için Azure [Otomasyonu'nda runbook'u Yönet'e](manage-runbooks.md) bakın
* PowerShell ve PowerShell İş Akışı arasındaki farkları runbook'larla anlamak için [Bkz.](automation-powershell-workflow.md)
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
