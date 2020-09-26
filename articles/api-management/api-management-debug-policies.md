---
title: Visual Studio Code 'de Azure API Management ilkeleri hatalarını ayıklama | Microsoft Docs
description: Azure API Management Visual Studio Code uzantısı 'nı kullanarak Azure API Management Ilkelerini nasıl ayıklayacağınızı öğrenin
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/22/2020
ms.author: apimpm
ms.openlocfilehash: 4eb32243df219d721d7baae80984c45d0fc4cf25
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91345130"
---
# <a name="debug-azure-api-management-policies-in-visual-studio-code"></a>Visual Studio Code Azure API Management ilkeleri hatalarını ayıklama

Azure API Management [ilkeleri](api-management-policies.md) , API yayımcıların kimlik doğrulama, yetkilendirme, azaltma, önbelleğe alma ve dönüştürme gibi çapraz kesme sorunlarını ele almasını sağlayan güçlü yetenekler sağlar. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. 

Bu makalede [Visual Studio Code Için Azure API Management uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement)kullanılarak API Management ilkelerinin nasıl ayıklanacağı açıklanır. 

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

Önce bu [öğreticiyi](get-started-create-service-instance.md) Izleyerek API Management geliştirici katmanı örneği oluşturun.

Visual Studio Code için [Visual Studio Code](https://code.visualstudio.com/) ve [Azure API Management uzantısının](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement)en son sürümünü yükler. 

## <a name="restrictions-and-limitations"></a>Kısıtlamalar ve sınırlamalar

Bu özellik yalnızca API Management geliştirici katmanında kullanılabilir. Her API Management örneği yalnızca bir eşzamanlı hata ayıklama oturumu destekler.

## <a name="initiate-a-debugging-session"></a>Hata ayıklama oturumu başlatma

1. Visual Studio Code'u başlatma
2. Azure uzantıları altındaki API Management uzantısına gidin
3. Hata ayıklamak için API Management örneğini bulma
4. Hata ayıklamak için API ve işlem bulma
5. İşleme sağ tıklayın ve **ilke hata ayıklamayı Başlat** ' ı seçin.

Bu noktada, uzantı API Management ağ geçidiyle bir hata ayıklama oturumu başlatmaya ve kurmaya çalışacaktır.

![hata ayıklamayı Başlat](media/api-management-debug-policies/initiate-debugging-session.png)

## <a name="send-a-test-request"></a>Test isteği gönder
Hata ayıklama oturumu oluşturulduğunda, uzantı, [rest istemci uzantısı](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)aracılığıyla bu işleme BIR test http isteği oluşturup gönderebileceğimizi sağlayan yeni bir düzenleyici açar.

**OCP-apim-Debug** üst bilgisinin isteğe zaten eklenmiş olduğunu fark edeceksiniz. Bu üst bilgi gereklidir ve API Management ağ geçidinde hata ayıklama işlevini tetiklemek için değerin hizmet düzeyi, tüm erişim abonelik anahtarına ayarlanması gerekir.

Düzenleyicideki HTTP isteğini test senaryonuza göre değiştirin. Ardından, API Management ağ geçidine test isteği göndermek için **isteği gönder** ' e tıklayın.

![test isteği gönder](media/api-management-debug-policies/rest-client.png)

## <a name="debug-policies"></a>Hata ayıklama ilkeleri
Sınama HTTP isteği gönderildikten sonra uzantı, bu işlemin etkili ilkelerini gösteren hata ayıklama penceresini açar ve ilk etkin ilkede durur. 

![hata ayıklama ilkeleri](media/api-management-debug-policies/main-window.png)

İlke ardışık düzenini izlemek için, tek tek ilkelerde tek bir işlem yapabilir veya ilkede bir kesme noktası ayarlayabilir ve doğrudan bu ilkeye adımlayın. 

**Değişkenler** panelinde, sistem tarafından oluşturulan ve Kullanıcı tarafından oluşturulan değişkenlerin değerlerini inceleyebilirsiniz. **Kesme noktaları** panelinde, ayarlanmış tüm kesme noktalarının listesini görebilirsiniz. **Çağrı yığını** panelinde geçerli etkili ilke kapsamını görebilirsiniz. 

İlke yürütme sırasında bir hata oluşursa, hatanın oluştuğu ilkede hatanın ayrıntılarını görürsünüz. 

![larý](media/api-management-debug-policies/exception.png)

> [!TIP]
> İşiniz bittiğinde **Durdur** düğmesine tıklayarak hata ayıklama oturumundan çıkmanız gerektiğini unutmayın.


## <a name="next-steps"></a>Sonraki adımlar

+ [Visual Studio Code API Management uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement)hakkında daha fazla bilgi edinin. 
+ [GitHub deposundaki](https://github.com/Microsoft/vscode-apimanagement) sorunları bildirme

