---
title: Azure'a bağlı bir Video Dizinleyici hesabı oluşturma
titleSuffix: Azure Media Services
description: Azure'a bağlı bir Video Dizinleyici hesabı oluşturmayı öğrenin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 08/05/2019
ms.author: juliako
ms.openlocfilehash: 7ec8004fa0a54265962e79320c02287b2fcf814a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499925"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Dizinleyici hesabı oluşturma

Video Indexer hesabı oluştururken, ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası aldığınız) veya ücretli bir seçenek (kotayla sınırlı olmadığınız) seçebilirsiniz. Ücretsiz deneme sürümüyle Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ücretsiz dizin oluşturma ve API kullanıcılarına 2400 dakikaya kadar ücretsiz dizin oluşturma sağlar. Ücretli seçenekle, Azure aboneliğinize bağlı bir Video Dizinleyici hesabı ve Azure Medya Hizmetleri hesabı oluşturursunuz. Endeksli dakikaların yanı sıra medya hesabıyla ilgili ücretleri de öden.

Bu makalede, Bir Azure aboneliği ve Azure Medya Hizmetleri hesabına bağlı bir Video Dizinleyici hesabının nasıl oluşturulutamamgerektiği gösterilmektedir. Konu, otomatik (varsayılan) akışı kullanarak Azure'a bağlanmak için adımlar sağlar. Ayrıca Azure'a el ile nasıl bağlanılabildiğini (gelişmiş) de gösterir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği.

    Henüz bir Azure aboneliğiniz yoksa, Azure [Ücretsiz Deneme](https://azure.microsoft.com/free/)sürümüne kaydolun.

* Azure Etkin Dizin (Azure AD) etki alanı.

    Azure AD etki alanınız yoksa, Azure aboneliğinizle bu etki alanını oluşturun. Daha fazla bilgi için Azure [REKLAM'ınızda özel alan adlarını yönetme](../../active-directory/users-groups-roles/domains-manage.md)

* **Uygulama yöneticisi** rolü olan Azure AD etki alanınızdaki bir kullanıcı. Video Dizinleyici hesabınızı Azure'a bağlarken bu üyeyi kullanırsınız.

    Bu kullanıcı, bir iş veya okul hesabı olan bir Azure AD kullanıcısı olmalıdır. outlook.com, live.com veya hotmail.com gibi kişisel bir hesap kullanmayın.

    ![tüm AAD kullanıcıları](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Otomatik akış için ek ön koşullar

* Azure AD etki alanınızda bir kullanıcı ve üye.

    Video Dizinleyici hesabınızı Azure'a bağlarken bu üyeyi kullanırsınız.

    Bu kullanıcı, Azure aboneliğinizde **Sahibi** rolü olan veya hem **Katılımcı** hem de Kullanıcı **Erişim Yöneticisi** rolleri olan bir üye olmalıdır. Bir kullanıcı iki rol ile iki kez eklenebilir. Bir kez Katılımcı ile ve bir kez kullanıcı Access Administrator ile.

    ![erişim kontrolü](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>Manuel akış için ek ön koşullar

* Azure portalını kullanarak EventGrid kaynak sağlayıcısını kaydedin.

    Azure [portalında,](https://portal.azure.com/)Kaynak **Sağlayıcıları**>**Abonelikler**->[abonelik][abonelik]'e gidin.

    **Microsoft.Media** ve **Microsoft.EventGrid'i**arayın. "Kayıtlı" durumunda değilse, **Kaydol'u**tıklatın. Kayıt yaptırmak birkaç dakika sürer.

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma

> [!NOTE]
> Azure aboneliğiniz sertifika tabanlı çok faktörlü kimlik doğrulaması kullanıyorsa, gerekli sertifikaları yüklü olan bir aygıtta aşağıdaki adımları gerçekleştirmeniz çok önemlidir.

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.

2. Yeni **hesap oluştur** düğmesini seçin:

    ![Yeni Video Indexer hesabı oluşturma](./media/create-account/connect-to-azure.png)

3. Aboneliklistesi göründüğünde, kullanmak istediğiniz aboneliği seçin.

    ![Video DizinLeyici'yi Azure'a Bağla](./media/create-account/connect-vi-to-azure-subscription.png)

4. Desteklenen konumlardan bir Azure bölgesi seçin: Batı ABD 2, Kuzey Avrupa veya Doğu Asya.
5. **Azure Medya Hizmetleri hesabı**altında, şu seçeneklerden birini seçin:

    * Yeni bir Medya Hizmetleri hesabı oluşturmak için **yeni kaynak grubu oluştur'u**seçin. Kaynak grubunuz için bir ad sağlayın.

        Azure, aboneliğinizde yeni bir Azure Depolama hesabı da dahil olmak üzere yeni hesabınızı oluşturur. Yeni Medya Hizmetleri hesabınızda akış bitiş noktası ve 10 S3 ayrılmış birim içeren varsayılan bir ilk yapılandırma vardır.
    * Varolan bir Medya Hizmetleri hesabını kullanmak için **varolan kaynağı kullan'ı**seçin. Hesap listesinden hesabınızı seçin.

        Medya Hizmetleri hesabınız, Video Dizinleyici hesabınızla aynı bölgeye sahip olmalıdır.

        > [!NOTE]
        > Dizin oluşturma süresini ve düşük iş hacmini en aza indirmek için, Medya Hizmetleri hesabınızdaki [Ayrılmış Birimlerin](../previous/media-services-scale-media-processing-overview.md ) türünü ve sayısını **10 S3 Ayrılmış Birim**olarak ayarlamanız önerilir. [Ayrılmış birimleri değiştirmek için portalı kullanın'a](../previous/media-services-portal-scale-media-processing.md)bakın.

    * Bağlantınızı el ile yapılandırmak **için el ile yapılandırma bağlantısına geçiş** bağlantısını seçin.

        Ayrıntılı bilgi için aşağıdaki [Azure'a el ile Bağlan](#connect-to-azure-manually-advanced-option) (gelişmiş seçenek) bölümüne bakın.
6. Bittiğinde Connect'i **seçin.** Bu işlem birkaç dakika kadar sürebilir.

    Azure'a bağlandıktan sonra, yeni Video Dizinleyici hesabınız hesap listesinde görünür:

    ![yeni hesap](./media/create-account/new-account.png)

7. Yeni hesabınıza göz atın.

## <a name="connect-to-azure-manually-advanced-option"></a>Azure'a el ile bağlanma (gelişmiş seçenek)

Azure bağlantısı başarısız olduysa, el ile bağlanarak sorunu gidermeye çalışabilirsiniz.

> [!NOTE]
> Aynı bölgede aşağıdaki üç hesaba sahip olmak önerilir: Medya Hizmetleri hesabına bağladığınız Video Indexer hesabı nın yanı sıra aynı Medya Hizmetleri hesabına bağlı Azure depolama hesabı.

### <a name="create-and-configure-a-media-services-account"></a>Medya Hizmetleri hesabı oluşturma ve yapılandırma

1. Hesap Oluştur'da açıklandığı gibi bir Azure Medya Hizmetleri hesabı oluşturmak için [Azure](https://portal.azure.com/) [portalını](../previous/media-services-portal-create-account.md)kullanın.

    Medya Hizmetleri hesabınız için bir depolama hesabı oluştururken, hesap türü için **StorageV2'yi** ve çoğaltma alanları için **Coğrafi yedekli (GRS)** seçeneğini belirleyin.

    ![Yeni AMS hesabı](./media/create-account/create-ams-account1.png)

    > [!NOTE]
    > Medya Hizmetleri kaynağını ve hesap adlarını yazdığından emin olun. Bir sonraki bölümdeki adımlar için bunlara ihtiyacınız olacak.

2. Oluşturduğunuz Medya Hizmetleri hesabında [rezerve edilen birimlerin](../previous/media-services-scale-media-processing-overview.md ) türünü ve sayısını **10 S3 Ayrılmış Birim** olarak ayarlayın. [Ayrılmış birimleri değiştirmek için portalı kullanın'a](../previous/media-services-portal-scale-media-processing.md)bakın.
3. Video Dizinleyici web uygulamasında videolarınızı oynatmadan önce, yeni Medya Hizmetleri hesabının varsayılan **Akış Bitiş Noktasını** başlatmanız gerekir.

    Yeni Medya Hizmetleri hesabında **Akış uç noktalarını**seçin. Ardından akış bitiş noktasını seçin ve başlat'a basın.

    ![Yeni AMS hesabı](./media/create-account/create-ams-account2.png)

4. Video Indexer'ın Media Services API ile kimlik doğrulaması yapması için bir AD uygulamasının oluşturulması gerekir. Aşağıdaki adımlar, [Azure portalını kullanarak Azure AD kimlik doğrulaması ile başlayın'da](../previous/media-services-portal-get-started-with-aad.md)açıklanan Azure AD kimlik doğrulama işlemi nde size yol gösterin:

    1. Yeni Medya Hizmetleri hesabında **API erişimi'ni**seçin.
    2. [Hizmet ana kimlik doğrulama yöntemini](../previous/media-services-portal-get-started-with-aad.md)seçin.
    3. İstemci kimliğini ve istemcisırrını alın

        **Ayarlar**->**Tuşlarını**seçtikten sonra, **Açıklama**ekle, **Kaydet**tuşuna basın ve anahtar değeri doldurulur.

        Anahtarın süresi dolduğunda, hesap sahibinin anahtarı yenilemek için Video Indexer desteğine başvurması gerekir.

        > [!NOTE]
        > Anahtar değerini ve Uygulama Kimliğini yazdığından emin olun. Bir sonraki bölümdeki adımlar için ihtiyacınız olacak.

### <a name="connect-manually"></a>El ile bağlanma

[Video Dizinleyici'yi Video Dizinleyici](https://www.videoindexer.ai/) sayfanızın Azure abonelik iletişim **kutusuna bağlayın,** **el ile yapılandırma bağlantısına geçiş** bağlantısını seçin.

İletişim kutusunda, aşağıdaki bilgileri sağlayın:

|Ayar|Açıklama|
|---|---|
|Video Indexer hesap bölgesi|Video Indexer hesap bölgesinin adı. Daha iyi performans ve daha düşük maliyetler için, Azure Medya Hizmetleri kaynağının ve Azure Depolama hesabının bulunduğu bölgenin adını belirtmeniz önerilir. |
|Azure AD kiracı|Azure AD kiracısının adı, örneğin "contoso.onmicrosoft.com". Kiracı bilgileri Azure portalından alınabilir. İmlecinizi sağ üst köşedeki oturum açmış kullanıcının adının üzerine yerleştirin. **Etki Alanı'nın**sağındaki adı bulun.|
|Abonelik Kimliği|Bu bağlantının oluşturulması gereken Azure aboneliği. Abonelik kimliği Azure portalından alınabilir. Sol paneldeki **Tüm hizmetleri** seçin ve "abonelikleri" arayın. **Abonelikleri** seçin ve abonelikleriniz listesinden istediğiniz kimliği seçin.|
|Azure Medya Hizmetleri kaynak grubu adı|Medya Hizmetleri hesabını oluşturduğunuz kaynak grubunun adı.|
|Medya hizmeti kaynak adı|Önceki bölümde oluşturduğunuz Azure Medya Hizmetleri hesabının adı.|
|Uygulama Kimliği|Önceki bölümde oluşturduğunuz Azure AD uygulama kimliği (belirtilen Medya Hizmetleri hesabı için izinlerle birlikte).|
|Uygulama anahtarı|Önceki bölümde oluşturduğunuz Azure AD uygulama anahtarı. |

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki Azure Medya Hizmetleri ile ilgili hususlar geçerlidir:

* Otomatik olarak bağlanırsanız, Azure aboneliğinizde yeni bir Kaynak Grubu, Medya Hizmetleri hesabı ve bir Depolama hesabı görürsünüz.
* Otomatik olarak bağlanırsanız, Video Dizinleyici ortam **Ayrılmış Birimleri** 10 S3 birimine ayarlar:

    ![Medya Hizmetleri ayrılmış birimler](./media/create-account/ams-reserved-units.png)

* Varolan bir Medya Hizmetleri hesabına bağlanırsanız, Video Dizinleyici varolan ortam **Ayrılmış Birimler** yapılandırmasını değiştirmez.

   Ortam Ayrılmış Birimlerin türünü ve sayısını planladığınız yüke göre ayarlamanız gerekebilir. Yükünüz yüksekse ve yeterli biriminiz veya hızınız yoksa, video işlemenin zaman arızası yla sonuçlanabileceğini unutmayın.

* Yeni bir Medya Hizmetleri hesabına bağlanırsanız, Video Dizinleyici varsayılan **Akış Bitiş Noktasını** otomatik olarak başlatır:

    ![Medya Hizmetleri akışı bitiş noktası](./media/create-account/ams-streaming-endpoint.png)

    Akış uç noktalarının önemli bir başlangıç süresi vardır. Bu nedenle, videolarınızın Video Indexer web uygulamasında yayınlanıp izlenekadar hesabınızı Azure'a bağlamanızdan birkaç dakika sürebilir.

* Varolan bir Medya Hizmetleri hesabına bağlanırsanız, Video Dizinleyici varsayılan Akış Bitiş Noktası yapılandırmasını değiştirmez. **Akış Bitiş Noktası'nda**çalışan yoksa, bu Medya Hizmetleri hesabından veya Video Dizinleyici'den video izleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Deneme hesabınızla ve/veya Azure'a bağlı Video Dizinleyici hesaplarınızla aşağıdaki yönergeleri izleyerek programlanabilir şekilde etkileşimkurabilirsiniz: [API'leri kullanın.](video-indexer-use-apis.md)

Azure'a bağlanırken kullandığınız Aynı Azure AD kullanıcısını kullanmalısınız.