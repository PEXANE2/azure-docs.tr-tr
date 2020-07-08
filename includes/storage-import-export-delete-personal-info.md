---
title: dosya dahil etme
description: dosya dahil etme
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188584"
---
## <a name="deleting-personal-information"></a>Kişisel bilgileri silme

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

Kişisel bilgiler içeri/dışarı aktarma hizmeti (portal ve API aracılığıyla) içeri ve dışarı aktarma işlemleri sırasında ilgilidir. Bu süreçler sırasında kullanılan veriler şunlardır:

- Kişi adı
- Telefon numarası
- E-posta
- Açık adres
- Şehir
- Posta kodu
- Durum
- Ülke/İl/Bölge
- Sürücü kimliği
- Taşıyıcı hesap numarası
- Kargo takip numarası

İçeri/dışarı aktarma işi oluşturulduğunda, kullanıcılar kişi bilgilerini ve bir sevkiyat adresini sağlar. Kişisel bilgiler en fazla iki farklı konuma depolanır: işte ve isteğe bağlı olarak portal ayarları. Kişisel bilgiler yalnızca portal ayarları ' nda depolanır, bir onay kutusunu işaretlerseniz, **taşıyıcı ve iade adresini varsayılan olarak kaydedin ve** dışarı aktarma işleminin *Teslim bilgileri* bölümüne bakın.

Kişisel iletişim bilgileri aşağıdaki yollarla silinebilir:

- İşle kaydedilen veriler işle birlikte silinir. Kullanıcılar işleri el ile silebilir ve tamamlanan işler, 90 gün sonra otomatik olarak silinir. REST API veya Azure portal aracılığıyla işleri el ile silebilirsiniz. Azure portal işi silmek için içeri/dışarı aktarma işinize gidin ve komut çubuğundan *Sil* ' e tıklayın. İçeri/dışarı aktarma işini REST API aracılığıyla silme hakkında daha fazla bilgi için [içeri/dışarı aktarma Işini silme](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md)bölümüne bakın.

- Portalda kaydedilen iletişim bilgileri portal ayarları silinerek kaldırılabilir. Aşağıdaki adımları izleyerek Portal ayarlarını silebilirsiniz:
  - [Azure portalında](https://portal.azure.com) oturum açın.
  - *Ayarlar* simgesi ![ Azure ayarları simgesine tıklayın.](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - *Tüm ayarları dışarı aktar* ' a tıklayın (geçerli ayarlarınızı bir dosyaya kaydetmek için `.json` ).
  - Kayıtlı iletişim bilgileri dahil tüm ayarları silmek için *tüm ayarları ve özel panoları Sil* ' e tıklayın.

Daha fazla bilgi için [Güven Merkezi](https://www.microsoft.com/trustcenter) 'Nde Microsoft gizlilik ilkesini gözden geçirin