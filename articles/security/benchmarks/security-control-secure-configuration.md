---
title: Azure Güvenlik Denetimi - Güvenli Yapılandırma
description: Güvenlik Denetimi Güvenli Yapılandırma
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 03564effeee36ddb3316d48329ccab8ccfce75b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934289"
---
# <a name="security-control-secure-configuration"></a>Güvenlik Denetimi: Güvenli Yapılandırma

Saldırganların güvenlik açığı olan hizmetlerden ve ayarlardan yararlanmasını önlemek için Azure kaynaklarının güvenlik yapılandırmasını kurun, uygulayın ve etkin bir şekilde yönetin (izleme, rapor etme, doğru)

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.1 | 5.1 | Müşteri |

Tüm Azure Kaynakları için güvenlik yapılandırmalarını korumak için Azure İlkesi veya Azure Güvenlik Merkezi'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.2 | 5.1 | Müşteri |

Tüm bilgi işlem &quot;kaynaklarındaki güvenlik yapılandırmalarını korumak&quot; için Sanal Makinelerinizdeki Güvenlik Yapılandırmalarında Güvenlik Açıklarını Düzeltme önerisi ni kullanın.

Azure Güvenlik Merkezi önerileri nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-recommendations

Azure Güvenlik Merkezi önerilerini nasıl düzeltiresınız:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.3 | 5.2 | Müşteri |

Azure kaynaklarınız arasında güvenli ayarları uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.4 | 5.2 | Paylaşımlı |

Temel işletim sistemi görüntüleri Microsoft tarafından yönetilir ve korunur.

Ancak, Azure Kaynak Yöneticisi şablonlarını ve/veya İstenen Durum Yapılandırması'nı kullanarak kuruluşunuzun gerektirdiği güvenlik ayarlarını uygulayabilirsiniz.

Azure Kaynak Yöneticisi şablonundan Azure Sanal Makinesi nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-machines/windows/ps-template

Azure Sanal Makineler için İstenen Durum Yapılandırması'nı anlayın:

https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-overview

## <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7,5 | 5.3 | Müşteri |

Özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure DevOps veya Azure Repos'unu kullanın.

Azure DevOps'lerde kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

## <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.6 | 5.3 | Müşteri |

Özel görüntüler kullanıyorsanız, görüntülere yalnızca yetkili kullanıcıların erişebileceğinden emin olmak için RBAC'ı kullanın. Kapsayıcı görüntüler için bunları Azure Kapsayıcı Kayıt Defteri'nde saklayın ve görüntülere yalnızca yetkili kullanıcıların erişebilmesini sağlamak için RBAC'dan yararlanın.

Azure'da RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles

Konteyner Kayıt Defteri için RBAC'ı anlayın:

https://docs.microsoft.com/azure/container-registry/container-registry-roles

Azure'da RBAC nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

## <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.7 | 5,4 | Müşteri |

Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için Azure İlkesi'ni kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.8 | 5,4 | Müşteri |

Windows compute için PowerShell İstenen Durum Yapılandırması veya Linux için Linux Chef Extension gibi Azure işlem uzantılarını kullanın.

Azure'da Sanal Makine Uzantıları nasıl yüklenir:

https://docs.microsoft.com/azure/virtual-machines/extensions/overview#how-can-i-install-an-extension

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.9 | 5.5 | Müşteri |

Azure Kaynaklarınız için temel taramaları gerçekleştirmek için Azure Güvenlik Merkezi'ni kullanın

Azure Güvenlik Merkezi'nde öneriler nasıl düzeltilir:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.1 | 5.5 | Müşteri |

Kapsayıcılar için Işletim Sistemi ve Docker Ayarları için temel tarama yapmak için Azure Güvenlik Merkezi'ni kullanın.

Azure Güvenlik Merkezi kapsayıcı önerilerini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-container-recommendations

## <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme 

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.11 | 13.1 | Müşteri |

Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:

https://docs.microsoft.com/azure/key-vault/managed-identity

## <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.12 | 4.1 | Müşteri |

Azure AD'de otomatik olarak yönetilen bir kimlikle Azure hizmetlerine sunmak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Yönetilen Kimlikler nasıl yapılandırılır:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

## <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.13 | 13.3 | Müşteri |

Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

Kimlik Bilgisi Tarayıcı nasıl kurulturur:

https://secdevtools.azurewebsites.net/helpcredscan.html

## <a name="next-steps"></a>Sonraki adımlar

Sonraki güvenlik denetimine bakın: [Kötü Amaçlı Yazılım Savunması](security-control-malware-defense.md)
