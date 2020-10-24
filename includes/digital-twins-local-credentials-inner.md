---
author: baanders
description: Azure dijital TWINS örnekleri-giriş olmadan DefaultAzureCredential için yerel kimlik doğrulamasını ayarlamaya yönelik dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 8bf0590b867d37a22706f679bfbeee2140935637
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494686"
---
İle `DefaultAzureCredential` örnek, yerel ortamınızda yerel bir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) veya Visual Studio/Visual Studio Code 'de Azure oturum açma gibi kimlik bilgilerini arar. Bu, örneğin kimlik bilgilerini ayarlamak için bu mekanizmalardan biri aracılığıyla **yerel olarak Azure 'da oturum açmanız** gerektiği anlamına gelir.

Kod örneğini çalıştırmak için Visual Studio 'Yu veya Visual Studio Code kullanıyorsanız, Azure dijital TWINS örneğinize erişmek için kullanmak istediğiniz Azure kimlik bilgileriyle aynı düzenleyicide oturum açtığınızdan emin olun.

Aksi takdirde, [yerel **Azure CLI**'yı yükleyebilir](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true), makinenizde bir komut istemi başlatabilir ve `az login` Azure hesabınızda oturum açmak için komutunu çalıştırabilirsiniz. Bundan sonra, kod örneğinizi çalıştırdığınızda otomatik olarak oturumunuzu açmanız gerekir.