---
title: Ücretsiz Azure hizmet kullanımını izleme
description: Azure portalında ve kullanım bilgilerini içeren CSV dosyasında ücretsiz hizmet kullanımının nasıl denetleneceğini öğrenin.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: ae8b26f0032c6659da18b822f4f8568995eff82b
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709776"
---
# <a name="check-free-service-usage-included-with-your-azure-free-account"></a>Ücretsiz Azure hesabınıza dahil olan ücretsiz hesap kullanımını denetleme

Hizmetlerin limitlerini aşmadığınız sürece, ücretsiz Azure hesabıyla ücretsiz olarak dahil olan hizmetler için ücretlendirilmezsiniz. Limitler dahilinde kalmak için, ücretsiz hizmet kullanımını izlemek için Azure portalını veya kullanım dosyanızı kullanabilirsiniz.

## <a name="check-usage-in-the-azure-portal"></a>Azure portalında kullanım bilgilerini denetleme

1.  [Azure Portal](https://portal.azure.com) oturum açın.

2.  Sol gezinti alanında **Tüm hizmetler**’i seçin.

3.  **Abonelikler**'i seçin.

4.  Ücretsiz hesaba kaydolduğunuzda oluşturduğunuz aboneliği seçin.

    ![Tüm abonelikleri gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  Genel bakış bölümünde, aboneliğinizle ilgili önemli bilgiler gösterilir. Örneğin, abonelik kimliği, teklif türü ve abonelik adı. Ayrıca ücretsiz hesap kredinizin süresinin ne zaman sona ereceğine dair bilgileri de bulabilirsiniz.

    ![Önemli abonelik bilgilerini gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Mevcut ve tahmini maliyetinizle ilgili bilgileri bulmak için sayfayı aşağı kaydırın. Ücretsiz hesabınıza dahil edilmeyen hizmet kullanımı ve ücretsiz hesap limitlerini aşan kullanım maliyete dahildir.

    ![Abonelik maliyeti bilgilerini gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Genel bakış bölümünün son kısmında, ücretsiz hizmet kullanımını gösteren bir tablo bulunur.

    ![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Tabloda aşağıdaki sütunlar bulunur:

* **Ölçüm Adı:** Kullanılmakta olan ölçüm için ölçü birimini belirtir. Ölçüm eşleme hizmeti hakkında bilgi edinmek için bkz. [Ücretsiz ölçüm eşleme hizmetini anlama](billing-understand-free-service-meter-mapping.md).
* **Kullanım/Limit:** Geçerli aya ait ölçüm kullanımı ve limiti. Bu bilgileri durum çubuğunda bulabilirsiniz.
* **Durum:** Ölçümün kullanım durumu. Kullanım deseninize göre aşağıdaki durumlardan birine sahip olabilirsiniz:
  * **Kullanılmıyor:** Ölçümü kullanmadınız veya ölçümün kullanımı, faturalama sistemine ulaşmadı.
  * **\<Date> tarihinde aşıldı:** \<Date> tarihinde ölçüm limitini aştınız.
  * **Aşma Olasılığı Düşük:** Ölçüm sınırını aşma olasılığınız düşüktür.
  * **\<Date> tarihinde aşıyor:** \<Date> tarihinde ölçüm limiti aşma olasılığınız yüksektir.

## <a name="check-usage-with-the-usage-file"></a>Kullanım dosyası ile kullanımı denetleme

Kullanım dosyanız, Azure aboneliğiniz için ayrıntılı bilgiler sağlar. Azure Hesap Merkezi’nden aylık ve günlük kullanım dosyanızı indirebilirsiniz. Kullanım dosyanızın nasıl indirileceği hakkında bilgi edinmek ve gerekli erişimi anlamak için bkz. [Fatura ve Kullanım Bilgilerini Alma](billing-download-azure-invoice-daily-usage-date.md). Kullanım dosyasındaki sütunlar hakkında bilgi edinmek için bkz. [Kullanım bilgilerinizdeki terimleri anlama](billing-understand-your-usage.md).

Kullanım dosyası, ücretsiz ve ücretli hizmetler için kullanım bilgilerini içerir. Ücretsiz hizmet ölçümlerinin adının sonuna **Ücretsiz** ifadesi eklenir. Ücretsiz ölçümleri bulmak için dosyayı Excel’de açın ve **- Ücretsiz** metnini içeren hücreler için **Ölçüm Kategorisi sütununu** filtreleyin (Metin Filtreleri &rarr; İçerir filtresini kullanın).


![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Aboneliğinizi yükseltme](billing-upgrade-azure-subscription.md)
