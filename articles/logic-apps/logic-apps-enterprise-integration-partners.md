---
title: B2B entegrasyonları için ticaret ortakları ekleyin
description: Azure Logic Apps ile kullanmak üzere entegrasyon hesabınızda ticaret ortakları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: e58cbe85f30ea09adde45d55bb7b80c710c45495
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792430"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Azure Logic Apps için tümleştirme hesaplarına ticaret ortakları ekleme

[Azure Logic Apps'ta,](../logic-apps/logic-apps-overview.md)mantık uygulamalarınızla bir entegrasyon [hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) kullanarak otomatik işletmeden işletmeye (B2B) tümleştirme iş akışları oluşturabilirsiniz. Kuruluşunuzu ve diğerlerini temsil etmek için, tümleştirme hesabınıza ticari ortaklar oluşturur ve yapı olarak eklersiniz. Ortaklar, B2B işlemlerine katılan ve birbirleriyle mesaj alışverişinde bulunan kuruluşlardır.

Bu iş ortaklarını oluşturmadan önce, diğerinin gönderdiği iletileri nasıl tanımlayıp doğrulayaceksiniz hakkında ortaklarınızla bilgi tartıştığınızdan ve paylaştığınızdan emin olun. Bu ayrıntıları kabul ettikten sonra, entegrasyon hesabınızda iş ortakları oluşturmaya hazırsınız.

## <a name="partner-roles-in-integration-accounts"></a>Tümleştirme hesaplarındaki iş ortağı rolleri

Ortaklarınızla değiş tokuş edilen iletilerle ilgili ayrıntıları tanımlamak için, tümleştirme hesabınıza yapı olarak [anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md) oluşturur ve eklersiniz. Anlaşmalar, entegrasyon hesabınızda en az iki ortak gerektirir. Kuruluşunuz her zaman anlaşmanın *ev sahibi ortağıdır.* Kuruluşunuzla ileti alışverişinde bulunduran kuruluş *konuk iş ortağıdır.* Konuk ortak başka bir şirket, hatta kendi kuruluşunuzdaki bir departman olabilir. Bu ortakları ekledikten sonra bir anlaşma oluşturabilirsiniz.

Bir anlaşmada, gelen ve giden iletileri ana bilgisayar ortağının bakış açısından işleme ayrıntılarını belirtirsiniz. Gelen iletiler **için, Alma Ayarları,** ev sahibi ortağın sözleşmedeki konuk ortaktan nasıl ileti aldığını belirtir. Giden iletiler için, Ayar **Gönder,** ev sahibi ortağın konuk iş ortağına nasıl ileti gönderdiğini belirtir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Ortaklarınızı, anlaşmalarınızı ve diğer B2B yapıtlarını depolamak için bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Bu tümleştirme hesabı, Azure aboneliğinizle ilişkilendirilmelidir.

## <a name="create-partner"></a>İş ortağı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Ana Azure menüsünde **Tüm hizmetler'i**seçin. Arama kutusuna "tümleştirme" girin ve **Tümleştirme hesaplarını**seçin.

   !["Tümleştirme hesapları" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. **Tümleştirme**Hesapları'nda, iş ortaklarınızı eklemek istediğiniz tümleştirme hesabını seçin.

   ![Entegrasyon hesabını seçin](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. **Ortaklar** döşemesini seçin.

   !["Ortaklar" döşemesini seçin](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. **Ortaklar**altında **Ekle'yi**seçin. **İş Ortağı Ekle**altında, aşağıdaki tabloda açıklandığı gibi ortağın ayrıntılarını sağlayın.

   !["Ekle"yi seçin ve iş ortağı ayrıntıları sağlayın](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Evet | Ortağın adı |
   | **Niteleyici** | Evet | **D-U-N-S (Dun & Bradstreet)** gibi kuruluşlara benzersiz iş kimlikleri sağlayan kimlik doğrulama gövdesi. <p>Ortaklar, karşılıklı olarak tanımlanmış bir iş kimliğini seçebilirler. Bu senaryolar için, EDIFACT için **Karşılıklı Tanımlı** veya X12 için Karşılıklı **Tanımlı (X12)** seçeneğini belirleyin. <p>RosettaNet için standart olan yalnızca **DUNS'ı**seçin. |
   | **Değer** | Evet | Mantık uygulamalarınızın aldığı belgeleri tanımlayan bir değer. <p>RosettaNet için bu değer DUNS numarasına karşılık gelen dokuz basamaklı bir sayı olmalıdır. |
   ||||

   > [!NOTE]
   > RosettaNet kullanan iş ortakları için, önce bu ortakları oluşturarak ve [daha sonra bunları düzenleyerek](#edit-partner)ek bilgiler belirtebilirsiniz.

1. İşiniz bittiğinde **Tamam**’ı seçin.

   Yeni ortağınız artık **Ortaklar** listesinde görünüyor. Ayrıca, **Ortaklar** döşemesi mevcut iş ortağı sayısını güncelleştirir.

   ![Yeni ortak](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>İş ortağını edin

1. Azure [portalında](https://portal.azure.com)entegrasyon hesabınızı bulun ve seçin.
**Ortaklar** döşemesini seçin.

   !["Ortaklar" döşemesini seçin](./media/logic-apps-enterprise-integration-partners/edit.png)

1. **İş Ortakları**altında, yapmak istediğiniz iş ortağını seçin ve **Edit'i**seçin. **Edit**altında, değişikliklerinizi yapın.

   ![Değişikliklerinizi yapın ve kaydedin](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   RosettaNet için, **RosettaNet İş Ortağı Özellikleri**altında, bu ek bilgileri belirtebilirsiniz:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Ortak Sınıflandırması** | Hayır | Ortağın kuruluş türü |
   | **Tedarik zinciri kodu** | Hayır | Ortağın tedarik zinciri kodu, örneğin, "Bilgi Teknolojisi" veya "Elektronik Bileşenler" |
   | **İlgili Kişinin Adı** | Hayır | Ortağın kişi adı |
   | **E-posta** | Hayır | Ortağın e-posta adresi |
   | **Faks** | Hayır | Ortağın faks numarası |
   | **Telefon** | Hayır | Ortağın telefon numarası |
   ||||

1. Işiniz bittiğinde, değişikliklerinizi kaydetmek için **Tamam'ı** seçin.

## <a name="delete-partner"></a>İş ortağını silme

1. Azure [portalında](https://portal.azure.com)entegrasyon hesabınızı bulun ve seçin. **Ortaklar** döşemesini seçin.

   !["Ortaklar" döşemesini seçin](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. **Ortaklar**altında, silmek istediğiniz iş ortağını seçin. **Sil**’i seçin.

   ![İş ortağını silme](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md) hakkında daha fazla bilgi edinin