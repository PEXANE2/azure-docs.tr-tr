---
title: Azure Güvenlik Denetimi - Güvenli Yapılandırma
description: Azure Güvenlik Denetimi Güvenli Yapılandırma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: bfa16f59155e420209ab6370056a7c612b5327e4
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759049"
---
# <a name="security-control-secure-configuration"></a>Güvenlik Denetimi: Güvenli Yapılandırma

Saldırganların güvenlik açığı olan hizmetlerden ve ayarlardan yararlanmasını önlemek için Azure kaynaklarının güvenlik yapılandırmasını kurun, uygulayın ve etkin bir şekilde yönetin (izleme, rapor etme, doğru)

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.1 | 5.1 | Müşteri |

Azure kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için Azure İlkesi takma adlarını kullanın. Yerleşik Azure İlkesi tanımlarını da kullanabilirsiniz.

Ayrıca, Azure Kaynak Yöneticisi, yapılandırmaların kuruluşunuz için güvenlik gereksinimlerini karşılamasını veya aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi'nde (JSON) şablonu dışa aktarma yeteneğine sahiptir.

Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak Azure Güvenlik Merkezi'nden gelen önerileri de kullanabilirsiniz.

- [Kullanılabilir Azure İlkesi takma adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Öğretici: Uyumluluğu uygulamak için ilkeler oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure portalındaki şablona tek ve çok kaynak lı dışa aktarma](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

- [Güvenlik önerileri - bir başvuru kılavuzu](https://docs.microsoft.com/azure/security-center/recommendations-reference)

## <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.2 | 5.1 | Müşteri |

Tüm işlem kaynaklarında güvenlik yapılandırmalarını korumak için Azure Güvenlik Merkezi önerilerini kullanın.  Ayrıca, kuruluşunuzun gerektirdiği işletim sisteminin güvenlik yapılandırmasını oluşturmak için özel işletim sistemi görüntülerini veya Azure Otomasyon Durumu yapılandırmasını kullanabilirsiniz.

- [Azure Güvenlik Merkezi önerileri nasıl izlenir?](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

- [Güvenlik önerileri - bir başvuru kılavuzu](https://docs.microsoft.com/azure/security-center/recommendations-reference)

- [Azure Otomasyon Durumu Yapılandırması Genel Bakış](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

- [Bir VHD yükleyin ve Azure'da yeni Windows VM'leri oluşturmak için kullanın](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed)

- [Azure CLI ile özel bir diskten Linux VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd)

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.3 | 5.2 | Müşteri |

Azure kaynaklarınız arasında güvenli ayarları uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.  Ayrıca, kuruluşunuzun gerektirdiği Azure kaynaklarının güvenlik yapılandırmasını korumak için Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz. 

- [Azure İlkesi efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Kaynak Yöneticisi şablonlarına genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.4 | 5.2 | Paylaşımlı |

Azure bilgisayar kaynaklarınızda güvenlik açığı değerlendirmeleri gerçekleştirme konusunda Azure Güvenlik Merkezi'nin önerilerine uyun.  Ayrıca, kuruluşunuzun gerektirdiği işletim sisteminin güvenlik yapılandırmasını korumak için Azure Kaynak Yöneticisi şablonlarını, özel işletim sistemi görüntülerini veya Azure Otomasyon Durumu yapılandırmasını kullanabilirsiniz.   Microsoft sanal makine şablonları, Azure Otomasyonu İstenen Durum Yapılandırması ile birlikte güvenlik gereksinimlerinin karşılanmasına ve korunmasına yardımcı olabilir. 

Ayrıca, Microsoft tarafından yayınlanan Azure Marketplace Sanal Makine Görsellerinin Microsoft tarafından yönetildiğini ve korunadığını unutmayın. 

- [Azure Güvenlik Merkezi güvenlik açığı değerlendirme önerileri nasıl uygulanır?](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

- [Azure Kaynak Yöneticisi şablonundan Azure Sanal Makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

- [Azure Otomasyon Durumu Yapılandırması Genel Bakış](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

- [Azure portalında Windows sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

- [VM şablonu nasıl indirilir hakkında bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/download-template)

- [Bir VHD’yi Azure’a yüklemek ve yeni bir sanal makine oluşturmak için örnek betik](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script)

## <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7,5 | 5.3 | Müşteri |

Özel Azure ilkeleri, Azure Kaynak Yöneticisi şablonları ve İstenilen Devlet Yapılandırma komut dosyaları gibi kodlarınızı güvenli bir şekilde depolamak ve yönetmek için Azure DevOps'leri kullanın. Azure DevOps'te yönettiğiniz kaynaklara erişmek için, Azure DevOps ile entegre edilmişse Azure Etkin Dizini'nde (Azure AD) tanımlanan belirli kullanıcılara, yerleşik güvenlik gruplarına veya gruplara izin verebilir veya reddedebilir veya TFS ile entegre edildiyseniz Active Directory'yi.

- [Azure DevOps'lerde kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps'teki izinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

## <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.6 | 5.3 | Müşteri |

Özel görüntüler kullanıyorsanız, resimlere yalnızca yetkili kullanıcıların erişebileceğinden emin olmak için rol tabanlı erişim denetimini (RBAC) kullanın. Paylaşılan Resim Galerisi'ni kullanarak resimlerinizi kuruluşunuzdaki farklı kullanıcılarla, hizmet müdürleriyle veya REKLAM gruplarına paylaşabilirsiniz.  Kapsayıcı görüntüler için bunları Azure Kapsayıcı Kayıt Defteri'nde saklayın ve görüntülere yalnızca yetkili kullanıcıların erişebilmesini sağlamak için RBAC'dan yararlanın.  

- [Azure'da RBAC'ı anlayın](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)

- [Konteyner Kayıt Defteri için RBAC'ı anlayın](https://docs.microsoft.com/azure/container-registry/container-registry-roles)

- [Azure'da RBAC nasıl yapılandırılmaz?](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

- [Paylaşılan Resim Galerisi'ne genel bakış](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)

## <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Azure kaynakları için yapılandırma yönetimi araçlarını dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.7 | 5,4 | Müşteri |

Azure İlkesi'ni kullanarak Azure kaynakları için standart güvenlik yapılandırmalarını tanımlayın ve uygulayın. Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya uygulamak için özel ilkeler oluşturmak için Azure İlkesi takma adlarını kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz.  Ayrıca, yapılandırma değişikliklerini dağıtmak için Azure Otomasyonu'ni kullanabilirsiniz.

- [Azure İlkesi nasıl yapılandırılatır ve yönetilir?](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Diğer Adları nasıl kullanılır?](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

## <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için yapılandırma yönetim araçlarını dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.8 | 5,4 | Müşteri |

Azure Otomasyon Durumu Yapılandırması, herhangi bir bulutveya şirket içi veri merkezinde İstenilen Durum Yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. Makinelere kolayca binebilir, bunları bildirimsel yapılandırmalar atayabilir ve her makinenin uyumluluğunu belirttiğiniz duruma göre görüntüleyebilirsiniz. 

- [Azure Otomasyon Durumu Yapılandırması ile yönetim için onboarding makineleri](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

## <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Azure kaynakları için otomatik yapılandırma izleme uygulama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.9 | 5.5 | Müşteri |

Azure Kaynaklarınız için temel taramaları gerçekleştirmek için Azure Güvenlik Merkezi'ni kullanın.  Ayrıca, Azure kaynak yapılandırmalarını uyarmak ve denetlemek için Azure İlkesi'ni kullanın.

- [Azure Güvenlik Merkezi'nde öneriler nasıl düzeltilir?](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.10 | 5.5 | Müşteri |

Kapsayıcılar için Işletim Sistemi ve Docker Ayarları için temel tarama yapmak için Azure Güvenlik Merkezi'ni kullanın.

- [Azure Güvenlik Merkezi'ndeki kapsayıcı önerilerini anlama](https://docs.microsoft.com/azure/security-center/security-center-container-recommendations)

## <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.11 | 13.1 | Müşteri |

Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın.

- [Azure Yönetilen Kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

- [Anahtar Kasası nasıl oluşturulur?](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

- [Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlar?](https://docs.microsoft.com/azure/key-vault/managed-identity)

## <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.12 | 4.1 | Müşteri |

Azure AD'de otomatik olarak yönetilen bir kimlikle Azure hizmetlerine sunmak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

- [Yönetilen Kimlikler nasıl yapılandırılır?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

## <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 7.13 | 18.1, 18.7 | Müşteri |

Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

- [Kimlik Bilgisi Tarayıcı nasıl kurulturur?](https://secdevtools.azurewebsites.net/helpcredscan.html)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Kontrolüne Bakın: [Kötü Amaçlı Yazılım Savunması](security-control-malware-defense.md)