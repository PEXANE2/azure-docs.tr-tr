---
author: baanders
description: Azure dijital TWINS örnekleri-giriş olmadan DefaultAzureCredential için yerel kimlik doğrulamasını ayarlamaya yönelik dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 3cb053f9673532ac19e2098678ec341f2f676486
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011267"
---
İle `DefaultAzureCredential` , örnek, yerel ortamınızda, yerel bir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) veya Visual Studio ya da Visual Studio Code bir Azure oturum açma gibi kimlik bilgilerini arar. Bu nedenle, örnek için kimlik bilgilerini ayarlamak üzere bu mekanizmalardan biri aracılığıyla *yerel olarak Azure 'da oturum açmanız* gerekir.

Kod örneğini çalıştırmak için Visual Studio 'Yu veya Visual Studio Code kullanıyorsanız, Bu düzenleyicide Azure dijital TWINS örneğinize erişmek için kullanmak istediğiniz Azure kimlik bilgileriyle oturum açtığınızdan emin olun.

Aksi takdirde, [Yerel Azure CLI 'yı yükleyebilir](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true), makinenizde bir komut istemi başlatabilir ve `az login` Azure hesabınızda oturum açmak için komutunu çalıştırabilirsiniz. Oturum açtıktan sonra, kod örneğinizi çalıştırdığınızda otomatik olarak oturumunuzu açmanız gerekir.