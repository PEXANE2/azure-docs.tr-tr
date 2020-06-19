---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 95bd83575809f6ecda716ff751b47b7bb499cae3
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85073367"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Kimlik doğrulaması için bir ortam değişkeni yapılandırma

Uygulamaların kullandıkları bilişsel hizmetlere erişiminin kimliğini doğrulaması gerekir. Kimlik doğrulaması yapmak için, Azure kaynaklarınızın anahtarlarını depolamak üzere bir ortam değişkeni oluşturmanızı öneririz. 

Anahtarınızı aldıktan sonra, uygulamayı çalıştıran yerel makinede yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. `your-key`Kaynağınızın anahtarlarından biriyle değiştirin.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx COGNITIVE_SERVICE_KEY "your-key"
```

Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, düzenleyiciniz olarak Visual Studio kullanıyorsanız, örneği çalıştırmadan önce Visual Studio 'Yu yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

.bash_profile dosyanızı düzenleyin ve ortam değişkenini ekleyin:

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bash_profile` çalıştırın.

***
