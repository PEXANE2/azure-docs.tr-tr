---
title: PowerShell örnekleri
description: Bazı yaygın App Service senaryolarından bazıları için Azure PowerShell örnekler bulun. App Service dağıtımınızı veya yönetim görevlerinizi otomatikleştirmeyi öğrenin.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 03/08/2017
ms.custom: mvc
ms.openlocfilehash: f32db2a4dda0ceb69644b64a5c76155b448c5a28
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687020"
---
# <a name="powershell-samples-for-azure-app-service"></a>Azure App Service için PowerShell örnekleri

Aşağıdaki tablo, Azure PowerShell kullanılarak oluşturulan PowerShell betiklerinin bağlantılarını içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [GitHub 'dan dağıtım ile uygulama oluşturma](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub 'dan kod çeken bir App Service uygulaması oluşturur. |
| [GitHub 'dan sürekli dağıtım ile uygulama oluşturma](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub 'dan sürekli olarak kod dağıtan bir App Service uygulaması oluşturur. |
| [FTP ile uygulama oluşturma ve kod dağıtma](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve FTP kullanarak yerel bir dizinden dosya yükler. |
| [Yerel bir git deposundan uygulama oluşturma ve kod dağıtma](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve yerel bir git deposundan kod gönderimi yapılandırır. |
| [Bir uygulama oluşturma ve hazırlama ortamına kod dağıtma](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hazırlama kodu değişiklikleri için dağıtım yuvası olan bir App Service uygulaması oluşturur. |
|**Uygulama yapılandırma**||
| [Özel bir etki alanını uygulamayla eşleme](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması oluşturur ve buna özel bir etki alanı adı eşleştirir. |
| [Özel bir SSL sertifikasını uygulamaya bağlama](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması oluşturur ve bir özel etki alanı adının SSL sertifikasını buna bağlar. |
|**Uygulamayı ölçeklendirin**||
| [Uygulamayı el ile ölçeklendirme](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve 2 örnek boyunca ölçeklendirir. |
| [Yüksek kullanılabilirlik mimarisi ile bir uygulamayı dünya genelinde ölçeklendirin](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | İki farklı coğrafi bölgede iki App Service uygulama oluşturur ve Azure Traffic Manager kullanarak tek bir uç nokta üzerinden kullanılabilir hale getirir. |
|**Uygulamayı kaynaklara bağlama**||
| [Bir uygulamayı SQL veritabanına bağlama](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması ve bir SQL veritabanı oluşturur, ardından veritabanı bağlantı dizesini uygulama ayarlarına ekler. |
| [Bir uygulamayı depolama hesabına bağlama](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması ve depolama hesabı oluşturur, ardından depolama bağlantı dizesini uygulama ayarlarına ekler. |
|**Uygulamayı yedekleme ve geri yükleme**||
| [Bir uygulamayı yedekleme](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve bu uygulama için bir kerelik yedekleme oluşturur. |
| [Bir uygulama için zamanlanmış yedekleme oluşturma](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve bu uygulama için zamanlanmış bir yedekleme oluşturur. |
| [Uygulama için bir yedeklemeyi silme](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulama için var olan bir yedeği siler. |
| [Bir uygulamayı yedekten geri yükleme](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulamayı daha önce tamamlanmış bir yedekten geri yükler. |
| [Bir yedeği abonelikler arasında geri yükleme](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Web uygulamasını başka bir abonelikteki yedekten geri yükler. |
|**Uygulamayı izleme**||
| [Web sunucusu günlükleri ile bir uygulamayı izleme](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | App Service bir uygulama oluşturur, günlüğe kaydetmeyi sağlar ve günlükleri yerel makinenize indirir. |
| | |
