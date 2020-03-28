---
title: Azure Media Services ile video dosyalarını akışa alma - .NET | Microsoft Docs
description: Yeni bir Azure Medya Hizmetleri hesabı oluşturmak, bir dosyayı kodlamak ve Azure Media Player'a aktarmak için bu öğreticinin adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc
ms.date: 08/19/2019
ms.author: juliako
ms.openlocfilehash: df4092ecc3f7d075f1a2821854cdb668ee2cebe5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77191215"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---net"></a>Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışlayın - .NET

Bu öğretici, Azure Medya Hizmetleri'ni kullanarak çok çeşitli tarayıcılarda ve cihazlarda video kodlamanın ve video akışı başlatmanın ne kadar kolay olduğunu gösterir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.
Bu konu başlığındaki örnek, bir HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri kodlar. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Öğreticinin sonunda bir video akışı mümkün olacak.  

![Videoyu yürütme](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)’yi edinebilirsiniz.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)<br/>Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

Örnek [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles) klasöründe bulunur.

İndirilen projede [appsettings.json'ı](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/appsettings.json) açın. Değerleri [API'lara erişen](access-api-cli-how-to.md)kimlik bilgileriyle değiştirin.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. **Dönüşüm** oluşturur (ilk olarak, belirtilen Dönüşüm var olup olmadığını denetler). 
2. Kodlama **İş**'in çıktısı olarak kullanılan bir çıktı **Varlık** oluşturur.
3. Bir HTTPS URL'sini temel alan **İş**girişi oluşturur.
4. Daha önce oluşturulan giriş ve çıktıyı kullanarak kodlama **Işini** gönderir.
5. İşin durumunu denetler.
6. **Akış Lı Yer Bulucu**oluşturur.
7. Akış URL'leri oluşturur.

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Uygulamayı çalıştırdığınızda, farklı protokolleri kullanarak videoyu kayıttan yürütmek için kullanılabilen URL’ler görüntülenir. 

1. *EncodeAndStreamFiles* uygulamasını çalıştırmak için Ctrl+F5 tuşlarına basın.
2. Apple’ın **HLS** protokolünü (*manifest(format=m3u8-aapl)* ile biter) seçin ve konsoldan akış URL’sini kopyalayın.

![Çıktı](./media/stream-files-tutorial-with-api/output.png)

Örneğin [kaynak kodunda](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs), URL’nin nasıl oluşturulduğunu görebilirsiniz. Bunu derlemek için, akış uç noktasının ana bilgisayar adını ve akış bulucu yolunu birleştirmeniz gerekir.  

## <a name="test-with-azure-media-player"></a>Azure Media Player ile test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/)açın ve 'ye gidin.
2. **URL:** kutusuna, uygulamayı çalıştırdığınızda aldığınız akış URL değerlerinden birini yapıştırın. 
 
     URL'yi HLS, Dash veya Smooth biçiminde yapıştırabilirsiniz ve Azure Media Player cihazınızda otomatik olarak oynatılmak üzere uygun bir akış protokolüne geçer.
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici için oluşturduğunuz Medya Hizmetleri ve depolama hesapları da dahil olmak üzere kaynak grubunuzdaki kaynaklardan herhangi biri ne olursa olsun artık ihtiyacınız yoksa, kaynak grubunu silin.

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>Kodu inceleme

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

[Dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md) öğreticisi size ayrıntılı açıklamaları içeren daha gelişmiş bir akışa alma örneği sunar. 

### <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK’ları, iş parçacığı güvenli değildir. Çok iş parçacıklı uygulama ile çalışırken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
