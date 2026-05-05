---
title: "Email threat landscape: Q1 2026 trends and insights"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/30/email-threat-landscape-q1-2026-trends-and-insights/"
date: "Thu, 30 Apr 2026 15:00:00 +0000"
author: "Microsoft Threat Intelligence and Microsoft Defender Security Research Team"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<aside class="table-of-contents-block accordion wp-block-bloginabox-theme-table-of-contents" id="accordion-62d8f4ac-eb40-41b3-85bc-6887649f1add">
	<button class="btn btn-collapse" type="button">
		<span class="table-of-contents-block__label">In this article</span>
		<span class="table-of-contents-block__current"></span>

		<svg class="table-of-contents-block__arrow" fill="none" height="11" viewBox="0 0 18 11" width="18" xmlns="http://www.w3.org/2000/svg">
			<path d="M15.7761 11L18 8.82043L9 0L0 8.82043L2.22394 11L9 4.35913L15.7761 11Z" fill="currentColor">
		</svg>
	</button>
	<div class="table-of-contents-block__collapse-wrapper collapse show" id="accordion-collapse-62d8f4ac-eb40-41b3-85bc-6887649f1add">
		<div class="table-of-contents-block__content">
			<ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#tycoon2fa-disruption-impact">Tycoon2FA disruption impact</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#qr-code-phishing-attacks">QR code phishing attacks</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#captcha-tactics">CAPTCHA tactics</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#malicious-payloads">Malicious payloads</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#business-email-compromise">Business email compromise</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#defending-against-email-threats">Defending against email threats</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#microsoft-defender-detections">Microsoft Defender detections</a></li></ol>		</div>
	</div>
	<span class="table-of-contents-block__progress-bar"></span>
</aside>



<p class="wp-block-paragraph">During the first quarter of 2026 (January-March), Microsoft Threat Intelligence detected approximately 8.3 billion email-based phishing threats, with monthly volumes declining slightly from 2.9 billion in January to 2.6 billion in March. By the end of the quarter, QR code phishing emerged as the fastest-growing attack vector, more than doubling over the period, while CAPTCHA-gated phishing evolved rapidly across payload types. Overall, 78% of email threats were link-based, while malicious payloads accounted for 19% of attacks in January—boosted by large HTML and ZIP campaigns—before settling at 13% in both February and March. Credential phishing remained the dominant objective behind malicious payloads throughout the quarter. This shift toward link-based delivery, combined with the payload trends, suggests that threat actors increasingly preferred hosted credential phishing infrastructure over locally-rendered payloads as the quarter progressed.</p>



<div class="wp-block-group is-vertical is-layout-flex wp-container-core-group-is-layout-fe9cc265 wp-block-group-is-layout-flex">
<p class="wp-block-paragraph">These trends reflect how threat actors continue to iterate on both scale and delivery techniques to improve effectiveness. At the same time, disruption efforts can meaningfully impact this activity. Following Microsoft’s Digital Crime Unit-led action against the <a href="https://www.microsoft.com/en-us/security/blog/2026/03/04/inside-tycoon2fa-how-a-leading-aitm-phishing-kit-operated-at-scale/">Tycoon2FA</a> phishing-as-a-service (PhaaS) platform in early March, associated email volume declined 15% over the remainder of the month, alongside a significant reduction in access to active phishing pages, limiting the platform’s immediate effectiveness. While Tycoon2FA has since adapted by shifting hosting providers and domain registration patterns, these changes reflect partial recovery rather than full restoration of previous capabilities. Alongside these shifts, business email compromise (BEC) activity remained prevalent, totaling approximately 10.7 million attacks in the quarter, largely driven by low-effort, generic outreach messages. At the same time, Microsoft Defender Research observed early indications of emerging techniques such as <a href="https://www.microsoft.com/en-us/security/blog/2026/04/06/ai-enabled-device-code-phishing-campaign-april-2026">device code phishing</a>—sometimes enabled by offerings like EvilTokens—which, while not yet at the scale of the trends discussed below, reflect continued innovation in credential theft methods.</p>
</div>



<div class="alignright wp-block-bloginabox-theme-kicker">
	<div class="kicker">
		<h2 class="kicker__title">
			Tycoon2fa ecosystem 		</h2>
		<p class="kicker__content">
							<a class="kicker__link" href="https://www.microsoft.com/en-us/security/blog/2026/03/04/inside-tycoon2fa-how-a-leading-aitm-phishing-kit-operated-at-scale/" rel="noopener noreferrer" target="_blank">
						Detect, investigate, and disrupt AiTM phishing							›</a>
					</p>
	</div>
</div>



<div class="alignright wp-block-bloginabox-theme-kicker">
	<div class="kicker">
		<h2 class="kicker__title">
			AI-enabled phishing		</h2>
		<p class="kicker__content">
							<a class="kicker__link" href="https://www.microsoft.com/en-us/security/blog/2026/04/06/ai-enabled-device-code-phishing-campaign-april-2026/" rel="noopener noreferrer" target="_blank">
						Examine device code authentication attacks at scale							›</a>
					</p>
	</div>
</div>



<p class="wp-block-paragraph">This blog provides a view of email threat activity across the first quarter of 2026, highlighting key trends in phishing techniques, payload delivery, and threat actor behavior observed by Microsoft Threat Intelligence. We examine shifts in QR code phishing, CAPTCHA evasion tactics, malicious payloads, and BEC activity, analyze how disruption efforts and infrastructure changes influenced threat actor operations, and provide recommendations and <a href="https://www.microsoft.com/security/business/microsoft-defender">Microsoft Defender</a> detections to help mitigate these threats. By bringing these trends together, this blog can help defenders understand how email-based attacks are evolving and where to focus detection, mitigation, and user protection strategies.</p>



<h2 class="wp-block-heading" id="tycoon2fa-disruption-impact">Tycoon2FA disruption impact</h2>



<p class="wp-block-paragraph">Since its emergence in August 2023,&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/2026/03/04/inside-tycoon2fa-how-a-leading-aitm-phishing-kit-operated-at-scale/">Tycoon2FA</a>&nbsp;has rapidly become one of the most widespread PhaaS platforms, leveraging&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/tag/adversary-in-the-middle/">adversary-in-the-middle (AiTM)</a> techniques&nbsp;to attempt to defeat non-phishing-resistant multifactor authentication (MFA) defenses. The group behind the PhaaS platform (tracked by Microsoft Threat Intelligence as&nbsp;Storm-1747) leases malicious infrastructure and sells phishing kits that impersonate various enterprise application sign-in pages and incorporate evasion tactics, such as fake CAPTCHA pages.</p>



<p class="wp-block-paragraph">The quarter began with Tycoon2FA in a period of reduced activity. January volumes represented a 54% decline from December 2025, marking the second consecutive month of sharp decreases. While post-holiday seasonal effects may have contributed to this decrease in volume, some of the reduction might also have been the result of Microsoft’s Digital Crimes Unit <a href="https://blogs.microsoft.com/on-the-issues/2026/01/14/microsoft-disrupts-cybercrime/">disruption of RedVDS</a>, a service used by many Tycoon2FA customers to distribute malicious email campaigns.</p>



<div class="alignright wp-block-bloginabox-theme-kicker">
	<div class="kicker">
		<h2 class="kicker__title">
			Inside RedVDS		</h2>
		<p class="kicker__content">
							<a class="kicker__link" href="https://www.microsoft.com/en-us/security/blog/2026/01/14/inside-redvds-how-a-single-virtual-desktop-provider-fueled-worldwide-cybercriminal-operations/" rel="noopener noreferrer" target="_blank">
						Investigate infrastructure, tooling, and attack chains							›</a>
					</p>
	</div>
</div>



<p class="wp-block-paragraph">After surging 44% in February, phishing attacks pointing to Tycoon2FA fell 15% in March driven largely by the effects of a coordinated disruption operation. In early March 2026, Microsoft’s Digital Crimes Unit, in coordination with Europol and industry partners, took action to&nbsp;<a href="https://blogs.microsoft.com/on-the-issues/2026/03/04/how-a-global-coalition-disrupted-tycoon/">disrupt Tycoon2FA’s infrastructure and operations</a>, significantly impairing the platform&#8217;s hosting capabilities. While Tycoon2FA-linked messages continued to circulate after the disruption, almost one-third of March&#8217;s total volume was concentrated in a three-day period early in the month; daily volumes for the remainder of March were notably lower than historical averages, and targets&#8217; ability to reach active phishing pages was substantially reduced.</p>


<figure class="wp-block-image size-full"><img alt="Line graph displays monthly phishing email volume from November to March for Tycoon2FA, showing a sharp decline from about 23 million in November to around 9 million in January, followed by a slight increase and stabilization near 11 million in February and March." class="wp-image-146976 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-1.-Tycoon2FA-monthly-malicious-messages-volume-November-2025-%E2%80%93-March-2026-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 1. Tycoon2FA monthly malicious messages volume (November 2025 &#8211; March 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">Tycoon2FA’s infrastructure composition evolved multiple times during the first three months of 2026. In January, Tycoon2FA domains started shifting toward newer generic top-level domains (TLDs) such as .DIGITAL, .BUSINESS, .CONTRACTORS, .CEO, and .COMPANY, moving away from previous commonly used TLDs or second-level domains like .SA.COM, .RU, and .ES. This trend became even more well-established in February. Following the March disruption, however, Microsoft Threat Intelligence observed a notable increase in Tycoon2FA domains with .RU registrations, with more than 41% of all Tycoon2FA domains using a .RU TLD since the last week of March.</p>


<figure class="wp-block-image size-full"><img alt="Line chart showing percentage trends of Tycoon2FA TLDs and 2LDs from November 2025 to March 2026, with six categories: SA.COM, RU, ES, DIGITAL, DE, and DEV. SA.COM starts highest near 22% and declines to about 6%, while RU rises sharply from 13% to 23% in March, with other categories remaining below 7% throughout." class="wp-image-147005 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-2.-Top-TLDs-and-second-level-domains-2LDs-associated-with-Tycoon2FA-infrastructure-November-2025-March-2026-1-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 2. Top TLDs and second-level domains (2LDs) associated with Tycoon2FA infrastructure (November 2025 &#8211; March 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">Additionally, toward the end of March, we saw Tycoon2FA moving away from Cloudflare as a hosting service and now hosts most of its domains across a variety of alternative platforms, suggesting the group is attempting to find replacement services that offer comparable anti-analysis protections.&nbsp;&nbsp;</p>



<h2 class="wp-block-heading" id="qr-code-phishing-attacks">QR code phishing attacks</h2>



<p class="wp-block-paragraph">In recent years,&nbsp;QR codes have rapidly emerged as a preferred tool&nbsp;among phishing threat actors seeking to bypass traditional email defenses. By embedding malicious URLs within image-based QR codes in the body of an email or within the contents of an attachment, threat actors attempt to exploit the limitations of text-based scanning engines and redirect victims to phishing sites on unmanaged mobile devices.</p>



<p class="wp-block-paragraph">The most significant shift in Q1 2026 was the rapid escalation of QR code phishing, with attack volumes increasing from 7.6 million in January to 18.7 million in March, a 146% increase over the quarter. After an initial 35% decline in January (continuing a late-2025 downtrend), volumes reversed course dramatically, growing 59% in February and another 55% in March. By the end of the quarter, QR code phishing had reached its highest monthly volume in at least a year.</p>


<figure class="wp-block-image size-full"><img alt="Line graph showing weekly volume of QR-code phishing attacks from November 2025 to March 2026, with phishing email counts fluctuating and peaking in March 2026." class="wp-image-146931 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-68.webp" /><figcaption class="wp-element-caption"><em>Figure 3. Trend of QR code phishing attacks by weekly volume (November 2025 &#8211; March 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">PDF attachments were the dominant delivery method throughout the quarter, growing from 65% of QR code attacks in January to 70% in March. While the overall volume of DOC/DOCX payloads containing malicious QR codes steadily increased each month, their share of overall delivery payloads decreased from 31% in January to 24% in March. A notable late-quarter development was the emergence of QR codes embedded directly in email bodies, which surged 336% in March. While still a small share of total volume (5%), this approach eliminates the need for an attachment altogether and highlights a shift in threat actor delivery methods that defenders should continue to monitor.</p>



<h2 class="wp-block-heading" id="captcha-tactics">CAPTCHA tactics</h2>



<p class="wp-block-paragraph">Threat actors use CAPTCHA pages to delay detection and increase user interaction. These pages function as a visual decoy, giving the appearance of a legitimate security check while concealing a transition to malicious content. By forcing users to engage with the CAPTCHA before accessing the payload, threat actors reduce the likelihood of automated scanning tools identifying the threat and increase the chances of successful credential harvesting or malware delivery. Additionally, fake CAPTCHAs are used in&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/">ClickFix attacks</a>&nbsp;to trick users into copying and executing malicious commands under the guise of human verification, allowing malware to bypass conventional security controls.</p>



<p class="wp-block-paragraph">After declining in both January (-45%) and February (-8%), CAPTCHA-gated phishing volumes exploded in March, more than doubling (+125%) to 11.9 million attacks, the highest volume observed over the last year.</p>


<figure class="wp-block-image size-full"><img alt="Line chart showing CAPTCHA-gated phishing volume between November 2025 and March 2026. The chart highlights a peak around December, a decline through January and February, followed by a sharp increase in March to over 12 million attacks." class="wp-image-146978 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-4.-CAPTCHA-gated-phishing-volume-November-2025-%E2%80%93-March-2026-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 4. CAPTCHA-gated phishing volume (November 2025 &#8211; March 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">The most notable aspect of Q1 CAPTCHA trends was the rapid rotation of delivery methods, as threat actors appeared to actively experiment with which payload formats most effectively evade email defenses:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>HTML attachments</strong> started the year as the most common method to deliver CAPTCHA-gated phishing (37% in January), but dropped 34% in February, hitting its lowest monthly volume since August 2025. Although their volume more than doubled in March, hitting an annual monthly high, HTML files were still only the second-most common delivery method to close the quarter.</li>
</ul>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>SVG files</strong>, which had seen consecutive months of decreasing volumes, grew by 49% in February at the same time nearly every other delivery payload type decreased. Because of this, it was the most common delivery method for the month, which had not happened since November 2025. This one-month spike reversed itself in March, however, and the number of SVG files delivering CAPTCHA-gated phish fell by 57%, accounting for just 7% of delivery payloads.</li>



<li class="wp-block-list-item"><strong>PDF files</strong> saw a meteoric rise in volume during the first quarter of the year. After seeing steady month-over-month declines since July 2025, and hitting an annual monthly low point in January 2026, the number of PDF attachments leading to CAPTCHA-gated phishing sites more than quadrupled in March (+356%). Not only did it retake its spot as the most common delivery method for these attacks since last July, but it eclipsed its annual high by more than 37%.</li>



<li class="wp-block-list-item"><strong>DOC/DOCX files</strong>, which didn’t make up more than 9% of CAPTCHA-gated phishing payloads over the previous nine months, increased almost five times (+373%) in March to account for 15% of payloads.</li>



<li class="wp-block-list-item"><strong>Email-embedded URLs</strong>, which had once delivered more than half of CAPTCHA-gated phish at the end of August 2025, hit an eight-month low after falling 85% between December and February. While their volume nearly doubled in March, they remained well below late-2025 levels.</li>
</ul>


<figure class="wp-block-image size-full"><img alt="Line graph comparing monthly data usage for five file types. XLS shows a sharp increase in March, PDF declines steadily, HTML peaks in December, and DOC/DOCX and URL remain relatively low with slight fluctuations." class="wp-image-147007 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-5.-Monthly-CAPTCHA-gated-phishing-volume-by-distribution-method-Q1-2026-1-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 5. Monthly CAPTCHA-gated phishing volume by distribution method (Q1 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">Another notable shift in CAPTCHA-gated phishing attacks was the erosion of Tycoon2FA’s impact on the landscape. At the end of 2025, more than three-quarters of CAPTCHA-gated phishing sites were hosted on Tycoon2FA infrastructure. This share decreased significantly over the course of the first three months of 2026, falling to just 41% in March. This broadening of CAPTCHA-gated phishing sites being used by an increasing number of threat actors and phishing kits, combined with the overall surge in volume, indicates that this technique is becoming a more entrenched component of the phishing playbook rather than a specialty of a small number of tools.</p>



<h3 class="wp-block-heading" id="three-day-campaign-delivers-captcha-gated-phishing-content-using-malicious-svg-attachments"><strong>Three-day campaign delivers CAPTCHA-gated phishing content using malicious SVG attachments</strong></h3>



<p class="wp-block-paragraph">Between February 23 and February 25, 2026, a large, sustained campaign sent more than 1.2 million messages to users at more than 53,000 organizations in 23 countries. Messages in the campaign included a number of different themes, including an important 401K update, a credit hold warning, a question about a received payment, a payment request for a past due invoice, and a voice message notification.</p>



<p class="wp-block-paragraph">Many of the messages contained a fake confidentiality disclaimer to enhance the credibility of the messages and provide a proactive excuse about why a recipient may have mistakenly received an email that may not be applicable to them.</p>


<figure class="wp-block-image size-full"><img alt="A screenshot of an email confidentiality notice warning recipients against sharing the message with third parties without sender consent. The text emphasizes the message's intended recipient, prohibits unauthorized distribution, and clarifies that the email does not constitute a legally binding agreement." class="wp-image-146932 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-71.webp" /><figcaption class="wp-element-caption"><em>Figure 6. Example fake confidentiality message used in February 23-25 phishing campaign</em></figcaption></figure>



<p class="wp-block-paragraph">Attached to each message was an SVG file that was named to appropriately match the theme of the email. All the file names included a Base64-encoded version of the recipient’s email address. Example of file names used in the campaign include the following:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><em>&lt;Recipient Email Domain&gt;_statements_inv_&lt;Base64-encoded Email Address&gt;.svg</em></li>



<li class="wp-block-list-item"><em>401K_copy_&lt;Recipient Name&gt;_&lt;Base64-encoded Email Address&gt;_241.svg</em></li>



<li class="wp-block-list-item"><em>Check_2408_Payment_Copy_&lt;Recipient First Name&gt;_&lt;Base64-encoded Email Address&gt;_241.svg</em></li>



<li class="wp-block-list-item"><em>INV#_1709612175_&lt;Base64-encoded Email Address&gt;.svg</em></li>



<li class="wp-block-list-item"><em>Listen_(&lt;Base64-encoded Email Address&gt;).svg</em></li>



<li class="wp-block-list-item"><em>PLAY_AUDIO_MESSAGE__&lt;Recipient Name&gt;_&lt;Base64-encoded Email Address&gt;_241.svg</em></li>
</ul>



<p class="wp-block-paragraph">If an attached SVG file was opened, the user’s browser would open locally and fetch content from one of the three following hostnames:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><em>bouleversement.niovapahrm[.]com</em></li>



<li class="wp-block-list-item"><em>haematogenesis.hvishay[.]com</em></li>



<li class="wp-block-list-item"><em>ubiquitarianism.drilto[.]com</em></li>
</ul>



<p class="wp-block-paragraph">Initially, the user would be shown a “security check” CAPTCHA. Once the CAPTCHA had been successfully completed, the user would then be shown a fake sign-in page used to compromise their account credentials.</p>



<h2 class="wp-block-heading" id="malicious-payloads">Malicious payloads</h2>



<p class="wp-block-paragraph">Credential phishing tightened its grip on the malicious payload landscape across Q1, growing from 89% of all payload-based attacks in January to 95% in February before settling at 94% in March. These credential phishing payloads either linked users to phishing pages or locally loaded spoofed sign-in screens on a user’s device. Traditional malware delivery continued its long-term decline, representing just 5–6% of payloads by the end of the quarter.</p>


<figure class="wp-block-image size-full is-resized"><img alt="Pie chart showing distribution of malicious payloads: HTML (31%), PDF (28%), SVG (19%), DOC/DOCX (12%), and URL (10%)." class="wp-image-146934 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-5.-Malicious-payloads-by-file-type-March-2026.webp" style="width: 669px; height: auto;" /><figcaption class="wp-element-caption"><em>Figure 7. Malicious payloads by file type (Q1 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">The most striking payload trend was the volatility across file types, driven by large campaigns that created dramatic week-to-week swings:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>HTML attachments</strong> started Q1 as the leading file type (37% of payloads in January), fell to an annual low in February (-57%), then nearly tripled in March (+175%). This volatility was largely campaign-driven, with concentrated activity in the first half of January and the third week of March.</li>



<li class="wp-block-list-item"><strong>Malicious PDFs</strong> followed a steady upward trajectory, increasing 38% in February and another 50% in March to reach their highest monthly volume in over a year. By March, PDFs accounted for 29% of payloads, up from 19% in January.</li>



<li class="wp-block-list-item"><strong>ZIP/GZIP attachments</strong> were similarly volatile by nearly doubling in January (+94%), dropping 38% in February, then surging 79% in March. Threat actors commonly use ZIP files to circumvent Mark of the Web (MOTW) protections.</li>



<li class="wp-block-list-item"><strong>SVG files</strong> emerged briefly in February as a notable delivery method (with a 50% volume increase) before declining 32% in March, mirroring the pattern seen in CAPTCHA-gated phishing.</li>
</ul>


<figure class="wp-block-image size-full"><img alt="Line graph showing daily usage trends of five file formats (DOC/DOCX, HTML, PDF, SVG, and ZIP). HTML files exhibit the highest and most frequent spikes, reaching over 2 million, while other formats maintain lower, more stable usage with occasional peaks." class="wp-image-146980 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-8.-Daily-malicious-payload-file-type-Q1-2026-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 8. Daily malicious payload file type (Q1 2026)</em></figcaption></figure>



<h3 class="wp-block-heading" id="large-scale-html-phishing-campaign-hosts-content-on-multiple-phaas-infrastructures"><strong>Large-scale HTML phishing campaign hosts content on multiple PhaaS infrastructures</strong></h3>



<p class="wp-block-paragraph">On March 17, 2026, Microsoft Threat Intelligence observed a massive phishing campaign that drove a significant surge in malicious HTML attachments during the month. The campaign involved more than 1.5 million confirmed malicious messages sent to over 179,000 organizations across 43 countries, accounting for approximately 7% of all malicious HTML attachments observed in March.</p>



<p class="wp-block-paragraph">All messages in this campaign were likely sent using the same tool or service, which exhibited several distinct and highly consistent characteristics. Most notably, sender addresses across the campaign featured excessively long, keyword‑stuffed usernames that embedded URLs, tracking identifiers, and service references. These usernames were crafted to resemble legitimate transactional, billing, or document‑related notification senders. Examples of observed sender usernames include:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><em>eReceipt_Payment_Alert_Noreply-/m939k6d7.r.us-west-2.awstrack.me/L0/%2F%2Fspectrumbusiness.net%2Fbilling%2F/2/010101989f2c1f29-ab5789bd-1426-4800-ae7d-877ea7f61d24-000000/LHnBIXX0VmCLVoXwNWtt23hGCdc=439/us02web.zoom.nl/j/81163775943?pwd=bLoo4JaWavsiTAuLWNoRsmbmALwjLB.1-qq8m2tzd</em></li>



<li class="wp-block-list-item"><em>Center-=AAP1eU7NKykAABXNznVa8w___listenerId=AAP1eU7NKykAABXNznVa8w___aw_0_device.player_name=Chrome___aw_0_ivt.result=unknown___cbs=9901711___aw_0_azn.zposition=%5B%22undefined%22%5D___us_privacy=___aw_0_app.name=Second+Screen___externalClickUrl=otdk-takaki-h</em></li>



<li class="wp-block-list-item"><em>DocExchange_Noreply-m939k6d7.r.us_west_2.awstrack.me/L0/%2F%2Fspectrumbusiness.net%2Fbilling%2F/2/010101989f2c1f29ab5789bd14264800ae7d877ea7f61d24000000/LHnBIXX0VmCLVoXwNWtt23hGCdc=439/us02web.zoom.nl/j/81163775943?pwd=bLoo4JaWavsiTAuLWNoRsmbmALwjLB.1-angie</em></li>
</ul>



<p class="wp-block-paragraph">The emails themselves contained little to no message body content. While subject lines varied, they consistently impersonated routine business and workflow notifications, including payment and remittance alerts (for example, Automated Clearing House (ACH), Electronic Funds Transfer (EFT), wire), invoice or aging statements, and e‑signature or document delivery requests. These subjects relied on urgency, approval language, and transactional framing to prompt recipients to review, sign, or access an attached document.</p>



<p class="wp-block-paragraph">Each message included an HTML attachment with a file name aligned to the email’s theme. When opened, the HTML file launched locally on the recipient’s device and immediately redirected the user to an initial external staging page. This page performed basic screening and then redirected the user to a secondary landing page hosting the phishing content. On the final landing page, users were presented with a CAPTCHA challenge before being directed to a fraudulent sign‑in page designed to harvest account credentials.</p>



<p class="wp-block-paragraph">Interestingly, although messages in this campaign shared common tooling, structure, and delivery characteristics, the infrastructure hosting the final phishing payload was linked to multiple different PhaaS providers. Most observed phishing endpoints were associated with Tycoon2FA, while additional activity was linked to Kratos (formerly Sneaky2FA) and EvilTokens infrastructure.</p>



<h2 class="wp-block-heading" id="business-email-compromise">Business email compromise</h2>



<p class="wp-block-paragraph">Microsoft defines business email compromise (BEC) as a&nbsp;text-based attack targeting enterprise users that impersonates a trusted entity&nbsp;for the purpose of persuading a recipient into initiating a fraudulent financial transaction or sending the threat actor sensitive documents. These attacks fluctuated across Q1, totaling approximately 10.7 million attacks: rising 24% in January, dipping 8% in February, then surging 26% in March.</p>


<figure class="wp-block-image size-full"><img alt="Line chart displays monthly BEC attack volume data for five months, with attacks starting high in November, dip in December, rise through January and February, and peak sharply in March to over 4 million attacks." class="wp-image-146982 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-9.-Monthly-BEC-attack-volume-November-2025-%E2%80%93-March-2026-scaled.webp" /><figcaption class="wp-element-caption"><em>Figure 9. Monthly BEC attack volume (November 2025 &ndash; March 2026)</em></figcaption></figure>



<p class="wp-block-paragraph">The composition of BEC attacks remained consistent throughout Q1. Generic outreach messages (like &#8220;Are you at your desk?&#8221;) accounted for 82–84% of initial contact emails each month, while explicit requests for specific financial transactions or documents represented just 9–10%. This pattern underscores that BEC operators overwhelmingly favor establishing a conversational rapport before making fraudulent requests, rather than leading with direct financial asks.</p>



<p class="wp-block-paragraph">Within the smaller subset of explicit financial requests, two sub-categories showed notable movement. Payroll update requests grew 15% in February, reaching their highest volume in eight months, potentially reflecting tax season-related social engineering. Gift card requests fell 37% in February to their lowest level since July before rebounding sharply in March (+108%), though they still represented less than 3% of overall BEC messages. These fluctuations suggest that BEC operators adjust their specific financial pretexts seasonally while maintaining a consistent overall approach.</p>


<figure class="wp-block-image size-full"><img alt="Pie chart displays BEC email content distribution for Q1 2026. Generic outreach contact dominates at 83.1%, followed by generic task request at 7.0%, payroll update at 4.2%, invoice payment at 3.1%, gift card request at 2.2%, and other at 0.4%, with each segment color-coded and labeled." class="wp-image-146983 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Figure-10.-Initial-BEC-email-content-by-type-Q1-2026.webp" /><figcaption class="wp-element-caption"><em>Figure 10. Initial&nbsp;BEC email content by type (Q1 2026)</em></figcaption></figure>



<h2 class="wp-block-heading" id="defending-against-email-threats">Defending against email threats</h2>



<p class="wp-block-paragraph">Microsoft recommends the following mitigations to reduce the impact of this threat. </p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-office-365/recommended-settings-for-eop-and-office365">Review the recommended settings</a>&nbsp;for Exchange Online Protection and Microsoft Defender for Office 365 to ensure your organization has established essential defenses and knows how to monitor and respond to threat activity.</li>



<li class="wp-block-list-item">Invest in user awareness training and phishing simulations.&nbsp;<a href="https://learn.microsoft.com/defender-office-365/attack-simulation-training-get-started">Attack simulation training</a>&nbsp;in Microsoft Defender for Office 365, which also includes simulating phishing messages in Microsoft Teams, is one approach to running realistic attack scenarios in your organization.</li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-office-365/zero-hour-auto-purge">Enable Zero-hour auto purge (ZAP)</a>&nbsp;in Defender for Office 365 to quarantine sent mail in response to newly acquired threat intelligence and retroactively neutralize malicious phishing, spam, or malware messages that have already been delivered to mailboxes.</li>



<li class="wp-block-list-item">Responders could also manually check for and purge unwanted emails containing URLs and/or Subject fields that are similar, but not identical, to those of known bad messages.&nbsp;<a href="https://learn.microsoft.com/defender-office-365/threat-explorer-investigate-delivered-malicious-email">Investigate malicious email that was delivered in Microsoft 365</a>&nbsp;and use&nbsp;<a href="https://security.microsoft.com/threatexplorer">Threat Explorer</a>&nbsp;to find and delete phishing emails.</li>



<li class="wp-block-list-item">Turn on&nbsp;<a href="https://learn.microsoft.com/defender-office-365/safe-links-about">Safe Links</a>&nbsp;and&nbsp;<a href="https://learn.microsoft.com/defender-office-365/safe-attachments-about">Safe Attachments</a>&nbsp;in Microsoft Defender for Office 365.</li>



<li class="wp-block-list-item">Enable&nbsp;<a href="https://learn.microsoft.com/defender-endpoint/enable-network-protection">network protection</a>&nbsp;in Microsoft Defender for Endpoint.</li>



<li class="wp-block-list-item">Encourage users to use Microsoft Edge and other web browsers that support&nbsp;<a href="https://learn.microsoft.com/deployedge/microsoft-edge-security-smartscreen">Microsoft Defender SmartScreen</a>, which identifies and blocks malicious websites, including phishing sites, scam sites, and sites that host malware.</li>



<li class="wp-block-list-item">Enable password-less authentication methods (for example, Windows Hello, FIDO keys, or Microsoft&nbsp;Authenticator) for accounts that support password-less. For accounts that still require passwords, use authenticator apps like Microsoft Authenticator for MFA.&nbsp;<a href="https://learn.microsoft.com/entra/identity/authentication/concept-authentication-methods">Refer to this article</a>&nbsp;for the different authentication methods and features.
<ul class="wp-block-list">
<li class="wp-block-list-item">Conditional access policies can also be scoped to&nbsp;<a href="https://learn.microsoft.com/entra/identity/conditional-access/policy-admin-phish-resistant-mfa">strengthen privileged accounts with phishing resistant MFA</a>.</li>
</ul>
</li>



<li class="wp-block-list-item">Configure&nbsp;<a href="https://learn.microsoft.com/defender-xdr/automatic-attack-disruption">automatic attack disruption</a>&nbsp;in Microsoft Defender XDR. Automatic attack disruption is designed to contain attacks in progress, limit the impact on an organization&#8217;s assets, and provide more time for security teams to remediate the attack fully.</li>
</ul>



<h2 class="wp-block-heading" id="microsoft-defender-detections">Microsoft Defender detections</h2>



<p class="wp-block-paragraph"><a href="https://www.microsoft.com/security/business/microsoft-defender">Microsoft Defender</a> customers can refer to the list of applicable detections below. Microsoft Defender coordinates detection, prevention, investigation, and response across endpoints, identities, email, apps to provide integrated protection against attacks like the threat discussed in this blog.</p>



<h3 class="wp-block-heading" id="microsoft-defender-for-endpoint">Microsoft Defender for Endpoint</h3>



<p class="wp-block-paragraph">The following alert might indicate threat activity associated with this threat. The alert, however, can be triggered by unrelated threat activity.</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Suspicious activity likely indicative of a connection to an adversary-in-the-middle (AiTM) phishing site</li>
</ul>



<h3 class="wp-block-heading" id="microsoft-defender-for-office-365">Microsoft Defender for Office 365</h3>



<p class="wp-block-paragraph">The following alerts might indicate threat activity associated with this threat. These alerts, however, can be triggered by unrelated threat activity.</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">A potentially malicious URL click was detected</li>



<li class="wp-block-list-item">A user clicked through to a potentially malicious URL</li>



<li class="wp-block-list-item">Suspicious email sending patterns detected</li>



<li class="wp-block-list-item">Email messages containing malicious URL removed after delivery</li>



<li class="wp-block-list-item">Email messages removed after delivery</li>



<li class="wp-block-list-item">Email reported by user as malware or phish</li>
</ul>



<h3 class="wp-block-heading" id="microsoft-security-copilot">Microsoft Security Copilot</h3>



<p class="wp-block-paragraph"><a href="https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot">Microsoft Security Copilot</a> is <a href="https://learn.microsoft.com/defender-xdr/security-copilot-in-microsoft-365-defender">embedded in Microsoft Defender</a> and provides security teams with AI-powered capabilities to summarize incidents, analyze files and scripts, summarize identities, use guided responses, and generate device summaries, hunting queries, and incident reports.</p>



<p class="wp-block-paragraph">Customers can also <a href="https://learn.microsoft.com/defender-xdr/security-copilot-agents-defender">deploy AI agents</a>, including the following <a href="https://learn.microsoft.com/copilot/security/agents-overview">Microsoft Security Copilot agents</a>, to perform security tasks efficiently:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-xdr/threat-intel-briefing-agent-defender">Threat Intelligence Briefing agent</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-xdr/phishing-triage-agent">Phishing Triage agent</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-xdr/advanced-hunting-security-copilot-threat-hunting-agent">Threat Hunting agent</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/defender-xdr/dynamic-threat-detection-agent">Dynamic Threat Detection agent</a></li>
</ul>



<p class="wp-block-paragraph">Security Copilot is also available as a <a href="https://learn.microsoft.com/en-us/copilot/security/experiences-security-copilot">standalone experience</a> where customers can perform specific security-related tasks, such as incident investigation, user analysis, and vulnerability impact assessment. In addition, Security Copilot offers <a href="https://learn.microsoft.com/copilot/security/developer/custom-agent-overview">developer scenarios</a> that allow customers to build, test, publish, and integrate AI agents and plugins to meet unique security needs.</p>



<h3 class="wp-block-heading" id="threat-intelligence-reports">Threat intelligence reports</h3>



<p class="wp-block-paragraph">Microsoft Defender XDR customers can use the following <a href="https://learn.microsoft.com/defender-xdr/threat-analytics">Threat Analytics</a> reports in the Defender portal (requires license for at least one Defender XDR product) to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.</p>



<p class="wp-block-paragraph"><strong>Microsoft Defender XDR threat analytics</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/40914705-f2de-45e4-8ac1-b7554d442ad2/analystreport">Activity Profile: Email threat landscape, March 2026</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/387aa2b5-fb1a-4398-b1ac-bda360f65a16/analystreport">Activity Profile: Email threat landscape, February 2026</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/c5ecb5fe-8b3b-4d0b-8bfc-86320dcc0901/analystreport">Activity Profile: Email threat landscape, January 2026</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/d5c0cc60-ff54-41f3-b174-45adc0216497/analystreport">Tool Profile: Tycoon2FA</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/2a699ab5-b4f4-4c72-8b30-d33f3759f807/analystreport">Actor Profile: Storm-1747</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/2b78ac51-0fd1-48ff-a2c0-256a21019c48/analystreport">Technique Profile: QR code phishing</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/bdf0d0c5-f5f3-435a-b4a1-6e3beb73b5b9/analystreport">Technique Profile: ClickFix technique leverages clipboard to run malicious commands</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/e01d0430-a92d-457e-909c-de9d2ad93b8d/analystreport">Threat Overview Profile: Business Email Compromise</a></li>



<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/edd01a8c-283d-42f6-bdd4-0b7b4dbd369b/analystreport">Threat Overview Profile: Adversary-in-the-middle credential phishing</a></li>
</ul>



<p class="wp-block-paragraph">Microsoft Security Copilot customers can also use the <a href="https://learn.microsoft.com/defender/threat-intelligence/security-copilot-and-defender-threat-intelligence?bc=%2Fsecurity-copilot%2Fbreadcrumb%2Ftoc.json&amp;toc=%2Fsecurity-copilot%2Ftoc.json#turn-on-the-security-copilot-integration-in-defender-ti">Microsoft Security Copilot integration</a> in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the <a href="https://learn.microsoft.com/defender/threat-intelligence/using-copilot-threat-intelligence-defender-xdr">embedded experience</a> in the Microsoft Defender portal to get more information about this threat actor.</p>



<h3 class="wp-block-heading" id="learn-more">Learn more</h3>



<p class="wp-block-paragraph">For the latest security research from the Microsoft Threat Intelligence community, check out the <a href="https://aka.ms/threatintelblog">Microsoft Threat Intelligence Blog</a>.</p>



<p class="wp-block-paragraph">To get notified about new publications and to join discussions on social media, follow us on <a href="https://www.linkedin.com/showcase/microsoft-threat-intelligence">LinkedIn</a>, <a href="https://x.com/MsftSecIntel">X (formerly Twitter)</a>, and <a href="https://bsky.app/profile/threatintel.microsoft.com">Bluesky</a>.</p>



<p class="wp-block-paragraph">To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the <a href="https://thecyberwire.com/podcasts/microsoft-threat-intelligence">Microsoft Threat Intelligence podcast</a>.</p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/30/email-threat-landscape-q1-2026-trends-and-insights/">Email threat landscape: Q1 2026 trends and insights</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
