---
title: Yazma için Azure kaynağına geçiş 2
titleSuffix: Azure Cognitive Services
description: Azure yazma kaynak anahtarı 2 ' ye geçirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 06/17/2020
ms.author: diberry
ms.openlocfilehash: 5f0778436db7bd8c3a09e3ba346d8a9a6c4af454
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84983755"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Azure Kaynak yazma anahtarına geçiş

Language Understanding (LUSıS) yazma kimlik doğrulaması, bir e-posta hesabından Azure kaynağına değişti. Şu anda gerekli olmasa da, bir Azure kaynağına geçiş daha sonra zorlanabilir.


## <a name="what-is-migration"></a>Geçiş nedir?

Geçiş, bir e-posta hesabından Azure kaynağına yazma kimlik doğrulamasını değiştirme işlemidir. Hesabınız, geçiş işleminden sonra bir Azure aboneliğine ve bir Azure yazma kaynağına bağlanır. **Tüm LUYA kullanıcıları (sahipler veya ortak çalışanlar) sonunda geçiş yapması gerekecektir.** Geçişin, LUıN portalından gerçekleştirilmesi gerekiyor. LUSıS CLı gibi yazma anahtarlarını oluşturursanız, yine de LUI portalındaki geçiş işlemini gerçekleştirmeniz gerekecektir. Geçiş sonrasında uygulamalarınızda hala ortak yazarlara sahip olabilirsiniz, ancak bunlar uygulama düzeyi yerine Azure Kaynak düzeyine eklenecektir.

> [!Note]
> Geçişten önce, ortak yazarlar, LUSıS uygulama düzeyinde _ortak çalışanlar_ olarak bilinir. Geçişten sonra, _katkıda bulunan_ Azure rolü aynı işlevsellik için, ancak Azure Kaynak düzeyinde kullanılır.

## <a name="note-before-you-migrate"></a>Geçirmeden önce göz önüne alın

* Geçiş **tek yönlü** bir işlemdir. Geçiş yaptıktan sonra geri gidemezsiniz.
* Uygulama **sahibiyseniz** uygulamalar sizinle **otomatik olarak geçiş** yapılır.
* Sahip, geçirilecek uygulamaların bir alt kümesini seçemez ve işlem geri alınamaz.
* Uygulamalar, **sahibi geçirdikten**sonra **ortak çalışan taraflarından kaybolur** .
* Sahipleri, geçişe bildirmek üzere ortak kişilere e-posta göndermeleri istenir.
* Uygulama üzerinde **ortak çalışan** bir çalışmadıysanız uygulamalar sizinle **geçiş uygulamacaktır** .
* Bir sahibin ortak çalışanların geçirildiğini bilmesi için bir yol yoktur.
* **Geçiş** , otomatik olarak ortak bir şekilde toplanmaz ve Azure yazma kaynağına taşıyabilir veya eklemez. Uygulama sahibi, geçişten sonra bu adımı tamamlaması gereken bir uygulamadır. Bu adım [, Azure yazma kaynağı için izinler](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-collaborate)gerektirir.
* Azure kaynağına atandıktan sonra, **ortak çalışanların uygulamalara erişim uygulamalarına geçirilmesi gerekir**. Aksi takdirde, uygulamaları yazmak için erişimleri olmaz.
* Geçirilen bir Kullanıcı, uygulamanın ortak çalışan olarak eklenemez.
* **Başka bir kullanıcının sahip olduğu uygulamalara atanan tahmin anahtarlarınız**varsa, bu, hem sahip hem de ortak çalışan için **geçişi engeller** . Öneriler için aşağıya bakın.

> [!Note]
> Bir tahmin çalışma zamanı kaynağı oluşturmanız gerekiyorsa, bunu oluşturmak için [ayrı bir işlem](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) vardır.

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

* Geçerli bir Azure aboneliğiyle ilişkilendirilmesi gerekir. Kiracı yöneticinizden aboneliğinizi size eklemesini isteyin veya [buradan](https://azure.microsoft.com/free/)ücretsiz olarak kaydolabilirsiniz.
* LUSıS portalından veya Azure portal bir LUSıS Azure yazma kaynağı oluşturmanız gerekir. LUO portalından bir yazma kaynağı oluşturma, sonraki bölümde ele alınan geçiş akışının bir parçasıdır.
* **Uygulamalar üzerinde ortak çalışan**değilseniz, uygulamalar otomatik olarak geçirilmez. Bu uygulamaları dışarı aktararak veya [dışarı aktarma API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)'sini kullanarak **yedeklemeniz önerilir** . Geçişten sonra uygulamayı LUO 'ya geri aktarabilirsiniz. İçeri aktarma işlemi, sahip olduğunuz yeni bir uygulama KIMLIĞIYLE yeni bir uygulama oluşturur.
* **Uygulamanın sahibiyseniz**, uygulamalarınızı otomatik olarak geçirirken dışarı aktarmanız gerekmez. Her bir **uygulamanın ortak kayıt listesinin kaydedilmesi önerilir.** Bu listeye sahip e-posta şablonu, isteğe bağlı olarak geçiş işleminin bir parçası olarak sağlanır.


|Portal|Amaç|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Tahmin ve yazma kaynakları oluşturun.<br>* Kaynaklarda katkıda bulunanlar atayın.|
|[LUIS](https://www.luis.ai)|* Yeni yazma kaynaklarına geçirin.<br>* Geçiş akışında yeni yazma kaynağı oluşturun.<br>* **Manage-> Azure Resources** sayfasından tahmine ve yazma kaynaklarını uygulamalara atayın veya atamayı yapın. <br>* Uygulamaları bir yazma kaynağından diğerine taşıyın.  |

> [!Note]
> **Lusıs uygulamanızı yazma**, katman tarafından belirtilen ücretsizdir `F0` . [Fiyatlandırma katmanları hakkında daha fazla](luis-limits.md#key-limits)bilgi edinin.


## <a name="migration-steps"></a>Geçiş adımları

1. Üzerinde çalıştığınız LUU portalında, en üstteki araç çubuğundaki **Azure** simgesinden geçiş işlemini başlatabilirsiniz.

   > [!div class="mx-imgBorder"]
   > ![Geçiş simgesi](./media/migrate-authoring-key/migration-button.png)

2. Geçiş açılır penceresi geçişe devam etmenize veya daha sonra geçiş yapmanıza olanak sağlar. **Şimdi geçir**' i seçin.

   > [!div class="mx-imgBorder"]
   > ![Geçiş işlemindeki ilk açılan pencere, şimdi geçir ' i seçin.](./media/migrate-authoring-key/prompt-when-migrating-2.png)

3. İsteğe bağlı olarak, uygulamalarınızdan birinde ortak çalışanlar varsa, geçiş hakkında bilgi veren **bir e-posta göndermeniz** istenir. Bu, isteğe bağlı bir adımdır.

   Her ortak çalışan ve uygulama için varsayılan e-posta uygulaması, hafif biçimli bir e-posta ile açılır. Göndermeden önce e-postayı düzenleyebilirsiniz. E-posta şablonu, tam uygulama KIMLIĞI ve uygulama adını içerir.
   
   ```html
   Dear Sir/Madam,

   I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

   App Id: <app-ID-omitted>
   App name: Human Resources
      
   Thank you
   ```
   
   > [!Note]
   > Hesabınızı Azure 'a geçirdikten sonra uygulamalarınız artık ortak olmayan uygulamalar tarafından kullanılamaz.

4. İsteğe bağlı olarak, herhangi bir uygulamada ortak çalışan değilseniz, geçiş akışı sırasında bu seçeneği belirleyerek **uygulamaların bir kopyasını dışarı aktarmanız** istenir. Seçeneği belirledikten sonra, istediğiniz uygulamaları dışarı aktarmak için soldaki indir düğmesine tıkladığınızda aşağıdaki sayfayı görürsünüz. Bu uygulamaları geçirdikten sonra geri alabilirsiniz. bu uygulamalar sizinle otomatik olarak geçirilmeyecektir. Bu, isteğe bağlı bir adımdır.

   > [!div class="mx-imgBorder"]
   > ![Uygulamanızı dışarı aktarmak için istemde bulun.](./media/migrate-authoring-key/export-app-for-collabs-2.png)

5. Azure 'dan zaten bir tane oluşturduysanız, yeni bir LUYA yazma kaynağı oluşturmayı veya var olan bir yazma kaynağına geçiş yapabilirsiniz. Aşağıdan doğru düğmeyi seçerek istediğiniz seçeneği belirleyin.

   > [!div class="mx-imgBorder"]
   > ![Yazma kaynağı oluşturma](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

### <a name="create-new-authoring-resource-from-luis-to-migrate"></a>Geçiş için LUSıS 'den yeni yazma kaynağı oluşturma

Yeni bir yazma kaynağı oluşturmak istiyorsanız **Yeni bir yazma kaynağı oluştur** ' u seçin ve sonraki pencerede aşağıdaki bilgileri sağlayın.

> [!div class="mx-imgBorder"]
> ![Yazma kaynağı oluşturma](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

* **Kaynak adı** -seçtiğiniz özel bir ad, yazma ve tahmin uç noktası sorgularınız için URL 'nin bir parçası olarak kullanılır.
* **Abonelik adı** -kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
* **Kaynak grubu** -açılan listeden seçtiğiniz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.
* **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı. Bu, varsayılan olarak seçtiğiniz kiracıya ayarlanır. Baş harflerinizi içeren en sağdaki Avatar ' i seçerek kiracılar arasında geçiş yapabilirsiniz.

Yukarıdaki bilgileri girdikten sonra **bitti**' yi seçin.

Her bir abonelik için bölge başına 10 ücretsiz yazma kaynağınız olabileceğini unutmayın. Aboneliğiniz aynı bölgede 10 ' dan fazla yazma kaynağına sahipse yeni bir tane oluşturamazsınız.

* Yazma kaynağı oluşturulduğunda, başarı iletisi gösterilir. Açılır pencereyi kapatmak için **Kapat** ' ı seçin.

  > [!div class="mx-imgBorder"]
  > ![Yazma kaynağınız başarıyla oluşturuldu.](./media/migrate-authoring-key/migration-success-2.png)

### <a name="use-existing-authoring-resource-to-migrate"></a>Geçirmek için mevcut yazma kaynağını kullanın

Aboneliğiniz zaten bir LUSıS Authoring Azure kaynağı ile ilişkilendirilmişse veya Azure portal üzerinde oluşturduysanız ve yeni bir kaynak oluşturmak yerine buna geçiş yapmak istiyorsanız, **mevcut yazma kaynağını kullan** ' ı seçin ve sonraki pencerede aşağıdaki bilgileri sağlayın.

> [!div class="mx-imgBorder"]
>![Yazma kaynağı oluşturma](./media/migrate-authoring-key/choose-existing-authoring-resource-2.png)

* **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı. Bu, varsayılan olarak seçtiğiniz kiracıya ayarlanır.
* **Abonelik adı** -kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
* **Kaynak adı** -geçiş yapmak istediğiniz yazma kaynağını seçin.

> [!Note]
> Yazma kaynağınızı açılan listede göremiyorsanız, oturum açtığınız LUO portalı uyarınca **doğru konumda** oluşturulmuş olduğunuzdan emin olun. Ayrıca, oluşturduğunuz öğenin bir **tahmin kaynağı**değil gerçekten bir **yazma kaynağı** olduğundan emin olun.


* Yazma kaynak adınızı doğrulayıp **Şimdi geçir** düğmesine tıklayın.

 > [!div class="mx-imgBorder"]
 > ![Yazma kaynağı oluşturma](./media/migrate-authoring-key/choose-authoring-resource-and-migrate-2.png)

* Başarı iletisi gösterilir. Açılır pencereyi kapatmak için **Kapat** ' ı seçin.

 > [!div class="mx-imgBorder"]
 > ![Yazma kaynağınız başarıyla oluşturuldu.](./media/migrate-authoring-key/migration-success-2.png)

## <a name="using-apps-after-migration"></a>Geçişten sonra uygulamaları kullanma

* Geçiş işleminden sonra, sahibi olduğunuz tüm LUO uygulamalarınız, artık tek bir LUıN yazma kaynağına atanır.
* **Uygulamalarım** listesinde, yeni yazma kaynağına geçirilen uygulamalar gösterilir.
* Uygulamalarınıza erişmeden önce, yazarlarınızın bulunduğu uygulamaları görmek için abonelik ve LUSıS yazma kaynağını seçin.

 > [!div class="mx-imgBorder"]
 > ![Yazanların yazarındaki uygulamaları görmek için abonelik ve LUSıS yazma kaynağı ' nı seçin.](./media/create-app-in-portal-select-subscription-luis-resource.png)

* Uygulamalarınızı LUE portalında düzenlemeyle çalışmaya devam etmek için yazma kaynağının anahtarını bilmeniz gerekmez.
* Uygulamalarınızı programlama yoluyla düzenlemeyi planlıyorsanız, yazma anahtarı değerleri gerekir. Bu değerler, Lua portalındaki **Manage-> Azure kaynakları** sayfasında görüntülenir ve ayrıca kaynağın **anahtarlar** sayfasındaki Azure Portal de mevcuttur. Ayrıca, daha fazla yazma kaynağı oluşturabilir ve bunları aynı sayfadan atayabilirsiniz.

 > [!div class="mx-imgBorder"]
 > ![Yazma kaynağını yönetin.](./media/migrate-authoring-key/manage-authoring-resource-2.png)

## <a name="add-contributors-to-authoring-resources"></a>Yazma kaynaklarına katkıda bulunanlar ekleme

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Yazma kaynağınız üzerinde [katkıda bulunanlar eklemeyi](luis-how-to-collaborate.md) öğrenin. Katkıda bulunanlar, bu kaynaktaki tüm uygulamalara erişebilir.

Kaynak için **Access Control (IAM)** sayfasında _Azure Portal_yazma kaynağına katkıda bulunanlar ekleyebilirsiniz. Daha fazla bilgi için bkz. [katılımcı erişimi ekleme](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

> [!Note]
> LUSıS uygulamasının sahibi geçirilmiş ve Azure kaynağına katkıda bulunan çalışanı olarak eklediyseniz, ortak çalışan, aynı zamanda geçirilmediği sürece uygulamaya erişemez.

## <a name="luis-portal-migration-reminders"></a>LUSıS portalı geçiş anımsatıcıları

[LUA portalı](https://www.luis.ai) , geçiş işlemini sağlar.

Şu durumlarda geçiş yapmanız istenir:
* Yazma için e-posta kimlik doğrulama sisteminde uygulamalarınız var.
* Uygulama sahibisiniz.

Haftalık olarak, uygulamalarınızı geçirmeniz istenir. Bu pencereyi geçirmeden iptal edebilirsiniz. Bir sonraki zamanlanan dönemden önce geçiş yapmak istiyorsanız, HALU portalının üst araç çubuğundaki **Azure** simgesinden geçiş işlemini başlatabilirsiniz.

Pencereyi iptal ederek geçiş işlemini erteleyebilirsiniz. Geçiş yapılıncaya veya geçiş son tarihi geçirilmeden, düzenli aralıklarla geçiş yapmanız istenir. Geçiş işlemini üst gezinti çubuğunun kilit simgesinden başlatabilirsiniz.

## <a name="prediction-resources-blocking-migration"></a>Tahmin kaynakları geçişi engelliyor
Geçişiniz, tüm uygulama çalışma zamanını olumsuz etkiler. Geçiş yaparken, tüm ortak çalışanlar uygulamalarınızdan kaldırılır ve diğer uygulamalardan ortak çalışan olarak kaldırılır. Bu işlem, bir Işbirleyicisi 'nin atadığı anahtarların de kaldırılması anlamına gelir. Bu, üretim süreciyle uygulamanızı bozabilirler. Bu, el ile atanmış olan ortak çalışanları veya anahtarları kaldırana kadar geçişi engellemenin nedenidir.

### <a name="when-does-prediction-resources-block-migration"></a>Tahmin kaynakları geçişi engelliyor mu?
* Sahip olmadığınız uygulamalarda tahmin/çalışma zamanı kaynakları atadıysanız geçiş engellenir.
* Başka kullanıcılarınız sahip olduğunuz uygulamalara tahmin/çalışma zamanı kaynakları atarsanız geçiş engellenir.

### <a name="recommended-steps-to-do-if-you-are-the-owner-of-the-app"></a>Uygulamanın sahibiyseniz yapmanız önerilen adımlar
Bazı uygulamaların sahibiyseniz ve bu uygulamaya tahmine/çalışma zamanı anahtarı atanmışsa, bu listeye geçiş yaptığınızda bir hata gösterilir. Bu,, başka kullanıcılara ait olan tahmin anahtarları atanmış uygulama kimliklerini listeler.

Şunları yapmanız önerilir:
* Geçiş hakkındaki ortak çalışanları bilgilendirin.
* Hatada gösterilen uygulamalardan tüm ortak çalışanları kaldırın.
* Ortak çalışanları el ile kaldırdığınızda, geçiş işleminin başarılı olması gerekir.
* Yeni yazma kaynağınız için ortak çalışanlar katkıda bulunan olarak atayın.
* Ortak çalışanlar, tahmine yönelik kaynakları geçirmek ve uygulamalara yeniden atamak için kullanılır.
Bu, tahmin kaynakları yeniden atanıncaya kadar uygulamada geçici olarak bir kesintiye neden olur.

Burada başka bir çözüm, sahip geçişten önce, çalışanların Azure portal Azure aboneliklerinde katkıda bulunanlar olarak uygulama sahipleri ekleyebilirler. Bu, çalışma zamanı tahmin kaynağına sahip erişimine izin verir. Sahip, eklenen yeni aboneliği kullanarak (yeni bir kiracı altında bulunur), bu, hem ortak çalışan hem de uygulama sahibi için geçiş işleminin engellemesini ortadan kaldırmaya devam eder, ancak tahmin anahtarı olan uygulamaların kesintisiz bir şekilde geçiş yapmasına olanak tanır.


### <a name="recommended-steps-to-do-if-you-are-a-collaborator-on-an-app"></a>Uygulama üzerinde ortak çalışan olmanız durumunda yapmanız önerilen adımlar
Uygulamalar üzerinde işbirliği yapıyorsanız ve bu uygulamaya tahmin/çalışma zamanı anahtarı atadıktan sonra, geçişi engelleyen uygulama kimliklerini ve anahtar yollarını listeleyen bir hata gösterilir.

Şunları yapmanız önerilir:
* Uygulamaları yedekleme olarak dışarı aktarın. Bu, geçiş işleminde isteğe bağlı bir adım olarak sağlanır.
* **Yönetim > Azure kaynakları** sayfasından tahmin kaynaklarını atamayı kaldırın.
* Geçiş işlemini daha az yapın.
* Geçişten sonra uygulamaları geri alın.
* Uygulama **yönetme-> Azure kaynakları** sayfasına tahmin anahtarlarını yeniden atayın.

> [!Note]
> Taşıdıktan sonra uygulamalarınızı geri aktardığınızda, farklı uygulama kimliklerine sahip olur ve üretimde vurdıklardan farklı olacaktır. Artık bu uygulamaların sahibi olursunuz.

## <a name="troubleshooting-migration-process"></a>Geçiş işlemi sorunlarını giderme

Geçiş yapmayı denediğinizde, ancak Azure aboneliğinizi açılan listede bulamadığınızda:
* Bilişsel hizmetler kaynakları oluşturma yetkisine sahip geçerli bir Azure aboneliğiniz olduğundan emin olun. [Azure Portal](https://ms.portal.azure.com) gidin ve aboneliğin durumunu denetleyin. Bir tane yoksa [ücretsiz deneme sürümü oluşturun](https://azure.microsoft.com/free/).
* Geçerli aboneliğinizle ilişkili doğru kiracıda olduğunuzdan emin olun. Kiracıların aşağıdaki araç çubuğundaki diğer sol avatar öğesinden geçiş yapabilirsiniz: ![ kiracı değiştirin.](./media/migrate-authoring-key/switch-user-tenant-2.png)

 Zaten var olan bir yazma kaynağınız varsa ancak "var olan yazarlık kaynağını kullan" seçeneği belirlendiğinde bu kaynağı bulamadıysanız:
* Kaynak muhtemelen oturum açmış olduğunuz portaldan farklı bir konumda oluşturulmuştur. Lütfen [Lusıs yazma bölgelerini ve portallarını](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-authoring-regions) denetleyin
* Bunun yerine LUO portalından yeni bir kaynak oluşturun

"Yeni yazma kaynağı oluştur" seçeneğini belirlerseniz geçiş işlemi "kullanıcının Azure bilgileri alınamadı, daha sonra yeniden deneyin" hata iletisiyle başarısız oldu
* Aboneliğiniz abonelik başına bölge başına 10 veya daha fazla yazma kaynağına sahip olabilir. Bu durumda, yeni bir yazma kaynağı oluşturabileceksiniz.
* "Mevcut yazma kaynağını kullan" seçeneğini belirleyerek geçirin ve aboneliğiniz altında sahip olduğunuz mevcut kaynaklardan birini seçin.

Aşağıdaki hatayı görürseniz, [uygulamanın sahibiyseniz yapmanız önerilen adımlar] bölümüne bakın. ![ sahipler için geçiş başarısız olur](./media/migrate-authoring-key/migration-failed-for-owner-2.png)

Aşağıdaki hatayı görürseniz, [bir uygulama üzerinde ortak çalışan olmanız durumunda yapmanız gereken adımları yapın] ![ ortak çalışanlar için geçiş başarısız olur](./media/migrate-authoring-key/migration-failed-for-collab-2.png)


## <a name="next-steps"></a>Sonraki adımlar

* Yazma ve çalışma zamanı anahtarları hakkındaki [kavramları](luis-concept-keys.md) gözden geçirin
* [Anahtar atamayı](luis-how-to-azure-subscription.md) ve [katkıda bulunanlar](luis-how-to-collaborate.md) eklemeyi inceleyin
