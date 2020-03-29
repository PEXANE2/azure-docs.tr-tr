---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 367ae27a6afec803d2e3f98f54bdcf852330ddc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70274643"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Kimlik doğrulama için bir ortam değişkenini yapılandırma

Uygulamaların kullandıkları Bilişsel Hizmetlere erişimi doğrulaması gerekir. Kimlik doğrulaması yapmak için, Azure Kaynaklarınız için anahtarları depolamak için bir ortam değişkeni oluşturmanızı öneririz. 

Anahtarınızı aldıktan sonra, uygulamayı çalıştıran yerel makinede yeni bir ortam değişkenine yazın. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve işletim sisteminizin yönergelerini izleyin. Kaynağınızın anahtarlarından biriyle değiştirin. `your-key`

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx COGNITIVE_SERVICE_KEY "your-key"
```

Ortam değişkenini ekledikten sonra, konsol penceresi de dahil olmak üzere ortam değişkenini okumak için gereken tüm çalışan programları yeniden başlatmanız gerekebilir. Örneğin, Visual Studio'yu düzenleyiciniz olarak kullanıyorsanız, örneği çalıştırmadan önce Visual Studio'yu yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

.bash_profile dosyanızı düzenleyin ve ortam değişkenini ekleyin:

```bash
export COGNITIVE_SERVICE_KEY=your-key
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

***