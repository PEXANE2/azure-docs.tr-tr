---
title: Maliyeti optimize etmek için Azure Cosmos DB'de ayrılmış kapasite
description: İşlem maliyetlerinizden tasarruf etmek için Azure Cosmos DB ayrılmış kapasiteyi nasıl satın alacağınızı öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: tisande
ms.reviewer: sngun
ms.openlocfilehash: 16e8f770445218e10ab7e7645a81325d11be55da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77505959"
---
# <a name="optimize-cost-with-reserved-capacity-in-azure-cosmos-db"></a>Azure Cosmos DB'de ayrılmış kapasiteyle maliyeti iyileştirme

Azure Cosmos DB ayrılmış kapasite, Azure Cosmos DB kaynakları için bir veya üç yıllık rezervasyon taahhüdü vererek tasarruf etmenize yardımcı olur. Azure Cosmos DB ayrılmış kapasite ile Cosmos DB kaynakları için sağlanan aktarım hızında indirim elde edebilirsiniz. Veritabanları ve kapsayıcılar (tablolar, koleksiyonlar ve graflar), kaynaklara örnek olarak verilebilir.

Azure Cosmos DB rezerve edilmiş kapasite,&mdash;bir yıllık veya üç yıllık peşin taahhütle Cosmos DB maliyetlerinizi normal fiyatlarla yüzde 65'e kadar önemli ölçüde azaltabilir. Ayrılmış kapasite bir fatura indirimi sağlar ve Azure Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez.

Azure Cosmos DB ayrılmış kapasite, kaynaklarınız için sağlanan iş ortası kapsar. Depolama ve ağ ücretlerini kapsamaz. Bir rezervasyon satın alır almaz, rezervasyon özelliklerine uyan iş yükü artık kullandıkça öde fiyatlarıüzerinden tahsil edilemez. Rezervasyonlar hakkında daha fazla bilgi için [Azure rezervasyonları](../cost-management-billing/reservations/save-compute-costs-reservations.md) makalesine bakın.

[Azure portalından](https://portal.azure.com)Azure Cosmos DB rezerve edilmiş kapasiteyi satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](../cost-management-billing/reservations/monthly-payments-reservations.md) rezervasyon ödemesi yapın. Ayrılmış kapasite satın almak için:

* Kullandıkça öde oranlarıyla en az bir Kurumsal veya bireysel abonelik için Sahip rolünde olmalısınız.  
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
* Bulut Çözüm Sağlayıcısı (CSP) programı için yalnızca yönetici aracılar veya satış temsilcileri Azure Cosmos DB ayrılmış kapasite satın alabilir.

## <a name="determine-the-required-throughput-before-purchase"></a>Satın almadan önce gerekli iş bürünme işlemini belirleme

Ayrılmış kapasite satın alma boyutu, varolan veya yakında dağıtılacak Azure Cosmos DB kaynaklarının saatlik olarak kullanacağı toplam üretim miktarına dayanmalıdır. Örneğin: Tutarlı saatlik kullanım deseniniz buysa 30.000 RU/s ayrılmış kapasite satın alın. Bu örnekte, 30.000 RU/s'nin üzerinde sağlanan tüm iş ortası kullandıkça öde oranınız kullanılarak faturalandırılır. Sağlanan iş bünyesinde bir saat içinde 30.000 RU/s'nin altındaysa, o saat için ekstra ayrılmış kapasite boşa harcanır.

Saatlik kullanım deseninize göre satın alma önerilerini hesaplıyoruz. Son 7, 30 ve 60 gün içinde kullanım analiz edilir ve tasarruflarınızı en üst düzeye çıkaran ayrılmış kapasite satın alma önerilir. Önerilen rezervasyon boyutlarını Azure portalında aşağıdaki adımları kullanarak görüntüleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın.  

2. **Tüm Hizmetleri** > Seçin**Rezervasyonlar** > **Ekle**.

3. **Satınalma rezervasyonları** bölmesinden **Azure Cosmos DB'yi**seçin.

4. Önerilen rezervasyonları görüntülemek için **Önerilen** sekmeyi seçin:

Önerileri aşağıdaki özniteliklere göre filtreleyebilirsiniz:

- **Dönem** (1 yıl veya 3 yıl)
- **Faturalama sıklığı** (Aylık veya Peşin)
- **İşlem Türü** (RU's vs Multi-master RU's)

Ayrıca, önerileri tek bir kaynak grubunda, tek bir abonelikte veya Azure kaydınızın tamamında olacak şekilde kapsamdışı kullanabilirsiniz. 

Aşağıda bir örnek öneri verilmiştir:

![Ayrılmış Kapasite önerileri](./media/cosmos-db-reserved-capacity/reserved-capacity-recommendation.png)

30.000 RU/s rezervasyon satın almak için bu öneri, 3 yıllık rezervasyonlar arasında, 30.000 RU/s rezervasyon boyutunun tasarrufları en üst düzeye çıkaracağını gösterir. Bu durumda, öneri Azure Cosmos DB kullanımının son 30 gününe göre hesaplanır. Bu müşteri, son 30 günlük Azure Cosmos DB kullanımının gelecekteki kullanımı temsil etmesini beklerse, 30.000 RU/s rezervasyon satın alarak tasarrufları en üst düzeye çıkarır.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasite satın alın

1. [Azure portalında](https://portal.azure.com)oturum açın.  

2. **Tüm Hizmetleri** > Seçin**Rezervasyonlar** > **Ekle**.  

3. **Satınalma rezervasyonları** bölmesinden, yeni bir rezervasyon satın almak için **Azure Cosmos DB'yi** seçin.  

4. Aşağıdaki tabloda açıklandığı gibi gerekli alanları doldurun:

   ![Ayrılmış kapasite formunu doldurun](./media/cosmos-db-reserved-capacity/fill-reserved-capacity-form.png)

   |Alan  |Açıklama  |
   |---------|---------|
   |Kapsam   |   Rezervasyonla ilişkili fatura avantajını kaç aboneliğin kullanabileceğini kontrol eden seçenek. Ayrıca, rezervasyonun belirli aboneliklere nasıl uygulandığını da denetler. <br/><br/>  **Paylaşılan'u**seçerseniz, rezervasyon indirimi faturalandırma bağlamınızda herhangi bir abonelikte çalışan Azure Cosmos DB örneklerine uygulanır. Faturalandırma bağlamı, Azure'a nasıl kaydolduğunuza bağlıdır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan kullandıkça öde oranlarına sahip tüm bireysel aboneliklerdir.  <br/><br/>  **Tek abonelik**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Cosmos DB örneklerine uygulanır. <br/><br/> **Tek kaynak grubunu**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Cosmos DB örneklerine ve bu abonelik içindeki seçili kaynak grubuna uygulanır. <br/><br/> Rezerve edilen kapasiteyi satın aldıktan sonra rezervasyon kapsamını değiştirebilirsiniz.  |
   |Abonelik  |   Azure Cosmos DB ayrılmış kapasite için ödeme yapmak için kullanılan abonelik. Seçilen abonelikteki ödeme yöntemi, maliyetlerin tahsilinde kullanılır. Abonelik aşağıdaki türlerden biri olmalıdır: <br/><br/>  Kurumsal Sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P): Kurumsal abonelik için ücretler kaydın parasal taahhüt bakiyesinden düşülür veya fazla lık olarak tahsil edilir. <br/><br/> Kullandıkça öde oranlarına sahip bireysel abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P): Kullandıkça öde oranlarına sahip bireysel abonelik için ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.    |
   | Kaynak Grubu | Ayrılmış kapasite iskontosu uygulanan kaynak grubu. |
   |Sözleşme Dönemi  |   Bir ya da üç yıl.   |
   |İşlem Türü   |  Elde etme birimi istek birimleri olarak sağlanır. Her iki kurulum için sağlanan iş ortası için bir rezervasyon satın alabilirsiniz - tek bölge yanı sıra birden çok bölge yazıyor yazıyor. İş verme türü arasında seçim yapmak için iki değervardır: saatte 100 RU/s ve saatte 100 Çok analı RU/s.|
   | Ayrılmış Kapasite Birimleri| Rezerve etmek istediğiniz iş miktarı. Bölge başına tüm Cosmos DB kaynaklarınız (örneğin, veritabanları veya kapsayıcılar) için gereken iş verisini belirleyerek bu değeri hesaplayabilirsiniz. Daha sonra cosmos veritabanınızla ilişkilendireceğiniz bölgelerin sayısıyla çarparsınız. Örneğin: Her bölgede 1 milyon RU/sn içeren beş bölgeniz varsa, rezervasyon kapasitesi satın alımı için 5 milyon RU/sn seçin. |


5. Formu doldurduktan sonra, ayrılmış kapasiteyi satın almak için gereken fiyat hesaplanır. Çıktı, seçilen seçeneklerle aldığınız indirim yüzdesini de gösterir. Sonraki **tıklatseçin Seç**

6. **Satınalma rezervasyonları** bölmesinde, indirimve rezervasyon fiyatını inceleyin. Bu rezervasyon fiyatı, tüm bölgelerde sağlanan iş kaynağı yla Azure Cosmos DB kaynakları için geçerlidir.  

   ![Ayrılmış kapasite özeti](./media/cosmos-db-reserved-capacity/reserved-capacity-summary.png)

7. **Gözden Geçir + satın al'ı** seçin ve hemen satın **alın.** Satın alma işlemi başarılı olduğunda aşağıdaki sayfayı görürsünüz:

Bir rezervasyon satın aldıktan sonra, rezervasyon koşullarına uyan mevcut Azure Cosmos DB kaynaklarına hemen uygulanır. Varsa varsa, rezervasyon koşullarıyla eşleşen yeni bir Cosmos DB örneği dağıttığınızda rezervasyon geçerli olacaktır. Her iki durumda da, rezervasyon süresi başarılı bir satın alma işleminden hemen sonra başlar.

Rezervasyonunuz sona erdiğinde, Azure Cosmos DB örnekleriniz çalışmaya devam eder ve normal kullandıkça öde fiyatlarıyla faturalandırılır.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon indirimi, rezervasyon kapsamı ve öznitelikleriyle eşleşen Azure Cosmos DB kaynaklarına otomatik olarak uygulanır. Azure portalı, PowerShell, Azure CLI veya API aracılığıyla rezervasyonkapsamını güncelleştirebilirsiniz.

*  Azure Cosmos DB'ye ayrılmış kapasite indirimlerinin nasıl [uygulandığını](../cost-management-billing/reservations/understand-cosmosdb-reservation-charges.md)öğrenmek için bkz.

* Azure rezervasyonları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

   * [Azure Rezervasyonları nedir?](../cost-management-billing/reservations/save-compute-costs-reservations.md)  
   * [Azure rezervasyonlarını yönetme](../cost-management-billing/reservations/manage-reserved-vm-instance.md)  
   * [Kurumsal kaydınız için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)  
   * [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage.md)
   * [İş Merkezi CSP programında azure rezervasyonları](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)
