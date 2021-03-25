---
title: Test teklifi oluşturma
description: Microsoft Iş Ortağı Merkezi 'nde ticari Market programında üretim teklifinizi test etmek için ayrı bir geliştirme teklifi oluşturma.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 58649e9a864e64ab5781cff3b663e190dac50cb6
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105050726"
---
# <a name="create-a-test-offer"></a>Test teklifi oluşturma

Üretim teklifinizden ayrı bir ortamda geliştirme yapmak için ayrı bir test ve geliştirme (DEV) teklifi ve ayrı bir üretim (ÜRETIM) teklifi oluşturacaksınız. Ayrı bir GELIŞTIRME teklifi kullanmanın avantajları hakkında daha fazla bilgi için bkz. [SaaS teklifi planlaması](plan-saas-offer.md#test-offer).

Çoğu ayarı GELIŞTIRME ve ÜRETIM tekliflerle aynı şekilde yapılandıracaksınız. Örneğin, resmi pazarlama dili ve ekran görüntüleri ve amblemler gibi varlıkların aynı olması gerekir. Yapılandırmanın aynı olduğu durumlarda, DEV teklifinde planlardaki alanları, ÜRETIM teklifinin planlarına kopyalayabilir ve yapıştırabilirsiniz.

Aşağıdaki bölümlerde GELIŞTIRME ve ÜRETIM teklifleri arasındaki yapılandırma farklılıkları açıklanır.

## <a name="offer-setup-page"></a>Teklif kurulum sayfası

Her iki teklifin de **diğer** ad kutusunda aynı diğer adı kullanmanızı öneririz, daha sonra dev teklifinin diğer adına "_test" ekleyin. Örneğin, ÜRETIM teklifinizin diğer adı "contososolution" ise, DEV teklifinin diğer adı "contososolution_test" olmalıdır. Bu şekilde, ÜRETIM teklifinizden hangi GELIŞTIRME teklifinizi teklifinizi kolayca belirleyebilirsiniz.

**Müşteri adayları** bölümünde, geliştirme teklifi Için ve Azure tablosu ya da bır test CRM ortamı kullanın. ÜRETIM teklifi için amaçlanan müşteri adayı yönetim sistemini kullanın.

## <a name="properties-page"></a>Özellikler sayfası

Bu sayfayı hem GELIŞTIRME hem de ÜRETIM tekliflerle aynı şekilde yapılandırın.

## <a name="offer-listing-page"></a>Teklif listesi sayfası

Bu sayfayı hem GELIŞTIRME hem de ÜRETIM tekliflerle aynı şekilde yapılandırın.

## <a name="preview-audience"></a>İzleyiciyi Önizle

GELIŞTIRME teklifinde, geliştiriciler ve test ediciler için kendi kendinize dahil Azure Active Directory (AAD) Kullanıcı asıl adı veya Microsoft hesabı (MSA) e-posta adresini ekleyin. Lütfen AAD 'deki bir kullanıcının Kullanıcı asıl adı, kullanıcının e-postaından farklı olabilir. Örneğin, jane.doe@contoso.com çalışmayacak ancak çalışacaktır janedoe@contoso.com . Geliştirme ve test aşamasında **Önizleme** bağlantısını paylaştığınızda dev teklifine erişimi olan kişiler şunlardır.

ÜRETIM teklifinde, teklifi doğrulamak için **canlı çalış düğmesini** seçmeden önce teklifi doğrulayacak KULLANıCıLARıN Azure AD Kullanıcı asıl adını veya Microsoft hesabı e-postasını dahil edin.

## <a name="technical-configuration-page"></a>Teknik yapılandırma sayfası

Bu tabloda, DEV teklifleri ve ÜRETIM teklifleri için ayarlar arasındaki farklar açıklanmaktadır.

***Tablo 1: teknik yapılandırma farkları***

| Ayar | GELIŞTIRME teklifi | ÜRETIM teklifi |
| ------------ | ------------- | ------------- |
| Giriş sayfası URL 'SI | Geliştirme ve test uç noktanızı girin. | Üretim uç noktanızı girin. |
| Bağlantı Web kancası | Geliştirme ve test uç noktanızı girin. | Üretim uç noktanızı girin. |
| Azure Active Directory kiracı KIMLIĞI | Test uygulaması kayıt kiracı KIMLIĞINIZI (AAD dizin KIMLIĞI) girin. | Üretim uygulaması kayıt kiracı KIMLIĞINIZI girin. |
| Azure Active Directory uygulama KIMLIĞI | Test uygulaması kayıt uygulaması KIMLIĞINIZI (istemci KIMLIĞI) girin. | Üretim uygulaması kayıt uygulaması KIMLIĞINIZI girin. |
||||

## <a name="plan-overview-page"></a>Plana genel bakış sayfası

Planlarınızı oluştururken, **_test** ile geliştirme TEKLIFINE plan kimliği ekleme harıç hem geliştirme hem de üretim tekliflerinin her ikisinde de aynı _plan kimliği_ ve _plan adını_ kullanmanızı öneririz. Örneğin, ÜRETIM teklifinin plan KIMLIĞI "Kurumsal" ise, DEV teklifinde plan KIMLIĞI "enterprise_test" olmalıdır. Bu şekilde, ÜRETIM teklifinizden hangi GELIŞTIRME teklifinizi teklifinizi kolayca belirleyebilirsiniz. ÜRETIM teklifinde teklif için en iyi şekilde karar vereceğiniz fiyatlandırma modellerini ve fiyatlarını içeren planlar oluşturacaksınız.

### <a name="plan-listing"></a>Planı listeleme

**Plana genel bakış**  >  **planı listeleme** sekmesinde hem geliştirme hem de üretim planlarına aynı plan açıklamasını girin.

### <a name="pricing-and-availability-page"></a>Fiyatlandırma ve kullanılabilirlik sayfası

Bu bölüm, **plana genel bakış**  >  **fiyatlandırma ve kullanılabilirlik** sayfasının tamamlanmasına yönelik rehberlik sağlar.

#### <a name="markets"></a>Pazar

GELIŞTIRME ve ÜRETIM teklifleri için aynı pazarları seçin.

#### <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma modelleriyle denemeler yapmak için GELIŞTIRME teklifini kullanın. Hangi fiyatlandırma modelinin veya modellerinin en iyi şekilde çalıştığını doğruladıktan sonra, ÜRETIM teklifinde istediğiniz fiyatlandırma modellerini ve fiyatlarını içeren planlar oluşturacaksınız.

DEV teklifinin, planlarda sıfır veya çok düşük fiyatlara sahip planları olmalıdır. ÜRETIM teklifi, müşterilere ücretlendirmenize istediğiniz fiyatlara sahip olacaktır.

> [!NOTE]
> Önizlemede yapılan Skımingsatınalmaları hem GELIŞTIRME hem de ÜRETIM teklifleri için işlenebilse bile, kullanıcının bildirimde bulunan bilgiler. Bir teklifin $100/ay fiyatı varsa şirketiniz ücretlendirilir $100. Bu durumda, bir [destek bileti](support.md) açabilirsiniz ve tüm miktar için bir ödeme (ve bir acenteücret ödemesiz) olarak ödersiniz.

#### <a name="pricing-model"></a>Fiyatlandırma modeli

GELIŞTIRME ve ÜRETIM tekliflerinin planlarında aynı fiyatlandırma modelini kullanın. Örneğin, ÜRETIM teklifinin planı bir aylık fatura dönemi ile sabit bir ücret ise, GELIŞTIRME teklifinde planı aynı modeli kullanarak yapılandırın.

Market özel ölçüm boyutları da dahil olmak üzere fiyatlandırma modellerini test etme maliyetinizi azaltmak için, **fiyatlandırma ve kullanılabilirlik** sekmesinin **fiyatlandırma** bölümünü üretim teklifinden daha düşük fiyatlarla daha düşük fiyatlarla yapılandırmanızı öneririz. GELIŞTIRME teklifinde planlar için fiyatları ayarlarken kullanabileceğiniz bazı yönergeler aşağıda verilmiştir.

***Tablo 2: fiyatlandırma yönergeleri***

| Fiyat | Yorum |
| ------------ | ------------- |
| 0,00$ | Mali bir etkisi olmayan toplam işlem maliyetini sıfır olarak ayarlayın. Bu fiyatı, ölçüm API 'Lerine çağrılar yaparken veya Çözümünüzü geliştirirken teklifinizin satın alma planlarını test etmek için kullanın. |
| $0,01-$49,99 | Analiz, raporlama ve satın alma sürecini test etmek için bu fiyat aralığını kullanın. |
| $50,00 ve üzeri | Bu fiyat aralığını, ödeme test etmek için kullanın. Ödeme zamanlamamız hakkında daha fazla bilgi için bkz. [ödeyen zamanlamalar ve süreçler](/partner-center/payout-policy-details). |
|||

Testinizde bir işleme ücreti ücretlendirmeden kaçınmak için bir [destek bileti](support.md)açın.

#### <a name="free-trial"></a>Ücretsiz deneme sürümü

GELIŞTIRME teklifi için ücretsiz deneme sürümü etkinleştirmeyin.

## <a name="co-sell-with-microsoft-page"></a>Microsoft sayfasıyla ortak satış

**Ortak SATıŞı** geliştirme teklifinin Microsoft sekmesi ile yapılandırmayın.

## <a name="resell-through-csps"></a>CSP’ler aracılığıyla satış

GELIŞTIRME teklifinin **CSP aracılığıyla yeniden satılmasını** yapılandırmayın.

## <a name="next-steps"></a>Sonraki adımlar

- [SaaS teklifini test etme ve yayımlama](test-publish-saas-offer.md)
