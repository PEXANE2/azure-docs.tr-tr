---
title: Soru-cevap Oluşturucu bir soru-cevap Oluşturucu hizmeti - Kurulum
titleSuffix: Azure Cognitive Services
description: Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: f30b55eda4a02cfb3e961c0019128e4fe686cf53
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967661"
---
# <a name="create-a-qna-maker-service"></a>Soru-cevap Oluşturucu hizmeti oluşturma

Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

Bu yordam birkaç Azure kaynağı dağıtır. Birlikte, bu kaynaklar, Bilgi Bankası içeriği yönetmek ve bir uç nokta ancak soru-yanıt özellikleri sağlar.

1. Azure portal oturum açın ve [bir soru-cevap oluşturma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Hüküm ve koşulları okuduktan sonra **Oluştur** ' u seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. İçinde **soru-cevap Oluşturucu**, uygun katmanları ve bölgeleri seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti - fiyatlandırma katmanı ve bölgeleri oluşturma](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dolgu **adı** Bu soru-cevap Oluşturucu hizmetini tanımlamak için benzersiz bir ada sahip. Bu ad Ayrıca, bilgi bankalarından ilişkili edileceği soru-cevap Oluşturucu uç nokta tanımlar.
    * Seçin **abonelik** soru-cevap Oluşturucu kaynağın dağıtılacağı.
    * Soru-Cevap Oluşturma Management Services (portal ve yönetim API 'Leri) için **fiyatlandırma katmanını** seçin. Bkz: [burada](https://aka.ms/qnamaker-pricing) SKU'ları fiyatlandırması hakkında ayrıntılı bilgi için.
    * Yeni bir **kaynak grubu** (önerilir) veya bu soru-cevap Oluşturucu kaynak dağıtacağınız var olanı kullanın. Soru-Cevap Oluşturma birkaç Azure kaynağı oluşturur; Bu kaynakları barındıracak bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * **Kaynak grubu konumu**seçin.
    * Seçin **arama fiyatlandırma katmanı** Azure Search hizmeti. Gri ücretsiz katmanı seçeneğini görürseniz, aboneliğinizde bir ücretsiz Azure arama katmanı zaten sahip olduğunuz anlamına gelir. Bu durumda Azure arama temel katman ile başlatmanız gerekir. Azure arama fiyatlandırma ayrıntılarına [burada](https://azure.microsoft.com/pricing/details/search/).
    * Seçin **arama konumu** dağıtılacak Azure Search veri istediğiniz. Müşteri verilerinin nerede depolanacağını gerekir, kısıtlamaları, Azure arama için seçtiğiniz konumu bilgilendirecektir.
    * App service içinde bir ad verin **uygulama adı**.
    * Varsayılan olarak App service için standart (S1) katman varsayılan olarak. Plan oluşturulduktan sonra değiştirebilirsiniz. App service fiyatlandırması hakkında daha fazla ayrıntı görmek [burada](https://azure.microsoft.com/pricing/details/app-service/).
    * Seçin **Web sitesi konumu** App Service dağıtılacağı.

        > [!NOTE]
        > Arama konumu Web sitesi konumundan farklı olabilir.

    * Etkinleştirmek isteyip istemediğinizi seçin **Application Insights** veya yok. Varsa **Application Insights** olan etkin, soru-cevap Oluşturucu telemetri trafik, sohbet günlükleri ve hataları toplar.
    * Seçin **uygulama öngörülerinin konumu** Application Insights kaynağı dağıtılacağı.
    * Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) . 

1. Tüm alanlar doğrulandıktan sonra, aboneliğinizde bu hizmetlerin dağıtımına başlamak için **Oluştur** ' u seçebilirsiniz. Tamamlanması birkaç dakika sürer.

1. Dağıtım tamamlandığında, aboneliğinizde oluşturduğunuz aşağıdaki kaynakları görürsünüz.

    ![Yeni bir soru-cevap Oluşturucu hizmeti kaynağı oluşturuldu](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="region-of-management-service"></a>Yönetim hizmeti bölgesi

Yalnızca ilk veri işleme için Portal & kullanılan Soru-Cevap Oluşturma yönetim hizmeti yalnızca Batı ABD kullanılabilir. Bu Batı ABD hizmetinde hiçbir müşteri verisi depolanmaz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md)
