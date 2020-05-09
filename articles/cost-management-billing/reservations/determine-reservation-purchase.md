---
title: Hangi Azure rezervasyonunu satın almanız gerektiğini saptama
description: Bu makale hangi rezervasyonu satın almanız gerektiğini saptamanıza yardımcı olur.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: banders
ms.openlocfilehash: feee7475dcadc6d06693d9e60020097f8dc9149c
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628614"
---
# <a name="determine-what-reservation-to-purchase"></a>Satın alınacak rezervasyonu saptama

Azure Databricks dışındaki tüm rezervasyonlar saatlik olarak uygulanır. Rezervasyonları tutarlı kullanıma göre satın almalısınız. Neyin satın alınması gerektiğini saptamanın çeşitli yolları vardır ve bu makale hangi rezervasyonu satın almanız gerektiğini saptamanıza yardımcı olur.

Geçmiş kullanımınızdan daha fazla kapasite satın almak bir rezervasyonun yetersiz kullanılmasına yol açar. Mümkün olduğunca yetersiz kullanımdan kaçınmalısınız. Kullanılmayan ayrılmış kapasite bir saatten diğerine taşınmaz. Ayrılmış kapasiteyi aşan kullanım daha pahalı kullandıkça ödeme fiyatlarıyla ücretlendirilir.

## <a name="analyze-usage-data"></a>Kullanım verilerini analiz etme

Günlük kullanım verilerinizi analiz edip temel kullanımınızı saptamanıza ve hangi rezervasyonun satın alınacağını saptamanıza yardımcı olur.

### <a name="analyze-usage-for-a-vm-reserved-instance-purchase"></a>Ayrılmış sanal makine örneği satın almak için kullanımı analiz etme

Satın almanız için doğru sanal makine boyutunu belirleyin. Örneğin ES serisi sanal makineleri için satın alınan rezervasyon E serisi sanal makinelere uygulanmaz (tersi de geçerlidir).

Promo serisi sanal makineler rezervasyon indirimi almaz, dolayısıyla onları analizinizden çıkarın.

Uygun sanal makine kullanımını içerecek şekilde daraltmak için, kullanım verilerinize aşağıdaki filtreleri uygulayın:

- **MeterCategory** öğesini **Sanal Makineler**’e göre filtreleyin.
- **ServiceType** bilgisini **AdditionalInfo** alanından alın. Bu bilgi doğru sanal makine boyutunu önerir. Örneğin, Standart E32.
- Kullanım verileri merkezini saptamak için **ResourceLocation** alanını kullanın.

Günlük kullanımı 24 saatten az olan kaynakları yoksayın.

Örnek boyutu ailesi düzeyinde analiz etmek isterseniz, örnek boyutu esneklik değerlerini [https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv) adresinden alabilirsiniz. Analiz etmek için değerleri verilerle birleştirin. Örnek boyutu esnekliği hakkında daha fazla bilgi için bkz. [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](../../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

### <a name="analyze-usage-for-an-azure-synapse-analytics-reserved-instance-purchase"></a>Azure Synapse Analytics ayrılmış örneği satın alımı için kullanımı analiz etme

Ayrılmış kapasite Azure Synapse Analytics DWU fiyatlandırmasına uygulanır. Azure Synapse Analytics lisans maliyetine veya işlem dışında hiçbir maliyete uygulanmaz.

Uygun kullanımı daraltmak için kullanım verilerinize aşağıdaki filtreleri uygulayın:


- **MeterCategory** öğesini **SQL Veritabanı**’na göre filtreleyin.
- **MeterName** öğesini **Sanal Çekirdek**’e göre filtreleyin.
- **MeterSubCategory** öğesini, adında _İşlem_ bulunan tüm kullanım kayıtlarına göre filtreleyin.

**AdditionalInfo** alanından **Sanal Çekirdekler** değerini alın. Bu size kaç sanal çekirdek kullanıldığını gösterir. Miktar, **Sanal Çekirdekler** değerinin veritabanının kullanıldığı saat sayısıyla çarpılmasıyla elde edilir.

Veriler size şunların tutarlı kullanımı hakkında bilgi verir:

- Veritabanı türü bileşimi. Örneğin, tek veritabanı başına yönetilen örnek veya elastik havuz sayısı.
- Hizmet katmanı. Örneğin, genel amaçlı veya iş açısından kritik.
- Nesil. Örneğin, 5. Nesil.
- Kaynak Konumu

### <a name="analysis-for-azure-synapse-analytics"></a>Azure Synapse Analytics için analiz

Ayrılmış kapasite Azure Synapse Analytics DWU kullanımına uygulanır ve 100 DWU’luk artımlarla satın alınır. Uygun kullanımı daraltmak için kullanım verilerinize aşağıdaki filtreleri uygulayın:

- **MeterName** öğesini **100 DWU**’ya göre filtreleyin.
- **Meter Sub-Category** öğesini **İşlem İçin İyileştirilmiş 2. Nesil**’e göre filtreleyin.

Bir bölgedeki Azure Synapse Analytics kullanımını saptamak için **Kaynak Konumu** alanını kullanın.

Azure Synapse Analytics kullanımının ölçeği gün boyunca artırılabilir ve azaltılabilir. Temel kullanımı öğrenmek için Azure Synapse Analytics örneğini yöneten ekiple görüşün.

Azure portalında Rezervasyonlar’a gidin ve 100 DWU’nun katlarında Azure Synapse Analytics ayrılmış kapasitesi satın alın.

## <a name="reservation-purchase-recommendations"></a>Rezervasyon satın alma önerileri

Rezervasyon satın alma önerileri son 7, 30 ve 60 gün içindeki saatlik kullanım verilerinizin analiz edilmesiyle hesaplanır. Azure, bir rezervasyonunuz olsa maliyetlerinizin ne kadar olacağını hesaplar ve bunu söz konusu süredeki fiili kullandıkça öde maliyetlerinizle karşılaştırır. Hesaplama, zaman çerçevesinde kullandığınız her miktar için gerçekleştirilir. Tasarruflarınızı en üst düzeye çıkaran miktar önerilir.

Örneğin çoğunlukla 500 sanal makine kullanıyor olabilirsiniz ama bazen kullanım 700 sanal makineye fırlıyor olabilir. Bu örnekte Azure hem 500 hem de 700 sanal makinelik miktarlar için tasarruflarınızı hesaplar. 700 sanal makine kullanımı seyrek görülen bir durum olduğundan, öneri hesaplaması 500 sanal makine rezervasyonu satın alındığında tasarrufların en üst düzeye çıktığını saptar ve 500 sanal makine miktarına yönelik öneri sağlanır.

Aşağıdaki noktalara dikkat edin:

- Rezervasyon önerileri sizin için geçerli olan isteğe bağlı kullanım fiyatlarıyla hesaplanır.
- Öneriler örnek boyutu ailesi için değil tek tek boyutlar için hesaplanır.
- Kapsam için önerilen miktar, bu kapsama yönelik rezervasyon satın aldığınız gün düşürülür.
    - Öte yandan, kapsamlar arasında rezervasyon miktarı önerisinin güncelleştirilmesi 25 gün kadar sürebilir. Örneğin paylaşılan kapsam önerileri temelinde satın alırsanız, tek abonelik kapsamı önerilerinin ayarlanıp düşürülmesi 25 gün kadar sürebilir.

## <a name="recommendations-in-the-azure-portal"></a>Azure portalındaki öneriler

Öneri altyapısı tarafından hesaplanan rezervasyon satın almaları [Azure portalındaki](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/docs) **Önerilen** sekmesinde gösterilir. Burada bir örnek resim verilmiştir.

![Önerileri gösteren resim](./media/determine-reservation-purchase/select-product-ri.png)

## <a name="recommendations-in-the-cost-management-power-bi-app"></a>Maliyet Yönetimi Power BI uygulamasındaki öneriler

Kurumsal Anlaşma ve Microsoft Müşteri Sözleşmesi müşterileri sanal makineler ve satın alma önerileri için VM RI Kapsam raporlarını kullanabilir. Kapsam raporları size toplam kullanımı ve ayrılmış örneklerin kapsamına giren kullanımı gösterir.

1. [Maliyet Yönetimi Uygulaması](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp)’nı alın.
2. Hangi kapsamda satın almak istediğinize bağlı olarak VM RI Kapsam raporu – Paylaşılan veya Tek kapsamına gidin.
3. Bölgeyi, kullanımı görecek örnek boyutu ailesini, RI kapsamını ve seçili filtrenin satın alma önerisini seçin.

## <a name="recommendations-in-azure-advisor"></a>Azure Danışmanı’nda Öneriler

Rezervasyon satın alma önerileri [Azure Danışmanı](https://portal.azure.com/#blade/Microsoft_Azure_Expert/AdvisorMenuBlade/overview)’nda kullanıma sunuldu.

- Danışman, yalnızca tek abonelik kapsamında öneriler sunar.
- Danışman önerileri 30 günlük geriye bakma dönemi kullanılarak hesaplanır. Tahmini tasarruflar 3 yıllık rezervasyon dönemine yöneliktir.
- Paylaşılan kapsam rezervasyonu satın alırsanız, Danışman rezervasyon satın alma önerilerinin kapatılması 30 güne kadar sürebilir.

## <a name="recommendations-using-apis"></a>API’ler kullanılarak öneriler

Önerileri program aracılığıyla görüntülemek için [Rezervasyon Önerileri](/rest/api/consumption/reservationrecommendations/list) REST API’sini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça öde fiyatlarına tabi aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](reserved-instance-windows-software-costs.md)