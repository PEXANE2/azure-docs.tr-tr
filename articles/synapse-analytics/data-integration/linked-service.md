---
title: Bağlı bir hizmetin güvenliğini sağlama
description: Yönetilen VNet ile bağlı bir hizmeti sağlamayı ve güvenliğini sağlama hakkında bilgi edinin
services: synapse-analytics
author: acomet
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 1ce127dbfd9984b3fb18e518701cbbd3a87f5988
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387259"
---
# <a name="secure-a-linked-service-with-private-links"></a>Bağlı bir hizmetin özel bağlantılarla güvenliğini sağlama 

Bu makalede, özel bir uç nokta ile SYNAPSE 'de bağlı bir hizmetin nasıl güvenli hale alınacağını öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: Azure Data Lake Gen 2 ' nı *kaynak* veri deposu olarak kullanırsınız. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../../storage/blobs/data-lake-storage-quickstart-create-account.md) . Depolama hesabının buna erişmek için SYNAPSE Studio IP filtrelemesine sahip olduğundan ve yalnızca **Seçili ağların** depolama hesabına erişmesine izin verdiğinizden emin olun. Dikey pencere **güvenlik duvarları ve sanal ağlar** altındaki ayar aşağıdaki resim gibi görünmelidir.

![Güvenli depolama hesabı](./media/secure-storage-account.png)

## <a name="create-a-linked-service-with-private-links"></a>Özel bağlantılarla bağlantılı hizmet oluşturma

Azure SYNAPSE Analytics 'te, bağlı bir hizmet, bağlantı bilgilerinizi diğer hizmetlere tanımladığınız yerdir. Bu bölümde, Azure SYNAPSE Analytics ve Azure Data Lake Gen 2 ' yi bağlı hizmetler olarak ekleyeceksiniz.

1. Azure SYNAPSE Studio 'Yu açın ve **Yönet** sekmesine gidin.
1. **Dış bağlantılar**altında **bağlı hizmetler**' i seçin.
1. Bağlı bir hizmet eklemek için **Yeni**' ye tıklayın.
1. Listeden Azure Data Lake Storage 2. kutucuğunu seçin ve **devam**' a tıklayın.
1. **Etkileşimli yazma**özelliğini etkinleştirdiğinizden emin olun. 1 dakika içinde etkinleştirilmesi gerekebilir. 
1. Kimlik doğrulama kimlik bilgilerinizi girin. Hesap anahtarı, hizmet sorumlusu ve yönetilen kimlik, şu anda desteklenen kimlik doğrulama türleridir. Kimlik bilgilerinizin doğru olduğunu doğrulamak için Bağlantıyı Sına ' ya tıklayın.
1. **Bağlantıyı test**et ' i seçtiğinizde, depolama hesabı özel bir uç noktanın oluşturulması ve onaylanması olmadan bu hesaba erişimi etkinleştirmediğinden başarısız olması gerekir. Hata iletisinde, bir sonraki bölüme gitmek için izleyebileceğiniz bir **Özel uç nokta** oluşturmak üzere bir bağlantı görmeniz gerekir. Bu bağlantıyı izlerseniz, sonraki parçayı atlayın.
1. Bittiğinde **Oluştur**’u seçin.

## <a name="create-a-managed-private-endpoint"></a>Yönetilen özel uç nokta oluşturma

Bu durumda, yukarıdaki bağlantıyı sınarken köprüye tıklamadınız, aşağıdaki yolu izleyin. Şimdi, yukarıda oluşturulan bağlı hizmete bağlanacak bir yönetilen özel uç noktası oluşturmanız gerekir.

1. **Yönet** sekmesine gidin.
1. **Yönetilen sanal ağlar** bölümüne gidin.
1. Yönetilen özel uç nokta altında **+ Yeni** ' yi seçin.
1. Listeden Azure Data Lake Storage 2. kutucuğunu seçin ve **devam**' ı seçin.
1. Yukarıda oluşturduğunuz depolama hesabının adını girin.
1. **Oluştur**’u seçin
1. Özel bağlantının oluşturulduğu saniye bekledikten sonra bir onay gerektiğini görmeniz gerekir.

## <a name="private-link-approval"></a>Özel bağlantı onayı
1. Yukarıda oluşturduğunuz özel uç noktayı seçin. Depolama hesabı düzeyinde özel uç noktasını onaylamanıza olanak sağlayacak bir köprü görebilirsiniz. *Alternatif olarak, Azure portal depolama hesabına doğrudan gidip **Özel uç nokta bağlantıları** dikey penceresine gidebilirsiniz.*
1. Studio 'da oluşturduğunuz özel uç noktayı işaret edin ve **Onayla**' yı seçin.
1. Bir açıklama ekleyin ve **Evet** ' e tıklayın.
1. **Yönet** sekmesinin **yönetilen sanal ağlar** bölümünde SYNAPSE Studio 'ya geri dönün.
1. Onayın özel uç noktanıza yansıtılmasıyla ilgili olarak yaklaşık 1 dakika sürer.

## <a name="check-the-connection-works"></a>Bağlantının çalışıp çalışmadığını denetleyin
1. **Yönet** sekmesine gidin ve oluşturduğunuz bağlı hizmeti seçin.
1. **Etkileşimli yazmanın** etkin olduğundan emin olun.
1. **Bağlantıyı sına**’yı seçin. Bağlantının başarılı olduğunu görmeniz gerekir.

Artık SYNAPSE ile bağlı hizmetiniz arasında güvenli ve özel bir bağlantı oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar

SYNAPSE Analytics 'te yönetilen özel uç nokta hakkında daha fazla bilgi için, [SYNAPSE tarafından yönetilen özel uç nokta makalesindeki kavram](data-integration-data-lake.md) bölümüne bakın.

SYNAPSE Analytics veri tümleştirmesi hakkında daha fazla bilgi için bkz. [verileri Data Lake bir makaleye dönüştürme](data-integration-data-lake.md) .
