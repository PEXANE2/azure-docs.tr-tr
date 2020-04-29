---
title: 'CLı: bir uygulamayı yedekten geri yükleme'
description: Azure CLı kullanarak App Service uygulamanızın dağıtımını ve yönetimini otomatik hale getirmeyi öğrenin. Bu örnek, bir uygulamanın bir yedekten nasıl geri yükleneceğini gösterir.
author: msangapu-msft
tags: azure-service-management
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/07/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: mvc, seodec18
ms.openlocfilehash: 8c3c0f4f57d21e5c07867a2c43ce3c3114f79a9b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80058864"
---
# <a name="restore-a-web-app-from-a-backup-using-cli"></a>CLı kullanarak bir Web uygulamasını yedekten geri yükleme

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra bu web uygulaması için tek seferlik bir yedekleme oluşturur. 

Bu betiği çalıştırmak için bir web uygulamasının var olan bir yedeği gerekir. Bir yedek oluşturmak için bkz. [Bir web uygulamasını yedekleme](cli-backup-onetime.md) veya [Bir web uygulaması için zamanlanmış yedekleme oluşturma](cli-backup-scheduled.md).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-restore/backup-restore.sh?highlight=3-4,9 "Restore a web app from a backup")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-list) | Bir web uygulamasının yedekleme listesini alır. |
| [`az webapp config backup restore`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-restore) | Bir web uygulamasını yedekten geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
