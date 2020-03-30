---
title: Azure Sentinel ile ilgili olayları araştırma| Microsoft Dokümanlar
description: Azure Sentinel ile ilgili olayları nasıl araştıracağımıöğrenmek için bu öğreticiyi kullanın.
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
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: ecd8c508d05bfeb541a6cb5efbcdf2fffd3c78d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587201"
---
# <a name="tutorial-investigate-incidents-with-azure-sentinel"></a>Öğretici: Azure Sentinel ile ilgili olayları araştırma

> [!IMPORTANT]
> Soruşturma grafiği şu anda genel önizlemede.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


Bu öğretici, Azure Sentinel ile ilgili olayları araştırmanıza yardımcı olur. Veri kaynaklarınızı Azure Sentinel'e bağladıktan sonra, şüpheli bir şey olduğunda bilgilendirilmek istersiniz. Azure Sentinel, bunu yapmanızı sağlamak için atayabileceğiniz ve araştırabileceğiniz olaylar oluşturan gelişmiş uyarı kuralları oluşturmanıza olanak tanır.

Bu makale şunları kapsamaktadır:
> [!div class="checklist"]
> * Olayları araştırma
> * Araştırma grafiğini kullanma
> * Tehditlere yanıt verme

Bir olay birden çok uyarı içerebilir. Belirli bir soruşturma için gerekli tüm kanıtların toplanması. Bir olay, **Analytics** sayfasında oluşturduğunuz analitik kurallara göre oluşturulur. Önem derecesi ve durum gibi uyarılarla ilgili özellikler olay düzeyinde ayarlanır. Azure Sentinel'e ne tür tehditler aradığınızı ve bunları nasıl bulacağınızı söyledikten sonra, olayları araştırarak algılanan tehditleri izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Olayı yalnızca analitik kuralınızı ayarladığınızda varlık eşleme alanlarını kullandıysanız araştırabilirsiniz. Soruşturma grafiği, orijinal olayınızın varlıkları içerdiğini gerektirir.

## <a name="how-to-investigate-incidents"></a>Olayları araştırma

1. **Olaylar'ı**seçin. **Olaylar** sayfası, kaç tane olay olduğunuzu, kaç tanesinin açık olduğunu, devam **etmekte**olan ların kaç tanesini ayarladığınızı ve kaç tanesinin kapalı olduğunu bilmenizi sağlar. Her olay için, olayın meydana geldiği zamanı ve olayın durumunu görebilirsiniz. Önce hangi olayların ele alınacağına karar vermek için önem derecesine bakın.

    ![Olay önem derecelerini görüntüleme](media/tutorial-investigate-cases/incident-severity.png)

1. Olayları gerektiği gibi filtreleyebilirsiniz, örneğin duruma veya önem derecesine göre.

1. Bir soruşturma başlatmak için belirli bir olay seçin. Sağda, olayın ciddiyeti, ilgili varlık sayısının özeti, bu olayı tetikleyen ham olaylar ve olayın benzersiz kimliği gibi ayrıntılı bilgileri görebilirsiniz.

1. Olaydaki uyarılar ve varlıklar hakkında daha fazla ayrıntı görüntülemek için, olay **sayfasındaki tüm ayrıntıları** görüntüle'yi seçin ve olay bilgilerini özetleyen ilgili sekmeleri gözden geçirin. **Uyarılar** sekmesinde, uyarının kendisini gözden geçirin. Uyarı yla ilgili tüm ilgili bilgileri görebilirsiniz – uyarıyı tetikleyen sorgu, sorgu başına döndürülen sonuç sayısı ve uyarılarda oyun defterlerini çalıştırma olanağı. Olayın daha da ayrıntılı bilgi sini yapmak **için, Olayların**sayısını seçin. Bu, Log Analytics'te sonuçları ve uyarıyı tetikleyen olayları oluşturan sorguyu açar. **Varlıklar** sekmesinde, eşlediğiniz tüm varlıkları uyarı kuralı tanımının bir parçası olarak görebilirsiniz.

    ![Uyarı ayrıntılarını görüntüleme](media/tutorial-investigate-cases/alert-details.png)

1. Bir olayı etkin bir şekilde araştırıyorsanız, siz olayı kapatana kadar olayın durumunu **devam ediyor** olarak ayarlamak iyi bir fikirdir.

1. Olaylar belirli bir kullanıcıya atanabilir. Her olay **için, Olay sahibi** alanını ayarlayarak bir sahip atayabilirsiniz. Tüm olaylar atanmamış olarak başlar. Ayrıca, diğer analistlerin neyi araştırdığınızı ve olayla ilgili endişelerinizi anlayabilmeleri için yorum da ekleyebilirsiniz.

    ![Kullanıcıya olay atama](media/tutorial-investigate-cases/assign-incident-to-user.png)

1. Araştırma haritasını görüntülemek için **Araştır'ı** seçin.

## <a name="use-the-investigation-graph-to-deep-dive"></a>Derin dalış için araştırma grafiğini kullanın

Soruşturma grafiği, analistlerin her soruşturma için doğru soruları sormasını sağlar. Araştırma grafiği, ilgili verileri ilgili herhangi bir varlıkla ilişkilendirerek olası bir güvenlik tehdidinin kapsamını anlamanıza ve temel nedenini belirlemenize yardımcı olur. Daha derine dalabilir ve grafikte sunulan herhangi bir varlığı seçerek ve farklı genişletme seçenekleri arasında seçim yaparak araştırabilirsiniz.  
  
Araştırma grafiği size şunları sağlar:

- **Ham verilerden görsel bağlam**: Ham verilerden otomatik olarak çıkarılan canlı, görsel grafik, varlık ilişkilerini görüntüler. Bu, farklı veri kaynakları arasındaki bağlantıları kolayca görmenizi sağlar.

- **Tam araştırma kapsamı bulma**: Bir ihlalin tüm kapsamını ortaya çıkarmak için dahili arama sorgularını kullanarak araştırma kapsamınızı genişletin.

- **Dahili araştırma adımları**: Bir tehdit karşısında doğru soruları sorduğunuzdan emin olmak için önceden tanımlanmış keşif seçeneklerini kullanın.

Araştırma grafiğini kullanmak için:

1. Bir olay seçin, ardından **Araştır'ı**seçin. Bu seni soruşturma grafiğine götürür. Grafik, doğrudan uyarıya bağlı varlıkların ve daha fazla bağlanan her kaynağın açıklayıcı bir eşlenişlerini sağlar.

   > [!IMPORTANT] 
   > Olayı yalnızca analitik kuralınızı ayarladığınızda varlık eşleme alanlarını kullandıysanız araştırabilirsiniz. Soruşturma grafiği, orijinal olayınızın varlıkları içerdiğini gerektirir.

   ![Haritayı görüntüleme](media/tutorial-investigate-cases/map1.png)

1. **Varlıklar** bölmesini açmak için bir varlık seçin, böylece bu varlıkla ilgili bilgileri gözden geçirebilirsiniz.

    ![Varlıkları haritada görüntüleme](media/tutorial-investigate-cases/map-entities.png)
  
1. Soruşturmanızı derinleştirmek için güvenlik uzmanlarımız ve analistlerimiz tarafından tasarlanmış bir soru listesini ortaya çıkarmak için her bir tarafın üzerinde gezinerek araştırmanızı genişletin. Biz bu seçenekleri **arama sorguları**diyoruz.

    ![Daha fazla ayrıntıyı keşfedin](media/tutorial-investigate-cases/exploration-cases.png)

   Örneğin, bir bilgisayarda ilgili uyarıları isteyebilirsiniz. Bir arama sorgusu seçerseniz, ortaya çıkan hak grafiklere geri eklenir. Bu örnekte, **İlgili uyarıları** seçmek grafiğe aşağıdaki uyarıları döndürmüştur:

    ![İlgili uyarıları görüntüleme](media/tutorial-investigate-cases/related-alerts.png)

1. Her keşif sorgusu için, **\>Etkinlikler'i**seçerek ham olay sonuçlarını ve Log Analytics'te kullanılan sorguyu açma seçeneğini seçebilirsiniz.

1. Olayı anlamak için grafik size paralel bir zaman çizelgesi verir.

    ![Zaman çizelgesini haritada görüntüleme](media/tutorial-investigate-cases/map-timeline.png)

1. Grafikteki hangi şeylerin zaman içinde hangi noktada oluştuğunu görmek için zaman çizelgesinin üzerine gidin.

    ![Uyarıları araştırmak için haritadaki zaman çizelgesini kullanma](media/tutorial-investigate-cases/use-timeline.png)



## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure Sentinel'i kullanarak olayları araştırmaya nasıl başlabildiğinizi öğrendiniz. [Otomatik oyun kitaplarını kullanarak tehditlere nasıl yanıt verilebildiğini](tutorial-respond-threats-playbook.md)öğrenin.
> [!div class="nextstepaction"]
> Tehditlere yanıtlarınızı otomatikleştirmek için [tehditlere yanıt verin.](tutorial-respond-threats-playbook.md)

