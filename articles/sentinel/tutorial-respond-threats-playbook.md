---
title: Azure Sentinel önizlemesinde bir PlayBook çalıştırın | Microsoft Docs
description: Bu makalede, Azure Sentinel 'de bir PlayBook 'un nasıl çalıştırılacağı açıklanır.
services: sentinel
documentationcenter: na
author: rkarlin
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
ms.date: 7/25/2019
ms.author: rkarlin
ms.openlocfilehash: cd3288e308308729cf9b97e3c699d489857840a8
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780368"
---
# <a name="tutorial-set-up-automated-threat-responses-in-azure-sentinel-preview"></a>Öğretici: Azure Sentinel önizlemesinde otomatik tehdit yanıtlarını ayarlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğretici, Azure Sentinel tarafından algılanan güvenlikle ilgili sorunlara otomatik tehdit yanıtları ayarlamak için Azure Sentinel 'de güvenlik PlayBook 'ları kullanmanıza yardımcı olur.


> [!div class="checklist"]
> * PlayBook 'ları anlama
> * PlayBook oluşturma
> * PlayBook çalıştırma
> * Tehdit yanıtlarını otomatikleştirin


## <a name="what-is-a-security-playbook-in-azure-sentinel"></a>Azure Sentinel 'de güvenlik PlayBook nedir?

Bir güvenlik PlayBook, bir uyarıya yanıt olarak Azure Sentinel 'den çalıştırılabilecek bir yordamlar koleksiyonudur. Bir güvenlik PlayBook, yanıtınızı otomatikleştirmeye ve düzenlemeye yardımcı olabilir ve el ile çalıştırılabilir veya belirli uyarılar tetiklendiğinde otomatik olarak çalışacak şekilde ayarlanabilir. Azure Sentinel 'deki güvenlik PlayBook 'ları [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps)tabanlıdır, bu da tüm güç, özelleştirme ve yerleşik şablonları Logic Apps alır. Her bir PlayBook, seçtiğiniz belirli bir abonelik için oluşturulur, ancak PlayBook 'ları sayfasına baktığınızda, tüm PlayBook 'ları seçili abonelikler arasında görürsünüz.

> [!NOTE]
> PlayBook 'lar Azure Logic Apps yararlanır, bu nedenle ücretler geçerlidir. Ayrıntılı bilgi için [Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) fiyatlandırma sayfasını ziyaret edin.

Örneğin, ağ kaynaklarınıza erişen kötü amaçlı saldırganlar hakkında endişeli varsa, ağınıza erişen kötü amaçlı IP adreslerini aramak için bir uyarı ayarlayabilirsiniz. Ardından, aşağıdakileri yapan bir PlayBook oluşturabilirsiniz:
1. Uyarı tetiklendiğinde ServiceNow veya diğer bir It anahtar sistemi sisteminde bir bilet açın.
2. Güvenlik analistlerinizin olaydan haberdar olduğundan emin olmak için Microsoft ekiplerinde veya bolluk 'de güvenlik işlemleri kanalınıza bir ileti gönderin.
3. Uyarı içindeki tüm bilgileri, üst düzey ağ yöneticinize ve güvenlik yöneticinize gönderin. E-posta iletisi ayrıca iki Kullanıcı seçeneği düğmesini **engeller** veya **yoksayar**.
4. Yöneticilerden bir yanıt alındıktan sonra PlayBook çalışmaya devam eder.
5. Yöneticiler **Engelle**seçeneğini BELIRLERSENIZ, IP adresi güvenlik duvarında engellenir ve Kullanıcı Azure AD 'de devre dışı bırakılır.
6. Yöneticiler **yoksayma**seçeneğini belirlerseniz, uyarı Azure Sentinel 'de kapatılır ve olay ServiceNow 'da kapatılır.

Güvenlik PlayBook 'ları el ile ya da otomatik olarak çalıştırılabilir. Bunları el ile çalıştırmak, bir uyarı aldığınızda, seçili uyarıya yanıt olarak isteğe bağlı bir PlayBook çalıştırmayı tercih edebilirsiniz. Otomatik olarak çalıştırmak, bağıntı kuralını yazarken, uyarı tetiklendiğinde otomatik olarak bir veya daha fazla PlayBook çalıştıracak şekilde ayarlamanız anlamına gelir.


## <a name="create-a-security-playbook"></a>Güvenlik PlayBook oluşturma

Azure Sentinel 'de yeni bir güvenlik PlayBook oluşturmak için aşağıdaki adımları izleyin:

1. **Azure Sentinel** panosunu açın.
2. **Yönetim**altında **playbooks**' ı seçin.

   ![Mantıksal Uygulama](./media/tutorial-respond-threats-playbook/playbookimg.png)

3. **Azure Sentinel-playbooks (Önizleme)** sayfasında **Ekle** düğmesine tıklayın.

   ![Mantıksal uygulama oluşturma](./media/tutorial-respond-threats-playbook/create-playbook.png) 

4. **Mantıksal uygulama oluştur** sayfasında, yeni mantıksal uygulamanızı oluşturmak için istenen bilgileri yazın ve **Oluştur**' a tıklayın. 

5. [**Mantıksal uygulama Tasarımcısı**](../logic-apps/logic-apps-overview.md)' nda, kullanmak istediğiniz şablonu seçin. Kimlik bilgilerini gerektiren bir şablon seçerseniz, bunları sağlamanız gerekir. Alternatif olarak, sıfırdan yeni bir boş PlayBook oluşturabilirsiniz. **Boş mantıksal uygulama**' yı seçin. 

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/playbook-template.png)

6. Yeni oluşturabileceğiniz veya şablonu düzenleyebileceğiniz Logic App Designer 'a yönlendirilirsiniz. [Logic Apps](../logic-apps/logic-apps-create-logic-apps-from-templates.md)ile bir PlayBook oluşturma hakkında daha fazla bilgi için.

7. Boş bir PlayBook oluşturuyorsanız, **tüm bağlayıcılar ve Tetikleyiciler ara** alanında *Azure Sentinel*' i yazın ve bir **Azure Sentinel uyarısı yanıtı tetiklendiğinde**öğesini seçin. <br>Oluşturulduktan sonra, **playbooks** listesinde yeni PlayBook görüntülenir. Görünmüyorsa **Yenile**' ye tıklayın. 

7. Şimdi playbook'unuz tetiklendiğinde gerçekleştirilecek işlemleri tanımlayabilirsiniz. Bir eylem, mantıksal koşul, anahtar durum koşulları veya döngüler ekleyebilirsiniz.

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/logic-app.png)

## <a name="how-to-run-a-security-playbook"></a>Güvenlik PlayBook 'u çalıştırma

İsteğe bağlı olarak bir PlayBook çalıştırabilirsiniz.

Bir PlayBook 'u isteğe bağlı olarak çalıştırmak için:

1. **Olaylar** sayfasında bir olay seçin ve **tüm ayrıntıları görüntüle**' ye tıklayın.

2. **Uyarılar** sekmesinde, PlayBook 'u çalıştırmak istediğiniz uyarıya tıklayın ve sağa doğru bir şekilde ilerleyin ve PlayBook 'ları **görüntüle** ' ye tıklayın ve abonelik üzerindeki kullanılabilir PlayBook 'ları listesinden **çalıştırmak** için bir PlayBook seçin. 



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






## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel 'de bir PlayBook 'u nasıl çalıştıracağınızı öğrendiniz. Azure Sentinel 'i kullanarak [tehditler için nasıl](hunting.md) proaktif olarak bir çözüm araya geçin.


