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
ms.openlocfilehash: 137aca7c6c857ee6e833c359b710e1c1848d15ed
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95560436"
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

- İşle kaydedilen veriler işle birlikte silinir. Kullanıcılar işleri el ile silebilir ve tamamlanan işler, 90 gün sonra otomatik olarak silinir. REST API veya Azure portal aracılığıyla işleri el ile silebilirsiniz. Azure portal işi silmek için içeri/dışarı aktarma işinize gidin ve komut çubuğundan *Sil* ' e tıklayın. İçeri/dışarı aktarma işini REST API aracılığıyla silme hakkında daha fazla bilgi için [içeri/dışarı aktarma Işini silme](/previous-versions/azure/storage/common/storage-import-export-cancelling-and-deleting-jobs)bölümüne bakın.

- Portalda kaydedilen iletişim bilgileri portal ayarları silinerek kaldırılabilir. Aşağıdaki adımları izleyerek Portal ayarlarını silebilirsiniz:
  - [Azure portalında](https://portal.azure.com) oturum açın.
  - *Ayarlar* simgesi ![ Azure ayarları simgesine tıklayın.](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - *Tüm ayarları dışarı aktar* ' a tıklayın (geçerli ayarlarınızı bir dosyaya kaydetmek için `.json` ).
  - Kayıtlı iletişim bilgileri dahil tüm ayarları silmek için *tüm ayarları ve özel panoları Sil* ' e tıklayın.

Daha fazla bilgi için [Güven Merkezi](https://www.microsoft.com/trustcenter) 'Nde Microsoft gizlilik ilkesini gözden geçirin