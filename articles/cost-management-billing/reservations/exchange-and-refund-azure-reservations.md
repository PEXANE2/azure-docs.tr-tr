---
title: Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri
description: Azure Ayrılmış Sanal Makine Örnekleri’ni nasıl değiştirebileceğinizi veya para iadesi alabileceğinizi öğrenin. Rezervasyonları değiştirmek veya para iadesi almak için Rezervasyon Siparişi üzerinde sahip erişiminiz olmalıdır.
author: yashesvi
ms.service: cost-management-billing
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: f1323d21ee59386220a35cc31b17cab3441f0e0d
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87457643"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Azure Rezervasyonları için self servis değişimler ve para iadeleri

Azure Ayrılmış Sanal Makine Örnekleri, değişen ihtiyaçlarınızın karşılanmasına yardımcı olma esnekliği sağlar. Bir rezervasyonu aynı türdeki başka bir rezervasyonla değiştirebilirsiniz. Örneğin, bir sanal makine rezervasyonunu değiştirerek başka bir sanal makine boyutu veya bölgesi için başka bir rezervasyon satın alabilirsiniz. Benzer şekilde SQL PaaS Veritabanı rezervasyonu, başka bir SQL PaaS Veritabanı türü veya bölgesi için rezervasyon satın almak üzere değiştirilebilir. Ayrıca rezervasyonlar için para iadesi de alabilirsiniz ama faturalama kapsamınızdaki (EA; Microsoft Müşteri Sözleşmesi ve Microsoft İş Ortağı Sözleşmesi gibi) tüm iptal edilen rezervasyon taahhütlerinin toplamı son 12 aylık hareketli zaman penceresinde 50.000 ABD Dolarını aşamaz. Azure Databricks ayrılmış kapasitesi, CloudSimple Azure VMware çözümü rezervasyonu, Azure Red Hat Açık Vardiya rezervasyonu, Red Hat planları ve SUSE Linux planları para iadesine uygun değildir.

Self servis değişim ve iptal özelliği US Government Kurumsal Anlaşma müşterileri tarafından kullanılamaz. Kullandıkça Öde ve Bulut Çözümü Sağlayıcısı (CSP) da dahil olmak üzere diğer US Government abonelik türleri desteklenir.

> [!NOTE]
> - **Mevcut rezervasyonu değiştirmek veya para iadesi almak için Rezervasyon Siparişi üzerinde sahip erişiminiz olmalıdır**. [Rezervasyonu yönetebilecek kullanıcıları ekleyebilir veya değiştirebilirsiniz](https://docs.microsoft.com/azure/cost-management-billing/reservations/manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).
> - Microsoft şu anda rezervasyon para iadelerinde erken sonlandırma ücretlerini almamaktadır. Gelecekte yapılacak para iadelerinde ücretleri alabiliriz. Şu anda bu ücretin etkinleştirileceği tarihi belirlemedik.

## <a name="how-to-exchange-or-refund-an-existing-reservation"></a>Mevcut rezervasyonu değiştirme veya rezervasyon için para iadesi alma

Rezervasyonunu [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) değiştirebilirsiniz.

1. Para iadesi almak istediğiniz rezervasyonları seçin ve **Değiştir**’i seçin.  
    [![İade edilecek rezervasyonları gösteren örnek resim](./media/exchange-and-refund-azure-reservations/exchange-refund-return.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-return.png#lightbox)
1. Satın almak istediğiniz VM ürününü seçip miktar girin. Yeni satın alma toplamının, iade toplamından fazla olduğundan emin olun. [Satın almadan önce doğru boyutu belirleyin](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    [![Değişim ile satın alınacak sanal makine ürününü gösteren örnek resim](./media/exchange-and-refund-azure-reservations/exchange-refund-select-purchase.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-select-purchase.png#lightbox)
1. İşlemi gözden geçirin ve tamamlayın.  
    [![İadenin tamamlandığı, değişim ile satın alınacak sanal makine ürününü gösteren örnek resim](./media/exchange-and-refund-azure-reservations/exchange-refund-confirm-exchange.png)](./media/exchange-and-refund-azure-reservations/exchange-refund-confirm-exchange.png#lightbox)

Rezervasyon para iadesi almak için **Rezervasyon Ayrıntıları** bölümüne gidip **Para İadesi**’ni seçin.

## <a name="exchange-non-premium-storage-for-premium-storage"></a>Premium depolama için premium olmayan depolama değişimi

Premium depolamayı desteklemeyen bir sanal makine boyutu için satın alınan bir rezervasyonu, Premium depolamayı destekleyen ilgili bir sanal makine boyutuyla değiştirebilirsiniz. Örneğin, _F1_ ile _F1s_. Değişim yapmak için Rezervasyon Ayrıntıları’na gidin ve **Değiştir**’i seçin. Değiştir işlemi sonucunda, ayrılmış örneğin süresi sıfırlanmaz veya yeni bir işlem oluşturulmaz. 

## <a name="how-transactions-are-processed"></a>İşlemler nasıl işlenir?

İlk olarak Microsoft, mevcut rezervasyonu iptal eder ve o rezervasyon için eşit olarak dağıtılan tutarı iade eder. Bir değişim varsa yeni satın alma işleme alınır. Microsoft, hesap türünüze ve ödeme yönteminize bağlı olarak aşağıdaki yöntemlerden birini kullanarak para iadelerini işleme alır:

### <a name="enterprise-agreement-customers"></a>Kurumsal anlaşma müşterileri

İlk satın alma, değişim veya para iadesi kullanılarak yapıldıysa, değişim ve para iadeleri için parasal taahhüde para eklenir. Satın alınan rezervasyonu kullanan parasal taahhüt dönemi artık etkin değilse kredi, geçerli kurumsal anlaşma parasal taahhüt döneminize eklenir. Kredi, iade tarihinden itibaren 90 gün geçerlidir. Kullanılmayan kredilerin süresi 90 günün sonunda dolar.

Başlangıçtaki satın alma işlemi fazla kullanım olarak yapıldıysa, rezervasyonun satın alındığı ilk fatura ve sonraki tüm faturalar yeniden açılır ve yeniden ayarlanır. Microsoft para iadeleri için bir alacak faturası düzenler.

### <a name="pay-as-you-go-invoice-payments-and-csp-program"></a>Kullandıkça öde fatura ödemeleri ve CSP programı

İlk rezervasyon satın alma faturası iptal edilir ve sonra para iadesi için yeni bir fatura oluşturulur. Değişimler için yeni faturada para iadesi ve yeni satın alma işlemi gösterilir. Para iadesi tutarı, satın almaya göre ayarlanır. Yalnızca bir rezervasyon para iadesi aldıysanız, eşit olarak dağıtılan tutar Microsoft’ta kalır ve gelecekteki rezervasyon satın alımına karşı ayarlanır. Rezervasyonu kullandıkça öde fiyatlarından satın alıp sonra CSP'ye geçtiyseniz, rezervasyon para cezası ödenmeden iade edilebilir ve yeniden satın alınabilir.

### <a name="pay-as-you-go-credit-card-customers"></a>Kullandıkça öde kredi kartı müşterileri

Özgün fatura iptal edilir ve yeni bir fatura oluşturulur. Para, ilk satın almada kullanılan kredi kartına iade edilir. Kartınızı değiştirdiyseniz, [desteğe başvurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="cancel-exchange-and-refund-policies"></a>İptal, değişim ve para iadesi ilkeleri

Azure, iptaller, değişimler ve para iadeleri için aşağıdaki ilkelere sahiptir.

**Değişim ilkeleri**

- Aynı türde yeni bir rezervasyon satın almak için birden fazla mevcut rezervasyonu iade edebilirsiniz. Bir türdeki rezervasyonu başka bir türle değiştiremezsiniz. Örneğin, bir SQL rezervasyonu satın almak için sanal makine rezervasyonu iade edemezsiniz. Aile, seri, sürüm, SKU, bölge, miktar ve süre gibi bir rezervasyon özelliğini değişim ile değiştirebilirsiniz.
- Yalnızca rezervasyon sahipleri bir değişimi işleme alabilir. [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme hakkında bilgi edinin](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Değişim, para iadesi veya yeniden satın alma olarak işleme alınır. İptal ve yeniden rezervasyon satın alma için farklı işlemler oluşturulur. Eşit olarak dağıtılan rezervasyon tutarı, takas yapılan rezervasyonlar için iade edilir. Yeni satın alma için tam ücret size yansıtılır. Eşit olarak dağıtılan rezervasyon tutarı, iade edilen rezervasyonun günlere eşit olarak dağıtılan kalan değeridir.
- Rezervasyonu satın almak için kullanılan kurumsal anlaşmanın süresi dolup yeni bir anlaşma olarak yenilenmiş olsa da rezervasyonları değiştiremez veya para iadesi alamazsınız.
- Yeni rezervasyonun yaşam süresi taahhüdü, iade edilen rezervasyonun kalan taahhüdüne eşit veya ondan büyük olmalıdır. Örnek: Aylık 100 ABD Doları tutarında olan ve 18. ödemeden sonra değiştirilen üç yıllık bir rezervasyon için, yeni rezervasyonun yaşam süresi taahhüdü 1.800 ABD Doları veya daha fazla olmalıdır (aylık veya peşin).
- Değişim kapsamında satın alınan yeni rezervasyonlar, değişim anından başlayan yeni bir döneme sahip olur.
- Değişimler için ceza veya yıllık limit yoktur.

**Para iadesi ilkeleri**

- Şu anda erken sonlandırma ücreti almıyoruz ama gelecekte iptaller için %12 erken sonlandırma ücreti uygulanabilir.
- Faturalama profili veya tek bir kayıt için iptal edilen toplam taahhüt son 12 aylık hareketli zaman penceresinde 50.000 ABD Dolarını aşamaz. Örneğin aylık 100 ABD Doları tutarında olan ve 18. ayda para iadesi yapılan üç yıllık bir rezervasyon için, iptal edilen taahhüt 1.800 ABD Dolarıdır. Para iadesinden sonra, yeni kullanılabilir para iadesi limitiniz 48.200 ABD Doları olur. Para iadesinden 365 gün sonra 48.200 ABD Doları tutarındaki limit 1.800 ABD Doları daha artırılarak yeni havuzunuz 50.000 ABD Doları olur. Faturalama profili veya EA kaydı için diğer tüm rezervasyon iptalleri aynı havuzdan düşülür ve aynı yenileme mantığı uygulanır.
- Azure bir faturalama profili veya EA kaydı için son 12 aylık sürede 50.000 ABD Doları sınırını aşan hiçbir para iadesini işlemez.
- Para iadeleri, satın aldığınız fiyattan veya geçerli rezervasyon fiyatından düşük olana göre hesaplanır.
- Yalnızca rezervasyon siparişi sahipleri para iadesini işleme alabilir. [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme hakkında bilgi edinin](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonu yönetme hakkında bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri’ni Yönetme](manage-reserved-vm-instance.md).
- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
    - [Azure’da Rezervasyonları Yönetme](manage-reserved-vm-instance.md)
    - [Rezervasyon indiriminin nasıl uygulandığını anlama](../manage/understand-vm-reservation-charges.md)
    - [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
    - [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](reserved-instance-windows-software-costs.md)
    - [CSP programında Azure Rezervasyonları](/partner-center/azure-reservations)
