---
title: Video Indexer hesabı yönetme
titleSuffix: Azure Media Services
description: Azure 'a bağlı bir Video Indexer hesabını yönetmeyi öğrenin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/16/2019
ms.author: juliako
ms.openlocfilehash: 5b9ec1c04dd90af9d9380a1f4d30386c8f9cc3ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79499660"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Azure 'a bağlı Video Indexer hesabını yönetme

Bu makalede, Azure aboneliğinize ve bir Azure Media Services hesabına bağlı Video Indexer hesabının nasıl yönetileceği gösterilmektedir.

> [!NOTE]
> Bu konuda açıklanan hesap yapılandırma ayarlarını yapmak için Video Indexer hesap sahibi olmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar

[Azure 'A bağlı](connect-to-azure.md)olarak açıklandığı gibi video Indexer hesabınızı Azure 'a bağlayın.

Makalesindeki [önkoşulları](connect-to-azure.md#prerequisites) ve gözden geçirme [konularını](connect-to-azure.md#considerations) izlediğinizden emin olun.

## <a name="examine-account-settings"></a>Hesap ayarlarını inceleyin

Bu bölümde Video Indexer hesabınızın ayarları incelenir.

Ayarları görüntülemek için:

1. Sağ üst köşedeki Kullanıcı simgesine tıklayın ve **Ayarlar**' ı seçin.

    ![Video Indexer ayarları](./media/manage-account-connected-to-azure/select-settings.png)

2. **Ayarlar** sayfasında **Hesap** sekmesini seçin.

Videolarınızın Dizin Oluşturucu hesabınız Azure 'a bağlıysa, aşağıdaki işlemleri görürsünüz:

* Temel alınan Azure Media Services hesabının adı.
* Çalıştıran ve kuyruğa alınan dizin oluşturma işi sayısı.
* Ayrılmış ayrılmış birimlerin sayısı ve türü.

Hesabınızın bazı ayarlamaları olması gerekiyorsa, **Ayarlar** sayfasında hesap yapılandırmanızla ilgili hataları ve uyarıları görürsünüz. İletiler, değişiklikler yapmanız gereken Azure portal tam konumlarına bağlantılar içerir. Daha fazla bilgi için aşağıdaki [hata ve uyarılar](#errors-and-warnings) bölümüne bakın.

## <a name="repair-the-connection-to-azure"></a>Azure bağlantısını onarın

[Video Indexer](https://www.videoindexer.ai/) sayfanızın **Azure Media Services bağlantısını Güncelleştir** iletişim kutusunda, aşağıdaki ayarlar için değerler sağlamanız istenir:

|Ayar|Açıklama|
|---|---|
|Azure abonelik KIMLIĞI|Abonelik KIMLIĞI Azure portal elde edilebilir. Sol panelde **tüm hizmetler** ' e tıklayın ve "abonelikler" i arayın. **Abonelikler** ' i seçin ve aboneliklerinizin LISTESINDEN istediğiniz kimliği seçin.|
|Azure Media Services kaynak grubu adı|Media Services hesabı oluşturduğunuz kaynak grubunun adı.|
|Uygulama Kimliği|Bu Video Indexer hesabı için oluşturduğunuz Azure AD uygulama KIMLIĞI (belirtilen Media Services hesabının izinleriyle). <br/><br/>Uygulama KIMLIĞINI almak için Azure portal gidin. Media Services hesap altında, hesabınızı seçin ve **API erişimi**' ne gidin. **Hizmet sorumlusu Azure AD uygulaması Media Services API 'sine Bağlan**' ı seçin  ->  **Azure AD App**. İlgili parametreleri kopyalayın.|
|Uygulama anahtarı|Yukarıda belirttiğiniz Media Services hesabınızla ilişkili Azure AD uygulama anahtarı. <br/><br/>Uygulama anahtarını almak için Azure portal gidin. Media Services hesap altında, hesabınızı seçin ve **API erişimi**' ne gidin. **Hizmet sorumlusu ile Media Services API 'sine Bağlan**' ı seçerek  ->  gizli dizileri &**uygulama**  ->  **sertifikalarını**yönetin. İlgili parametreleri kopyalayın.|

## <a name="autoscale-reserved-units"></a>Otomatik ölçeklendirme ayrılmış birimleri

**Ayarlar** sayfası, medya ayrılmış BIRIMLERININ (ru) otomatik ölçeklendirilmesini ayarlamanıza olanak sağlar. Seçenek açık ise, en fazla ru sayısını ayırabilir ve Video Indexer ru **'yi**otomatik olarak durdurduğundan emin olabilirsiniz. Bu seçenekle, boş zamanlı olarak fazladan para ödemezsiniz, ancak dizin oluşturma yükü yüksek olduğunda dizin oluşturma işlerinin uzun bir süre tamamlanmasını bekleyemez.

Otomatik ölçeklendirme, 1 RU veya üzerinde Media Services hesabının varsayılan sınırının üzerinde ölçeklendirmez. Limiti artırmak için bir hizmet isteği oluşturun. Kotalar ve sınırlamalar ve bir destek biletini açma hakkında bilgi için bkz. [Kotalar ve sınırlamalar](../../media-services/previous/media-services-quotas-and-limitations.md).

![Otomatik ölçeklendirme ayrılmış birimleri Video Indexer](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Hatalar ve uyarılar

Hesabınızın bazı ayarlamaları olması gerekiyorsa, **Ayarlar** sayfasında hesap yapılandırmanızla ilgili ilgili hataları ve uyarıları görürsünüz. İletiler, değişiklikler yapmanız gereken Azure portal tam konumlarına bağlantılar içerir. Bu bölüm, hata ve uyarı iletileri hakkında daha fazla ayrıntı sağlar.

* EventGrid

    Azure portal kullanarak EventGrid kaynak sağlayıcısını kaydetmeniz gerekir. [Azure Portal](https://portal.azure.com/) **abonelikler** ' e gidin > [abonelik] > **resourceproviders**  >  **Microsoft. eventgrid**. **Kayıtlı** durumda değilse, **Kaydet**' i seçin. Kaydolmak birkaç dakika sürer.

* Akış uç noktası

    Temel alınan Media Services hesabının, başlatılmış durumda varsayılan **akış uç noktasına** sahip olduğundan emin olun. Aksi takdirde, bu Media Services hesabından veya Video Indexer videoları izleyemiyoruz.

* Medya ayrılmış birimleri

    Videoları indekslemek için medya hizmeti kaynağınız üzerinde medya ayrılmış birimleri ayırmanız gerekir. En iyi dizin oluşturma performansı için, en az 10 S3 ayrılmış birim ayırmanız önerilir. Fiyatlandırma bilgileri için [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfasının SSS bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

' Deki yönergeleri izleyerek deneme hesabınızla Video Indexer veya Azure 'a bağlı hesaplarınızla programlı bir şekilde etkileşim kurabilirsiniz: [API 'Leri kullanma](video-indexer-use-apis.md).

Azure 'a bağlanırken kullandığınız Azure AD kullanıcısını kullanın.
