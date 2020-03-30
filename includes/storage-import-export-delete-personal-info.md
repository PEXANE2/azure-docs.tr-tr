---
title: include dosyası
description: include dosyası
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188584"
---
## <a name="deleting-personal-information"></a>Kişisel bilgileri silme

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

Kişisel bilgiler, ithalat ve ihracat işlemleri sırasında ithalat/ihracat hizmeti (portal ve API üzerinden) ile ilgilidir. Bu işlemler sırasında kullanılan veriler şunlardır:

- Kişi adı
- Telefon numarası
- Email
- Açık adres
- Şehir
- Posta kodu
- Durum
- Ülke/İl/Bölge
- Sürücü kimliği
- Taşıyıcı hesap numarası
- Kargo takip numarası

Bir alma/dışa aktarım işi oluşturulduğunda, kullanıcılar iletişim bilgilerini ve bir sevkiyat adresi sağlar. Kişisel bilgiler en fazla iki farklı konumda saklanır: işte ve isteğe bağlı olarak portal ayarlarında. Kişisel bilgiler yalnızca, ihracat sürecinin *İade gönderi bilgileri* bölümünde varsayılan olarak etiketlenmiş onay kutusunu, operatör **kaydet ve iade adresini** kontrol ederseniz portal ayarlarında depolanır.

Kişisel iletişim bilgileri aşağıdaki yollarla silinebilir:

- İşle birlikte kaydedilen veriler iş ile birlikte silinir. Kullanıcılar işleri el ile silebilir ve tamamlanan işler 90 gün sonra otomatik olarak silinir. Rest API veya Azure portalı aracılığıyla işleri el ile silebilirsiniz. Azure portalındaki işi silmek için içe aktarma/dışa aktarma işine gidin ve komut çubuğundan *Sil'i* tıklatın. REST API üzerinden bir alma/dışa aktarma işi nasıl silinir hakkında ayrıntılı bilgi için, [bir alma/dışa aktarma işini sil'e](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md)bakın.

- Portal ayarlarında kaydedilen iletişim bilgileri, portal ayarları silinerek kaldırılabilir. Aşağıdaki adımları izleyerek portal ayarlarını silebilirsiniz:
  - [Azure portalında](https://portal.azure.com)oturum açın.
  - *Ayarlar* simgesi ![Azure Ayarları Simgesi'ne tıklayın](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - *Tüm ayarları Dışa Aktar'ı* tıklatın `.json` (geçerli ayarlarınızı bir dosyaya kaydetmek için).
  - Kaydedilmiş kişi bilgileri de dahil olmak üzere tüm ayarları silmek için *tüm ayarları ve özel panoları* sil'i tıklatın.

Daha fazla bilgi için Güven [Merkezi'ndeki](https://www.microsoft.com/trustcenter) Microsoft Gizlilik ilkesini inceleyin