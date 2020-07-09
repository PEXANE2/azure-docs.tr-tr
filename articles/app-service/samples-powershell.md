---
title: PowerShell örnekleri
description: Bazı yaygın App Service senaryolarından bazıları için Azure PowerShell örnekler bulun. App Service dağıtımınızı veya yönetim görevlerinizi otomatikleştirmeyi öğrenin.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 07/07/2020
ms.custom: mvc
ms.openlocfilehash: a1577d42de9a4452467a448a0de5cd5f9575a55f
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169434"
---
# <a name="powershell-samples-for-azure-app-service"></a>Azure App Service için PowerShell örnekleri

Aşağıdaki tablo, Azure PowerShell kullanılarak oluşturulan PowerShell betiklerinin bağlantılarını içerir.

| Komut Dosyası | Açıklama |
|-|-|
|**Uygulama oluşturma**||
| [GitHub 'dan dağıtım ile uygulama oluşturma](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub 'dan kod çeken bir App Service uygulaması oluşturur. |
| [GitHub 'dan sürekli dağıtım ile uygulama oluşturma](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub 'dan sürekli olarak kod dağıtan bir App Service uygulaması oluşturur. |
| [FTP ile uygulama oluşturma ve kod dağıtma](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve FTP kullanarak yerel bir dizinden dosya yükler. |
| [Yerel bir git deposundan uygulama oluşturma ve kod dağıtma](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve yerel bir git deposundan kod gönderimi yapılandırır. |
| [Bir uygulama oluşturma ve hazırlama ortamına kod dağıtma](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hazırlama kodu değişiklikleri için dağıtım yuvası olan bir App Service uygulaması oluşturur. |
|  [Bir uygulama oluşturun ve uygulamanızı özel bir uç noktayla sunun](./scripts/powershell-deploy-private-endpoint.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Özel bir uç nokta ile App Service uygulaması oluşturur. |
|**Uygulama yapılandırma**||
| [Özel bir etki alanını uygulamayla eşleme](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması oluşturur ve buna özel bir etki alanı adı eşleştirir. |
| [Uygulamaya özel bir TLS/SSL sertifikası bağlama](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması oluşturur ve özel bir etki alanı adının TLS/SSL sertifikasını buna bağlar. |
|**Uygulama ölçeklendirme**||
| [Uygulamayı el ile ölçeklendirme](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve 2 örnek boyunca ölçeklendirir. |
| [Yüksek kullanılabilirlik mimarisi ile bir uygulamayı dünya genelinde ölçeklendirin](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | İki farklı coğrafi bölgede iki App Service uygulama oluşturur ve Azure Traffic Manager kullanarak tek bir uç nokta üzerinden kullanılabilir hale getirir. |
|**Uygulamayı kaynaklara bağlama**||
| [Bir uygulamayı SQL veritabanına bağlama](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure SQL veritabanı 'nda bir App Service uygulaması ve veritabanı oluşturur, ardından veritabanı bağlantı dizesini uygulama ayarlarına ekler. |
| [Bir uygulamayı depolama hesabına bağlama](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir App Service uygulaması ve depolama hesabı oluşturur, ardından depolama bağlantı dizesini uygulama ayarlarına ekler. |
|**Uygulamayı yedekleme ve geri yükleme**||
| [Uygulama yedekleme](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve bu uygulama için bir kerelik yedekleme oluşturur. |
| [Bir uygulama için zamanlanmış yedekleme oluşturma](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir App Service uygulaması oluşturur ve bu uygulama için zamanlanmış bir yedekleme oluşturur. |
| [Uygulama için bir yedeklemeyi silme](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulama için var olan bir yedeği siler. |
| [Bir uygulamayı yedekten geri yükleme](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulamayı daha önce tamamlanmış bir yedekten geri yükler. |
| [Bir yedeği abonelikler arasında geri yükleme](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Web uygulamasını başka bir abonelikteki yedekten geri yükler. |
|**Uygulama izleme**||
| [Web sunucusu günlükleri ile bir uygulamayı izleme](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | App Service bir uygulama oluşturur, günlüğe kaydetmeyi sağlar ve günlükleri yerel makinenize indirir. |
| | |
