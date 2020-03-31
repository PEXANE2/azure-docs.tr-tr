---
title: Azure Hizmet Veri Servisi ad alanlarını geçirin - standart- premium
description: Mevcut Azure Hizmet Veri Servisi standart ad alanlarının premium'a geçişine izin verme kılavuzu
services: service-bus-messaging
documentationcenter: ''
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2019
ms.author: aschhab
ms.openlocfilehash: 27e3260b91bebee14ff12188a7dbd6c7cf76355c
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385036"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-the-premium-tier"></a>Varolan Azure Hizmet Veri Servisi standart ad alanlarını premium katmana geçirin

Azure Hizmet Veri Servisi önceden yalnızca standart katmanda ad alanları sunmaktadır. Ad alanları, düşük iş ortası ve geliştirici ortamları için optimize edilmiş çok kiracılı kurulumlardır. Premium katman, öngörülebilir gecikme süremi ve sabit bir fiyata artan iş miktarı için ad alanı başına özel kaynaklar sunar. Premium katman, ek kurumsal özellikler gerektiren yüksek üretim ve üretim ortamları için optimize edilmiştir.

Bu makalede, varolan standart katman ad alanlarını premium katmana nasıl geçirilir açıklanmaktadır.  

>[!WARNING]
> Geçiş, Servis Veri Servisi standart ad alanlarının premium katmana yükseltilmesi için tasarlanmıştır. Geçiş aracı aşağılamayı desteklemez.

Dikkat edilmesi gereken noktalardan bazıları:

- Bu geçiş, varolan gönderen ve alıcı uygulamaları nın **kod veya yapılandırmada herhangi bir değişiklik gerektirmemesi**anlamına gelen bir yerde gerçekleşmesi içindir. Varolan bağlantı dizesi otomatik olarak yeni premium ad alanına işaret edecektir.
- Geçişin başarılı olaması için **premium** ad alanının içinde **hiçbir varlık** olmalıdır.
- Standart ad alanındaki tüm **varlıklar** geçiş işlemi sırasında premium ad alanına **kopyalanır.**
- Geçiş, premium katmandaki **ileti birimi başına 1.000 varlığı** destekler. Kaç ileti birimine ihtiyacınız olduğunu belirlemek için, geçerli standart ad alanınızdaki varlık sayısıyla başlayın.
- **Temel katmandan** **premium katmana**doğrudan geçiş yapamazsınız, ancak bunu bir sonraki adımda temelden standarda, sonra da standarttan premium'a geçirerek dolaylı olarak yapabilirsiniz.

## <a name="migration-steps"></a>Geçiş adımları

Bazı koşullar geçiş işlemiyle ilişkilidir. Hata olasılığını azaltmak için aşağıdaki adımları kendinize tanıtın. Bu adımlar geçiş işlemini ana hatlar ve adım adım ayrıntıları izleyen bölümlerde listelenir.

1. Yeni bir premium ad alanı oluşturun.
1. Standart ve premium ad alanlarını birbiriyle eşleştirin.
1. Standarttan premium ad alanına eşitleme (copy-over) varlıkları.
1. Geçişi gerçekleştirin.
1. Ad alanının geçiş sonrası adını kullanarak standart ad alanındaki varlıkları boşaltın.
1. Standart ad alanını silin.

>[!IMPORTANT]
> Geçiş yapıldıktan sonra, eski standart ad alanına erişin ve kuyrukları ve abonelikleri boşaltın. İletiler boşaltıldıktan sonra, alıcı uygulamaları tarafından işlenecek yeni premium ad alanına gönderilebilir. Kuyruklar ve abonelikler boşaltıldıktan sonra, eski standart ad alanını silmenizi öneririz.

### <a name="migrate-by-using-the-azure-cli-or-powershell"></a>Azure CLI veya PowerShell'i kullanarak geçiş

Azure CLI veya PowerShell aracını kullanarak Hizmet Veri Yolundan standart ad alanınızı premium'a geçirmek için aşağıdaki adımları izleyin.

1. Yeni bir Service Bus premium ad alanı oluşturun. Azure Kaynak [Yöneticisi şablonlarına](service-bus-resource-manager-namespace.md) başvuruyapabilir veya [Azure portalını kullanabilirsiniz.](service-bus-create-namespace-portal.md) **ServiceBusSku** parametresi için **premium** seçtiğinizden emin olun.

1. Geçiş komutlarını basitleştirmek için aşağıdaki ortam değişkenlerini ayarlayın.

   ```
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the standard namespace>
   ```

    >[!IMPORTANT]
    > Geçiş sonrası geçiş ekibe/adı (post_migration_dns_name) eski standart ad alanı sonrası geçişe erişmek için kullanılır. Kuyrukları ve abonelikleri boşaltmak ve ardından ad alanını silmek için bunu kullanın.

1. Standart ve premium ad alanlarını eşleştirin ve aşağıdaki komutu kullanarak eşitlemeye başlayın:

    ```azurecli-interactive
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```

1. Aşağıdaki komutu kullanarak geçiş durumunu denetleyin:

    ```azurecli-interactive
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    Aşağıdaki değerleri gördüğünüzde geçiş tamamlanmış olarak kabul edilir:

    * MigrationState = "Etkin"
    * beklemeÇoğaltmaİşlemleriSay sayı = 0
    * provisioningState = "Başarılı"

    Bu komut, geçiş yapılandırmasını da görüntüler. Değerlerin doğru ayarlandığından emin olun. Ayrıca, tüm kuyrukların ve konuların oluşturulduğundan ve standart ad alanında var olanlarla eşleştiklerinden emin olmak için portaldaki premium ad alanını denetleyin.

1. Aşağıdaki tam komutu çalıştırarak geçiş iveder:

   ```azurecli-interactive
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-by-using-the-azure-portal"></a>Azure portalını kullanarak geçiş

Azure portalını kullanarak geçiş, komutları kullanarak geçişle aynı mantıksal akışa sahiptir. Azure portalını kullanarak geçiş yapmak için aşağıdaki adımları izleyin.

1. Sol bölmedeki **Gezinti** menüsünde, **premium'a Geçir'i**seçin. Bir sonraki sayfaya devam etmek için **Başlat** düğmesini tıklatın.
    ![Göç Açılış Sayfası][]

1. **Kurulumu Tamamlayın.**
   ![Kurulum ad alanı][]
   1. Varolan standart ad alanını geçirmek için premium ad alanını oluşturun ve atayın.
        ![Kurulum ad alanı - premium ad alanı oluşturma][]
   1. Geçiş **Sonrası adı**seçin. Geçiş tamamlandıktan sonra standart ad alanına erişmek için bu adı kullanırsınız.
        ![Kurulum ad alanı - geçiş sonrası adı seçin][]
   1. Devam etmek için **'Sonraki'yi** seçin.
1. Varlıkları standart ve premium ad alanları arasında eşitleme.
    ![Kurulum ad alanı - eşitleme varlıkları - başlat][]

   1. Varlıkları eşitlemeye başlamak için **Eşitlemeyi Başlat'ı** seçin.
   1. Eşitlemeyi onaylamak ve başlatmak için iletişim kutusunda **Evet'i** seçin.
   1. Eşitleme tamamlanana kadar bekleyin. Durum durumu çubuğunda kullanılabilir.
        ![Kurulum ad alanı - eşitleme varlıkları - ilerleme][]
        >[!IMPORTANT]
        > Geçişi herhangi bir nedenle iptal etmeniz gerekiyorsa, lütfen bu belgenin SSS bölümündeki iptal akışını gözden geçirin.
   1. Eşitleme tamamlandıktan sonra sayfanın altındaki **İleri'yi** seçin.

1. Özet sayfasındaki değişiklikleri gözden geçirin. Ad boşluklarını değiştirmek ve geçişi tamamlamak için **Geçişi Tamamla'yı** seçin.
    ![Ad alanını değiştir - geçiş menüsü][]  
    Geçiş tamamlandığında onay sayfası görüntülenir.
    ![Anahtar namespace - başarı][]

## <a name="caveats"></a>Uyarılar

Azure Hizmet Veri Hizmeti Veri Hizmeti Standart katmanı tarafından sağlanan özelliklerden bazıları Azure Hizmet Veri Servisi Premium katmanı tarafından desteklenmez. Bunlar, premium katman öngörülebilir iş sonu ve gecikme için özel kaynaklar sunduğundan, tasarım gereğidir.

Burada Premium ve onların azaltma tarafından desteklenmeyen özelliklerin bir listesi -

### <a name="express-entities"></a>İfade varlıkları

   Depolamaya ileti verisi işlememiş ekspres varlıklar Premium'da desteklenmez. Özel kaynaklar, herhangi bir kurumsal ileti sisteminden beklendiği gibi verilerin kalıcı olmasını sağlarken önemli bir iş elde etme artışı sağlar.

   Geçiş sırasında, Standart ad alanınızdaki herhangi bir ekspres varlık, Premium ad alanında ifade edilmeyen bir varlık olarak oluşturulur.

   Azure Kaynak Yöneticisi (ARM) şablonlarını kullanıyorsanız, otomatik iş akışlarınızın hatasız çalışması için lütfen dağıtım yapılandırmasından 'enableExpress' bayrağını kaldırdığınızdan emin olun.

### <a name="partitioned-entities"></a>Bölümlenen varlıklar

   Bölümlenmiş varlıklar, çok kiracılı bir kurulumda daha iyi kullanılabilirlik sağlamak için Standart katmanda desteklendi. Premium katmanında ad alanı başına özel kaynakların sağlanması yla, artık buna gerek yoktur.

   Geçiş sırasında, Standart ad alanında bölümlenmiş herhangi bir varlık, Premium ad alanında bölümlenmemiş bir varlık olarak oluşturulur.

   ARM şablonunuzun belirli bir Kuyruk veya Konu için 'enablePartitioning' ile 'true' olarak belirlediği nde, bu şablon aracı tarafından yoksayılır.

## <a name="faqs"></a>SSS

### <a name="what-happens-when-the-migration-is-committed"></a>Geçiş sözlediğinde ne olur?

Geçiş ayrıldıktan sonra, standart ad alanına işaret eden bağlantı dizesi premium ad alanına işaret eder.

Gönderen ve alıcı uygulamaları standart Ad alanı bağlantısını keser ve premium ad alanına otomatik olarak yeniden bağlanır.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Premium geçiş standardı tamamlandıktan sonra ne yapmalıyım?

Premium geçiş standardı, konular, abonelikler ve filtreler gibi varlık meta verilerinin standart ad alanından premium ad alanına kopyalanmasını sağlar. Standart ad alanına işlenen ileti verileri standart ad alanından premium ad alanına kopyalanır.

Standart ad alanında, geçiş devam ederken gönderilen ve işlenen bazı iletiler olabilir. Bu iletileri standart Ad Alanı'ndan el ile boşaltın ve bunları premium Namespace'e el ile gönderin. İletileri el ile boşaltmak için, geçiş komutlarında belirttiğiniz Geçiş Sonrası DNS adını kullanarak standart ad alanı varlıklarını tüketen bir konsol uygulaması veya komut dosyası kullanın. Bu iletileri, alıcılar tarafından işlenebilmeleri için premium ad alanına gönderin.

İletiler boşaltıldıktan sonra standart ad alanını silin.

>[!IMPORTANT]
> Standart ad alanından gelen iletiler boşaltıldıktan sonra, standart ad alanını silin. Başlangıçta standart ad alanına başvuran bağlantı dizesi artık premium ad alanına atıfta bulunduğundan, bu önemlidir. Artık standart Ad alanına ihtiyacınız olmayacak. Geçiş yaptığınız standart ad alanını silerken daha sonraki karışıklığı azaltır.

### <a name="how-much-downtime-do-i-expect"></a>Ne kadar kesinti bekliyorum?

Geçiş işlemi, uygulamalar için beklenen kapalı kalma süresini azaltmak içindir. Kapatma süresi, gönderen ve alıcı uygulamalarının yeni premium ad alanına işaret etmek için kullandığı bağlantı dizesi kullanılarak azaltılır.

Uygulama tarafından karşılaşılan kapalı kalma süresi, DNS girişini güncelleştirmek için gereken süreyle sınırlıdır. Çalışma süresi yaklaşık 5 dakikadır.

### <a name="do-i-have-to-make-any-configuration-changes-while-doing-the-migration"></a>Geçiş yaparken yapılandırma değişikliği yapmam gerekiyor mu?

Hayır, geçiş yapmak için gerekli kod veya yapılandırma değişikliği yok. Gönderen ve alıcı uygulamalarının standart Ad alanına erişmek için kullandığı bağlantı dizesi, premium ad alanının diğer adı olarak kullanılmak üzere otomatik olarak eşlenir.

### <a name="what-happens-when-i-abort-the-migration"></a>Göçü iptal ettiğimde ne olur?

Geçiş, `Abort` komutu kullanarak veya Azure portalını kullanarak iptal edilebilir.

#### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace
```

#### <a name="azure-portal"></a>Azure portalında

![İptal akışı - abort][]
![senkronizasyon Abort akışı - iptal tam][]

Geçiş işlemi iptal edildiğinde, varlıkları (konular, abonelikler ve filtreler) standarttan premium ad alanına kopyalama işlemini iptal eder ve eşleştirmeyi ayırır.

Bağlantı dizesi premium ad alanına işaret etmek için güncelleştirilemiyor. Varolan uygulamalarınız, geçişi başlatmadan önce olduğu gibi çalışmaya devam eder.

Ancak, premium ad alanındaki varlıkları veya premium ad alanını silmez. Geçişe devam etmemeye karar verdiyseniz varlıkları el ile silin.

>[!IMPORTANT]
> Geçişi iptal etmeye karar verirseniz, kaynaklar için ücretlendirilmemek için geçiş için sağladığınız premium Ad alanını silin.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>Mesajları boşaltmak zorunda kalmak istemiyorum. Ne yapmalıyım?

Geçiş gerçekleşirken ve geçiş taahhüt edilmeden hemen önce gönderen uygulamalar tarafından gönderilen ve standart Ad alanı üzerindeki depolamaya adanmış iletiler olabilir.

Geçiş sırasında, gerçek ileti verileri/yükü standarttan premium ad alanına kopyalanır. İletilerin el ile boşaltılması ve daha sonra premium ad alanına gönderilmesi gerekir.

Ancak, planlı bir bakım/temizlik penceresi sırasında geçiş yapabilir ve iletileri el ile boşaltmak ve göndermek istemiyorsanız aşağıdaki adımları izleyin:

1. Gönderen uygulamaları durdurun. Alıcı uygulamaları, şu anda standart ad alanında olan iletileri işleyecek ve sırayı boşaltacaktır.
1. Standart Ad alanında ki kuyruklar ve abonelikler boş aldıktan sonra, standarttan premium ad alanına geçişi yürütmek için daha önce açıklanan yordamı izleyin.
1. Geçiş tamamlandıktan sonra gönderen uygulamaları yeniden başlatabilirsiniz.
1. Gönderenler ve alıcılar artık otomatik olarak premium ad alanına bağlanır.

    >[!NOTE]
    > Geçiş için alıcı uygulamalarını durdurmanız gerekmez.
    >
    > Geçiş tamamlandıktan sonra, alıcı uygulamaları standart ad alanından kesilir ve otomatik olarak premium ad alanına bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

* Standart ve [birinci sınıf Mesajlaşma arasındaki farklar](./service-bus-premium-messaging.md)hakkında daha fazla bilgi edinin.
* [Servis Veri Yolu primi için Yüksek Kullanılabilirlik ve Coğrafi Durum kurtarma yönleri](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium)hakkında bilgi edinin.

[Göç Açılış Sayfası]: ./media/service-bus-standard-premium-migration/1.png
[Kurulum ad alanı]: ./media/service-bus-standard-premium-migration/2.png
[Kurulum ad alanı - premium ad alanı oluşturma]: ./media/service-bus-standard-premium-migration/3.png
[Kurulum ad alanı - geçiş sonrası adı seçin]: ./media/service-bus-standard-premium-migration/4.png
[Kurulum ad alanı - eşitleme varlıkları - başlat]: ./media/service-bus-standard-premium-migration/5.png
[Kurulum ad alanı - eşitleme varlıkları - ilerleme]: ./media/service-bus-standard-premium-migration/8.png
[Ad alanını değiştir - geçiş menüsü]: ./media/service-bus-standard-premium-migration/9.png
[Anahtar namespace - başarı]: ./media/service-bus-standard-premium-migration/12.png

[İptal akışı - skrondisi iptal et]: ./media/service-bus-standard-premium-migration/abort1.png
[İptal akışı - iptal tam]: ./media/service-bus-standard-premium-migration/abort3.png
