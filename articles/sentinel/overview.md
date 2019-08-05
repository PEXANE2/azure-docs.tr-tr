---
title: Azure Sentinel önizlemesi nedir? | Microsoft Docs
description: Azure Sentinel, temel özellikleri ve nasıl çalıştığı hakkında bilgi edinin.
services: sentinel
documentationcenter: na
author: rkarlin
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
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 15cf770a372c9a1386dd0293abeac01fd3cacf63
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779738"
---
# <a name="what-is-azure-sentinel-preview"></a>Azure Sentinel önizlemesi nedir?

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Microsoft Azure Sentinel, ölçeklenebilir, bulutta yerel, **güvenlik bilgileri olay yönetimi (SıEM)** ve **güvenlik Orchestration otomatik yanıtı (Soar)** çözümüdür. Azure Sentinel, bir uyarı algılama, tehdit görünürlüğü, proaktif arama ve tehdit yanıtı için tek bir çözüm sunarak kuruluş genelinde akıllı güvenlik Analizi ve tehdit bilgileri sunar. 

Azure Sentinel, daha fazla gelişmiş saldırı elde etmek, uyarı hacimlerini artırmak ve uzun çözüm zaman çerçevelerinden elde hafifletmesini, kurumsal bir modern bakış görünümünüz.

- Hem şirket içinde hem de birden çok bulutta, tüm kullanıcılar, cihazlar, uygulamalar ve altyapıda **bulut ölçeğinde veri toplayın** . 

- **Daha önce algılanmayan tehditleri algılayın**ve Microsoft 'un analizlerini ve benzersiz tehdit bilgilerini kullanarak yanlış pozitif sonuçları en aza indirin. 

- **Yapay zeka ile tehditleri araştırın**ve ölçekteki şüpheli etkinlikler Için, Microsoft 'ta bulunan siber güvenlik çalışmalarına dokunduktan fazla bilgi için arama yapın. 

- Ortak görevlerin yerleşik düzenlemesi ve otomasyonu ile **olaylara hızla yanıt verin** .


![Azure Sentinel temel özellikleri](./media/overview/core-capabilities.png)

Azure Sentinel, mevcut Azure hizmetlerinin tam aralığında oluşturma, Log Analytics ve Logic Apps gibi kanıtlanmış temelleri yerel olarak içerir. Azure Sentinel, araştırma ve algılamalarınızı AI ile zenginleştirir ve Microsoft 'un tehdit bilgileri akışını sağlar ve kendi tehdit zekaızın bilgilerini almanıza olanak sağlar. 

 
## <a name="connect-to-all-your-data"></a>Tüm verilerinize bağlanın

Azure Sentinel 'de, ilk olarak [güvenlik kaynaklarınıza bağlanmanız](connect-data-sources.md)gerekir. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcı sunar ve Microsoft tehdit koruması çözümleri dahil gerçek zamanlı tümleştirme ve Office 365, Azure AD, Azure ATP ve dahil olmak üzere Microsoft 365 kaynakları sağlar. Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca, veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi, syslog veya REST API de kullanabilirsiniz.  

![Veri toplayıcılar](./media/collect-data/collect-data-page.png)

## <a name="dashboards"></a>Panolar

Veri kaynaklarını bağladıktan sonra, veri kaynaklarınızdaki öngörüleri sunan [expertly oluşturulan panolar](quickstart-get-visibility.md#dashboards) galerisinden seçim yapabilirsiniz. Her Pano tamamen özelleştirilebilir-kendi mantığınızı ekleyebilir veya sorguları değiştirebilir veya sıfırdan Pano oluşturabilirsiniz.

Panolar, güvenlik analistlerinizin saldırı sırasında neler olduğunu daha iyi anlamak için gelişmiş analiz kullanarak etkileşimli görselleştirme sağlar. Araştırma araçları, herhangi bir alandan, tehdit bağlamını hızlı bir şekilde geliştirmeye kadar her türlü alanı derinlemesine incelemenize olanak sağlar. 

![Panolar](./media/overview/dashboards.png)

## <a name="analytics"></a>Analiz

Azure Sentinel, paraziti azaltmanıza ve inceleyecek ve araştırmanız gereken uyarı sayısını en aza indirmenize yardımcı olmak için, [uyarıları olaylar ile ilişkilendirmek için analiz](tutorial-detect-threats.md)kullanır. **Olaylar** , araştırıp çözebileceğiniz, uygulanabilir olabilecek bir tehdit oluşturan ilgili uyarı gruplarıdır. Yerleşik bağıntı kurallarını olduğu gibi kullanın veya kendi kendinize oluşturmak için bir başlangıç noktası olarak kullanın. Azure Sentinel Ayrıca, ağ davranışınızı eşlemek için makine öğrenimi kuralları sağlar ve kaynaklarınızın tamamında bozukluklar olup olmadığına bakar. Bu analizler, farklı varlıklar hakkında düşük doğruluk uyarılarını potansiyel yüksek kaliteli güvenlik olaylarına birleştirerek noktaları birbirine bağlanır.

![Çalışmaların](./media/overview/cases.png)

## <a name="user-analytics"></a>Kullanıcı analizi

Machine Learning (ML) ve [Kullanıcı analizinin](user-analytics.md)yerel tümleştirmesiyle, Azure Sentinel tehditleri hızlı bir şekilde algılamaya yardımcı olabilir. Azure Sentinel, Kullanıcı davranışını analiz etmek ve ilk olarak araştırmanız gereken kullanıcılara, uyarıları ve Azure Sentinel ve Microsoft 365 karşı şüpheli etkinlik düzenlerini belirlemek için Azure Gelişmiş tehdit koruması ile sorunsuz bir şekilde tümleşir.

![Kullanıcı analizi](./media/overview/user-analytics.png)


## <a name="security-automation--orchestration"></a>Güvenlik Otomasyonu & düzenleme

Ortak görevlerinizi otomatikleştirin ve Azure hizmetleriyle ve mevcut araçlarınızla tümleştirilen PlayBook 'lar [ile güvenlik düzenlemesini kolaylaştırın](tutorial-respond-threats-playbook.md) . Azure Logic Apps temel alınarak geliştirilen Azure Sentinel otomasyonu ve düzenleme çözümü, yeni teknolojiler ve tehditler ortaya çıktı olarak ölçeklenebilir Otomasyonu sağlayan yüksek düzeyde genişletilebilir bir mimari sağlar. Azure Logic Apps ile PlayBook 'lar oluşturmak için, büyüyen bir yerleşik PlayBook Galerisi arasından seçim yapabilirsiniz. Bunlar Azure işlevleri gibi hizmetler için [200 + bağlayıcı](https://docs.microsoft.com/azure/connectors/apis-list) içerir. Bağlayıcılar kodda, ServiceNow, Jira, Zendesk, HTTP istekleri, Microsoft ekipleri, bolluk, Windows Defender ATP ve Cloud App Security özel mantık uygulamanıza olanak tanır.

Örneğin, ServiceNow bilet sistemi kullanıyorsanız, iş akışlarınızı otomatikleştirmek için Azure Logic Apps kullanmak için sunulan araçları kullanabilir ve belirli bir olay algılandığında ServiceNow içinde bir bilet açabilirsiniz.

![Playbook'lar](./media/tutorial-respond-threats-playbook/logic-app.png)



## <a name="investigation"></a>Araştırma

Azure Sentinel [derin araştırma](tutorial-investigate-cases.md) araçları, olası bir güvenlik tehdidi kapsamında kapsamı anlamanıza ve kök nedenini bulmanıza yardımcı olur. Belirli bir varlık için ilginç sorular sormak üzere etkileşimli grafikte bir varlık seçebilir ve bu varlığa ve bu varlığın bağlantı ayrıntılarına gidebilir ve bu da tehdidin temel nedenine ulaşın. 

![Araştırma](./media/overview/investigation.png)


## <a name="hunting"></a>Avlanma

Azure Sentinel 'in, bir uyarı tetiklenene kadar, kuruluşunuzun veri kaynakları genelinde güvenlik tehditleri için güvenli bir şekilde araştırma yapma imkanı sunan, MITRE çerçevesini temel alan, [arama ve sorgu araçlarını](hunting.md)kullanın. Hangi Arayıcı sorgusunun olası saldırılara yönelik yüksek değerli Öngörüler sağladığını bulduktan sonra, sorgunuza göre özel algılama kuralları oluşturabilir ve bu öngörüleri güvenlik olay yanıtlamalarınıza uyarı olarak bırakabilirsiniz. Arama yaparken, ilginç olaylar için yer işaretleri oluşturabilir, daha sonra bunları daha sonra geri dönüp başkalarıyla paylaşabilir ve araştırmaya yönelik ilgi çekici bir durum oluşturmak için bunları başka bir ilişkili olayla gruplayabilirsiniz.

![Avlanma](./media/overview/hunting.png)

## <a name="community"></a>Topluluk

Azure Sentinel topluluğu, tehdit algılama ve otomasyonu için güçlü bir kaynaktır. Microsoft Güvenlik analistlerimiz sürekli olarak yeni panolar, PlayBook 'lar oluşturma ve ekleme sorguları ve daha fazlası oluşturup, ortamınızda kullanabilmeniz için bunları topluluğa aktarıyoruz. Özel panolar oluşturmak için özel topluluk GitHub [deposundan](https://aka.ms/asicommunity) örnek içeriği Indirebilir, Azure Sentinel için sorgular, Not defterleri ve PlayBook 'lar oluşturabilirsiniz. 

![Topluluk](./media/overview/community.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel ile çalışmaya başlamak için Microsoft Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel 'e](quickstart-onboard.md) [ekleme ve verilerinize ilişkin görünürlük alma ve olası tehditler](quickstart-get-visibility.md)hakkında bilgi edinin.
