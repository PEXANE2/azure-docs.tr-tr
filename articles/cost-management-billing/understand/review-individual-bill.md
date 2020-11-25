---
title: Bireysel Azure aboneliği faturanızı inceleme
description: Kullandıkça öde planı dahil olmak üzere bireysel Azure aboneliğinize ait faturanızı ve kaynak kullanımınızı anlama ve ücretleri doğrulama konusunda bilgi edinin.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: tutorial
ms.date: 10/26/2020
ms.author: banders
ms.custom: contentperfq2
ms.openlocfilehash: c4a9a981017becd0d9343d9c287a4fe9341d7c43
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006158"
---
# <a name="tutorial-review-your-individual-azure-subscription-bill"></a>Öğretici: Bireysel Azure aboneliği faturanızı inceleme

Bu makale, kullandıkça öde ve Visual Studio dahil olmak üzere kullandıkça öde ya da Visual Studio Azure aboneliğinize yönelik faturanızı anlamanıza ve incelemenize yardımcı olur. Her faturalama döneminde e-posta ile bir fatura gönderilir. Faturanız Azure ücretlerinizin gösterimidir. Faturadaki maliyet bilgileri Azure portalında da mevcuttur. Bu öğreticide Azure portalında faturanızı ayrıntılı günlük kullanım dosyası ve maliyet analizi ile karşılaştıracaksınız.

Bu öğretici yalnızca bireysel aboneliğe sahip olan Azure müşterileri için geçerlidir. Bireysel abonelikler genellikle doğrudan Azure web sitesinden satın alınan ve kullandıkça öde ücretlerini kullanan aboneliklerdir.

Beklenmeyen ücretleri anlamayla ilgili yardıma ihtiyacınız varsa bkz. [Beklenmeyen ücretleri analiz etme](analyze-unexpected-charges.md). Azure aboneliğinizi iptal etmeniz gerekiyorsa bkz. [Azure aboneliğinizi iptal etme](../manage/cancel-azure-subscription.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Faturadaki ücretleri kullanım dosyasıyla karşılaştırma
> * Ücretleri ve kullanımı maliyet analiziyle karşılaştırma

## <a name="prerequisites"></a>Ön koşullar

Ücretli bir *Microsoft Online Services Program* ödeme hesabına sahip olmanız gerekir. Bu hesap, Azure web sitesi üzerinden Azure’a kaydolduğunuzda oluşturulur. Örneğin kullandıkça öde ücretlerine sahip bir hesabınız veya Visual Studio aboneliğiniz olabilir.

Azure Ücretsiz Hesapları için faturalar yalnızca aylık kredi tutarı aşıldığında düzenlenir.

Azure'a abone olmanızın üzerinden en az 30 gün geçmiş olmalıdır. Azure sizi fatura döneminizin sonunda faturalar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="compare-billed-charges-with-your-usage-file"></a>Faturaya yansıtılan ücretleri kullanım dosyanızla karşılaştırma

<a name="charges"></a>

Kullanım verilerini ve maliyetleri karşılaştırmanın ilk adımı, faturanızı ve kullanım dosyalarınızı indirmektir. Ayrıntılı kullanım CSV dosyası faturalama dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Vergi konusunda bilgi içermez. Dosyaları indirmek için hesap yöneticisi olmanız veya Sahip rolüne sahip olmanız gerekir.

Azure portalında arama kutusuna *abonelikler* yazın ve **Abonelikler**'e tıklayın.

[![Aboneliklere gitme](./media/review-individual-bill/navigate-subscriptions.png)](./media/review-individual-bill/navigate-subscriptions.png#lightbox)

Abonelikler listesinde aboneliğe tıklayın.

**Faturalama**'nın altında **Faturalar**'a tıklayın.

Faturalar listesinde indirmek istediğiniz faturayı bulun ve indir simgesine tıklayın. Eski faturaları görüntülemek için zaman aralığı değerini değiştirmeniz gerekebilir. Kullanım ayrıntıları dosyasının ve faturanın oluşturulması birkaç dakika sürebilir.

![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/review-individual-bill/download-invoice.png)

Kullanımı + Ücretleri İndir penceresinde **CSV dosyasını indir**'e ve **Faturayı indir**'e tıklayın.

![Faturayı indir'i ve kullanım sayfasını gösteren ekran görüntüsü](./media/review-individual-bill/usageandinvoice.png)

**Kullanılamıyor** iletisiyle karşılaşırsanız kullanım verilerinin veya faturanın mevcut olmamasının birkaç nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.
- Fatura dönemine ait kullanım yoktur.
- Fatura henüz düzenlenmemiştir. Fatura döneminin sonuna kadar bekleyin.
- Faturaları görüntüleme izniniz yoktur. Hesap Yöneticisi değilseniz eski faturaları göremezsiniz.
- Aboneliğinizde Ücretsiz Deneme sürümünüz veya aşmadığınız bir aylık kredi tutarınız varsa, Microsoft Müşteri Sözleşmeniz olmadığı sürece bir fatura almazsınız.

Sonraki adımda ücretleri gözden geçirin. Faturanızda vergiler ve kullanım ücretleriniz gösterilir.

![Azure Faturası Örneği](./media/review-individual-bill/invoice-usage-charge.png)

İndirdiğiniz CSV kullanım dosyasını açın. Dosyanın sonunda, *Maliyet* sütunundaki tüm satırların değerini toplayın.

![Toplam maliyetin gösterildiği örnek kullanım dosyası](./media/review-individual-bill/usage-file-usage-charges.png)

 Toplam *Maliyet* değeri, faturanızdaki *kullanım ücretleri* maliyeti ile tam olarak aynı olmalıdır.

Kullanım ücretleriniz ölçüm düzeyinde görüntülenir. Aşağıdaki terimler hem faturada hem de ayrıntılı kullanım dosyasında aynı anlama gelir. Örneğin faturadaki faturalama dönemi, ayrıntılı kullanım dosyasındaki faturalama dönemiyle aynıdır.

| Fatura (PDF) | Ayrıntılı kullanım (CSV)|
| --- | --- |
|Fatura döngüsü | BillingPeriodStartDate BillingPeriodEndDate |
|Adı |Ölçüm Kategorisi |
|Tür |Ölçüm Alt Kategorisi |
|Kaynak |MeterName |
|Bölge |MeterRegion |
|Kullanılan | Miktar |
|Dahil |Dahil Edilen Miktar |
|Faturalanabilir |Kapasite Aşım Miktarı |
|Fiyat | EffectivePrice|
| Değer | Maliyet |

Faturanızın **Kullanım Ücretleri** bölümü, faturalama döneminiz boyunca kullanılan her ölçümün toplam değerini (maliyet) gösterir. Örneğin, aşağıdaki resimde Azure Depolama hizmetinin *P10 Diskleri* kaynağı için kullanım ücreti gösterilir.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

CSV kullanım bilgileri dosyanıza *MeterName* filtresi uygulayarak faturanızda gösterilen kaynağa belirtin. Ardından sütundaki öğelerin *Maliyet* değerini toplayın. Buradaki örnek, faturada aynı satır öğesine karşılık gelen ölçüm adına (P10 diskleri) odaklanır.

![Kullanım dosyasının MeterName için toplam değeri](./media/review-individual-bill/usage-file-usage-charge-resource.png)

Özetlenen *Maliyet* değeri, faturanıza yansıtılan tek kaynağın *kullanım maliyetleri* ile aynı olmalıdır.

## <a name="compare-billed-charges-and-usage-in-cost-analysis"></a>Faturaya yansıtılan ücretleri ve maliyet analizindeki kullanımı karşılaştırma

Azure portalındaki maliyet analizi de ücretlerinizi doğrulamanıza yardımcı olabilir. Faturaya yansıtılan kullanım ve ücretlerle ilgili genel bakış için Azure portalındaki Abonelikler sayfasından aboneliğinizi seçin. Ardından **Maliyet analizi**'ne ve daha sonra görünüm listesinde **Fatura ayrıntıları**'na tıklayın.

![Fatura ayrıntıları seçimini gösteren örnek](./media/review-individual-bill/cost-analysis-select-invoice-details.png)

Ardından tarih aralığı listesinde faturanız için bir tarih aralığı belirtin. Fatura numarasını filtre olarak ekleyin ve faturanızla eşleşen InvoiceNumber değerini seçin. Maliyet analizi, faturalanmış öğelerinizin maliyet ayrıntılarını gösterir.

![Maliyet analizinde faturalanmış maliyet ayrıntılarını gösteren örnek](./media/review-individual-bill/cost-analysis-service-usage-charges.png)

Maliyet analizinde gösterilen maliyetler, faturanıza yansıtılan tek kaynağın *kullanım maliyetleri* ile aynı olmalıdır.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

## <a name="external-marketplace-services-are-billed-separately"></a>Dış Market hizmetleri ayrı faturalandırılır

<a name="external"></a>

Dış hizmetler veya market ücretleri, üçüncü taraf yazılımı satıcıları tarafından oluşturulan kaynaklar için geçerlidir. Bu kaynakları Azure Market’ten sağlayabilirsiniz. Örneğin Barracuda Güvenlik Duvarı, üçüncü tarafın kullanıma sunduğu bir Azure Market kaynağıdır. Güvenlik duvarının tüm ücretleri ve bunlara karşılık gelen ölçümler dış hizmet ücretleri olarak görünür.

Dış hizmetlerin ücretleri ayrı faturalanır. Ücretler Azure faturanızda gösterilmez.

### <a name="resources-are-billed-by-usage-meters"></a>Kaynaklar, kullanım ölçümlerine göre faturalandırılır

Azure doğrudan kaynak maliyetine göre faturalama yapmaz. Kaynağın ücretleri bir veya birden fazla ölçüm kullanılarak hesaplanır. Ölçümler, kaynağın yaşam süresi boyunca tüketilen kaynak kullanımını izlemek için kullanılır. Bu ölçümler daha sonra faturayı hesaplamak için kullanılır.

Sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda bu kaynağın bir veya birden fazla ölçüm örneği oluşturulur. Ölçümler, kaynağın zaman içinde kullanımını izlemek için kullanılır. Her ölçüm, Azure tarafından faturayı hesaplamak için kullanılan kullanım kayıtlarını üretir.

Örneğin, Azure’da oluşturulan tek bir sanal makinenin (VM) kullanımını izlemek için aşağıdaki ölçümler oluşturulmuş olabilir:

- İşlem Saatleri
- IP Adresi Saatleri
- Gelen Veri Aktarımı
- Giden Veri Aktarımı
- Standart Yönetilen Diskler
- Standart Yönetilen Disk İşlemleri
- Standart GÇ-Disk
- Standart GÇ-Blok Blobu Okuma
- Standart GÇ-Blok Blobu Yazma
- Standart GÇ-Blok Blobu Silme

VM oluşturulduğunda her ölçüm kullanım kayıtları üretmeye başlar. Bu kullanım ve ölçümün fiyatı Azure ölçüm sisteminde izlenir.

Önceki örnekte olduğu gibi, CSV biçimindeki kullanım dosyanızda faturanızın hesaplanması için kullanılan ölçümleri görebilirsiniz.

## <a name="pay-your-bill"></a>Faturanızı ödeme

<a name="payment"></a>

Ödeme yönteminiz olarak bir kredi kartı ayarladıysanız ödeme faturalama döneminin bitişinden sonraki 10 gün içinde otomatik olarak ücretlendirilir. Kredi kartı hesap özetinizde satır öğesi **MSFT Azure** olarak görünür.

Ücretlendirilen kredi kartını değiştirmek için bkz. [Azure’da kredi kartı ekleme, güncelleştirme veya kaldırma](../manage/change-credit-card.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Faturadaki ücretleri kullanım dosyasıyla karşılaştırma
> * Ücretleri ve kullanımı maliyet analiziyle karşılaştırma

Maliyet analizini kullanmaya başlamak için hızlı başlangıcı tamamlayın.

> [!div class="nextstepaction"]
> [Maliyet analizini başlatma](../costs/quick-acm-cost-analysis.md)
