---
title: 'Hızlı başlangıç: Azure Sentinel ile başlayın'
description: Azure Sentinel Quickstart - Azure Sentinel ile başlayın
services: sentinel
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: quickstart
ms.custom: mvc, fasttrack-edit
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 95dcc135593c566eb1319ed52df3df6c1ada6609
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067674"
---
# <a name="quickstart-get-started-with-azure-sentinel"></a>Hızlı başlangıç: Azure Sentinel ile başlayın




Bu hızlı başlangıçta, Azure Sentinel'i kullanarak çevrenizde neler olup bittiğini hızlı bir şekilde nasıl görüntüleyebilir ve izleyebilirsiniz. Veri kaynaklarınızı Azure Sentinel'e bağladıktan sonra, bağlı olduğunuz tüm veri kaynaklarında neler olduğunu öğrenebilmeniz için anında görselleştirme ve veri analizi alırsınız. Azure Sentinel, size Azure'da zaten mevcut olan araçların tam gücünü sağlayan çalışma kitaplarının yanı sıra günlükleriniz ve sorgularınız için size analiz sağlamak üzere oluşturulmuş tablolar ve grafikler sağlar. Yerleşik çalışma kitaplarını kullanabilir veya sıfırdan veya varolan bir çalışma kitabını temel alan yeni bir çalışma kitabı oluşturabilirsiniz. 

## <a name="get-visualization"></a>Görselleştirme alın

Ortamınızda neler olup bittiğine ilişkin görselleştirmek ve analiz etmek için öncelikle kuruluşunuzun güvenlik duruşu hakkında bir fikir edinmek için genel bakış panosuna göz atın. Oluşturuldukları ham verilere sondaj yapmak için bu döşemelerin her öğesini tıklatabilirsiniz. Azure Sentinel, gürültüyü azaltmanıza ve incelemeniz ve araştırmanız gereken uyarı sayısını en aza indirmek için uyarıları olaylarla ilişkilendirmek için bir füzyon tekniği kullanır. **olaylar,** birlikte araştırıp çözebileceğiniz eyleme geçirilebilir bir olay oluşturan ilgili uyarı gruplarıdır.

- Azure portalında Azure Sentinel'i seçin ve ardından izlemek istediğiniz çalışma alanını seçin.

  ![Azure Sentinel'e genel bakış](./media/qs-get-visibility/overview.png)

- Üstteki araç çubuğu, seçilen zaman dilimi içinde kaç etkinlik aldığınızı söyler ve önceki 24 saatle karşılaştırır. Araç çubuğu, bu olaylardan, tetiklenen uyarıları (küçük sayı son 24 saat içinde değişimi temsil eder) söyler ve daha sonra bu olaylar için kaç kişinin açık, devam eden ve kapalı olduğunu söyler. Olay sayısında dramatik bir artış veya düşüş olup olmadığını kontrol edin. Bir düşüş varsa, bir bağlantı Azure Sentinel'e raporlamayı durdurmuş olabilir. Eğer bir artış varsa, şüpheli bir şey olmuş olabilir. Yeni uyarılarınız olup olmadığını kontrol edin.

   ![Azure Sentinel hunisi](./media/qs-get-visibility/funnel.png)

Genel bakış sayfasının ana gövdesi, çalışma alanınızın güvenlik durumuna bir bakışta bir bakışta içgörü sağlar:

- **Zaman daki olaylar ve uyarılar**: Olayların sayısını ve bu olaylardan kaç uyarı oluşturulduğunu listeler. Olağandışı bir artış görürseniz, bunun için uyarılar görmelisiniz - olaylarda bir artış olduğu ancak uyarıları görmediğiniz olağandışı bir şey varsa, bu endişe kaynağı olabilir.

- **Olası kötü amaçlı olaylar**: Kötü amaçlı olduğu bilinen kaynaklardan trafik algılandığında, Azure Sentinel sizi haritada uyarır. Turuncu görürseniz, gelen trafiktir: birisi kuruluşunuza bilinen kötü amaçlı bir IP adresinden erişmeye çalışır. Giden (kırmızı) etkinliği görürseniz, bu, ağınızdaki verilerin kuruluşunuzdan bilinen kötü amaçlı bir IP adresine aktarıldığı anlamına gelir.

   ![Azure Sentinel haritası](./media/qs-get-visibility/map.png)


- **Son olaylar**: Son olayları, bunların ciddiyetini ve olayla ilişkili uyarı sayısını görüntülemek için. Belirli bir uyarı türünde ani bir tepe noktası olarak görürseniz, şu anda çalışan etkin bir saldırı olduğu anlamına gelebilir. Örneğin, Azure ATP'den 20 tane karma geçiş olayının ani bir zirvesine sahipseniz, birisi şu anda size saldırmaya çalışıyor olabilir.

- **Veri kaynağı anormallikleri**: Microsoft'un veri analistleri, veri kaynaklarınızdaki verileri sürekli olarak anomaliler için arayan modeller oluşturdu. Herhangi bir anormallik yoksa, hiçbir şey görüntülenmez. Eğer anomaliler tespit edilirse, ne olduğunu görmek için onlara derin denmelisin. Örneğin, Azure Etkinliği'ndeki ani artışa tıklayın. Ani artışne ne zaman olduğunu görmek için **Grafik'e** tıklayabilir ve ardından bu dönemde gerçekleşen etkinlikleriçin filtre uygulayarak ani artışa neyin neden olduğunu görebilirsiniz.

   ![Azure Sentinel haritası](./media/qs-get-visibility/anomolies.png)

## <a name="use-built-in-workbooks"></a>Yerleşik çalışma kitaplarını kullanma<a name="dashboards"></a>

Yerleşik çalışma kitapları, bağlı veri kaynaklarınızdan gelen tümleşik verileri sağlar ve bu hizmetlerde oluşturulan olaylara derinlemesine dalan veriler sağlar. Yerleşik çalışma kitapları, Azure REKLAM'ı, Azure etkinlik etkinliklerini ve windows'a dayalı güvenli olmayan protokoller de dahil olmak üzere herhangi bir üçüncü taraftan sunuculardan, birinci taraf uyarılarından, Windows Etkinlikleri'nden veri olabilecek yerleşik çalışma kitaplarını içerir Olay. Çalışma kitapları, size kendi çalışma kitabınızı tasarlamada gelişmiş özelleştirilebilirlik ve esneklik sağlamak için Azure Monitor Çalışma Kitaplarını temel adatır. Daha fazla bilgi için [Çalışma Kitapları'na](../azure-monitor/app/usage-workbooks.md)bakın.

1. **Ayarlar**altında **Çalışma Kitapları'nı**seçin. **Yüklü**altında, tüm yüklü çalışma kitabı görebilirsiniz. **Tümü**altında, yükleme için kullanılabilir yerleşik çalışma kitaplarının tüm galerisini görebilirsiniz. 
2. Her birinin sunduğu tüm listeyi ve açıklamayı görmek için belirli bir çalışma kitabını arayın. 
3. Azure Sentinel ile çalışmaya devam etmek için Azure AD kullandığınızı varsayarsak, en az aşağıdaki çalışma kitaplarını yüklemenizi öneririz:
   - **Azure AD**: Aşağıdakilerden birini veya her ikisini kullanın:
       - **Azure AD oturum açma bilgileri,** zaman içinde oturum açma oturum açma ları analiz edip anormallikolup olmadığını iyi karşılar. Bu çalışma kitapları, uygulamalar, aygıtlar ve konumlar tarafından başarısız oturum açmalar sağlar, böylece olağandışı bir şey olursa bir bakışta fark edebilirsiniz. Birden fazla başarısız oturum açma ya da oturum açma ile ilgili dikkat edin. 
       - **Azure AD denetim günlükleri,** kullanıcılardaki değişiklikler (ekleme, kaldırma, vb.), grup oluşturma ve değişiklikler gibi yönetici etkinliklerini analiz eder.  

   - Güvenlik duvarınız için bir çalışma kitabı ekleyin. Örneğin, Palo Alto çalışma kitabını ekleyin. Çalışma kitabı güvenlik duvarı trafiğinizi analiz ederek güvenlik duvarı verileriniz ve tehdit olayları nız arasında korelasyon sağlar ve taraflar arasındaki şüpheli olayları vurgular. Çalışma kitapları size trafiğinizdeki eğilimler hakkında bilgi sağlar ve sonuçları ayrıntılı olarak belirlemenize ve filtrelemenize izin verir. 

      ![Pal Alto pano](./media/qs-get-visibility/palo-alto-week-query.png)


Ana sorgu ![düğmesini](./media/qs-get-visibility/edit-query-button.png)düzenleyerek çalışma kitaplarını özelleştirebilirsiniz. Sorguyu orada ![yeniden](./media/qs-get-visibility/go-to-la-button.png) yapmak [için Log Analytics'e](../azure-monitor/log-query/get-started-portal.md)gitmek için düğme düğmesini tıklatabilir ve elipsleri (...) seçebilir ve ana zaman filtresini tıklatmanızı veya çalışma kitabından belirli kutucukları kaldırmanızı sağlayan **kutucuğu verilerini Özelleştir'i**seçebilirsiniz.

Sorgularla çalışma hakkında daha fazla bilgi için [Bkz. Öğretici: Log Analytics'te görsel veriler](../azure-monitor/learn/tutorial-logs-dashboards.md)

### <a name="add-a-new-tile"></a>Yeni bir döşeme ekleme

Yeni bir döşeme eklemek istiyorsanız, bu döşemeyi oluşturduğunuz çalışma kitabına veya Azure Sentinel yerleşik çalışma kitabına ekleyebilirsiniz. 
1. Log Analytics'te, [Tutorial: Visual data in Log Analytics'te](../azure-monitor/learn/tutorial-logs-dashboards.md)bulunan yönergeleri kullanarak bir döşeme oluşturun. 
2. Döşeme oluşturulduktan sonra, **Pin'in**altında, döşemenin görünmesini istediğiniz çalışma kitabını seçin.

## <a name="create-new-workbooks"></a>Yeni çalışma kitapları oluşturma
Sıfırdan yeni bir çalışma kitabı oluşturabilir veya yeni çalışma kitabınızın temeli olarak yerleşik bir çalışma kitabı kullanabilirsiniz.

1. Sıfırdan yeni bir çalışma kitabı oluşturmak için **Çalışma Kitapları'nı** ve **ardından +Yeni çalışma kitabını**seçin.
2. Çalışma kitabının oluşturulduğu aboneliği seçin ve ona açıklayıcı bir ad verin. Her çalışma kitabı da diğerleri gibi bir Azure kaynağıdır ve kimlere erişebileceğini tanımlamak ve sınırlamak için bu kitap için roller (RBAC) atayabilirsiniz. 
3. Görselleştirmeleri sabitlemek için çalışma kitaplarınızda görünebilmesi için bunu paylaşmanız gerekir. **Paylaş'ı** tıklatın ve ardından **kullanıcıları yönetin.** 
 
1. Diğer Azure kaynaklarında olduğu gibi **Denetimi erişimini** ve **Rol atamalarını** kullanın. Daha fazla bilgi için [RBAC'ı kullanarak Azure çalışma kitaplarını paylaş'a](../azure-portal/azure-portal-dashboard-share-access.md)bakın.


## <a name="new-workbook-examples"></a>Yeni çalışma kitabı örnekleri

Aşağıdaki örnek sorgu, haftalar boyunca trafik eğilimlerini karşılaştırmanızı sağlar. Sorguyu çalıştırdığınız aygıt satıcısı ve veri kaynağını kolayca değiştirebilirsiniz. Bu örnek, Windows'tan SecurityEvent kullanır, azureetkinliği veya CommonSecurityLog'da çalıştırmak için başka bir güvenlik duvarında çalıştırabilirsiniz.

     |where DeviceVendor == "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Katlar kaynaklardan gelen verileri içeren bir sorgu oluşturmak isteyebilirsiniz. Yeni oluşturulan yeni kullanıcılar için Azure Etkin Dizin denetim günlüklerine bakan bir sorgu oluşturabilir ve ardından kullanıcının oluşturuldundan sonraki 24 saat içinde rol atama değişiklikleri yapmaya başlayıp başlamadığını görmek için Azure günlüklerinizi denetleyebilirsiniz. Bu şüpheli etkinlik bu panoda görünecek:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

Verilere bakan kişinin rolüne ve ne aradığına göre farklı çalışma kitapları oluşturabilirsiniz. Örneğin, ağ yöneticiniz için güvenlik duvarı verilerini içeren bir çalışma kitabı oluşturabilirsiniz. Ayrıca, çalışma kitaplarına ne sıklıkta bakmak istediğinize, günlük olarak incelemek istediğiniz şeyler olup olmadığına ve örneğin saatte bir kontrol etmek istediğiniz diğer öğelere göre, anormallikleri aramak için her saat başı Azure REKLAM oturum açma günlerine bakmak isteyebilirsiniz . 

## <a name="create-new-detections"></a>Yeni algılamalar oluşturun

Kuruluşunuzdaki tehditleri araştırmak [için Azure Sentinel'e bağlı olduğunuz veri kaynaklarında](connect-data-sources.md) algılamalar oluşturun.

Yeni bir algılama oluşturduğunuzda, Bağlı olduğunuz veri kaynaklarına göre uyarlanmış Microsoft güvenlik araştırmacıları tarafından hazırlanmış yerleşik algılamalardan yararlanın.

Tüm kutudan çıkan algılamaları görüntülemek için **Analytics'e** gidin ve ardından **şablonları kuralın.** Bu sekme, tüm Azure Sentinel yerleşik kurallarını içerir.

   ![Azure Sentinel ile tehditleri bulmak için yerleşik algılamaları kullanma](media/tutorial-detect-built-in/view-oob-detections.png)

Kutudan çıkışlı algılamalar hakkında daha fazla bilgi için [Bkz. Öğretici: Yerleşik analizler alın.](tutorial-detect-threats-built-in.md)
 
## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Sentinel'i kullanmaya nasıl başlayacağınızı öğrendiniz. [Tehditleri algılamak için öğretici](tutorial-detect-threats-built-in.md)devam edin.
> [!div class="nextstepaction"]
> Tehditlere yanıtlarınızı otomatikleştirmek için [özel tehdit algılama kuralları oluşturun.](tutorial-detect-threats-custom.md)

