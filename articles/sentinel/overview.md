---
title: Azure Sentinel nedir?| Microsoft Dokümanlar
description: Azure Sentinel, temel özellikleri ve nasıl çalıştığı hakkında bilgi edinin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 10cce91a-421b-4959-acdf-7177d261f6f2
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: yelevin
ms.openlocfilehash: 7d0f246a96072a6e433b0762256467f1c20a45fe
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77581727"
---
# <a name="what-is-azure-sentinel"></a>Azure Sentinel nedir?

Microsoft Azure Sentinel ölçeklenebilir, bulut ait, **güvenlik bilgileri olay yönetimi (SIEM)** ve güvenlik düzenleme otomatik yanıt **(SOAR)** çözümüdür. Azure Sentinel, kuruluş genelinde akıllı güvenlik analitiği ve tehdit istihbaratı sunarak uyarı algılama, tehdit görünürlüğü, proaktif avlanma ve tehdit yanıtı için tek bir çözüm sunar. 

Azure Sentinel, giderek daha karmaşık hale gelen saldırıların stresini, artan uyarı hacimlerini ve uzun çözünürlüklü zaman dilimlerini hafifleten kuruluş genelinde kuş bakışı görünümünüzdür.

- Tüm kullanıcılar, aygıtlar, uygulamalar ve altyapı da, şirket içinde ve birden çok bulutta **bulut ölçeğinde veri toplayın.** 

- Microsoft'un analitik ve benzersiz tehdit istihbaratını kullanarak **daha önce algılanmayan tehditleri algılayın**ve yanlış pozitif leri en aza indirin. 

- **Yapay zeka ile tehditleri araştırın**ve Microsoft'ta siber güvenlik çalışmaları yıl dokunarak, ölçekte şüpheli faaliyetleri için avı. 

- Dahili orkestrasyon ve ortak görevlerin otomasyonu ile **olaylara hızla yanıt verin.**

![Azure Sentinel temel özellikleri](./media/overview/core-capabilities.png)

Mevcut Azure hizmetlerinin tüm yelpazesini temel alan Azure Sentinel, Log Analytics ve Logic Apps gibi kanıtlanmış temelleri yerel olarak birleştirir. Azure Sentinel, araştırmanızı ve algılamanızı Yapay Zeka ile zenginleştirir ve Microsoft'un tehdit istihbaratı akışını sağlar ve kendi tehdit zekanızı getirmenizi sağlar. 

## <a name="connect-to-all-your-data"></a>Tüm verilerinize bağlanın

Yerleşik Azure Sentinel için öncelikle [güvenlik kaynaklarınıza bağlanmanız](connect-data-sources.md)gerekir. Azure Sentinel, Microsoft çözümleri için kutudan çıkabilen ve Microsoft Tehdit Koruması çözümleri ve Office 365, Azure AD, Azure ATP gibi Microsoft 365 kaynakları ve gerçek zamanlı tümleştirme sağlayan bir dizi bağlayıcıyla birlikte gelir ve Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar da vardır. Veri kaynaklarınızı Azure Sentinel'e bağlamak için ortak etkinlik biçimini, Syslog'u veya REST-API'yi de kullanabilirsiniz.  

![Veri toplayıcıları](./media/collect-data/collect-data-page.png)

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="workbooks"></a>Çalışma Kitapları

Veri [kaynaklarınızı](quickstart-onboard.md) Azure Sentinel'e bağladıktan sonra, özel çalışma kitapları oluşturmada çok yönlülük sağlayan Azure Monitor Çalışma Kitapları ile Azure Sentinel tümleştirmesini kullanarak verileri izleyebilirsiniz. Çalışma Kitapları Azure Sentinel'de farklı görüntülense de, [Azure Monitör Çalışma Kitapları ile etkileşimli raporların](../azure-monitor/app/usage-workbooks.md)nasıl oluşturulabileceğini görmeniz yararlı olabilir. Azure Sentinel, verileriniz arasında özel çalışma kitapları oluşturmanıza olanak tanır ve bir veri kaynağını bağlar bağlamaz verilerinizde hızlı bir şekilde öngörüler elde etmenizi sağlayan yerleşik çalışma kitabı şablonlarıyla birlikte gelir.

![Panolar](./media/tutorial-monitor-data/access-workbooks.png)

## <a name="analytics"></a>Analiz

Azure Sentinel, gürültüyü azaltmanıza ve incelemeniz ve araştırmanız gereken uyarı sayısını en aza indirmek için [uyarıları olaylarla ilişkilendirmek için analitiği](tutorial-detect-threats-built-in.md)kullanır. **Olaylar,** birlikte araştırıp çözebileceğiniz eyleme geçirilebilir bir tehdit oluşturan ilgili uyarı gruplarıdır. Yerleşik korelasyon kurallarını olduğu gibi kullanın veya kendi kurallarınızı oluşturmak için başlangıç noktası olarak kullanın. Azure Sentinel ayrıca ağ davranışınızı eşlemek ve ardından kaynaklarınızda anormallikleri aramak için makine öğrenimi kuralları da sağlar. Bu analizler, farklı varlıklar la ilgili düşük doğruluk uyarılarını potansiyel yüksek sadakat güvenlik olaylarına birleştirerek noktaları birbirine bağlar.

![Olaylar](./media/tutorial-investigate-cases/incident-severity.png)


## <a name="security-automation--orchestration"></a>Güvenlik otomasyonu & orkestrasyon

Azure hizmetlerinin yanı sıra mevcut araçlarınızla tümleşen çalma kitaplarıyla ortak görevlerinizi otomatikleştirin ve [güvenlik düzenlemenizi basitleştirin.](tutorial-respond-threats-playbook.md) Azure Logic Apps'ın temeli üzerine inşa edilen Azure Sentinel otomasyon ve orkestrasyon çözümü, yeni teknolojiler ve tehditler ortaya çıktıkça ölçeklenebilir otomasyon sağlayan yüksek genişletilebilir bir mimari sağlar. Azure Logic Apps ile oyun kitapları oluşturmak için, büyüyen yerleşik oyun kitapları galerisi arasından seçim yapabilirsiniz. Bunlar arasında Azure işlevleri gibi hizmetler için [200'den fazla konektör](https://docs.microsoft.com/azure/connectors/apis-list) yer alır. Bağlayıcılar kod, ServiceNow, Jira, Zendesk, HTTP istekleri, Microsoft Teams, Slack, Windows Defender ATP ve Cloud App Security herhangi bir özel mantık uygulamak için izin verir.

Örneğin, ServiceNow bilet sistemini kullanıyorsanız, iş akışlarınızı otomatikleştirmek ve belirli bir olay algılandığı her zaman ServiceNow'da bir bilet açmak için Azure Logic Apps'ı kullanmak için sağlanan araçları kullanabilirsiniz.

![Playbook'lar](./media/tutorial-respond-threats-playbook/logic-app.png)


## <a name="investigation"></a>Araştırma

Şu anda önizlemede olan Azure Sentinel [derin araştırma](tutorial-investigate-cases.md) araçları, olası bir güvenlik tehdidinin kapsamını anlamanıza ve temel nedenini bulmanıza yardımcı olur. Etkileşimli grafikte belirli bir varlık için ilginç sorular sormak için bir varlık seçebilir ve tehdidin temel nedenine ulaşmak için bu varlığı ve bağlantılarını ayrıntılı olarak araştırabilirsiniz. 

![Araştırma](./media/tutorial-investigate-cases/map-timeline.png)


## <a name="hunting"></a>Avlanma

Bir uyarı tetiklenmeden önce, kuruluşunuzun veri kaynaklarında güvenlik tehditlerini proaktif olarak aramanızı sağlayan MITRE çerçevesini temel alan Azure Sentinel'in [güçlü arama ve sorgu araçlarını](hunting.md)kullanın. Hangi av sorgusunun olası saldırılara ilişkin yüksek değerli öngörüler sağladığını keşfettikten sonra, sorgunuza dayalı özel algılama kuralları oluşturabilir ve bu öngörüleri güvenlik olayı yanıtlayıcılarınıza uyarı olarak sunabilirsiniz. Avlanırken, ilginç olaylar için yer imleri oluşturabilir, daha sonra bunlara geri dönmenizi, başkalarıyla paylaşmanızı ve soruşturma için zorlayıcı bir olay yaratmak için bunları diğer ilişkili olaylarla gruplandırmanızı sağlayabilirsiniz.

![Avlanma](./media/overview/hunting.png)

## <a name="community"></a>Topluluk

Azure Sentinel topluluğu, tehdit algılama ve otomasyon için güçlü bir kaynaktır. Microsoft güvenlik analistlerimiz sürekli olarak yeni çalışma kitapları, oyun kitapları, av sorguları ve daha fazlasını oluşturarak, bunları çevrenizde kullanmanız için topluluğa gönderir. Azure Sentinel için özel çalışma kitapları, av sorguları, not defterleri ve oyun kitapları oluşturmak için özel topluluk GitHub [deposundan](https://aka.ms/asicommunity) örnek içerik indirebilirsiniz. 

![Topluluk](./media/overview/community.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel'e başlamak için Microsoft Azure aboneliğine ihtiyacınız var. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel'e nasıl ekeceklerinizi](quickstart-onboard.md)ve [verilerinizle ve olası tehditlerde görünürlüğü](quickstart-get-visibility.md)nasıl elde edebilirsiniz öğrenin.
