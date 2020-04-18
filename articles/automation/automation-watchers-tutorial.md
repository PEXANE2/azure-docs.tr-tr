---
title: Azure Otomasyonu hesabında bir izleyici görevi oluşturma
description: Bir klasörde oluşturulan yeni dosyaları izlemek için Azure Otomasyonu hesabında nasıl bir izleyici görevi oluşturabilirsiniz öğrenin.
services: automation
ms.subservice: process-automation
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 1175350e7f9f4db92d7d59eba0cc66ac4bb49f5f
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617352"
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Yerel bir makinede dosya değişikliklerini izlemek için Azure Otomasyon izleyicisi görevleri oluşturma

Azure Otomasyonu, PowerShell runbook'ları ile olayları aramak ve eylemleri tetiklemek için bir izleyici görevi kullanır. İzleyici görevi iki bölümden oluşur: gözleyici ve eylem. Bir izleyici runbook izleyici görevinde tanımlanan bir aralıkta çalışır ve verileri bir eylem runbook'a aktarıyor. 

Bu öğretici, bir dizine yeni bir dosya eklendiğinde izlemek için bir izleyici görevi oluşturmada size yol açar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İzleyici runbook alma
> * Otomasyon değişkeni oluşturma
> * Eylem runbook oluşturma
> * İzleyici görevi oluşturma
> * Bir izleyiciyi tetikle
> * Çıktıyı inceleyin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* Watcher ve action runbooks ve Watcher Task tutmak için [otomasyon hesabı.](automation-offering-get-started.md)
* İzleyici görevinin çalıştığı [karma çalışma kitabı çalışanı.](automation-hybrid-runbook-worker.md)
* PowerShell runbooks. PowerShell İş Akışı runbook'ları izleyici görevleri tarafından desteklenmez.

> [!NOTE]
> Gözetmen görevleri Azure China'da desteklenmez.

## <a name="import-a-watcher-runbook"></a>İzleyici runbook alma

Bu öğretici, bir dizinde yeni dosyaları aramak için **Watch-NewFile** adlı bir izleyici runbook kullanır. İzleyici runbook, bir klasördeki dosyalara bilinen son yazma süresini alır ve filigrandan daha yeni dosyalara bakar.

Bu alma işlemi [PowerShell Galerisi](https://www.powershellgallery.com)üzerinden yapılabilir.

1. [Watch-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-runbook-that-looks-36fc82cd)için galeri sayfasına gidin.
2. Azure **Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

Ayrıca, aşağıdaki adımları kullanarak bu runbook'u portaldan otomasyon hesabınıza aktarabilirsiniz.

1. Otomasyon hesabınızı açın ve RunBook'lar sayfasına tıklayın.
2. **Galeriye Gözat'ı**tıklatın.
3. **İzleyici runbook'u**arayın, **dizinde yeni dosyalar arayan İzleyici runbook'u**seçin ve **İçe Aktar'ı**tıklatın.
  ![UI'den alma otomasyonu runbook](media/automation-watchers-tutorial/importsourcewatcher.png)
4. Runbook'a bir ad ve açıklama verin ve runbook'u Otomasyon hesabınıza almak için **Tamam'ı** tıklatın.
5. **Edit'i** seçin ve ardından **Yayımla'yı**tıklatın. İstendiğinde, runbook'u yayımlamak için **Evet'i** tıklatın.

## <a name="create-an-automation-variable"></a>Otomasyon değişkeni oluşturma

Bir [otomasyon değişkeni,](automation-variables.md) önceki runbook'un okuduğu ve her dosyadan depolanan zaman damgalarını depolamak için kullanılır.

1. **Paylaşılan Kaynaklar** altında **Değişkenler'i** seçin ve **+ Değişken ekle'yi**tıklatın.
1. Ad için Watch-NewFileTimestamp'ı girin.
1. Türü için DateTime'ı seçin.
1. Otomasyon değişkenini oluşturmak için **Oluştur'u** tıklatın.

## <a name="create-an-action-runbook"></a>Eylem runbook oluşturma

Eylem runbook bir izleyici runbook ona aktarılan verileri hareket etmek için bir izleyici görevinde kullanılır. [PowerShell Galerisi'nden](https://www.powershellgallery.com) **Process-NewFile** adlı önceden tanımlanmış bir eylem runbook'u içe aktarmanız gerekir. 

Eylem runbook oluşturmak için:

1. [İşlem-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-action-that-b4ff7cdf)için galeri sayfasına gidin.
2. Azure **Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

Bu runbook'u Azure portalından Otomasyon hesabınıza da aktarabilirsiniz:

1. Otomasyon hesabınıza gidin ve **Proses Otomasyonu**altında **Runbook'ları** seçin.
1. **Galeriye Gözat'ı**tıklatın.
1. **İzleyici eylemini**ara, **bir izleyici runbook tarafından tetiklenen olayları işleyen İzleyici eylemini**seçin ve **İçe Aktar'ı**tıklatın.
  ![UI'den alma eylem runbook](media/automation-watchers-tutorial/importsourceaction.png)
1. Runbook'a bir ad ve açıklama verin ve runbook'u Otomasyon hesabınıza almak için **Tamam'ı** tıklatın.
1. **Edit'i** seçin ve ardından **Yayımla'yı**tıklatın. İstendiğinde, runbook'u yayımlamak için **Evet'i** tıklatın.

## <a name="create-a-watcher-task"></a>İzleyici görevi oluşturma

Bu adımda, önceki bölümlerde tanımlanan izleyici ve eylem runbook'lara atıfta bulunan izleyici görevini yapılandırırsınız.

1. Otomasyon hesabınıza gidin ve Süreç **Otomasyonu**altında **Watcher görevlerini** seçin.
1. İzleyici görevleri sayfasını seçin ve **+ İzleyici görevi ekle'yi**tıklatın.
1. Ad olarak **WatchMyFolder'ı** girin.

1. **İzleyiciyi Yapılandır'ı** seçin ve **Watch-NewFile** runbook'u seçin.

1. Parametreler için aşağıdaki değerleri girin:

   * **FOLDERPATH** - Karma Runbook Worker'da yeni dosyaların oluşturulduğu klasör, **örneğin, d:\examplefiles**.
   * **EXTENSION** - Yapılandırma için uzantı. Tüm dosya uzantılarını işlemek için boş bırakın.
   * **RECURSE** - Özyinelemeli işlem. Bu değeri varsayılan olarak bırakın.
   * **ÇALIŞMA AYARLARI** - Runbook'u çalıştırmak için ayar. Melez işçiyi seç.

1. **Tamam'ı**tıklatın ve ardından İzleyici sayfasına dönmek için **seçin.**
1. **Eylemi yapılandır'ı** seçin ve **İşlem-NewFile** runbook'u seçin.
1. Parametreler için aşağıdaki değerleri girin:

   * **EVENTDATA** - Olay verileri. Boş bırakın. Veriler izleyici runbook geçirilir.
   * **Çalışma Ayarları** - Runbook çalıştırmak için ayarlama. Bu runbook Azure Otomasyonu'nda çalıştığından Azure olarak bırakın.

1. **Tamam'ı**tıklatın ve ardından İzleyici sayfasına dönmek için **seçin.**
1. İzleyici görevi oluşturmak için **Tamam'ı** tıklatın.

![UI'den izleyici eylemini yapılandırma](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Bir izleyiciyi tetikle

İzleyici görevinin beklendiği gibi çalıştığından emin olmak için aşağıda açıklandığı gibi bir test çalıştırmanız gerekir. 

1. Hibrit Runbook Worker'a uzak. 
2. **PowerShell'i** açın ve klasörde bir test dosyası oluşturun.

```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

Aşağıdaki örnek, beklenen çıktıyı gösterir.

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Çıktıyı inceleyin

1. Otomasyon hesabınıza gidin ve Süreç **Otomasyonu**altında **Watcher görevlerini** seçin.
1. **WatchMyFolder watcher**görev seçin.
1. İzleyicinin yeni dosyayı bulduğunu ve eylem runbook'u başlattığını görmek için **Akışların** altındaki **izleyici akışlarını** görüntüle'yi tıklatın.
1. Eylem runbook işlerini görmek için **izleyici eylem işlerini görüntüle'yi**tıklatın. Her iş, işin ayrıntılarını görüntülemek için seçilebilir.

   ![UI'den Watcher aksiyon işleri](media/automation-watchers-tutorial/WatcherActionJobs.png)

Yeni dosya bulunduğunda beklenen çıktı aşağıdaki örnekte görülebilir:

```output
Message is Process new file...

Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İzleyici runbook alma
> * Otomasyon değişkeni oluşturma
> * Eylem runbook oluşturma
> * İzleyici görevi oluşturma
> * Bir izleyiciyi tetikle
> * Çıktıyı inceleyin

Kendi runbook yazma hakkında daha fazla bilgi edinmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Benim ilk PowerShell runbook](automation-first-runbook-textual-powershell.md).

