---
title: Ölçülen faturalandırma için anomali algılama hizmeti-Microsoft Azure Market
description: Anomali algılamanın nasıl çalıştığını, bildirimlerin ne zaman gönderileceğini ve bunlarla ne yapılacağını açıklar ve destek seçenekleri.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: anbene
ms.author: mingshen
ms.date: 06/10/2020
ms.openlocfilehash: becd15ceea41e40b35848f46f9657c501acf659a
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122039"
---
# <a name="anomaly-detection-service-for-metered-billing"></a>Ölçülen faturalandırma için anomali algılama hizmeti

[Market ölçüm hizmeti](marketplace-metering-service-apis-faq.md) , ticari Market programında standart olmayan birimlere göre ücretlendirilen teklifler oluşturmanıza imkan tanır. Tarifeli faturalandırma sayesinde, müşterinizin kullanımı için kullanım olaylarını Microsoft 'a gönderirsiniz ve bu kullanımı temel alarak faturalandırmayı hazırlarız.

Hatalı kullanım verileri, hata, tüketim izlemeinizdeki yanlış yapılandırma veya sahtekarlık gibi çeşitli nedenlerden gelebilir. Hatalı kullanım verileri, müşteri ücretleri ve faturalandırma anlaşmazlıklarına neden olur.

Anomali algılama hizmeti, riski azaltmak için, normal ölçülen faturalandırma davranışını saptamak, ölçülen faturalandırma kullanımını çözümlemek ve en az Kullanıcı müdahalesiyle bozukluklar bulmalarına yönelik makine öğrenimi algoritmalarını uygular.

Ölçümlü faturalandırma kullanımınız için bir anomali algılandığında bilgilendirilirsiniz. Bu, bir anomali gerçek bir sorun olup olmadığını araştırırken bize bildirimde bulunur ve bu durum, müşteri faturalandırma sorununa karşı bir süre önce ele alınması gereken noktada, bu durum üzerinde işlem yapılabilir.

Ölçülen faturalandırmalar, DIB 'ler ve tarifeli faturalama kullanımının eğilimi değişikliklerine ek olarak modelimiz de mevsimsel etkilere yönelik hesaplardır. Ölçülen faturalandırma fazla kullanım verileri aracılığıyla iletileceği için, modelimiz eksik verileri uzun süreler da düzgün bir şekilde işleyebilir.

Aşağıda anomali algılama sonuçlarının örnekleri verilmiştir. Beklenen Aralık, sarı bir bant olarak gösterilir. Kabul edilebilir tarifeli faturalandırma kullanımı, bantta yeşil yıldız olarak gösterilir. Bant dışındaki faturalandırma kullanımı kırmızı nokta olarak görüntülenir.  

Öngörülebilir bir eğilim dışında algılanan bozukluklar:

![Öngörülebilir bir eğilim dışında algılanan bozukluklar gösterilmektedir.](media/anomaly-1.png)

Yinelenen bir döngüsel eğilim dışında algılanan bozukluklar:

![Yinelenen bir döngüsel eğilim dışında algılanan bozukluklar gösterilmektedir.](media/anomaly-2.png)

Yukarı doğru bir eğilim halinde algılanan bozukluklar:

![, Yukarı doğru bir eğilime göre algılanan bozukluklar gösterilmektedir.](media/anomaly-3.png)

## <a name="how-anomaly-detection-service-works"></a>Anomali algılama hizmeti nasıl kullanılır

Anomali algılama, ölçülen tüm faturalandırma kullanımı için otomatik olarak etkinleştirilir. Kullanım olaylarını Microsoft 'a gönderdiğinizde anomali algılama hizmeti, geçmiş kullanım verilerine göre beklenen değerler modeli oluşturur. Bu model haftalık olarak çalışır.

Anomali algılama işlevleri, ölçüm ve müşteri başına düzeyindedir. Bu, her müşteriye ait her bir ölçerin bu ölçerin bu müşterinin son kullanım düzenine göre eğitilen bir modele sahip olacağı anlamına gelir.

Model, geriye dönük güven aralıkları oluşturarak işe yarar. Zaman serisi tahmini, bir eğilim tahmin parçasını ve mevsimsellik parçasını içeren genelleştirilmiş bir eklenebilir modeldir. Model bir gerileme görevi olarak formüllenmiş olduğundan, uzun süreler eksik verileri düzgün bir şekilde işleyebilir. Bir gözlemlik, tahmin edilen güven aralıklarının dışında kalırsa, gözlemlenen faturalandırmayla ilgili geçmiş desenlerine göre açıklanamayan ve bu nedenle bir anomali olabilir.

## <a name="anomaly-detection-notification"></a>Anomali algılama bildirimi

Anomali algılama bildirimlerini haftalık olarak e-posta ile sağlıyoruz. Tüm ölçerler ve müşteriler için bu hafta algılanan tüm anormallikleri içerir. Bu e-posta, teklifi oluştururken belirtilen **mühendislik** ve **destek** kişilerine gönderilir.

Algılanan anormalilerin gerçek sorunlar olup olmadığını araştırmanız beklenmektedir ve bu durumda, yanlış kullanımı bildirmek için Microsoft 'a başvurun (aşağıdaki destek bölümüne bakın).

Algılanan anormalilerin normal kullanım olduğunu onaylayıp, başka bir işlem yapmanız gerekmez. Ancak, bir anomali potansiyel olarak yüksek kaliteli riski gösteriyorsa, kullanımı onaylamak için sizinle iletişim kurabiliriz.  

## <a name="when-and-how-to-get-support"></a>Ne zaman ve nasıl destek alınır?

Microsoft 'a yanlış kullanım yaptıysanız ve bu veya müşteri üzerinden ücretlendiriliyorsa, Microsoft, müşteri tarafından bildirilen kullanım için bir fatura başlatmaz veya size bu kullanım için ödeme yapmayacaktır. Raporlama altında olması nedeniyle gelir kaybını da bilmeniz gerekir.

Aşağıdaki durumlardan biri geçerliyse, müşterileriniz için para iadesi veya faturalama ayarlaması istemek üzere bir destek bileti açabilirsiniz:

- Bulduğumuz anormalilerin gerçek bir sorun olduğunu doğrulamıştır ve yanlış kullanım müşterinin **fazla ücretlendirmesinden** kaynaklanır.
- Bizim için yanlış kullanım gönderdik ve yanlış kullanım müşterinin **fazla ödenmesine** neden olur.
- Müşterinizin tarifeli faturalandırma kullanımının ücreti için bir para iadesi istemek istiyorsunuz.

Bilet göndermek için:

1. Destek sayfasına gidin. **Sorununuzu bize bildirin kutusunda**"yanlış kullanım" yazın.
2. Destek konularında, arama sonuçlarının açılan kutusunda aşağıdakilerden birini seçin:
    - **Ticari Market**  >  **Ölçülen faturalandırma**  >  **Azure uygulamaları teklifi Için yanlış kullanım gönderildi**veya
    - **Ticari Market**  >  **Ölçülen faturalandırma**  >  **SaaS teklifi Için yanlış kullanım gönderildi**
3. Bir **sonraki adımda**, bir destek bileti göndermek Için Iş Ortağı Merkezi 'nde oturum açmak üzere yönlendirilmek üzere **çözüm gözden geçir** düğmesini seçin.

Daha fazla yayımcı desteği seçeneği için bkz. [Iş Ortağı Merkezi 'nde ticari Market programı desteği](support.md).

## <a name="next-step"></a>Sonraki adım

- [Market ölçüm hizmeti API 'si](marketplace-metering-service-apis.md)hakkında bilgi edinin.
