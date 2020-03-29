---
title: Yazma için Azure kaynağına geçiş
titleSuffix: Azure Cognitive Services
description: Azure yazma kaynak anahtarına geçirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: ec6f9592a4c149be382fab66cca27d929644d988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194518"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Azure kaynak yazma anahtarına geçiş

Dil Anlama (LUIS) kimlik doğrulaması yazma bir e-posta hesabından Azure kaynağına değiştirildi. Şu anda gerekli olmamakla birlikte, gelecekte bir Azure kaynağına geçiş zorunlu olacaktır.

## <a name="why-migrate"></a>Neden göç?

Yazma için bir Azure kaynağı kullanmak, kaynağın sahibi olarak, yazara erişimi denetlemenize olanak tanır. Farklı yazar gruplarını yönetmek için yazarkaynakları oluşturabilir ve adlandırabilirsiniz.

Örneğin, 2 LUIS uygulamasının sahibisiniz ve her uygulamada ortak çalışan farklı üyelere sahipsiniz. İki farklı yazma kaynağı oluşturabilir ve her uygulamayı her ayrı kaynağa atayabilirsiniz. Ardından, işbirliği yaptıkları uygulamaya bağlı olarak her üyeyi uygun yazma kaynağına katkıda bulunan kişi olarak atayın. Azure yazma kaynağı yetkilendirmeyi denetler.

> [!Note]
> Geçişten önce, ortak yazarlar LUIS uygulama düzeyinde _ortak çalışanlar_ olarak bilinir. Geçişten _sonra, katkıda bulunan ın_ Azure rolü aynı işlevsellik için değil, Azure kaynak düzeyinde kullanılır.

## <a name="what-is-migrating"></a>Göç nedir?

Geçiş şunları içerir:

* LUIS'in tüm kullanıcıları, sahipleri ve katkıda bulunanlar.
* **Tüm** uygulamalar.
* **Tek yönlü** bir geçiş.

Sahibi, geçiş yapmak için bir uygulama alt kümesi seçemez ve işlem geri döndürülemez.

Geçiş şu şekilde değildir:

* Ortak çalışanları toplayan ve Azure yazarlık kaynağına otomatik olarak taşınan veya ekleyen bir işlem. Uygulama sahibi olarak bu adımı tamamlamanız gerekir. Bu adım, ilgili kaynağa izinler gerektirir.
* Tahmin çalışma zamanı kaynağı oluşturmak ve atamak için bir işlem. Bir tahmin çalışma zamanı kaynağıgerekiyorsa, bu [ayrı bir işlemdir](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ve değişmez.

## <a name="how-are-the-apps-migrating"></a>Uygulamalar nasıl geçiş yapıyor?

[LUIS portalı](https://www.luis.ai) geçiş işlemini sağlar.

Şu larsa, geçiş için istenir:

* Yazma için e-posta kimlik doğrulama sisteminde uygulamalarınız vardır.
* Ve uygulama sahibi sizsiniz.

Pencereden çıkanları iptal ederek geçiş işlemini geciktirebilirsiniz. Geçiş yapılana veya geçiş tarihi geçene kadar düzenli olarak geçiş yapmak istenir. Geçiş işlemini üst gezinti çubuğunun kilit simgesinden başlatabilirsiniz.

## <a name="migration-for-the-app-owner"></a>Uygulama sahibi için geçiş

### <a name="before-you-migrate"></a>Göç etmeden önce

* **Gerekli**, bir [Azure aboneliği](https://azure.microsoft.com/free/)olması gerekir. Abonelik işleminin bir bölümü fatura bilgilerini gerektirir. Ancak, LUIS'i kullandığınızda Ücretsiz (`F0`) fiyatlandırma katmanını kullanabilirsiniz.
* **İsteğe bağlı olarak,** her uygulamayı dışa aktararak veya dışa aktarma [API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)kullanarak luis portalının uygulamalar listesindeki uygulamaları yedekleyin.
* **İsteğe bağlı olarak,** her uygulamanın ortak çalışan listesini kaydedin. Bu e-posta listesi geçiş sürecinin bir parçası olarak sağlanır.


**LUIS uygulamanızı yazmanız ücretsizdir**, `F0` katman tarafından gösterilir. [Fiyatlandırma katmanları hakkında daha fazla](luis-boundaries.md#key-limits)bilgi edinin.

Azure aboneliğiniz yoksa [kaydolun.](https://azure.microsoft.com/free/)

### <a name="migration-steps"></a>Geçiş adımları

[Bu geçiş adımlarını](luis-migration-authoring-steps.md)izleyin.

### <a name="after-you-migrate"></a>Göç ettikten sonra

Geçiş işleminden sonra, tüm LUIS uygulamalarınız artık tek bir LUIS yazma kaynağına atanır.

Daha fazla yazma kaynağı oluşturabilir ve _LUIS portalındaki_ **Azure kaynaklarını yönet >** sayfasından atayabilirsiniz.

Bu kaynak için **Erişim Denetimi (IAM)** sayfasındaki _Azure portalından_yazarkaynağa katkıda bulunanlar ekleyebilirsiniz. Daha fazla bilgi için [bkz.](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource)

|Portal|Amaç|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Tahmin ve yazma kaynakları oluşturun.<br>* Katkıda bulunanları atayın.|
|[LUIS](https://www.luis.ai)|* Yeni yazma kaynaklarına geçirin.<br>* **Yönet -> Azure kaynakları** sayfasından uygulamalara tahmin ve yazarlık kaynakları atama veya atama.|

## <a name="migration-for-the-app-contributor"></a>Uygulama katılımcısı için geçiş

LUIS'in her kullanıcısının, ortak çalışanlar/katkıda bulunanlar da dahil olmak üzere geçiş yapması gerekir. Bir ortak çalışanın uygulamaya erişimi olması için geçiş yapması gerekir.

> [!Note]
> LUIS uygulamasının sahibi taşınır ve ortak çalışanı Azure kaynağına katkıda bulunan kişi olarak eklediyse, ortak çalışan ın da geçiş yapmadıkça uygulamaya erişimi olmaz.

### <a name="before-the-app-is-migrated"></a>Uygulama geçirilmeden önce

İş birlikçi olduğunuz bir uygulamayı dışa aktarmayı seçebilir, ardından uygulamayı LUIS'e geri aktarabilirsiniz. Alma işlemi, sahibi olduğunuz yeni bir uygulama kimliğiyle yeni bir uygulama oluşturur.

### <a name="after-the-app-is-migrated"></a>Uygulama geçirildikten sonra

Uygulama [sahibinin e-postanızı ortak çalışan olarak Azure yazarlık kaynağına eklemesi](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource)gerekir.

Geçiş işleminden sonra, sahip olduğunuz tüm uygulamalar LUIS portalının **Uygulamalarım** sayfasında mevcuttur.

## <a name="troubleshooting-the-migration-process-for-luis-authoring"></a>LUIS yazma işlemi için geçiş işleminde sorun giderme

* LUIS yazma anahtarları yalnızca geçiş işlemi tamamlandıktan sonra LUIS portalında görünür. LUIS CLI gibi yazarlık anahtarlarını oluşturursanız, kullanıcının yine de LUIS portalındaki geçiş işlemini tamamlaması gerekir.
* Geçirilen bir kullanıcı azure kaynağına katkıda bulunan olarak geçirilemeyen bir kullanıcı yıklarsa, geçirilemeyen kullanıcı, geçiş yapmadıkça uygulamalara erişemaz.
* Geçirilmeyen bir kullanıcı herhangi bir uygulamanın sahibi değilse, ancak başkaları tarafından sahip olunan diğer uygulamaların işbirlikçisiyse ve sahipleri geçiş işleminden geçtiyse, bu kullanıcının uygulamalara erişmek için geçiş yapmasına gerek vardır.
* Geçirilemeyen bir kullanıcı uygulamasına ortak çalışan olarak başka bir geçirilen kullanıcı eklediyse, bir uygulamaya ortak çalışan olarak geçirilen bir kullanıcıyı ekleyemeyeceksiniz diye bir hata oluşur. Geçirilmeyen kullanıcının geçiş işleminden geçmesi ve azure bir kaynak oluşturması ve geçirilen kullanıcıyı bu kaynağa katkıda bulunan olarak eklemesi gerekir.

Aşağıdakileri varsa geçiş işlemi sırasında bir hata alırsınız:
* Aboneliğiniz Bilişsel Hizmetler kaynakları oluşturmanız için size yetki vermez
* Geçişiniz, çalışma zamanı tüm uygulamaları olumsuz etkiler. Geçiş yaparken, tüm ortak çalışanlar uygulamalarınızdan kaldırılır ve diğer uygulamalardan ortak çalışan olarak kaldırılırsınız. Bu işlem, atanan anahtarların da kaldırılamayacağı anlamına gelir. Diğer uygulamalarda anahtarlar atadıysanız geçiş engellenir. Geçirmeden önce güvenli bir şekilde atadığınız anahtarı kaldırın. Atadığınız anahtarın çalışma zamanında kullanılmadığını biliyorsanız, geçişte ilerleme kaydedebilmek için anahtarı kaldırmanız gerekir.

Aşağıdaki URL biçimini kullanarak uygulamanızın Azure kaynak listesine erişin:

`https://www.luis.ai/applications/REPLACE-WITH-YOUR-APP-ID/versions/REPLACE-WITH-YOUR-VERSION-ID/manage/resources`

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanızı bir yazar kaynağa geçirme](luis-migration-authoring-steps.md)
