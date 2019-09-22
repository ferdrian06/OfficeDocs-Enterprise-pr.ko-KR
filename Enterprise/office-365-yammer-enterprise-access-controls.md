---
title: Office 365 Yammer Enterprise 액세스 제어
ms.author: robmazz
author: robmazz
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: O365-seccomp
localization_priority: Normal
search.appverid:
- MET150
ms.collection:
- Strat_O365_IP
- M365-security-compliance
description: '요약: 프로덕션 환경의 Yammer Enterprise 액세스 제어에 대 한 간략 한 요약입니다.'
ms.openlocfilehash: 19910ec0771b3034e7a290190ae7045ffc8651cf
ms.sourcegitcommit: 55a046bdf49bf7c62ab74da73be1fd1cf6f0ad86
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "37067677"
---
# <a name="yammer-enterprise-access-controls"></a><span data-ttu-id="4492c-103">Yammer Enterprise 액세스 제어</span><span class="sxs-lookup"><span data-stu-id="4492c-103">Yammer Enterprise Access Controls</span></span> 

<span data-ttu-id="4492c-104">Yammer 프로덕션 환경에 대 한 물리적 및 논리적 액세스는 소수의 사용자 (인프라 및 작업)로 제한 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-104">Physical and logical access to the Yammer production environment is restricted to a small set of people (infrastructure and operations).</span></span> <span data-ttu-id="4492c-105">다른 Office 365 엔지니어와 마찬가지로 Yammer 엔지니어는 고객 데이터에 대 한 액세스를 영 없이 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-105">As with other Office 365 engineers, Yammer engineers have zero standing access to Customer Data.</span></span> <span data-ttu-id="4492c-106">승인자 수가 제한 된 Lockbox와 유사한 승인 기반 just-in-time 액세스 제어 시스템을 사용 하 여 액세스를 요청 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-106">Access must be requested using an approval-based just-in-time access control system similar to Lockbox with a limited number of approvers.</span></span> <span data-ttu-id="4492c-107">승인자는 요청 (예: 요구 사항에 따라 요청이 합법적인 지, 비즈니스 사례, 시간 등)을 확인 한 다음 요청을 승인 하거나 거부 합니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-107">Approvers verify the request (for example, they verify whether the request is legitimate based on need, business case, time, etc.), and then approve or deny the request.</span></span> <span data-ttu-id="4492c-108">요청이 승인 되 면 정의 되 고 제한 된 시간에 JIT 액세스가 부여 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-108">If the request is approved, JIT access is granted for a defined and limited time.</span></span> <span data-ttu-id="4492c-109">액세스 시간이 초과 되 면 액세스가 자동으로 만료 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-109">After access time is exceeded, the access automatically expires.</span></span>

<span data-ttu-id="4492c-110">다른 Office 365 서비스와 마찬가지로 Yammer 프로덕션 환경에 대 한 모든 액세스에서 다단계 인증을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-110">As with other Office 365 services, all access to the Yammer production environment uses multi-factor authentication.</span></span> <span data-ttu-id="4492c-111">모든 액세스 및 명령 기록은 사용자에 게 특성을 사용 하며 Yammer 보안 팀에서 정기적으로 기록 및 검토 합니다.</span><span class="sxs-lookup"><span data-stu-id="4492c-111">All access and command history is attributed to a user, and logged and reviewed regularly by the Yammer security team.</span></span>

<span data-ttu-id="4492c-112">Yammer 관리 및 관리에 대 한 자세한 내용은 [Yammer Admin Help](https://support.office.com/article/yammer-–-admin-help-e1464355-1f97-49ac-b2aa-dd320b179dbe?ui=en-US&rs=en-US&ad=US)를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="4492c-112">For more information about Yammer administration and management, see [Yammer Admin Help](https://support.office.com/article/yammer-–-admin-help-e1464355-1f97-49ac-b2aa-dd320b179dbe?ui=en-US&rs=en-US&ad=US).</span></span>