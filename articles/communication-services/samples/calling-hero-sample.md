---
title: Hero örneği çağıran Grup
titleSuffix: An Azure Communication Services sample overview
description: Geliştiricilerin, örneğin iç işleyişi hakkında daha fazla bilgi edinmek için Azure Iletişim hizmetlerini kullanarak Hero örneği çağırma konusuna genel bakış.
author: ddematheu
manager: nimag
services: azure-communication-services
ms.author: dademath
ms.date: 07/20/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: caee5686695594604f49dcbade54342a9134abc0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947996"
---
# <a name="get-started-with-the-group-calling-hero-sample"></a>Hero örneği çağıran grup ile çalışmaya başlama

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

<!----
> [!WARNING]
> Add links to our Hero Sample repo when the sample is publicly available.
---->

**Hero örneğini çağıran** Azure Iletişim Hizmetleri grubu, bir grup çağırma deneyimi oluşturmak için Web istemcisi kitaplığı 'Nı çağıran iletişim hizmetlerinin nasıl kullanılabileceğini gösterir.

Bu örnek hızlı başlangıçta, örneği yerel makinenizde çalıştırmadan önce örneğin nasıl çalıştığını öğreneceksiniz. Daha sonra Azure Iletişim Hizmetleri kaynaklarınızı kullanarak örneği Azure 'a dağıtırsınız.

> [!IMPORTANT]
> [GitHub 'dan örneği indirin](https://github.com/Azure/Communication/tree/master/samples)

## <a name="overview"></a>Genel Bakış

Örnekte hem istemci tarafı uygulaması hem de sunucu tarafı uygulaması vardır. **İstemci tarafı uygulama** , Microsoft 'un akıcı Kullanıcı arabirimi çerçevesini kullanan bir yanıt veren/Redux Web uygulamasıdır. Bu uygulama, istemci tarafı uygulamanın Azure 'a bağlanmasına yardımcı olan bir ASP.NET Core **sunucu tarafı uygulamasına** istek gönderir. 

Örnek şöyle görünür:

:::image type="content" source="./media/calling/landing-page.png" alt-text="Örnek uygulamanın giriş sayfasını gösteren ekran görüntüsü.":::

"Çağrı Başlat" düğmesine bastığınızda Web uygulaması, sunucu tarafı uygulamadan bir Kullanıcı erişim belirteci getirir. Bu belirteç daha sonra istemci uygulamasını Azure Communication Services 'a bağlamak için kullanılır. Belirteç alındıktan sonra, kullanmak istediğiniz kamerayı ve mikrofonu belirtmeniz istenecektir. İki durumlu denetimlerle cihazlarınızı devre dışı bırakabileceksiniz/etkinleştirebilirsiniz:

:::image type="content" source="./media/calling/pre-call.png" alt-text="Örnek uygulamanın çağrı öncesi ekranını gösteren ekran görüntüsü.":::

Görüntü adınızı ve cihazlarınızı yapılandırdıktan sonra, çağrı oturumuna katılabilir. Artık çekirdek çağırma deneyiminin yaşadığı ana çağrı tuvali görüntülenir.

:::image type="content" source="./media/calling/main-app.png" alt-text="Örnek uygulamanın ana ekranını gösteren ekran görüntüsü.":::

Ana arama ekranının bileşenleri:

- **Medya Galerisi**: katılımcıların gösterildiği ana aşama. Bir katılımcının Kamerası etkinse, video akışları burada gösterilir. Her katılımcının, görünen adını ve video akışını (bir tane olduğunda) gösteren bir tek kutucuğu vardır
- **Üst bilgi**: Bu, birincil çağrı denetimlerinin ayarları ve katılımcı tarafı çubuğunu açıp, video ve karışımı açma/kapatma, ekran paylaşma ve çağrıyı bırakma olarak bulunduğu yerdir.
- **Yan çubuk**: Bu, üst bilgi üzerindeki denetimler kullanılarak, katılımcılar ve ayar bilgilerinin gösterildiği yerdir. Bileşen sağ üst köşedeki ' X ' kullanılarak kapatılabilir. Katılımcılar yan çubuğu, katılımcıların bir listesini ve sohbet 'e daha fazla kullanıcı davet etmek için bir bağlantı gösterir. Ayarlar yan çubuğu, mikrofon ve kamera ayarlarını yapılandırmanıza olanak tanır.

Aşağıda, Önkoşullar hakkında daha fazla bilgi, örneği ayarlama adımları ve çeşitli bileşenleri konusunda kendinizi tanımak için adım adım öğreticiler bulacaksınız.

## <a name="prerequisites"></a>Önkoşullar

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Node.js (12.18.4 ve üzeri)](https://nodejs.org/en/download/)
- [Visual Studio (2019 ve üstü)](https://visualstudio.microsoft.com/vs/)
- [.NET Core 2,2](https://dotnet.microsoft.com/download/dotnet-core/2.2) (Visual Studio örneğinize karşılık gelen sürümü yüklediğinizden emin olun, 32 vs 64 bit)
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../quickstarts/create-communication-resource.md). Bu hızlı başlangıç için kaynak **Bağlantı dizenizi** kaydetmeniz gerekir.

## <a name="locally-deploy-the-service--client-applications"></a>Hizmet & istemci uygulamalarını yerel olarak dağıtın

Örnek çağıran grup temelde iki uygulamadır: ClientApp ve Service.NET uygulaması.

Yerel olarak dağıtmak istiyoruz, her iki uygulamayı da başlatmanız gerekir. Sunucu uygulaması tarayıcıdan ziyaret edildiğinde, Kullanıcı deneyimi için yerel olarak dağıtılan ClientApp kullanır.

Çoklu Kullanıcı çağrısının benzetimini yapmak için çağrın URL 'siyle birden çok tarayıcı oturumu açarak örneği yerel olarak test edebilirsiniz.

### <a name="before-running-the-sample-for-the-first-time"></a>Örneği ilk kez çalıştırmadan önce

1. PowerShell, Windows Terminal, komut Istemi veya eşdeğer bir örnek açın ve örneği kopyalamak istediğiniz dizine gidin.
2. `git clone`
3. **Çağrı/ClientApp klasörüne** gidin ve Çalıştır`npm run setup`
   1. Bir hata 1 görürseniz, istemcinizi yetkilendirmek için gitmeniz gereken bir URL için çıktıda yukarıya bakın. (URL şöyle görünür: `app.vssps.visualstudio.com/oauth2/authorize?clientid=...` ) Bir tarayıcıda URL 'YI ziyaret ettiğinizde, tarayıcı penceresinden komutu kopyalayın ve çalıştırın.
   2. `npm run setup-vsts-auth`Önceki adımı tamamladıktan sonra komutu tekrar çalıştırın.
4. Azure portal alın `Connection String` . Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [Azure Iletişim kaynakları oluşturma](../quickstarts/create-communication-resource.md)
5. Bağlantı dizesini aldıktan sonra, bağlantı dizesini hizmet .NET klasörü altında bulunan **çağrı/appsetting.js** dosyasına ekleyin. Bağlantı dizenizi şu değişkende girin: `ResourceConnectionString` .

### <a name="local-run"></a>Yerel çalıştırma

1. Çağıran klasöre git
2. `Calling.csproj`Visual Studio 'da çözümü açın
2. Projeyi Çalıştır `Calling` *

* Tarayıcı, `localhost:5000` (düğümün istemci uygulamasını dağıttığı) konumunda açılır. Uygulama Internet Explorer 'da desteklenmiyor.

#### <a name="troubleshooting"></a>Sorun giderme

- Çözüm derlenmez; NPM yükleme/oluşturma sırasında hatalar oluşturur.

   Projeleri temizlemeyi/yeniden derlemeyi deneyin.

## <a name="publish-the-sample-to-azure"></a>Örneği Azure 'da yayımlayın

1. Projeye sağ tıklayın `Calling` ve Yayımla ' yı seçin.
2. Yeni bir yayımlama profili oluşturun ve Azure aboneliğinizi seçin.
3. Yayımlamadan önce, Bağlantı dizenizi ile ekleyin `Edit App Service Settings` ve `ResourceConnectionString` anahtar olarak girin ve Bağlantı dizenizi (appsettings.jsüzerinde kopyalanmış) değer olarak girin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](../quickstarts/create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Çağıran istemci kitaplığını kullanma](../quickstarts/voice-video-calling/calling-client-samples.md) hakkında bilgi edinin
- [İstemci kitaplığı yeteneklerini çağırma](../quickstarts/voice-video-calling/calling-client-samples.md) hakkında bilgi edinin
- [Nasıl çalıştığını çağırma](../concepts/voice-video-calling/about-call-types.md) hakkında daha fazla bilgi edinin

## <a name="additional-reading"></a>Ek okuma

- [Azure Iletişim önizlemesi](https://github.com/Azure/communication-preview) -çağıran Web SDK 'sı hakkında daha fazla bilgi edinmek için
- [Redux](https://redux.js.org/) -istemci tarafı durum yönetimi
- [Floentuı](https://developer.microsoft.com/fluentui#/) -MICROSOFT Powered UI kitaplığı
- Kullanıcı arabirimleri oluşturmak için [tepki](https://reactjs.org/) verme kitaplığı
- Web uygulamaları oluşturmak için [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1&preserve-view=true) çerçeve
