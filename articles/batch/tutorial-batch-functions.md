---
title: Azure İşlevlerini kullanarak Toplu iş tetikleme
description: Öğretici - Bir depolama blob'una eklendiklerinde taranan belgelere OCR uygulayın
author: LauraBrenner
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc
ms.openlocfilehash: a967fdc14b85f294ee11cbcc57a8d2280dba38e8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77017199"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Öğretici: Azure İşlevlerini kullanarak toplu iş tetikleme

Bu eğitimde, Azure İşlevlerini kullanarak toplu iş başlatmayı öğreneceksiniz. Azure Depolama blob kapsayıcısına eklenen belgelerin Azure Toplu İşi aracılığıyla optik karakter tanıma (OCR) uygulandığı bir örnek üzerinde çalışacağız. OCR işlemini kolaylaştırmak için, blob kapsayıcısına her dosya ekıldığında Toplu OCR işi çalıştıran bir Azure işlevi yapılandırAcağız.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure Batch hesabı ve bağlı bir Azure Depolama hesabı. Bkz. Hesap oluşturma ve bağlantı kurma hakkında daha fazla bilgi için [Toplu İş hesabı oluştur.](quick-create-portal.md#create-a-batch-account)
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Toplu İş Gezgini'ni kullanarak Toplu Birleştirme havuzu ve Toplu Iş oluşturma

Bu bölümde, OCR görevlerini çalıştıracak Toplu Iş havuzu ve Toplu Iş oluşturmak için Toplu Gezgin'i kullanırsınız. 

### <a name="create-a-pool"></a>Havuz oluşturma

1. Azure kimlik bilgilerinizi kullanarak Toplu İş Gezgini'nde oturum açın.
1. Sol taraftaki çubuktaki **Havuzlar'ı,** ardından arama formunun üzerindeki **Ekle** düğmesini seçerek bir havuz oluşturun. 
    1. Bir kimlik ve görüntü adı seçin. Bu örneği `ocr-pool` kullanacağız.
    1. Ölçek türünü **Sabit boyuta**ayarlayın ve özel düğüm sayısını 3 olarak ayarlayın.
    1. İşletim sistemi olarak **Ubuntu 18.04-LTS'yi** seçin.
    1. Sanal `Standard_f2s_v2` makine boyutu olarak seçin.
    1. Başlangıç görevini etkinleştirin ve `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"`komutu ekleyin. Kullanıcı kimliğini, başlangıç görevlerinin `sudo`komutları eklemesine olanak tanıyan Görev varsayılan kullanıcısı **(Yönetici)** olarak ayarladığınızdan emin olun.
    1. **Tamam'ı**seçin.
### <a name="create-a-job"></a>Bir iş oluşturma

1. Sol taraftaki çubuktaki **İşler'i** seçerek havuzda bir iş oluşturun, ardından arama formunun üzerindeki **Ekle** düğmesini seçin. 
    1. Bir kimlik ve görüntü adı seçin. Bu örneği `ocr-job` kullanacağız.
    1. Havuzu `ocr-pool`veya havuzunuz için hangi adı seçtiğinize ayarlayın.
    1. **Tamam'ı**seçin.


## <a name="create-blob-containers"></a>Blob kapları oluşturma

Burada, OCR Toplu iş için giriş ve çıktı dosyalarınızı depolayacak blob kapsayıcıları oluşturacaksınız.

1. Azure kimlik bilgilerinizi kullanarak Depolama Gezgini'nde oturum açın.
1. Toplu Iş hesabınıza bağlı depolama hesabını kullanarak, [blob kapsayıcısı oluştur'daki](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#create-a-blob-container)adımları izleyerek iki blob kapsayıcısı (biri giriş dosyaları için, diğeri çıkış dosyaları için) oluşturun.

Bu örnekte, giriş kapsayıcısı `input` adlandırılır ve OCR olmayan tüm belgelerin ilk olarak işlenmek üzere yüklendiği yerdir. Çıktı kapsayıcısı `output` adlandırılmış ve Toplu iş OCR ile işlenmiş belgeleri yazdığı yerdir.  
    * Bu örnekte, giriş kabımızı `input`ve çıkış kabımızı `output`çağıracağız.  
    * Giriş kapsayıcısı, OCR olmayan tüm belgelerin ilk yüklendiği yerdir.  
    * Çıktı kapsayıcısı, Toplu iş OCR ile belge yazdığı yerdir.  

Depolama Gezgini'nde çıktı kapsayıcınız için paylaşılan bir erişim imzası oluşturun. Bunu, çıkış kabına sağ tıklayarak ve **Paylaşılan Erişim İmzasını Al'ı**seçerek yapın... . **İzinler**altında , **Yaz'ı denetleyin.** Başka izin gerekmez.  

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu bölümde, bir dosya giriş kapsayıcınıza yüklendiğinde OCR Toplu Iş işini tetikleyen Azure İşi'ni oluşturursunuz.

1. Bir işlev oluşturmak için [Azure Blob depolama tarafından tetiklenen bir işlev oluştur'daki](https://docs.microsoft.com/azure/azure-functions/functions-create-storage-blob-triggered-function) adımları izleyin.
    1. Bir depolama hesabı için istendiğinde, Toplu İş hesabınıza bağlı olduğunuz aynı depolama hesabını kullanın.
    1. **Çalışma zamanı yığını**için .NET'i seçin. Toplu .NET SDK'dan yararlanmak için c# fonksiyonumuzu yazacağız.
1. Blob tarafından tetiklenen işlev oluşturulduktan [`run.csx`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) sonra, Fonksiyon'daki GitHub'ı ve [`function.proj`](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) GitHub'ı kullanın.
    * `run.csx`yeni bir blob giriş blob konteyner eklendiğinde çalıştırılır.
    * `function.proj`İşlev kodunuzdaki dış kitaplıkları listeler, örneğin Toplu İşlem .NET SDK.
1. Toplu Iş ve depolama kimlik bilgilerinizi `Run()` yansıtacak `run.csx` şekilde dosyanın işlevindeki değişkenlerin yer tutucu değerlerini değiştirin. Toplu İş ve depolama hesabı kimlik bilgilerinizi Azure portalında Toplu Hesabınızın **Anahtarlar** bölümünde bulabilirsiniz.
    * Toplu İş hesabınızın **Anahtarlar** bölümündeki Azure portalında Toplu Iş ve depolama hesabı kimlik bilgilerinizi alın. 

## <a name="trigger-the-function-and-retrieve-results"></a>İşlevi tetikle ve sonuçları alın

[`input_files`](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) GitHub'daki dizinden taranan dosyalardan herhangi birini veya tümlerini giriş kapsayıcınıza yükleyin. Her dosyaiçin bir görevin eklendiğini doğrulamak `ocr-pool` için Toplu İş Gezgini'ni izleyin. Birkaç saniye sonra, OCR uygulanan dosya çıkış kabına eklenir. Dosya daha sonra görünür ve Storage Explorer'da geri alınamaz.

Ayrıca, giriş kapsayıcınıza yüklediğiniz her dosya için buna benzer iletileri göreceğiniz Azure İşlemeweb düzenleyicisi penceresinin altındaki günlükler dosyasını izleyebilirsiniz:

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-05-29T19:45:25.848 [Information] Name of output text file: <outputTxtFile>
2019-05-29T19:45:25.848 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

Çıktı dosyalarını Depolama Gezgini'nden yerel makinenize indirmek için önce istediğiniz dosyaları seçin ve ardından üst şeritteki **İndir'i** seçin. 

> [!TIP]
> İndirilen dosyalar PDF okuyucuda açıldığında aranabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Havuzlar ve iş işleri oluşturmak için Toplu Gezgin'i kullanma
> * Blob kapsayıcıları ve paylaşılan erişim imzası (SAS) oluşturmak için Depolama Gezgini'ni kullanın
> * Blob tetiklenen Azure İşlevi oluşturma
> * Depolama hizmetine giriş dosyaları yükleme
> * Görev yürütmeyi izleme
> * Çıkış dosyalarını alma

* Toplu iş yüklerini planlamak ve işlemek için .NET API'sini kullanmanın diğer örnekleri [için, GitHub'daki örneklere](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp)bakın. 

* Toplu Iş yükünü çalıştırmak için kullanabileceğiniz daha fazla Azure İşi tetikleyicisi görmek için [Azure İşlevleri belgelerine](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)bakın.
