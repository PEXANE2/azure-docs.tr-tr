---
title: Ticaret ortağı anlaşmaları
description: Azure Logic Apps ve Kurumsal Tümleştirme Paketi'ni kullanarak ticari iş ortakları arasında anlaşmalar oluşturun ve yönetin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 521a0ef4053be55e6c7322da5af26ccfc6c844e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790728"
---
# <a name="create-and-manage-trading-partner-agreements-in-azure-logic-apps"></a>Azure Logic Apps'ta ticari iş ortağı anlaşmaları oluşturma ve yönetme

Bir [ticaret ortağı](../logic-apps/logic-apps-enterprise-integration-partners.md) 
*anlaşması,* işletmelerin ve işletmelerin işletmelerarası (B2B) iletilerini değiş tokuş ederken kullanılacak belirli endüstri standardı protokolünü tanımlayarak birbirleriyle sorunsuz bir şekilde iletişim kurmasına yardımcı olur. Anlaşmalar, örneğin, ortak faydalar sağlar:

* Kuruluşların iyi bilinen bir biçim kullanarak bilgi alışverişini sağlamasını sağlar.
* B2B işlemlerini yürütürken verimliliği artırın.
* Kurumsal tümleştirme çözümleri oluşturmak, yönetmek ve kullanmak kolaydır.

Bu makalede, [Kurumsal Tümleştirme Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md) ve [Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md)kullanarak B2B senaryoları için kurumsal tümleştirme çözümleri oluştururken kullanabileceğiniz bir AS2, EDIFACT veya X12 sözleşmesinin nasıl oluşturulabileceğinizgösterilmektedir. Bir anlaşma oluşturduktan sonra, B2B iletileri alışverişi için AS2, EDIFACT veya X12 bağlayıcılarını kullanabilirsiniz.

RosettaNet iletileri alışverişi için anlaşmalar oluşturmak için Bkz. [Exchange RosettaNet iletileri.](../logic-apps/logic-apps-enterprise-integration-rosettanet.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Anlaşmanızı ve diğer B2B eserlerini depolamak için bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Bu tümleştirme hesabı, Azure aboneliğinizle ilişkilendirilmelidir.

* Entegrasyon hesabınızda oluşturduğunuz en az iki [ticaret ortağı.](../logic-apps/logic-apps-enterprise-integration-partners.md) Bir anlaşma hem ev sahibi ortak hem de konuk ortak gerektirir. Her iki ortak da, AS2, X12 veya EDIFACT gibi oluşturmak istediğiniz anlaşmayla aynı "iş kimliği" niteleyicisini kullanmalıdır.

* İsteğe bağlı: Anlaşmanızı kullanmak istediğiniz mantık uygulaması ve mantık uygulamanızın iş akışını başlatan bir tetikleyici. Tümleştirme hesabınızı ve B2B yapılarınızı oluşturmak için bir mantık uygulamasına ihtiyacınız yoktur. Ancak, mantık uygulamanız b2B yapılarını entegrasyon hesabınızda kullanabilmek için, entegrasyon hesabınızı mantık uygulamanıza bağlamanız gerekir. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-agreements"></a>Anlaşmalar oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
Ana Azure menüsünde **Tüm hizmetler'i**seçin. Arama kutusuna filtreniz olarak "tümleştirme" girin. Sonuçlardan şu kaynağı seçin: **Tümleştirme hesapları**

   ![Entegrasyon hesabınızı bulun](./media/logic-apps-enterprise-integration-agreements/find-integration-accounts.png)

1. **Tümleştirme hesapları**altında, sözleşme oluşturmak istediğiniz tümleştirme hesabını seçin.

   ![Anlaşmanın oluşturulacağı tümleştirme hesabını seçin](./media/logic-apps-enterprise-integration-agreements/select-integration-account.png)

1. Sağ bölmede, **Bileşenler**altında, **Anlaşmalar** döşemesini seçin.

   !["Anlaşmalar" seçeneğini seçin](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

1. **Anlaşmalar**altında **Ekle'yi**seçin. **Ekle** bölmesinde, örneğin sözleşmeniz hakkında bilgi sağlayın:

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Adı** | Evet | <*anlaşma adı*> | Anlaşmanızın adı |
   | **Anlaşma türü** | Evet | **AS2**, **X12**, veya **EDIFACT** | Anlaşmanız için protokol türü. Anlaşma dosyanızı oluşturduğunuzda, bu dosyadaki içeriğin anlaşma türüyle eşleşmesi gerekir. | |  
   | **Ev Sahibi Ortak** | Evet | <*ev sahibi-ortak-ad*> | Ana bilgisayar ortağı, anlaşmayı belirten kuruluşu temsil eder |
   | **Ev Sahibi Kimliği** | Evet | <*ana bilgisayar-ortak tanımlayıcı*> | Ev sahibi ortağın tanımlayıcısı |
   | **Misafir Ortak** | Evet | <*konuk-ortak-isim*> | Konuk ortak, ev sahibi ortakla iş yapan kuruluşu temsil eder |
   | **Misafir Kimliği** | Evet | <*konuk-ortak tanımlayıcı*> | Konuk ortağın tanımlayıcısı |
   | **Ayarları Al** | Değişir | Değişir | Bu özellikler, ana bilgisayar ortağının sözleşmedeki konuk ortaktan gelen tüm iletileri nasıl aldığını belirtir. Daha fazla bilgi için ilgili anlaşma türüne bakın: <p>- [AS2 ileti ayarları](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [EDIFACT ileti ayarları](logic-apps-enterprise-integration-edifact.md) <br>- [X12 ileti ayarları](logic-apps-enterprise-integration-x12.md) |
   | **Ayarları Gönder** | Değişir | Değişir | Bu özellikler, ev sahibi ortağın sözleşmedeki konuk ortaka giden tüm iletileri nasıl gönderdiğini belirtir. Daha fazla bilgi için ilgili anlaşma türüne bakın: <p>- [AS2 ileti ayarları](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [EDIFACT ileti ayarları](logic-apps-enterprise-integration-edifact.md) <br>- [X12 ileti ayarları](logic-apps-enterprise-integration-x12.md) |
   |||||

1. Sözleşmenizi oluşturmayı bitirdiğinizde, **Ekle** sayfasında **Tamam'ı**seçin ve tümleştirme hesabınıza geri dönün.

   **Anlaşmalar** listesi artık yeni anlaşmanızı gösterir.

## <a name="edit-agreements"></a>Anlaşmaları edin

1. Azure [portalında,](https://portal.azure.com)ana Azure menüsünde **Tüm hizmetleri**seçin.

1. Arama kutusuna filtreniz olarak "tümleştirme" girin. Sonuçlardan şu kaynağı seçin: **Tümleştirme hesapları**

1. **Tümleştirme hesapları**altında, yeniden yapmak istediğiniz sözleşmeye sahip tümleştirme hesabını seçin.

1. Sağ bölmede, **Bileşenler**altında, **Anlaşmalar** döşemesini seçin.

1. **Anlaşmalar**uyarınca, sözleşmenizi seçin ve **Edit'i**seçin.

1. Değişikliklerinizi yapın ve kaydedin.

## <a name="delete-agreements"></a>Anlaşmaları silme

1. Azure [portalında,](https://portal.azure.com)ana Azure menüsünde **Tüm hizmetleri**seçin.

1. Arama kutusuna filtreniz olarak "tümleştirme" girin. Sonuçlardan şu kaynağı seçin: **Tümleştirme hesapları**

1. **Tümleştirme hesapları**altında, silmek istediğiniz sözleşmeye sahip tümleştirme hesabını seçin.

1. Sağ bölmede, **Bileşenler**altında, **Anlaşmalar** döşemesini seçin.

1. **Anlaşmalar**uyarınca, sözleşmenizi seçin ve **Sil'i**seçin.

1. Seçili anlaşmayı silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 iletilerini paylaşma](logic-apps-enterprise-integration-as2.md)
* [EDIFACT iletilerini paylaşma](logic-apps-enterprise-integration-edifact.md)
* [X12 iletilerini paylaşma](logic-apps-enterprise-integration-x12.md)
