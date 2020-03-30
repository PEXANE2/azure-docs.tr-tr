---
title: Terraform'u Azure ile birlikte kullanma
description: Azure altyapısını sürüm ve dağıtma için Terraform'u kullanmaya giriş.
ms.topic: overview
ms.date: 10/26/2019
ms.openlocfilehash: 05b92fdf8c0a0f84d2f29b4aa7479850b2721441
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77472171"
---
# <a name="terraform-with-azure"></a>Terraform ve Azure

[Hashicorp Terraform](https://www.terraform.io/), bulut altyapısı sağlamak ve yönetmek için kullanılan bir açık kaynak araçtır. Bulut kaynaklarının topolojisini açıklayan yapılandırma dosyalarındaki altyapıyı kodlar. Bu kaynaklar sanal makineleri, depolama hesaplarını ve ağ arabirimlerini içerir. Terraform CLI, yapılandırma dosyalarını Azure'a dağıtmak ve sürümlemek için basit bir mekanizma sağlar.

Bu makalede Azure altyapısını yönetmek için Terraform'u kullanmanın avantajları anlatılmaktadır.

## <a name="automate-infrastructure-management"></a>Altyapı yönetimini otomatikleştirin.

Terraform'un şablon tabanlı yapılandırma dosyaları Azure kaynaklarını yinelenebilir ve tahmin edilebilir bir şekilde tanımlamanızı, sağlamanızı ve yapılandırmanızı sağlar. Altyapıyı otomatikleştirmek birçok avantaja sahiptir:

- Altyapıyı dağıtma ve yönetme aşamalarındaki insan hatası ihtimalini azaltır.
- Birbirinin aynı geliştirme, test ve üretim ortamları oluşturmak için aynı şablonu birçok kez dağıtabilir.
- Geliştirme ve test ortamlarını istek üzerine oluşturarak geliştirme maliyetini azaltır.

## <a name="understand-infrastructure-changes-before-being-applied"></a>Uygulanmadan önce altyapı değişikliklerini anlama

Kaynak topolojisi araçları daha karmaşık hale geldikçe altyapıda gerçekleştirilen değişikliklerin anlamının ve etkisinin anlaşılması zor olabilir.

Terraform CLI, kullanıcıların uygulamadan önce altyapı değişikliklerini doğrulamalarını ve önizlemelerini sağlar. Altyapı değişikliklerini güvenli bir şekilde önizlemenin birkaç faydası vardır:
- Ekip üyeleri önerilen değişiklikleri ve etkisini daha hızlı anlayarak daha verimli bir şekilde çalışabilir.
- İstenmeden yapılan değişiklikler geliştirme sürecinin erken dönemlerinde fark edilebilir

## <a name="deploy-infrastructure-to-multiple-clouds"></a>Altyapı birden fazla buluta dağıtılabilir

Terraform, birden çok bulut sağlayıcısı arasında bir altyapı dağıtma da ustadır. Geliştiricilerin her altyapı tanımını yönetmek için tutarlı araç kullanmalarına olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Terraform ve avantajlarına genel bir bakış elde ettiğinize göre, aşağıdaki önerilen adımlara geçebilirsiniz:

- [Terraform'u yükleyip Azure'u kullanacak şekilde yapılandırarak](terraform-install-configure.md) başlayın.
- [Terraform'u kullanarak bir Azure sanal makinesi oluşturma](terraform-create-complete-vm.md)
- [Terraform için Azure Resource Manager modülünü](https://www.terraform.io/docs/providers/azurerm/) keşfedin 