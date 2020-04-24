---
title: Yazma için Azure kaynağına geçiş
titleSuffix: Azure Cognitive Services
description: Azure yazma kaynak anahtarına geçiş yapın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: 679073715588a4a81e69e3e7ba2d18341b1bab4b
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82096631"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Azure Kaynak yazma anahtarına geçiş

Language Understanding (LUSıS) yazma kimlik doğrulaması, bir e-posta hesabından Azure kaynağına değişti. Şu anda gerekli olmasa da, bir Azure kaynağına geçiş daha sonra zorlanabilir.

## <a name="why-migrate"></a>Neden geçirilir?

Yazma için bir Azure kaynağı kullanmak, yazma erişimini denetlemek için kaynağın sahibi olarak size izin verir. Farklı yazar gruplarını yönetmek için yazma kaynakları oluşturabilir ve ad verebilirsiniz.

Örneğin, 2 LUO uygulama sahibisiniz ve her bir uygulamada ortak çalışan farklı üyelere sahipsiniz. İki farklı yazma kaynağı oluşturabilir ve her uygulamayı ayrı bir kaynağa atayabilirsiniz. Ardından, her üyeyi, birlikte çalıştıkları uygulamaya bağlı olarak uygun yazma kaynağına katkıda bulunan olarak atayın. Azure Authoring Resource, yetkilendirmeyi denetler.

> [!Note]
> Geçişten önce, ortak yazarlar, LUSıS uygulama düzeyinde _ortak çalışanlar_ olarak bilinir. Geçişten sonra, _katkıda bulunan_ Azure rolü aynı işlevsellik için, ancak Azure Kaynak düzeyinde kullanılır.

## <a name="what-is-migrating"></a>Ne geçiriliyor?

Geçişe şunlar dahildir:

* Tüm LULAR, sahipler ve katkıda bulunanlar kullanıcıları.
* **Tüm** uygulamalar.
* **Tek yönlü** geçiş.

Sahip, geçirilecek uygulamaların bir alt kümesini seçemez ve işlem geri alınamaz.

Geçiş şu değildir:

* Ortak çalışanları toplayan ve otomatik olarak taşınan veya Azure yazma kaynağına ekleyen bir işlem. Uygulamanın sahibi olarak bu adımı gerçekleştirmeniz gerekir. Bu adım uygun kaynak için izinleri gerektirir.
* Tahmin çalışma zamanı kaynağı oluşturma ve atama işlemi. Bir tahmin çalışma zamanı kaynağına ihtiyacınız varsa, bu [ayrı bir işlemdir](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ve değiştirilmez.

## <a name="how-are-the-apps-migrating"></a>Uygulamalar nasıl taşınıyor?

[LUA portalı](https://www.luis.ai) , geçiş işlemini sağlar.

Şu durumlarda geçiş yapmanız istenir:

* Yazma için e-posta kimlik doğrulama sisteminde uygulamalarınız var.
* Uygulama sahibisiniz.

Pencereyi iptal ederek geçiş işlemini erteleyebilirsiniz. Geçiş yapılıncaya veya geçiş son tarihi geçirilmeden, düzenli aralıklarla geçiş yapmanız istenir. Geçiş işlemini üst gezinti çubuğunun kilit simgesinden başlatabilirsiniz.

## <a name="migration-for-the-app-owner"></a>Uygulama sahibi için geçiş

### <a name="before-you-migrate"></a>Geçirmeden önce

* **Gerekli**, bir [Azure aboneliğinizin](https://azure.microsoft.com/free/)olması gerekir. Abonelik işleminin bir parçası faturalandırma bilgileri gerektirir. Ancak, LUSıS kullandığınızda ücretsiz (`F0`) fiyatlandırma katmanını kullanabilirsiniz.
* **Isteğe bağlı**olarak, her uygulamayı dışarı aktararak ve dışarı aktarma [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)'sini kullanarak, bu uygulamaları lusıs portalının uygulamalar listesinden yedekleyin.
* **Isteğe bağlı**olarak, her bir uygulamanın ortak kayıt listesini kaydedin. Bu e-posta listesi, geçiş işleminin bir parçası olarak sağlanır.


**Lusıs uygulamanızı yazma**, `F0` katman tarafından belirtilen ücretsizdir. [Fiyatlandırma katmanları hakkında daha fazla](luis-limits.md#key-limits)bilgi edinin.

Azure aboneliğiniz yoksa [kaydolun](https://azure.microsoft.com/free/).

### <a name="migration-steps"></a>Geçiş adımları

[Bu geçiş adımlarını](luis-migration-authoring-steps.md)izleyin.

### <a name="after-you-migrate"></a>Geçiş yaptıktan sonra

Geçiş işleminden sonra, tüm LUO uygulamalarınız tek bir LUıN yazma kaynağına atanır.

_LUA portalında_daha fazla yazma kaynağı oluşturabilir ve **> Azure kaynaklarını yönet** sayfasından atayabilirsiniz.

Kaynak için **Access Control (IAM)** sayfasında _Azure Portal_yazma kaynağına katkıda bulunanlar ekleyebilirsiniz. Daha fazla bilgi için bkz. [katılımcı erişimi ekleme](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

|Portal|Amaç|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Tahmin ve yazma kaynakları oluşturun.<br>* Katkıda bulunanlar ata.|
|[LUIS](https://www.luis.ai)|* Yeni yazma kaynaklarına geçirin.<br>* **Manage-> Azure Resources** sayfasından tahmine ve yazma kaynaklarını uygulamalara atayın veya atamayı yapın.|

## <a name="migration-for-the-app-contributor"></a>Uygulama katılımcısı için geçiş

Her LUıN kullanıcısının, ortak çalışanlar/katkıda bulunanlar dahil olmak üzere geçirilmesi gerekir. Bir ortak çalışan, uygulamaya erişim sağlamak için geçirilmesi gerekir.

> [!Note]
> LUSıS uygulamasının sahibi geçirilmiş ve Azure kaynağına katkıda bulunan çalışanı olarak eklediyseniz, ortak çalışan, aynı zamanda geçirilmediği sürece uygulamaya erişemez.

### <a name="before-the-app-is-migrated"></a>Uygulama geçirilmeden önce

Ortak çalışan bir uygulamayı dışarı aktarmayı tercih edebilir ve ardından uygulamayı LUSıS 'e geri aktarabilirsiniz. İçeri aktarma işlemi, sahip olduğunuz yeni bir uygulama KIMLIĞIYLE yeni bir uygulama oluşturur.

### <a name="after-the-app-is-migrated"></a>Uygulama geçirildikten sonra

Uygulama sahibinin [e-postanızı Azure yazma kaynağına ortak çalışan olarak eklemesi](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource)gerekir.

Geçiş işleminden sonra, sahip olduğunuz tüm uygulamalar, LUı portalının **uygulamalarım** sayfasında bulunur.

## <a name="troubleshooting-the-migration-process-for-luis-authoring"></a>LUSıS yazma işlemi için geçiş işleminin sorunlarını giderme

* LUSıS yazma anahtarları yalnızca, geçiş işlemi tamamlandıktan sonra LUO portalında görünür. LUSıS CLı gibi yazma anahtarlarını oluşturursanız, kullanıcının hala LUSıS portalındaki geçiş işlemini tamamlaması gerekir.
* Geçirilmiş bir Kullanıcı Azure kaynaklarına katkıda bulunan bir kullanıcı olarak eklediyse, geçirilmeyen Kullanıcı, geçirilmedikleri sürece uygulamalara erişemez.
* Geçirilmemiş bir Kullanıcı herhangi bir uygulama için sahip değilse ancak başkalarına ait olan diğer uygulamalar için ortak çalışan ise ve sahipler geçiş işlemini gerçekleştirdi, bu kullanıcının uygulamalara erişimi olması için geçirilmesi gerekir.
* Geçirilmemiş bir Kullanıcı başka bir geçirilmiş kullanıcıyı uygulamasına ortak çalışan olarak eklediyse, geçirilen bir kullanıcıyı bir uygulamaya ortak çalışan olarak ekleyemeyeceksiniz, bir hata meydana gelir. Geçişi yapılan kullanıcının geçiş işlemini geçmesi ve bir Azure kaynağı oluşturması ve geçirilmiş kullanıcıyı bu kaynağa katkıda bulunan olarak eklemesi gerekir.

Şu durumlarda geçiş işlemi sırasında bir hata alırsınız:
* Aboneliğiniz bilişsel hizmetler kaynakları oluşturma konusunda sizi yetkilendiremez
* Geçişiniz, tüm uygulama çalışma zamanını olumsuz etkiler. Geçiş yaparken, tüm ortak çalışanlar uygulamalarınızdan kaldırılır ve diğer uygulamalardan ortak çalışan olarak kaldırılır. Bu işlem, atadığınız anahtarların çok fazla kaldırıldığı anlamına gelir. Başka uygulamalarda anahtarlar atadıysanız geçiş engellenir. Geçirmeden önce güvenli olarak atadığınız anahtarı kaldırın. Atadığınız anahtarın çalışma zamanında kullanılmadığını biliyorsanız, geçiş sırasında ilerlemeniz için onu kaldırmanız gerekir.

Aşağıdaki URL biçimini kullanarak uygulamanızın Azure Kaynak listesine erişin:

`https://www.luis.ai/applications/REPLACE-WITH-YOUR-APP-ID/versions/REPLACE-WITH-YOUR-VERSION-ID/manage/resources`

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanızı bir yazma kaynağına geçirme](luis-migration-authoring-steps.md)
