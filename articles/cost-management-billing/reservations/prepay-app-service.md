---
title: Ayrılmış kapasite ile Azure App Service’te maliyetlerden tasarruf etme
description: Azure App Service Premium v3 ayrılmış örnekleri ve yalıtılmış damga ücreti için maliyetleri nasıl kaydedebileceğinizi öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 02/01/2021
ms.author: banders
ms.custom: references_regions
ms.openlocfilehash: 89e0c62b580c0c354fc7277e61b452005a86e3d9
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99577816"
---
# <a name="save-costs-with-azure-app-service-reserved-instances"></a>Azure App Service ayrılmış örneklerle maliyet tasarrufu yapın

Bu makalede, Premium v3 örnekleri ve yalıtılmış damga ücretleri için Azure App Service ayrılmış örneklerle nasıl tasarruf sağlayabileceğiniz açıklanmaktadır.

## <a name="save-with-premium-v3-reserved-instances"></a>Premium v3 ile ayrılmış örneklerle Kaydet

Bir Azure App Service Premium v3 ayrılmış örneğine kayıt yaptığınızda para tasarrufu sağlayabilirsiniz. Rezervasyon indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen çalışan örneklerin sayısına otomatik olarak uygulanır. İndirimleri almak için bir örneğe rezervasyon atamanız gerekmez.

## <a name="determine-the-right-reserved-instance-size-before-you-buy"></a>Satın almadan önce doğru ayrılmış örnek boyutunu belirleme

Bir ayırma satın almadan önce, ihtiyacınız olan Premium v3 ayrılmış örneğinin boyutunu belirlemelisiniz. Aşağıdaki bölümler, doğru Premium v3 ayrılmış örnek boyutunu belirlemenize yardımcı olur.

### <a name="use-reservation-recommendations"></a>Ayırma önerilerini kullanma

Satın almanız gereken rezervasyonları belirlemede yardımcı olması için rezervasyon önerilerini kullanabilirsiniz.

- Satın alma önerileri ve önerilen miktar, Azure portal Premium v3 ile ayrılmış bir örnek satın alırken gösterilmektedir.
- Azure Advisor bireysel abonelikler için satın alma önerileri sağlar.
- Hem paylaşılan kapsam hem de tek abonelik kapsamı için satın alma önerileri almak üzere API 'Leri kullanabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal müşterilere yönelik ayrılmış örnek satın alma önerisi API 'leri](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).
- Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi (MCA) müşterileri için, [Azure Consumption Insights Power BI içerik paketi](/power-bi/service-connect-to-azure-consumption-insights)ile paylaşılan ve tek abonelik kapsamlarına yönelik satın alma önerileri sunulmaktadır.

#### <a name="instance-size-flexibility-setting"></a>Örnek boyut esnekliği ayarı

Örnek boyutu esnekliği ayarı, ayrılmış örnek indirimlerini hangi hizmetlerin alacağını belirler.

Ayarın açık veya kapalı olup olmadığı, rezervasyon iskontoları, eşleşen tüm Premium v3 ayrılmış örnek kullanımı için otomatik olarak uygulanır.

### <a name="analyze-your-usage-information"></a>Kullanım bilgilerinizi çözümleyin

Satın almanız gereken rezervasyonları belirlemede yardımcı olması için kullanım bilgilerinizi çözümleyin. Kullanım verileri, kullanım dosyası ve API 'lerde kullanılabilir. Hangi ayırmayı satın almak istediğinizi öğrenmek için bunları birlikte kullanın. Satın alınacak rezervasyonların miktarını öğrenmek için günlük olarak yüksek kullanıma sahip Premium v3 örneklerini denetleyin.

Kullanım dosyanız, faturalandırma dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Kullanım dosyanızı indirme hakkında daha fazla bilgi için bkz. [Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](../understand/download-azure-daily-usage.md). Ardından, kullanım dosyası bilgilerini kullanarak [hangi ayırmayı satın almayı belirleyebilirsiniz](determine-reservation-purchase.md).

### <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması konuları

Aşağıdaki Premium v3 örnekleri için rezervasyon iskontoları uygulanmaz:

- **Önizleme veya promosyon örnekleri** -önizlemede olan tüm Premium v3 ayrılmış örnek serisi veya boyutu veya promosyon ölçümü kullanır.
- **Bulutlar** -rezervasyonlar Almanya veya Çin bölgelerinde satın alınabilir.

## <a name="buy-a-premium-v3-reserved-instance"></a>Premium v3 ile ayrılmış örnek satın alma

Ayrılmış bir Premium v3 ayrılmış örneğini [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](prepare-buy-reservation.md) rezervasyon ödemesi yapın. Bu gereksinimler Premium v3 ayrılmış örneği satın almak için geçerlidir:

- En az bir EA aboneliği veya Kullandıkça Öde tarifesine sahip bir abonelik için sahip rolünde olmanız gerekir.
- EA abonelikleri için, **ayrılmış örnekler Ekle** seçeneğinin [EA portalında](https://ea.azure.com/)etkinleştirilmiş olması gerekir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetici aracıları veya satış aracıları ayırmaları satın alabilir.

Örnek satın almak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmetler** > **Rezervasyonlar**’ı seçin.
3. **Ekle** ' yi seçerek yeni bir ayırma satın alın ve ardından **örnek**' e tıklayın.
4. Gerekli alanları doldurun. Rezervasyon indirimine uygun olarak seçtiğiniz özniteliklerle eşleşen Premium v3 ayrılmış örneklerini çalıştırma. İskontoyu alan Premium v3 ayrılmış örneklerinizin gerçek sayısı, seçilen kapsama ve miktara göre değişir.

EA sözleşmeniz varsa, daha fazla örnek eklemek için **daha fazla ek ekle seçeneğini** kullanabilirsiniz. Bu seçenek diğer abonelik türleri için kullanılamaz.

| **Alan** | **Açıklama** |
|---|---|
| Abonelik | Ayırma için ödeme yapmak üzere kullanılan abonelik. Rezervasyon maliyetleri, aboneliğin ödeme yöntemiyle ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi ya da Kullandıkça Öde tarifesine sahip tek bir abonelik olmalıdır (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, parasal taahhüt bakiyesinden düşülür veya varsa fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine sahip bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır. |
| Kapsam | Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz: <ul><li>**Tek kaynak grubu kapsamı**: Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular. </li><li>**Tek abonelik kapsamı**: Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.</li><li>**Paylaşılan kapsam**: Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. EA müşterileri için, faturalandırma bağlamı kayıt olur. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.</li></ul> |
| Region | Ayırma kapsamındaki Azure bölgesi. |
| Premium v3 ayrılmış örnek boyutu | Premium v3 ayrılmış örneklerinin boyutu. |
| İçin iyileştirin | Premium v3 ayrılmış örnek boyutu esnekliği varsayılan olarak seçilidir. Aynı [Premium v3 ayrılmış örnek Boyut grubundaki](../../virtual-machines/reserved-vm-instance-size-flexibility.md)diğer Premium v3 ile ayrılmış örneklere rezervasyon iskontosunu uygulamak için örnek boyutu esneklik değerini değiştirmek için **Gelişmiş ayarlar** ' a tıklayın. Kapasite önceliği dağıtımlarınızda veri merkezi kapasitesine öncelik verir. İhtiyacınız olduğunda Premium v3 ayrılmış örneklerini başlatma yeteneizin daha fazla güvenilirlik sağlar. Kapasite önceliği yalnızca rezervasyon kapsamı tek abonelik olduğunda kullanılabilir. |
| Süre | Bir yıl veya üç yıl. Ayrıca, yalnızca HBv2 Premium v3 ile ayrılmış örnekler için kullanılabilir 5 yıllık bir terim vardır. |
| Miktar | Ayırma içinde satın alınan örneklerin sayısı. Miktar, fatura iskontosunu elde eden, çalışan Premium v3 ile ayrılmış örnek sayısıdır. Örneğin, Doğu ABD 10 standart \_ D2 Premium v3 ile ayrılmış örneği çalıştırıyorsanız, çalışan tüm Premium v3 ayrılmış örneklerinin avantajını en üst düzeye çıkarmak için miktarı 10 olarak belirtirsiniz. |

## <a name="save-with-isolated-stamp-fees"></a>Yalıtılmış damga ücretleri ile kaydet

Üç yıllık bir süre için damga kullanımınız için rezervasyon taahhüdünde bulunarak Azure App Service Yalıtılmış Damga Pulu Ücretlerinden para tasarrufu sağlayabilirsiniz. Yalıtılmış Damga Pulu Ücreti ayrılmış kapasitesi satın almak için, damga pulunun dağıtılacağı Azure bölgesini ve satın alınacak damga pulu sayısını seçmeniz gerekir.

Rezervasyon satın aldığınızda, rezervasyonun öznitelikleriyle eşleşen Yalıtılmış Damga Pulu Ücreti kullanımı artık kullandıkça öde fiyatlarıyla ücretlendirilmez. Rezervasyon, ayrılmış kapasite kapsamı ve bölgesiyle eşleşen Yalıtılmış damga pullarına otomatik olarak uygulanır. Yalıtılmış bir damga puluna rezervasyon atamanız gerekmez. Rezervasyon, çalışanlara uygulanmaz, bu nedenle damga puluyla ilişkili diğer tüm kaynaklar ayrı olarak ücretlendirilir.

Ayrılmış kapasitenin süresi dolduğunda, Yalıtılmış Damga Pulları çalışmaya devam eder, ancak kullandıkça öde fiyatlarıyla faturalanır. Rezervasyonlar otomatik olarak yenilenmez.

## <a name="determine-the-right-isolated-stamp-reservation-to-purchase"></a>Satın almak için doğru yalıtılmış damga ayırmayı belirleme

Bir rezervasyon satın alarak, ayrılmış miktarları gelecek üç yıl boyunca kullanma taahhüdü vermiş olursunuz. Düzenli olarak kullandığınız ve gelecekte kullanabileceğiniz App Service Yalıtılmış Damga Pulu sayısını belirlemek için kullanım verilerinizi denetleyin.

Buna ek olarak, Yalıtılmış Damga Pulunun Linux veya Windows ölçümünü nasıl gösterdiğini anladığınızdan emin olun.

- Boş bir Yalıtılmış Damga Pulu varsayılan olarak Windows damga pulu ölçümünü gösterir. Örneğin, hiçbir çalışan dağıtılmamış şekilde. Damga pulu üzerinde Windows çalışanları dağıtılırsa bu ölçümü göstermeye devam eder.
- Linux çalışanı dağıtırsanız ölçüm, Linux damga pulu ölçümüne değişir.
- Damga pulu, hem Linux hem de Windows çalışanlarının dağıtılması durumunda Windows ölçümünü gösterir.

Bu nedenle damga pulu ölçümü, damga pulunun ömrü boyunca Windows ve Linux arasında değişebilir.

Damga pulu üzerinde bir veya daha fazla Windows çalışanınız varsa Windows damga pulu rezervasyonu satın alın. Yalnızca damga pulunda _sırf_ Linux çalışanları olmasını planlıyorsanız Linux damga pulu rezervasyonu satın almanız gerekir.

## <a name="buy-isolated-stamp-reserved-capacity"></a>Yalıtılmış Damga Pulu ayrılmış kapasitesi satın alma

[Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D) Yalıtılmış Damga Pulu ayrılmış kapasitesi satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](./prepare-buy-reservation.md) rezervasyon ödemesi yapın. Ayrılmış kapasite satın almak için, en az bir kurumsal abonelik için sahip rolüne veya kullandıkça öde fiyatlarına tabi bireysel bir aboneliğe sahip olmanız gerekir.

- Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya ayar devre dışı bırakılırsa EA Yöneticisi olmanız gerekir.
- Bulut Çözümü Sağlayıcısı (CSP) programı için yalnızca yönetici aracılar veya satış temsilcileri, Azure Synapse Analytics ayrılmış kapasitesi satın alabilir.

**Satın almak için:**

1. [Azure portalına](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D) gidin.
1. Bir abonelik seçin. Ayrılmış kapasitenin ödemesini yapmak için kullanılan aboneliği seçmek amacıyla **Abonelik** listesini kullanın. Ayrılmış kapasite maliyetleri, aboneliğin ödeme yöntemiyle ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numarası: MS-AZR-0017P veya MS-AZR-0148P) ya da Kullandıkça Öde (teklif numarası: MS-AZR-0003P veya MS-AZR-0023P) ya da CSP aboneliği.
    - Kurumsal abonelik için ücretler kaydın Azure Ön Ödemesinden (eski adıyla parasal taahhüt) düşülür ve fazla kullanım olarak ücretlendirilir.
    - Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.
1. Bir **Kapsam** seçerek bir abonelik kapsamı seçin.
    - **Tek kaynak grubu kapsamı**: Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.
    - **Tek abonelik kapsamı**: Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.
    - **Paylaşılan kapsam**: Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. Kurumsal Anlaşma müşterileri için faturalama bağlamı kayıttır. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.
1. Ayrılmış kapasite kapsamındaki bir Azure bölgesi seçmek için bir **Bölge** seçin ve rezervasyonu sepete ekleyin.
1. Bir Yalıtılmış Plan türü seçin ve **Seç**’e tıklayın.  
    ![Örnek ](./media/prepay-app-service/app-service-isolated-stamp-select.png)
1. Ayrılacak App Service Yalıtılmış damga pulu miktarını girin. Örneğin, üç miktarı, size bir bölge için üç ayrılmış damga pulu sunar. **İleri: Gözden Geçirme + Satın Alma** seçeneğine tıklayın.
1. Gözden geçirin ve **Şimdi satın al** seçeneğine tıklayın.

Satın aldıktan sonra, satın alma durumunu görüntülemek ve istediğiniz zaman izlemek için [Rezervasyonlar](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)’a gidin.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md).

## <a name="discount-application-shown-in-usage-data"></a>Kullanım verilerinde gösterilen indirim uygulaması

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Kullanım verileri, her rezervasyondaki her damga pulu örneği için rezervasyon indirimini gösterir.

Kullanım verilerinde rezervasyon indiriminin nasıl gösterileceği hakkında daha fazla bilgi için, Kurumsal Anlaşma (EA) müşterisiyseniz bkz. [Kurumsal Anlaşma rezervasyon maliyetleri ve kullanım bilgilerini alma](understand-reserved-instance-usage-ea.md). Aksi takdirde bkz. [Kullandıkça öde fiyatlarına tabi bireysel aboneliğiniz için Azure rezervasyon kullanımını anlama](understand-reserved-instance-usage.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure App Service Yalıtılmış Damga Pulu rezervasyon indiriminin nasıl uygulandığını anlama](reservation-discount-app-service.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)