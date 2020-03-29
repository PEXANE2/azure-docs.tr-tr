---
title: Tasarruf etmek için Azure Özel Ana Bilgisayarlar için ön ödeme
description: İşlem maliyetlerinizden tasarruf etmek için Azure'a Özel Ana Bilgisayar Ları Ayrılmış Örnekleri nasıl satın alacağınızı öğrenin.
services: virtual-machines
author: yashar
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: 57123abfe7f343a75d264d43afb88f9de1409e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78207751"
---
# <a name="save-costs-with-a-reserved-instance-of-azure-dedicated-hosts"></a>Azure Özel Ana Bilgisayarlarının Ayrılmış Örneği yle maliyetlerden tasarruf edin

Azure Özel Ana Bilgisayarların ayrılmış bir örneğine bağlandığınızda, paradan tasarruf edebilirsiniz. Rezervasyon indirimi, rezervasyon kapsamı ve öznitelikleriyle eşleşen çalışan özel ana bilgisayar sayısına otomatik olarak uygulanır. İndirimleri almak için özel bir ev sahibine rezervasyon yapmanız gerekmez. Ayrılmış bir örnek satın alma, yalnızca kullanımınızın bilgi işlem kısmını kapsar ve yazılım lisanslama maliyetlerini içerir. Sanal [makineler için Azure Özel Ana Bilgisayarlarına Genel Bakış'a](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)bakın.

## <a name="determine-the-right-dedicated-host-sku-before-you-buy"></a>Satın almadan önce doğru özel ana bilgisayar SKU'yu belirleyin


Rezervasyon satın almadan önce, ihtiyacınız olan özel ana bilgisayara sahip olduğunuza karar verilmelidir. VM serisini ve türünü temsil eden özel bir ana bilgisayar için SKU tanımlanır. 

VM serisini tanımlamak için [Windows sanal makinesi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) için desteklenen boyutları n üzerinden geçirerek başlayın.

Ardından, Azure Özel Ana Bilgisayarlarda desteklenip desteklenmediğini kontrol edin. [Azure Özel Ana Bilgisayar ları fiyatlandırma](https://aka.ms/ADHPricing) sayfasında özel ana bilgisayarların tam listesi, CPU bilgileri ve çeşitli fiyatlandırma seçenekleri (ayrılmış örnekler dahil).

VM serisini destekleyen birkaç SUS bulabilirsiniz (farklı Türleri ile). Ana bilgisayar kapasitesini (vCPUs sayısı) karşılaştırarak en iyi SKU'yu tanımlayın. Aynı VM serisini destekleyen birden fazla özel ana bilgisayar adabına (örneğin DSv3_Type1 ve DSv3_Type2) rezervasyon uygulayabileceğinizi, ancak farklı VM serilerinde (DSv3 ve ESv3 gibi) rezervasyon uygulayabileceğinizi unutmayın.



## <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması hususları

Ayrılmış örnekler, bazı istisnalar dışında, en özel ana bilgisayar boyutları için kullanılabilir.

Rezervasyon indirimleri aşağıdakiler için geçerli değildir:

- **Bulutlar** - Rezervasyonlar Almanya veya Çin bölgelerinde satın alınamaz.

- **Yetersiz kota** - Tek bir aboneliği kapsamaalan bir rezervasyon, yeni ayrılmış örnek için abonelikte vCPU kotası olmalıdır. Örneğin, hedef aboneliğin DSv3 Serisi için 10 vCPUs kota sınırı varsa, bu seriyi destekleyen rezervasyona özel ana bilgisayarları satın alamazsınız. Rezervasyonlar için kota denetimi, abonelikte zaten dağıtılan VM'leri ve özel ana bilgisayarlarını içerir. Bu sorunu gidermek için [kota artış isteği](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) oluşturabilirsiniz.

- **Kapasite kısıtlamaları** - Nadir durumlarda Azure, bir bölgedeki düşük kapasite nedeniyle özel ana bilgisayar SUK'larının alt kümesi için yeni rezervasyonsatın almayı sınırlar.

## <a name="buy-a-reservation"></a>Ayırma satın alma

 [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)ayrılmış bir Azure Özel Ana Bilgisayar örneği satın alabilirsiniz.

Rezervasyon için [peşin veya aylık ödemelerle](https://docs.microsoft.com/azure/billing/billing-monthly-payments-reservations)ödeme. Bu gereksinimler ayrılmış bir Ayrılmış Ana Bilgisayar örneği satın almak için geçerlidir:

- En az bir EA aboneliği veya istediğiniz kadar öde fiyatına sahip bir abonelik için Sahip rolünde olmalısınız.

- EA abonelikleri için **Ayrılmış Örnekler Ekle** seçeneği, [EA portalında](https://ea.azure.com/)etkinleştirilmelidir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.

- Bulut Çözüm Sağlayıcısı (CSP) programı için rezervasyonları yalnızca yönetici aracılar veya satış temsilcileri satın alabilir.

Bir örneği satın almak için:

1.  [Azure portalında](https://portal.azure.com/)oturum açın.

2.  **Tüm hizmetler** \> **rezervasyonlarını**seçin.

3. Yeni bir rezervasyon satın almak için **Ekle'yi** seçin ve ardından **Özel Ana Bilgisayarlar'ı**tıklatın.

4. Gerekli alanları doldurun. Seçtiğiniz özniteliklerle eşleşen Özel Ana Bilgisayar örnekleri nin çalıştırılması rezervasyon indirimini almaya hak kazanır. İndirim alan Özel Ana Bilgisayar örneklerinizin gerçek sayısı, seçilen kapsam ve miktara bağlıdır.

Bir EA anlaşmanız varsa, hızlı bir şekilde ek örnekler eklemek için **daha fazla ekle seçeneğini** kullanabilirsiniz. Bu seçenek diğer abonelik türleri için kullanılamaz.

| **Alan**           | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abonelik        | Abonelik rezervasyon için ödeme yapmak için kullanılır. Abonelikteki ödeme yöntemi, rezervasyon masraflarından tahsil edilir. Abonelik türü bir kurumsal sözleşme olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi veya kullandıkça öde oranlarına sahip bireysel bir abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, varsa parasal taahhüt bakiyesinden düşülür veya fazlalık olarak tahsil edilir. Kullandıkça öde oranlarına sahip bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır. |
| Kapsam               | Rezervasyonun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsam) kapsayabilir. Seçerseniz:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Bölge              | Rezervasyon kapsamındaki Azure bölgesi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Özel Ana Bilgisayar Boyutu | Adanmış Ana Bilgisayar örneklerinin boyutu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Sözleşme Dönemi                | Bir ya da üç yıl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Miktar            | Rezervasyon içinde satın alınan örnek sayısı. Miktar, faturalandırma iskontosu alabilen çalışan Adanmış Ana Bilgisayar örneklerinin sayısıdır.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

- **Tek kaynak grubu kapsamı** — Rezervasyon iskontosu yalnızca seçili kaynak grubundaki eşleşen kaynaklara uygulanır.

- **Tek abonelik kapsamı** — Seçilen abonelikteki eşleşen kaynaklara rezervasyon indirimi uygulanır.

- **Paylaşılan kapsam** — Rezervasyon indirimi, faturalandırma bağlamında uygun aboneliklerde eşleşen kaynaklara uygulanır. EA müşterileri için faturalandırma bağlamı kayıttır. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

## <a name="usage-data-and-reservation-utilization"></a>Kullanım verileri ve rezervasyon kullanımı

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Her rezervasyon için hangi VM örneğinin rezervasyon indirimialdığını görebilirsiniz.

Kullanım verilerinde rezervasyon indirimlerinin nasıl göründüğü hakkında daha fazla bilgi için, EA müşterisiyseniz Kurumsal  [kaydınız için Azure rezervasyon kullanımını anlayın'](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)a bakın. Tek bir aboneliğiniz varsa, [Kullan-Öde aboneliğiniz için Azure rezervasyon kullanımını anlayın'](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)a bakın.

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

- Rezervasyon kapsamını güncelleştirme

- Örnek boyutu esnekliği (varsa)

- Sahiplik

Ayrıca, bir rezervasyonu daha küçük parçalara bölebilir ve zaten bölünmüş rezervasyonları birleştirebilirsiniz. Değişikliklerin hiçbiri yeni bir ticari işlem yapılmasına veya rezervasyonun bitiş tarihini değiştirmeme neden olmaz.

Satın alma işleminden sonra aşağıdaki değişiklik türlerini doğrudan yapamazsınız:

- Mevcut bir rezervasyonbölgesi

- SKU

- Miktar

- Süre

Ancak, değişiklik yapmak isterseniz rezervasyon *değiştirebilirsiniz.* 

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için Azure [Rezervasyonları için Self servis değişimlerine ve geri ödeme lere](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund)bakın.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonu nasıl yöneteceklerini öğrenmek için Azure [Rezervasyonlarını Yönet'e](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)bakın.

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Rezervasyonları nedir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)

- [Azure'a Özel Ana Bilgisayar kullanma](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)

- [Özel Ev Sahipleri Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Azure’da Rezervasyonları Yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)

- [Rezervasyon indiriminin nasıl uygulandığını anlama](https://docs.microsoft.com/azure/billing/billing-understand-vm-reservation-charges)

- [Kullandıkça öde fiyatlarına tabi bir abonelik için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)

- [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)

- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](https://docs.microsoft.com/azure/billing/billing-reserved-instance-windows-software-costs)

- [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)


