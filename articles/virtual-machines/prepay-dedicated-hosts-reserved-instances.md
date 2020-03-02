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
ms.openlocfilehash: 57123abfe7f343a75d264d43afb88f9de1409e8a
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78207751"
---
# <a name="save-costs-with-a-reserved-instance-of-azure-dedicated-hosts"></a>Ayrılmış bir Azure ayrılmış ana bilgisayar örneğiyle maliyetleri kaydetme

Ayrılmış bir Azure ayrılmış ana bilgisayar örneğine kaydetme yaptığınızda para tasarrufu sağlayabilirsiniz. Ayırma indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen çalışan adanmış ana bilgisayar sayısına otomatik olarak uygulanır. İndirimleri almak için ayrılmış bir konağa bir ayırma atamanız gerekmez. Ayrılmış örnek satın alma yalnızca kullanımınızın işlem parçasını kapsar ve yazılım lisanslama maliyetlerini içerir. [Sanal makineler Için Azure adanmış konaklarına genel bakış](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)bölümüne bakın.

## <a name="determine-the-right-dedicated-host-sku-before-you-buy"></a>Satın almadan önce doğru adanmış ana bilgisayar SKU 'sunu belirleme


Bir ayırma satın almadan önce, hangi adanmış ana bilgisayara ihtiyacınız olduğunu belirlemelisiniz. Bir SKU, VM serisini ve türünü temsil eden ayrılmış bir konak için tanımlanır. 

VM serisini belirlemek için [Windows sanal makinesi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) için desteklenen boyutlara giderek başlayın.

Ardından, Azure adanmış konaklarda desteklenip desteklenmediğini kontrol edin. [Azure adanmış konaklar fiyatlandırma](https://aka.ms/ADHPricing) sayfası, adanmış ana bilgisayar SKU 'LARıNıN, CPU bilgilerinin ve çeşitli fiyatlandırma seçeneklerinin (ayrılmış örnekler dahil) tüm listesini içerir.

VM serisini (farklı türlerle) destekleyen çeşitli SKU 'Lar bulabilirsiniz. Konağın kapasitesini (vCPU sayısı) karşılaştırarak en iyi SKU 'YU belirler. Aynı VM serisini (örneğin, DSv3_Type1 ve DSv3_Type2) destekleyen, ancak farklı VM serisine (DSv3 ve ESv3 gibi) sahip olan birden çok adanmış ana bilgisayar SKU 'Larına rezervasyon uygulayabileceğinizi unutmayın.



## <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması konuları

Ayrılmış örnekler, bazı özel durumlar dışında, çoğu adanmış konak boyutları için kullanılabilir.

Rezervasyon iskontoları şunlar için uygulanmaz:

- **Bulutlar** -rezervasyonlar Almanya veya Çin bölgelerinde satın alınabilir.

- **Yetersiz kota** -tek bir abonelik kapsamındaki bir ayırma, yeni ayrılmış örnek Için abonelikte vCPU kotası kullanılabilir olmalıdır. Örneğin, hedef aboneliğin DSv3 serisi için 10 vCPU kota sınırı varsa, bu seriyi destekleyen bir ayırma adanmış ana bilgisayar satın alamazsınız. Rezervasyonların kota denetimi, abonelikte zaten dağıtılmış olan VM 'Leri ve ayrılmış Konakları içerir. Bu sorunu çözmek için [Kota artışı isteği oluşturabilirsiniz](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) .

- **Kapasite kısıtlamaları** -nadir koşullarda Azure, bir bölgedeki düşük kapasiteden dolayı adanmış ana bilgisayar SKU 'larının alt kümesi için yeni rezervasyonların satın alınmasını sınırlandırır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

 [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)Azure ayrılmış ana bilgisayar örneğinin ayrılmış bir örneğini satın alabilirsiniz.

Rezervasyon için [veya aylık ödemeler](https://docs.microsoft.com/azure/billing/billing-monthly-payments-reservations)için ödeme yapın. Bu gereksinimler ayrılmış bir ayrılmış ana bilgisayar örneği satın almak için geçerlidir:

- En az bir EA aboneliği veya Kullandıkça Öde tarifesine sahip bir abonelik için sahip rolünde olmanız gerekir.

- EA abonelikleri için, [EA portalında](https://ea.azure.com/), **ayrılmış örnekler Ekle** seçeneğinin etkinleştirilmiş olması gerekir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.

- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetici aracıları veya satış aracıları ayırmaları satın alabilir.

Örnek satın almak için:

1.  [Azure portalda](https://portal.azure.com/) oturum açın.

2.  **Tüm hizmetler** \> **ayırmaları** seçin.

3. Yeni bir ayırma satın almak için  **Ekle** öğesini seçin ve ardından **adanmış konaklar**' a tıklayın.

4. Gerekli alanları doldurun. Ayırma indirimi elde etmek için, seçtiğiniz özniteliklerle eşleşen ayrılmış ana bilgisayar örneklerini çalıştırma. İskontoyu alan adanmış ana bilgisayar örneklerinizin gerçek sayısı, seçilen kapsama ve miktara göre değişir.

EA anlaşmanız varsa, daha fazla örnek eklemek için  **Ekle seçeneğini** kullanabilirsiniz. Bu seçenek diğer abonelik türleri için kullanılamaz.

| **Alan**           | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abonelik        | Ayırma için ödeme yapmak üzere kullanılan abonelik. Abonelikteki ödeme yöntemi, rezervasyonun maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi ya da Kullandıkça Öde tarifesine sahip tek bir abonelik olmalıdır (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, varsa parasal taahhüt bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine sahip bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır. |
| Kapsam               | Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Bölge              | Ayırma kapsamındaki Azure bölgesi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Ayrılmış ana bilgisayar boyutu | Adanmış konak örneklerinin boyutu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Süre                | Bir yıl veya üç yıl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Miktar            | Ayırma içinde satın alınan örneklerin sayısı. Miktar, fatura iskontosunu elde eden, çalışan ayrılmış ana bilgisayar örneklerinin sayısıdır.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

- **Tek kaynak grubu kapsamı** — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.

- **Tek abonelik kapsamı** — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.

-  **paylaşılan kapsam** —, fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. EA müşterileri için, faturalandırma bağlamı kayıt olur. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

## <a name="usage-data-and-reservation-utilization"></a>Kullanım verileri ve ayırma kullanımı

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Her rezervasyon için rezervasyon iskontosunu hangi VM örneğinin aldığını görebilirsiniz.

Kullanım verilerinde rezervasyon iskontolarının nasıl göründüğü hakkında daha fazla bilgi için bkz. EA müşterisiyseniz, [Kurumsal kaydınız Için Azure ayırma kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) . Tek bir aboneliğiniz varsa, bkz. [Kullandıkça Öde aboneliğiniz Için Azure ayırma kullanımını anlayın](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

- Rezervasyon kapsamını güncelleştirme

- Örnek boyutu esnekliği (varsa)

- Fazlasının

Ayrıca, bir ayırmayı daha küçük parçalara bölebilir ve zaten bölünmüş rezervasyonları birleştirebilirsiniz. Değişikliklerden hiçbiri yeni bir ticari işleme neden olmaz ya da ayırmanın bitiş tarihini değiştirmez.

Satın alma işleminden sonra aşağıdaki değişiklik türlerini yapamazsınız:

- Mevcut bir ayırmanın bölgesi

- SKU

- Miktar

- Süre

Ancak, değişiklik yapmak istiyorsanız, bir rezervasyon  *Exchange* gönderebilirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Bir ayırmayı yönetme hakkında bilgi edinmek için bkz. [Azure ayırmalarını yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance).

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)

- [Azure ayrılmış Konakları kullanma](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)

- [Adanmış konaklar fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Azure’da Rezervasyonları Yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)

- [Rezervasyon indiriminin nasıl uygulandığını anlama](https://docs.microsoft.com/azure/billing/billing-understand-vm-reservation-charges)

- [Kullandıkça öde fiyatlarına tabi bir abonelik için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)

- [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)

- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](https://docs.microsoft.com/azure/billing/billing-reserved-instance-windows-software-costs)

- [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)


