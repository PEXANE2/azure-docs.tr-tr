---
title: Commercial Marketplace teklifinizin yayın durumunu kontrol edin
description: Microsoft İş Ortağı Merkezi'ndeki Commercial Marketplace aracılığıyla bir teklifi yayımlamak için gereken doğrulama, sertifika ve önizleme adımlarının durumunu denetleyin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 012a574887d9980e0c71c3af84ff70ca8d31312c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275688"
---
# <a name="check-the-publishing-status-of-your-commercial-marketplace-offer"></a>Commercial Marketplace teklifinizin yayın durumunu kontrol edin

İş Ortağı Merkezi'ndeki Ticari Pazar Yeri [portalının](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) **Teklif genel bakış** sekmesinde geçerli **Yayımlama durumunuzu** görüntüleyebilirsiniz.

Her teklif için aşağıdaki durum göstergelerinden biri görüntülenmelidir.

| **Durum**    | **Açıklama**  |
| :---------- | :-------------------|
| **Taslak** | Teklif oluşturuldu, ancak yayınlanmadı. |
| **Devam eden yayımlama** | Teklif/Plan, yayımlama sürecinin adımlarını tamamlıyor. |
| **Dikkat gerekli** | Microsoft tarafından veya yayımlama adımlarından herhangi biri tarafından sertifikalandırılım sırasında kritik bir sorun keşfedildi. |
| **Önizleme** | Teklif Microsoft tarafından onaylandı ve şimdi yayıncı tarafından son doğrulama bekliyor. Teklifi canlı yapmak için canlı yayına geçin'i seçin. |
| **Canlı** | Teklif pazarda canlı ve görülebilir ve müşteriler tarafından elde edilebedilir. |
| **Bekleyen stop satmak** | Yayımcı teklif veya plan "satış" olarak seçti, ancak eylem henüz tamamlanmadı. |
| **Pazarda mevcut değil** | Pazarda daha önce yayınlanmış bir teklif/plan kaldırıldı. |

## <a name="automated-validation"></a>Otomatik doğrulama

Yayımlama işlemindeki ilk adım, otomatik doğrulama kümesidir. Her doğrulama adımı, teklifinizin oluşturulmasında etkinleştirmeyi seçtiğiniz bir özelliğe karşılık gelir. Bu özellik etkinleştirilemediyse, doğrulama bir sonraki yayımlama adımına atlar. Yayımlama durumu onaylanmadan önce her doğrulama denetiminin tamamlanması gerekir.

- **Teklif satın alma akışı kurulumu (10 dk <)**

Bu adımda, azure portalı üzerinden müşteriler tarafından satın alındığında teklifinizin yerine getirilebileceğinden emin oluruz. Bu adım yalnızca Microsoft üzerinden satılan teklifler için geçerlidir.

- **Test sürüşü veri doğrulaması (~5 dk)**

Bu adımda, teklifin test sürüşü Teknik yapılandırma bölümünde sağladığınız verileri doğrularız. Test sürüşü işlevselliği test edilir ve onaylanır. Bu adım yalnızca test sürüşü etkinleştirilmiş teklifler için geçerlidir.

- **Test sürüşü sağlama (~30 dk)**

Bu adımda, önceki adımda test sürücünüzün verilerini ve işlevselliğini doğruladıktan sonra, test sürücünüzün örneklerini müşteri kullanımına hazır olacak şekilde dağıtıp çoğaltıyoruz.  Bu adım yalnızca test sürüşü etkinleştirilmiş teklifler için geçerlidir.

- **Müşteri adayı yönetimi doğrulama ve kayıt (15 dk <)**

Bu adımda, müşteri adayı yönetim sisteminizin Teklif kurulumunda sağlanan ayrıntılara bağlı olarak müşteri müşteri adaylarını alabileceğini onaylıyoruz. Bu adım yalnızca Müşteri Adayı yönetimi etkinleştirilmiş teklifler için geçerlidir.

## <a name="certification"></a>Sertifika

Yayınlanmadan önce, İş Ortağı Merkezi'ndeki Commercial Marketplace'e sunulan tekliflerin onaylanması gerekmektedir. Gönderilen teklifler, [Azure Marketi sertifika ilkelerine](https://docs.microsoft.com/legal/marketplace/general-policies)karşı bir denetim de dahil olmak üzere bazı otomatik ve diğerleri el kitabından sıkı bir testten geçer. Teklif gönderimleri, yayımlama akışındaki bir sonraki adıma geçmeden önce sertifika almaya uygun olarak işaretlenmelidir.

### <a name="types-of-validation-that-take-place-during-certification"></a>Sertifikalama sırasında gerçekleşen doğrulama türleri

Sunulan her teklif için sertifika sürecine dahil edilen üç doğrulama düzeyi vardır.

- Yayıncı iş uygunluğu
- İçerik doğrulama
- Teknik doğrulama

#### <a name="publisher-business-eligibility"></a>Yayıncı iş uygunluğu

Her teklif türü, yayımcının karşılaması gereken bir dizi temel uygunluk ölçütlerini denetler. Uygunluk kriterleri yayıncının MPN durumunu, düzenlenen yetkinlikleri, yetkinlik düzeylerini vb. içerebilir.

#### <a name="content-validation"></a>İçerik doğrulama

İçerik doğrulama sırasında, teklifinizi oluşturduğunuzda girilen bilgiler kalite ve alaka düzeyi açısından denetlenir. Bu denetimler, pazar yeri listeleme ayrıntıları, fiyatlandırma, kullanılabilirlik, ilişkili planlar, vb. için girişlerinizi inceler. Azure Marketi ve/veya AppSource giriş kriterlerini karşılamak için teklifinizin şunları içerdiğini doğrularız:

- teklifi doğru bir şekilde açıklayan bir başlık;
- kapsamlı bir genel bakış ve değer teklifi sağlayan iyi yazılmış açıklamalar;
- kaliteli ekran görüntüleri ve beraberindeki videolar; Ve
- teklifin Microsoft platformlarını ve araçlarını nasıl kullandığına ilişkin bir açıklama.

[Genel listeleme politikalarını](https://docs.microsoft.com/legal/marketplace/certification-policies#100-general)okuyarak içerik doğrulama ölçütleri hakkında daha fazla bilgi edinin.

#### <a name="technical-validation"></a>Teknik doğrulama

Teknik doğrulama sırasında, teklif (paket veya ikili) aşağıdaki kontrollerden geçer.
- Kötü amaçlı yazılımlar için taranan
- Ağ aramaları izlenir
- Paket analiz edildi
- Teklifin gerçek işlevselliğinin kapsamlı olarak taranması

Teklif, sağlam olduğundan emin olmak için çeşitli platformlarda ve sürümlerde test edilir.

Bu belgenin Teknik yapılandırma bölümünde teklifiniz için gereken belirli yapılandırma ayrıntılarını gözden geçirin.

### <a name="certification-failure-report"></a>Sertifika hatası raporu

İnceleme tamamlandıktan sonra, teklifiniz sertifikayı geçtiyse, yayımlama sürecinde bir sonraki adıma geçer. Teklifiniz giriş, teknik veya ilke denetimlerinden herhangi birinde başarısız olduysa veya bu tür bir teklifi göndermeye uygun değilseniz, bir sertifika hatası raporu oluşturulur ve size e-posta ile gönderir.

Bu rapor, gözden geçirme notları ile birlikte başarısız olan tüm ilkelerin açıklamalarını içerir. Bu e-posta raporunu gözden geçirin, herhangi bir sorunu ele alın, gerektiğinde teklifinizi güncelleyin ve İş Merkezi'ndeki [Ticari Pazar Portalı'nı](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) kullanarak teklifi yeniden gönderin. (Teklifi, sertifikayı geçene kadar gerektiği kadar yeniden gönderebilirsiniz).

## <a name="preview-creation"></a>Önizleme oluşturma

Önizleme **oluşturma** adımı sırasında, teklifinizin yalnızca teklifinizin Önizleme bölümünde belirttiğiniz hedef kitletarafından erişilebilir bir sürümünü oluştururuz.

>[!Note]
> Bu adımı, kuruluşunuz dışındaki kişilere bir teklifte görünürlük sağlamak için kullanmayın. Bunun yerine **Özel Teklif** seçeneğini kullanın. Bu noktada, teklifiniz tam olarak test edilmemiş ve doğrulanmamıştır ve dış dağıtım için hazır değildir.

## <a name="publisher-approval"></a>Yayıncı onayı

Bu adımda, son yayımlama adımından önce teklif önizlemenizi gözden geçirmeniz ve onaylamanız için size bir istek gönderilecektir.

Teklifinizi Microsoft aracılığıyla satmayı seçtiyseniz, bu önizleme onay aşamasında gereksinimlerinizi karşıladığından emin olmak için teklifinizin edinisini ve dağıtımını test edebilirsiniz. Teklifiniz henüz halka açık pazarda sunulmayacaktır. Bu önizlemeyi test edip onayladıktan sonra, [**Teklife Genel Bakış**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) panosunda **Go-Live'ı** seçmeniz gerekir.

Bu önizleme aşamasında teklifte değişiklik yapmak istiyorsanız, yeni bir önizleme yayınlamak için düzenleyebilir ve yeniden gönderebilirsiniz. Makaleye bakın Daha fazla değişiklik hakkında ayrıntılar için [varolan pazar teklifini](#update-existing-marketplace-offers) güncelleştirin.

Teklifiniz zaten yayındaysa ve pazarda herkese açıksa, [**TeklifE Genel Bakış**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) panosunda **Go-live'ı** seçene kadar yaptığınız güncellemeler geçerli olmaz.

### <a name="publish-offer-to-the-public"></a>Teklifi halka yayınlama

Partner Center'da oturum açın ve teklife erişin. **Teklif'e genel bakış** sayfasına yönlendirilirsiniz. Bu sayfanın üst **kısmında, canlı git**seçeneği göreceksiniz. **Canlı Git'i** seçin ve onaylandıktan sonra teklif herkese yayınlanmaya başlayacaktır. Teklif canlı olduğunda bir e-posta bildirimi alırsınız.

## <a name="publish"></a>Yayımlama

Artık teklifinizle **birlikte yaşamaya devam** etmeyi seçtiğinize göre, teklifi pazarda kullanıma sunmak için, canlı teklifin teklifin önizleme sürümü gibi yapılandırıldığından emin olmak için aşamalı olarak verilecek bir dizi son doğrulama denetimi vardır.

- **Teklif satın alma akışı kurulumu (10 dk >)**

Bu adımda, azure portalı üzerinden müşteriler tarafından satın alındığında teklifinizin yerine getirilebileceğinden emin oluruz. Bu adım yalnızca Microsoft üzerinden satılan teklifler için geçerlidir.

- **Test sürüşü veri doğrulaması (~5 dk)**

Bu adımda, teklifin test sürüşü Teknik yapılandırma bölümünde sağladığınız verileri doğrularız. Test sürüşü işlevselliği test edilir ve onaylanır. Bu adım yalnızca test sürüşü etkinleştirilmiş teklifler için geçerlidir.

- **Test sürüşü sağlama (~30 dk)**

Bu adımda, test sürüşünüzün örneklerini müşteri kullanımına hazır olacak şekilde dağıtıp çoğaltıyoruz.  Bu adım yalnızca test sürüşü etkinleştirilmiş teklifler için geçerlidir.

- **Müşteri adayı yönetimi doğrulama ve kayıt (15 dk >)**

Bu adımda, müşteri adayı yönetim sisteminizin Teklif kurulumunda sağlanan ayrıntılara bağlı olarak müşteri müşteri adaylarını alabileceğini onaylıyoruz. Bu adım yalnızca Müşteri Adayı yönetimi etkinleştirilmiş teklifler için geçerlidir.

- **Son yayın (>30 dakika)**

Bu adımda, teklifinizin pazarda herkese açık olmasını sağlıyoruz.

## <a name="update-existing-marketplace-offers"></a>Mevcut pazar tekliflerinin güncelleştirin

Daha önce yayımlamış olduğunuz bir teklifte değişiklik yapmak istiyorsanız, önce varolan teklifi güncelleştirmeniz ve ardından yeniden yayımlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
