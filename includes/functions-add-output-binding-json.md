---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8530f4469a0c25f3c32e652e2b0752c51c28ff3f
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191076"
---
Bağlama öznitelikleri doğrudan function. json dosyasında tanımlanır. Bağlama türüne bağlı olarak ek özellikler gerekli olabilir. [Kuyruk çıkış yapılandırması](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) , bir Azure depolama kuyruğu bağlaması için gereken alanları açıklar. Uzantı, function. JSON dosyasına bağlama eklemeyi kolaylaştırır. 

Bir bağlama oluşturmak için, HttpTrigger klasörünüzdeki `function.json` dosyasını sağ tıklatın (macOS üzerinde CTRL + tıklama) ve **bağlama Ekle...** seçeneğini belirleyin. Yeni bağlama yönelik aşağıdaki bağlama özelliklerini tanımlamak için istemleri izleyin:

| İstem | Değer | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıkış bağlamadır. |
| **Yön içeren bağlamayı seçin...** | `Azure Queue Storage` | Bağlama bir Azure depolama kuyruğu bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresini tanımlayan ad. |
| **İletinin gönderileceği kuyruk** | `outqueue` | Bağlamanın yazdığı kuyruğun adı. *SıraAdı* mevcut olmadığında, bağlama ilk kullanımda oluşturulur. |
| **"Yerel. Setting. JSON" ayarını seçin** | `AzureWebJobsStorage` | Depolama hesabı için bağlantı dizesini içeren bir uygulama ayarının adı. `AzureWebJobsStorage` ayarı, işlev uygulamasıyla oluşturduğunuz depolama hesabı için bağlantı dizesini içerir. |

İşlevinizdeki `bindings` dizisine bir bağlama eklenir. Bu, aşağıdaki gibi görünmelidir:

:::code language="son" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::