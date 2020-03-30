---
title: 'Öğretici: Kullanıcı arabirimini özelleştirin'
titleSuffix: Azure AD B2C
description: Azure portalını kullanarak Azure Active Directory B2C'deki uygulamalarınızın kullanıcı arabirimini (UI) nasıl özelleştireceğimize bakın.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b2b2bc8dd4e60348553228b8b418df252a8c426a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186259"
---
# <a name="tutorial-customize-the-interface-of-user-experiences-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'deki kullanıcı deneyimlerinin arabirimini özelleştirin

Kaydolma, kaydolma ve profil düzenleme gibi daha yaygın kullanıcı deneyimleri için Azure Active Directory B2C'de (Azure AD B2C) [kullanıcı akışlarını](user-flow-overview.md) kullanabilirsiniz. Bu öğreticideki bilgiler, kendi HTML ve CSS dosyalarınızı kullanarak bu [deneyimlerin kullanıcı arabirimini (UI)](customize-ui-overview.md) nasıl özelleştireceğimize yardımcı olur.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı Arabirimi özelleştirme dosyaları oluşturma
> * Dosyaları kullanmak için kullanıcı akışını güncelleştirme
> * Özelleştirilmiş u-u bilgi sini test edin

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların uygulamanıza kaydolmasını ve oturum açmalarını sağlamak için [bir kullanıcı akışı oluşturun.](tutorial-create-user-flows.md)

## <a name="create-customization-files"></a>Özelleştirme dosyaları oluşturma

Bir Azure depolama hesabı ve kapsayıcı oluşturun ve ardından temel HTML ve CSS dosyalarını kapsayıcıya yersiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Dosyalarınızı birçok şekilde depolayabiliyor olsanız da, bu öğretici için dosyalarınızı [Azure Blob depolama alanında](../storage/blobs/storage-blobs-introduction.md)saklarsınız.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure aboneliğinizi içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve aboneliğinizi içeren dizini seçin. Bu dizin, Azure B2C kiracınızı içeren dizinden farklıdır.
3. Azure portalının sol üst köşesindeki tüm hizmetleri seçin, **Depolama hesaplarını**arayın ve seçin.
4. **Ekle'yi**seçin.
5. **Kaynak grubu**altında, **yeni oluştur'u**seçin, yeni kaynak grubu için bir ad girin ve sonra **Tamam'ı**tıklatın.
6. Depolama hesabı için bir ad girin. Seçtiğiniz ad Azure’da benzersiz olmalı, uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca sayı ile küçük harf içermelidir.
7. Depolama hesabının konumunu seçin veya varsayılan konumu kabul edin.
8. Diğer tüm varsayılan değerleri kabul edin, **Gözden Geçir + oluştur'u**seçin ve sonra **Oluştur'u**tıklatın.
9. Depolama hesabı oluşturulduktan sonra **kaynağa git'i**seçin.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

1. Depolama hesabının genel bakış sayfasında **Blobs'u**seçin.
2. **Kapsayıcı'yı**seçin, kapsayıcı için bir ad girin, **Blob'u seçin (yalnızca bloblar için anonim okuma erişimi)** ve ardından **Tamam'ı**tıklatın.

### <a name="enable-cors"></a>CORS'yi etkinleştirme

 Tarayıcıdaki Azure AD B2C kodu, kullanıcı akışında belirttiğiniz bir URL'den özel içerik yüklemek için modern ve standart bir yaklaşım kullanır. Orijinler arası kaynak paylaşımı (CORS), web sayfasındaki kısıtlı kaynakların diğer etki alanlarından istenmesine olanak tanır.

1. Menüde **CORS'i**seçin.
2. **İzin verilen kökenler**için , girin. `https://your-tenant-name.b2clogin.com` Azure `your-tenant-name` AD B2C kiracınızın adıyla değiştirin. Örneğin, `https://fabrikam.b2clogin.com`. Kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
3. **İzin Verilen**Yöntemler `GET``PUT`için `OPTIONS`, , , ve .
4. **İzin Verilen Üstbilgi için**yıldız işareti (*) girin.
5. **Exposed Üstbilgi için**yıldız işareti (*) girin.
6. **Max yaş**için, 200 girin.

    ![Azure portalında Azure Blob depolamaalanında CORS yapılandırma sayfası](./media/tutorial-customize-ui/enable-cors.png)

5. **Kaydet**'e tıklayın.

### <a name="create-the-customization-files"></a>Özelleştirme dosyalarını oluşturma

Kayıt deneyiminin Kullanıcı Bira'sını özelleştirmek için basit bir HTML ve CSS dosyası oluşturarak başlarsınız. HTML'nizi istediğiniz şekilde yapılandırabilirsiniz, ancak 'ın tanımlayıcısı olan bir **div** öğesi `api`olmalıdır. Örneğin, `<div id="api"></div>`. Azure AD B2C, sayfa `api` görüntülendiğinde kapsayıcıya öğeler enjekte eder.

1. Yerel bir klasörde, aşağıdaki dosyayı oluşturun `your-storage-account` ve depolama hesabının adını `your-container` ve oluşturduğunuz kapsayıcının adına geçiş yaptığınızdan emin olun. Örneğin, `https://store1.blob.core.windows.net/b2c/style.css`.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>My B2C Application</title>
        <link rel="stylesheet" href="https://your-storage-account.blob.core.windows.net/your-container/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>
      </body>
    </html>
    ```

    Sayfa istediğiniz şekilde tasarlanabilir, ancak **api** div öğesi oluşturduğunuz herhangi bir HTML özelleştirme dosyası için gereklidir.

3. Dosyayı *custom-ui.html*olarak kaydedin.
4. Azure AD B2C'nin enjekte ettiği öğeler de dahil olmak üzere kaydolma veya kaydolma sayfasındaki tüm öğeleri merkezleyen aşağıdaki basit CSS'yi oluşturun.

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center;
    }
    .entry {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center;
    }
    .create {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

5. Dosyayı *style.css*olarak kaydet.

### <a name="upload-the-customization-files"></a>Özelleştirme dosyalarını yükleme

Bu eğitimde, oluşturduğunuz dosyaları Azure AD B2C'nin bunlara erişebilmeleri için depolama hesabında saklarsınız.

1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin, **Depolama hesaplarını**arayın ve seçin.
2. Oluşturduğunuz depolama hesabını seçin, **Blobs'u**seçin ve ardından oluşturduğunuz kapsayıcıyı seçin.
3. **Upload'u**seçin, gidin ve *custom-ui.html* dosyasını seçin ve ardından **Yükle'yi**tıklatın.

    ![Upload düğmesi ve Dosyalar vurgulanmış portalda blob sayfası yükleyin](./media/tutorial-customize-ui/upload-blob.png)

4. Daha sonra öğreticide kullanmak üzere yüklediğiniz dosyanın URL'sini kopyalayın.
5. *style.css* dosyası için adım 3 ve 4'u tekrarlayın.

## <a name="update-the-user-flow"></a>Kullanıcı akışını güncelleştirme

1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
2. **Kullanıcı akışlarını (ilkeler)** seçin ve ardından *B2C_1_signupsignin1* kullanıcı akışını seçin.
3. **Sayfa düzenlerini**seçin ve ardından **Birleşik Kayıt veya Oturum Açma sayfası**altında, Özel sayfa içeriğini **kullanmak**için **Evet'i** tıklatın.
4. **Özel sayfa URI,** daha önce kaydettiğiniz *custom-ui.html* dosyası için URI girin.
5. Sayfanın üst kısmında **Kaydet'i**seçin.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

1. Azure AD B2C kiracınızda **Kullanıcı akışlarını** seçin ve *B2C_1_signupsignin1* kullanıcı akışını seçin.
2. Sayfanın üst **kısmında, Kullanıcı akışını çalıştır'ı**tıklatın.
3. Kullanıcı **akışını Çalıştır** düğmesini tıklatın.

    ![Kaydolma veya oturum açma kullanıcı akışı için kullanıcı akış sayfasını çalıştırma](./media/tutorial-customize-ui/run-user-flow.png)

    Oluşturduğunuz CSS dosyasına dayalı öğelerle aşağıdaki örneğe benzer bir sayfa görmeniz gerekir:

    ![Özel Kullanıcı Sürümü öğeleriyle kaydolma veya oturum açma yı gösteren web tarayıcısı](./media/tutorial-customize-ui/run-now.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Kullanıcı Arabirimi özelleştirme dosyaları oluşturma
> * Dosyaları kullanmak için kullanıcı akışını güncelleştirme
> * Özelleştirilmiş u-u bilgi sini test edin

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de dil özelleştirme](user-flow-language-customization.md)
