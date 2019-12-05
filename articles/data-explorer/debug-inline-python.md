---
title: VS Code-Azure Veri Gezgini kullanarak hata ayıklama kusto sorgu dili satır içi Python
description: VS Code kullanarak kusto sorgu dili (KQL) satır içi Python hatalarını ayıklamayı öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/04/2019
ms.openlocfilehash: f0bf81ce6392e1650d9986d81a55eac9c416c24d
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74822909"
---
# <a name="debug-kusto-query-language-inline-python-using-vs-code"></a>VS Code kullanarak kusto sorgu dili satır içi Python hatalarını ayıklama

Azure Veri Gezgini, [Python () eklentisi](/azure/kusto/query/pythonplugin)kullanılarak kusto sorgu dilinde Embedded Python kodunun çalıştırılmasını destekler. Eklenti çalışma zamanı, yalıtılmış ve güvenli bir Python ortamında bir korumalı alan içinde barındırılır. Python () eklenti özelliği, kusto sorgu dili yerel işlevlerini, OSS Python paketlerinin çok büyük arşiviyle genişletir. Bu uzantı, sorgunun parçası olarak makine öğrenimi, yapay zeka, istatistiksel ve zaman serisi gibi gelişmiş algoritmaları çalıştırmanızı sağlar.

Kusto sorgu dili araçları, Python algoritmalarının geliştirilmesi ve hata ayıklaması için uygun değildir. Bu nedenle, Jupyıter, Pydüğme, VS veya VS Code gibi, en sevdiğiniz Python tümleşik geliştirme ortamınızda algoritmayı geliştirin. Algoritma tamamlandığında, kopyalayın ve KQL içine yapıştırın. Azure Veri Gezgini, bu iş akışını geliştirmek ve kolaylaştırmak için kusto Explorer veya Web UI istemcileri arasındaki tümleştirmeyi ve KQL satır içi Python kodunu yazma ve hata ayıklama için VS Code destekler. 

> [!NOTE]
> Bu iş akışı, yalnızca görece küçük giriş tablolarında (en fazla MB) hata ayıklamak için kullanılabilir. Bu nedenle, hata ayıklama için girişi sınırlandırlamanız gerekebilir.  Büyük bir tabloyu işlemek gerekirse, `| take`, `| sample`veya `where rand() < 0.x`kullanarak hata ayıklama için sınırlayın.

## <a name="prerequisites"></a>Önkoşullar

1. Python [Anaconda dağıtımını](https://www.anaconda.com/distribution/#download-section)yükler. **Gelişmiş Seçenekler**' de, **path ortam değişkenine Anaconda Ekle**' yi seçin.
2. [Visual Studio Code](https://code.visualstudio.com/Download)'u yükleme
3. [Visual Studio Code Için Python uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-python.python)yükler.

## <a name="run-your-query-in-your-client-application"></a>Sorgunuzu istemci uygulamanızda çalıştırın

1. İstemci uygulamanızda, `set query_python_debug;` satır içi Python içeren bir sorgu öneki
1. Sorguyu çalıştırın.
    * Kusto Explorer: VS Code otomatik olarak *debug_python. Kopyala* betiği ile başlatılır.
    * Kusto Web Kullanıcı arabirimi: 
        1. *Debug_python. Kopyala*, *df. txt*ve *kargs. txt*dosyasını indirip kaydedin. Penceresinde **Izin ver**' i seçin. Dosyaları seçili dizine **kaydedin** . 

            ![Web Kullanıcı arabirimi, satır içi Python dosyalarını indirir](media/debug-inline-python/webui-inline-python.png)

        1. *Debug_python. Kopyala* öğesine sağ tıklayın ve vs Code ile açın. 
        *Debug_python. Kopyala* betiği, KQL sorgusundan, giriş veri çerçevesini *df. txt* ' den ve *kargs. txt*' den parametrelerin sözlüğünden başlatmak için şablon kodunun ön eki olan satır içi Python kodunu içerir.    
            
1. VS Code 'da VS **Code hata ayıklayıcısını başlatın: hata** **ayıklamayı başlatın > (F5)** , **Python** yapılandırması ' nı seçin. Hata ayıklayıcı başlatılır ve satır içi kodda hata ayıklamak için otomatik olarak kesme noktası oluşturulur.

### <a name="how-does-inline-python-debugging-in-vs-code-work"></a>VS Code 'da satır içi Python hata ayıklaması nasıl çalışır?

1. Sorgu, gerekli `| evaluate python()` yan tümcesine ulaşılana kadar sunucuda ayrıştırılır ve yürütülür.
1. Python korumalı alanı çağrılır, ancak kodu çalıştırmak yerine, giriş tablosu, parametrelerin sözlüğü ve kod sözlüğü serileştirir ve bunları istemciye geri gönderir.
1. Bu üç nesne üç dosyaya kaydedilir: *df. txt*, *kargs. txt*ve *debug_python.* , seçili dizinde (Web Kullanıcı arabirimi) veya% Temp% Directory (kusto Explorer) istemcisinde.
1. VS Code başlatılır, ilgili dosyalarından df ve kargs 'yi başlatmak için bir ön ek kodu içeren *debug_python. Kopyala* dosyası ve sonra KQL sorgusuna gömülü Python betiği gelir.

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

    Elde edilen tabloya bakın:

    | x  | x4  |
    |---------|---------|
    | 1     |   1      |
    | 2     |   16      |
    | 3     |   81      |
    | 4     |    256     |
    
1. İstemci uygulamanızda `set query_python_debug;`kullanarak aynı KQL sorgusunu çalıştırın:

    ```kusto
    set query_python_debug;
    range x from 1 to 4 step 1
    | evaluate python(typeof(*, x4:int), 
    'exp = kargs["exp"]\n'
    'result = df\n'
    'result["x4"] = df["x"].pow(exp)\n'
    , pack('exp', 4))
    ```

1. VS Code başlatıldı:

    ![VS Code 'u Başlat](media/debug-inline-python/launch-vs-code.png)

1. Hata ayıklamalar VS Code ve hata ayıklama konsolundaki ' Result ' veri çerçevesini yazdırır:

    ![VS Code hata ayıklaması](media/debug-inline-python/debug-vs-code.png)

> [!NOTE]
> Python korumalı alan görüntüsü ile yerel yüklemeniz arasında farklılıklar olabilir. [Eklentiyi sorgulayarak belirli paketler için korumalı alan görüntüsünü kontrol edin](https://github.com/Azure/azure-kusto-analytics-lib/blob/master/Utils/functions/get_modules_version.csl).
