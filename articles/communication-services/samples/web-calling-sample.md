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
ms.openlocfilehash: e6fc3441fac5fe037e9a268d26012761d1fece70
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92463282"
---
# <a name="get-started-with-the-web-calling-sample"></a>Web 'i çağıran örnekle çalışmaya başlama

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

> [!IMPORTANT]
> [Bu örnek GitHub ' da kullanılabilir.](https://github.com/Azure-Samples/communication-services-web-calling-tutorial/)

Azure Iletişim Hizmetleri **Web çağırma örneği** , JavaScript ile çağırma deneyimi oluşturmak için istemci kitaplığı çağırma iletişim hizmetlerinin nasıl kullanılabileceğini gösterir.

Bu örnek hızlı başlangıçta, örneği yerel makinenizde çalıştırmadan önce örneğin nasıl çalıştığını öğreneceksiniz. Daha sonra Azure Iletişim Hizmetleri kaynaklarınızı kullanarak örneği Azure 'a dağıtırsınız.

## <a name="overview"></a>Genel Bakış

Web çağıran örnek, Iletişim Hizmetleri Web 'i çağıran istemci kitaplığı tarafından sunulan çeşitli yeteneklere adım adım yönergeler olarak hizmet veren bir Web uygulamasıdır. 

Bu örnek, geliştiriciler için oluşturulmuştur ve Iletişim Hizmetleri 'ni kullanmaya başlamanıza çok daha kolay hale gelir. Kullanıcı arabirimi, her biri bir "kodu göster" düğmesine sahip olan birden fazla bölüme ayrılmıştır ve bu, kodu doğrudan tarayıcınızdan kendi Iletişim Hizmetleri uygulamanıza kopyalamanızı sağlar.

[Web çağıran örnek](https://github.com/Azure-Samples/communication-services-web-calling-tutorial) makinenizde çalışırken, aşağıdaki giriş sayfasını görürsünüz:

:::image type="content" source="./media/web-calling-tutorial-page-1.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/web-calling-tutorial-page-1.png":::

:::image type="content" source="./media/web-calling-tutorial-page-2.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/web-calling-tutorial-page-2.png":::


## <a name="user-provisioning-and-sdk-initialization"></a>Kullanıcı hazırlama ve SDK başlatması 

Tanıtımı kullanmaya başlamak için [Iletişim Hizmetleri kaynağınızın](../quickstarts/create-communication-resource.md) bağlantı dizesini içine girin `config.json` . Bu, çağıran SDK 'nizin başlatılabilmesi için bir [Kullanıcı erişim belirteci](../concepts/authentication.md) sağlamak üzere kullanılacaktır.

Kullanıcı kimliği girişinde kendi kişisel tanımlayıcıyı girin. Burada hiçbir şey sağlanmazsa, rastgele bir kullanıcı kimliği oluşturulur. 

SDK 'nizi arka uç belirteç sağlama hizmeti tarafından sağlanan bir belirteç kullanarak başlatmak için "Kullanıcı sağlama ve SDK 'Yı başlatma" seçeneğine tıklayın. Bu arka uç hizmeti içinde `/project/webpack.config.js` .

Kendi çözümünüzde kullanabileceğiniz örnek kodu görmek için "kodu göster" düğmesine tıklayın.

SDK 'niz başlatıldıktan sonra aşağıdakileri görmeniz gerekir:

:::image type="content" source="./media/user-provisioning.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/user-provisioning.png":::

Artık Iletişim Hizmetleri kaynağınızı kullanarak çağrı yapmaya başlamaya hazırsınız!

## <a name="placing-and-receiving-calls"></a>Çağrıları yerleştirme ve alma

Iletişim Hizmetleri Web 'i çağıran SDK, **1:1**, **1: N**ve **Grup** çağırma için izin verir.

1:1 veya 1: N giden çağrılar için, virgülle ayrılmış değerler kullanarak çağırmak üzere birden fazla Iletişim hizmeti kullanıcı kimliği belirtebilirsiniz. Ayrıca, virgülle ayrılmış değerler kullanarak çağırmak için geleneksel (PSTN) telefon numaralarını da belirtebilirsiniz. 

PSTN telefon numaralarını çağırırken alternatif arayan KIMLIĞINIZI belirtin. Giden bir çağrı yerleştirmek için "Çağrı Yerleştir" düğmesine tıklayın:

:::image type="content" source="./media/place-a-call.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/place-a-call.png":::

Bir grup çağrısına katmak için, çağrıyı tanımlayan GUID 'yi girin ve "grubu Birleştir" düğmesine tıklayın:

:::image type="content" source="./media/join-a-group-call.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/join-a-group-call.png":::

Çağrı yerleştirme, çağrı alma ve Grup çağrılarını birleştirme için örnek kodu görmek üzere "kodu göster" düğmesine tıklayın.

Etkin bir çağrı şöyle görünür:

:::image type="content" source="./media/group-call.png" alt-text="Web çağrı öğreticisi 1" lightbox="./media/group-call.png":::

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
- [API başvuru belgelerini](https://docs.microsoft.com/javascript/api/azure-communication-services/@azure/communication-calling/?view=azure-communication-services-js) gözden geçirme

## <a name="additional-reading"></a>Ek okuma

- [Azure Iletişim GitHub](https://github.com/Azure/communication) -resmi GitHub sayfasında daha fazla örnek ve bilgi bulun
- [Redux](https://redux.js.org/) -istemci tarafı durum yönetimi
- [Floentuı](https://aka.ms/fluent-ui) -MICROSOFT Powered UI kitaplığı
- Kullanıcı arabirimleri oluşturmak için [tepki](https://reactjs.org/) verme kitaplığı
- Web uygulamaları oluşturmak için [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1&preserve-view=true) çerçeve
