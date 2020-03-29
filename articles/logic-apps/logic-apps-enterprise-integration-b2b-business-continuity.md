---
title: Tümleştirme hesapları için olağanüstü durum kurtarma
description: Azure Mantık Uygulamaları'nda bölgeler arası olağanüstü durum kurtarma yla entegrasyon hesaplarınızı ve B2B yapılarınızı ayarlama
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/10/2017
ms.openlocfilehash: 09b77862ad3379efeb8b3063a9d6c60b062ca2d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905131"
---
# <a name="set-up-cross-region-disaster-recovery-for-integration-accounts-in-azure-logic-apps"></a>Azure Logic Apps'taki tümleştirme hesapları için bölgeler arası olağanüstü durum kurtarma ayarlama

B2B iş yükleri, sipariş ler ve faturalar gibi para hareketlerini içerir. Bir felaket olayı sırasında, bir işletmenin iş ortaklarıyla üzerinde anlaştığı iş düzeyindeki SLA'ları karşılamak için hızla toparlanması çok önemlidir. Bu makalede, B2B iş yükleri için bir iş sürekliliği planı oluşturmak için nasıl gösterir. 

* Olağanüstü Durum Kurtarma hazırlık 
* Bir felaket olayı sırasında ikincil bölgeye geçeme 
* Bir felaket olayından sonra birincil bölgeye geri dön

## <a name="disaster-recovery-readiness"></a>Olağanüstü Durum Kurtarma hazırlık  

1. İkincil bir bölge tanımlayın ve ikincil bölgede bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) oluşturun.

2. Çalışma durumunun ikincil bölge tümleştirme hesabına çoğaltılması gereken gerekli ileti akışları için ortaklar, şemalar ve anlaşmalar ekleyin.

   > [!TIP]
   > Tümleştirme hesabı artifakı'nın bölgeler arasında adlandırma kuralında tutarlılık olduğundan emin olun. 

3. Birincil bölgeden çalışma durumunu çekmek için ikincil bölgede bir mantık uygulaması oluşturun. 

   Bu mantık uygulaması bir *tetikleyici* ve bir *eylem*olmalıdır. 
   Tetikleyici birincil bölge tümleştirme hesabına bağlanmalı ve eylem ikincil bölge tümleştirme hesabına bağlanmalıdır. 
   Zaman aralığına bağlı olarak, tetikleyici birincil bölge çalıştırma durum tablosunu yoklar ve varsa yeni kayıtları çeker. Eylem bunları ikincil bölge tümleştirme hesabına güncelleştirir. 
   Bu, birincil bölgeden ikincil bölgeye artımlı çalışma zamanı durumu elde etmeye yardımcı olur.

4. Logic Apps entegrasyon hesabındaiş süreklilik B2B protokollerini temel almak üzere tasarlanmıştır - X12, AS2 ve EDIFACT. Ayrıntılı adımları bulmak için ilgili bağlantıları seçin.

5. Öneri, tüm birincil bölge kaynaklarını da ikincil bir bölgeye dağıtmaktır. 

   Birincil bölge kaynakları arasında Azure SQL Veritabanı veya Azure Cosmos DB, Mesajlaşma için kullanılan Azure Hizmet Veri Mes'leri ve Azure Etkinlik Hub'ları, Azure API Yönetimi ve Azure Uygulama Hizmeti'ndeki Azure Mantıksal Uygulamaları bulunmaktadır.   

6. Birincil bölgeden ikincil bir bölgeye bağlantı kurun. Birincil bir bölgeden çalışma durumunu çekmek için, ikincil bir bölgede bir mantık uygulaması oluşturun. 

   Mantık uygulaması bir tetikleyici ve bir eylem olmalıdır. 
   Tetikleyici birincil bölge tümleştirme hesabına bağlanmalıdır. 
   Eylem, ikincil bir bölge tümleştirme hesabına bağlanmalıdır. 
   Zaman aralığına bağlı olarak, tetikleyici birincil bölge çalıştırma durum tablosunu yoklar ve varsa yeni kayıtları çeker. 
   Eylem bunları ikincil bir bölge tümleştirme hesabına güncelleştirir. 
   Bu işlem, birincil bölgeden ikincil bölgeye artımlı çalışma zamanı durumu elde etmeye yardımcı olur.

Logic Apps entegrasyon hesabındaki iş sürekliliği, B2B protokolleri X12, AS2 ve EDIFACT'e dayalı destek sağlar. X12 ve AS2'yi kullanma yla ilgili ayrıntılı adımlar için bu makalede [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) ve [AS2'ye](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) bakın.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Bir felaket olayı sırasında ikincil bir bölgeye geçeme

Bir felaket olayı sırasında, birincil bölge iş sürekliliği için kullanılamadığında, trafiği ikincil bölgeye yönlendirin. İkincil bir bölge, bir işletmenin, ortakları tarafından kararlaştırılan RPO/RTO'yu karşılamak için işlevleri hızla kurtarmasına yardımcı olur. Ayrıca, bir bölgeden diğerine başarısız olmak için çabaları en aza indirir. 

Denetim numaralarını birincil bölgeden ikincil bir bölgeye kopyalarken beklenen bir gecikme vardır. Bir felaket olayı sırasında ortaklara yinelenen oluşturulan denetim numaralarını göndermemek için, öneri [PowerShell cmdlets](https://docs.microsoft.com/powershell/module/azurerm.logicapp/set-azurermintegrationaccountgeneratedicn?view=azurermps-6.13.0)kullanarak ikincil bölge anlaşmalarında kontrol numaralarını artım etmektir.

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Afet sonrası bir birincil bölgeye geri dön

Kullanılabilir olduğunda birincil bölgeye geri dönmek için aşağıdaki adımları izleyin:

1. İkinci bölgedeki iş ortaklarından gelen iletileri kabul etmeyi durdurun.  

2. [PowerShell cmdlets](https://docs.microsoft.com/powershell/module/azurerm.logicapp/set-azurermintegrationaccountgeneratedicn?view=azurermps-6.13.0)kullanarak tüm birincil bölge anlaşmaları için oluşturulan kontrol numaralarını artım.  

3. İkinci bölgeden birincil bölgeye doğrudan trafik.

4. Birincil bölgeden çalıştırma durumunu çekmek için ikincil bölgede oluşturulan mantık uygulamasının etkin olup olmadığını denetleyin.

## <a name="x12"></a>X12 

EDI X12 belgeleri için iş sürekliliği kontrol numaralarına dayanmaktadır:

> [!TIP]
> Ayrıca mantık uygulamaları oluşturmak için [X12 hızlı başlatma şablonu](https://azure.microsoft.com/resources/templates/201-logic-app-b2b-disaster-recovery-replication/) kullanabilirsiniz. Birincil ve ikincil tümleştirme hesapları oluşturmak, şablonu kullanmak için ön koşullardır. Şablon, biri alınan denetim numaraları ve diğeri oluşturulan denetim numaraları için olmak üzere iki mantık uygulaması oluşturmaya yardımcı olur. İlgili tetikleyiciler ve eylemler, tetikleyiciyi birincil tümleştirme hesabına ve eylemi ikincil tümleştirme hesabına bağlayarak mantık uygulamalarında oluşturulur.

**Ön koşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek için, X12 anlaşmasının Alma Ayarları'ndaki yinelenen denetim ayarlarını seçin.

![Yinelenen denetim ayarlarını seçin](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. İkincil bir bölgede [bir mantık uygulaması](../logic-apps/quickstart-create-first-logic-app-workflow.md) oluşturun.    

2. **X12'de**arama yapın ve **X12'yi seçin - Bir kontrol numarası değiştirildiğinde.**   

   ![X12 ara](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   Tetikleyici, bir tümleştirme hesabına bağlantı kurmanızı ister. 
   Tetikleyici birincil bölge tümleştirme hesabına bağlanmalıdır.

3. Bir bağlantı adı girin, listeden *birincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin.   

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. Denetim numarası eşitleme ayarını **başlatmak için DateTime** isteğe bağlıdır. **Sıklık,** bir aralıkla **Gün,** **Saat,** **Dakika**veya **Saniye** olarak ayarlanabilir.   

   ![DateTime ve Sıklık](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. **Yeni adım** > seçin**Eylem ekle.**

   ![Yeni adım, eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. **X12'de**arama yapın ve **X12'yi seçin - Kontrol numaraları ekleyin veya güncelleyin.**   

   ![Denetim numaraları ekleme veya güncelleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Bir eylemi ikincil bölge tümleştirme hesabına bağlamak için, kullanılabilir tümleştirme hesaplarının listesi > **için** **bağlantıyı Değiştir'i**seçin. Bir bağlantı adı girin, listeden *ikincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin. 

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Sağ üst köşedeki simgeye tıklayarak ham girişlere geçin.

   ![Ham girişlere geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Dinamik içerik seçiciden Gövde'yi seçin ve mantık uygulamasını kaydedin.

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Zaman aralığına bağlı olarak, tetikleyici birincil bölge denetim numarası tablosunu aldı ve yeni kayıtları çeker. 
   Eylem, ikincil bölge tümleştirme hesabındaki kayıtları güncelleştirir. 
   Güncelleştirme yoksa, tetikleyici durumu **Atlanan**olarak görünür.   

   ![Kontrol numarası tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Zaman aralığına bağlı olarak, artımlı çalışma zamanı durumu birincil bir bölgeden ikincil bir bölgeye çoğalır. Birincil bölge kullanılamadığı bir felaket olayı sırasında, iş sürekliliği için trafiği ikincil bölgeye yönlendirin. 

## <a name="edifact"></a>EDIFACT 

EDI EDIFACT belgelerinin iş sürekliliği kontrol numaralarına dayanmaktadır.

**Ön koşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek için EDIFACT anlaşmanızın Alma Ayarları'ndaki yinelenen denetim ayarlarını seçin.

![Yinelenen denetim ayarlarını seçin](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. İkincil bir bölgede [bir mantık uygulaması](../logic-apps/quickstart-create-first-logic-app-workflow.md) oluşturun.    

2. **EDIFACT'de**arama yapın ve **EDIFACT 'i**seçin - Bir kontrol numarası değiştirildiğinde .

   ![EDIFACT ara](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   Tetikleyici, bir tümleştirme hesabına bağlantı kurmanızı ister. 
   Tetikleyici birincil bölge tümleştirme hesabına bağlanmalıdır. 

3. Bir bağlantı adı girin, listeden *birincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin.    

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. Denetim numarası eşitleme ayarını **başlatmak için DateTime** isteğe bağlıdır. **Sıklık,** bir aralıkla **Gün,** **Saat,** **Dakika**veya **Saniye** olarak ayarlanabilir.    

   ![DateTime ve Sıklık](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. **Yeni adım** > seçin**Eylem ekle.**    

   ![Yeni adım, eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. **EDIFACT'de**arama yapın ve **EDIFACT - Denetim numaraları ekleyin veya güncelleyin'i**seçin.   

   ![Denetim numaraları ekleme veya güncelleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Bir eylemi ikincil bölge tümleştirme hesabına bağlamak için, kullanılabilir tümleştirme hesaplarının listesi > **için** **bağlantıyı Değiştir'i**seçin. Bir bağlantı adı girin, listeden *ikincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin.

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Sağ üst köşedeki simgeye tıklayarak ham girişlere geçin.

   ![Ham girişlere geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Dinamik içerik seçiciden Gövde'yi seçin ve mantık uygulamasını kaydedin.   

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Zaman aralığına bağlı olarak, tetikleyici birincil bölge denetim numarası tablosunu aldı ve yeni kayıtları çeker.
   Eylem, kayıtları ikincil bölge tümleştirme hesabına güncelleştirir. 
   Güncelleştirme yoksa, tetikleyici durumu **Atlanan**olarak görünür.

   ![Kontrol numarası tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Zaman aralığına bağlı olarak, artımlı çalışma zamanı durumu birincil bir bölgeden ikincil bir bölgeye çoğalır. Birincil bölge kullanılamadığı bir felaket olayı sırasında, iş sürekliliği için trafiği ikincil bölgeye yönlendirin. 

## <a name="as2"></a>AS2 

AS2 protokolünü kullanan belgeler için iş sürekliliği ileti kimliğive MIC değerini temel alar.

> [!TIP]
> Ayrıca, mantık uygulamaları oluşturmak için [AS2 hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/pull/3302) da kullanabilirsiniz. Birincil ve ikincil tümleştirme hesapları oluşturmak, şablonu kullanmak için ön koşullardır. Şablon, tetikleyicive eylemi olan bir mantık uygulaması oluşturmaya yardımcı olur. Mantık uygulaması, tetikleyiciden birincil tümleştirme hesabına ve bir eylemden ikincil tümleştirme hesabına bir bağlantı oluşturur.

1. İkinci lüksel bölgede bir [mantık uygulaması](../logic-apps/quickstart-create-first-logic-app-workflow.md) oluş  

2. **AS2'de**arama yapın ve **MIC değeri oluşturulduğunda AS2 - select'i**seçin.   

   ![AS2 ara](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Tetikleyici, bir tümleştirme hesabına bağlantı kurmanızı ister. 
   Tetikleyici birincil bölge tümleştirme hesabına bağlanmalıdır. 
   
3. Bir bağlantı adı girin, listeden *birincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin.

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. MIC değer eşitleme ayarını **başlatmak için DateTime** isteğe bağlıdır. **Sıklık,** bir aralıkla **Gün,** **Saat,** **Dakika**veya **Saniye** olarak ayarlanabilir.   

   ![DateTime ve Sıklık](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. **Yeni adım** > seçin**Eylem ekle.**  

   ![Yeni adım, eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. **AS2'de**arama yapın ve **AS2 - MIC içeriği ekleyin veya güncelleyin'i**seçin.  

   ![MIC ekleme veya güncelleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Bir eylemi ikincil bir tümleştirme hesabına bağlamak için, kullanılabilir tümleştirme hesaplarının listesi > **için** **bağlantıyı Değiştir'i**seçin. Bir bağlantı adı girin, listeden *ikincil bölge tümleştirme hesabınızı* seçin ve **Oluştur'u**seçin.

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Sağ üst köşedeki simgeye tıklayarak ham girişlere geçin.

   ![Ham girişlere geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Dinamik içerik seçiciden Gövde'yi seçin ve mantık uygulamasını kaydedin.   

   ![Dinamik içerik](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Zaman aralığına bağlı olarak, tetikleyici birincil bölge tablosunu yoklar ve yeni kayıtları çeker. Eylem bunları ikincil bölge tümleştirme hesabına güncelleştirir. 
   Güncelleştirme yoksa, tetikleyici durumu **Atlanan**olarak görünür.  

   ![Birincil bölge tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Zaman aralığına bağlı olarak, artımlı çalışma zamanı durumu birincil bölgeden ikincil bölgeye çoğalır. Birincil bölge kullanılamadığı bir felaket olayı sırasında, iş sürekliliği için trafiği ikincil bölgeye yönlendirin. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure İzleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)

