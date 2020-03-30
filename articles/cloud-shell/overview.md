---
title: Azure Bulut BulutU'na genel bakış | Microsoft Dokümanlar
description: Azure Bulut Kabuğuna Genel Bakış.
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/03/2019
ms.author: damaerte
ms.openlocfilehash: 513c3da8031774f5f111ee357b5a3c43e1d09d95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75832503"
---
# <a name="overview-of-azure-cloud-shell"></a>Azure Cloud Shell'e Genel Bakış
Azure Cloud Shell, Azure kaynaklarını yönetmek için kullanabileceğiniz etkileşimli, kimliği doğrulanmış ve tarayıcı ile erişilebilen bir kabuktur.
Bu hizmet, Bash veya PowerShell ile çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği getirir.

Aşağıdaki tıklayarak shell.azure.com deneyin.

[![Gömme başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

Bulut Kabuğu simgesini kullanarak Azure portalından deneyin.

![Portal lansmanı](media/overview/portal-launch-icon.png)

## <a name="features"></a>Özellikler

### <a name="browser-based-shell-experience"></a>Tarayıcı tabanlı kabuk deneyimi
Cloud Shell, Azure yönetim görevleri göz önünde bulundurularak oluşturulmuş tarayıcı tabanlı komut satırı deneyimine erişim sağlar.
Yerel bir makineden yalnızca bulutun sağlayabileceği şekilde tek tek çalışmak için Bulut Shell'den yararlanın.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen kabuk deneyimi seçimi
Kullanıcılar Bash veya PowerShell arasında seçim yapabilir.
1. **Bulut Kabuğu'nu**seçin.

    ![Bulut Kabuğu simgesi](media/overview/overview-cloudshell-icon.png)

2. **Bash** veya **PowerShell'i**seçin.

    ![Bash veya PowerShell'i seçin](media/overview/overview-choices.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Kimlik doğrulaması ve yapılandırılan Azure iş istasyonu
Cloud Shell, Microsoft tarafından yönetildiği için popüler komut satırı araçları ve dil desteği yle birlikte gelir. Cloud Shell ayrıca Azure CLI veya Azure PowerShell cmdlets aracılığıyla kaynaklarınıza anında erişim için otomatik olarak güvenli bir şekilde kimlik doğrulaması sağlar.

[Cloud Shell'de yüklü araçların](features.md#tools) tam listesini görüntüleyin.

### <a name="integrated-cloud-shell-editor"></a>Entegre Cloud Shell editörü
Cloud Shell, açık kaynaklı Monako Editörü'ne dayalı entegre bir grafik metin editörü sunar. Azure CLI veya Azure `code .` PowerShell üzerinden sorunsuz dağıtım için çalıştırarak yapılandırma dosyalarını oluşturmanız ve düzenlemenizi zedelenin.

[Cloud Shell düzenleyicisi hakkında daha fazla bilgi edinin.](using-cloud-shell-editor.md)

### <a name="integrated-with-docsmicrosoftcom"></a>docs.microsoft.com ile entegre

Cloud Shell'i [doğrudan docs.microsoft.com](https://docs.microsoft.com)barındırılan belgelerden kullanabilirsiniz. [Microsoft Learn,](https://docs.microsoft.com/learn/)Azure [PowerShell](https://docs.microsoft.com/powershell/azure/overview) ve [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure) entegre edilmiştir - sürükleyici kabuk deneyimini açmak için kod snippet'teki "Try It" düğmesine tıklayın. 

### <a name="multiple-access-points"></a>Birden çok erişim noktası
Cloud Shell aşağıdaki lerden kullanılabilecek esnek bir araçtır:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure)
* [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview)
* [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Visual Studio Code Azure Hesap uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure Dosyaları depolamanızı bağlama
Bulut Kabuğu makineleri geçicidir, ancak dosyalarınız iki şekilde devam eder: bir disk görüntüsü `clouddrive`ve 'adlı bir dosya paylaşımı aracılığıyla.  Cloud Shell ilk kez çalıştırıldığında sizin adınıza bir kaynak grubu, depolama hesabı ve Azure Dosyaları paylaşımı oluşturur. Bu tek seferlik bir adımdır ve tüm oturumlar için otomatik olarak eklenecektir. Tek bir dosya paylaşımı eşlenebilir ve Cloud Shell'de Hem Bash hem de PowerShell tarafından kullanılacaktır.

[Yeni veya varolan](persisting-shell-storage.md) bir depolama hesabını nasıl monte edineceklerini veya [Cloud Shell'de kullanılan kalıcılık mekanizmaları](persisting-shell-storage.md#how-cloud-shell-storage-works)hakkında bilgi edinmek için daha fazla bilgi edinin.

> [!NOTE]
> Bulut kabuğu depolama hesapları için Azure depolama güvenlik duvarı desteklenmez.

## <a name="concepts"></a>Kavramlar
* Cloud Shell, kullanıcı başına oturum başına sağlanan geçici bir ana bilgisayarda çalışır
* Cloud Shell, etkileşimli etkinlik olmadan 20 dakika sonra zamanları sildi
* Cloud Shell'in monte edilmesi için Azure dosya paylaşımı gerekir
* Cloud Shell, hem Bash hem de PowerShell için aynı Azure dosya paylaşımını kullanır
* Cloud Shell kullanıcı hesabı başına bir makine atanır
* Cloud Shell, dosya paylaşımınızda tutulan 5 GB'lık bir görüntü kullanarak $HOME devam eder
* İzinler Bash'te normal bir Linux kullanıcısı olarak ayarlanır

[Cloud Shell'de Bash](features.md) ve [Cloud Shell'deki PowerShell'deki](features-powershell.md)özellikler hakkında daha fazla bilgi edinin.

## <a name="pricing"></a>Fiyatlandırma
Cloud Shell'i barındıran makine, monte edilmiş Azure Dosyaları paylaşımının ön koşuluyla ücretsizdir. Düzenli depolama maliyetleri uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuğunda Bash hızlı başlat](quickstart.md) <br>
[PowerShell Bulut Shell hızlı başlat](quickstart-powershell.md)
