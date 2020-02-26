---
title: Azure ile Chef kullanma
description: Azure altyapınızı yapılandırmak ve test etmek için Chef kullanmaya giriş
keywords: Azure, Chef, DevOps, sanal makineler, genel bakış, Otomatikleştir
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: c22faa69bf8f42111d328a9c156dc1a2432731b2
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586351"
---
# <a name="using-chef-with-azure"></a>Azure ile Chef kullanma
[Chef](https://www.chef.io) , Azure 'daki sanal makine altyapısını koda dönüştüren güçlü bir Otomasyon platformudur. Chef, altyapının, boyutu ne kadar bir şekilde yapılandırıldığını, dağıtıldığını ve yönetildiğini otomatik hale getirir.

Bu makalede, Azure altyapısını yönetmek için Chef kullanmanın avantajları açıklanmaktadır.

## <a name="chef-extension-on-azure"></a>Azure 'da Chef uzantısı
[Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) [Chef](https://docs.microsoft.com/azure/chef/chef-extension-portal) ile arka plan hizmeti olarak çalışan Chef istemcisiyle bir sanal makine sağlayın. Bu sanal makineler, sağlandıktan sonra bir Chef sunucusu tarafından yönetilmeye hazır hale gelir.

## <a name="chef-cloud-shell"></a>Chef Cloud Shell
Chef Workstation 'ı doğrudan Azure Cloud Shell ' de kullanın! Cloud Shell tüm Chef yardımcı programlarını ve InSpec hakkını çalıştırın. Chef komutlarının arasından yararlanabilirsiniz:

* [Chef](https://docs.chef.io/ctl_chef.html)
* [ğa](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [bı](https://docs.chef.io/knife.html)
* [Tanıtım stili](https://docs.chef.io/cookstyle.html)
* [Chef-Çalıştır](https://www.chef.sh/docs/chef-workstation/getting-started/)

`git`, `az-cli`ve `terraform`gibi Cloud Shell bulunan diğer araçlarla komut yardımcı programlarını birleştirin ve altyapı ve uyumluluk Otomasyonunuzu tarayıcıdan yazın.

## <a name="automate-infrastructure-apps-and-compliance-with-one-platform"></a>Altyapıyı, uygulamaları ve bir platformla uyumluluğu otomatikleştirin
Şirketler, dijital market 'te yarışmaya yönelik hız, hız ve güvenlik gerektirir. Chef ve Microsoft Yardım bireyleri, takımları ve kuruluşları bu işlemleri gerçekleştirebilir. Tek platformlu Chef sayesinde, artık altyapınızı, uygulamalarınızı ve uyumluluğunu Microsoft 'a göre otomatik hale getirebilir ve sürekli teslim edebilirsiniz.

## <a name="test-drive-chef-automate-on-azure"></a>Azure üzerinde sürücü Chef Otomasyonu test etme
Chef tarafından desteklenen [Chef, Azure Marketi çözümünü otomatikleştirin](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate) , altyapı ve uygulamalarınızı işbirliği yaparak oluşturmanıza, dağıtmanıza ve yönetmenize olanak sağlar. Tek tıklamada Chef otomatikleştirmesine dahil olan tüm ticari özelliklere anında erişebilirsiniz; Tüm Fleet genelinde uçtan uca görünürlük elde edin, sürekli uyumluluğu etkinleştirin ve Birleşik bir iş akışıyla tüm değişikliği yönetin.

## <a name="next-steps"></a>Sonraki adımlar

* [Chef kullanarak Azure 'da Windows sanal makinesi oluşturma](chef-automation.md)
