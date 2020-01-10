---
title: Azure Güvenlik denetimi-güvenli yapılandırma
description: Güvenlik denetimi güvenli yapılandırma
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 08458777d8a8c5c74ac20a63245135ffaf0a5c04
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564120"
---
# <a name="security-control-secure-configuration"></a>Güvenlik denetimi: güvenli yapılandırma

Saldırganların güvenlik açığı bulunan hizmetler ve ayarlarla yararlanmasını engellemek için Azure kaynaklarının güvenlik yapılandırmasını oluşturun, uygulayın ve etkin bir şekilde yönetin (izleyin, raporlayın).

## <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.1 | 5.1 | Müşteri |

Tüm Azure kaynakları için güvenlik yapılandırmalarının bakımını yapmak üzere Azure Ilkesi veya Azure Güvenlik Merkezi 'ni kullanın.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.2 | 5.1 | Müşteri |

Tüm işlem kaynaklarında güvenlik yapılandırmalarının bakımını yapmak için&quot; sanal makinelerinizdeki güvenlik yapılandırmalarında güvenlik açıklarını düzeltmek &quot;Azure Güvenlik Merkezi önerisi ' ni kullanın.

Azure Güvenlik Merkezi önerilerini izleme:

https://docs.microsoft.com/azure/security-center/security-center-recommendations

Azure Güvenlik Merkezi önerilerini Düzeltme:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.3 | 5.2 | Müşteri |

Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlama:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

## <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.4 | 5.2 | Paylaşılan |

Temel işletim sistemi görüntüleri Microsoft tarafından yönetilir ve sürdürülür.

Ancak, Azure Resource Manager şablonları ve/veya Istenen durum yapılandırması ' nı kullanarak kuruluşunuz için gereken güvenlik ayarlarını uygulayabilirsiniz.

Azure Resource Manager şablonundan Azure sanal makinesi oluşturma:

https://docs.microsoft.com/azure/virtual-machines/windows/ps-template

Azure sanal makineleri için Istenen durum yapılandırmasını anlayın:

https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-overview

## <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.5 | 5.3 | Müşteri |

Özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Azure DevOps veya Azure Repos kullanın.

Azure DevOps 'da kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

## <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,6 | 5.3 | Müşteri |

Özel görüntüler kullanıyorsanız, yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için RBAC kullanın. Kapsayıcı görüntüleri için Azure Container Registry depolayın ve yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için RBAC 'den yararlanın.

Azure 'da RBAC 'yi anlayın:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles

Container Registry için RBAC 'yi anlayın:

https://docs.microsoft.com/azure/container-registry/container-registry-roles

Azure 'da RBAC 'yi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

## <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,7 | 5.4 | Müşteri |

Azure Ilkesini kullanarak sistem yapılandırmasını uyarır, denetleyebilir ve zorunlu kılabilirsiniz. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

## <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.8 | 5.4 | Müşteri |

Linux için Windows işlem veya Linux Chef uzantısı için PowerShell Istenen durum yapılandırması gibi Azure işlem uzantıları kullanın.

Azure 'da sanal makine uzantıları 'nı yüklemek:

https://docs.microsoft.com/azure/virtual-machines/extensions/overview#how-can-i-install-an-extension

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,9 | 5.5 | Müşteri |

Azure Güvenlik Merkezi 'ni kullanarak Azure kaynaklarınız için temel taramalar gerçekleştirin

Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği:

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7.1 | 5.5 | Müşteri |

Kapsayıcılar için işletim sistemi ve Docker ayarlarına yönelik temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

Azure Güvenlik Merkezi kapsayıcısı önerilerini anlama:

https://docs.microsoft.com/azure/security-center/security-center-container-recommendations

## <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin 

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,11 | 13,1 | Müşteri |

Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın.

Azure yönetilen kimliklerle tümleştirme: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma: https://docs.microsoft.com/azure/key-vault/quick-create-portal

Yönetilen bir kimlikle Key Vault kimlik doğrulaması sağlama: https://docs.microsoft.com/azure/key-vault/managed-identity

## <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,12 | 4.1 | Müşteri |

Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Yönetilen kimlikleri yapılandırma: https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

## <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 7,13 | 13,3 | Müşteri |

Kod içinde kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

Kimlik bilgileri tarayıcısını ayarlama: https://secdevtools.azurewebsites.net/helpcredscan.html

## <a name="next-steps"></a>Sonraki adımlar

Sonraki güvenlik denetimine bakın: [kötü amaçlı yazılımdan koruma](security-control-malware-defense.md)
