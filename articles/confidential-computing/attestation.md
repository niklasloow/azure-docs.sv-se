---
title: Attestering av enclaves i Azure
description: Lär dig hur du kan använda attestering för att kontrol lera att din konfidentiella dator betrodda miljö är säker
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 9/22/2020
ms.author: JenCook
ms.openlocfilehash: 70a17aacde67744eae74ca263200f2c65fbd300a
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90997833"
---
# <a name="attesting-sgx-enclaves"></a>Attestering av SGX-Enclaves

Konfidentiell data behandling på Azure erbjuder Intel SGX-baserade virtuella datorer som kan isolera en del av kod eller data. När du arbetar med dessa [enclaves](confidential-computing-enclaves.md)vill du få verifiering och validering av att din betrodda miljö är säker. Den här verifieringen är processen för attestering. 

## <a name="overview"></a>Översikt 

Attestering gör det möjligt för en förlitande part att ha ökat förtroende för att deras program vara (1) körs i en enklaven och (2) att enklaven är uppdaterat och säkert. En enklaven ställer till exempel den underliggande maskin varan för att generera en autentiseringsuppgift som innehåller bevis på att enklaven finns på plattformen. Rapporten kan sedan ges till en andra enklaven som verifierar att rapporten genererades på samma plattform.

![attesterings kod i enklaven](media/attestation/attestation.png)



Attestering måste implementeras med hjälp av en säker attesterings tjänst som är kompatibel med systemprogram vara och kisel. Några exempel på tjänster som du kan använda är

- [Microsoft Azure attestering (för hands version)](https://docs.microsoft.com/azure/attestation/overview) eller
- [Intels attesterings-och etablerings tjänster](https://software.intel.com/sgx/attestation-services)


som båda är kompatibla med Azures konfidentiella data behandling Intel SGX-infrastruktur. 

## <a name="next-steps"></a>Nästa steg
Prova [Microsoft Azure attesterings exempel för enklaven-medvetna appar](https://docs.microsoft.com/samples/azure-samples/microsoft-azure-attestation/sample-code-for-intel-sgx-attestation-using-microsoft-azure-attestation/).