---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8530f4469a0c25f3c32e652e2b0752c51c28ff3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78191076"
---
Bağlama öznitelikleri doğrudan function.json dosyasında tanımlanır. Bağlama türüne bağlı olarak ek özellikler gerekebilir. [Sıra çıktısı yapılandırması,](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) Azure Depolama sıra bağlaması için gereken alanları açıklar. Uzantı, function.json dosyasına bağlama eklemeyi kolaylaştırır. 

Bağlama oluşturmak için, HttpTrigger klasörünüzdeki dosyaya `function.json` sağ tıklayın (macOS'a Ctrl+tıkla) ve **Bağlayıcıekle'yi seçin...**. Yeni bağlama için aşağıdaki bağlama özelliklerini tanımlamak için istemleri izleyin:

| İstem | Değer | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıktı bağlayıcısidır. |
| **Yön ile bağlamayı seçin...** | `Azure Queue Storage` | Bağlama, Azure Depolama sırasına bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresini tanımlayan ad. |
| **İletinin gönderilecek sıra** | `outqueue` | Bağlamanın yazdığı sıranın adı. *QueueName* yoksa, bağlama ilk kullanımda oluşturur. |
| **"Local.setting.json" adresinden ayarı seçin** | `AzureWebJobsStorage` | Depolama hesabının bağlantı dizesini içeren bir uygulama ayarının adı. Ayar, `AzureWebJobsStorage` işlev uygulamasıyla oluşturduğunuz Depolama hesabının bağlantı dizesini içerir. |

İşlev.json `bindings` dizinize aşağıdaki gibi görünmesi gereken bir bağlama eklenir:

:::code language="son" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::