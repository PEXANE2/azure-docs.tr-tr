---
author: baanders
description: Azure dijital TWINS örnekleri-giriş olmadan DefaultAzureCredential için yerel kimlik doğrulamasını ayarlamaya yönelik dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: fb148551db798207a52bd7aef629da79dd3341e1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102212832"
---
İle `DefaultAzureCredential` , örnek, yerel ortamınızda, yerel bir [Azure CLI](/cli/azure/install-azure-cli) veya Visual Studio ya da Visual Studio Code bir Azure oturum açma gibi kimlik bilgilerini arar. Bu nedenle, örnek için kimlik bilgilerini ayarlamak üzere bu mekanizmalardan biri aracılığıyla *yerel olarak Azure 'da oturum açmanız* gerekir.

Kod örneğini çalıştırmak için Visual Studio 'Yu veya Visual Studio Code kullanıyorsanız, Bu düzenleyicide Azure dijital TWINS örneğinize erişmek için kullanmak istediğiniz Azure kimlik bilgileriyle oturum açtığınızdan emin olun.

Aksi takdirde, [Yerel Azure CLI 'yı yükleyebilir](/cli/azure/install-azure-cli), makinenizde bir komut istemi başlatabilir ve `az login` Azure hesabınızda oturum açmak için komutunu çalıştırabilirsiniz. Oturum açtıktan sonra, kod örneğinizi çalıştırdığınızda otomatik olarak oturumunuzu açmanız gerekir.