---
title: Azure Media Services ile video dosyalarını akışla - Node.js | Microsoft Dokümanlar
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
ms.openlocfilehash: fa9fbf3bac55ca0b26c3644b7f6818fa96088612
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "69639400"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---nodejs"></a>Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - Node.js

Bu öğretici, Azure Medya Hizmetleri'ni kullanarak çok çeşitli tarayıcılarda ve cihazlarda video kodlamanın ve video akışı başlatmanın ne kadar kolay olduğunu gösterir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.

Bu makaledeki örnek, https URL üzerinden erişilebilen içerikleri kodlar. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Öğreticinin sonunda bir video akışı mümkün olacak.  

![Videoyu yürütme](./media/stream-files-nodejs-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://nodejs.org/en/download/) yükle
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)<br/>Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.
- [Azure CLI ile Azure Medya Hizmetleri API'sindeki](access-api-cli-how-to.md) adımları izleyin ve kimlik bilgilerini kaydedin. API'ye erişmek için bunları kullanmanız gerekir.

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Akış Node.js örneğini içeren bir GitHub deposunu aşağıdaki komutu kullanarak makinenize kopyala:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Örnek [StreamFilesSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/master/AMSv3Samples/StreamFilesSample) klasöründe yer alır.

İndirdiğiniz projede [index.js'yi](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L25) açın. `endpoint config` Değerleri [API'lara erişen](access-api-cli-how-to.md)kimlik bilgileriyle değiştirin.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. **Dönüşüm** oluşturur (ilk olarak, belirtilen Dönüşüm var olup olmadığını denetler). 
2. Kodlama **İş**'in çıktısı olarak kullanılan bir çıktı **Varlık** oluşturur.
3. Bir HTTPS URL'sini temel alan **İş**girişi oluşturur.
4. Daha önce oluşturulan giriş ve çıktıyı kullanarak kodlama **Işini** gönderir.
5. İşin durumunu denetler.
6. **Akış Lı Yer Bulucu**oluşturur.
7. Akış URL'leri oluşturur.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. Uygulama kodlanmış dosyaları indirir. Çıktı dosyalarının gitmesini istediğiniz bir klasör oluşturun ve [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L39) dosyasındaki **outputFolder** değişkeninin değerini güncelleştirin.
1. **Komut istemini**açın, örneğin dizinine göz atın ve aşağıdaki komutları çalıştırın.

    ```
    npm install 
    node index.js
    ```

Çalıştırma yı bitirdikten sonra, benzer çıktıyı görmeniz gerekir:

![Çalıştırın](./media/stream-files-nodejs-quickstart/run.png)

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

## <a name="see-also"></a>Ayrıca bkz.

[İş hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Medya Hizmetleri kavramları](concepts-overview.md)
