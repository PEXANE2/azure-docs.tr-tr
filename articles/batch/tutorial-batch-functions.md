---
title: Azure Işlevleri 'ni kullanarak bir Batch işi tetikleyin
description: Öğretici-bir depolama blobuna eklendikçe taranmış belgelere OCR uygulama
author: LauraBrenner
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc
ms.openlocfilehash: a967fdc14b85f294ee11cbcc57a8d2280dba38e8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77017199"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Öğretici: Azure Işlevleri 'ni kullanarak bir Batch işi tetikleyin

Bu öğreticide, Azure Işlevleri 'ni kullanarak bir toplu işi nasıl tetikleyeceğinizi öğreneceksiniz. Azure Storage blob kapsayıcısına eklenen belgelere, Azure Batch aracılığıyla bunlara uygulanan optik karakter tanıma (OCR) ile bir örnek adım adım inceleyeceğiz. OCR işlemesini kolaylaştırmak için, blob kapsayıcısına her dosya eklendiğinde bir Batch OCR işi çalıştıran bir Azure işlevi yapılandıracağız.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Hesap oluşturma ve bağlama hakkında daha fazla bilgi için bkz. [Batch hesabı oluşturma](quick-create-portal.md#create-a-batch-account) .
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Batch Explorer kullanarak bir Batch havuzu ve toplu iş oluşturma

Bu bölümde, OCR görevlerini çalıştıracak toplu iş havuzunu ve toplu işi oluşturmak için Batch Explorer kullanacaksınız. 

### <a name="create-a-pool"></a>Havuz oluşturma

1. Batch Explorer için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Sol taraftaki çubukta **havuzlar** ' ı seçerek bir havuz oluşturun, sonra arama formunun üzerindeki **Ekle** düğmesine basın. 
    1. Bir KIMLIK ve görünen ad seçin. Bu örnek için `ocr-pool` kullanacağız.
    1. Ölçek türünü **sabit boyut**olarak ayarlayın ve adanmış düğüm sayısını 3 olarak ayarlayın.
    1. İşletim sistemi olarak **Ubuntu 18,04-LTS** ' yi seçin.
    1. Sanal makine boyutu olarak `Standard_f2s_v2` seçin.
    1. Başlangıç görevini etkinleştirin ve komutu `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"`ekleyin. Kullanıcı kimliğini **görev varsayılan kullanıcısı (yönetici)** olarak ayarladığınızdan emin olun. Bu, başlangıç görevlerinin `sudo`komutları içermesini sağlar.
    1. **Tamam**’ı seçin.
### <a name="create-a-job"></a>Bir iş oluşturma

1. Sol taraftaki çubukta **işler** ' i seçerek havuzda iş oluşturun, sonra arama formunun üzerindeki **Ekle** düğmesine basın. 
    1. Bir KIMLIK ve görünen ad seçin. Bu örnek için `ocr-job` kullanacağız.
    1. Havuzu `ocr-pool`veya havuzunuz için seçtiğiniz herhangi bir adı ayarlayın.
    1. **Tamam**’ı seçin.


## <a name="create-blob-containers"></a>Blob kapsayıcıları oluşturma

Burada, OCR toplu işi için giriş ve çıkış dosyalarınızı depolayacak blob kapsayıcıları oluşturacaksınız.

1. Depolama Gezgini için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Batch hesabınıza bağlı depolama hesabını kullanarak, [BLOB kapsayıcısı oluşturma](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#create-a-blob-container)konumundaki adımları izleyerek iki blob kapsayıcı (bir diğeri çıkış dosyaları için bir tane) oluşturun.

Bu örnekte, giriş kapsayıcısına `input` adı verilir ve OCR olmayan tüm belgelerin işlenmek üzere başlangıçta karşıya yüklendiği yerdir. Çıkış kapsayıcısına `output` adı verilir ve toplu iş, işlenen belgeleri OCR ile yazar.  
    * Bu örnekte, giriş kapsayımuzu `input`ve çıkış kapsayımızın `output`arayacağız.  
    * Giriş kapsayıcısı, OCR olmayan tüm belgelerin başlangıçta karşıya yüklendiği yerdir.  
    * Çıktı kapsayıcısı, toplu işin belge OCR ile yazdığı yerdir.  

Depolama Gezgini ' de çıkış Kapsayıcınız için bir paylaşılan erişim imzası oluşturun. Bunu, çıkış kapsayıcısına sağ tıklayıp, **paylaşılan erişim Imzası al..** . seçeneğini belirleyerek yapın. **İzinler**altında, **yazma**' yı işaretleyin. Başka izin gerekmez.  

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu bölümde, giriş kapsayıcınıza her dosya yüklendiğinde OCR toplu işini tetikleyen Azure Işlevini oluşturacaksınız.

1. Bir işlev oluşturmak için [Azure Blob depolama tarafından tetiklenen bir Işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-storage-blob-triggered-function) bölümündeki adımları izleyin.
    1. Bir depolama hesabı istendiğinde, Batch hesabınıza bağladığınız depolama hesabını kullanın.
    1. **Çalışma zamanı yığını**için .net ' i seçin. Batch .NET SDK özelliğinden yararlanmak için C# ' de işlevimizi yazacağız.
1. Blob ile tetiklenen işlev oluşturulduktan sonra, Işlevindeki GitHub 'dan [`run.csx`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) ve [`function.proj`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) kullanın.
    * Giriş blobu kapsayıcınıza yeni bir blob eklendiğinde `run.csx` çalıştırılır.
    * `function.proj`, Işlev kodunuzda dış kitaplıkları listeler, örneğin Batch .NET SDK.
1. `run.csx` dosyasının `Run()` işlevindeki değişkenlerin yer tutucu değerlerini, Batch ve depolama kimlik bilgilerinizi yansıtacak şekilde değiştirin. Batch ve Storage hesabı kimlik bilgilerinizi Batch hesabınızın **anahtarlar** bölümündeki Azure Portal bulabilirsiniz.
    * Batch hesabınızın **anahtarlar** bölümündeki Azure Portal Batch ve Storage hesabı kimlik bilgilerinizi alın. 

## <a name="trigger-the-function-and-retrieve-results"></a>İşlevi tetikleme ve sonuçları alma

Taranmış dosyaların herhangi birini veya tümünü GitHub 'daki [`input_files`](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) dizininden giriş kapsayıcınıza yükleyin. Her dosya için `ocr-pool` bir görevin eklendiğini onaylamak için Batch Explorer izleyin. Birkaç saniye sonra, OCR uygulanmış dosya çıkış kapsayıcısına eklenir. Dosya daha sonra görünür ve Depolama Gezgini alınabilir.

Ayrıca, Azure Işlevleri Web Düzenleyicisi penceresinin alt kısmında bulunan günlük dosyasını izleyebilirsiniz. burada, giriş kapsayıcınıza yüklediğiniz her dosya için bunun gibi iletiler görürsünüz:

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-05-29T19:45:25.848 [Information] Name of output text file: <outputTxtFile>
2019-05-29T19:45:25.848 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

Çıkış dosyalarını Depolama Gezgini yerel makinenize indirmek için önce istediğiniz dosyaları seçin ve ardından üst şeritte **İndir** ' i seçin. 

> [!TIP]
> İndirilen dosyalar bir PDF okuyucusunda açılırsa aranabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Havuzlar ve işler oluşturmak için Batch Explorer kullanma
> * Blob kapsayıcıları ve paylaşılan erişim imzası (SAS) oluşturmak için Depolama Gezgini kullanın
> * Blob ile tetiklenen bir Azure Işlevi oluşturma
> * Depolama hizmetine giriş dosyaları yükleme
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

* Batch iş yüklerini zamanlamak ve işlemek üzere .NET API kullanma hakkında daha fazla örnek için [GitHub 'daki örneklere](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp)bakın. 

* Batch iş yüklerini çalıştırmak için kullanabileceğiniz daha fazla Azure Işlevleri tetikleyicisi görmek için bkz. [Azure işlevleri belgeleri](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).
