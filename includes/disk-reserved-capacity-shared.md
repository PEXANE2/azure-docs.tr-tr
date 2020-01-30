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
ms.openlocfilehash: 89586f932ee358664a2869c87ced72594336b404
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76847140"
---
Ayrılmış kapasiteye sahip Premium katı hal sürücüsü (SSD) kullanımınıza kaydedin ve ayrılmış sanal makine örnekleriyle birlikte, toplam VM maliyetlerinizi azaltabilirsiniz. Rezervasyon indirimi, seçilen ayırma kapsamındaki eşleşen disklere otomatik olarak uygulanır, indirimleri almak için yönetilen diske bir ayırma atamanız gerekmez. İndirimler, disk kullanımı için saatlik olarak uygulanır ve kullanılmayan tüm ayrılmış kapasite üzerinden taşınmaz. Yönetilen disk ayırma indirimi yönetilmeyen disklere, Ultra disklere veya Sayfa Blobu tüketimine uygulanmaz.

## <a name="determine-your-storage-needs"></a>Depolama ihtiyaçlarınızı belirleme

Bir ayırma satın almadan önce, depolama ihtiyaçlarınıza ne olduğunu belirlemelisiniz. Şu anda disk ayırma yalnızca select Premium SSD SKU 'Ları için kullanılabilir. Bir Premium SSD 'nin SKU 'SU, diskin boyutunu ve performansını belirler. Depolama ihtiyaçlarınızı belirlerken, disklerin yalnızca toplam kapasiteye sahip olduğunu düşündük, daha büyük bir disk (P40 gibi) için bir ayırma kullanamaz ve bunu iki daha küçük disk (P30) için ödeme yapmak üzere kullanabilirsiniz. Bir rezervasyon satın alırken yalnızca SKU başına toplam disk sayısını satın alabilirsiniz.

Disk ayırma, disk SKU 'SU başına yapılır, bu nedenle ayırma tüketimi, belirtilen boyut yerine disk SKU 'Larının birimine göre yapılır. Örneğin, 1 P40 2 TiB sağlanan kapasiteyi ayırdıysanız ancak yalnızca 2 P30 disk ayırdıysanız, iki P30 tüketimi P40 rezervasyonuna göre hesaba katılmaz ve Kullandıkça Öde tarifesine göre ödeme yaparsınız.



[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="purchase-considerations"></a>Satın alma konuları

Disk rezervasyonu satın almayı düşünürken aşağıdaki en iyi yöntemleri öneririz:

- Satın almanız gereken rezervasyonları belirlemede yardımcı olması için kullanım bilgilerinizi çözümleyin. Sağlanan veya kullanılan disk kapasitesi yerine disk SKU 'Larında kullanımı izlemediğinizden emin olun. 
- VM ayırmayla birlikte disk ayırmayı inceleyin. Maksimum kaydetme için hem VM hem de disk kullanımı için rezervasyon yapmanız önemle önerilir. Doğru VM ayırmasını belirleyerek başlayabilir ve ardından disk ayırmayı uygun şekilde değerlendirin. Genellikle, iş yüklerinizin her biri için standart bir yapılandırmaya sahip olacaksınız. Örneğin, bir SQL Server iki P40 veri diskine ve bir P30 işletim sistemi diskine sahip olabilir. Bu tür bir model, satın alabileceğiniz rezervasyonların miktarını belirlemenize yardımcı olabilir. Bu yaklaşım, değerlendirme sürecini basitleştirebilir ve ayrıca abonelikler, bölgeler ve diğer koşullarda hem VM 'ler hem de diskler için hizalanmış bir plana sahip olduğunuzdan emin olmanızı sağlayabilir. 

## <a name="purchase-restrictions"></a>Satın alma kısıtlamaları

Şu disklerde rezervasyon iskontoları Şu anda kullanılamıyor:
- Yönetilmeyen diskler/sayfa Blobları
- Standart SSD veya standart HDD
- P30 ' den küçük Premium SSD SKU 'Lar P1/P2/P3/P4/P6/P10/P15/P20 Premium SSD SKU 'Larında kullanılamaz.
- Bulutlar-Azure gov, Almanya veya Çin bölgelerinde satın alınabilecek rezervasyonlar yoktur.
- Kapasite kısıtlamaları-nadir koşullarda Azure, bir bölgedeki düşük kapasite nedeniyle disk SKU 'Larının alt kümesi için yeni rezervasyonların satın alınmasını sınırlandırır.

## <a name="buy-a-disk-reservation"></a>Disk ayırması satın alma

Azure disk ayırmalarını [Azure Portal](https://portal.azure.com/)aracılığıyla satın alabilirsiniz. Rezervasyon için ya da aylık ödemeler ile ödeme yapabilirsiniz. Aylık ödemeler ile satın alma hakkında daha fazla bilgi için bkz. [aylık ödemeler Ile satın alma rezervasyonları](../articles/cost-management-billing/reservations/monthly-payments-reservations.md).

Ayrılmış kapasiteyi satın almak için aşağıdaki adımları izleyin:

1. Azure portal [satın alma rezervasyonları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) dikey penceresine gidin.
1. Bir ayırma satın almak için **Azure yönetilen diskler** ' i seçin.

    ![Disks-Reserved-Purchase-Reservation. png](media/disks-reserved-capacity/disks-reserved-purchase-reservation.png)    

1. Gerekli alanları aşağıdaki tabloda açıklandığı gibi girin:

   |Alan  |Açıklama  |
   |---------|---------|
   |**Kapsam**   |  Rezervasyonla ilişkili faturalandırma avantajını kaç tane aboneliğin kullanabileceğinizi gösterir. Ayrıca, rezervasyonun belirli aboneliklere nasıl uygulanacağını de denetler. <br/><br/> **Paylaşılan**' i seçerseniz, rezervasyon indirimi, faturalandırma bağlamınızın içindeki herhangi bir abonelikte Azure depolama kapasitesine uygulanır. Faturalandırma bağlamı, Azure 'a kaydolmanızı temel alır. Kurumsal müşteriler için, paylaşılan kapsam kayıt içindedir ve kayıt dahilindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için, paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan Kullandıkça Öde tarifesine sahip tüm bireysel abonelikleri içerir.  <br/><br/>  **Tek bir abonelik**seçerseniz, rezervasyon indirimi seçili abonelikte Azure depolama kapasitesine uygulanır. <br/><br/> **Tek kaynak grubu**' nu seçerseniz, rezervasyon indirimi seçili abonelikteki Azure depolama kapasitesine ve bu abonelikteki seçili kaynak grubuna uygulanır. <br/><br/> Ayırmayı satın aldıktan sonra rezervasyon kapsamını değiştirebilirsiniz.  |
   |**Abonelik**  | Azure depolama ayırması için ödeme yapmak üzere kullanılan abonelik. Seçili abonelikteki ödeme yöntemi, maliyetleri borçlandırmak üzere kullanılır. Abonelik aşağıdaki türlerden biri olmalıdır: <br/><br/>  Kurumsal Anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P): kurumsal bir abonelik Için ücretler, kayıt parasal taahhüt bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir. <br/><br/> Kullandıkça Öde tarifesine sahip tek tek abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P): Kullandıkça Öde tarifesine sahip bireysel bir abonelik Için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.    |
   | **Diskler** | Oluşturmak istediğiniz SKU. |
   | **Bölge** | Ayırmanın etkin olduğu bölge. |
   | **Faturalandırma sıklığı** | Hesabın rezervasyon için ne sıklıkta faturalandırıldığını gösterir. Seçenekler *ayda* veya *önünde*bulunur. |

    ![Premium-SSD-Reserved-Purchase-Selection. png](media/disks-reserved-capacity/premium-ssd-reserved-purchase-selection.png)

1. Rezervasyonunuzun parametrelerini seçtikten sonra Azure portal maliyeti görüntüler. Portal Ayrıca Kullandıkça Öde faturalandırmasına göre indirim yüzdesini gösterir. **İleri ' yi** seçerek, **satın alma rezervasyonları** dikey penceresine geçin.

1. **Satın alma rezervasyonları** bölmesinde, rezervasyonunuzun adını verebilir ve yapmak istediğiniz toplam ayırma miktarını seçebilirsiniz. Ayırma sayısı, disk sayısıyla eşlenir. Örneğin, yüz disk ayırmak isterseniz **miktarı** 100 olarak değiştirirsiniz.
1. Ayırmanın toplam maliyetini gözden geçirin.

    ![Premium-SSD-Reserved-Selecting-SKU-Total-Purchase. png](media/disks-reserved-capacity/premium-ssd-reserved-selecting-sku-total-purchase.png)

Bir ayırma satın aldıktan sonra, rezervasyon koşullarına uyan mevcut Azure disk depolama kaynaklarına otomatik olarak uygulanır. Henüz hiç Azure disk depolama kaynağı oluşturmadıysanız, rezervasyon koşullarına uyan bir kaynak oluşturduğunuzda rezervasyon uygulanır. Her iki durumda da, rezervasyon dönemi başarılı bir satın alma işleminden hemen sonra başlar.

## <a name="exchange-or-refund-a-reservation"></a>Bir ayırmayı Exchange veya para iadesi

Belirli sınırlamalar sayesinde bir ayırmayı Exchange veya geri ödeme yapabilirsiniz.

Bir ayırmayı Exchange veya para iadesi için Azure portal rezervasyon ayrıntılarına gidin. **Exchange veya para iadesi**' ni seçin ve destek isteği göndermek için yönergeleri izleyin. İstek işlendiğinde, Microsoft isteğin tamamlandığını onaylamak için size bir e-posta gönderir.

Azure ayırmaları ilkeleri hakkında daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](../articles/cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

### <a name="exchange-a-reservation"></a>Bir ayırma Exchange

Ayırma değişimi, rezervasyonun kullanılmayan kısmına göre eşit olarak dağıtılmış bir para iadesi almanızı sağlar. Daha sonra para iadesini yeni bir Azure disk rezervasyonunun satın alma fiyatına uygulayabilirsiniz.
Gerçekleştirebileceğiniz değiştirme işlemlerinin sayısında herhangi bir sınırlama yoktur. Değiştirme işlemi için ek ücret de uygulanmaz. Satın aldığınız yeni rezervasyon, orijinal rezervasyondan eşit veya daha büyük bir değere eşit veya daha büyük olmalıdır. Azure disk ayırması, diğer Azure hizmetleri için bir ayırma için değil yalnızca başka bir Azure disk ayırması için değiş tokuş edilebilir.

### <a name="refund-a-reservation"></a>Ayırmayı geri ödeme

Azure disk ayırmayı dilediğiniz zaman iptal edebilirsiniz. İptal ederseniz, rezervasyonun kalan terimine göre eşit olarak dağıtılmış bir para iadesi elde edersiniz ve yüzde 12 erken sonlandırma ücreti alınır. Yıl başına maksimum para iadesi $50.000 ' dir.
Bir ayırmanın iptal edilmesi hemen ayırmayı sonlandırır ve kalan ayları Microsoft 'a geri döndürür. Kalan eşit oranda dağıtılmış Bakiye, eksi ücret, orijinal satın alma formunuza iade edilir.

## <a name="expiration-of-a-reservation"></a>Bir ayırmanın süre sonu

Bir rezervasyon süresi dolmuşsa, bu rezervasyon altında kullandığınız tüm Azure disk kapasitesi Kullandıkça Öde fiyatı üzerinden faturalandırılır. Rezervasyonlar otomatik olarak yenilenmez.
Ayırmanın süresinin dolması için 30 gün önce ve sona erme tarihi üzerinden bir e-posta bildirimi alacaksınız. Bir rezervasyonun sağladığı maliyet tasarruflarından yararlanmaya devam etmek için süreyi, sona erme tarihinden sonra değil, yenileyin.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bize ulaşın

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Rezervasyonlar nedir?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Rezervasyon iskontosunun Azure Disk Depolama nasıl uygulanacağını anlayın](../articles/cost-management-billing/reservations/understand-disk-reservations.md)