---
title: Azure VMware çözümü ayrılmış örneği ile maliyetleri kaydetme
description: Azure VMware çözümü için ayrılmış bir örnek satın almayı öğrenin.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: a477b1730d70ec729a2be333b545b6faeb009998
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91492494"
---
# <a name="save-costs-with-azure-vmware-solution"></a>Azure VMware çözümü ile maliyetleri kaydetme

Ayrılmış bir Azure VMware çözümü örneğine kaydetme yaptığınızda para tasarrufu sağlayabilirsiniz. Ayırma indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen çalışan Azure VMware Çözüm ana bilgisayarlarının sayısına otomatik olarak uygulanır. İndirimleri almak için ayrılmış bir konağa bir ayırma atamanız gerekmez. Ayrılmış örnek satın alma yalnızca kullanımınızın işlem parçasını kapsar ve yazılım lisanslama maliyetlerini içerir. Bkz. [Azure VMware çözümüne genel bakış](introduction.md).

## <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması konuları

Ayrılmış örnekler bazı özel durumlarla birlikte kullanılabilir.

-   **Bulutlar**   -Ayırmalar yalnızca [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) sayfasında listelenen bölgelerde kullanılabilir.

-   **Yetersiz kota**   -Tek/paylaşılan bir abonelik kapsamındaki bir ayırma, yeni ayrılmış örnek için abonelikte kullanılabilir Konaklar kotasına sahip olmalıdır. Bu sorunu çözmek için [Kota artışı isteği oluşturabilirsiniz](enable-azure-vmware-solution.md) .

-   **Teklif uygunluğu**-Microsoft Ile bir [Azure Kurumsal Anlaşma (EA)](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements)gerekir   .

-   **Kapasite kısıtlamaları**   Nadir koşullarda Azure, bir bölgedeki düşük kapasiteden dolayı Azure VMware Çözüm Konağı SKU 'Ları için yeni ayırmalar satın almayı sınırlandırır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

Azure VMware Çözüm ana bilgisayar örneğinin ayrılmış bir örneğini [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)satın alabilirsiniz.

Rezervasyon için [veya aylık ödemeler](../cost-management-billing/reservations/prepare-buy-reservation.md)için ödeme yapın.

Bu gereksinimler ayrılmış bir ayrılmış ana bilgisayar örneği satın almak için geçerlidir:

-   En az bir EA aboneliği veya Kullandıkça Öde tarifesine sahip bir abonelik için sahip rolünde olmanız gerekir.

-   EA abonelikleri için, **ayrılmış örnekler Ekle**   seçeneğinin [EA portalında](https://ea.azure.com/)etkinleştirilmiş olması gerekir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.

Örnek satın almak için:

1.  [Azure Portal](https://portal.azure.com/)oturum açın.

2.  **Tüm hizmet**ayırmaları ' ni seçin   >  **Reservations**.

3.  **Ekle**   ' yi seçerek yeni bir ayırma satın alın ve ardından **Azure VMware çözümü**' ni seçin.

4. Gerekli alanları doldurun. Ayırma iskontosunu almak için, seçtiğiniz özniteliklerle eşleşen Azure VMware Çözüm Konakları çalıştırılıyor. İskontoyu alan Azure VMware Çözüm ana bilgisayarlarının gerçek sayısı, seçilen kapsama ve miktara göre değişir.

   EA sözleşmeniz varsa, daha fazla örnek eklemek için **daha fazla ek ekle seçeneğini**kullanabilirsiniz   . Bu seçenek diğer abonelik türleri için kullanılamaz.

   | Alan        |  Açıklama |
   | ------------ | ------------ |
   | Abonelik | Ayırma için ödeme yapmak üzere kullanılan abonelik. Rezervasyon maliyetleri, aboneliğin ödeme yöntemiyle ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi ya da Kullandıkça Öde tarifesine sahip tek bir abonelik olmalıdır (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, parasal taahhüt bakiyesinden düşülür veya varsa fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine sahip bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır. |
   | Kapsam        | Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz:<br><ul><li><b>Tek kaynak grubu kapsamı — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.</li><li><b>Tek abonelik kapsamı — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.</li><li><b>Paylaşılan kapsam — fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. EA müşterileri için, faturalandırma bağlamı kayıt olur. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.</li></ul>       |
   | Bölge       | Ayırma kapsamındaki Azure bölgesi.   |
   | Ana bilgisayar boyutu    | AV36    |
   | Süre         | Bir yıl veya üç yıl.  |
   | Miktar     | Ayırma içinde satın alınan örneklerin sayısı. Bu miktar, fatura iskontosunu elde eden, çalışan Azure VMware Çözüm ana bilgisayarlarının sayısıdır.    |

## <a name="usage-data-and-reservation-utilization"></a>Kullanım verileri ve ayırma kullanımı

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Hangi Azure VMware Çözüm örneğinin her rezervasyon için rezervasyon iskontosunu aldığını görebilirsiniz.

Rezervasyon iskontolarının kullanım verilerinde nasıl göründüğü hakkında daha fazla bilgi edinmek ve bir EA müşterisiyseniz, bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlayın](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md). Tek bir aboneliğiniz varsa, bkz. [Kullandıkça Öde aboneliğiniz Için Azure ayırma kullanımını anlayın](../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

-   Rezervasyon kapsamını güncelleştirme

-   Örnek boyutu esnekliği (varsa)

-   Sahiplik

Ayrıca, bir ayırmayı daha küçük parçalara bölebilir veya rezervasyonları birleştirebilirsiniz. Değişikliklerden hiçbiri yeni bir ticari işleme neden olmaz ya da ayırmanın bitiş tarihini değiştirmez.

>[!NOTE]
>Ayırmayı satın aldıktan sonra, aşağıdaki değişiklik türlerini doğrudan yapamazsınız:
>
> - Mevcut bir ayırmanın bölgesi
> - SKU
> - Miktar
> - Süre
>
>Ancak, *exchange*   değişiklik yapmak istiyorsanız bir rezervasyon alışverişi yapabilirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).
