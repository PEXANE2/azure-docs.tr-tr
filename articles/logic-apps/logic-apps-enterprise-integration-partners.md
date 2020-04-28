---
title: B2B tümleştirmeleri için ticari iş ortakları ekleyin
description: Tümleştirme hesabınızda Azure Logic Apps ile kullanmak için ticari iş ortakları oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: e58cbe85f30ea09adde45d55bb7b80c710c45495
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74792430"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Azure Logic Apps için tümleştirme hesaplarına ticari iş ortakları ekleyin

[Azure Logic Apps](../logic-apps/logic-apps-overview.md)' de, mantıksal uygulamalarınızla bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) kullanarak otomatikleştirilmiş IŞLETMEDEN işletmeye (B2B) tümleştirme iş akışları oluşturabilirsiniz. Kuruluşunuzu ve diğerlerini temsil etmek için, tümleştirme hesabınıza yapıt olarak ticaret ortakları oluşturup eklersiniz. İş ortakları, B2B işlemlerine katılan ve birbirleriyle Exchange iletileri olan varlıklardır.

Bu iş ortaklarını oluşturmadan önce, diğer bilgilerin gönderdiği iletileri belirleme ve doğrulama hakkında iş ortaklarınız ile bilgileri tartışıp paylaştığınızdan emin olun. Bu ayrıntıları kabul ettikten sonra tümleştirme hesabınızda iş ortakları oluşturmaya hazırsınız demektir.

## <a name="partner-roles-in-integration-accounts"></a>Tümleştirme hesaplarındaki iş ortağı rolleri

İş ortaklarınızla değiştirilen iletilerle ilgili ayrıntıları tanımlamak için, tümleştirme hesabınıza [anlaşma olarak anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md) oluşturup eklersiniz. Sözleşmeler, tümleştirme hesabınızda en az iki iş ortağı gerektirir. Kuruluşunuz her zaman sözleşmede bulunan *ana bilgisayar ortağıdır* . Kuruluşunuz ile ileti alışverişi yapan kuruluş, *Konuk iş ortağıdır*. Konuk iş ortağı başka bir şirket veya hatta kendi kuruluşunuzda bir departman olabilir. Bu iş ortaklarını ekledikten sonra bir anlaşma oluşturabilirsiniz.

Bir anlaşmada, ana bilgisayar ortağının perspektifinden gelen ve giden iletileri işlemeye ilişkin ayrıntıları belirtirsiniz. Gelen iletilerde, **alma ayarları** , konak ortağının anlaşmada Konuk ortağından gelen iletileri nasıl alacağını belirtir. Giden iletiler için **gönderme ayarları** , ana bilgisayar ortağının Konuk iş ortağına ileti gönderme şeklini belirtir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* İş ortaklarınızı, sözleşmeleri ve diğer B2B yapıtlarını depolamak için bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) . Bu tümleştirme hesabının Azure aboneliğinizle ilişkilendirilmesi gerekir.

## <a name="create-partner"></a>İş ortağı oluştur

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Ana Azure menüsünde **tüm hizmetler**' i seçin. Arama kutusuna "tümleştirme" yazın ve **tümleştirme hesapları**' nı seçin.

   !["Tümleştirme hesapları" nı seçin](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. **Tümleştirme hesapları**altında, iş ortaklarınızı eklemek istediğiniz tümleştirme hesabını seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. **Iş ortakları** kutucuğunu seçin.

   !["Iş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. **Iş ortakları**altında **Ekle**' yi seçin. **Iş ortağı Ekle**altında aşağıdaki tabloda açıklandığı gibi iş ortağının ayrıntılarını sağlayın.

   !["Ekle" yi seçin ve iş ortağı ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Yes | Ortağın adı |
   | **Leyicisini** | Yes | Kuruluşlara benzersiz iş kimlikleri sağlayan kimlik doğrulama gövdesi (örneğin, **D-U-N-S (Dun & Bradstreet)**. <p>İş ortakları, karşılıklı tanımlanmış bir iş kimliğini kabul edebilir. Bu senaryolar için, x12 için **birlikte kullanılamayan** , ediolgu Için ve **karşılıklı tanımlanmış (x12)** seçeneğini belirleyin. <p>RosettaNet için, yalnızca standart olan **Dçalıştırır**' ı seçin. |
   | **Deeri** | Yes | Mantıksal uygulamalarınızın alacağı belgeleri tanımlayan bir değer. <p>RosettaNet için bu değer, dı numarasına karşılık gelen dokuz basamaklı bir sayı olmalıdır. |
   ||||

   > [!NOTE]
   > RosettaNet kullanan iş ortakları için, önce bu ortakları oluşturup daha sonra [düzenleyerek](#edit-partner)ek bilgileri belirtebilirsiniz.

1. İşiniz bittiğinde **Tamam**’ı seçin.

   Yeni iş ortağınız artık **Iş ortakları** listesinde görünür. Ayrıca, **Iş ortakları** kutucuğunun geçerli iş ortakları sayısını güncelleştirir.

   ![Yeni iş ortağı](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>İş ortağını Düzenle

1. [Azure Portal](https://portal.azure.com)tümleştirme hesabınızı bulun ve seçin.
**Iş ortakları** kutucuğunu seçin.

   !["Iş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/edit.png)

1. **Iş ortakları**' nın altında, düzenlemek istediğiniz iş ortağını seçin ve **Düzenle**' yi seçin. **Düzenle**' nin altında, değişikliklerinizi yapın.

   ![Değişikliklerinizi yapın ve kaydedin](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   RosettaNet için, **RosettaNet Iş ortağı özellikleri**altında bu ek bilgileri belirtebilirsiniz:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İş ortağı sınıflandırması** | Hayır | Ortağın kuruluş türü |
   | **Tedarik zinciri kodu** | Hayır | Ortağın tedarik zinciri kodu, örneğin "bilgi teknolojisi" veya "elektronik bileşenler" |
   | **İlgili Kişinin Adı** | Hayır | Ortağın ilgili kişi adı |
   | **E-posta** | Hayır | Ortağın e-posta adresi |
   | **Faks** | Hayır | Ortağın Faks numarası |
   | **Telefon** | Hayır | Ortağın telefon numarası |
   ||||

1. İşiniz bittiğinde, değişikliklerinizi kaydetmek için **Tamam** ' ı seçin.

## <a name="delete-partner"></a>İş ortağını Sil

1. [Azure Portal](https://portal.azure.com)tümleştirme hesabınızı bulun ve seçin. **Iş ortakları** kutucuğunu seçin.

   !["Iş ortakları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. **Iş ortakları**' nın altında, silmek istediğiniz iş ortağını seçin. **Sil**’i seçin.

   ![İş ortağını Sil](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Sözleşmeler](../logic-apps/logic-apps-enterprise-integration-agreements.md) hakkında daha fazla bilgi edinin