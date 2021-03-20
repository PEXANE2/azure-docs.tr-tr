---
title: Azure dosyalarını tarama
description: Bu kılavuzda, Azure dosyalarını taramanın ayrıntıları açıklanmaktadır.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/01/2020
ms.openlocfilehash: a0bd7a4cd8afafc16f05b4a37cd5723304ad931e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96555477"
---
# <a name="register-and-scan-azure-files"></a>Azure dosyalarını kaydetme ve tarama

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure dosyaları, meta verileri yakalamak ve sistem ve müşteri sınıflandırmalarına bağlı olarak meta verilerde sınıflandırmalar uygulamak için tam ve artımlı taramaları destekler.

## <a name="prerequisites"></a>Önkoşullar

- Veri kaynaklarını kaydetmeden önce bir Azure purview hesabı oluşturun. Bir purview hesabı oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
- Taramaları ayarlamak ve zamanlamak için bir veri kaynağı Yöneticisi olmanız gerekir, Ayrıntılar için lütfen [Katalog izinleri](catalog-permissions.md) bölümüne bakın.

## <a name="register-an-azure-files-storage-account"></a>Azure dosyaları depolama hesabı kaydetme

Yeni bir Azure dosyaları hesabını veri kataloğunuza kaydetmek için aşağıdakileri yapın:

1. Purview veri kataloğunuza gidin.
1. Sol gezinti bölmesinde **Yönetim Merkezi** ' ni seçin.
1. **Kaynaklar ve tarama** altında **veri kaynakları** ' nı seçin.
1. **+ Yeni** seçeneğini belirleyin.
1. **Kaynakları kaydet**' de **Azure dosyaları**' nı seçin. **Devam**’ı seçin.

:::image type="content" source="media/register-scan-azure-files/register-new-data-source.png" alt-text="Yeni veri kaynağını Kaydet" border="true":::

**Kaynakları Kaydet (Azure dosyaları)** ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin.
1. İstediğiniz depolama hesabınıza nasıl işaret etmek istediğinizi seçin:
   1. **Azure aboneliği ' nden** seçim yapın, **Azure aboneliği** açılan kutusundan ilgili abonelik ' ı ve **depolama hesabı adı** açılan kutusundan uygun depolama hesabı ' nı seçin.
   1. İsterseniz **El Ile gir** ' i seçebilir ve bir hizmet uç noktası (URL) girebilirsiniz.
1. Veri kaynağını kaydetmek için **son** ' a gidin.

:::image type="content" source="media/register-scan-azure-files/register-sources.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

## <a name="set-up-authentication-for-a-scan"></a>Tarama için kimlik doğrulamasını ayarlama

Hesap anahtarı kullanarak Azure dosyaları depolaması için kimlik doğrulaması ayarlamak üzere aşağıdakileri yapın:

1. **Hesap anahtarı** olarak kimlik doğrulama yöntemini seçin.
2. **Azure aboneliği** seçeneğini belirleyin.
3. Azure dosyaları hesabının bulunduğu Azure aboneliğinizi seçin.
4. Listeden depolama hesabınızın adını seçin.
5. **Finish (Son)** düğmesine tıklayın.

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)