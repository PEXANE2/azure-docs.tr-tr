---
title: Azure ayrılmış konakları için para tasarrufu sağlamak üzere ön ödeme
description: İşlem maliyetlerinizi kaydetmek için Azure ayrılmış konakları için ayrılmış örnekler satın almayı öğrenin.
services: virtual-machines
author: yashar
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: c3c4c4ea25a8f8057a5830ad2207bb674d9cc011
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501560"
---
# <a name="save-costs-with-azure-dedicated-host-reservations"></a>Azure ayrılmış ana bilgisayar ayırmaları ile maliyetleri kaydetme

Ayrılmış bir Azure ayrılmış ana bilgisayar örneğine kaydetme yaptığınızda para tasarrufu sağlayabilirsiniz. Ayırma indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen çalışan adanmış ana bilgisayar sayısına otomatik olarak uygulanır. İndirimleri almak için ayrılmış bir konağa bir ayırma atamanız gerekmez. Ayrılmış örnek satın alma yalnızca kullanımınızın işlem parçasını kapsar ve yazılım lisanslama maliyetlerini içerir. [Sanal makineler Için Azure adanmış konaklarına genel bakış](./windows/dedicated-hosts.md)bölümüne bakın.

## <a name="determine-the-right-dedicated-host-sku-before-you-buy"></a>Satın almadan önce doğru adanmış ana bilgisayar SKU 'sunu belirleme


Bir ayırma satın almadan önce, hangi adanmış ana bilgisayara ihtiyacınız olduğunu belirlemelisiniz. Bir SKU, VM serisini ve türünü temsil eden ayrılmış bir konak için tanımlanır. 

VM serisini belirlemek için [Windows sanal makinesi](./windows/sizes.md) veya [Linux](./linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) için desteklenen boyutlara giderek başlayın.

Ardından, Azure adanmış konaklarda desteklenip desteklenmediğini kontrol edin. [Azure adanmış konaklar fiyatlandırma](https://aka.ms/ADHPricing) sayfası, adanmış ana bilgisayar SKU 'LARıNıN, CPU bilgilerinin ve çeşitli fiyatlandırma seçeneklerinin (ayrılmış örnekler dahil) tüm listesini içerir.

VM serisini (farklı türlerle) destekleyen çeşitli SKU 'Lar bulabilirsiniz. Konağın kapasitesini (vCPU sayısı) karşılaştırarak en iyi SKU 'YU belirler. Aynı VM serisini (örneğin, DSv3_Type1 ve DSv3_Type2) destekleyen, ancak farklı VM serisine (DSv3 ve ESv3 gibi) sahip olan birden çok adanmış ana bilgisayar SKU 'Larına rezervasyon uygulayabileceğinizi unutmayın.



## <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması konuları

Ayrılmış örnekler, bazı özel durumlar dışında, çoğu adanmış konak boyutları için kullanılabilir.

Rezervasyon iskontoları şunlar için uygulanmaz:

- **Bulutlar**   -Rezervasyonlar Almanya veya Çin bölgelerinde satın alınabilir.

- **Yetersiz kota**   -Tek bir abonelik kapsamındaki bir ayırma, yeni ayrılmış örnek için abonelikte vCPU kotası kullanılabilir olmalıdır. Örneğin, hedef aboneliğin DSv3 serisi için 10 vCPU kota sınırı varsa, bu seriyi destekleyen bir ayırma adanmış ana bilgisayar satın alamazsınız. Rezervasyonların kota denetimi, abonelikte zaten dağıtılmış olan VM 'Leri ve ayrılmış Konakları içerir. Bu sorunu çözmek için [Kota artışı isteği oluşturabilirsiniz](../azure-portal/supportability/resource-manager-core-quotas-request.md)   .

- **Kapasite kısıtlamaları**   Nadir koşullarda Azure, bir bölgedeki düşük kapasiteden dolayı adanmış ana bilgisayar SKU 'Larının alt kümesi için yeni rezervasyonların satın alınmasını sınırlandırır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

 [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)Azure ayrılmış ana bilgisayar örneğinin ayrılmış bir örneğini satın alabilirsiniz.

Rezervasyon için [veya aylık ödemeler](../cost-management-billing/reservations/prepare-buy-reservation.md)için ödeme yapın. Bu gereksinimler ayrılmış bir ayrılmış ana bilgisayar örneği satın almak için geçerlidir:

- En az bir EA aboneliği veya Kullandıkça Öde tarifesine sahip bir abonelik için sahip rolünde olmanız gerekir.

- EA abonelikleri için, **ayrılmış örnekler Ekle**   seçeneğinin [EA portalında](https://ea.azure.com/)etkinleştirilmiş olması gerekir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.

- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetici aracıları veya satış aracıları ayırmaları satın alabilir.

Örnek satın almak için:

1.  [Azure Portal](https://portal.azure.com/)oturum açın.

2.  **Tüm hizmet**ayırmaları ' ni seçin   \>  **Reservations**.

3.  **Ekle**   ' yi seçerek yeni bir ayırma satın alın ve **ayrılmış konaklar**' a tıklayın.

4. Gerekli alanları doldurun. Ayırma indirimi elde etmek için, seçtiğiniz özniteliklerle eşleşen ayrılmış ana bilgisayar örneklerini çalıştırma. İskontoyu alan adanmış ana bilgisayar örneklerinizin gerçek sayısı, seçilen kapsama ve miktara göre değişir.

EA sözleşmeniz varsa, daha fazla örnek eklemek için **daha fazla ek ekle seçeneğini**kullanabilirsiniz   . Bu seçenek diğer abonelik türleri için kullanılamaz.

| **Alan**           | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abonelik        | Ayırma için ödeme yapmak üzere kullanılan abonelik. Abonelikteki ödeme yöntemi, rezervasyonun maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi ya da Kullandıkça Öde tarifesine sahip tek bir abonelik olmalıdır (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, varsa parasal taahhüt bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine sahip bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır. |
| Kapsam               | Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Bölge              | Ayırma kapsamındaki Azure bölgesi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Ayrılmış ana bilgisayar boyutu | Adanmış konak örneklerinin boyutu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Terim                | Bir yıl veya üç yıl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Miktar            | Ayırma içinde satın alınan örneklerin sayısı. Miktar, fatura iskontosunu elde eden, çalışan ayrılmış ana bilgisayar örneklerinin sayısıdır.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

- **Tek kaynak grubu kapsamı**   — Rezervasyon iskontosunu yalnızca seçili kaynak grubundaki eşleşen kaynaklara uygular.

- **Tek abonelik kapsamı**   — Rezervasyon iskontosunu seçili abonelikteki eşleşen kaynaklara uygular.

- **Paylaşılan kapsam**   — Fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. EA müşterileri için, faturalandırma bağlamı kayıt olur. Kullandıkça öde tarifelerine tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

## <a name="usage-data-and-reservation-utilization"></a>Kullanım verileri ve ayırma kullanımı

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Her rezervasyon için rezervasyon iskontosunu hangi VM örneğinin aldığını görebilirsiniz.

Kullanım verilerinde rezervasyon iskontolarının nasıl göründüğü hakkında daha fazla bilgi için bkz. EA müşterisiyseniz [Kurumsal kaydınız Için Azure ayırma kullanımını anlayın](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)   . Tek bir aboneliğiniz varsa, bkz. [Kullandıkça Öde aboneliğiniz Için Azure ayırma kullanımını anlayın](../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

- Rezervasyon kapsamını güncelleştirme

- Örnek boyutu esnekliği (varsa)

- Sahiplik

Ayrıca, bir ayırmayı daha küçük parçalara bölebilir ve zaten bölünmüş rezervasyonları birleştirebilirsiniz. Değişikliklerden hiçbiri yeni bir ticari işleme neden olmaz ya da ayırmanın bitiş tarihini değiştirmez.

Satın alma işleminden sonra aşağıdaki değişiklik türlerini yapamazsınız:

- Mevcut bir ayırmanın bölgesi

- SKU

- Miktar

- Süre

Ancak, *exchange*   değişiklik yapmak istiyorsanız bir rezervasyon alışverişi yapabilirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız  [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Bir ayırmayı yönetme hakkında bilgi edinmek için bkz. [Azure ayırmalarını yönetme](../cost-management-billing/reservations/manage-reserved-vm-instance.md).

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](../cost-management-billing/reservations/save-compute-costs-reservations.md)

- [Azure Ayrılmış Konaklarını kullanma](./windows/dedicated-hosts.md)

- [Ayrılmış Konaklar Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Azure’da Rezervasyonları Yönetme](../cost-management-billing/reservations/manage-reserved-vm-instance.md)

- [Rezervasyon indiriminin nasıl uygulandığını anlama](../cost-management-billing/manage/understand-vm-reservation-charges.md)

- [Kullandıkça öde fiyatlarına tabi bir abonelik için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage.md)

- [Kurumsal kaydınız için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)

- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](../cost-management-billing/reservations/reserved-instance-windows-software-costs.md)

- [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](/partner-center/azure-reservations)
