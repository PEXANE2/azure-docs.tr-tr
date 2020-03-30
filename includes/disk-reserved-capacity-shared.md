---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: cb959b94807678187363d3132ece273584f13a0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77590737"
---
Azure Disk Depolama kullanımınızdan ayrılmış kapasiteyle kaydedin. Azure Disk Depolama rezervasyonları ve Azure Ayrılmış Sanal Makine Örnekleri, toplam sanal makine (VM) maliyetlerinizi düşürmenize sağlar. Rezervasyon indirimi, seçili rezervasyon kapsamındaki eşleşen disklere otomatik olarak uygulanır. Bu otomatik uygulama nedeniyle, indirimleri almak için yönetilen bir diske rezervasyon atamanız gerekmez.

İndirimler disk kullanımına bağlı olarak saatlik olarak uygulanır. Kullanılmayan ayrılmış kapasite üzerinde taşımaz. Azure Disk Depolama rezervasyon indirimleri yönetilmeyen diskler, ultra diskler veya sayfa blob tüketimi için geçerli değildir.

## <a name="determine-your-storage-needs"></a>Depolama ihtiyaçlarınızı belirleme

Rezervasyon satın almadan önce depolama ihtiyaçlarınızı belirleyin. Şu anda Azure Disk Depolama rezervasyonları yalnızca belirli Azure premium SSD SK'leri için kullanılabilir. Premium SSD'nin SKU'su diskin boyutunu ve performansını belirler.

Depolama gereksinimlerinizi belirlerken, diskleri sadece kapasiteye göre düşünmeyin. Örneğin, bir P40 disk için rezervasyon yaptıramaz ve bunu iki küçük P30 diski için ödeme yapmak için kullanamazsınız. Rezervasyon satın alırken, yalnızca SKU başına toplam disk sayısı için rezervasyon satın alabilirsiniz.

Disk SKU başına bir disk rezervasyonu yapılır. Sonuç olarak, rezervasyon tüketimi sağlanan boyut yerine disk SUS birimine dayanır.

Örneğin, 2 TiB'lik koruma depolama kapasitesine sahip bir P40 diski rezerve ettiğinizi varsayalım. Ayrıca yalnızca iki P30 disk ayırdığınızı varsayalım. Bu durumda P40 rezervasyon P30 tüketimini hesaba katmaz ve P30 disklerde istediğiniz kadar öde oranını ödin.
<br/>
<br/>
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Satın alma hususları

Disk rezervasyonu satın alma işlemini değerlendirirken aşağıdaki uygulamaları öneririz:

- Hangi rezervasyonları satın almanız gerektiğini belirlemeye yardımcı olmak için kullanım bilgilerinizi analiz edin. Verilen veya kullanılan disk kapasitesi yerine disk STU'larında kullanımı izlediğinden emin olun.
- VM rezervasyonunuzla birlikte disk rezervasyonunuzu inceleyin. Maksimum tasarruf için hem VM kullanımı hem de disk kullanımı için rezervasyon yapmanızı şiddetle öneririz. Doğru VM rezervasyonuna başlayabilir ve ardından disk rezervasyonundan değerlendirilmesi yapabilirsiniz. Genellikle, iş yüklerinizin her biri için standart bir yapılandırmanız olur. Örneğin, bir SQL Server sunucusunda iki P40 veri diski ve bir P30 işletim sistemi diski olabilir.
  
  Bu tür bir desen, satın alabileceğiniz ayrılmış tutarı belirlemenize yardımcı olabilir. Bu yaklaşım, değerlendirme işlemini basitleştirebilir ve hem VM'leriniz hem de diskleriniz için hizalanmış bir planınız olduğundan emin olabilir. Plan, abonelikler veya bölgeler gibi hususları içerir.

## <a name="purchase-restrictions"></a>Satın alma kısıtlamaları

Rezervasyon indirimleri şu anda aşağıdakiler için kullanılamıyor:

- Yönetilmeyen diskler veya sayfa lekeleri.
- Standart SSD'ler veya standart sabit disk sürücüleri (HDD'ler).
- P30'dan küçük Premium SSD SK'lar: P1, P2, P3, P4, P6, P10, P15 ve P20 SSD SKUs.
- Azure Devlet, Azure Almanya veya Azure Çin bölgelerindeki diskler.

Azure, nadir durumlarda, bir bölgedeki düşük kapasite nedeniyle yeni rezervasyon satın almayı disk SUK'larının bir alt kümesiyle sınırlandırmaktadır.

## <a name="buy-a-disk-reservation"></a>Disk rezervasyonu satın alın

Azure Disk Depolama rezervasyonlarını [Azure portalı](https://portal.azure.com/)üzerinden satın alabilirsiniz. Rezervasyon ücretini peşin veya aylık ödemelerle ödeyebilirsiniz. Aylık ödemelerle satın alma hakkında daha fazla bilgi için aylık [ödemelerle satın alma rezervasyonlarına](../articles/cost-management-billing/reservations/monthly-payments-reservations.md)bakın.

Ayrılmış kapasite satın almak için aşağıdaki adımları izleyin:

1. Azure portalındaki [Satınalma rezervasyonları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) bölmesine gidin.

1. Bir rezervasyon satın almak için **Azure Yönetilen Diskler'i** seçin.

    ![Rezervasyon satın alma için bölme](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png) 

1. Aşağıdaki tabloda açıklanan gerekli değerleri belirtin:

   |Öğe  |Açıklama  |
   |---------|---------|
   |**Kapsam**   |  Rezervasyonla ilişkili fatura avantajını kaç abonelik kullanabilir. Bu değer, rezervasyonun belirli aboneliklere nasıl uygulandığını da belirtir. <br/><br/> **Paylaşılan'u**seçerseniz, rezervasyon indirimi faturalandırma bağlamınızdaki her abonelikte Azure Depolama kapasitesine uygulanır. Faturalandırma bağlamı, Azure'a nasıl kaydolduğunuza bağlıdır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan kullandıkça öde oranlarına sahip tüm bireysel abonelikleri içerir.  <br/><br/>  **Tek abonelik**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Depolama kapasitesine uygulanır. <br/><br/> **Tek kaynak grubu**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Depolama kapasitesine ve bu aboneliğin seçili kaynak grubuna uygulanır. <br/><br/> Rezervasyonu satın aldıktan sonra rezervasyon kapsamını değiştirebilirsiniz.  |
   |**Abonelik**  | Azure Depolama rezervasyonu için ödeme yapmak için kullandığınız abonelik. Seçilen abonelikteki ödeme yöntemi, maliyetlerin tahsilinde kullanılır. Abonelik aşağıdaki türlerden biri olmalıdır:<br/><ul><li> Kurumsal Sözleşme (teklif numaraları MS-AZR-0017P ve MS-AZR-0148P). Kurumsal abonelik için ücretler kaydın parasal taahhüt bakiyesinden düşülür veya fazlalık olarak tahsil edilir.</li><br/><li>Kullandıkça öde oranlarına sahip bireysel abonelik (teklif numaraları MS-AZR-0003P ve MS-AZR-0023P). Kullandıkça öde oranlarına sahip tek bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.</li></ul>    |
   | **Disk** | Oluşturmak istediğiniz SKU. |
   | **Bölge** | Rezervasyonun geçerli olduğu bölge. |
   | **Faturalama sıklığı** | Rezervasyon için hesabın ne sıklıkta faturalandırıldırı. Seçenekler **aylık** ve **peşin**içerir. |

    ![Satın almak istediğiniz ürünü seçmek için bölme.png](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. Rezervasyonunuziçin değerleri belirttikten sonra, Azure portalı maliyeti görüntüler. Portal ayrıca, istediğiniz kadar öde faturalandırmaüzerinden indirim yüzdesini de gösterir. **Satınalma rezervasyonları** bölmesine devam etmek için **İleri'yi** seçin.

1. **Satınalma rezervasyonları** bölmesine, rezervasyonunuzu adlandırabilir ve yapmak istediğiniz toplam rezervasyon miktarını seçebilirsiniz. Rezervasyon sayısı, disk sayısıyla eşler. Örneğin, yüz disk ayırmak istiyorsanız, **Miktar** değerini **100**girin.

1. Rezervasyonun toplam maliyetini gözden geçirin.

    ![Satınalma rezervasyonları bölmesi](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

Bir rezervasyon satın aldıktan sonra, rezervasyon koşullarına uyan varolan Disk Depolama kaynaklarına otomatik olarak uygulanır. Henüz herhangi bir Disk Depolama kaynağı oluşturmadıysanız, rezervasyon koşullarına uyan bir kaynak oluşturduğunuzda rezervasyon geçerlidir. Her iki durumda da, rezervasyon süresi başarılı bir satın alma işleminden hemen sonra başlar.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalar dahilinde rezervasyonları iptal edebilir, değiştirebilir veya iade edebilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="expiration-of-a-reservation"></a>Rezervasyonun sona ermesi

Bir rezervasyonun süresi dolduğunda, söz olsun ki kullandığınız Azure Disk Depolama kapasitesi kullandıkça öde fiyatına faturalandırılır. Rezervasyonlar otomatik olarak yenilenmez.

Rezervasyonun sona ermesinden 30 gün önce ve son kullanma tarihinde tekrar bir e-posta bildirimi alırsınız. Bir rezervasyonun sağladığı maliyet tasarruflarından yararlanmaya devam etmek için, son kullanma tarihinden sonra yenilemek.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://go.microsoft.com/fwlink/?linkid=2083458)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Rezervasyonları nedir?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Rezervasyon indiriminizin Azure Disk Depolama'ya nasıl uygulandığını anlama](../articles/cost-management-billing/reservations/understand-disk-reservations.md)
