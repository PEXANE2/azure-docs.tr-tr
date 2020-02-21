---
title: Azure Sentinel ile şüpheli tehditleri algılamak için özel analitik kurallar oluşturma | Microsoft Docs
description: Azure Sentinel ile şüpheli tehditleri algılamak için özel analitik kurallar oluşturmayı öğrenmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: rkarlin
ms.openlocfilehash: c643c037506725b1a48588ca779d074b6aecf7c2
ms.sourcegitcommit: 934776a860e4944f1a0e5e24763bfe3855bc6b60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506101"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Öğretici: şüpheli tehditleri algılamak için özel analitik kurallar oluşturma

Veri kaynaklarınızı Azure Sentinel 'e [bağladıktan](quickstart-onboard.md) sonra, ortamınızda belirli kriterleri arayabilmeniz ve ölçütler eşleştirildiği zaman bir olay oluşturarak bu kuralları araştırabilmeniz için özel kurallar oluşturabilirsiniz. Bu öğretici, Azure Sentinel ile tehditleri algılamak için özel kurallar oluşturmanıza yardımcı olur.

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur.
> [!div class="checklist"]
> * Analitik kuralları oluşturma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="create-custom-analytic-rules"></a>Özel analitik kurallar oluşturma

Ortamınızda şüpheli olan tehditler ve bozukluklar için arama yapmanıza yardımcı olacak özel analitik kurallar oluşturabilirsiniz. Bu kural, tehditleri önceliklendirmenize, araştırmanıza ve düzeltebilmeniz için hemen bilgilendirildiğinizden emin olur.

1. Azure Sentinel altında Azure portal **analiz**' yı seçin.

1. Üstteki menü çubuğunda **+ Oluştur** ' u seçin ve **zamanlanan sorgu kuralı**' nı seçin. Bu, **analiz Kuralı sihirbazını**açar.

    ![Zamanlanmış sorgu oluştur](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. **Genel** sekmesinde, benzersiz bir **ad**ve bir **Açıklama**belirtin. **Tacler** alanında, kuralın sınıflandırılacağı saldırı kategorileri arasından seçim yapabilirsiniz. Uyarı **önem derecesini** gerektiği şekilde ayarlayın. Kuralı oluşturduğunuzda, **durumu** varsayılan olarak **etkindir** ve oluşturma işlemi tamamlandıktan sonra hemen çalışacağı anlamına gelir. Hemen çalışmasını istemiyorsanız, **devre dışı**' yı seçin ve kural **etkin kurallar** sekmesine eklenir ve ihtiyacınız olduğunda bu seçeneği etkinleştirebilirsiniz.

    ![Özel analitik kural oluşturmaya başlayın](media/tutorial-detect-threats-custom/general-tab.png)

1. **Kural mantığını ayarla** sekmesinde, doğrudan **kural sorgu** alanına bir sorgu yazabilir veya Log Analytics sorgu oluşturabilir ve ardından buraya kopyalayabilir ve yapıştırabilirsiniz.
 
   ![Azure Sentinel 'de sorgu oluştur](media/tutorial-detect-threats-custom/settings-tab.png)

   - Azure Sentinel 'in sorgunun üretebileceği sonuç sayısını (günlük olayları) gösterdiği, sorguyu yazarken ve yapılandırırken geçiş yaparken değiştiren **sonuçlar önizleme** alanına bakın. Grafik, **sorgu zamanlama** bölümündeki ayarlar tarafından belirlenen, tanımlanan süre içindeki sonuçların sayısını gösterir.
    - Sorgunuzun çok fazla veya çok sık uyarı tetikleyeceğini görürseniz, **uyarı eşiği** bölümünde bir taban çizgisi ayarlayabilirsiniz.

      İşte Azure etkinliğinde anormal sayıda kaynak oluşturulduğunda sizi uyaran örnek bir sorgu.

      `AzureActivity
     \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
     \| where ActivityStatus == "Succeeded"
     \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

      > [!NOTE]
      > Sorgu uzunluğu 1 ila 10.000 karakter arasında olmalıdır ve "Search \*" veya "Union \*" içeremez.

    1. Sorgu sonuçlarınızdan parametreleri Azure Sentinel tarafından tanınan varlıklara bağlamak için **varlıkları eşle** bölümünü kullanın. Bu varlıklar, **olay ayarları** sekmesinde uyarıların gruplandırılmasına dahil olmak üzere daha fazla çözümlemenin temelini oluşturur.
    1. **Sorgu zamanlaması** bölümünde aşağıdaki parametreleri ayarlayın:

       1. Sorgunun her 5 dakikada bir veya bir günde yaklaşık olarak ne sıklıkta çalıştırılacağını kontrol etmek için bir kez **çalıştırma sorgusu** ayarlayın.

       1. Sorgunun kapsadığı verilerin zaman dilimini öğrenmek için **en son ' dan arama verileri** ayarlama-Örneğin, son 10 dakikalık verileri veya son 6 saatlik veriyi sorgulayabilir.

       > [!NOTE]
       > Bu iki ayar, bir noktaya kadar birbirinden bağımsızdır. Bir sorguyu, zaman aralığından daha uzun bir süre (çakışan sorgulara sahip olan) kapsayan kısa bir aralıkta çalıştırabilirsiniz, ancak kapsam süresini aşan bir aralıkta bir sorgu çalıştıramazsınız, aksi takdirde, genel sorgu kapsamında boşluklar olur.

    1. Bir taban çizgisi tanımlamak için **uyarı eşiği** bölümünü kullanın. Örneğin, **sorgu sonucu sayısı daha büyük olduğunda uyarı üret** ' i ayarlayın ve kuralın yalnızca sorgu her çalıştığında en fazla 1000 sonuç oluşturursa bir uyarı oluşturmasını istiyorsanız 1000 numarasını girin. Bu gerekli bir alan olduğundan, bir taban çizgisi ayarlamak istemiyorsanız (yani, uyarının her olayı kaydetmesini istiyorsanız) sayı alanına 0 girin.

    1. **Gizleme** bölümünde, uyarı **oluşturulduktan sonra sorguyu çalıştırmayı durdur** **ayarı,** bir uyarı alındıktan sonra, bu kuralın işlemini sorgu aralığını aşan bir süre için askıya almak istediğinizde kullanabilirsiniz. Bunu açarsanız, 24 saate kadar, sorgunun çalışmayı durdurması gereken süre kadar olan **sorguyu durdur** olarak ayarlamanız gerekir.

1. **Olay ayarları** sekmesinde, Azure Sentinel 'in uyarıları eyleme dönüştürülebilir olaylara nasıl dönüşdiğini ve ne yapılacağını seçebilirsiniz. Bu sekme tek başına bırakılırsa, Azure Sentinel her uyarıdan tek ve ayrı bir olay oluşturur. Bu sekmedeki ayarları değiştirerek, herhangi bir olayın oluşturulmasını veya tek bir olayda birden çok uyarıyı gruplandırmadığının tercih edilebilir.

    1. **Olay ayarları** bölümünde, **Bu analiz kuralı tarafından tetiklenen uyarılardan olaylar oluşturun** , varsayılan olarak **etkin**olarak ayarlanır, yani Azure Sentinel, her biri ve kural tarafından tetiklenen her uyarıdan tek ve ayrı bir olay oluşturur.<br></br>Bu kuralın herhangi bir olay oluşturmaya neden olmasını istemiyorsanız (örneğin, bu kural sonraki Analize ilişkin bilgileri toplamak için), bunu **devre dışı**olarak ayarlayın.

    1. **Uyarı gruplama** bölümünde, benzer veya yinelenen uyarılar grubundan tek bir olayın oluşturulmasını istiyorsanız, **Bu analiz kuralı tarafından tetiklenen grup ile ilgili uyarıları** ayarlayın ve olayları **etkin**hale getirerek, aşağıdaki parametreleri ayarlayın.

    1. **Grubu seçili zaman çerçevesinde oluşturulan uyarılarla sınırlayın**:<br></br> Benzer veya yinelenen uyarıların birlikte gruplandırılacağı zaman çerçevesini belirleme. Bu zaman çerçevesinde karşılık gelen tüm uyarılar toplu olarak bir olay veya bir olay kümesi oluşturur (aşağıdaki gruplandırma ayarlarına bağlı olarak). Bu zaman dilimi dışındaki uyarılar, ayrı bir olay veya olay kümesi oluşturur.

    2. **Bu analiz kuralı tarafından tetiklenen uyarıları tek bir olaya göre Gruplandır**: uyarıların birlikte gruplandırılacağı temeli seçin:

        - **Tüm varlıkların eşleşmesi durumunda uyarıları tek bir olaya gruplayın**: <br></br>Bir eşlenen varlıkların her biri için özdeş değerleri paylaşıyorsa, uyarılar bir araya getirilir (Yukarıdaki kural mantığını ayarla sekmesinde tanımlanmıştır). Önerilen ayar budur.

        - **Bu kural tarafından tetiklenen tüm uyarıları tek bir olaya gruplayın**: <br></br>Bu kural tarafından oluşturulan tüm uyarılar özdeş değerler paylaşmasa bile birlikte gruplandırılır.

        - **Seçili varlıkların eşleşmesi durumunda uyarıları tek bir olaya gruplayın**: <br></br>Uyarı, eşlenmiş varlıkların bazıları için aynı değerleri paylaşıyorsa (açılan listeden seçim yapabilirsiniz) bir araya getirilir. Örneğin, kaynak veya hedef IP adreslerine göre ayrı olaylar oluşturmak istiyorsanız bu ayarı kullanmak isteyebilirsiniz.

    3. **Kapalı eşleşen olayları yeniden aç**: bir olay kapatılmışsa (temel alınan Sorun çözümlenmişse) ve daha sonra bu olaya gruplanmış başka bir uyarı oluşturulursa, kapatılan olayın yeniden açılmasını istiyorsanız bu ayarı **etkin** olarak ayarlayın ve uyarının yeni bir olay oluşturmasını istiyorsanız, **devre dışı** olarak bırakın.

1. **Otomatik yanıtlar** sekmesinde, özel kural tarafından bir uyarı oluşturulduğunda otomatik olarak çalıştırmak istediğiniz tüm PlayBook 'ları seçin. PlayBook 'ları oluşturma ve otomatikleştirme hakkında daha fazla bilgi için bkz. [tehditlere yanıt verme](tutorial-respond-threats-playbook.md).

1. Yeni uyarı kuralınızın tüm ayarlarını gözden geçirmek için **gözden geçir ve oluştur** ' u seçin ve ardından Oluştur ' u seçerek **Uyarı kuralınızı başlatın**.
  
1. Uyarı oluşturulduktan sonra, **etkin kurallar**altındaki tabloya özel bir kural eklenir. Bu listeden her kuralı etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.

1. Oluşturduğunuz uyarı kurallarının sonuçlarını görüntülemek için **Olaylar** sayfasına gidin; burada, olayları önceliklendirebilirsiniz, [araştırın](tutorial-investigate-cases.md)ve tehditleri düzeltin.


> [!NOTE]
> Azure Sentinel 'de oluşturulan uyarılar [Microsoft Graph güvenliği](https://aka.ms/securitygraphdocs)aracılığıyla kullanılabilir. Daha fazla bilgi için [Microsoft Graph güvenlik uyarıları belgelerine](https://aka.ms/graphsecurityreferencebetadocs)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel kullanarak tehditleri algılamaya nasıl başlacağınız hakkında daha fazla öğrenirsiniz.

Tehditlere yönelik yanıtlarınızı otomatik hale getirmeyi öğrenmek için [Azure Sentinel 'de otomatik tehdit yanıtları ayarlayın](tutorial-respond-threats-playbook.md).

