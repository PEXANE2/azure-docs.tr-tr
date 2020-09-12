---
title: Ticari Market teklifleri için planlar ve fiyatlandırma
description: Iş Ortağı Merkezi 'nde Microsoft ticari Market teklifleri planları hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: mingshen
ms.author: mingshen
ms.date: 09/02/2020
ms.openlocfilehash: ef860d57556e17d432988fd0fd43f861a31fde6c
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89381144"
---
# <a name="plans-and-pricing-for-commercial-marketplace-offers"></a>Ticari Market teklifleri için planlar ve fiyatlandırma

Bir plan, bir teklifin kapsamını ve sınırlarını ve uygunsa ilişkili fiyatlandırmayı tanımlar. Örneğin, teklif türüne bağlı olarak bölgesel pazarlar ' ı seçebilir ve bir planın herkese açık mı yoksa özel bir hedef kitleye mı görünemediğini seçebilirsiniz. Bazı teklif türleri yinelenen abonelikleri, bazıları ise kullanım tabanlı fiyatlandırmayı destekler ve bir müşterinin teklifi doğrudan yayımcıdan satın almış oldukları bir lisansla satın almasına izin verir. Bu, müşterilerinize uygun olduğunda farklı teknik ve fiyatlandırma seçenekleri sağlamak için esneklik sağlar.

Planları destekleyen her teklif için en fazla 100 plan oluşturabilirsiniz ve bu planların en fazla 45 ' e kadar [özel](#plan-visibility)olabilir. Ücretli planları yalnızca Azure sanal makine teklifleri, Azure Uygulama teklifleri (yönetilen uygulamalar) ve hizmet olarak yazılım (SaaS) teklifleri için oluşturabilirsiniz. Bu tekliflerden herhangi birini Microsoft (transactable teklifleri) ile satmayı seçtiğinizde, en az bir plan oluşturmanız gerekir. Diğer teklif türleri için planlar oluşturabilirsiniz, ancak bu teklif türleri için planlar fiyatlandırma seçeneklerini içermez.

> [!TIP]
> Transactable teklifi, Microsoft 'un yayımcı adına yazılım lisansı için para değişimini kolaylaştıran bir tekliftir.

## <a name="plans-by-offer-type"></a>Teklif türüne göre planlar

Aşağıdaki tabloda her teklif türü için plan seçenekleri gösterilmektedir. Aşağıdaki bölümler bu seçenekler hakkında daha fazla bilgi açıklamaktadır. 

| Teklif türü | Fiyatlandırma seçenekleriyle planlar | Fiyatlandırma seçenekleri olmayan planlar | Özel hedef kitle seçeneği |
| ------------ | ------------- | ------------- | ------------- |
| Azure yönetilen uygulaması | &#10004; | | &#10004; |
| Azure çözüm şablonu | | &#10004; | &#10004; |
| Azure kapsayıcısı | | &#10004; (KLG) | |
| IoT Edge modülü |  | &#10004; |  |
| Yönetilen hizmet |  | &#10004; (KLG) | &#10004; |
| Hizmet olarak yazılım | &#10004; |  | &#10004; |
| Azure sanal makine | &#10004; |  | &#10004; |
|||||

Planlar aşağıdaki teklif türleri için desteklenmez:
-  Danışmanlık hizmeti
-  Dynamics 365 Business Central
-  Dynamics 365 müşteri katılımı & PowerApps
-  Dynamics 365 for Operations
-  Power BI uygulaması

## <a name="plan-information"></a>Plan bilgileri

Yeni bir plan oluşturduğunuzda her teklif türü farklı bilgiler gerektirir. Teklif [türüne göre yayımlama kılavuzunda](publisher-guide-by-offer-type.md), sunmaya özgü makalelerin bağlantılarını bulabilirsiniz. **Plana genel bakış** sayfasında yeni bir plan oluşturduktan sonra, planınız için farklı Ayrıntılar yapılandırmak üzere **plan listeleme** veya **fiyatlandırma ve kullanılabilirlik** gibi farklı sekmeler görürsünüz. Bu alanlar üzerinde çalışırken her sekme, tamamlanmamış veya tamamlanmış durumunu gösterir.

![Iş Ortağı Merkezi 'nde plan listesi sayfasını gösterir. Plan adı, plan Özeti ve plan açıklaması alanları vurgulanır.](./media/commercial-marketplace-plans/plan-listing-tab.png)

Yeni bir planda tamamlanacak birkaç ortak ayrıntı vardır:

-  **Plan kimliği**: Bu teklifte her plan için BENZERSIZ bir kimlik oluşturun. En fazla 50 karakter kullanın: yalnızca küçük harf, alfasayısal karakter, çizgi ve alt çizgi. Bu KIMLIK, ürün URL 'sindeki ve Azure Resource Manager şablonlarındaki (varsa) müşterilere görünür olacaktır. Teklifi yayımladıktan sonra bu KIMLIĞI değiştiremezsiniz.
-  **Plan adı**: (yukarıdaki görüntüde belirtme çizgisi 1).) Bu teklifte her plan için benzersiz bir ad oluşturun. En fazla 50 karakter kullanın. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (örneğin, teklif adı: Standart plan ve kurumsal plan). Tekliflerinizi hangi plana seçeceğinize karar verirken, müşteriler bu adı görür.
-  **Plan Özeti**: (yukarıdaki görüntüde belirtme çizgisi 2.) Bu Özet, Azure Marketi arama sonuçlarında görünür ve en fazla 100 karakter içerebilir.
   > [!NOTE]
   > Bu alan SaaS teklifleri için geçerlidir.
-  **Plan açıklaması**: (yukarıdaki görüntüde belirtme çizgisi 3.) Bu planı teklifiniz için diğer planlardan benzersiz hale getiren bir plan açıklaması ekleyin. En fazla 500 karakter kullanın. Bu içerik, müşterilerinize göz atarak teklif listesi sayfasında ve teklifinize yönelik bir plan seçtiğinizde görünür.

Plan adı ve açıklaması, ticari Market çevrimiçi mağazasındaki teklif listesi sayfasında görüntülenir. Aşağıdaki ekran görüntüsünde, Azure Marketi 'nde SaaS teklifi listesinin üç planı gösterilmektedir.

![Iş Ortağı Merkezi 'nde bir teklif listesi sayfasını gösterir. Üç plan gösterilmektedir.](./media/commercial-marketplace-plans/offer-listing-page.png)

Planlarınızı oluşturduktan sonra **plana genel bakış** sayfasında, planlarınızın adı, kimliği, diğer ayrıntılar, geçerli yayımlama durumu ve kullanılabilir eylemlerin bir listesi gösterilir. Kullanılabilir eylemler planınızın durumuna bağlıdır ve şunları içerebilir:

-  Plan durumu **taslak** ise, taslağı silin.
-  Plan durumu **canlı** ise, planı satmayı veya özel izleyiciyi eşitlemeyi durdurun.

Aşağıdaki ekran görüntüsünde iki taslak teklif gösterilmektedir.

![Iş Ortağı Merkezi 'nde plana genel bakış sayfasını gösterir. İki plan gösterilmektedir.](./media/commercial-marketplace-plans/plan-overview-tab.png)

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Ticari Market, yayımcıların fiyatları, Microsoft ürün müşterileri ve Microsoft 'un bir kurum ücretini stopajına göre gelir ödediği bir kurum modeli üzerinde çalışır. Teklifinizin pazarlarınızı, görünürlüğünü ve fiyatlandırmasını (uygun olduğunda) **fiyatlandırma ve kullanılabilirlik** veya **kullanılabilirlik** sekmesinde tanımlarsınız.

-  **Pazarlar**: her plan en az bir pazarda kullanılabilir olmalıdır.  Sizin adınıza Microsoft remits Sales ve Use vergi olarak yalnızca "vergi havalesi" ülkeleri seçebilirsiniz.
-  **Fiyatlandırma**: fiyatlandırma modelleri yalnızca Azure yönetilen uygulaması, SaaS ve Azure sanal makine teklifleri planları için geçerlidir. Aynı teklif için tüm planların aynı fiyatlandırma modelini kullanması gerekir.  
-  **Plan görünürlüğü**: teklif türüne bağlı olarak, özel bir hedef kitle tanımlayabilir veya teklifi veya planı Azure Marketi 'nden gizleyebilirsiniz. Bu makalenin ilerleyen bölümlerinde [plan görünürlüğü](#plan-visibility) bölümünde daha ayrıntılı olarak açıklanmaktadır.

> [!TIP]
> Hedef müşteri tabanınızın kullanım düzenlerine en uygun planlar oluşturmanızı öneririz. Bu, kullanıcıların, kullanımlardaki değişikliklere göre sıklıkla planlamalarını azaltır. Üç ölçümlü faturalandırma planına sahip SaaS teklifinin bir örneği için bkz. [örnek teklif](./partner-center-portal/saas-metered-billing.md#sample-offer).

### <a name="pricing-models"></a>Fiyatlandırma modelleri

Aşağıdaki teklif türleri için bir fiyatlandırma modelini her planla ilişkilendirmeniz gerekir. Bu teklif türlerinin her biri farklı kullanılabilir fiyatlandırma modellerine sahiptir:

-  **Azure yönetilen uygulaması**: düz fiyat (aylık) ve kullanım tabanlı fiyatlandırma (ölçüm hizmeti boyutları).
-  **Hizmet olarak yazılım**: sabit fiyat (aylık veya yıllık), Kullanıcı başına ve kullanım tabanlı fiyatlandırma (ölçüm hizmeti boyutları). 
-  **Azure sanal makinesi**: kendi lisansınızı getırın (KLG) ve kullanım tabanlı fiyatlandırma. Kullanım tabanlı fiyatlandırma modeli için, çekirdek başına, çekirdek başına boyut veya Pazar ve çekirdek boyut başına ücretlendirileyebilirsiniz. KLG lisans modeli, ek, kullanım tabanlı ücretler için izin vermez.   (KLG sanal makine teklifleri fiyatlandırma modeli gerektirmez.)

Aynı teklif için tüm planların aynı fiyatlandırma modelini kullanması gerekir. Örneğin, SaaS teklifinin, sabit bir ücret ve Kullanıcı başına başka bir plan olan bir plana sahip olamaz. Ayrıntılı bilgi için bkz. belirli teklif belgeleri.

Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Değişikliklerinizi kaydettikten sonra, yayımlamadan önce her bir pazarın fiyatını gözden geçirmek ve değiştirmek için kullanabileceğiniz bir **dışarı aktarma fiyatları (xlsx)** bağlantısı görürsünüz.

> [!IMPORTANT]
> Teklifiniz yayımlandıktan sonra fiyatlandırma modeli seçimi değiştirilemez.

Sabit fiyat SaaS teklifleri ve yönetilen uygulama, Market ölçüm hizmeti kullanılarak tarifeli faturalandırma desteği sunar. Bu, müşterilerinizin Tüketim esasına göre ödeme yapacak bant genişliği veya e-postalar gibi standart olmayan birimler tanımlamanızı sağlayan, kullanım tabanlı bir faturalandırma modelidir. [Yönetilen uygulamalar](./partner-center-portal/azure-app-metered-billing.md) ve [SaaS uygulamaları](./partner-center-portal/saas-metered-billing.md)için ölçülen faturalandırma hakkında daha fazla bilgi edinmek için ilgili belgelere bakın.

## <a name="custom-prices"></a>Özel fiyatlar

Tek bir pazarda özel fiyatlar ayarlamak için, fiyatlandırma elektronik tablosunu dışarı aktarın, değiştirin ve içeri aktarın. Bu fiyatlandırmayı doğrulamadan ve bu ayarların sahibi olduğunuzdan siz sorumlusunuz.

Bir plan yayımlandıktan sonra nelerin değiştirebilecekleri hakkında bazı kısıtlamalar olduğundan, yayımlamadan önce fiyatlarınızı dikkatlice gözden geçirin:

-  Bir plan yayımlandıktan sonra fiyatlandırma modeli değiştirilemez.
-  Bir plan için faturalandırma dönemi yayımlandıktan sonra, daha sonra kaldırılamaz.
-  Planınızdaki bir pazar bedeli yayımlandıktan sonra, daha sonra değiştirilemez.

Birleşik Devletler dolar (USD) cinsinden ayarlanan fiyatlar, kaydedildiği sırada geçerli döviz kurları kullanılarak seçilen tüm piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışa aktararak ve seçtiğiniz her bir pazardaki fiyatı inceleyerek yayımlamadan önce bu fiyatları doğrulayın.

1.  Fiyatlandırma verilerinin dışarı aktarılmasını sağlamak için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir. **Fiyatlandırma ve kullanılabilirlik** sekmesinin altındaki **Taslağı kaydet**' i seçin.
1.  **Fiyatlandırma**bölümünde **fiyatlandırma verilerini dışarı aktar** bağlantısını seçin.
1.  exportedPrice.xlsx dosyasını Microsoft Excel 'de açın.
1.  Elektronik tabloda, istediğiniz güncelleştirmeleri Fiyatlandırma bilgileriniz yapın ve ardından öğesini kaydedin. CSV dosyası.
    > [!NOTE]
    > Dosyayı güncelleştirebilmeniz için önce Excel 'de düzenlemesini etkinleştirmeniz gerekebilir.
1.  Fiyatlandırma **ve kullanılabilirlik** sekmesindeki **fiyatlandırma**bölümünde **fiyatlandırma verilerini içeri aktar** bağlantısını seçin.
1.  Görüntülenen iletişim kutusunda **Evet**' i seçin.
1.  Güncelleştirdiğiniz exportedPrice.xlsx dosyasını seçin ve sonra **Aç**' ı seçin.
















### <a name="plan-visibility"></a>Plan görünürlüğü

Bazı teklif türleri için genel kullanıma açık olan veya yalnızca belirli (özel) bir hedef kitle tarafından kullanılabilen planlar yapabilirsiniz. Özel planlarla teklifler Azure portal yayımlanacak. [Microsoft ticari Market 'Teki özel tekliflerle](private-offers.md)ilgili özel planlar hakkında daha fazla bilgi edinin.

Özel hedef kitlenizi, teklif türüne bağlı olarak Azure kiracı kimliklerini veya Azure abonelik kimliklerini kullanarak tanımlarsınız. 10 adede kadar kimliği el ile girebilir veya bir. csv dosyasıyla en fazla 10, 00 abonelik kimliği veya 20.000 Kiracı kimliği (varsa) aktarabilirsiniz. Ayrıca danışmanlık hizmetleri ve planı olmayan Dynamics 365 teklifleri için özel izleyiciler tanımlayabilirsiniz.

Teklifiniz özel bir planla yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür kalması gerekir ve yeniden özel bir plan olarak yapılandırılamaz.

> [!NOTE]
> Özel bir hedef kitle önizleme izleyicinden farklıdır. **Kullanılabilirlik** sayfasında, bazı teklif türleri için, ticari markette canlı olarak yayınlanmadan önce teklifinizi önizleyebilir bir hedef kitle tanımlayabilirsiniz. Özel hedef kitle ataması yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi tüm planları (özel veya değil) görüntüleyebilir, ancak yalnızca planlama test edilirken ve doğrulanırken sınırlı önizleme döneminde görüntülenir.

Teklifinizin yönetilen bir uygulamanın veya çözüm şablonunun parçası olarak dağıtılması durumunda planı tamamen Azure Marketi 'nden gizlemeyi de seçebilirsiniz.

## <a name="free-trials"></a>Ücretsiz denemeler

Transactable Azure sanal makinesi ve SaaS teklifleri için planlar üzerinde ücretsiz deneme sürümü sağlayabilirsiniz.

> [!NOTE]
> Bu bölümde, Microsoft aracılığıyla satılan tekliflerle ilgili ücretli planlardaki ücretsiz denemeler açıklanmaktadır. Bu, işlemlerini bağımsız olarak işlemeyi seçen iş ortaklarının ücretsiz deneme listelerinden farklıdır. Aşağıdaki teklif türleri için ücretsiz bir deneme listesi (bir planla ilişkili olmayan) oluşturabilirsiniz:
> -  Azure sanal makine 
> -  SaaS 
> -  Dynamics 365 Business Central
> -  PowerApps & müşteri katılımı için Dynamics 365
> -  Dynamics 365 for Operations
> 
> Seçenekleri listeleme hakkında daha fazla bilgi için bkz. [Yayımlama seçeneğinizi belirleme](determine-your-listing-type.md).

Ücretsiz denemeler, ölçümlü planlar hariç tüm faturalandırma modellerinde desteklenir. SaaS planları 1 aylık ücretsiz denemelerdeki izin verir. Azure sanal makine planları, 1, 3 veya 6 aylık ücretsiz denemelerdeki izin verir.

Bir müşteri ücretsiz bir deneme seçtiğinde, faturalandırma bilgilerini topladık, ancak deneme ücretli aboneliğe dönüştürülene kadar müşteriyi faturalandırmaya başlamayız. Ücretsiz denemeler, deneme süresi sona ermeden önce müşteri aboneliği iptal etmediği takdirde, deneme sonunda otomatik olarak ücretli bir aboneliğe dönüştürülür.

Deneme süresi boyunca, müşteriler ücretsiz deneme sürümü etkinleştirilmiş olan aynı teklifde desteklenen planların herhangi birini değerlendirebilir. Aynı teklif içinde farklı bir denemeye geçiş yaptıysanız, deneme süresi yeniden başlatmaz. Örneğin, bir müşteri 15 gün boyunca ücretsiz bir deneme sürümü kullanıyorsa ve sonra aynı teklif için farklı bir ücretsiz denemeye geçiş yaptığında, yeni deneme süresi 15 gün önce hesaba acaktır. Deneme süresi sona erdiğinde müşterinin değerlendirdiği ücretsiz deneme, otomatik olarak ücretli bir aboneliğe dönüştürüledir.

Bir müşteri, bir plan için ücretsiz bir deneme seçtikten sonra deneme süresi sona erene kadar bu plan için ücretli aboneliğe dönüştürülemez. Bir müşteri, teklif içinde ücretsiz deneme olmayan farklı bir plana dönüştürmeyi seçerse, dönüştürme gerçekleşir ancak ücretsiz deneme anında sona erecektir ve tüm veriler kaybedilir.

> [!NOTE]
> Bir müşteri bir plan için ödeme başlattıktan sonra, ücretsiz denemeleri destekleyen başka bir plana geçseler bile, aynı teklifte ücretsiz bir deneme alamaz.

Ücretsiz bir denemeye katılmış olan müşteri abonelikleri hakkında bilgi edinmek için, `isFreeTrial` doğru veya yanlış olarak işaretlenecek yenı API özelliğini kullanın. Daha fazla bilgi için [SaaS Get abonelik API 'sine](./partner-center-portal/pc-saas-fulfillment-api-v2.md#get-subscription)bakın.

## <a name="next-steps"></a>Sonraki adımlar

-  Mevcut bir teklifle plan ekleme veya güncelleştirme hakkında bilgi edinmek için bkz. [ticari Market 'te mevcut teklifi güncelleştirme](./partner-center-portal/update-existing-offer.md).
-  İşlem seçenekleri ve ilişkili fiyatlandırma modelleri hakkında daha fazla bilgi edinmek için bkz. [ticari Market Transact özellikleri](./marketplace-commercial-transaction-capabilities-and-considerations.md).
