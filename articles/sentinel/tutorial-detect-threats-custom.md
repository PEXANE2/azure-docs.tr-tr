---
title: Azure Sentinel ile tehditleri algılamak için özel analiz kuralları oluşturma | Microsoft Docs
description: Azure Sentinel ile güvenlik tehditlerini algılamak üzere özel analiz kuralları oluşturmayı öğrenmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2020
ms.author: yelevin
ms.openlocfilehash: 0e5989490603e22745a8bc972b16ed016c894893
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605909"
---
# <a name="tutorial-create-custom-analytics-rules-to-detect-threats"></a>Öğretici: tehditleri algılamak için özel analiz kuralları oluşturma

[Veri kaynaklarınızı](quickstart-onboard.md)   Azure Sentinel 'e bağladıktan sonra, ortamınızda belirli kriterleri arayabilmeniz ve ölçüt eşleştirildiği zaman bunları araştırabilmeniz için olaylar oluşturabileceğiniz özel kurallar oluşturabilirsiniz. Bu öğretici, Azure Sentinel ile tehditleri algılamak için özel kurallar oluşturmanıza yardımcı olur.

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur.
> [!div class="checklist"]
> * Analiz kuralları oluşturma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="create-custom-analytics-rules"></a>Özel analiz kuralları oluşturma

Ortamınızda şüpheli olan tehditler ve bozukluklar için arama yapmanıza yardımcı olması için özel analiz kuralları oluşturabilirsiniz. Bu kural, tehditleri önceliklendirmenize, araştırmanıza ve düzeltebilmeniz için hemen bilgilendirildiğinizden emin olur.

1. Azure Sentinel altında Azure portal **analiz**' yı seçin.

1. Üstteki menü çubuğunda **+ Oluştur** ' u seçin ve **zamanlanan sorgu kuralı**' nı seçin. Bu, **analiz Kuralı sihirbazını**açar.

    :::image type="content" source="media/tutorial-detect-threats-custom/create-scheduled-query.png" alt-text="Zamanlanmış sorgu oluştur":::

1. **Genel** sekmesinde, benzersiz bir **ad** ve **Açıklama**sağlayın. **Tacler** alanında, kuralın sınıflandırılacağı saldırı kategorileri arasından seçim yapabilirsiniz. Uyarı **önem derecesini** gerektiği şekilde ayarlayın. Kuralı oluşturduğunuzda, **durumu** varsayılan olarak **etkindir** ve oluşturma işlemi tamamlandıktan sonra hemen çalışacağı anlamına gelir. Hemen çalışmasını istemiyorsanız, **devre dışı**' yı seçin ve kural **etkin kurallar** sekmesine eklenir ve ihtiyacınız olduğunda bu seçeneği etkinleştirebilirsiniz.

    ![Özel analiz kuralı oluşturmaya başlayın](media/tutorial-detect-threats-custom/general-tab.png)

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
      > Sorgu uzunluğu 1 ila 10.000 karakter arasında olmalıdır ve "Search \* " veya "Union \* " içeremez.

    1. Sorgu sonuçlarınızdan parametreleri Azure Sentinel tarafından tanınan varlıklara bağlamak için **varlıkları eşle** bölümünü kullanın. Bu varlıklar, **olay ayarları** sekmesinde uyarıların gruplandırılmasına dahil olmak üzere daha fazla çözümlemenin temelini oluşturur.
  
    1. **Sorgu zamanlaması** bölümünde aşağıdaki parametreleri ayarlayın:

       1. Sorgunun her 5 dakikada bir veya bir günde yaklaşık olarak ne sıklıkta çalıştırılacağını kontrol etmek için bir kez **çalıştırma sorgusu** ayarlayın.

       1. Sorgunun kapsadığı verilerin zaman dilimini öğrenmek için **en son ' dan arama verileri** ayarlama-Örneğin, son 10 dakikalık verileri veya son 6 saatlik veriyi sorgulayabilir.

       > [!NOTE]
       > Bu iki ayar, bir noktaya kadar birbirinden bağımsızdır. Bir sorguyu, zaman aralığından daha uzun bir süre (çakışan sorgulara sahip olan) kapsayan kısa bir aralıkta çalıştırabilirsiniz, ancak kapsam süresini aşan bir aralıkta bir sorgu çalıştıramazsınız, aksi takdirde, genel sorgu kapsamında boşluklar olur.

    1. Bir taban çizgisi tanımlamak için **uyarı eşiği** bölümünü kullanın. Örneğin, **sorgu sonucu sayısı şundan büyük olduğunda uyarı üret** ' i **Is greater than** ayarlayın ve kuralın yalnızca sorgu her çalıştığında 1000 ' den fazla sonuç döndürürse bir uyarı oluşturmasını istiyorsanız 1000 numarasını girin. Bu gerekli bir alandır. bu nedenle, bir taban çizgisi ayarlamak istemiyorsanız (yani, uyarının her olayı kaydetmesini istiyorsanız) sayı alanına 0 girin.
    
    1. **Olay gruplandırması**altında, **olayların** gruplandırılmasına yönelik iki yönden birini **seçin:** 

       - **Tüm olayları tek bir uyarıya gruplandırın** (varsayılan ayar). Sorgu, yukarıda belirtilen **uyarı eşiğine** göre daha fazla sonuç döndürdüğü sürece kural her çalıştığında tek bir uyarı oluşturur. Uyarı, sonuçlarda döndürülen tüm olayların bir özetini içerir. 

       - **Her olay için bir uyarı tetikleyin**. Kural, sorgu tarafından döndürülen her olay için benzersiz bir uyarı oluşturur. Olayların tek tek görüntülenmesini istiyorsanız veya kullanıcıları, ana bilgisayar adı ya da başka bir şeye göre belirli parametrelere göre gruplandırmak istiyorsanız bu kullanışlıdır. Bu parametreleri sorguda tanımlayabilirsiniz.
    
       Şu anda bir kuralın oluşturabileceği uyarı sayısı 20 ' nin üzerinde olabilir. Belirli bir kuralda **olay gruplama** , **her olay için bir uyarı tetikleyecek**şekilde ayarlanır ve kuralın sorgusu 20 ' den fazla olay döndürürse, ilk 19 olayın her biri benzersiz bir uyarı oluşturur ve yirminci uyarısı döndürülen olayların tüm kümesini özetler. Diğer bir deyişle, yirminci uyarısı, **tüm olayları Grup altında tek bir uyarı** seçeneğinde oluşturulan şeydir.

       > [!NOTE]
       > **Olaylar** ve **Uyarılar**arasındaki fark nedir?
       >
       > - **Olay** , tek bir oluşumun açıklamasıdır. Örneğin, bir günlük dosyasındaki tek bir girdi olay olarak sayabilir. Bu bağlamda bir olay, bir analiz kuralındaki sorgu tarafından döndürülen tek bir sonuca başvurur.
       >
       > - **Uyarı** , bir güvenlik açısından önemli olan, bir araya gelen olaylar koleksiyonudur. Olayda önemli güvenlik etkileri varsa, bir uyarı tek bir olay içerebilir; Örneğin, bir yabancı ülkeden ofis saatleri dışında bir yönetim oturum açma.
       >
       > - Bu şekilde, **Olaylar**nelerdir? Azure Sentinel 'in iç mantığı, **uyarılardan** veya uyarı gruplarından **Olaylar** oluşturur. Olaylar kuyruğu, analistlerin iş önceliklendirme, araştırma ve düzeltme için odak noktasıdır.
       > 
       > Azure Sentinel, bazı veri kaynaklarından gelen ham olayları ve diğer kaynaklardan zaten işlenmiş uyarıları alır. İstediğiniz zaman hangi bir ilgilendiğiniz hakkında dikkat etmeniz önemlidir.

       > [!IMPORTANT]
       > Olay gruplandırması Şu anda genel önizleme aşamasındadır. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
    
    1. **Gizleme** bölümünde, uyarı **oluşturulduktan sonra sorguyu çalıştırmayı durdur** **ayarı,** bir uyarı alındıktan sonra, bu kuralın işlemini sorgu aralığını aşan bir süre için askıya almak istediğinizde kullanabilirsiniz. Bunu açarsanız, 24 saate kadar, sorgunun çalışmayı durdurması gereken süre kadar olan **sorguyu durdur** olarak ayarlamanız gerekir.

1. **Olay ayarları** sekmesinde, Azure Sentinel 'in uyarıları eyleme dönüştürülebilir olaylara nasıl dönüşdiğini ve ne yapılacağını seçebilirsiniz. Bu sekme tek başına bırakılırsa, Azure Sentinel her uyarıdan tek ve ayrı bir olay oluşturur. Bu sekmedeki ayarları değiştirerek, herhangi bir olayın oluşturulmasını veya tek bir olayda birden çok uyarıyı gruplandırmadığının tercih edilebilir.

   > [!IMPORTANT]
   > Olay ayarları sekmesi şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
    
    1. **Olay ayarları** bölümünde, **Bu analiz kuralı tarafından tetiklenen uyarılardan olaylar oluşturun** , varsayılan olarak **etkin**olarak ayarlanır, yani Azure Sentinel, her biri ve kural tarafından tetiklenen her uyarıdan tek ve ayrı bir olay oluşturur.
       - Bu kuralın herhangi bir olay oluşturmaya neden olmasını istemiyorsanız (örneğin, bu kural sonraki Analize ilişkin bilgileri toplamak için), bunu **devre dışı**olarak ayarlayın.

    1. **Uyarı gruplama** bölümünde, tek bir olayın 150 benzer veya yinelenen uyarılarla (bkz. nota) oluşturulmasını istiyorsanız, **Bu analiz kuralı tarafından tetiklenen, grup ile ilgili uyarıları** ve aşağıdaki parametreleri ayarlayın. **Enabled**

    - **Grubu seçilen zaman çerçevesinde oluşturulan uyarılarla sınırlayın**: benzer veya yinelenen uyarıların birlikte gruplandırılacağı zaman çerçevesini belirleme. Bu zaman çerçevesinde karşılık gelen tüm uyarılar toplu olarak bir olay veya bir olay kümesi oluşturur (aşağıdaki gruplandırma ayarlarına bağlı olarak). Bu zaman dilimi dışındaki uyarılar, ayrı bir olay veya olay kümesi oluşturur.

    - **Bu analiz kuralı tarafından tetiklenen uyarıları tek bir olaya göre Gruplandır**: uyarıların birlikte gruplandırılacağı temeli seçin:

        - **Tüm varlıkların eşleşmesi durumunda uyarıları tek bir olaya gruplayın**: her eşlenmiş varlıkların (Yukarıdaki kural mantığını ayarla sekmesinde tanımlanan) her biri için aynı değerleri paylaşıyorsa, uyarılar birlikte gruplandırılır. Önerilen ayar budur.

        - **Bu kural tarafından tetiklenen tüm uyarıları tek bir olaya grupla**: Bu kural tarafından oluşturulan tüm uyarılar, özdeş değerler paylaşmasa bile birlikte gruplandırılır.

        - **Seçili varlıkların eşleşmesi durumunda uyarıları tek bir olaya gruplayın**: uyarılar, eşlenmiş varlıkların bazıları için özdeş değerleri paylaşıyorsa (açılan listeden seçim yapabilirsiniz). Örneğin, kaynak veya hedef IP adreslerine göre ayrı olaylar oluşturmak istiyorsanız bu ayarı kullanmak isteyebilirsiniz.

    - **Kapalı eşleşen olayları yeniden aç**: bir olay çözümlenirse ve kapatılmışsa ve daha sonra bu olaya ait olması gereken başka bir uyarı varsa, bu ayarı devre dışı bırakmak istiyorsanız **etkin** olarak ayarlayın ve uyarının yeni bir olay oluşturmasını istiyorsanız **devre dışı** bırakın.
    
        > [!NOTE]
        > En çok 150 uyarı tek bir olay halinde gruplandırılabilir. Tek bir olaya gruplandıran bir kural tarafından 150 ' den fazla uyarı oluşturulursa, orijinalle aynı olay ayrıntıları ile yeni bir olay oluşturulur ve fazlalık uyarılar yeni olaya göre gruplandırılır.

1. **Otomatik yanıtlar** sekmesinde, özel kural tarafından bir uyarı oluşturulduğunda otomatik olarak çalıştırmak istediğiniz tüm PlayBook 'ları seçin. PlayBook 'ları oluşturma ve otomatikleştirme hakkında daha fazla bilgi için bkz. [tehditlere yanıt verme](tutorial-respond-threats-playbook.md).

1. Yeni uyarı kuralınızın tüm ayarlarını gözden geçirmek için **gözden geçir ve oluştur** ' u seçin ve ardından Oluştur ' u seçerek **Uyarı kuralınızı başlatın**.
  
1. Uyarı oluşturulduktan sonra, **etkin kurallar**altındaki tabloya özel bir kural eklenir. Bu listeden her kuralı etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.

1. Oluşturduğunuz uyarı kurallarının sonuçlarını görüntülemek için **Olaylar** sayfasına gidin; burada, olayları önceliklendirebilirsiniz, [araştırın](tutorial-investigate-cases.md)ve tehditleri düzeltin.


> [!NOTE]
> Azure Sentinel 'de oluşturulan uyarılar [Microsoft Graph güvenliği](https://aka.ms/securitygraphdocs)aracılığıyla kullanılabilir. Daha fazla bilgi için [Microsoft Graph güvenlik uyarıları belgelerine](https://aka.ms/graphsecurityreferencebetadocs)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel kullanarak tehditleri algılamaya nasıl başlacağınız hakkında daha fazla öğrenirsiniz.

Tehditlere yönelik yanıtlarınızı otomatik hale getirmeyi öğrenmek için [Azure Sentinel 'de otomatik tehdit yanıtları ayarlayın](tutorial-respond-threats-playbook.md).

