---
title: Azure Synapse Analytics'te bağlantılı bir hizmet sağlama ve koruma
description: Yönetilen Vnet ile bağlantılı bir hizmeti nasıl sağlayacağınızı ve güvenli hale nasıl sağlayacağınızı öğrenin
services: synapse-analytics
author: acomet
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 435c3fd6b1e6444fa3a31c68b4d74c2553d2e634
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430558"
---
# <a name="securing-a-linked-service-with-private-links"></a>Özel Bağlantılar ile bağlantılı bir hizmeti güvence altına alma 

Bu makalede, Synapse'de bağlantılı bir hizmeti özel bir bitiş noktasıyla nasıl güvence altına alacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure Depolama hesabı**: Azure Veri Gölü Gen 2'yi *kaynak* veri deposu olarak kullanıyorsunuz. Depolama hesabınız yoksa, bir depolama hesabı oluşturmak için adımlar için [bir Azure Depolama hesabı oluşturma'ya](../../storage/blobs/data-lake-storage-quickstart-create-account.md) bakın. Depolama Hesabı'na erişmek için Synapse Studio IP filtresine sahip olduğundan ve yalnızca **Seçili ağların** Depolama hesabına erişmesine izin verdiğinden emin olun. Bıçak **Güvenlik Duvarları ve sanal ağların** altındaki ayar aşağıdaki resimdeki gibi görünmelidir.

![Güvenli Depolama Hesabı](./media/secure-storage-account.png)

## <a name="create-a-linked-service-with-private-links"></a>Özel Bağlantılar ile bağlantılı bir hizmet oluşturma

Azure Synapse Analytics'te bağlantılı bir hizmet, bağlantı bilgilerinizi diğer hizmetlerle tanımladığınız yerdir. Bu bölümde, Azure Synapse Analytics ve Azure Data Lake Gen 2'yi bağlantılı hizmetler olarak eklersiniz.

1. Azure Synapse Studio'yu açın ve **Yönet** sekmesine gidin.
1. **Dış bağlantılar**altında, **Bağlantılı hizmetleri**seçin.
1. Bağlantılı bir hizmet eklemek için **Yeni'yi**tıklatın.
1. Listeden Azure Veri Gölü Depolama Gen2 döşemesini seçin ve **Devam et'i**tıklatın.
1. **Etkileşimli Yazma'yı etkinleştirdiğinizden**emin olun. Etkinleştirilmesi yaklaşık 1 dakika sürebilir. 
1. Kimlik doğrulama kimlik bilgilerinizi girin. Hesap anahtarı, hizmet sorumlusu ve yönetilen kimlik şu anda desteklenen kimlik doğrulama türleridir. Kimlik bilgilerinizin doğru luğunu doğrulamak için test bağlantısını tıklatın.
1. **Test bağlantısını**seçin, Depolama Hesabı özel bir Bitiş Noktası oluşturma ve onayı olmadan bu hesaba erişim emmediği için başarısız olmalıdır. Hata iletisinde, bir sonraki bölüme gitmek için izleyebileceğiniz özel bir **bitiş noktası** oluşturmak için bir bağlantı görmeniz gerekir. Bu bağlantıyı izlerseniz, bir sonraki bölümü atlayın.
1. Bittiğinde **Oluştur**’u seçin.

## <a name="create-a-managed-private-endpoint"></a>Yönetilen özel bitiş noktası oluşturma

Yukarıdaki bağlantıyı test ederken köprüye tıklamadıysanız, aşağıdaki yolu izleyin. Şimdi, yukarıda oluşturulan bağlantılı hizmete bağlanacağınız yönetilen bir özel bitiş noktası oluşturmanız gerekir.

1. **Yönet** sekmesine gidin.
1. **Yönetilen Sanal Ağlar** bölümüne gidin.
1. Yönetilen özel bitiş noktası altında **+ Yeni'yi** seçin.
1. Listeden Azure Veri Gölü Depolama Gen2 döşemesini seçin ve **Devam et'i**seçin.
1. Yukarıda oluşturduğunuz Depolama Hesabının adını girin.
1. **Oluştur'u** seçin
1. Birkaç saniye bekledikten sonra oluşturulan özel bağlantının onay alması gerektiğini görmelisiniz.

## <a name="approval-of-a-private-link"></a>Özel bir bağlantının onayı
1. Yukarıda oluşturduğunuz Özel Bitiş Noktası'nı seçin. Depolama Hesabı düzeyinde Özel Bitiş Noktasını onaylamanızı sağlayacak bir köprü görebilirsiniz. *Alternatif olarak doğrudan Azure portalı Depolama Hesabı'na gidin ve **Özel uç nokta bağlantıları** bıçağına gidin.*
1. Stüdyoda oluşturduğunuz Özel bitiş noktasını işaretleyin ve **Onayla'yı**seçin.
1. Açıklama ekleyin ve **evet'i** tıklatın
1. **Yönet**Sekmesinin **Yönetilen Sanal Ağlar** bölümü altındaki Synapse Studio'ya geri dön.
1. Özel bitiş noktanız için onay almak yaklaşık 1 dakika sürer.

## <a name="check-the-connection-works"></a>Bağlantı çalışmalarını denetleme
1. **Yönet** sekmesine gidin ve oluşturduğunuz bağlantılı hizmeti seçin.
1. **Etkileşimli yazmanın** etkin olduğundan emin olun.
1. **Bağlantıyı sına**’yı seçin. Bağlantının başarılı olduğunu görmelisiniz.

Şimdi Synapse ve bağlantılı hizmet arasında güvenli ve özel bir bağlantı kurduk!

## <a name="next-steps"></a>Sonraki adımlar

Synapse Analytics'te Yönetilen özel bitiş noktası hakkında daha fazla bilgi geliştirmek için [Synapse Yönetilen özel bitiş noktası](data-integration-data-lake.md) makalesi nin etrafındaki konsepte bakın.

Synapse Analytics için veri tümleştirmesi hakkında daha fazla bilgi için, [Verileri Veri Gölü makalesine doğru sindirin'](data-integration-data-lake.md) e bakın.