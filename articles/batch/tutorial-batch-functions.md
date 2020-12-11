---
title: Öğretici-Azure Işlevleri 'ni kullanarak bir Batch işi tetikleyin
description: Öğretici-bir depolama blobuna eklendikçe taranmış belgelere OCR uygulama
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc, devx-track-csharp
ms.openlocfilehash: b441b4c4fcbeb089cef24c3a84fa33021e7840de
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97106391"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Öğretici: Azure Işlevleri 'ni kullanarak bir Batch işi tetikleyin

Bu öğreticide, [Azure işlevleri](../azure-functions/functions-overview.md)'ni kullanarak bir toplu işi nasıl tetikleyeceğinizi öğreneceksiniz. Azure Storage blob kapsayıcısına eklenen belgelere, Azure Batch aracılığıyla bunlara uygulanan optik karakter tanıma (OCR) ile bir örnek adım adım inceleyeceğiz. OCR işlemesini kolaylaştırmak için, blob kapsayıcısına her dosya eklendiğinde bir Batch OCR işi çalıştıran bir Azure işlevi yapılandıracağız. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Havuzlar ve işler oluşturmak için Batch Explorer kullanma
> * Blob kapsayıcıları ve paylaşılan erişim imzası (SAS) oluşturmak için Depolama Gezgini kullanın
> * Blob ile tetiklenen bir Azure Işlevi oluşturma
> * Depolama hizmetine giriş dosyaları yükleme
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Hesap oluşturma ve bağlama hakkında daha fazla bilgi için bkz. [Batch hesabı oluşturma](quick-create-portal.md#create-a-batch-account) .
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Batch Explorer kullanarak bir Batch havuzu ve toplu iş oluşturma

Bu bölümde, OCR görevlerini çalıştıracak toplu iş havuzunu ve toplu işi oluşturmak için Batch Explorer kullanacaksınız. 

### <a name="create-a-pool"></a>Havuz oluşturma

1. Batch Explorer için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Sol taraftaki çubukta **havuzlar** ' ı seçerek bir havuz oluşturun, sonra arama formunun üzerindeki **Ekle** düğmesine basın. 
    1. Bir KIMLIK ve görünen ad seçin. `ocr-pool`Bu örnek için kullanacağız.
    1. Ölçek türünü **sabit boyut** olarak ayarlayın ve adanmış düğüm sayısını 3 olarak ayarlayın.
    1. İşletim sistemi olarak **Ubuntu 18,04-LTS** ' yi seçin.
    1. `Standard_f2s_v2`Sanal makine boyutu olarak seçin.
    1. Başlangıç görevini etkinleştirin ve komutunu ekleyin `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"` . Kullanıcı kimliğini **görev varsayılan kullanıcısı (yönetici)** olarak ayarladığınızdan emin olun. Bu, ile komutları dahil etmek için başlangıç görevlerine izin verir `sudo` .
    1. **Tamam**’ı seçin.
### <a name="create-a-job"></a>İş oluşturma

1. Sol taraftaki çubukta **işler** ' i seçerek havuzda iş oluşturun, sonra arama formunun üzerindeki **Ekle** düğmesine basın. 
    1. Bir KIMLIK ve görünen ad seçin. `ocr-job`Bu örnek için kullanacağız.
    1. Havuzu `ocr-pool` veya havuzunuz için seçtiğiniz herhangi bir adı ayarlayın.
    1. **Tamam**’ı seçin.


## <a name="create-blob-containers"></a>Blob kapsayıcıları oluşturma

Burada, OCR toplu işi için giriş ve çıkış dosyalarınızı depolayacak blob kapsayıcıları oluşturacaksınız.

1. Depolama Gezgini için Azure kimlik bilgilerinizi kullanarak oturum açın.
1. Batch hesabınıza bağlı depolama hesabını kullanarak, [BLOB kapsayıcısı oluşturma](../vs-azure-tools-storage-explorer-blobs.md#create-a-blob-container)konumundaki adımları izleyerek iki blob kapsayıcı (bir diğeri çıkış dosyaları için bir tane) oluşturun.

Bu örnekte, giriş kapsayıcısı olarak adlandırılır ve bu, `input` OCR olmayan tüm belgelerin işlenmek üzere ilk olarak karşıya yüklendiği yerdir. Çıkış kapsayıcısı olarak adlandırılır `output` ve toplu işin, işlenen BELGELERI OCR ile yazdığı yerdir.  
    * Bu örnekte, giriş kapsayımuzu `input` ve çıkış kapsayımuzu arayacağız `output` .  
    * Giriş kapsayıcısı, OCR olmayan tüm belgelerin başlangıçta karşıya yüklendiği yerdir.  
    * Çıktı kapsayıcısı, toplu işin belge OCR ile yazdığı yerdir.  

Depolama Gezgini ' de çıkış Kapsayıcınız için bir paylaşılan erişim imzası oluşturun. Bunu, çıkış kapsayıcısına sağ tıklayıp, **paylaşılan erişim Imzası al..**. seçeneğini belirleyerek yapın. **İzinler** altında, **yazma**' yı işaretleyin. Başka izin gerekmez.  

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu bölümde, giriş kapsayıcınıza her dosya yüklendiğinde OCR toplu işini tetikleyen Azure Işlevini oluşturacaksınız.

1. Bir işlev oluşturmak için [Azure Blob depolama tarafından tetiklenen bir Işlev oluşturma](../azure-functions/functions-create-storage-blob-triggered-function.md) bölümündeki adımları izleyin.
    1. Bir depolama hesabı istendiğinde, Batch hesabınıza bağladığınız depolama hesabını kullanın.
    1. **Çalışma zamanı yığını** için .net ' i seçin. Batch .NET SDK 'sının yararlanmak Için C# ' de işlevimizi yazacağız.
1. Blob ile tetiklenen işlev oluşturulduktan sonra, [`run.csx`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) Işlevindeki GitHub 'dan ve ' ı kullanın [`function.proj`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) .
    * `run.csx` , giriş blobu kapsayıcınıza yeni bir blob eklendiğinde çalıştırılır.
    * `function.proj` Işlev kodunuzda dış kitaplıkları listeler, örneğin Batch .NET SDK.
1. Dosya işlevindeki değişkenlerin yer tutucu değerlerini `Run()` `run.csx` yığın ve depolama kimlik bilgilerinizi yansıtacak şekilde değiştirin. Batch ve Storage hesabı kimlik bilgilerinizi Batch hesabınızın **anahtarlar** bölümündeki Azure Portal bulabilirsiniz.
    * Batch hesabınızın **anahtarlar** bölümündeki Azure Portal Batch ve Storage hesabı kimlik bilgilerinizi alın. 

## <a name="trigger-the-function-and-retrieve-results"></a>İşlevi tetikleme ve sonuçları alma

Taranmış dosyaların herhangi birini veya tümünü [`input_files`](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) GitHub 'daki dizinden giriş kapsayıcınıza yükleyin. Her dosya için bir görevin eklendiğini doğrulamak üzere Batch Explorer izleyin `ocr-pool` . Birkaç saniye sonra, OCR uygulanmış dosya çıkış kapsayıcısına eklenir. Dosya daha sonra görünür ve Depolama Gezgini alınabilir.

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Zamanlanmış bir iş olmasa bile, düğümler çalışırken havuz için sizden ücret alınır. Havuz artık gerekli değilse silin. Hesap görünümünde **Havuzlar**'ı ve havuzun adını seçin. Ardından **Sil**’i seçin. Havuzu sildiğinizde düğümler üzerindeki tüm görev çıkışları silinir. Ancak çıkış dosyaları depolama hesabında kalır. Artık gerekli değilse, Batch hesabını ve depolama hesabını da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Havuzlar ve işler oluşturmak için Batch Explorer kullanma
> * Blob kapsayıcıları ve paylaşılan erişim imzası (SAS) oluşturmak için Depolama Gezgini kullanın
> * Blob ile tetiklenen bir Azure Işlevi oluşturma
> * Depolama hizmetine giriş dosyaları yükleme
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma


**Galeri** bölümündeki Batch Explorer aracılığıyla kullanılabilen işleme uygulamalarını inceleyerek devam edin. Her uygulama için birkaç şablon vardır ve şablon sayısı zaman içinde artacaktır. Örneğin bir görüntüyü parçalara ayırarak bu parçaların paralel olarak işlenmesini sağlayan Blender şablonları bulunmaktadır.

Batch iş yüklerini zamanlamak ve işlemek üzere .NET API kullanmaya ilişkin daha fazla örnek için GitHub üzerindeki örneklere bakın.

> [!div class="nextstepaction"]
> [Batch C# örnekleri](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp)
