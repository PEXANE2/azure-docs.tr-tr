---
title: Video Indexer hesabını yönetme
titleSuffix: Azure Media Services
description: Azure'a bağlı bir Video Dizinleyici hesabını nasıl yönetirize edin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/16/2019
ms.author: juliako
ms.openlocfilehash: 5b9ec1c04dd90af9d9380a1f4d30386c8f9cc3ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499660"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Dizinleyici hesabı yönetme

Bu makalede, Azure aboneliğinize ve Azure Medya Hizmetleri hesabınıza bağlı bir Video Dizinleyici hesabının nasıl yönetilen gösterildiği gösterilebilir.

> [!NOTE]
> Bu konuda tartışılan hesap yapılandırma ayarlamalarını yapmak için Video Indexer hesap sahibi niz olmalıdır.

## <a name="prerequisites"></a>Ön koşullar

Video Dizinleyici hesabınızı Azure'a [Bağlı'da](connect-to-azure.md)açıklandığı gibi Azure'a bağlayın.

[Önkoşulları](connect-to-azure.md#prerequisites) takip edip makalede [dikkat edilmesi gerekenleri](connect-to-azure.md#considerations) gözden geçirdiğinizden emin olun.

## <a name="examine-account-settings"></a>Hesap ayarlarını inceleme

Bu bölümde Video Dizinleyici hesabınızın ayarları incelenir.

Ayarları görüntülemek için:

1. Sağ üst köşedeki kullanıcı simgesine tıklayın ve **Ayarlar'ı**seçin.

    ![Video Dizinleyici'deki Ayarlar](./media/manage-account-connected-to-azure/select-settings.png)

2. **Ayarlar** sayfasında **Hesap** sekmesini seçin.

Videolar Dizinleyici hesabınız Azure'a bağlıysa aşağıdaki leri görürsünüz:

* Temel Azure Medya Hizmetleri hesabının adı.
* Çalışan ve sıraya dizleyen işlerin sayısı.
* Ayrılan birimlerin sayısı ve türü.

Hesabınızın bazı ayarlamalara ihtiyacı varsa, **Ayarlar** sayfasında hesap yapılandırmanızla ilgili alakalı hatalar ve uyarılar görürsünüz. İletiler, Azure portalında değişiklik yapmanız gereken tam yerlere bağlantılar içerir. Daha fazla bilgi için aşağıdaki [hatalar ve uyarılar](#errors-and-warnings) bölümüne bakın.

## <a name="repair-the-connection-to-azure"></a>Azure bağlantısını onarın

[Video Dizinleyici](https://www.videoindexer.ai/) sayfanızın **Azure Medya Hizmetleri bağlantısını güncelleştir** kutusunda, aşağıdaki ayarlar için değerler sağlamanız istenir:

|Ayar|Açıklama|
|---|---|
|Azure abonelik kimliği|Abonelik kimliği Azure portalından alınabilir. Sol paneldeki **tüm hizmetleri** tıklatın ve "abonelikleri" arayın. **Abonelikleri** seçin ve abonelikleriniz listesinden istediğiniz kimliği seçin.|
|Azure Medya Hizmetleri kaynak grubu adı|Medya Hizmetleri hesabını oluşturduğunuz kaynak grubunun adı.|
|Uygulama Kimliği|Bu Video Dizinleyici hesabı için oluşturduğunuz Azure AD uygulama kimliği (belirtilen Medya Hizmetleri hesabı için izinlerle birlikte). <br/><br/>Uygulama kimliğini almak için Azure portalına gidin. Medya Hizmetleri hesabı altında, hesabınızı seçin ve **API Access**gidin. Hizmet müdürü -> **Azure AD Uygulaması**yla Medya Hizmetlerine Bağlan **API'yi**seçin. İlgili parametreleri kopyalayın.|
|Uygulama anahtarı|Yukarıda belirttiğiniz Medya Hizmetleri hesabınızla ilişkili Azure REKLAM uygulama anahtarı. <br/><br/>Uygulama anahtarını almak için Azure portalına gidin. Medya Hizmetleri hesabı altında, hesabınızı seçin ve **API Access**gidin. Hizmet ->  **ana lığıyla Medya Hizmetlerine Bağlan API'yi**seçin Uygulama -> **Sertifikalarını & sırları****yönetin.** İlgili parametreleri kopyalayın.|

## <a name="autoscale-reserved-units"></a>Otomatik ölçek ayrılmış birimler

**Ayarlar** sayfası, ortam ayrılmış birimlerin (RU) otomatik ölçekleme sini ayarlamanızı sağlar. Seçenek A'daysa, en fazla RUs sayısını ayırabilir ve Video Dizinleyici'nin RUs'u otomatik olarak durdurduğundan/başlattığından emin olabilirsiniz. **On** Bu seçenekle, boşta kalma süresi için ekstra para ödemezsiniz, ancak dizin oluşturma yükü yüksek olduğunda dizin oluşturma işlerinin tamamlanmasını da beklemezsiniz.

Otomatik ölçek, 1 RU'nun altında veya Medya Hizmetleri hesabının varsayılan sınırının üzerinde ölçeklendirme yapmaz. Sınırı artırmak için bir hizmet isteği oluşturun. Kotalar ve sınırlamalar ve destek biletinin nasıl açılacağını hakkında bilgi için [Kotalar ve sınırlamalar](../../media-services/previous/media-services-quotas-and-limitations.md)bölümüne bakın.

![Otomatik ölçek ayrılmış birimler Video Indexer](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Hatalar ve uyarılar

Hesabınızın bazı ayarlamalara ihtiyacı varsa, **Ayarlar** sayfasında hesap yapılandırmanızla ilgili alakalı hatalar ve uyarılar görürsünüz. İletiler, Azure portalında değişiklik yapmanız gereken tam yerlere bağlantılar içerir. Bu bölümde hata ve uyarı iletileri hakkında daha fazla bilgi verir.

* EventGrid

    Azure portalını kullanarak EventGrid kaynak sağlayıcısını kaydetmeniz gerekir. Azure [portalında,](https://portal.azure.com/) **Kaynak Sağlayıcıları** > **Microsoft.EventGrid**> **Abonelikler** > [abonelik] gidin. **Kayıtlı** durumda değilse, **Kaydol'u**seçin. Kayıt yaptırmak birkaç dakika sürer.

* Akış uç noktası

    Temel Medya Hizmetleri hesabının başlangıç durumunda varsayılan **Akış Bitiş Noktası** olduğundan emin olun. Aksi takdirde, bu Medya Hizmetleri hesabından veya Video Indexer'da video izleyemezsiniz.

* Ortam ayrılmış birimler

    Videoları dizine dizine eklemek için Medya Hizmeti kaynağınıza Ortam Ayrılmış Birimler ayırmanız gerekir. En iyi dizin oluşturma performansı için en az 10 S3 Ayrılmış Birim ayırması önerilir. Fiyatlandırma bilgileri için [Medya Hizmetleri fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki yönergeleri izleyerek Azure'a bağlı deneme hesabınızla veya Video Dizinleyici hesaplarınızla programlanabilir şekilde etkileşimkurabilirsiniz: [API'leri kullanın.](video-indexer-use-apis.md)

Azure'a bağlanırken kullandığınız Aynı Azure AD kullanıcısını kullanın.
