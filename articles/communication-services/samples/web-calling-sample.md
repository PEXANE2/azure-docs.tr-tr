---
title: Azure Iletişim Hizmetleri-Web çağırma örneği
titleSuffix: An Azure Communication Services sample
description: Iletişim Hizmetleri Web çağrısı örneği hakkında bilgi edinin
author: chriswhilar
manager: mariusu-msft
services: azure-communication-services
ms.author: mariusu
ms.date: 10/15/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: aadecd3c57f5a145efd43058a5113205d7517c0b
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102486412"
---
# <a name="get-started-with-the-web-calling-sample"></a>Web 'i çağıran örnekle çalışmaya başlama

Web çağıran örnek, Iletişim Hizmetleri Web 'i çağıran istemci kitaplığı tarafından sunulan çeşitli yeteneklere adım adım yönergeler olarak hizmet veren bir Web uygulamasıdır.

Bu örnek, geliştiriciler için oluşturulmuştur ve Iletişim Hizmetleri 'ni kullanmaya başlamanıza çok daha kolay hale gelir. Kullanıcı arabirimi, her biri bir "kodu göster" düğmesine sahip olan birden fazla bölüme ayrılmıştır ve bu, kodu doğrudan tarayıcınızdan kendi Iletişim Hizmetleri uygulamanıza kopyalamanızı sağlar.

## <a name="get-started-with-the-web-calling-sample"></a>Web 'i çağıran örnekle çalışmaya başlama

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]


> [!IMPORTANT]
> [Bu örnek GitHub ' da kullanılabilir.](https://github.com/Azure-Samples/communication-services-web-calling-tutorial/)

Projeyi ayarlamak ve makinenizde yerel olarak çalıştırmak için/Project/Readme.exe ' i izleyin.
[Web çağırma örneği](https://github.com/Azure-Samples/communication-services-web-calling-tutorial) makinenizde çalışır olduktan sonra, aşağıdaki giriş sayfasını görürsünüz:

:::image type="content" source="./media/web-calling-tutorial-page-1.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/web-calling-tutorial-page-1.png":::

:::image type="content" source="./media/web-calling-tutorial-page-2.png" alt-text="Web çağırma öğreticisi 2" lightbox="./media/web-calling-tutorial-page-2.png":::

## <a name="user-provisioning-and-sdk-initialization"></a>Kullanıcı hazırlama ve SDK başlatması

SDK 'nizi arka uç belirteç sağlama hizmeti tarafından sağlanan bir belirteç kullanarak başlatmak için "Kullanıcı sağlama ve SDK 'Yı başlatma" seçeneğine tıklayın. Bu arka uç hizmeti içinde `/project/webpack.config.js` .

Kendi çözümünüzde kullanabileceğiniz örnek kodu görmek için "kodu göster" düğmesine tıklayın.

SDK 'niz başlatıldıktan sonra aşağıdakileri görmeniz gerekir:

:::image type="content" source="./media/user-provisioning.png" alt-text="Kullanıcı sağlama" lightbox="./media/user-provisioning.png":::

Artık Iletişim Hizmetleri kaynağınızı kullanarak çağrı yapmaya başlamaya hazırsınız!

## <a name="placing-and-receiving-calls"></a>Çağrıları yerleştirme ve alma

Iletişim Hizmetleri Web 'i çağıran SDK, **1:1**, **1: N** ve **Grup** çağırma için izin verir.

1:1 veya 1: N giden çağrılar için, virgülle ayrılmış değerler kullanarak çağırmak üzere birden fazla Iletişim hizmeti kullanıcı kimliği belirtebilirsiniz. Ayrıca, virgülle ayrılmış değerler kullanarak çağırmak için geleneksel (PSTN) telefon numaralarını da belirtebilirsiniz.

PSTN telefon numaralarını çağırırken alternatif arayan KIMLIĞINIZI belirtin. Giden bir çağrı yerleştirmek için "Çağrı Yerleştir" düğmesine tıklayın:

:::image type="content" source="./media/place-a-call.png" alt-text="Bir çağrı yerleştir" lightbox="./media/place-a-call.png":::

Bir grup çağrısına katmak için, çağrıyı tanımlayan GUID 'yi girin ve "grubu Birleştir" düğmesine tıklayın:

:::image type="content" source="./media/join-a-group-call.png" alt-text="Grup çağrısına katılır" lightbox="./media/join-a-group-call.png":::

Çağrı yerleştirme, çağrı alma ve Grup çağrılarını birleştirme için örnek kodu görmek üzere "kodu göster" düğmesine tıklayın.

Etkin bir çağrı şöyle görünür:

:::image type="content" source="./media/group-call.png" alt-text="Grup çağrısı" lightbox="./media/group-call.png":::

Bu örnek, aşağıdaki yetenekler için de kod parçacıkları sağlar:

  - Video kameranızı açmak/devre dışı bırakmak için video simgesine tıklanın
  - Mikrofon simgesine tıklayarak mikrofonunuzu açın/kapatın
  - Aramayı tutmak/saklamak için Oynat simgesine tıklanması
  - Ekran simgesine tıklayarak ekranınızı paylaşmayı başlatın/durdurun
  - Çağrıya katılımcı eklemek için kişi simgesine tıklanması
  - Belirli bir katılımcıyı çağrıdan kaldırmak için katılımcı içindeki "katılımcıyı kaldır" seçeneğine tıklanın


## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
>[GitHub 'dan örneği indirin](https://github.com/Azure-Samples/communication-services-web-calling-tutorial/)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Çağıran istemci kitaplığını kullanma](../quickstarts/voice-video-calling/calling-client-samples.md) hakkında bilgi edinin
- [Nasıl çalıştığını çağırma](../concepts/voice-video-calling/about-call-types.md) hakkında daha fazla bilgi edinin
- [API başvuru belgelerini](/javascript/api/azure-communication-services/@azure/communication-calling/?view=azure-communication-services-js) gözden geçirme
- [Contoso Med uygulama](https://github.com/Azure-Samples/communication-services-contoso-med-app) örneğini gözden geçirme

## <a name="additional-reading"></a>Ek okuma

- [Azure Iletişim GitHub](https://github.com/Azure/communication) -resmi GitHub sayfasında daha fazla örnek ve bilgi bulun
- [Redux](https://redux.js.org/) -istemci tarafı durum yönetimi
- [Floentuı](https://aka.ms/fluent-ui) -MICROSOFT Powered UI kitaplığı
- Kullanıcı arabirimleri oluşturmak için [tepki](https://reactjs.org/) verme kitaplığı
- Web uygulamaları oluşturmak için [ASP.NET Core](/aspnet/core/introduction-to-aspnet-core?preserve-view=true&view=aspnetcore-3.1) çerçeve
