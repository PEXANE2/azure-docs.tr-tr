---
title: Hata Ayıklama U-SQL işleri - Visual Studio Code için Azure Veri Gölü Araçları
description: U-SQL işlerini yerel olarak çalıştırmak ve hata ayıklamak için Visual Studio Code için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 07/14/2017
ms.openlocfilehash: e51b5640163546c673a1b0f61da47ccd992f27ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72030043"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>Visual Studio Code'da U-SQL'i çalıştırın ve yerel olarak hata ayıklama
Bu makalede, erken kodlama aşamalarını hızlandırmak veya Visual Studio Code'da yerel olarak kod ayıklamak için yerel geliştirme makinesinde U-SQL işlerinin nasıl çalıştırılacak olduğu açıklanmaktadır. Visual Studio Code için Azure Veri Gölü Aracı ile ilgili talimatlar [için](data-lake-analytics-data-lake-tools-for-vscode.md)bkz.

Yalnızca Visual Studio için Azure Veri Gölü Araçları'nın Windows yüklemeleri, U-SQL'i yerel olarak çalıştırmak ve U-SQL'i yerel olarak hata ayıklamak için eylemi destekler. macOS ve Linux tabanlı işletim sistemlerindeki yüklemeler bu özelliği desteklemez.

## <a name="set-up-the-u-sql-local-run-environment"></a>U-SQL yerel çalışma ortamını ayarlama

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin ve ardından **ADL girin:** Paketleri indirmek için Yerel Çalıştır Paketini İndirin.  

   ![ADL LocalRun Bağımlılık paketlerini indirin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Bağımlılık paketlerini **Çıktı** bölmesinde gösterilen yoldan bulun ve buildtools ve Win10SDK 10240'ı yükleyin. Aşağıda bir örnek yol verilmiştir:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Bağımlılık paketlerini bulma](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 **BuildTools'u**yüklemek için LocalRunDependency klasöründe visualcppbuildtools_full.exe'yi tıklatın ve ardından sihirbaz yönergelerini izleyin.   

    ![BuildTools'u Yükleyin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 **Win10SDK 10240**yüklemek için, LocalRunDependency/Win10SDK_10.0.10240_2 klasöründe sdksetup.exe tıklayın, sonra sihirbaz talimatları izleyin.  

    ![Win10SDK 10240 yükleyin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Ortam değişkenini ayarlayın. **SCOPE_CPP_SDK** ortamı değişkenini şu şekilde ayarlayın:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Yerel çalıştırma hizmetini başlatın ve U-SQL işini yerel bir hesaba gönderin 
İlk kez kullanıcı için **ADL:**  [U-SQL yerel çalıştırma ortamı ayarlamadıysanız](#set-up-the-u-sql-local-run-environment)yerel çalıştırma paketlerini indirmek için Yerel Çalıştır Paketini indirin.

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin ve ardından **ADL'yi girin: Yerel Çalıştırma Hizmetini Başlat.**   
2. Microsoft Yazılım Lisans Koşullarını ilk kez kabul etmek için **Kabul Et'i** seçin. 

   ![Microsoft Yazılım Lisans Koşullarını Kabul Edin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. CmD konsolu açılır. İlk kez kullananlar için **3**girmeniz ve ardından veri giriş ve çıktınız için yerel klasör yolunu bulmanız gerekir. Yolu ters eğik çizgilerle tanımlamada başarısız olursanız, ileri eğik çizgileri deneyin. Diğer seçenekler için varsayılan değerleri kullanabilirsiniz.

   ![Visual Studio Code yerel çalışma cmd için Veri Gölü Araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Komut paletini açmak için Ctrl+Shift+P'yi seçin, **ADL girin: İş Gönder'i**ve ardından işi yerel hesabınıza göndermek için **Yerel'i** seçin.

   ![Visual Studio Code için Veri Gölü Araçları yerel seçin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. İşi gönderdikten sonra, gönderim ayrıntılarını görüntüleyebilirsiniz. Gönderme ayrıntılarını görüntülemek için **Çıktı** penceresinde **jobUrl'yi** seçin. Ayrıca cmd konsolundan iş gönderme durumunu da görüntüleyebilirsiniz. Daha fazla iş ayrıntısı öğrenmek istiyorsanız cmd konsoluna **7** girin.

   ![Visual Studio Code yerel çalışma](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![çıkışı Için Veri Gölü Araçları Visual Studio Code yerel çalışma cmd durumu için Veri Gölü Araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>U-SQL işi için yerel hata ayıklama başlatma  
İlk kez kullanıcı için:

1. AdL'yi kullanın: [U-SQL yerel çalıştırma ortamını](#set-up-the-u-sql-local-run-environment)ayarlamadıysanız, yerel çalıştırma paketlerini indirmek için **Yerel Çalıştır Paketini indirin.**
2. Yüklü değilse, ileti kutusunda önerilen .NET Core SDK 2.0'ı yükleyin.
 
  ![hatırlatma yükler Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Yüklü değilse ileti kutusunda önerildiği gibi Visual Studio Kodu için C# yükleyin.Devam etmek için **Yükle'yi** tıklatın ve ardından VSCode'u yeniden başlatın.

    ![C yükleme anımsatıcı #](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Yerel hata ayıklama gerçekleştirmek için aşağıdaki adımları izleyin:
  
1. Komut paletini açmak için Ctrl+Shift+P'yi seçin ve ardından **ADL'yi girin: Yerel Çalıştırma Hizmetini Başlat.** CmD konsolu açılır. **DataRoot'un** ayarlandıkdan emin olun.
2. C# kod arkanızda bir kesme noktası ayarlayın.
3. Komut dosyası düzenleyicisine geri dön, sağ tıklayın ve **ADL: Yerel Hata Ayıklama'yı**seçin.
    
   ![Visual Studio Code yerel hata ayıklama sonucu için Veri Gölü Araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [VSCode'da Azure Veri Gölü Analitiği için Python, R ve CSharp ile U-SQL geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [PowerShell'i kullanarak Data Lake Analytics ile başlayın](data-lake-analytics-get-started-powershell.md)
* [Azure portalını kullanarak Data Lake Analytics ile başlayın](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamaları geliştirmek için Visual Studio için Veri Gölü Araçlarını Kullanma](data-lake-analytics-data-lake-tools-get-started.md)
* [Veri Gölü Analizi(U-SQL) kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md)
