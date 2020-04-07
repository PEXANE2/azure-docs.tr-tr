---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/30/2020
ms.author: glenga
ms.openlocfilehash: 44969063765099d350e21abfbd07792891443911
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673200"
---
::: zone pivot="programming-language-python"  
## <a name="create-and-activate-a-virtual-environment"></a><a name="create-venv"></a>Sanal ortam oluşturma ve etkinleştirme

Uygun bir klasörde, ', adlı sanal bir ortam `.venv`oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın. Azure İşlevleri tarafından desteklenen Python 3.8, 3.7 veya 3.6'yı kullandığınızdan emin olun.

# <a name="bash"></a>[bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Python venv paketini Linux dağıtımınıza yüklemediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Bu etkinleştirilen sanal ortamda sonraki tüm komutları çalıştırın. (Sanal ortamdan çıkmak `deactivate`için çalıştırın.)
::: zone-end