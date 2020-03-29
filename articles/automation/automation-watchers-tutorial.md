---
title: Azure Otomasyonu hesabında bir izleyici görevi oluşturma
description: Bir klasörde oluşturulan yeni dosyaları izlemek için Azure Otomasyonu hesabında nasıl bir izleyici görevi oluşturabilirsiniz öğrenin.
services: automation
ms.subservice: process-automation
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 5dc6145940883ff6f4446ad67c399cdf4931d38e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75419751"
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Yerel bir makinede dosya değişikliklerini izlemek için Azure Otomasyon izleyicisi görevleri oluşturma

Azure Otomasyonu, PowerShell runbook'ları ile olayları izlemek ve eylemleri tetiklemek için izleyici görevlerini kullanır. Bu öğretici, bir dizine yeni bir dosya eklendiğinde izlemek için bir izleyici görevi oluşturmada size yol açar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

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

> [!NOTE]
> Gözetmen görevleri Azure China'da desteklenmez.

## <a name="import-a-watcher-runbook"></a>İzleyici runbook alma

Bu öğretici, bir dizinde yeni dosyaları aramak için **Watch-NewFile** adlı bir izleyici runbook kullanır. İzleyici runbook, bir klasördeki dosyalara bilinen son yazma süresini alır ve filigrandan daha yeni dosyalara bakar.

Bu alma işlemi [PowerShell Galerisi](https://www.powershellgallery.com)üzerinden yapılabilir.

1. [Watch-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-runbook-that-looks-36fc82cd)için galeri sayfasına gidin.
2. Azure **Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

Ayrıca, aşağıdaki adımları kullanarak bu runbook'u portaldan otomasyon hesabınıza aktarabilirsiniz.

1. Otomasyon hesabınızı açın ve **RunBook'lar** sayfasına tıklayın.
2. **Galeriye Gözat** düğmesine tıklayın.
3. "İzleyici runbook"u arayın, **dizinde yeni dosyalar arayan İzleyici runbook'u** seçin ve **Içe Aktar'ı**seçin.
  ![UI'den alma otomasyonu runbook](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Runbook'a bir ad ve açıklama verin ve runbook'u Otomasyon hesabınıza almak için **Tamam'ı** seçin.
1. **Edit'i** seçin ve ardından **Yayımla'yı**tıklatın. Runbook'u yayımlamak için **Evet'i** seçin.

## <a name="create-an-automation-variable"></a>Otomasyon değişkeni oluşturma

Bir [otomasyon değişkeni,](automation-variables.md) önceki runbook'un okuduğu ve her dosyadan depolanan zaman damgalarını depolamak için kullanılır.

1. **PAYLAŞıLAN KAYNAKLAR** altında **Değişkenler'i** seçin ve **+ Değişken ekle'yi**seçin.
1. Ad için "Watch-NewFileTimestamp" girin
1. Tür için DateTime'ı seçin.
1. **Oluştur** düğmesine tıklayın. Bu otomasyon değişkeni oluşturur.

## <a name="create-an-action-runbook"></a>Eylem runbook oluşturma

Eylem runbook bir izleyici runbook ona aktarılan verileri hareket etmek için bir izleyici görevinde kullanılır. PowerShell İş Akışı runbook'ları izleyici görevleri tarafından desteklenmez, PowerShell runbook'larını kullanmanız gerekir. **İşlem-NewFile**adlı önceden tanımlanmış bir eylem runbook almanız gerekir.

Bu alma işlemi [PowerShell Galerisi](https://www.powershellgallery.com)üzerinden yapılabilir.

1. [İşlem-NewFile.ps1](https://gallery.technet.microsoft.com/scriptcenter/Watcher-action-that-b4ff7cdf)için galeri sayfasına gidin.
2. Azure **Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

Ayrıca, aşağıdaki adımları kullanarak bu runbook'u portaldan otomasyon hesabınıza aktarabilirsiniz.

1. Otomasyon hesabınıza gidin ve PROCESS **AUTOMATION** kategorisi altındaki **Runbook'ları** seçin.
1. **Galeriye Gözat** düğmesine tıklayın.
1. "İzleyici eylemi" arama ve **bir izleyici runbook tarafından tetiklenen olayları işleyen Watcher eylem** seçin ve **Alma**seçin.
  ![UI'den alma eylem runbook](media/automation-watchers-tutorial/importsourceaction.png)
1. Runbook'a bir ad ve açıklama verin ve runbook'u Otomasyon hesabınıza almak için **Tamam'ı** seçin.
1. **Edit'i** seçin ve ardından **Yayımla'yı**tıklatın. Runbook'u yayımlamak için **Evet'i** seçin.

## <a name="create-a-watcher-task"></a>İzleyici görevi oluşturma

İzleyici görevi iki bölümden oluşur. Gözcü ve aksiyon. İzleyici, izleyici görevinde tanımlanan bir aralıkta çalışır. İzleyici runbook'tan gelen veriler eylem runbook'a aktarılır. Bu adımda, önceki adımlarda tanımlanan izleyici ve eylem runbook'lara atıfta bulunan izleyici görevini yapılandırırsınız.

1. Otomasyon hesabınıza gidin ve **PROCESS AUTOMATION** kategorisi altında **Watcher görevlerini** seçin.
1. İzleyici görevleri sayfasını seçin ve **+ İzleyici görev** düğmesine tıklayın.
1. Ad olarak "WatchMyFolder" girin.

1. **İzleyiciyi Yapılandır'ı** seçin ve **Watch-NewFile** runbook'u seçin.

1. Parametreler için aşağıdaki değerleri girin:

   * **FOLDERPATH** - Karma çalışma cılızında yeni dosyaların oluşturulduğu klasör. d:\örnek dosyalar
   * **EXTENSION** - Tüm dosya uzantılarını işlemek için boş bırakın.
   * **RECURSE** - Bu değeri varsayılan olarak bırakın.
   * **RUN AYARLARI** - Melez işçiyi seçin.

1. Tamam'ı tıklatın ve ardından izleyen sayfaya dönmek için Seçin.
1. **Eylemi Yapılandır'ı** seçin ve "İşlem-Yeni Dosya" runbook'u seçin.
1. Parametreler için aşağıdaki değerleri girin:

   * **EVENTDATA** - Boş bırakın. Veriler izleyici runbook geçirilir.
   * **Çalışma Ayarları** - Bu runbook Otomasyon hizmetinde çalışırken Azure olarak bırakın.

1. **Tamam'ı**tıklatın ve ardından izleyen sayfaya dönmek için seçin.
1. İzleyici görevi oluşturmak için **Tamam'ı** tıklatın.

![UI'den izleyici eylemini yapılandırma](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Bir izleyiciyi tetikle

İzleyicinin beklendiği gibi çalıştığını test etmek için bir test dosyası oluşturmanız gerekir.

Melez işçinin uzaktan kumandası. **PowerShell'i** açın ve klasörde bir test dosyası oluşturun.

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

1. Otomasyon hesabınıza gidin ve **PROCESS AUTOMATION** kategorisi altında **Watcher görevlerini** seçin.
1. "WatchMyFolder" izleyici görevini seçin.
1. İzleyicinin yeni dosyayı bulduğunu ve eylem runbook'u başlattığını görmek için **Akışların** altındaki **izleyici akışlarını** görüntüle'yi tıklatın.
1. Eylem runbook işlerini görmek **için, Görüntüley izleyici eylem işleri'ni**tıklatın. Her iş, işin ayrıntılarını görüntülemek için seçilebilir.

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

