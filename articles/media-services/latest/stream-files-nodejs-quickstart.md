---
title: Azure Media Services ile video dosyaları akışı Node.js
description: Bu öğreticinin adımlarını izleyerek yeni bir Azure Media Services hesabı oluşturun, bir dosya kodlayın ve Azure Media Player için akışını yapın.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc, devx-track-js
ms.date: 02/17/2021
ms.author: inhenkel
ms.openlocfilehash: 0eb334111a8f5ffc63d0f858966e4e65f99d3b16
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101095948"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---nodejs"></a>Öğretici: URL 'yi temel alarak uzak bir dosyayı kodlayın ve videoyu akışa Node.js

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu öğreticide, Azure Media Services kullanarak çok çeşitli tarayıcılarda ve cihazlarda akış videolarının kodlanması ve başlatılması ne kadar kolay olduğunu gösterir. Bir giriş video dosyası, HTTPS URL 'Leri, SAS URL 'Leri veya Azure Blob depolamada bulunan dosyaların yolları kullanılarak belirtilebilir.

Bu makaledeki örnek, bir HTTPS URL 'SI aracılığıyla erişilebilir hale getirmek istediğiniz içeriği kodluyor. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Öğreticinin sonuna kadar, HLS veya DASH istemci oynatıcı kullanarak bir videoyu karşıya yüklemeyi, kodlamayı ve akışa alma hakkında bilgi sahibi olursunuz.

![Videoyu yürütme](./media/stream-files-nodejs-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/) yüklensin
- [Media Services hesabı oluşturun](./create-account-howto.md).<br/>Kaynak grubu adı ve Media Services hesap adı için kullandığınız değerleri anımsadığınızdan emin olun.
- [Azure CLI Ile Access Azure Media Services API 'sindeki](./access-api-howto.md) adımları izleyin ve kimlik bilgilerini kaydedin. API 'ye erişmek için bunları kullanmanız gerekir.
- Node.js istemci SDK 'sını nasıl kullanacağınızı anlamak için [Node.jsIle yapılandırma ve bağlanma ](./configure-connect-nodejs-howto.md) hakkında bilgi edinin

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, akış Node.js örneğini içeren bir GitHub deposunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Örnek, [Streamfilesörnek](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/master/AMSv3Samples/StreamFilesSample) klasöründe bulunur.

Projeyi indirdiğiniz [Dizin. TS](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.ts) 'yi açın. Kök klasördeki Sample. env dosyasını, [API](./access-api-howto.md)'larından aldığınız değerler ve kimlik bilgileriyle güncelleştirin. Sample. env dosyasını. env olarak yeniden adlandırın. 

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Bir **dönüşüm** oluşturur (ilk olarak, belirtilen dönüştürmenin var olup olmadığını denetler). 
2. Kodlama **işinin** çıkışı olarak kullanılan bir çıktı **varlığı** oluşturur.
1. İsteğe bağlı olarak, Depolama Blobu SDK 'sını kullanarak yerel bir dosyayı karşıya yükler.
1. Bir HTTPS URL 'sini veya karşıya yüklenen dosyayı temel alan, **işin** girdisini oluşturur
1. Daha önce oluşturulmuş giriş ve çıktıyı kullanarak kodlama **Işini** [içerik algılayan kodlama ön ayarıyla](./content-aware-encoding.md)gönderir.
1. İşin durumunu denetler.
1. Kodlama işinin çıkışını yerel bir klasöre indirir.
1. Player 'da kullanmak üzere bir **akış Bulucu** oluşturur.
1. HLS ve DASH için akış URL 'Leri oluşturur.
1. İçeriği bir oyuncu uygulaması Azure Media Player geri çalar.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. Uygulama kodlanmış dosyaları indirir. Çıkış dosyalarının gitmesini istediğiniz klasörü oluşturun ve [index. TS](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L59) dosyasındaki **outputfolder** değişkeninin değerini güncelleştirin. Varsayılan olarak "Temp" olarak ayarlanır.
1. **Komut istemi**' ni açın, örnek dizinine gidin ve aşağıdaki komutları yürütün.
1. Dizini AMSv3Samples klasörüne değiştirme
    ```bash
    cd AMSv3Samples
    ```

1. Üzerinde packages.jskullanılan paketleri yükler
    ```bash
    npm install 
    ```

1. Dizini Streamfilesörnek klasörü olarak değiştirme
    ```bash
    cd StreamFilesSample
    ```

1. AMSv3Samples klasöründen Visual Studio Code başlatın. Bu, ". vscode" klasörünün ve dosyalardaki tsconfig.jsbulunduğu klasörden başlatmak için gereklidir

    ```bash
    cd ..
    code .
    ```

StreamFilesSample klasörünü açın ve Visual Studio Code düzenleyicisinde index. TS dosyasını açın.
İndex. TS dosyasında, hata ayıklayıcıyı başlatmak için F5 tuşuna basın.


## <a name="test-with-azure-media-player"></a>Azure Media Player ile test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. Ayrıca, Shaka oynatıcı, HLS.js, Dash.js veya diğerleri gibi her türlü HLS veya DASH uyumlu oynatıcı da kullanabilirsiniz.

Örnekte oluşturulan bağlantıya tıklayabilir ve daha önce yüklü olan DASH bildirimiyle AMP oynatıcı 'yı başlatabilirsiniz.

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir Web tarayıcısı açın ve adresine gidin [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) .
2. **URL:** kutusuna, uygulamayı çalıştırdığınızda aldığınız akış URL değerlerinden birini yapıştırın. 
 
     URL 'yi HLS, Dash veya kesintisiz biçimde yapıştırabilir ve Azure Media Player cihazınızda otomatik olarak kayıttan yürütmek için uygun bir akış protokolüne geçiş yapar.
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunuzda, bu öğretici için oluşturduğunuz Media Services ve depolama hesapları da dahil olmak üzere herhangi bir kaynağa ihtiyacınız yoksa, kaynak grubunu silin.

Aşağıdaki CLı komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="more-developer-documentation-for-nodejs-on-azure"></a>Azure 'da Node.js için daha fazla geliştirici belgesi
- [JavaScript & için Azure Node.js geliştiriciler](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Git hub deposunda Media Services kaynak kodu @azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)
- [Node.js geliştiricileri için Azure paket belgeleri](https://docs.microsoft.com/javascript/api/overview/azure/?view=azure-node-latest)

## <a name="see-also"></a>Ayrıca bkz.

- [İş hata kodları](/rest/api/media/jobs/get#joberrorcode).
- [NPM yüklemesi @azure/arm-mediaservices](https://www.npmjs.com/package/@azure/arm-mediaservices)
- [JavaScript & için Azure Node.js geliştiriciler](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Depodaki Media Services kaynak kodu @azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services kavramlar](concepts-overview.md)
