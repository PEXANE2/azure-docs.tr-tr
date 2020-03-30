---
title: AWS CloudTrail'i Azure Sentinel'e bağlayın | Microsoft Dokümanlar
description: AWS CloudTrail verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 5cbef1f31ea7088d4fab4888f5630af1b765a910
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588663"
---
# <a name="connect-azure-sentinel-to-aws-cloudtrail"></a>Azure Sentinel'i AWS CloudTrail'e bağlayın

Tüm AWS CloudTrail etkinliklerinizi Azure Sentinel'e aktarmak için AWS konektörünü kullanın. Bu bağlantı işlemi, AWS CloudTrail ile Azure Sentinel arasında güven ilişkisi oluşturarak Azure Sentinel'e AWS kaynak günlüklerinize erişmesini verir. Bu, Azure Sentinel'in AWS günlüklerinize erişmesi için izin veren bir rol oluşturarak AWS'de gerçekleştirilir.

## <a name="prerequisites"></a>Ön koşullar

Azure Sentinel çalışma alanında yazma izniniz olmalıdır.

> [!NOTE]
> Azure Sentinel, cloudtrail etkinliklerini tüm bölgelerden toplar. Olayları bir bölgeden diğerine akışı yapmamanız önerilir.

## <a name="connect-aws"></a>AWS’ye bağlanma 


1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından tablodaki **Amazon Web Hizmetleri** satırını ve sağdaki AWS bölmesinde Açık bağlayıcı **sayfasını**tıklatın.

1. Aşağıdaki adımları kullanarak **Yapılandırma** altındaki yönergeleri izleyin.
 
1.  Amazon Web Services konsolunuzda, **Güvenlik, Kimlik & Uyumluluk**altında **IAM'yi**seçin.

    ![AWS1](./media/connect-aws/aws-1.png)

1.  **Roller'i** seçin ve **Rol Oluştur'u**seçin.

    ![AWS2](./media/connect-aws/aws-2.png)

1.  **Başka bir AWS hesabı seçin.** Hesap **Kimliği** alanında, Azure Sentinel portalındaki AWS bağlayıcı sayfasında bulunan **Microsoft Hesap Kimliği'ni** **(12341234)** girin.

    ![AWS3](./media/connect-aws/aws-3.png)

1.  Dış **Kimlik İste'nin** seçildiğinden emin olun ve ardından Azure Sentinel portalındaki AWS bağlayıcı sayfasında bulunan Harici Kimliği'ni (Çalışma Alanı Kimliği) girin.

    ![AWS4](./media/connect-aws/aws-4.png)

1.  **İzinekle ilkesi** altında **AWSCloudTrailReadOnlyAccess**seçin.

    ![AWS5](./media/connect-aws/aws-5.png)

1.  Etiket girin (İsteğe Bağlı).

    ![AWS6](./media/connect-aws/aws-6.png)

1.  Ardından, bir **Rol adı** girin ve rol **oluştur** düğmesini seçin.

    ![AWS7](./media/connect-aws/aws-7.png)

1.  Roller listesinde, oluşturduğunuz rolü seçin.

    ![AWS8](./media/connect-aws/aws-8.png)

1.  Rol **ARN'yi**kopyalayın. Azure Sentinel portalında, Amazon Web Hizmetleri bağlayıcısı ekranında, alan **eklemek için Rolü'ne** yapıştırın ve **Ekle'yi**tıklatın.

    ![AWS9](./media/connect-aws/aws-9.png)

1. AWS etkinlikleri için Log Analytics'teki ilgili şemayı kullanmak için **AWSCloudTrail'i**arayın.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, AWS CloudTrail'i Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

