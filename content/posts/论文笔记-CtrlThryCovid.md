---
title: "【论文笔记】 How Control Theory Can Help Us Control COVID-19"
date: 2021-10-03T22:31:09+08:00
draft: false
tags: ["控制论","疫情"]
categories: ["论文笔记"]
showToc: true
TocOpen: false
disableShare: true
---

# How Control Theory Can Help Us Control COVID-19[^1]


## 主旨
Using feedback, a standard tool in control engineering, we can manage our response to the novel coronavirus pandemic for maximum survival while containing the damage to our economies


- 目标：Reducing the R0 to below 1
    - R0：basic reproduction number

         - an indication of how many people, on average, an infected person will infect during the course of her illness

         - not a fixed number, depending as it does on such factors as the density of a community, the general health of its populace, its medical infrastructure and resources, and countless details of the community’s response

- 途径：Two main approaches

    - mitigation: which focuses on slowing but not necessarily stopping the spread

         - R0 is reduced but remains greater than 1

    - suppression, which aims to reverse epidemic growth

         - R0 is smaller than 1

- 干扰：

    - Testing has been spotty in some countries

    - health care capacity

- 反馈：an on-off approach, where some restrictions are lifted when the number of new cases requiring intensive care is below a threshold and are put back into place when it exceeds a certain number

    - 反馈输入变量：the number of cases in hospital intensive care units


- 控制量：Policy

    - nonpharmaceutical interventions

    - Restrictions on travel

- 仿真

    - 场景一: not doing anything

         - The large and lengthy peak well above the available bed capacity in the intensive care unit indicates a huge number of cases that will likely result in death.

    - 场景二： relaxing all restrictions when the number of infections has come down will only lead to a second surge in infections

         - only lead to a second surge in infections. Not only could this second surge overwhelm our hospitals, it could also lead to an even higher mortality rate than the first surge

    - 场景三： the simple on-off approach

         - most of the usual restrictions on gatherings, travel, and social interaction are lifted entirely when the number of new ICU cases drops below a lower threshold, and then are put back into place when this number exceeds a higher threshold

         - the R0 swings sharply between two levels, a high above 2 and a low below 1

         - This approach leads to oscillations, and if it is applied too aggressively, the high points of these oscillations will exceed the health care system’s capacity to treat patients

    - 场景四： Systematic Approach

         - targeted 90 percent occupancy of hospital intensive care units

         - When R0 is high, many restrictions are put into place. People are largely confined to their homes and services are limited to the bare minimum needed for society to function—utilities, police, sanitation, and food distribution, for example. Then, as conditions begin to improve, as revealed by our feedback measure of hospital-bed occupancy, other services are gradually phased in. Recovered people are allowed to move freely as they can no longer contract, or transmit, the virus. Perhaps people are allowed to visit restaurants within walking distance, some small businesses are allowed to reopen under certain conditions, or certain age groups are subject to less-stringent restrictions.

         - This strategy results in a stable response that maximizes the rate of recovery. Furthermore, the demand for hospital ICU beds never exceeds a threshold.

         - The health care capacity limit is never breached. In addition, note the general upward trend for the release of restrictions, as the number of recovered and immune people grows and nonpharmaceutical interventions are gradually phased out.

    - 场景五：The Beauty of Feedback

         - early on in the outbreak, there will be a great deal of uncertainty about R0 because testing will still be spotty, and because an unknown number of people may have the disease without realizing it. That uncertainty will inevitably fuel a surge in initial cases. However, once the case count is stabilized by the initial restrictive regime, a policy based on feedback will prove very tolerant of variations in R0

         - As the graph shows, after a few months it doesn’t matter whether the R0 is 2 or 2.6 because the total case count stays well below the number of available hospital beds due to the use of feedback.

    - 场景六： Compliance Conundrum

         - what noncompliance means is that a given level of restrictions will result in an R0 that is slightly higher than expected, which in turn causes fluctuations in the number of people who are infected. Noncompliance might, for instance, result in the restrictions being 10 percent less effective than intended.However, through feedback, the policy will automatically tighten to compensate.

- 评价：Clearly, tried-and-true principles of control theory, particularly feedback, can help officials plot more robust and optimal strategies as they attempt to deal with the devastating COVID-19 pandemic.


[^1]: [原文地址](https://spectrum.ieee.org/how-control-theory-can-help-control-covid19)