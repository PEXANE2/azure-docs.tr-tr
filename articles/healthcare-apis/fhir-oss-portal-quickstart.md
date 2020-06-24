---
title: 'Azure portal: Azure için açık kaynak FHıR sunucusunu dağıtma-FHıR için Azure API'
description: Bu hızlı başlangıçta, Azure portal kullanarak Microsoft açık kaynak FHıR sunucusunun nasıl dağıtılacağı açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 5109c9a7c6432e42c6841b89cc28bde3e92c74aa
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "84820189"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak açık kaynak FHıR sunucusunu dağıtma

Bu hızlı başlangıçta, Azure portal kullanarak Azure 'da açık kaynaklı bir FHıR sunucusu dağıtmayı öğreneceksiniz. [Açık kaynak deposundaki](https://github.com/Microsoft/fhir-server) kolay dağıtım bağlantılarını kullanacağız

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="github-open-source-repository"></a>GitHub açık kaynak deposu

[GitHub dağıtım sayfasına](https://github.com/Microsoft/fhir-server/blob/master/docs/DefaultDeployment.md) gidin ve "Azure 'a dağıt" düğmelerini bulun:

![Kaynak dağıtım sayfasını aç](media/quickstart-oss-portal/deployment-page-oss.png)

Dağıtım düğmesine tıklayın ve Azure portal açılır.

## <a name="fill-in-deployment-parameters"></a>Dağıtım parametrelerini doldur

Yeni bir kaynak grubu oluşturmayı seçip bir ad verin. Hizmetin adı yalnızca diğer gerekli parametredir.

![Özel dağıtım parametreleri](media/quickstart-oss-portal/deployment-custom-parameters.png)

Dağıtımın kaynak kodu GitHub 'daki açık kaynaklı depodan doğrudan çekeceğini unutmayın. Depoyu kullandıysanız, için kendi kendinize ve belirli bir dala işaret edebilirsiniz.

Ayrıntıları doldurduktan sonra dağıtımı başlatabilirsiniz.

## <a name="validate-fhir-server-is-running"></a>FHıR sunucusunun çalıştığını doğrula

Dağıtım tamamlandıktan sonra, `https://SERVICENAME.azurewebsites.net/metadata` bir yetenek beyanı elde etmek için tarayıcınızı işaret edebilirsiniz. Sunucunun ilk kez yanıt vermesi birkaç dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için, sağlanan kaynakları içeren kaynak grubunu seçin, **kaynak grubunu sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHıR sunucusunu aboneliğinize dağıttınız. Postman kullanarak FHıR API 'sine nasıl erişebileceğinizi öğrenmek için Postman öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHıR API 'sine erişme](access-fhir-postman-tutorial.md)