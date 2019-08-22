---
title: Sorun giderme - QnAMaker
titleSuffix: Azure Cognitive Services
description: Kullanıcının Azure hesabında barındırılan bileşenlerinin QnAMaker oluşur. Hata ayıklama QnAMaker Azure kaynaklarını yönetmek veya QnAMaker destek ekibi kendi kurulumu hakkında ek bilgi sağlamak kullanıcıların gerektirebilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b58a08c4cfa97356be4064456b2e9ec5cceccfbc
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876332"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Çalışma zamanı ve soru-cevap Oluşturucu hizmetini desteklemek için sorun giderme ipuçları

QnAMaker, kullanıcının Azure aboneliğinde barındırılan kaynakları kapsar. Hata ayıklama, kullanıcıların Azure QnAMaker kaynaklarını işlemesini veya QnAMaker destek ekibine kurulum hakkında ek bilgiler sağlamasını gerektirebilir.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>QnAMaker çalışma zamanı güncelleştirmeleri alma

QnAMaker çalışma zamanı, Azure portal [bir qnaoluşturucu hizmeti oluşturduğunuzda](./set-up-qnamaker-service-azure.md) dağıtılan Azure App Service bir parçasıdır. Güncelleştirmeler çalışma zamanı için düzenli aralıklarla hale getirilir. Soru-Cevap Oluşturma App Service, Nisan 2019 site uzantısı sürümünden (sürüm 5 +) sonra otomatik güncelleştirme modunda. Bu, yükseltme sırasında sıfır kesinti olması için zaten tasarlanmıştır. 

Geçerli sürümünüzü adresinde https://www.qnamaker.ai/UserSettings denetleyebilirsiniz. Sürümünüz 5. x sürümünden eskiyse, en son güncelleştirmeleri uygulamak için App Service yeniden başlatmanız gerekir.

1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. App Service üzerinde tıklayın ve genel bakış bölümünü açın

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. App service'ı yeniden başlatın. Bu işlem birkaç saniye içinde tamamlanmalıdır. Bu yeniden başlatma döneminde, bu QnAMaker hizmetini kullanan tüm bağımlı uygulamaların veya botların son kullanıcılar için kullanılamadığını unutmayın.

    ![QnAMaker appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>QnAMaker hizmet ana bilgisayar adını alma
QnAMaker desteği veya UserVoice başvurduğunuzda QnAMaker hizmeti ana bilgisayar adı hata ayıklama amaçları için yararlıdır. Ana bilgisayar adı şu biçimdeki bir URL: https:// *{hostname}* . azurewebsites.net.
    
1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![Azure portalında QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Soru-Cevap Oluşturma kaynakla ilişkili App Service seçin. Genellikle, adlar aynıdır.

     ![QnAMaker uygulama hizmetini seçin](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Konak adı URL genel bakış bölümünde kullanılabilir

    ![QnAMaker ana bilgisayar adı](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Etkin öğrenme ile Bilgi Bankası sorularını geliştirme](./improve-knowledge-base.md)
