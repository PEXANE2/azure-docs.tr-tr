---
title: Azure Güvenlik denetimi-güvenli yapılandırma
description: Azure Güvenlik denetimi güvenli yapılandırma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: e3d43239dabe75bc2b25319945c2c6b08d726d2a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82193114"
---
# <a name="security-control-secure-configuration"></a>Güvenlik denetimi: güvenli yapılandırma

Saldırganların güvenlik açığı bulunan hizmetler ve ayarlarla yararlanmasını engellemek için Azure kaynaklarının güvenlik yapılandırmasını oluşturun, uygulayın ve etkin bir şekilde yönetin (izleyin, raporlayın).

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7.1 | 5.1 | Müşteri |

Azure kaynaklarınızın diğer adlarını denetlemek veya Azure kaynaklarınızın yapılandırmasını zorlamak için özel ilkeler oluşturmak üzere kullanın. Yerleşik Azure Ilke tanımlarını da kullanabilirsiniz.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Azure Güvenlik Merkezi 'nin önerilerini Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak da kullanabilirsiniz.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

- [Güvenlik önerileri-bir başvuru kılavuzu](https://docs.microsoft.com/azure/security-center/recommendations-reference)

## <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7.2 | 5.1 | Müşteri |

Tüm işlem kaynaklarında güvenlik yapılandırmalarının bakımını yapmak için Azure Güvenlik Merkezi önerilerini kullanın.  Ayrıca, kuruluşunuz tarafından istenen işletim sisteminin güvenlik yapılandırmasını oluşturmak için özel işletim sistemi görüntülerini veya Azure Automation durum yapılandırması ' nı kullanabilirsiniz.

- [Azure Güvenlik Merkezi önerilerini izleme](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

- [Güvenlik önerileri-bir başvuru kılavuzu](https://docs.microsoft.com/azure/security-center/recommendations-reference)

- [Azure Otomasyonu durum yapılandırmasına genel bakış](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

- [Bir VHD 'YI karşıya yükleyin ve Azure 'da yeni Windows VM 'Leri oluşturmak için kullanın](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed)

- [Azure CLı ile özel diskten bir Linux VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd)

## <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7.3 | 5,2 | Müşteri |

Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure Ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.  Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları kullanabilirsiniz. 

- [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Resource Manager şablonlarına genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

## <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7.4 | 5,2 | Shared |

Azure işlem kaynaklarınız üzerinde güvenlik açığı değerlendirmesi gerçekleştirerek Azure Güvenlik Merkezi önerilerini izleyin.  Ayrıca, kuruluşunuz tarafından istenen işletim sisteminin güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları, özel işletim sistemi görüntüleri veya Azure Otomasyonu durum yapılandırması ' nı kullanabilirsiniz.   Azure Otomasyonu Istenen durum yapılandırması ile birleştirilmiş Microsoft sanal makine şablonları, güvenlik gereksinimlerinin karşılanmasına ve bakımında yardımcı olabilir. 

Ayrıca, Microsoft tarafından yayımlanan Azure Marketi Sanal makine görüntülerinin Microsoft tarafından yönetildiğini ve bakımının yapılmadığını unutmayın. 

- [Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

- [Azure Resource Manager şablonundan Azure sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

- [Azure Otomasyonu durum yapılandırmasına genel bakış](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

- [Azure portal Windows sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

- [VM şablonunun nasıl indirileceği hakkında bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/download-template)

- [Bir VHD’yi Azure’a yüklemek ve yeni bir sanal makine oluşturmak için örnek betik](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script)

## <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,5 | 5.3 | Müşteri |

Kodunuzu özel Azure ilkeleri, Azure Resource Manager şablonları ve Istenen durum yapılandırması betikleri gibi güvenli bir şekilde depolamak ve yönetmek için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

## <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,6 | 5.3 | Müşteri |

Özel görüntüler kullanıyorsanız, görüntülere yalnızca yetkili kullanıcıların erişebildiğinden emin olmak için rol tabanlı erişim denetimi (RBAC) kullanın. Paylaşılan görüntü galerisini kullanarak, görüntülerinizi kuruluşunuzdaki farklı kullanıcılara, hizmet sorumlularına veya AD gruplarına paylaşabilirsiniz.  Kapsayıcı görüntüleri için Azure Container Registry depolayın ve yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için RBAC 'den yararlanın.  

- [Azure 'da RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)

- [Container Registry için RBAC 'yi anlayın](https://docs.microsoft.com/azure/container-registry/container-registry-roles)

- [Azure 'da RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

- [Paylaşılan görüntü galerisine genel bakış](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)

## <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,7 | 5,4 | Müşteri |

Azure Ilkesi 'ni kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz.  Ayrıca, Azure Otomasyonu ' nu yapılandırma değişikliklerini dağıtmak için de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Diğer adları kullanma](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

## <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,8 | 5,4 | Müşteri |

Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde Istenen durum yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. Makineleri kolayca ekleyebilir, bunlara bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen duruma göre uyumluluğunu gösteren raporları görüntüleyebilirsiniz. 

- [Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

## <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,9 | 5,5 | Müşteri |

Azure kaynaklarınızın temel taranmasını gerçekleştirmek için Azure Güvenlik Merkezi 'ni kullanın.  Ayrıca Azure Ilkesi 'ni kullanarak Azure Kaynak konfigürasyonları 'nı uyarır ve denetleyin.

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,10 | 5,5 | Müşteri |

Kapsayıcılar için işletim sistemi ve Docker ayarlarına yönelik temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

- [Azure Güvenlik Merkezi'ndeki kapsayıcı önerilerini anlama](https://docs.microsoft.com/azure/security-center/security-center-container-recommendations)

## <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,11 | 13,1 | Müşteri |

Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın.

- [Azure yönetilen kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

- [Key Vault oluşturma](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

- [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](https://docs.microsoft.com/azure/key-vault/managed-identity)

## <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,12 | 4.1 | Müşteri |

Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

- [Yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

## <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 7,13 | 18,1, 18,7 | Müşteri |

Kod içinde kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [kötü amaçlı yazılımdan koruma](security-control-malware-defense.md)