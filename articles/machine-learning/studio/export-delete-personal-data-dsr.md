---
title: Verilerinizi dışa aktarma ve silme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) tarafından depolanan ürün içi veriler, Azure portalı üzerinden ve ayrıca kimlik doğrulaması REST API'leri aracılığıyla dışa aktarma ve silme için kullanılabilir. Telemetri verilerine Azure Gizlilik Portalı üzerinden erişilebilir. Bu makalede, nasıl gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 05/25/2018
ms.openlocfilehash: c380d10d0c68794ec3810cea25341d68bb41400d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251693"
---
# <a name="export-and-delete-in-product-user-data-from-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'dan ürün içi kullanıcı verilerini dışa aktarma ve silme (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Portalı, Studio (klasik) arabirimi, PowerShell ve kimlik doğrulaması REST API'lerini kullanarak Azure Machine Learning Studio (klasik) tarafından depolanan ürün içi verileri silebilir veya dışa aktarabilirsiniz. Bu makalede nasıl söyler. 

Telemetri verilerine Azure Gizlilik portalından erişilebilir. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-classic-collect"></a>Studio (klasik) ne tür kullanıcı verileri toplar?

Bu hizmet için, kullanıcı verileri çalışma alanlarına ve hizmetle kullanıcı etkileşimlerinin telemetri kayıtlarına erişmeye yetkili kullanıcılar hakkındaki bilgilerden oluşur.

Machine Learning Studio'da iki tür kullanıcı verisi vardır (klasik):
- **Kişisel hesap verileri:** Hesapla ilişkili hesap tevkimi ve e-posta adresleri.
- **Müşteri verileri:** Analiz etmek için yüklediğiniz veriler.

## <a name="studio-classic-account-types-and-how-data-is-stored"></a>Stüdyo (klasik) hesap türleri ve verilerin nasıl depolanır

Machine Learning Studio (klasik) hesapları üç türü vardır. Sahip olduğunuz hesap türü, verilerinizin nasıl depolanabileceğini ve nasıl silebileceğinizi veya dışa aktarabileceğinizi belirler.

- **Konuk çalışma alanı** ücretsiz, anonim bir hesaptır. E-posta adresi veya parola gibi kimlik bilgileri sağlamadan kaydolursunuz.
    -  Konuk çalışma alanı sona erdikten sonra veriler temizlenir.
    - Konuk kullanıcılar müşteri verilerini Kullanıcı Bira, REST API'leri veya PowerShell paketi aracılığıyla dışa aktarabilir.
- **Ücretsiz çalışma alanı,** Microsoft hesap kimlik bilgileriyle oturum açtırdığınız ücretsiz bir hesaptır - bir e-posta adresi ve parola.
    - Veri konu haklarına (DSR) tabi olan kişisel ve müşteri verilerini dışa aktarabilir ve silebilirsiniz.
    - Müşteri verilerini UI, REST API'leri veya PowerShell paketi aracılığıyla dışa aktarabilirsiniz.
    - Azure AD hesaplarını kullanmayan ücretsiz çalışma alanlarında, telemetri Gizlilik Portalı kullanılarak ihraç edilebilir.
    - Çalışma alanını sildiğinizde, tüm kişisel müşteri verilerini silersiniz.
- **Standart çalışma alanı,** oturum açma kimlik bilgileriyle erişebildiğiniz ücretli bir hesaptır.
    - DSR isteklerine tabi olan kişisel ve müşteri verilerini dışa aktarabilir ve silebilirsiniz.
    - Azure Gizlilik portalı üzerinden verilere erişebilirsiniz
    - Kişisel ve müşteri verilerini UI, REST API'leri veya PowerShell paketi aracılığıyla dışa aktarabilirsiniz
    - Azure portalındaki verilerinizi silebilirsiniz.

## <a name="delete-workspace-data-in-studio-classic"></a><a name="delete"></a>Studio'daki çalışma alanı verilerini silme (klasik) 

### <a name="delete-individual-assets"></a>Tek tek varlıkları silme

Kullanıcılar, çalışma alanındaki varlıkları seçip silme düğmesini seçerek silebilir.

![Machine Learning Studio'daki varlıkları silme (klasik)](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Çalışma alanının tamamını silme

Kullanıcılar ayrıca tüm çalışma alanlarını da silebilir:
- Ücretli çalışma alanı: Azure portalı üzerinden silin.
- Boş çalışma alanı: **Ayarlar** bölmesinde sil düğmesini kullanın.

![Machine Learning Studio'da ücretsiz çalışma alanını silme (klasik)](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-classic-data-with-powershell"></a>PowerShell ile Export Studio (klasik) veri
Tüm bilgilerinizi komutları kullanarak Azure Machine Learning Studio'dan (klasik) taşınabilir bir biçime aktarmak için PowerShell'i kullanın. Daha fazla bilgi için [Azure Machine Learning Studio (klasik) makalesi için PowerShell modülüne](powershell-module.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini ve taahhüt planı faturalandırmasını kapsayan belgeler için [Azure Machine Learning Studio (klasik) REST API başvurusuna](https://docs.microsoft.com/rest/api/machinelearning/)bakın. 
