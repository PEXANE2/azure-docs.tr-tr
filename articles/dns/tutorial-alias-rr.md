---
title: 'Öğretici: bir bölgedeki kaynak kaydına başvurmak için bir diğer ad kaydı oluşturma'
titleSuffix: Azure DNS
description: Bu öğreticide, bölgedeki kaynak kaydına başvurmak için bir Azure DNS diğer ad kaydını yapılandırma işlemi gösterilir.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: rohink
ms.openlocfilehash: 2b122a34cfd382a58f7680743d3a1cb1ae598fd1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76939258"
---
# <a name="tutorial-create-an-alias-record-to-refer-to-a-zone-resource-record"></a>Öğretici: Bölge kaynak kaydına başvurmak için diğer ad kaydı oluşturma

Diğer ad kayıtları aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesinin aynı türdeki başka bir CNAME kayıt kümesine diğer ad olmasını sağlayabilirsiniz. Bazı kayıt kümelerinin diğer ad gibi davranmasını diğerlerinin diğer ad değilmiş gibi davranmasını istiyorsanız, bu özellik yararlı olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bölgedeki kaynak kaydı için diğer ad oluşturma.
> * Diğer ad kaydını test etme.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).


## <a name="create-an-alias-record"></a>Diğer ad kaydı oluşturma

Bölgedeki kaynak kaydına işaret eden bir diğer ad kaydı oluşturun.

### <a name="create-the-target-resource-record"></a>Hedef kaynak kaydını oluşturma
1. Azure DNS bölgenizi açmak için bölgeyi seçin.
2. **Kayıt kümesi**’ni seçin.
3. **Ad** metin kutusuna **server** yazın.
4. **Tür** olarak **A** seçin.
5. **IP ADRESİ** metin kutusuna **10.10.10.10** yazın.
6. **Tamam**’ı seçin.

### <a name="create-the-alias-record"></a>Diğer ad kaydını oluşturma
1. Azure DNS bölgenizi açmak için bölgeyi seçin.
2. **Kayıt kümesi**’ni seçin.
3. **Ad** metin kutusuna **test** yazın.
4. **Tür** olarak **A** seçin.
5. **Diğer Ad Kayıt Kümesi** onay kutusunda **Evet**'i seçin. Ardından **Bölge kayıt kümesi**'ni seçin.
6. **Bölge kayıt kümesi** için **server** kaydını seçin.
7. **Tamam**’ı seçin.

## <a name="test-the-alias-record"></a>Diğer ad kaydını test etme

1. Sık kullandığınız nslookup aracını başlatın. Bir seçenek, öğesine [https://network-tools.com/nslook](https://network-tools.com/nslook)gözatmanızı sağlar.
2. A kayıtları için sorgu türünü ayarlayın ve **test.\<etki alanınızın adı\>** için arama yapın. Yanıt **10.10.10.10** olacaktır.
3. Azure portalında **server** A kaydını **10.11.11.11** olarak değiştirin.
4. Birkaç dakika bekleyin ve ardından **test** kaydı için nslookup aracını yeniden kullanın. Yanıt **10.11.11.11** olacaktır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturulan kaynaklara ihtiyacınız kalmadığında bölgenizdeki **server** ve **test** kaynak kayıtlarını silin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bölgedeki kaynak kaydına başvurmak için bir diğer ad kaydı oluşturdunuz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
