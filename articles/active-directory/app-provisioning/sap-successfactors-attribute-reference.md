---
title: SAP SuccessFactors Öznitelik Referans | Microsoft Dokümanlar
description: SuccessFactors'in hangi özelliklerinin SuccessFactors-HR odaklı sağlama ile desteklenildiğini öğrenin
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: afb77f2d-5ddd-4c2e-a840-09021b0efef1
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: 00b16f969525e7b802c008ba247ecba015875689
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522365"
---
# <a name="sap-successfactors-attribute-reference"></a>SAP SuccessFactors Öznitelik Referans

## <a name="supported-successfactors-entities-and-attributes"></a>Desteklenen SuccessFactors Varlıklar ve Nitelikleri

Aşağıdaki tablo, aşağıdaki iki sağlama uygulaması tarafından desteklenen SuccessFactors özniteliklerilistesini yakalar: 
* [Active Directory Kullanıcı Sağlama Için SuccessFactors](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md)
* [Azure AD Kullanıcı Sağlama Için Başarı Faktörleri](../saas-apps/sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) 

| \# | SuccessFactors Varlık                  | SuccessFactors Öznitelik     | İşlem Türü |
|----|----------------------------------------|------------------------------|----------------|
| 1  | Kişi Başına                              | personIdExternal             | Okuma           |
| 2  | Kişi Başına                              | Personıd                     | Okuma           |
| 3  | Kişi Başına                              | perPersonUuid                | Okuma           |
| 4  | Kişiye Göre                            | displayName                  | Okuma           |
| 5  | Kişiye Göre                            | firstName                    | Okuma           |
| 6  | Kişiye Göre                            | cinsiyet                       | Okuma           |
| 7  | Kişiye Göre                            | lastName                     | Okuma           |
| 8  | Kişiye Göre                            | Middlename                   | Okuma           |
| 9  | Kişiye Göre                            | preferredName                | Okuma           |
| 10 | Kullanıcı                                   | adresLine1                 | Okuma           |
| 11 | Kullanıcı                                   | Addressline2                 | Okuma           |
| 12 | Kullanıcı                                   | adresiLIne3                 | Okuma           |
| 13 | Kullanıcı                                   | businessPhone                | Okuma           |
| 14 | Kullanıcı                                   | Cellphone                    | Okuma           |
| 15 | Kullanıcı                                   | city                         | Okuma           |
| 16 | Kullanıcı                                   | ülke                      | Okuma           |
| 17 | Kullanıcı                                   | özel01                     | Okuma           |
| 18 | Kullanıcı                                   | özel02                     | Okuma           |
| 19 | Kullanıcı                                   | özel03                     | Okuma           |
| 20 | Kullanıcı                                   | özel04                     | Okuma           |
| 21 | Kullanıcı                                   | özel05                     | Okuma           |
| 22 | Kullanıcı                                   | özel06                     | Okuma           |
| 23 | Kullanıcı                                   | özel07                     | Okuma           |
| 24 | Kullanıcı                                   | özel08                     | Okuma           |
| 25 | Kullanıcı                                   | özel09                     | Okuma           |
| 26 | Kullanıcı                                   | özel10                     | Okuma           |
| 27 | Kullanıcı                                   | özel11                     | Okuma           |
| 28 | Kullanıcı                                   | özel12                     | Okuma           |
| 29 | Kullanıcı                                   | özel13                     | Okuma           |
| 30 | Kullanıcı                                   | özel14                     | Okuma           |
| 31 | Kullanıcı                                   | Empıd                        | Okuma           |
| 32 | Kullanıcı                                   | anasayfaTelefon                    | Okuma           |
| 33 | Kullanıcı                                   | jobFamily                    | Okuma           |
| 34 | Kullanıcı                                   | Takma                     | Okuma           |
| 35 | Kullanıcı                                   | durum                        | Okuma           |
| 36 | Kullanıcı                                   | timeZone                     | Okuma           |
| 37 | Kullanıcı                                   | kullanıcı adı                     | Okuma           |
| 38 | Kullanıcı                                   | Zipcode                      | Okuma           |
| 39 | PerPhone                               | areaCode                     | Okuma           |
| 40 | PerPhone                               | ülkeKodu                  | Okuma           |
| 41 | PerPhone                               | uzantı                    | Okuma           |
| 42 | PerPhone                               | Phonenumber                  | Okuma           |
| 43 | PerPhone                               | phoneType                    | Okuma           |
| 44 | PerEmail                               | Emailaddress                 | Oku, Yaz    |
| 45 | PerEmail                               | emailType                    | Okuma           |
| 46 | EmpEmployment                          | ilkTarihÇalıştı              | Okuma           |
| 47 | EmpEmployment                          | lastDateWorked               | Okuma           |
| 48 | EmpEmployment                          | userId                       | Okuma           |
| 49 | EmpEmployment                          | isContingentWorker           | Okuma           |
| 50 | EmpJob                                 | countryOfCompany             | Okuma           |
| 51 | EmpJob                                 | emplDurum                   | Okuma           |
| 52 | EmpJob                                 | Bitiştarihi                      | Okuma           |
| 53 | EmpJob                                 | startDate                    | Okuma           |
| 54 | EmpJob                                 | jobTitle                     | Okuma           |
| 55 | EmpJob                                 | konum                     | Okuma           |
| 65 | EmpJob                                 | customString13               | Okuma           |
| 56 | EmpJob                                 | Managerıd                    | Okuma           |
| 57 | EmpJob\.İş Birimi                   | businessUnit                 | Okuma           |
| 58 | EmpJob\.İş Birimi                   | businessUnitId               | Okuma           |
| 59 | EmpJob\.Şirketi                        | şirket                      | Okuma           |
| 60 | EmpJob\.Şirketi                        | companyId                    | Okuma           |
| 61 | Empjob\.\.Şirket CountryOfregistration | twoCharCountryCode           | Okuma           |
| 62 | EmpJob\.CostCenter                     | costCenter                   | Okuma           |
| 63 | EmpJob\.CostCenter                     | maliyetCenterId                 | Okuma           |
| 64 | EmpJob\.CostCenter                     | maliyetCenterDescription        | Okuma           |
| 65 | EmpJob\.Bölümü                     | bölüm                   | Okuma           |
| 66 | EmpJob\.Bölümü                     | departmentId                 | Okuma           |
| 67 | EmpJob\.Bölümü                       | Bölümü                     | Okuma           |
| 68 | EmpJob\.Bölümü                       | tümenId                   | Okuma           |
| 69 | EmpJob\.İş Kodu                        | işKodu                      | Okuma           |
| 70 | EmpJob\.İş Kodu                        | işCodeId                    | Okuma           |
| 71 | EmpJob\.Konumu                       | Yer Adı                 | Okuma           |
| 72 | EmpJob\.Konumu                       | officeLocationAddress        | Okuma           |
| 73 | EmpJob\.Konumu                       | officeLocationCity           | Okuma           |
| 74 | EmpJob\.Konumu                       | ofisLocationCustomString4  | Okuma           |
| 75 | EmpJob\.Konumu                       | ofisLocationZipCode        | Okuma           |
| 76 | EmpJob\.PayGrade                       | payGrade                     | Okuma           |
| 77 | AmpemploymentTermination               | activeEmploymentsCount       | Okuma           |
| 78 | AmpemploymentTermination               | sonTerminationDate        | Okuma           |


## <a name="default-attribute-mapping"></a>Varsayılan öznitelik eşleme

Aşağıdaki tablo, yukarıda listelenen SuccessFactors öznitelikleri ile AD/Azure AD öznitelikleri arasında varsayılan öznitelik eşlemi sağlar. Azure AD sağlama uygulaması "Haritalama" bıçağında, bu varsayılan eşlemi yukarıdaki listeden öznitelikleri içerecek şekilde değiştirebilirsiniz. 

| \# | SuccessFactors Varlık                  | SuccessFactors Öznitelik | Varsayılan AD/Azure AD öznitelik eşleme   | İşlemE İşareti                                                                            |
|----|----------------------------------------|--------------------------|-----------------------------------------|----------------------------------------------------------------------------------------------|
| 1  | Kişi Başına                              | personIdExternal         | Employeeıd                              | Eşleşen öznitelik olarak kullanılır                                                                   |
| 2  | Kişi Başına                              | perPersonUuid            | \[Kaynak çapa \- olarak kullanılan eşlenmedi\] | İlk eşitleme sırasında, Sağlama Hizmeti personUuid'i varolan objectGuid\'e bağlar.  |
| 3  | Kişiye Göre                            | displayName              | displayName                             | NA                                                                                           |
| 4  | Kişiye Göre                            | firstName                | givenName                               | NA                                                                                           |
| 5  | Kişiye Göre                            | lastName                 | sn                                      | NA                                                                                           |
| 6  | Kullanıcı                                   | adresLine1             | Streetaddress                           | NA                                                                                           |
| 7  | Kullanıcı                                   | city                     | l                                       | NA                                                                                           |
| 8  | Kullanıcı                                   | ülke                  | Co                                      | NA                                                                                           |
| 9  | Kullanıcı                                   | durum                    | st                                      | NA                                                                                           |
| 10 | Kullanıcı                                   | kullanıcı adı                 | samAccountName                          | NA                                                                                           |
| 11 | Kullanıcı                                   | Zipcode                  | Postakodu                              | NA                                                                                           |
| 12 | PerEmail                               | Emailaddress             | posta                                    | NA                                                                                           |
| 13 | EmpJob                                 | jobTitle                 | başlık                                   | NA                                                                                           |
| 14 | EmpJob                                 | Managerıd                | manager                                 | NA                                                                                           |
| 15 | Empjob\.\.Şirket CountryOfregistration | twoCharCountryCode       | c                                       | NA                                                                                           |
| 16 | EmpJob\.Bölümü                     | bölüm               | bölüm                              | NA                                                                                           |
| 17 | EmpJob\.Bölümü                       | Bölümü                 | şirket                                 | NA                                                                                           |
| 18 | EmpJob\.Konumu                       | officeLocationAddress    | Streetaddress                           | NA                                                                                           |
| 19 | EmpJob\.Konumu                       | ofisLocationZipCode    | Postakodu                              | NA                                                                                           |
| 20 | AmpemploymentTermination               | activeEmploymentsCount   | hesapEtkin                          | activeEmploymentsCount=0 ise, hesabı devre dışı sayın\.                                           |

