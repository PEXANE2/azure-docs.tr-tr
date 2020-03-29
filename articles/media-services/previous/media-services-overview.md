---
title: Azure Media Services’a genel bakış | Microsoft Docs
description: Microsoft Azure Media Services, geliştiricilerin ölçeklenebilir medya yönetimi ve teslimi uygulamaları oluşturmalarına olanak tanıyan genişletilebilir bir bulut tabanlı platformdur. Bu makalede, Azure Medya Hizmetleri'ne genel bir bakış verebisi yer almaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: juliako
ms.openlocfilehash: 1c2d6287a89c7816c30cf26978859c07dba0251d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197513"
---
# <a name="azure-media-services-overview"></a>Azure Media Services’a genel bakış 

> [!div class="op_single_selector" title1="Kullandığınız Medya Hizmetleri sürümünü seçin:"]
> * [Sürüm 3](../latest/media-services-overview.md)
> * [Sürüm 2](media-services-overview.md)

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Microsoft Azure Media Services (AMS), geliştiricilerin ölçeklenebilir medya yönetimi ve teslimi uygulamaları oluşturmalarına olanak tanıyan genişletilebilir bir bulut tabanlı platformdur. Media Services, çeşitli istemcilere (TV, PC ve mobil cihazlar gibi) isteğe bağlı olarak veya canlı akış halinde teslim amacıyla video ve ses içeriklerini güvenli bir şekilde yüklemenizi, depolamanızı, kodlamanızı ve paketlemenizi sağlayan REST API'lerini temel alır.

Yalnızca Media Services’i kullanarak uçtan uca iş akışları oluşturabilirsiniz. Ayrıca, iş akışınızın bazı bölümleri için üçüncü taraf bileşenleri kullanmayı da tercih edebilirsiniz. Örneğin, bir üçüncü taraf kodlayıcısı kullanarak kodlayın. Daha sonra Media Services’i kullanarak yükleyin, koruyun, paketleyin ve teslim edin. İçeriğinizi canlı akışla aktarmayı veya isteğe bağlı içerik teslimini tercih edebilirsiniz. 


## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik

Önemli bir hatırlatma olarak, Azure Medya Hizmetlerini kullanımınızda geçerli tüm yasalara uymanız gerekir ve Medya Hizmetlerini veya herhangi bir Azure hizmetini başkalarının haklarını ihlal eden veya başkalarına zarar verebilecek şekilde kullanamazsınız.

Medya Hizmetlerine herhangi bir video/resim yüklemeden önce, video/görüntüyü kullanmadan önce, yasaların gerektirdiği durumlarda, video/görüntüdeki bireylerin (varsa) verilerinin Medya Hizmetleri ve Azure'da kullanımı, işlenmesi ve depolanması için gerekli tüm izinler de dahil olmak üzere, videoyu/görüntüyü kullanmak için gereken tüm haklara sahip olmalısınız. Bazı yargı bölgeleri, biyometrik veriler gibi belirli veri kategorilerinin toplanması, çevrimiçi işlenmesi ve depolanması için özel yasal gereklilikler uygulayabilir. Özel yasal gerekliliklere tabi tüm verilerin işlenmesi ve saklanması için Medya Hizmetleri ve Azure'u kullanmadan önce, sizin için geçerli olabilecek bu tür yasal gerekliliklere uygunluğu sağlamalısınız.

Medya Hizmetlerinde uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için lütfen Microsoft [Güven Merkezi'ni](https://www.microsoft.com/trust-center/?rtc=1)ziyaret edin. Microsoft'un gizlilik yükümlülükleri, verilerinizin nasıl silindigini içeren veri işleme ve saklama uygulamaları için lütfen Microsoft' un [Gizlilik Bildirimini,](https://privacy.microsoft.com/PrivacyStatement) [Çevrimiçi Hizmetler Koşulları'nı](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") ve [Veri İşleme Eki 'ni](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") inceleyin. Medya Hizmetlerini kullanarak, OST, DPA ve Gizlilik Bildirimi'ne bağlı olmayı kabul edeyim.
 
## <a name="prerequisites"></a>Ön koşullar

Azure Media Services’i kullanmaya başlamak için aşağıdakilerin bulunması gerekir:

* Bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com).
* Bir Azure Media Services hesabı. Daha fazla bilgi için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
* (İsteğe bağlı) Geliştirme ortamı ayarlayın. Geliştirme ortamınız için .NET veya REST API’yi seçin. Daha fazla bilgi için bkz. [Ortam Ayarlama](media-services-dotnet-how-to-use.md).

    Ayrıca, [AMS API’ye programlama aracılığıyla nasıl bağlanılacağını öğrenin](media-services-use-aad-auth-to-access-ams-api.md).
* Standart veya premium akış uç noktası başlatılmış durumda.  Daha fazla bilgi için [bkz: Akış uç noktalarını yönetme](media-services-portal-manage-streaming-endpoints.md)

## <a name="sdks-and-tools"></a>SDK’lar ve araçlar

Media Services çözümleri oluşturmak için şunları kullanabilirsiniz:

* [Media Services REST API'si](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Kullanılabilir istemci SDK'larından biri:
    * .NET için Azure Medya Hizmetleri SDK
    
        * [NuGet paketi](https://www.nuget.org/packages/windowsazure.mediaservices/)
        * [GitHub kaynak kodu](https://github.com/Azure/azure-sdk-for-media-services)
    * [Java için Azure SDK](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Node.js için Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Bu, Microsoft dışı bir Node.js SDK sürümüdür. Bir topluluğun gözetimi altındadır ve şu anda AMS API'lerinin %100’ünü kapsamamaktadır).
* Mevcut araçlar:
    * [Azure portalında](https://portal.azure.com/)
    * [Azure-Media-Services-Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Gezgini (AMSE), Windows için bir Winforms/C# uygulamasıdır)

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="code-samples"></a>Kod örnekleri

Birden fazla kod örneği için **Azure Kod Örnekleri** galerisine bakın: [Azure Media Services kod örnekleri](https://azure.microsoft.com/resources/samples/?service=media-services&sort=0).

## <a name="concepts"></a>Kavramlar

Azure Media Services kavramları hakkında bilgi edinmek için bkz. [Kavramlar](media-services-concepts.md).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Desteklenen senaryolar ve veri merkezleri arasında Media Services kullanılabilirliği

Ayrıntılı bilgi için bkz. [AMS senaryoları ve veri merkezleri arasında özelliklerin ve hizmetlerin kullanılabilirliği](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

Daha fazla bilgi için bkz. [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](scenarios-and-availability.md#availability) bölümüne bakın.

## <a name="support"></a>Destek

[Azure Desteği](https://azure.microsoft.com/support/options/), Media Services de dahil olmak üzere Azure için destek seçenekleri sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
