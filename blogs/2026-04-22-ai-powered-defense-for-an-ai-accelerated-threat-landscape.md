---
title: "AI-powered defense for an AI-accelerated threat landscape"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/22/ai-powered-defense-for-an-ai-accelerated-threat-landscape/"
date: "Wed, 22 Apr 2026 17:00:00 +0000"
author: "Ales Holecek"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<p class="wp-block-paragraph">We are at an inflection point in cybersecurity.</p>



<p class="wp-block-paragraph">Recent advances in AI model capabilities are changing how vulnerabilities are discovered and exploited. AI models can autonomously discover weaknesses, chain multiple lower-severity issues into working end-to-end exploits, and produce working proof-of-concept code. This significantly compresses the window between vulnerability discovery and exploitation.</p>



<p class="wp-block-paragraph">These changes require organizations to rethink exposure, response, and risk. However, the same capabilities that can give attackers an advantage also create a unique opportunity for defenders. When applied correctly, they can accelerate vulnerability discovery, improve detection engineering, and reduce time to mitigation. We look forward to working together as an industry to use these AI model capabilities as part of enterprise-grade solutions to tilt the balance in favor of defenders.</p>



<h2 class="wp-block-heading" id="partnering-with-leading-model-providers">Partnering with leading model providers</h2>



<p class="wp-block-paragraph">Security has been and remains the top priority at Microsoft. Over the last two years, through our <a href="https://www.microsoft.com/en-us/trust-center/security/secure-future-initiative" rel="noreferrer noopener" target="_blank">Secure Future Initiative (SFI)</a>, we have strengthened our security foundations for this age of AI, in part by using AI to accelerate vulnerability discovery and remediation and help defend against threats. We have also invested in fundamental AI for security research, including the development of open-source industry benchmarks that can be used to evaluate whether models are ready for real-world security work.</p>



<p class="wp-block-paragraph">As we move forward, we are accelerating this work and partnering with the industry to use leading models, paired with our platforms and expertise, to turn AI-driven discovery into protection at scale.</p>



<p class="wp-block-paragraph">Through <a href="https://www.anthropic.com/glasswing" rel="noreferrer noopener" target="_blank">Project Glasswing</a>, Microsoft is working closely with Anthropic and industry partners to test Claude Mythos Preview, identify and mitigate vulnerabilities earlier, and coordinate defensive response. We evaluated Mythos using <a href="https://www.microsoft.com/en-us/security/blog/2026/03/20/cti-realm-a-new-benchmark-for-end-to-end-detection-rule-generation-with-ai-agents/" rel="noreferrer noopener" target="_blank">CTI-REALM</a>, our open-source benchmark for real-world detection engineering tasks, and the results showed substantial improvements relative to prior models.</p>



<p class="wp-block-paragraph">Microsoft is&nbsp;also evaluating other models.&nbsp;As part of our overall security approach, we continuously evaluate models from multiple&nbsp;providers as they are made available and integrate them into our enterprise-grade security platform. This multi-model approach is intentional&nbsp;as no&nbsp;single model defines our strategy.</p>



<h2 class="wp-block-heading" id="taking-action-in-three-fundamental-areas">Taking action in three fundamental areas</h2>



<p class="wp-block-paragraph">Defenders need to move faster to keep pace with AI-driven threats. We are focusing on three areas to help customers reduce risk and improve resilience.</p>



<h3 class="wp-block-heading" id="1-ai-led-vulnerability-discovery-and-mitigations-to-stay-current-on-software">1. AI-led vulnerability discovery and mitigations to stay current on software</h3>



<p class="wp-block-paragraph">We plan to incorporate advanced AI models, like Claude Mythos Preview, directly into our <a href="https://www.microsoft.com/en-us/securityengineering/sdl/" rel="noreferrer noopener" target="_blank">Security Development Lifecycle (SDL)</a> to identify vulnerabilities and develop mitigations and updates. This allows us to discover more issues more quickly across a broader surface area than previous methods and address them earlier in the lifecycle.</p>



<p class="wp-block-paragraph">AI-assisted discoveries are handled through our existing <a href="https://www.microsoft.com/en-us/msrc/blog/2026/04/strengthening-secure-software-global-scale-how-msrc-is-evolving-with-ai" rel="noreferrer noopener" target="_blank">Microsoft Security Response Center (MSRC) processes</a>, including Update Tuesday—our predictable and systematic way of distributing updates to customers—and out-of-band updates, where appropriate. Customers using Microsoft platform as a service (PaaS) and software as a service (SaaS) cloud services do not need to take any action; mitigations and updates are applied automatically. For customers who deploy Microsoft products on their own infrastructure, whether on-premises or self-hosted, staying current on all security updates is now not only the best practice; it is a fundamental requirement for staying secure against AI exposure.</p>



<p class="wp-block-paragraph">We will deploy detections to <a href="https://www.microsoft.com/en-us/security/business/microsoft-defender" rel="noreferrer noopener" target="_blank">Microsoft Defender</a>, our threat protection solution, when updates are released and share details through the <a href="https://www.microsoft.com/en-us/msrc/mapp" rel="noreferrer noopener" target="_blank">Microsoft Active Protections Program (MAPP)</a> partners to help mitigate risk. We are also using advanced AI models to proactively scan select open-source codebases. Identified issues will be addressed through coordinated vulnerability disclosure.</p>



<h3 class="wp-block-heading" id="2-ai-ready-posture-to-reduce-exposure">2. AI-ready posture to reduce exposure</h3>



<p class="wp-block-paragraph">Patching, while critical, is not sufficient on its own. We have identified the five dimensions&nbsp;where autonomous AI<s>&nbsp;</s>driven attacks gain disproportionate advantage—patching, open-source software, customer source code, internet-facing assets, and baseline security hygiene.</p>



<p class="wp-block-paragraph">For each dimension, <a href="https://www.microsoft.com/en-us/security/business/siem-and-xdr/microsoft-security-exposure-management" rel="noreferrer noopener" target="_blank">Microsoft Security Exposure Management</a> provides guidance and capabilities that customers can use to:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Assess their current state.</li>



<li class="wp-block-list-item">Understand prioritized actions to reduce risk.</li>



<li class="wp-block-list-item">Evaluate “what-if” scenarios before making changes.</li>



<li class="wp-block-list-item">Apply automation to remediate issues at scale.</li>
</ul>



<div class="wp-block-buttons is-layout-flex wp-block-buttons-is-layout-flex">
<div class="wp-block-button has-custom-width wp-block-button__width-100"><a class="wp-block-button__link wp-element-button" href="https://www.microsoft.com/en-us/security/business/siem-and-xdr/microsoft-security-exposure-management" rel="noreferrer noopener" target="_blank">Optimize your security posture with Microsoft Security Exposure Management</a></div>
</div>



<p class="wp-block-paragraph">These capabilities include tools like <a href="https://www.microsoft.com/en-us/security/business/cloud-security/microsoft-defender-external-attack-surface-management" rel="noreferrer noopener" target="_blank">Microsoft Defender External Attack Surface Management (EASM)</a> for continuous discovery of internet-facing assets, <a href="https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security">GitHub Advanced Security with CodeQL</a>, <a href="https://docs.github.com/en/code-security/responsible-use/responsible-use-autofix-code-scanning">Copilot Autofix</a> for open-source and first-party code, and <a href="https://learn.microsoft.com/en-us/microsoft-365/baseline-security-mode/baseline-security-mode-settings?view=o365-worldwide">Microsoft Baseline Security Mode (BSM)</a> to apply foundational controls across Exchange, Microsoft Teams, SharePoint, OneDrive, Office, and Microsoft Entra—with impact simulation before enforcement.</p>



<p class="wp-block-paragraph">Others in the industry have shared guidance and rightly emphasized the importance of continuous asset discovery and posture management. We are delivering&nbsp;an&nbsp;integrated&nbsp;experience&nbsp;through&nbsp;a new&nbsp;Microsoft Security Exposure Management&nbsp;blade—Secure Now—that combines guidance with the ability to act, so customers proactively reduce their exposure. Secure&nbsp;Now is&nbsp;available today at <a href="https://security.microsoft.com/securenow" rel="noreferrer noopener" target="_blank">https://security.microsoft.com/securenow</a></p>



<h3 class="wp-block-heading" id="3-ai-powered-solutions-to-defend-at-scale">3. AI-powered solutions to defend at scale</h3>



<p class="wp-block-paragraph">Beyond plans to use advanced AI models&nbsp;directly into&nbsp;our Security Development Lifecycle (SDL),&nbsp;we are separately building new solutions to help customers leverage advanced AI models to improve their security at enterprise scale.</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Rapidly deployed Defender detections developed for AI-discovered vulnerabilities, sim-shipping with corresponding updates to help mitigate risk immediately.</li>



<li class="wp-block-list-item">We have learned through our own testing that model capability to discover potential vulnerabilities is only the beginning. Organizations must also be able to use AI to validate and prioritize based on exploitability and impact, and build the fix. To help we plan to productize a new multi-model AI-driven scanning harness developed internally and make it available to customers to streamline their experience and deliver outcomes more quickly. This solution is expected to be available in preview in June 2026.</li>
</ul>



<p class="wp-block-paragraph">Our goal is to ensure findings are actionable. While models are powerful on their own, without prioritization and context, large volumes of results can overwhelm development teams. These new solutions are designed to pair model output with the context and security solutions needed for enterprises to drive security effectiveness at scale.</p>



<h2 class="wp-block-heading" id="get-started-today">Get started today</h2>



<p class="wp-block-paragraph">Customers can get started now by reviewing the guidance at <a href="http://security.microsoft.com/securenow" rel="noreferrer noopener" target="_blank">https://security.microsoft.com/securenow</a>. Any customer&nbsp;with a Microsoft Entra ID will be able to access the guidance.&nbsp;In addition,&nbsp;Microsoft Security customers will have access to capabilities that enable them to assess their exposure and take action.</p>



<div class="wp-block-buttons is-content-justification-center is-layout-flex wp-container-core-buttons-is-layout-a89b3969 wp-block-buttons-is-layout-flex">
<div class="wp-block-button has-custom-width wp-block-button__width-75"><a class="wp-block-button__link wp-element-button" href="https://security.microsoft.com/securenow" rel="noreferrer noopener" target="_blank">Read the guidance and get started</a></div>
</div>



<p class="wp-block-paragraph">We have also mobilized our Customer Success organization to support customers in implementing this guidance.</p>



<h2 class="wp-block-heading" id="what-s-ahead">What’s ahead</h2>



<p class="wp-block-paragraph">This work is ongoing. We will continue to share updates as testing progresses, new models emerge, and new guidance and solutions become available. The threat landscape will continue to evolve, but so will our defenses—and we are committed to ensuring that our customers have the tools, guidance, and partnership they need to stay ahead.</p>



<p class="wp-block-paragraph">Security is a team sport. The organizations that act on this shift—by staying current on patches, reducing exposure, and leveraging AI-powered security solutions—will be significantly harder to compromise than those that do not. The time to act is now and we look forward to partnering with the industry to build a safer world for all.</p>



<p class="wp-block-paragraph">To learn more about Microsoft Security solutions, visit our&nbsp;<a href="https://www.microsoft.com/en-us/security/business" rel="noreferrer noopener" target="_blank">website.</a>&nbsp;Bookmark the&nbsp;<a href="https://www.microsoft.com/security/blog/" rel="noreferrer noopener" target="_blank">Security blog</a>&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (<a href="https://www.linkedin.com/showcase/microsoft-security/">Microsoft Security</a>) and X (<a href="https://twitter.com/@MSFTSecurity" rel="noreferrer noopener" target="_blank">@MSFTSecurity</a>)&nbsp;for the latest news and updates on cybersecurity.</p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/22/ai-powered-defense-for-an-ai-accelerated-threat-landscape/">AI-powered defense for an AI-accelerated threat landscape</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
