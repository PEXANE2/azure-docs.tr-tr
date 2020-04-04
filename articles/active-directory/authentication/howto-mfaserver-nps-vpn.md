---
title: Azure MFA Server ve üçüncü taraf VPN'leri - Azure Etkin Dizin
description: Azure MFA Server'ın Cisco, Citrix ve Juniper ile tümleştirmesi için adım adım yapılandırma kılavuzları.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01decb99a9eb24ae60250f83f1f961b4c1690bc0
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652855"
---
# <a name="advanced-scenarios-with-azure-mfa-server-and-third-party-vpn-solutions"></a>Azure MFA Server ve üçüncü taraf VPN çözümleri yle gelişmiş senaryolar

Azure Çok Faktörlü Kimlik Doğrulama Sunucusu (Azure MFA Server), çeşitli üçüncü taraf VPN çözümleriyle sorunsuz bir şekilde bağlantı kurmak için kullanılabilir. Bu makale, Cisco&reg; ASA VPN cihazı, Citrix NetScaler SSL VPN cihazı ve Ardıç Ağları Güvenli Erişim/Pulse Secure Connect Secure SSL VPN cihazına odaklanır. Bu üç ortak cihaza yönelik yapılandırma kılavuzları oluşturduk. Azure MFA Server, AD FS'de RADIUS, LDAP, IIS veya talep tabanlı kimlik doğrulaması kullanan diğer sistemlerin çoğuyla da tümleşebilir. [Azure MFA Server yapılandırmalarında](howto-mfaserver-deploy.md#next-steps)daha fazla ayrıntı bulabilirsiniz.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

## <a name="cisco-asa-vpn-appliance-and-azure-mfa-server"></a>Cisco ASA VPN cihazı ve Azure MFA Server
Azure MFA Server, Cisco&reg; AnyConnect&reg; VPN girişleri ve portal erişimi için ek güvenlik sağlamak için Cisco ASA VPN cihazınızla entegre dir.  LDAP veya RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Anyconnect VPN ve Azure MFA Yapılandırması ile Cisco ASA](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | LDAP'yi kullanarak Cisco ASA VPN cihazınızı Azure MFA ile tümleştirin |
| [RADIUS için Anyconnect VPN ve Azure MFA Yapılandırması ile Cisco ASA](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Cisco ASA VPN cihazınızı RADIUS kullanarak Azure MFA ile tümleştirin |

## <a name="citrix-netscaler-ssl-vpn-and-azure-mfa-server"></a>Citrix NetScaler SSL VPN ve Azure MFA Server
Azure MFA Server, Citrix NetScaler SSL VPN girişleri ve portal erişimi için ek güvenlik sağlamak için Citrix NetScaler SSL VPN cihazınızla bütünleşir.  LDAP veya RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Citrix NetScaler SSL VPN ve Azure MFA Yapılandırması](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | LDAP'yi kullanarak Citrix NetScaler SSL VPN'inizi Azure MFA cihazıyla bütünleştirin |
| [RADIUS için Citrix NetScaler SSL VPN ve Azure MFA Yapılandırması](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Radius'u kullanarak Citrix NetScaler SSL VPN cihazınızı Azure MFA ile tümleştirin |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-mfa-server"></a>Ardıç/Pulse Secure SSL VPN cihazı ve Azure MFA Server
Azure MFA Server, Ardıç/Pulse Secure SSL VPN cihazınızla entegre edin ve Ardıç/Pulse Secure SSL VPN girişleri ve portal erişimi için ek güvenlik sağlar.  LDAP veya RADIUS protokolünü kullanabilirsiniz.  Ayrıntılı adım adım yapılandırma kılavuzlarını indirmek için aşağıdakilerden birini seçin.

| Yapılandırma Kılavuzu | Açıklama |
| --- | --- |
| [LDAP için Ardıç/Darbe Güvenli SSL VPN ve Azure MFA Yapılandırması](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Ardıç/Pulse Secure SSL VPN'inizi LDAP kullanarak Azure MFA cihazıyla tümleştirin |
| [RADIUS için Ardıç/Darbe Güvenli SSL VPN ve Azure MFA Yapılandırması](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Ardıç/Pulse Secure SSL VPN cihazınızı RADIUS kullanarak Azure MFA ile tümleştirin |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Çok Faktörlü Kimlik Doğrulama için NPS uzantısı ile mevcut kimlik doğrulama altyapınızı artırın](howto-mfa-nps-extension.md)

- [Azure Çok Faktörlü Kimlik Doğrulama ayarlarını yapılandırma](howto-mfa-mfasettings.md)
