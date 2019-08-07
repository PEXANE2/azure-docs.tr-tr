---
title: Azure Sentinel önizleme ile uyarıları araştırın | Microsoft Docs
description: Azure Sentinel ile uyarıları araştırmaya yönelik bilgi edinmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2019
ms.author: rkarlin
ms.openlocfilehash: 2cc33a9ac55ae9e906d88b72476d4b5ee244d2c8
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780417"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Öğretici: Azure Sentinel önizleme ile tehditleri algılama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Veri kaynaklarınızı](quickstart-onboard.md) Azure Sentinel 'e bağladıktan sonra, şüpheli bir sorun olduğunda bildirim almak istersiniz. Bunu yapmanızı sağlamak için Azure Sentinel, ortamınızdaki aykırları ve tehditleri derinlemesine araştırmak için atayabileceğiniz ve kullanabileceğiniz olaylar üreten gelişmiş uyarı kuralları oluşturmanıza olanak sağlar. 

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur.
> [!div class="checklist"]
> * Algılama kuralları oluşturma
> * Tehdit yanıtlarını otomatikleştirin

## <a name="create-detection-rules"></a>Algılama kuralları oluşturma

Olayları araştırmak için, önce algılama kuralları oluşturmanız gerekir. 

> [!NOTE]
> Azure Sentinel 'de oluşturulan uyarılar [Microsoft Graph güvenliği](https://aka.ms/securitygraphdocs)aracılığıyla kullanılabilir. Daha fazla ayrıntı ve Tümleştirme iş ortakları için [Microsoft Graph güvenlik uyarıları belgelerine](https://aka.ms/graphsecurityreferencebetadocs) bakın.

Algılama kuralları, ortamınızda, araştırılması ve düzeltildiklerinden emin olmak istediğiniz, ortamınızda şüpheli olabilecek tehditler ve bozukluklar türlerini temel alır. 

1. Azure Sentinel altında Azure portal **analiz**' yı seçin.

   ![Analiz](./media/tutorial-detect-threats/alert-rules.png)

2. Üstteki menü çubuğunda **+ Ekle**' ye tıklayın.  

   ![Uyarı kuralı oluştur](./media/tutorial-detect-threats/create-alert-rule.png)

3. **Uyarı kuralı oluştur**altında açıklayıcı bir ad girin ve **önem derecesini** gerektiği gibi ayarlayın. 

4. Log Analytics sorguyu oluşturun ve **Uyarı kuralı ayarla** alanına yapıştırın. İşte Azure etkinliğinde anormal sayıda kaynak oluşturulduğunda sizi uyaran örnek bir sorgu.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

   > [!NOTE]
   > Sorgu uzunluğu 1 ila 10000 karakter arasında olmalıdır ve "Search *" ve "Union *" karakterlerini içeremez.


5. **Varlık eşleme** bölümünde, Sorgunuzdaki sütunları Azure Sentinel tarafından tanınan varlık alanlarıyla eşlemek için **varlık türü** altındaki alanları kullanın. Her alan için Log Analytics oluşturduğunuz sorgudaki ilgili sütunu uygun varlık alanına eşleyin. **Özellik**altında ilgili sütun adını seçin. Her varlık birden çok alan içerir, örneğin SID, GUID, vb. Varlığı yalnızca üst düzey varlık değil, alanlardan herhangi birine göre eşleyebilirsiniz.

6. **Uyarı tetikleyicisi**altında uyarı tetikleyicisi koşullarını tanımlayın. Bu, uyarıyı tetikleyen koşulları tanımlar. 

7. Sorgunun her 5 dakikada bir ve bir günde yaklaşık olarak ne sıklıkta çalıştırılacağını belirleme **sıklığını** ayarlayın. 

8. Sorgunun ne kadar veri üzerinde çalıştığı için zaman penceresini denetlemek üzere **dönemi** ayarlayın. örneğin, 60 dakikalık veriler arasında her saat çalışabilir.

9. **Gizleme**de ayarlayabilirsiniz. Aynı olay için yinelenen uyarıların tetiklenmesini durdurmak istediğinizde gizleme yararlı olur. Bu şekilde, belirli bir süre içinde uyarıların tetiklenmesi durdurabilirsiniz. Bu, aynı olay için yinelenen uyarıların oluşmasını önlemenize ve bir süre boyunca birbirini izleyen uyarıların görüntülenmesini sağlamanıza yardımcı olabilir. Örneğin, **Uyarı zamanlama** **sıklığı** 60 dakikaya ayarlanmışsa ve **Uyarı zamanlama süresi** iki saate ayarlanırsa ve sorgu sonuçları tanımlanan eşiği geçtiğinde, ilk algılandığında bir kez uyarı tetikleyecektir. Son 60 dakika boyunca ve örneklendiği 2 saatlik verinin ilk 60 dakikadır. Bir uyarı tetikleniyorsa, gizleme uyarı döneminde ayarlanan süre için olmalıdır. Bizim örneğimizde, uyarıların yalnızca en son saatte gerçekleşen olaylar için tetiklenmesi için 60 dakika boyunca gizleme ayarlamak isteyebilirsiniz.

8. Sorgunuzu **uyarı kuralına ayarla** alanına yapıştırdıktan sonra, uyarı için belirli bir zaman aralığı boyunca ne kadar veri oluşturulacağını anlayabilmeniz Için, **mantıksal uyarı simülasyonuna** göre uyarının benzetimini hemen görebilirsiniz. oluşturdunuz. Bu, **Sıklık** ve **eşik**için ayarlandığınıza bağlıdır. Bu durumda, uyarılarınızın çok sık tetikleneceğini görürseniz, sonuç sayısını ortalama taban çizgisinin üstünde olacak şekilde ayarlamak isteyeceksiniz.

9. Uyarı kuralınızı başlatmak için **Oluştur** ' a tıklayın. Uyarı oluşturulduktan sonra, uyarıyı içeren bir olay oluşturulur. Tanımlı algılama kurallarını, **Güvenlik analizi** sekmesinde satır olarak görebilirsiniz. Ayrıca, her bir kuralla ilgili eşleşme sayısını görebilirsiniz. uyarılar tetiklenir. Bu listeden her kuralı etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz. Ayrıca, her bir uyarının satır sonundaki üç nokta (...) simgesini sağ seçerek seçebilirsiniz, devre dışı bırakabilir, kopyalayabilir, eşleşmeleri gösterebilir veya bir kuralı silebilirsiniz. **Analiz** sayfası, etkinleştirdiğiniz şablonlar ve şablonlar temelinde oluşturduğunuz uyarı kuralları da dahil olmak üzere tüm etkin uyarı kurallarınızın bir galerisidir.

1. Uyarı kurallarının sonuçları, **Olaylar** sayfasında görülebilir; burada, olayları önceliklendirebilirsiniz, [araştırabilir](tutorial-investigate-cases.md)ve tehditleri düzeltebilirsiniz.



## <a name="automate-threat-responses"></a>Tehdit yanıtlarını otomatikleştirin

SıEM/SOC ekipleri, düzenli olarak güvenlik uyarılarını açığa kaldırmamalıdır. Oluşturulan uyarıların hacmi çok büyük olduğundan, kullanılabilir güvenlik yöneticileri bu kadar çok önemlidir. Bu, birçok uyarının araştırılamama durumlarında çok sık sonuçlar elde ederek, kuruluşun fark edilmemiş saldırılara karşı savunmasız bırakılır. 

Çoğu, bu uyarıların çoğu, belirli ve tanımlı düzeltme eylemleri tarafından giderilebildiğiniz yinelenen desenlerle uyumlu değildir. Azure Sentinel, zaten PlayBook 'lar için düzeltmeyi tanımlamanızı sağlar. Ayrıca, belirli güvenlik uyarılarına yönelik olarak tanımlanan yanıtı tamamen otomatikleştirmenizi sağlamak için PlayBook tanımınızın bir parçası olarak gerçek zamanlı Otomasyonu ayarlamak da mümkündür. Gerçek zamanlı Otomasyon kullanarak, yanıt ekipleri yinelenen uyarı türlerine yönelik rutin yanıtları tamamen otomatikleştirerek iş yükünü önemli ölçüde azaltabilir. böylece, benzersiz uyarılarda daha fazla odaklanmanızı, desenleri analiz etmeyi, tehdit arayanı ve daha fazlasını yapabilirsiniz.

Yanıtları otomatikleştirmek için:

1. Yanıtı otomatik hale getirmek istediğiniz uyarıyı seçin.
1. Azure Sentinel çalışma alanı gezinti menüsünde **analiz**' ı seçin.
1. Otomatikleştirmek istediğiniz uyarıyı seçin. 
1. **Uyarı kuralını Düzenle** sayfasında, **gerçek zamanlı Otomasyon**altında, bu uyarı kuralı eşleştiğinde çalıştırmak istediğiniz **tetiklenen PlayBook** ' u seçin.
1. **Kaydet**’i seçin.

   ![gerçek zamanlı Otomasyon](./media/tutorial-detect-threats/rt-configuration.png)


Ayrıca, uyarı içinden PlayBook çalıştırarak, PlayBook 'ları **görüntüle** ' ye tıklayıp çalıştırmak için bir PlayBook ' u seçerek bir uyarıyı el ile düzeltebilirsiniz. Yeni bir PlayBook oluşturmayı veya var olan bir görüntünün nasıl düzenlendiğini öğrenmek için bkz. [Azure Sentinel 'de PlayBook 'ları ile çalışma](tutorial-respond-threats-playbook.md).



## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Sentinel kullanarak tehditleri algılamaya nasıl başlacağınız hakkında daha fazla öğrenirsiniz. 

Tehditleri yanıtlarınızı otomatik hale getirmeyi öğrenmek için [Otomatikleştirilmiş PlayBook 'ları kullanarak tehditlere nasıl yanıt verileceğini](tutorial-respond-threats-playbook.md)öğrenin.
> [!div class="nextstepaction"]
> Tehditlere yönelik yanıtlarınızı otomatik hale getirmek için [tehditleri yanıtlayın](tutorial-respond-threats-playbook.md) .

