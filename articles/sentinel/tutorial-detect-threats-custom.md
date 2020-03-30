---
title: Azure Sentinel| ile şüpheli tehditleri algılamak için özel analitik kurallar oluşturun| Microsoft Dokümanlar
description: Azure Sentinel ile şüpheli tehditleri algılamak için özel analitik kuralları nasıl oluşturacağımı öğrenmek için bu öğreticiyi kullanın.
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
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: cea7429ecea105355b0afe306bfa334e55d5d9c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585116"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Öğretici: Şüpheli tehditleri algılamak için özel analitik kurallar oluşturun

Veri [kaynaklarınızı](quickstart-onboard.md) Azure Sentinel'e bağladıktan sonra, ortamınızda belirli ölçütleri arayabilen ve ölçütler eşleştiğinde olaylar oluşturabilen özel kurallar oluşturabilir ve böylece bunları araştırabilirsiniz. Bu öğretici, Azure Sentinel ile tehditleri algılamak için özel kurallar oluşturmanıza yardımcı olur.

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur.
> [!div class="checklist"]
> * Analitik kurallar oluşturma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="create-custom-analytic-rules"></a>Özel analitik kurallar oluşturun

Ortamınızda şüpheli olan tehdit ve anormallik türlerini aramanıza yardımcı olmak için özel analitik kurallar oluşturabilirsiniz. Kural, tehditleri hemen üçe katmanız, araştırmanız ve düzeltmeniz için size hemen bilgi verilmesine neden olmaktadır.

1. Azure Sentinel altındaki Azure portalında **Analytics'i**seçin.

1. Üst menü çubuğunda **+Oluştur'u** seçin ve **Zamanlanmış sorgu kuralını**seçin. Bu, **Analytics kural sihirbazını**açar.

    ![Zamanlanmış sorgu oluşturma](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. **Genel** sekmesinde, benzersiz bir **Ad**ve **Açıklama**sağlar. **Taktikler** alanında, kuralı sınıflandırmak için saldırı kategorileri arasından seçim yapabilirsiniz. Uyarı **Önem Derecesi'ni** gerektiği gibi ayarlayın. Kuralı oluşturduğunuzda, **durumu** varsayılan olarak **etkinleştirilir,** bu da siz onu oluşturmayı bitirdikten hemen sonra çalışacağı anlamına gelir. Hemen çalışmasını istemiyorsanız, **Devre Dışı'yı**seçin ve kural Etkin **kurallar** sekmenize eklenir ve ihtiyacınız olduğunda buradan etkinleştirebilirsiniz.

    ![Özel bir analitik kural oluşturmaya başlayın](media/tutorial-detect-threats-custom/general-tab.png)

1. Kural **kuralı sekmesinde,** bir sorguyu doğrudan **Kural sorgu** alanına yazabilir veya Log Analytics'te sorguyu oluşturabilir ve ardından kopyalayıp yapıştırabilirsiniz.
 
   ![Azure Sentinel'de sorgu oluşturma](media/tutorial-detect-threats-custom/settings-tab.png)

   - Azure Sentinel'in sorgunun oluşturacağı sonuç sayısını (günlük olayları) gösterdiği, sorgunuzu yazarken ve yapılandırırken anında değiştiği sağdaki **Sonuçlar önizleme** alanına bakın. Grafik, **Sorgu zamanlama** bölümündeki ayarlartarafından belirlenen tanımlı zaman dilimindeki sonuç sayısını gösterir.
    - Sorgunuzun çok fazla veya çok sık uyarı tetiklediğini görürseniz, **Uyarı eşik** bölümünde bir taban çizgisi ayarlayabilirsiniz.

      Azure Etkinliği'nde anormal sayıda kaynak oluşturulduğunda sizi uyaracak örnek bir sorgu aşağıda veda edebilirsiniz.

      `AzureActivity
     \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
     \| where ActivityStatus == "Succeeded"
     \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

      > [!NOTE]
      > Sorgu uzunluğu 1 ile 10.000 karakter arasında olmalıdır \*ve "arama" veya "birliktelik" \*içeremez.

    1. Sorgu sonuçlarınızdaki parametreleri Azure Sentinel tarafından tanınan varlıklara bağlamak için **Harita varlıkları** bölümünü kullanın. Bu varlıklar, uyarıların **Olay ayarları** sekmesindeki olaylara gruplandırması da dahil olmak üzere daha fazla analiz için temel oluşturur.
    1. Sorgu **zamanlama** bölümünde aşağıdaki parametreleri ayarlayın:

       1. Sorgunun ne sıklıkta çalıştırıldığını denetlemek için **Çalıştır sorgusunu her** 5 dakikada bir veya günde bir kez olduğu kadar seyrek olarak ayarlayın.

       1. Sorgu kapsamındaki verilerin zaman dilimini belirlemek için **sondaki Arama verilerini** ayarlayın - örneğin, son 10 dakika veriyi veya son 6 saatlik veriyi sorgulayabilir.

       > [!NOTE]
       > Bu iki ayar bir birbirlerinden bağımsızdır. Bir sorguyu, aralıktan daha uzun bir süreyi kapsayan kısa bir aralıkta çalıştırabilirsiniz (aslında çakışan sorgulara sahip olmak), ancak sorguyu kapsama süresini aşan bir aralıkta çalıştıramazsınız, aksi takdirde genel sorgu kapsamında boşluklar olur.

    1. Bir taban çizgisini tanımlamak için **Uyarı eşiği** bölümünü kullanın. Örneğin, sorgu sonuçları nın sayısı **daha büyük** olduğunda **uyarı oluştur'u** ayarlayın ve kuralın yalnızca sorgu her çalıştığında 1000'den fazla sonuç üretirse bir uyarı oluşturmasını istiyorsanız 1000 sayısını girin. Bu gerekli bir alan olduğundan, bir taban çizgisi ayarlamak istemiyorsanız – yani, uyarınızın her olayı kaydetmesini istiyorsanız – sayı alanına 0 girin.

    1. **Bastırma** bölümünde, bir uyarı aldıktan sonra bu kuralın çalışmasını sorgu aralığını aşan bir süre için askıya almak **istiyorsanız, uyarı oluşturulduktan sonra** sorguyu durdurun **ayarı** açabilirsiniz. Bunu açarsanız, sorguyu durdurma **işlemini** 24 saate kadar durdurmasüresine ayarlamanız gerekir.

1. Olay **Ayarları** sekmesinde, Azure Sentinel'in uyarıları eyleme geçirilebilir olaylara dönüştürüp dönüştürmeyeceğini ve nasıl dönüştüreceğini seçebilirsiniz. Bu sekme tek başına bırakılırsa, Azure Sentinel her uyarıdan ayrı bir olay oluşturur. Bu sekmedeki ayarları değiştirerek hiçbir olayın oluşturulmamasını veya birden fazla uyarıyı tek bir olaya gruplandırmayı seçebilirsiniz.

    1. Olay **Ayarları** bölümünde, **bu analiz kuralı tarafından tetiklenen uyarılardan olay oluşturma** varsayılan olarak **Etkin**olarak ayarlanır , yani Azure Sentinel kural tarafından tetiklenen her uyarıdan ayrı bir olay oluşturur.<br></br>Bu kuralın herhangi bir olayın oluşmasına neden olmasını istemiyorsanız (örneğin, bu kural yalnızca sonraki çözümleme için bilgi toplamak içinse), bunu **Devre Dışı Bırakılmış**olarak ayarlayın.

    1. Uyarı **gruplandırma** bölümünde, benzer veya yinelenen uyarılardan oluşan bir gruptan tek bir olayın oluşturulmasını istiyorsanız, **bu analiz kuralı tarafından tetiklenen Grup la ilgili uyarıları** **Etkin**olaylara ayarlayın ve aşağıdaki parametreleri ayarlayın.

    1. **Grubu seçili zaman dilimi içinde oluşturulan uyarılarla sınırlandırın:**<br></br> Benzer veya yinelenen uyarıların birlikte gruplandırılabilmek için zaman dilimini belirleyin. Bu zaman dilimi içinde karşılık gelen uyarıların tümü topluca bir olay veya bir dizi olay oluşturur (aşağıdaki gruplandırma ayarlarına bağlı olarak). Bu zaman diliminin dışındaki uyarılar ayrı bir olay veya bir dizi olay oluşturur.

    2. **Bu analiz kuralı tarafından tetiklenen grup uyarıları:** Uyarıların birlikte gruplandırılacak temeli seçin:

        - **Tüm varlıklar eşleşirse, grup uyarıları tek bir olaya dönüşür:** <br></br>Eşlenen varlıkların her biri için aynı değerleri paylaşıyorlarsa (yukarıdaki Kural Kuralı sekmesinde tanımlanan) uyarılar birlikte gruplanır. Bu önerilen ayardır.

        - **Bu kuralın tetiklediği tüm uyarıları tek bir olaya gruplandırın:** <br></br>Bu kural tarafından oluşturulan tüm uyarılar, aynı değerleri paylaşmasalar bile birlikte gruplandırılır.

        - **Seçili varlıklar eşleşirse, grup uyarıları tek bir olaya dönüşür:** <br></br>Eşlenen varlıkların bazıları için aynı değerleri paylaşıyorlarsa (açılan listeden seçebileceğiniz) uyarılar birlikte gruplanır. Örneğin, kaynak veya hedef IP adreslerine dayalı ayrı olaylar oluşturmak istiyorsanız, bu ayarı kullanmak isteyebilirsiniz.

    3. **Kapalı eşleştirme olaylarını yeniden aç**: Bir olay kapatıldıysa (yani altta yatan sorun çözüldüyse) ve daha sonra bu olaya gruplandırılan başka bir uyarı oluşturulursa, kapalı olayın yeniden açılmasını istiyorsanız bu ayarı **Etkin** olarak ayarlayın ve uyarının yeni bir olay oluşturmasını istiyorsanız **Devre Dışı** bırakın.

1. Otomatik **yanıtlar** sekmesinde, özel kural tarafından bir uyarı oluşturulduğunda otomatik olarak çalıştırmak istediğiniz playbook'ları seçin. Oyun kitapları oluşturma ve otomatikleme hakkında daha fazla bilgi için [tehditlere yanıt ver'](tutorial-respond-threats-playbook.md)konusu bilgi için bkz.

1. **Gözden Geçir'i** seçin ve yeni uyarı kuralınızın tüm ayarlarını gözden geçirmek için oluşturun ve ardından **uyarı kuralınızı başlatmayı oluşturmak'ı**seçin.
  
1. Uyarı oluşturulduktan sonra, **Etkin kurallar**altında tabloya özel bir kural eklenir. Bu listeden her kuralı etkinleştirebilir, devre dışı edebilir veya silebilirsiniz.

1. Oluşturduğunuz uyarı kurallarının sonuçlarını görüntülemek için, olayları üçleme yapabileceğiniz, [olayları araştırabileceğiniz](tutorial-investigate-cases.md)ve tehditleri düzeltebileceğiniz **Olaylar** sayfasına gidin.


> [!NOTE]
> Azure Sentinel'de oluşturulan uyarılar [Microsoft Graph Security](https://aka.ms/securitygraphdocs)aracılığıyla kullanılabilir. Daha fazla bilgi için [Microsoft Graph Security uyarıları belgelerine](https://aka.ms/graphsecurityreferencebetadocs)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Sentinel'i kullanarak tehditleri algılamaya nasıl başlabildiğinizi öğrendiniz.

Tehditlere karşı yanıtlarınızı nasıl otomatikleştirerinizi öğrenmek için [Azure Sentinel'de otomatik tehdit yanıtları ayarlayın.](tutorial-respond-threats-playbook.md)

