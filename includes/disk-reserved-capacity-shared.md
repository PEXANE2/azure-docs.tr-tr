---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: cb959b94807678187363d3132ece273584f13a0a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77590737"
---
Ayrılmış kapasiteye sahip Azure Disk Depolama kullanımınıza kaydedin. Azure ayrılmış sanal makine örnekleriyle birleştirilmiş Azure Disk Depolama ayırmalar, toplam sanal makine (VM) maliyetlerinizi düşürmenizi sağlar. Rezervasyon indirimi, seçilen ayırma kapsamındaki eşleşen disklere otomatik olarak uygulanır. Bu otomatik uygulama nedeniyle, indirimleri almak için yönetilen diske bir ayırma atamanız gerekmez.

İndirimler, disk kullanımına bağlı olarak saatlik olarak uygulanır. Kullanılmayan ayrılmış kapasite üzerinden taşınmaz. Azure Disk Depolama rezervasyon iskontoları yönetilmeyen disklere, Ultra disklere veya Sayfa Blobu tüketimine uygulanmaz.

## <a name="determine-your-storage-needs"></a>Depolama ihtiyaçlarınızı belirleme

Bir ayırma satın almadan önce depolama gereksinimlerinizi saptayın. Şu anda Azure Disk Depolama ayırmalar yalnızca Azure Premium SSD SKU 'Ları için kullanılabilir. Bir Premium SSD 'nin SKU 'SU, diskin boyutunu ve performansını belirler.

Depolama ihtiyaçlarınızı belirlerken, diskleri yalnızca kapasiteye göre düşünmeyin. Örneğin, bir P40 diski için bir ayırmaya sahip olamaz ve bunu iki küçük P30 diski için ödeme yapmak üzere kullanabilirsiniz. Bir rezervasyon satın alırken, yalnızca SKU başına toplam disk sayısı için bir rezervasyon satın alırsınız.

Disk için bir ayırma, disk SKU 'SU başına yapılır. Sonuç olarak, ayırma tüketimi, belirtilen boyut yerine disk SKU 'Larının birimini temel alır.

Örneğin, 2 TiB sağlanan depolama kapasitesi olan bir P40 disk ayırdığınızı varsayalım. Ayrıca yalnızca iki P30 disk ayıradığınızı varsayın. Bu durumda P40 ayırması P30 tüketimini hesaba almaz ve P30 disklerinde Kullandıkça Öde tarifesine göre ödeme yaparsınız.
<br/>
<br/>
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Satın alma konuları

Disk rezervasyonu satın almayı düşünürken aşağıdaki yöntemleri öneririz:

- Satın almanız gereken rezervasyonları belirlemede yardımcı olması için kullanım bilgilerinizi çözümleyin. Sağlanan veya kullanılan disk kapasitesi yerine disk SKU 'Larında kullanımı izlemediğinizden emin olun.
- VM ayırmayla birlikte disk ayırmayı inceleyin. Maksimum tasarruflar için hem VM kullanımı hem de disk kullanımı için rezervasyonlar yapmak çok tavsiye ederiz. Doğru VM ayırmasını belirleyip, ardından disk ayırmayı değerlendirebilmeniz için ' i kullanabilirsiniz. Genellikle, iş yüklerinizin her biri için standart bir yapılandırmanız olacaktır. Örneğin, bir SQL Server sunucusu iki P40 veri diskine ve bir P30 işletim sistemi diskine sahip olabilir.
  
  Bu tür bir model, satın alabileceğiniz ayrılmış miktarı belirlemenize yardımcı olabilir. Bu yaklaşım, değerlendirme sürecini basitleştirebilir ve hem VM 'niz hem de diskleriniz için hizalanmış bir plana sahip olduğunuzdan emin olabilir. Plan, abonelikler veya bölgeler gibi hususlar içerir.

## <a name="purchase-restrictions"></a>Satın alma kısıtlamaları

Rezervasyon iskontoları Şu anda şunlar için kullanılamaz:

- Yönetilmeyen diskler veya sayfa Blobları.
- Standart SSD 'Ler veya standart sabit disk sürücüleri (HDD 'Ler).
- Premium SSD SKU 'Lar P30: P1, P2, P3, P4, P6, P10, P15 ve P20 SSD SKU 'Larını küçüktür.
- Azure Kamu, Azure Almanya veya Azure Çin bölgelerindeki diskler.

Nadir koşullarda Azure, bir bölgedeki düşük kapasiteden dolayı yeni rezervasyonların satın alımını disk SKU 'Larının bir alt kümesiyle sınırlandırır.

## <a name="buy-a-disk-reservation"></a>Disk ayırması satın alma

[Azure Portal](https://portal.azure.com/)üzerinden Azure disk depolama ayırmaları satın alabilirsiniz. Rezervasyon için ön ya da aylık ödemeler için ödeme yapabilirsiniz. Aylık ödemeler ile satın alma hakkında daha fazla bilgi için bkz. [aylık ödemeler Ile satın alma rezervasyonları](../articles/cost-management-billing/reservations/monthly-payments-reservations.md).

Ayrılmış kapasiteyi satın almak için aşağıdaki adımları izleyin:

1. Azure portal [satın alma rezervasyonları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) bölmesine gidin.

1. Bir ayırma satın almak için **Azure yönetilen diskler** ' i seçin.

    ![Ayırma satın alma bölmesi](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png) 

1. Aşağıdaki tabloda açıklanan gerekli değerleri belirtin:

   |Öğe  |Açıklama  |
   |---------|---------|
   |**Kapsam**   |  Ayırma ile ilişkili faturalandırma avantajını kaç tane abonelik kullanabilir? Bu değer Ayrıca, rezervasyonun belirli aboneliklere nasıl uygulanacağını belirtir. <br/><br/> **Paylaşılan**' i seçerseniz, rezervasyon indirimi, faturalandırma bağlamınızın içindeki her abonelikte Azure depolama kapasitesine uygulanır. Faturalandırma bağlamı, Azure 'a kaydolmanızı temel alır. Kurumsal müşteriler için, paylaşılan kapsam kayıt içindedir ve kayıt dahilindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için, paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan Kullandıkça Öde tarifesine sahip tüm bireysel abonelikleri içerir.  <br/><br/>  **Tek bir abonelik**seçerseniz, rezervasyon indirimi seçili abonelikte Azure depolama kapasitesine uygulanır. <br/><br/> **Tek kaynak grubu**' nu seçerseniz, ayırma indirimi seçili abonelikte ve bu aboneliğin seçili kaynak grubunda Azure depolama kapasitesine uygulanır. <br/><br/> Ayırmayı satın aldıktan sonra rezervasyon kapsamını değiştirebilirsiniz.  |
   |**Abonelik**  | Azure depolama ayırması için ödeme yapmak üzere kullandığınız abonelik. Seçili abonelikteki ödeme yöntemi, maliyetleri borçlandırmak üzere kullanılır. Abonelik aşağıdaki türlerden biri olmalıdır:<br/><ul><li> Kurumsal Anlaşma (teklif numaraları MS-AZR-0017P ve MS-AZR-0148P). Kurumsal bir abonelik için ücretler, kayıt parasal taahhüt bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir.</li><br/><li>Kullandıkça Öde tarifesine sahip bireysel abonelik (MS-AZR-0003P ve MS-AZR-0023P). Kullandıkça Öde tarifesine sahip tek bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.</li></ul>    |
   | **Disklerinden** | Oluşturmak istediğiniz SKU. |
   | **Geli** | Ayırmanın etkin olduğu bölge. |
   | **Faturalandırma sıklığı** | Hesap rezervasyon için ne sıklıkta faturalandırılır. Seçenekler **ayda** ve **önünde**bulunur. |

    ![purchase.png istediğiniz ürünü seçme bölmesi](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. Rezervasyonunuzun değerlerini belirttikten sonra Azure portal maliyeti görüntüler. Portal Ayrıca Kullandıkça Öde faturalandırmasına göre indirim yüzdesini gösterir. **Satın alma rezervasyonları** bölmesine devam etmek için **İleri ' yi** seçin.

1. **Satın alma rezervasyonları** bölmesinde, rezervasyonunuzun adını değiştirebilir ve yapmak istediğiniz toplam ayırma miktarını seçebilirsiniz. Ayırma sayısı, disk sayısıyla eşlenir. Örneğin, yüz disk ayırmak istiyorsanız **100** **Miktar** değerini girin.

1. Ayırmanın toplam maliyetini gözden geçirin.

    ![Satın alma rezervasyonları bölmesi](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

Bir ayırma satın aldıktan sonra, rezervasyon koşullarına uyan mevcut Disk Depolama kaynaklarına otomatik olarak uygulanır. Henüz Disk Depolama kaynak oluşturmadıysanız, rezervasyon koşullarına uyan her kaynak oluşturduğunuzda rezervasyon uygulanır. Her iki durumda da, rezervasyon dönemi başarılı bir satın alma işleminden hemen sonra başlar.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalar dahilinde rezervasyonları iptal edebilir, Exchange veya para iadesi yapabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="expiration-of-a-reservation"></a>Bir ayırmanın süre sonu

Bir rezervasyon süresi dolmuşsa, bu rezervasyon altında kullandığınız Azure Disk Depolama kapasitesi Kullandıkça Öde fiyatı üzerinden faturalandırılır. Rezervasyonlar otomatik olarak yenilenmez.

Ayırmayı sona ermeden 30 gün önce ve sona erme tarihi için bir e-posta bildirimi alacaksınız. Bir rezervasyonun sağladığı maliyet tasarruflarından yararlanmaya devam etmek için süreyi, sona erme tarihinden sonra değil, yenileyin.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Rezervasyon indiriminizin Azure Disk Depolama'ya nasıl uygulandığını anlama](../articles/cost-management-billing/reservations/understand-disk-reservations.md)
