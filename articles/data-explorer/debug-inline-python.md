---
title: VS Code kullanarak Sıralı Kod hata Ayıklama Kusto sorgu dili Python - Azure Veri Gezgini
description: VS Kodu kullanarak Kusto sorgu dilini (KQL) satır içinde Python'u nasıl hata ayıklayarak öğreneceğiz.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/04/2019
ms.openlocfilehash: 96bd66f96b04bd7032d976ba9ebbbeb60c8415e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444464"
---
# <a name="debug-kusto-query-language-inline-python-using-vs-code"></a>VS Kodu kullanarak Sıralı Kod lu Kusto sorgu dili

Azure Veri Gezgini, [python() eklentisini](/azure/kusto/query/pythonplugin)kullanarak Kusto sorgu diline katıştırılmış Python kodunu çalıştırmayı destekler. Eklenti çalışma süresi, izole edilmiş ve güvenli bir Python ortamında bir kum havuzunda barındırılır. Python() eklenti yeteneği, OSS Python paketlerinin büyük arşivi yle Kusto sorgu dili yerel işlevlerini genişletir. Bu uzantı, sorgunun bir parçası olarak makine öğrenimi, yapay zeka, istatistiksel ve zaman serileri gibi gelişmiş algoritmalar çalıştırmanızı sağlar.

Kusto sorgu dil araçları Python algoritmaları geliştirmek ve hata ayıklamak için uygun değildir. Bu nedenle, Jupyter, PyCharm, VS veya VS Kodu gibi favori Python entegre geliştirme ortamında algoritmayı geliştirin. Algoritma tamamlandığında, Kopyalayıp KQL'ye yapıştırın. Bu iş akışını iyileştirmek ve kolaylaştırmak için Azure Veri Gezgini, Kusto Explorer veya Web UI istemcileri ile KQL satır arasını Python kodunu yazma ve hata ayıklama için VS Kodu arasındaki tümleştirmeyi destekler. 

> [!NOTE]
> Bu iş akışı yalnızca nispeten küçük giriş tabloları (en az MB) hata ayıklamak için kullanılabilir. Bu nedenle, hata ayıklama için girişi sınırlamanız gerekebilir.  Büyük bir tabloyu işlemeniz gerekiyorsa, hata ayıklama `| take` `| sample`için `where rand() < 0.x`sınırlayın, veya .

## <a name="prerequisites"></a>Ön koşullar

1. Python [Anaconda Dağıtım](https://www.anaconda.com/distribution/#download-section)yükleyin. **Gelişmiş**Seçenekler'de, PATH **ortamı değişkenime Anaconda ekle'yi**seçin.
2. [Visual Studio Kodunu](https://code.visualstudio.com/Download) Yükleyin
3. [Visual Studio Code için Python uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-python.python)yükleyin.

## <a name="run-your-query-in-your-client-application"></a>Sorgunuzu istemci uygulamanızda çalıştırın

1. İstemci uygulamanızda, satır içinde Python içeren bir sorgu öne`set query_python_debug;`
1. Sorguyu çalıştırın.
    * Kusto Explorer: VS Kodu *debug_python.py* komut dosyası ile otomatik olarak başlatılır.
    * Kusto Web UI: 
        1. *Debug_python.py,* *df.txt*ve *kargs.txt dosyasını*indirip kaydedin. Pencerede **İzin Ver'i**seçin. Dosyaları seçili dizine **kaydedin.** 

            ![Web UI satır içinde python dosyalarını karşıdan yükler](media/debug-inline-python/webui-inline-python.png)

        1. *debug_python.py'ye* sağ tıklayın ve VS koduyla açın. 
        *debug_python.py* komut dosyası, *df.txt'den* giriş veri çerçevesini ve *kargs.txt'den*parametre sözlüğüne para girişi ni sorgulamak için şablon kodu tarafından önceden belirlenmiş KQL sorgusundan satır altı Python kodunu içerir.    
            
1. VS kodunda, VS kod hata ayıklayıcısını başlatın: **Hata Ayıklama** > **Hata Ayıklama (F5)**, **Python** yapılandırmasını seçin. Hata ayıklayıcı satır ara kodunu hata ayıklamak için başlatılır ve otomatik olarak kesme noktası.

### <a name="how-does-inline-python-debugging-in-vs-code-work"></a>VS Kodu'nda satır ara Python hata ayıklama nasıl çalışır?

1. Sorgu ayrıştırılır ve gerekli `| evaluate python()` yan tümceye ulaşılıncaya kadar sunucuda yürütülür.
1. Python sandbox çağrılır, ancak kodu çalıştırmak yerine, giriş tablosunu, parametrelersözlüğünü ve kodu seri hale getirerek istemciye geri gönderir.
1. Bu üç nesne üç dosyaya kaydedilir: *df.txt*, *kargs.txt*, ve *debug_python.py* seçili dizinde (Web UI) veya istemcide %TEMP% dizini (Kusto Explorer).
1. VS kodu başlatılır, kendi dosyalarından df ve kargs başlatmak için bir önek kodu içeren *debug_python.py* dosyası ile önceden yüklenir, ardından KQL sorgusuna gömülü Python komut dosyası.

## <a name="query-example"></a>Sorgu örneği

1. İstemci uygulamanızda aşağıdaki KQL sorgusunu çalıştırın:

    ```kusto
    range x from 1 to 4 step 1
    | evaluate python(typeof(*, x4:int), 
    'exp = kargs["exp"]\n'
    'result = df\n'
    'result["x4"] = df["x"].pow(exp)\n'
    , pack('exp', 4))
    ```

    Ortaya çıkan tabloya bakın:

    | x  | x4  |
    |---------|---------|
    | 1     |   1      |
    | 2     |   16      |
    | 3     |   81      |
    | 4     |    256     |
    
1. İstemci uygulamanızda aynı KQL `set query_python_debug;`sorgusunu aşağıdakileri kullanarak çalıştırın:

    ```kusto
    set query_python_debug;
    range x from 1 to 4 step 1
    | evaluate python(typeof(*, x4:int), 
    'exp = kargs["exp"]\n'
    'result = df\n'
    'result["x4"] = df["x"].pow(exp)\n'
    , pack('exp', 4))
    ```

1. VS Kodu başlatıldı:

    ![vs kodu başlatmak](media/debug-inline-python/launch-vs-code.png)

1. VS Kodu hata ayıklama konsolunda 'sonuç' veri çerçevesi nin hatasını ayıklar ve yazdırır:

    ![VS kodu hata ayıklama](media/debug-inline-python/debug-vs-code.png)

> [!NOTE]
> Python kum havuzu görüntüsü ile yerel yüklemeniz arasında farklılıklar olabilir. [Eklentiyi sorgulayarak belirli paketler için kum havuzu görüntüsünü işaretleyin.](https://github.com/Azure/azure-kusto-analytics-lib/blob/master/Utils/functions/get_modules_version.csl)
