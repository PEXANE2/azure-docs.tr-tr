---
title: "Öğretici: Azure Sentinel'de bir oyun kitabı çalıştırma"
description: "Öğretici: Bu makalede, Azure Sentinel'de bir oyun kitabının nasıl çalıştırılacak olduğu açıklanmaktadır."
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: e4afc5c8-ffad-4169-8b73-98d00155fa5a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2019
ms.author: yelevin
ms.openlocfilehash: f6adcb978dbe540d3bdd352089d4dde407d0fb4c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77585093"
---
# <a name="tutorial-set-up-automated-threat-responses-in-azure-sentinel"></a>Öğretici: Azure Sentinel'de otomatik tehdit yanıtları ayarlama



Bu öğretici, Azure Sentinel tarafından algılanan güvenlikle ilgili sorunlara otomatik tehdit yanıtları ayarlamak için Azure Sentinel'deki güvenlik oyun kitaplarını kullanmanıza yardımcı olur.


> [!div class="checklist"]
> * Oyun kitaplarını anlama
> * Oyun kitabı oluşturma
> * Bir oyun kitabı çalıştırın
> * Tehdit yanıtlarını otomatikleştirin


## <a name="what-is-a-security-playbook-in-azure-sentinel"></a>Azure Sentinel'de güvenlik oyun kitabı nedir?

Güvenlik oyun kitabı, bir uyarıya yanıt olarak Azure Sentinel'den çalıştırılabilen yordamlar topluluğudur. Bir güvenlik oyun kitabı yanıtınızı otomatikleştirmenize ve düzenlemeye yardımcı olabilir ve belirli uyarılar tetiklendiğinde el ile çalıştırılabilir veya otomatik olarak çalışacak şekilde ayarlanabilir. Azure Sentinel'deki güvenlik oyun kitapları [Azure Mantık Uygulamaları'nı](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps)temel alır, bu da Logic Apps'ın tüm güç, özelleştirilebilirlik ve yerleşik şablonlarına sahip olduğunuz anlamına gelir. Her oyun kitabı seçtiğiniz belirli abonelik için oluşturulur, ancak Playbook'lar sayfasına baktığınızda, tüm oyun kitaplarını seçili aboneliklerde görürsünüz.

> [!NOTE]
> Oyun kitapları Azure Logic Apps'dan yararlanır, bu nedenle ücretler geçerlidir. Ayrıntılı bilgi için [Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) fiyatlandırma sayfasını ziyaret edin.

Örneğin, kötü niyetli saldırganların ağ kaynaklarınıza erişmesinden endişe ediyorsanız, ağınıza erişen kötü amaçlı IP adreslerini arayan bir uyarı ayarlayabilirsiniz. Ardından, aşağıdakileri yapan bir oyun kitabı oluşturabilirsiniz:
1. Uyarı tetiklendiğinde ServiceNow veya başka bir BT bilet sistemindebir bilet açın.
2. Güvenlik analistlerinizin olaydan haberdar olduğundan emin olmak için Microsoft Teams veya Slack'teki güvenlik işlemleri kanalınıza bir ileti gönderin.
3. Uyarıdaki tüm bilgileri üst düzey ağ yöneticinize ve güvenlik yöneticinize gönderin. E-posta iletisi ayrıca iki kullanıcı seçeneği düğmeleri **Blok** veya **Yoksay**içerir.
4. Oyun kitabı, yöneticilerden bir yanıt alındıktan sonra da devam eder.
5. Yöneticiler **Engelle'yi**seçerse, IP adresi güvenlik duvarında engellenir ve kullanıcı Azure AD'de devre dışı bırakılır.
6. Yöneticiler **Yoks'ı**seçerse, uyarı Azure Sentinel'de kapatılır ve olay ServiceNow'da kapatılır.

Güvenlik oyun kitapları el ile veya otomatik olarak çalıştırılabilir. Bunları el ile çalıştırmak, bir uyarı aldığınızda, seçili uyarıya yanıt olarak isteğe bağlı bir oyun kitabı çalıştırmayı seçebileceğiniz anlamına gelir. Bunları otomatik olarak çalıştırmak, korelasyon kuralını yazarken, uyarı tetiklendiğinde bir veya daha fazla oyun kitabını otomatik olarak çalıştıracak şekilde ayarladığınız anlamına gelir.


## <a name="create-a-security-playbook"></a>Güvenlik oyun kitabı oluşturma

Azure Sentinel'de yeni bir güvenlik oyun kitabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure **Sentinel** panosunu açın.
2. **Yönetim**altında, **Playbooks**seçin.

   ![Logic App](./media/tutorial-respond-threats-playbook/playbookimg.png)

3. Azure **Sentinel - Playbook** sayfasında **Ekle** düğmesini tıklatın.

   ![Mantıksal uygulama oluşturma](./media/tutorial-respond-threats-playbook/create-playbook.png) 

4. Mantık **Oluştur uygulaması** sayfasında, yeni mantık uygulamanızı oluşturmak için istenen bilgileri yazın ve **Oluştur'u**tıklatın. 

5. Mantık [**Uygulama Tasarımcısı'nda**](../logic-apps/logic-apps-overview.md)kullanmak istediğiniz şablonu seçin. Kimlik bilgilerini gerektiren bir şablon seçerseniz, bunları sağlamanız gerekir. Alternatif olarak, sıfırdan yeni bir boş oyun kitabı oluşturabilirsiniz. **Boş Mantık Uygulamasını**seçin. 

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/playbook-template.png)

6. Yeni bir yapı oluşturabileceğiniz veya şablonu oluşturabileceğiniz Mantık Uygulama Tasarımcısı'na götürülür. [Logic Apps](../logic-apps/logic-apps-create-logic-apps-from-templates.md)ile bir oyun kitabı oluşturma hakkında daha fazla bilgi için.

7. Boş bir oyun kitabı oluşturuyorsanız, **Tüm bağlayıcıları ve tetikleyicileri** alanında Arama'da Azure *Sentinel*yazın ve Azure **Sentinel uyarısıne yanıt tetiklendiğinde**seçin. <br>Oluşturulduktan sonra, yeni oyun kitabı **Playbook'lar** listesinde görünür. Görünmüyorsa, **Yenile'yi**tıklatın.

1. Hesaplar, IP adresleri ve ana bilgisayarlar gibi ilgili varlıkları **Varlıklar** listesinden almanızı sağlayan **Varlıkları Al** işlevlerini kullanın. Bu, belirli varlıklar üzerinde eylemleri çalıştırmanızı sağlar.

7. Şimdi playbook'unuz tetiklendiğinde gerçekleştirilecek işlemleri tanımlayabilirsiniz. Eylem, mantıksal durum, servis talebi koşullarını veya döngüleri değiştirebilirsiniz.

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/logic-app.png)

## <a name="how-to-run-a-security-playbook"></a>Güvenlik oyun kitabı nasıl çalıştırılabilen

İsteğe bağlı bir oyun kitabı çalıştırabilirsiniz.

İsteğe bağlı bir oyun kitabı çalıştırmak için:

1. **Olaylar** sayfasında, bir olay seçin ve **tüm ayrıntıları görüntüle'ye**tıklayın.

2. **Uyarılar** sekmesinde, oyun kitabını çalıştırmak istediğiniz uyarıyı tıklatın ve sağa doğru ilerleyin ve **Playbook'ları Görüntüle'yi** tıklatın ve abonelikteki kullanılabilir oyun kitapları listesinden **çalıştırmak** için bir oyun kitabı seçin. 



## <a name="automate-threat-responses"></a>Tehdit yanıtlarını otomatikleştirin

SIEM/SOC ekipleri düzenli olarak güvenlik uyarıları ile sular altında olabilir. Oluşturulan uyarıların hacmi o kadar büyüktür ki, kullanılabilir güvenlik yöneticileri bunalmış durumdadır. Bu, birçok uyarının soruşturulamayacağı durumlarda çoğu zaman sonuç verir ve bu da organizasyonu fark edilmeyen saldırılara karşı savunmasız bırakır. 

Çoğu olmasa da, bu uyarıların çoğu, belirli ve tanımlanmış düzeltme eylemleri tarafından ele alınabilen yinelenen desenlere uygundur. Azure Sentinel, düzeltmenizi oyun kitaplarında tanımlamanıza olanak tanır. Belirli güvenlik uyarılarına tanımlı bir yanıtı tam olarak otomatikleştirebilmeniz için oyun kitabı tanımınızın bir parçası olarak gerçek zamanlı otomasyon ayarlamak da mümkündür. Yanıt ekipleri, gerçek zamanlı otomasyon u kullanarak, yinelenen uyarı türlerine yönelik rutin yanıtları tam olarak otomatikleştirerek iş yüklerini önemli ölçüde azaltabilir ve benzersiz uyarılara daha fazla konsantre olabilirsiniz, desenleri analiz eder, tehdit avcılığı ve daha fazlası.

Yanıtları otomatikleştirmek için:

1. Yanıtı otomatikleştirmek istediğiniz uyarıyı seçin.
1. **Gerçek zamanlı otomasyon** **altında, Uyarı Kuralını Edle** sayfasında, bu uyarı kuralı eşleştiğinde çalıştırmak istediğiniz **Tetiklenmiş oyun kitabını** seçin.
1. **Kaydet'i**seçin.

   ![gerçek zamanlı otomasyon](./media/tutorial-detect-threats/rt-configuration.png)






## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Sentinel'de bir oyun kitabının nasıl çalıştırılabildiğini öğrendiniz. Azure Sentinel'i kullanarak [tehditleri proaktif olarak nasıl avlayacak](hunting.md) gerektiğini kullanmaya devam edin.


