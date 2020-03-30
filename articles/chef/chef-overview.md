---
title: Azure ile Şef Kullanma
description: Azure altyapınızı yapılandırmak ve test etmek için Chef'i kullanmaya giriş
keywords: azure, şef, devops, sanal makineler, genel bakış, otomatikleştirmek
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: c22faa69bf8f42111d328a9c156dc1a2432731b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586351"
---
# <a name="using-chef-with-azure"></a>Azure ile Şef Kullanma
[Chef,](https://www.chef.io) Azure'daki sanal makine altyapılarını koda dönüştüren güçlü bir otomasyon platformudur. Şef, boyutu ne olursa olsun altyapının ağınızda nasıl yapılandırılabildiğini, dağıtılabildiğini ve yönetildiğini otomatikleştirir.

Bu makalede, Azure altyapısını yönetmek için Chef'i kullanmanın yararları açıklanmaktadır.

## <a name="chef-extension-on-azure"></a>Azure'da Şef Uzantısı
[Azure portalındaki](https://go.microsoft.com/fwlink/p/?LinkID=525040) [Chef Uzantısı](https://docs.microsoft.com/azure/chef/chef-extension-portal) ile arka plan hizmeti olarak çalışan Chef Client ile sanal bir makine sağlama. Bir kez sağlanan, bu sanal makineler bir Chef sunucusu tarafından yönetilmeye hazırdır.

## <a name="chef-cloud-shell"></a>Şef Bulut Kabuk
Doğrudan Azure Bulut Bulutu'nda Chef Workstation'ı kullanın! Tüm Chef yardımcı programlarınızı ve InSpec'inizi Cloud Shell'den çalıştırın. Şef komutlarını aşağıdakilerden yararlanabilirsiniz:

* [Şef](https://docs.chef.io/ctl_chef.html)
* [Mutfak](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [Bıçak](https://docs.chef.io/knife.html)
* [yemek stili](https://docs.chef.io/cookstyle.html)
* [şef çalıştır](https://www.chef.sh/docs/chef-workstation/getting-started/)

Komut yardımcı programlarımızı Cloud Shell'de bulunan , `git` `az-cli`ve `terraform`, gibi diğer araçlarla birleştirin ve altyapınızı ve uyumluluk otomasyonunuzu tarayıcıdan yazın.

## <a name="automate-infrastructure-apps-and-compliance-with-one-platform"></a>Altyapıyı, uygulamaları ve uyumluluğu tek bir platformla otomatikleştirin
Şirketler dijital pazarda rekabet etmek için hız, hız ve güvenlik gerektirir. Chef ve Microsoft birlikte bireylerin, ekiplerin ve işletmelerin tüm bunları gerçekleştirmesine yardımcı olur. Chef Automate tek bir platformla artık altyapınızı, uygulamalarınızı ve uyumluluğunuzu Microsoft mülkünher yerinde otomatikleştirebilir ve sürekli olarak teslim edebilirsiniz.

## <a name="test-drive-chef-automate-on-azure"></a>Azure'da Test sürüşü Şef Otomatikleştir
Chef Chef tarafından desteklenen [Chef Automate Azure Market çözümü,](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate) altyapınızı ve uygulamalarınızı işbirliği içinde oluşturmanıza, dağıtmanıza ve yönetmenize olanak tanır. Tek bir tıklama, Chef Automate ile birlikte tüm ticari özelliklere anında erişmenizi sağlar; tüm filonuzda uça görünürlük elde edin, sürekli uyumluluk sağlayabilir ve tüm değişimi birleşik bir iş akışıyla yönetin.

## <a name="next-steps"></a>Sonraki adımlar

* [Chef'i kullanarak Azure'da Windows sanal makinesi oluşturma](chef-automation.md)
