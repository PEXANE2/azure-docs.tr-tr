---
title: Azure Sentinel hızlı başlangıç-Azure Sentinel önizlemesi ile çalışmaya başlama | Microsoft Docs
description: Azure Sentinel hızlı başlangıç-Azure Sentinel ile çalışmaya başlama
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5a4ae93c-d648-41fb-8fb8-96a025d2f73e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: c6434b4f5808c349d5e0ab04dafc5233f6ec63ac
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780479"
---
# <a name="quickstart-get-started-with-azure-sentinel-preview"></a>Hızlı Başlangıç: Azure Sentinel önizleme ile çalışmaya başlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Bu hızlı başlangıçta, Azure Sentinel kullanarak ortamınızda neler olduğunu hızla nasıl görüntüleyebileceğinizi ve izleyebileceğinizi öğreneceksiniz. Veri kaynaklarınızı Azure Sentinel 'e bağladıktan sonra, tüm bağlı veri kaynaklarınızda neler olduğunu bilmeniz için verilerin anında görselleştirilmesini ve analizini edinirsiniz. Azure Sentinel size, Azure 'da zaten sunulan araçların tam gücünden ve günlüklerinizi ve sorgularınıza yönelik analiz sağlamak üzere yerleşik olarak bulunan tabloları ve grafikleri sunan panolar sağlar. Yerleşik panoları kullanabilir ya da yeni bir panoyu sıfırdan kolayca veya var olan bir panoya göre kolayca oluşturabilirsiniz. 

## <a name="get-visualization"></a>Görselleştirmeyi al

Ortamınızda neler olduğunu görselleştirmek ve analiz etmek için öncelikle, kuruluşunuzun güvenlik duruşunu görmek üzere genel bakış panosuna göz atın. Bu kutucukların her öğesine tıklayarak oluşturuldukları ham verilerin detayına gidebilirsiniz. Azure Sentinel, paraziti azaltmanıza ve inceleyecek ve araştırmanız gereken uyarı sayısını en aza indirmenize yardımcı olmak için, uyarıları olaylar ile ilişkilendirmek için bir Fusion tekniği kullanır. **Olaylar** , birlikte araştırıp çözebileceğiniz bir işlem yapılabilir olay oluşturan ilgili uyarı gruplarıdır.

- Azure portal Azure Sentinel ' i seçin ve ardından izlemek istediğiniz çalışma alanını seçin.

  ![Azure Sentinel genel bakış](./media/qs-get-visibility/overview.png)

- Üstteki araç çubuğu, belirlenen zaman dilimine göre kaç olay olduğunu size bildirir ve bunu önceki 24 saat ile karşılaştırır. Araç çubuğu, bu olayları, tetiklenen uyarıları (küçük sayı, son 24 saat içindeki değişikliği temsil eder) ve ardından bu olayları, ne kadar açık, devam ediyor ve kapalı olduğunu bildirir. Olay sayısında önemli bir artış veya bırakma olmadığını kontrol edin. Bir bırakma işlemi varsa, bağlantı Azure Sentinel 'e raporlamayı durdurdu olabilir. Bir artış varsa şüpheli bir sorun oluşmuş olabilir. Yeni uyarılar olup olmadığını kontrol edin.

   ![Azure Sentinel huni](./media/qs-get-visibility/funnel.png)

Genel Bakış sayfasının ana gövdesi, çalışma alanınızın güvenlik durumunu gösteren bir bakışta fikir verir:

- **Zaman Içindeki olaylar ve uyarılar**: Olayların sayısını ve bu olaylardan kaç uyarı oluşturulduğunu listeler. Olağan dışı bir ani artış görürseniz, bunun için uyarıları görmeniz gerekir. olaylarda ani bir artış olduğunda ancak uyarı görmezseniz, sorun oluşması gerekebilir.

- **Olası kötü amaçlı olaylar**: Kötü amaçlı oldukları bilinen kaynaklardan trafik algılandığında, Azure Sentinel, haritada sizi uyarır. Turuncu görürseniz, bu gelen trafiktir: birisi kuruluşunuza bilinen bir kötü amaçlı IP adresinden erişmeyi deniyor. Giden (kırmızı) etkinlik görürseniz, ağınızdaki verilerin kuruluşunuzun dışında, bilinen bir kötü amaçlı IP adresine akışını sağlarsınız.

   ![Azure Sentinel eşlemesi](./media/qs-get-visibility/map.png)


- **Son olaylar**: Son olaylarınızı, bu olayların önem derecesini ve olayla ilişkili uyarı sayısını görüntülemek için. Belirli bir uyarı türünde ani Tepe olarak görüyorsanız, çalışmakta olan etkin bir saldırı olduğu anlamına gelebilir. Örneğin, Azure ATP 'den en fazla 20 geçiş karma olay uygulamanız varsa, birisi şu anda sizi saldırılara çalışıyor olabilir.

- **Veri kaynağı bozuklukları**: Microsoft 'un veri analistleri, veri kaynaklarınızdan verileri sürekli olarak arayanlara yönelik modeller oluşturdu. Herhangi bir sorun yoksa, hiçbir şey görüntülenmez. Bozukluklar algılanırsa, ne olduğunu görmek için bunlara ayrıntılı bir bakış uygulamanız gerekir. Örneğin, Azure 'da ani artış ' a tıklayın. Ne zaman meydana geldiğini görmek için **grafiğe** tıklayabilir ve sonra ne olduğunu görmek için o zaman diliminde gerçekleşen etkinliklere filtre uygulayabilirsiniz.

   ![Azure Sentinel eşlemesi](./media/qs-get-visibility/anomolies.png)

## Yerleşik panoları kullanma<a name="dashboards"></a>

Yerleşik panolar, bu hizmetlerde oluşturulan olaylara derinlemesine bakış sağlamak için bağlı veri kaynaklarınızdan tümleşik veriler sağlar. Yerleşik panolar, Azure KIMLIĞI, Azure etkinlik olayları ve şirket içi Windows olaylarından, ilk taraf uyarılardan, Windows 'a dayalı güvenlik duvarı trafik günlükleri, Office 365 ve güvenli olmayan protokoller dahil olmak üzere herhangi bir üçüncü tarafın verileri olabilir. olayları.

1. **Ayarlar**altında **panolar**' ı seçin. **Yüklü**altında, yüklü Panolarınızın tümünü görebilirsiniz. **Tüm** ' ın altında, yükleme için kullanılabilen yerleşik panoların tamamını görebilirsiniz. 
2. Tüm liste ve her bir tekliflerin açıklaması hakkında bilgi almak için belirli bir panoyu arayın. 
3. Azure AD 'yi kullandığınız varsayılarak, Azure Sentinel ile çalışmaya başlayın ve en azından aşağıdaki panoları yüklemenizi öneririz:
   - **Azure AD**: Aşağıdakilerden birini veya her ikisini birden kullanın:
       - **Azure AD oturum** açma işlemleri zaman içinde oturum açma işlemlerini çözümleyerek, bozukluklar olup olmadığını görebilir. Bu Pano, uygulamalar, cihazlar ve konumlar tarafından başarısız oturum açma işlemleri sunarak, olağan dışı bir durum oluşursa bir bakışta bildirimde bulunabilmeniz için. Birden çok başarısız oturum açma ile ilgilenmeyi ödeyin. 
       - **Azure AD denetim günlükleri** , kullanıcılar üzerindeki değişiklikler (ekleme, kaldırma, vs.), Grup oluşturma ve değişiklikler gibi yönetici etkinliklerini analiz eder.  

   - Güvenlik duvarınız için bir Pano ekleyin. Örneğin, Palo Alto panosunu ekleyin. Pano güvenlik duvarı trafiğinizi çözümleyerek güvenlik duvarınızın verileri ile tehdit olayları arasında bağıntılar sağlar ve varlıklar arasında şüpheli olayları vurgular. Panolar, trafiğinizin eğilimleri hakkında bilgi sağlar ve sonuçların ayrıntılarına inmenizi ve sonuçları filtrelemenizi sağlar. 

      ![PAL Alto panosu](./media/qs-get-visibility/palo-alto-week-query.png)


Ana sorgu ![düğmesini](./media/qs-get-visibility/edit-query-button.png)düzenleyerek panoları özelleştirebilirsiniz. [Sorguyu düzenlemek için Log Analytics](../azure-monitor/log-query/get-started-portal.md)gitmek ![üzere](./media/qs-get-visibility/go-to-la-button.png) düğme düğmesine tıklayabilir ve üç nokta (...) simgesini seçip, ana saat filtresini düzenlemenizi sağlayan **kutucuk verilerini Özelleştir**' i seçebilirsiniz. Panodan belirli kutucuklar.

Sorgularla çalışma hakkında daha fazla bilgi için bkz [. Öğretici: Log Analytics 'daki görsel veriler](../azure-monitor/learn/tutorial-logs-dashboards.md)

### <a name="add-a-new-tile"></a>Yeni kutucuk Ekle

Yeni bir kutucuk eklemek istiyorsanız, bunu oluşturduğunuz veya Azure Sentinel yerleşik panosu olan var olan bir panoya ekleyebilirsiniz. 
1. Log Analytics, [öğreticide bulunan yönergeleri kullanarak bir kutucuk oluşturun: Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md)görsel verileri. 
2. Kutucuk oluşturulduktan sonra, **PIN**altında kutucuğun görünmesini istediğiniz panoyu seçin.

## <a name="create-new-dashboards"></a>Yeni panolar oluşturma
Sıfırdan yeni bir pano oluşturabilir veya yeni panonuz için temel olarak yerleşik bir pano kullanabilirsiniz.

1. Sıfırdan yeni bir pano oluşturmak için **panolar** ' ı ve sonra **+ Yeni Pano**' yı seçin.
2. Panonun oluşturulduğu aboneliği seçin ve açıklayıcı bir ad verin. Her Pano birbirleriyle benzer bir Azure kaynağıdır ve kimlerin erişebileceğini tanımlamak ve sınırlamak için BT rolleri (RBAC) atayabilirsiniz. 
3. Görselleştirmeleri ' a sabitlemek üzere panolarınızda görünmesini sağlamak için bunu paylaşmanız gerekir. **Paylaşma** ' ya ve ardından **Kullanıcıları Yönet**' e tıklayın. 
 
1. Diğer tüm Azure kaynakları için yaptığınız gibi erişim ve **rol atamalarını** **Denetle** öğesini kullanın. Daha fazla bilgi için bkz. [RBAC kullanarak Azure panoları paylaşma](../azure-portal/azure-portal-dashboard-share-access.md).


## <a name="new-dashboard-examples"></a>Yeni Pano örnekleri

Aşağıdaki örnek sorgu, trafik eğilimlerini haftalar içinde karşılaştırmanızı sağlar. Sorguyu çalıştırdığınız cihaz satıcısını ve veri kaynağını kolayca geçirebilirsiniz. Bu örnek, Windows 'da SecurityEvent kullanır, diğer tüm güvenlik duvarında AzureActivity veya CommonSecurityLog üzerinde çalışacak şekilde geçiş yapabilirsiniz.

     |where DeviceVendor = = "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Katları kaynaklardan veri içeren bir sorgu oluşturmak isteyebilirsiniz. Yeni oluşturulan yeni kullanıcılar için Azure Active Directory Denetim günlüklerine bakar ve sonra kullanıcının, 24 saat içinde rol ataması değişikliği yapıp başlatmadığını görmek için Azure günlüklerinizi denetleyerek bir sorgu oluşturabilirsiniz. Bu şüpheli etkinlik bu panoda görünür:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

Verilere ve ne aradıklarını arayan kişilerin rolüne göre farklı panolar oluşturabilirsiniz. Örneğin, ağ yöneticiniz için güvenlik duvarı verilerini içeren bir pano oluşturabilirsiniz. Ayrıca, günlük olarak gözden geçirmek istediğiniz bir şey olup olmadığı ve bir saatte bir kez kontrol etmek istediğiniz diğer öğelerin, örneğin Azure AD oturum açma bilgilerinizi aramak için, anomali araması yapmak istediğiniz zaman es. 

## <a name="create-new-detections"></a>Yeni algılamalar oluştur

Kuruluşunuzdaki tehditleri araştırmak için [Azure Sentinel 'e bağladığınız veri kaynaklarında](connect-data-sources.md) algılamalar oluşturun.

Yeni bir algılama oluşturduğunuzda, Microsoft güvenlik araştırmacıları tarafından, bağlandığınız veri kaynaklarına uyarlanmış yerleşik algılamalardan yararlanın.

1. [GitHub topluluğunda](https://github.com/Azure/Azure-Sentinel/tree/master/Detections) , **algılama** klasörüne gidin ve ilgili klasörleri seçin.
   ![ilgili klasörler](./media/qs-get-visibility/detection-folders.png)
 
3.  **Analiz** sekmesine gidin ve **Ekle**' yi seçin.
   ![Log Analytics kural oluşturma](./media/qs-get-visibility/query-params.png)

3.  Tüm parametreleri kurala kopyalayın ve **Oluştur**' a tıklayın.
   ![Uyarı kuralı oluştur](./media/qs-get-visibility/create-alert-rule.png)

 
## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Sentinel 'i kullanmaya nasıl başladığınıza öğrendiniz. [Tehditleri algılayarak](tutorial-detect-threats.md)ilgili öğreticiye devam edin.
> [!div class="nextstepaction"]
> Tehditleri yanıtlarınızı otomatik hale getirmek için [tehditleri tespit](tutorial-detect-threats.md) edin.

